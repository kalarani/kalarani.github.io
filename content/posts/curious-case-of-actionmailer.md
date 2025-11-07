+++
date = '2012-04-14T08:13:56+05:30'
title = 'The Curious Case of Actionmailer'
+++
Came across a very weird issue while working with actionmailer last week. Debugging the issue gave a good understanding of how the [mail](https://github.com/mikel/mail) gem (which actually constructs the message, body and its headers) works. The problem statement is to send multiple emails on some particular event. I went ahead and implemented this problematic piece of code

```ruby
class MyMailer < ActionMailer::Base
  def send_multiple_mails(n)
    n.times do |i|
      mail(:to => 'receiver@gmail.com', :from => 'sender@gmail.com', :subject => "Mail no #{i}").deliver!
    end
  end
end
```

If you are still wondering what is the problem with this code, go ahead and try to send a mail to yourself using the above mailer for any value of n > 1. The basic rails code for the problem statement can be found [here](https://github.com/kalarani/MysteriousMailer). You can notice that the first mail is sent successfully. And the subsequent emails do not contain the full text. It stops abruptly while constructing a html link. Weird. The logs told me that the headers of all the mails are the same, but still they are all delivered differently.

Started digging the actionmailer gem for some clue. Came across a post that explained how we are able to call the instance methods of a mailer as if they are class methods. So, I took a sneak peak at the initializer of ActionMailer::Base

```ruby
def initialize(method_name=nil, *args)
  super()
  @_message = Mail.new
  process(method_name, *args) if method_name
end
```

and the most important mail method.

```ruby
def mail(headers={}, &block)
  m = @_message
  ...
  create_parts_from_responses(m, responses)
  ...
  m
end
```

It is evident from the above methods that a Mail::Message object is initialized once (when the mailer is initialized) and is used in the mail method. In an ideal scenario, the mailer object is constructed, which in turn constructs the message object,  processed and returned by the mail method and eventually gets delivered. The next time you call the same method, another mailer object is constructed, and as a result another message object is constructed. But in my_mailer the mailer is initialized once and I've called the mail method more than once and hence the message object is being reused.

Reusing the same message to send multiple mails made some sense, but still it didn't provide a clear explanation as to why the emails are delivered partially. I suspected the content of my email and removed the second paragraph and to my surprise all the emails were delivered completely. So, reusing the message object is not the only problem. The content that I had was also part of the problem.

Tried again with the full content and the response constructed by the actionmailer had all the content. It is only when the content was set as the body of the message, part of it disappeared.

Started digging into the mail gem. The mail gem is the one that forms the various components of a message. It constructs the body and sets various headers that are appropriate to send the message across, along with the default settings given by the user. One such header field is 'content_transfer_encoding', which determines the encoding method that would be used to send the email. The Mail::Body class determines the default encoding whenever it is initialized with the content.

```ruby
def initialize(string = '')
  # initialize @raw_source with string
  ...
  @encoding = (only_us_ascii? ? '7bit' : '8bit')
  ...
end
```

This encoding method is used when the mail is to be encoded to a different encoding method while sending.

```ruby
def encoded(transfer_encoding = '8bit')
  ..........
  be = get_best_encoding(transfer_encoding)
  dec = Mail::Encodings::get_encoding(encoding)
  enc = Mail::Encodings::get_encoding(be)
  if transfer_encoding == encoding and dec.nil?
    # Cannot decode, so skip normalization
    raw_source
  else
    # Decode then encode to normalize and allow transforming 
    # from base64 to Q-P and vice versa
    decoded = dec.decode(raw_source)
    if defined?(Encoding) && charset && charset != "US-ASCII"
      decoded.encode!(charset)
      decoded.force_encoding('BINARY') unless Encoding.find(charset).ascii_compatible?
    end
    enc.encode(decoded)
  end
end
```

The interesting piece of code is the normalization part in the above method. The raw_source is decoded from its default encoding and then encoded to the desired/best suitable format.

The encoding method of a body can also be set externally, from the Mail::Message, if the message header has the content_transfer_encoding field set.

```ruby
def add_encoding_to_body
  if has_content_transfer_encoding?
     @body.encoding = content_transfer_encoding
  end
end
```

```ruby
def has_content_transfer_encoding?
  header[:content_transfer_encoding] && header[:content_transfer_encoding].errors.blank?
end
```

Just before the message is delivered, it identifies and sets the transfer_encoding field in the header.

```ruby
def identify_and_set_transfer_encoding
  if body && body.multipart?
    self.content_transfer_encoding = @transport_encoding
  else
    self.content_transfer_encoding = body.get_best_encoding(@transport_encoding)
  end
end
```

In my case, when the first message was constructed, the default encoding identified by the body was '8-bit' encoding method. But, the header[:content_transfer_encoding] was set to 'Quoted-Printable' encoding method, the best method identified to send the message across. Since the message object is being reused when the mail method is called multiple times, the same header is used for the subsequent messages as well, which sets 'Q-P' as the encoding method for the body of the message.

## Quick summary:

While sending the first message,
* The message body is initialized with the content rendered from view
* The encoding method is identified as '8-bit'
* The message header doesn't have content_transfer_encoding set
* The body is decoded from '8-bit' and encoded to 'Q-P' (the best encoding method)
* The message header is set to have 'Q-P' as content_transfer_encoding
* Message delivered

While sending the second message,
* The message body is initialized with the content rendered from view
* The encoding method is identified as '8-bit'
* The message header has 'Q-P' as content_transfer_encoding and hence sets the encoding of the body to 'Q-P'
* The body is decoded from 'Q-P', where part of the message is lost, and the rest of the message is encoded to 'Q-P' again
* The message header remains to be 'Q-P'
* Message delivered

## Moral of the story: 
> Do not try to send more than one mail from a mailer method.
> Keep the loops outside of the mailer.
