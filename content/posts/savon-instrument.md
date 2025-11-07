+++
date = '2012-06-01T10:13:02+05:30'
title = 'Savon Instrument'
+++
Started working on a rails app that needs to communicate with a SOAP based web service in the backend. We are using [savon](http://savonrb.com/) for talking to the web service. Every service call took a long time to process and we wanted to measure how much time is spent in the service calls. Inspired by [rails_instrument](http://rubygems.org/gems/rails_instrument), a middleware to show instrumentation information in http headers, I thought of writing a similar one for the SOAP calls that we make. SavonInstrument is born, and this is how it looks, for the [sample application](https://github.com/kalarani/savon_instrument_example).

{{< figure src="/images/savon-instrument.jpg" title="Savon Instrument in Action" >}}

The idea is simple. Intercept the calls made by savon client (using savon hooks), measure the time taken, and add it to the http headers (in the middleware)

```ruby
module SavonInstrument
  class <<self
    def reset!
      $soap_calls = {}
    end

    def init
      $soap_calls ||= {}
    end

    def data
      $soap_calls
    end

    def add(method, time)
      data[method] = time
    end
  end
end
```

The challenge in the whole process was to identify how to intercept the calls made by the savon client. There was no documentation around using hooks and in fact no real hooks are provided by savon. The source code gave a slight hint that a Savon::Hook defined for :soap_request will be called while making a request.

Update: Savon provides [around hooks](https://github.com/rubiii/savon/issues/291) now. Hence the hook for :soap_request just captures the time before and after the request .

```ruby
Savon.configure do |c|
  c.hooks.define('new_hook', :soap_request) do |callback, req|
    start_time = Time.now
    response = callback.call
    soap_action = req.http.headers['SoapAction'].split("/").last.gsub("\"", '')
    SavonInstrument.add soap_action, Time.now - start_time
    response
  end
end
```
