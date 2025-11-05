+++
date = '2011-05-28T06:14:06+05:30'
title = 'Android vs Wp7'
+++
After a short stint in android development, I thought of experimenting with a different mobile platform. I didn't have too many options. iPhone development was ruled out since i did not have a mac and wasn't quite comfortable with Objective C. I chose WP7, hoping to catch up with C# coding which I haven't been doing for a looong time. And moreover the latest buzz about the next version of windows phone, Mango added up to the excitement to see what it offers to the developers. I made up my mind and setup the environment and started off with a simple BMI calculator application for WP7 and android. Here is the summary of stuff that I discovered last week.

|Android|WP7|
|---|---|
|The multiple screen sizes are handled by android itself. The developer is concerned with following the best practices to support this.|Though only two screen sizes are possible, you need to code differently for them. Even conditional statemenst in code and different xaml files might be required to cater different screen sizes.|
|The template editor is still basic. Throws up exceptions when background of an element is set to an xml. Needs a lot of improvement in this area.|Has a very decent template editor - more similar to what we've seen and used to design the template for VB and C# windows applications. It is very dynamic and responsive to changes in xaml file and vice versa.|
|Though you'll find some surprises its easy to draw analogy from web development.|Silverlight is used for development. Requires too much of code to achieve even the simplest functionality.|
|Takes a good amount of time to setup the eclipse plugin and android SDK. But once setup development and testing in emulator is much faster than WP7|Setting up the environment takes more than three hours and the tools are pretty slow if you are developing and testing it in the emulator frequently.|
|Costs only $25 to register as an android developer and start publishing.|To publish an app in the marketplace, you need to pay an annual subscription fee of $99|

The source code for [android](https://github.com/kalarani/BMI-Calculator-Android) and [windows phone](https://github.com/kalarani/BMI-Calculator-WP7) are available in github.
