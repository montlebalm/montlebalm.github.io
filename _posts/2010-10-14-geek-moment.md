---
title: "Geek Moment"
layout: single
summary: "Creating a calculator-based intervalometer for my DSLR"
---
Today I experienced a geek moment that resonated in my very core. It all started with an urge to take a time lapsed movie of myself Slacklining. If you're not familiar with this wonderful pastime, Slacklining is the art of balancing on a nylon band supported between two points. With a few hours practice, you can expect to walk, bounce (ever so gently), and fall without harming yourself or others.

Back to the task at hand; my first instinct was to use an external iSight to capture a movie since my desk is adjacent to a second floor window. However, sometime between mounting it on the monopod and positioning it in the frame, I realized it must be one of the creepiest looking things someone in a second floor window can do.

Abandoning that, I knew there must be some way to use my fancy pants DSLR camera to sate this dark thirst. Mere moments later I was teetering on the brink of nerd ecstasy after discovering an Instructables article detailing [how to create an intervalometer using a TI-83 calculator](http://www.instructables.com/id/Turn-a-TI-Graphing-Calculator-into-an-Intervalomet/).

I know.

Luckily, I already had everything I needed to assemble this geek golem. Specifically, it requires a DSLR camera with a remote shutter release socket (preferably 2.5mm), a Texas Instruments graphing calculator, and a 2.5mm link cable.

## Step 1: Cultivate the mind

First, we're going to create a controller program on our calculator. If you've forgotten how to create a new application I'll jog your memory. Simply hit the `PRGM` key, hit the right arrow twice, and press `ENTER`. Call it whatever you like so long as you can tell it apart from that quadratic equation program you made to cheat on your algebra test.

The actual code provided in the tutorial was a little too basic for my needs. Instead, I used one suggested by "silentfallen" in the posts comments. If you don't know how to get some of these functions, [consult a specialist](http://www.ticalc.org/programming/columns/83plus-bas/cherny/). Here's the code.

{% highlight c %}
Disp "START DELAY?"
Prompt A
A*333.333 &#x2192; W
Disp "HOW OFTEN?"
Prompt B
B*333.333 &#x2192; X
Disp "HOW MANY SHOTS?"
Prompt Y
For(H,1,W,1)
End
Send(A)
Y-1 &#x2192; Y
While Y>0
Y-1 &#x2192; Y
For(H,1,X,1)
End
Send(A)
End
{% endhighlight %}

I should note that all time inputs will be evaluated in seconds. Thus, an input of "15, 5, 100" will cause the camera to wait 15 seconds and then proceed to take 100 photos with 5 seconds in between each picture.

## There is no step 2

Connect your calculator to your camera using the link cable and you're done. Turn on the DSLR and execute your program to experience the magic firsthand. Next, realize you just saved hundreds of dollars by using your knock off GameBoy to create a featured intervalometer.

## The extra mile

The only thing left is that nagging problem of fashioning the calculator to the tripod. Never fear, for I have come to deliver thee from torment. Behold, the device.

![Calculator harness](/images/posts/calculatortron.jpg)

Disregard that it looks like something from the Tower of London's torture chamber. I constructed this frame from two coat hangers using a pair of pliers. The upturned feet at the bottom gently cups the calculator while the cross bar in the middle keeps it from tilting forward. Hang the hook from any point of the tripod and you're good to go.

This whole thing actually fits in my Canon camera bag, but it's less than ideal for traveling. If you come up with a better idea, tweet it to @montlebalm and I'll post it here for bragging rights.
