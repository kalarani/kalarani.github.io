+++
date = '2011-04-01T15:00:48+05:30'
title = 'Tamil in Android'
+++
Displaying tamil characters in android can be easily achieved by using [TSCII](http://www.tscii.org/) (Tamil Standard Code for Information Interchange) fonts. TSCII is an extended ASCII encoding standard. Unlike ASCII which uses 7 bits for encoding, TSCII makes use of all the 8 bits in a byte. The first 128 bytes remains the same as that of ASCII.  The tamil vowels and consonants along with the common prefixes, suffixes and other special characters fill up the rest of the 128 bytes.

{{< figure src="/images/tscii.png" title="TSCII Encoding" >}}

You can place the TSCII font in the assets folder of your android application and set it to any view where you need to display tamil text.

```java
Typeface tf = Typeface.createFromAsset(context.getAssets(), "fonts/tscFont.ttf");
TextView textView = new TextView(context);
textView.setTypeface(tf);
```

If you have the text in unicode format, there are online [tools](http://www.suratha.com/uni2tsc.htm) available that can help you convert the tamil text from unicode to TSCII format and vice versa.
