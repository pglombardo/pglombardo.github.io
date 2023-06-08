---
layout: single
categories: ["Robotics"]
title: "Advice: Don't Buy the WLKata Mirobot Robotic Arm"
header:
  teaser: /assets/images/posts/2021/wlkata-mirobot.jpeg
---

![](/assets/images/posts/2021/wlkata-mirobot.jpeg)

The WLKata Robotic Arm is an impressive piece of hardware and design but unfortunately the decisions and actions that this Chinese company has taken makes this product unusable for anything beyond the most basic usage.

For the current price tag of ~€1700 and the headaches it will bring, it's far from worth it.

Instead of making this a post an endless list of complaints about WLKata, I'll just list the major points to be aware of and resources that you can research further for yourself.  I hope to save at least some people from the guaranteed headaches that will come when you discover when getting into the details of the product.

# The Problems with WLKata and the Mirobot

The crux of the problem is not the robotic arm itself, but instead the WLKata product decisions and how the company has treated it's community of users.

These are the points that you should be aware of before buying a Mirobot.

## Once Open, Now Closed Firmware

Here is a FAQ from the original Kickstater project:

![](/assets/images/posts/2023/kickstarter-wlkata-firmware.png)

In the Kickstarter project, the company promised to make their firmware opensource and it was for a short while but that is no longer the case.

The firmware is currently closed source and you have only an API and a glimmer of hope that it will work (it often doesn't).

This absolutely kills community development when trying to develop software, tools and libraries for the arm.  Developers are left with an API that works sometimes with no recourse in working around problems in the firmware.

## Valuable Community Forum Taken Offline without Explanation

The WLKata [official discussion forum](https://www.facebook.com/wlkatarobotics/posts/669632290511594/) (previously hosted at `http://discuz.wlkata.com/`) has been shut down and is no longer responsive.

This was an excellent source of information.  It was a place for community users to share discoveries and work-arounds.

I suspect that the company lost patience with the forum as large amount of negative feedback was being posted when they decided to close their firmware but this this just speculation.

In any case, the forum went dark one day long ago without explanation and all of it's valuable threads are gone.

## Opensource Behavior 

The company allegedly forked the community created Python library for the Mirobot, bundled it in their product without contributing back or mentioning the source as required by the opensource license. The `mirobot-py` author [shared his experience](https://github.com/rirze/mirobot-py/issues/23#issuecomment-1572466514) with WLKata and the Mirobot.

## All Kickstarter and Models before 2021 Can't Be Updated

This is a screenshot from the [WLKata product documentation](https://document.wlkata.com/?doc=/wlkata-mirobot-user-manual-platinum/32-appendix-ii-mirobot-firmware-update-tutorial/) as of June 2023:

![](/assets/images/posts/2023/wlkata-firmware-notice.png)

All models before January 1, 2021 are no longer supported and cannot be updated.

This includes everyone who supported the project in Kickstarter and everyone who paid the full post Kickstarter price tag (€2500).

What's worse is that this is completely unnecessary.  If the hardware and firmware were opensource, the community could work out the upgrade path for these older models quickly.

## Mac Support Dropped for their GUI Development Environment

Mac/OSX support for WLKata studio has been dropped.  This is their GUI based development environment for the arm.  Windows and Ubuntu are still supported

# The Solution (That Won't Happen)

Despite all of the negative points above, WLKata could fix this immediately by:

* Completely opensourcing their hardware & specifications
* Opensourcing their firmware in it's entirety
* Start working with the community instead of against it
* Enabling the community to serve itself instead of having WLKata as a unresponsive software bug chokepoint

If they were to do this, they would attract a much larger (and happier) community and in turn sell far more hardware.

But in my experience this won't happen.  This would take a fundamental philosophy change in the company.  I've never seen this happen ever in a company.  In the corporate world, past performance does indicate future performance.  How they've acted in the past is unfortunately most likely how they will continue to operate.

# Summary

There is more to be aware of but these are the major points and are all deal breakers in my opinion.

Robotics is hard enough on it's own without having to deal with closed source proprietary bugs and largely unsupported products that you have no recourse for.

I hope someone finds this post useful and helps you to make a better informed purchase decision.

If the Robotics community wants to accelerate it's productivity and innovations, we need opensource hardware, firmware and companies that understand this.

If you have any experience with the WLKata Robotic arm and would like to share, please comment [in this Github issue in the mirobot-py Python Library](https://github.com/rirze/mirobot-py/issues/23).

If you would like to make an addition or modification to this article, [create an issue here](https://github.com/pglombardo/pglombardo.github.io/issues/new).

# Links & Resources

* [WLKata Company Website](https://www.wlkata.com)
* [Facebook Page](https://www.facebook.com/wlkatarobotics/)
* [Kickstarter Comment Forum](https://www.kickstarter.com/projects/mirobot/mirobot-6-axis-mini-industrial-robot-arm/comments)
* [Mirobot on Reddit](https://www.reddit.com/r/mirobot/)
* [mirobot-py Python Library](https://github.com/rirze/mirobot-py)