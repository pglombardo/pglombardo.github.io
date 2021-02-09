---
layout: single
categories: ["Robotics"]
classes: wide
header:
  teaser: /assets/images/posts/2021/robotic-arms-teaser.png
title: "Robotic Arms for Research: Comparison 2021"
---

When looking for a robotic arms in the past the choices were either plastic toys or industrial arms that can maim you both physically and financially.  Thank God for 3D printers because that middle space has been filling up over the last ten years.

After a few days of searching and scouring the net, here are the three best robotic arms I've found that fit within the following constraints:

* 6 degrees of freedom
* €800 - €3000
* Professional grade suitable for research

## Annin Robotics AR2/AR3

![](/assets/images/posts/2021/ar3.png)

This arm looks, feels and sounds like a top of the line industrial arm.  For me, the most impressive point was the silent operation.  Watch this [motor speed test video](https://youtu.be/qjtNjkFL3us?t=22) to get a sense of the operating motor noise.

Also great is the aluminum construction, quick movement speed and it's pickup payload of 1,9kg (~4lbs).

One potential downside is that you have to buy the components and build it yourself from scratch.  Unless you enjoy that type of work, this can be a blocker for people who want to focus on software and not the hardware.  For those that do pursue the build, the community is active and supportive with a lot of builds being posted on both [Youtube](https://www.youtube.com/watch?v=GFyARrTgjAI&list=PLqND52C3ndZTxpf6-XI0Zj6aettHOLKND) and in [the Annin Robotic forums](https://www.anninrobotics.com/forum).  

All parts, both hardware and software are open and easily accessible.  To help new builds, Annin Robotics provides kits that has most of the parts needed.

This is my favorite arm although I haven't committed myself to doing the build yet.  My rough guess is that it could take up to a month or more depending on the amount of time spent each day.

* Homepage: [https://www.anninrobotics.com](https://www.anninrobotics.com)
* YouTube Channel: [https://www.youtube.com/user/usernamseunavailable/featured](https://www.youtube.com/user/usernamseunavailable/featured)

## WLKata Mirobot

![](/assets/images/posts/2021/wlkata-mirobot.jpeg)

The Mirobot an elegant design.  It looks like a scale model of a full size industrial arm.  The total weight of the arm is 1,5kg and has a max payload 150g.  For many this is a non-issue.  For others, this may lack the strength needed.

WLKata has opened some code [on Github](https://github.com/wlkata) but some critical bits that are lacking are:

* STL files to re-print parts
* A full electrical and wiring schematic
* Teardown and rebuild guides
* Source code to the firmware on it's Arduino Mega 2560.

_Note: The STL files provided in their Github repos are for ROS modeling - not for reprinting parts._

Unfortunately, WLKata keeps these things confidential which is a negative considering that hacking, breaking, rebuilding and augmenting the product is the whole point for such a device.

For the price, it may not be a bad trade off but for me, I always consider if the company is going to go out of business (as most robotics companies do).  How long will the product work for after the fact?  Sometimes this happens and only then you find that the product has a phone home feature that is now broken since the company domain no longer exists.  I'm not sure that will be the case here but something similar happening is a very real risk.

But then again, for €1300 some may not be concerned.

One last thing: Despite the [Kickstarter Project](https://www.kickstarter.com/projects/mirobot/mirobot-6-axis-mini-industrial-robot-arm/description) (where this product came from) had 676 backers, the community today seems to be only slightly active.  The [WLKata technical blog](http://discuz.wlkata.com/) has been down everytime I've checked this week.  All community activity seems to be limited to [this Facebook group](https://www.facebook.com/groups/2230775393806711).

_Note: I've seen that Facebook group sell used Mirobots for as low as €350_

* Homepage: [https://www.wlkata.com](https://www.wlkata.com)
* YouTube Channel: [https://www.youtube.com/channel/UCrneaL8hjWUxmvFHinFEicw](https://www.youtube.com/channel/UCrneaL8hjWUxmvFHinFEicw)
* Facebook Group: [https://www.facebook.com/groups/2230775393806711](https://www.facebook.com/groups/2230775393806711)

## Niryo Ned and Niryo One

![](/assets/images/posts/2021/niryo-ned.png)

The Ned seems to be the improved successor to the [Niryo One](https://niryo.com/product/niryo-one/).  Both have the appearance of serious robotic arms but the grainy 3D printed parts do detract from the overall look.  Apart than that, the Niryo gets high marks for openness, documentation and community.

Similar to the Annin Robotics AR2/AR3, the Niryo One and Ned have a [pleasant not-too-loud operating decibal](https://www.youtube.com/watch?v=ApUPwGgPTrc) but one thing to note is the limited payload weight of only 300g.  This limit is surprising given the size and maturity of the arm.

The company has a fair website although it's often in French and one would have to use Google translate to better understand the site.  The [forums](https://niryo.com/forum/) appear to be active and available documentation and videos seem to be thorough.

[The founders and the team](https://www.youtube.com/watch?v=gCnSqbWwHbQ) appear genuinely sincere about their work and have been improving their arm since 2016 which I assume is now reflected in their new Ned model.  Generally, this goes a long way in product quality and post purchase support.

I would like to have a Niryo Ned myself but the €2500 price is hard to swallow.  That is >€1000 more than the Mirobot for only slighter bigger payload and reach.  The assemble-it-yourself version is €1599 which is comparable to the Annin Robotics AR2/AR3.  This makes this arm the most expensive pre-assembled of the three covered here.

I couldn't find any secondary markets for the Niryo arms - nothing on Facebook or eBay AFAICT.

* Homepage: [https://niryo.com/](https://niryo.com/)
* YouTube Channel: [https://www.youtube.com/channel/UCJoa4NQR3B0YMwh9GuuDTJw](https://www.youtube.com/channel/UCJoa4NQR3B0YMwh9GuuDTJw)

## Comparison Rundown 

| Detail                 | AR2/AR3 | Mirobot | Niryo Ned |
| ---------------------- | --- | ------- | --------- |
| Degrees of Freedom     | 6 | 6 | 6 |
| Supports 7th DOF       | Yes | Yes | No|
| Max Payload            | 1,9kg | 0,15kg | 0,3 kg |
| Repeatability          | 0,75mm | 0,2mm | 0,5mm |
| Reach                  | 62,9cm | 23,5cm | 44cm |
| Main Board             | Mega 2560 | Mega 2560 | Raspberry pi 4 |
| Opensource STL Files   | Yes | No | Yes |
| Opensource Software    | Yes | No | Yes |
| Requires Assembly      | Yes | No | No |
| Origin                 | America | China | France |
| Price                  | ~€1500* | ~€1300 | €2499 |

_* Total Price of the AR3 can vary based on component sourcing.  See the [available kits](https://www.anninrobotics.com/robot-kits) on the Annin Robotics site for a good idea but remember to also add in the electrical components since a kit for those doesn't exist._

## Summary

Up to this point, my favorite arm is the Annin Robotics AR3 but the build requirements give me pause.  My second choice would be the Niryo Ned mostly for the equivalent openness and the company behind it (but that price...).  The WLKata Mirobot offers a fine design and functionality for a modest price but my top concern there is that key parts are closed source and confidential.  The company disappearing will very likely happen given enough time.

Then again, you can find used WLKata Mirobots for as low as €350 on Facebook and eBay which is hard to ignore.

Update: I found a used WLKata Mirobot on eBay for half price so I picked it up.  A pre-assembled, fully programmable and very functional robotic arm for €700-800 is not bad even if it lacks some openness.   It should arrive in a couple weeks.  Despite finding this deal, I'm still thinking of undertaking the AR3 build.  We'll see.

Note: This article contains all of the info I could gather in a few days of research.  It should all be correct but some bits may not.  Feel free to contact me for any innaccuracies.
