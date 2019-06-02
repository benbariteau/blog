---
title: "In Defense of Timezones"
date: 2019-05-14T23:13:00-07:00
draft: true
---

Timezones are a much maligned concept in programming circles. It's not really that hard to see why. First, time is relevant to almost every application. Second, it has a lot of complexity. Much of that complexity has to do with dealing timezones. My hope is that I can dispel some of the more pernicious beliefs about them and hopefully give context to make your next interaction a less frustrating one.

# Timezones from first principles

I've had multiple conversations with fellow coders that went something like this:

>"Ugh! I hate timezones. I think we should all just use UTC."

>"That sounds great! I'd love to not have to deal with timezones. But does that mean I got to work from 5PM to 1AM?"

>"Well obviously you could just have some sort of offset related to the relative position of the sun. Oh wait..."

I really like these converstations (although this one is unrealistically abbreviated), becuase it usually marks the beginnings of understanding timezones. So, let's try to get a deeper understanding!

# A brief history of timezones

Prior to timezones, clocks were calibrated by having noon be when the sun was at its zenith (i.e. its highest point in the sky). This was a pretty intuitive and straightforward way to tell time, but it becomes less useful once you want to synchronize events that are far away from each other. This became more and more relevant as communication and travel became faster and more common. So, Railway Time--the first version of timezones--was invented. Systems based on this standardization eventually became the UTC offset system we use today.

# UTC

You've probably heard of UTC[^1]. Just to be on the same page, UTC is a time standard which uses the zenith time in Greenwich, UK, which lies on the Prime Meridian (0 degrees longitude). This time is sometimes known as Greenwich Mean Time (GMT) or Zulu Time (referring to "Z"ero offset). Each time zone corresponds to at least one offset from UTC. This offset can change depending on the time of the year (usually because of daylight savings time). Because each offset is well-defined, it makes it easy to translate a time between different time zones, making coordinating events between locations a lot easier.

# Location, location, location

One thing that eludes most people about timezones is that they oftentimes think of the timezones themselves as the most fundamental subdivision. However, the most important thing is actually location. This actually bubbles up straight to the user! If you have a timezone selector, you'll usually see options like "America/Los\_Angeles (UTC-07:00)" or "America/New\_York (UTC-04:00)". You might be reading the offsets from UTC (which is important), but the it's actually important that you're picking a location. For example, if you decided to pick "America/Bogota" (the capital of Colombia) which is also UTC-05:00 during standard time, you'd end up being out of sync with the Eastern time zone in the US depending on the time of year, since they don't observe daylight savings time.

A lot of this has to do with the fact that rules around time are public policy which can depend a lot on what the government decides. As I mentioned before, some areas do not observe daylight savings times. Others observe it, but change at different times of the year. And some others are simply out of sync with the rest of the world. An example of this is All of China is one timezone, despite spanning almost 4 timezones worth of the earth (about 60 degress).

There is an argument here that more standardizatoin is in order, but the current rules are fairly entrenched. Additionally, some things that may seem "inelegant" at first glance often have pratical benefit. For example, many islands in the pacific have adjusted their timezones to be closer to their trading partners, even if it means being "incorrect" in terms of their longitude.

# UTC and unix time

So far we haven't really talked about computers that much, so let's fix that. Most programmers will be familiar with unix time, but I'll reiterate so we're on the same page. Unix time is a method of storing and representing time as the number of seconds (and sometimes nanoseconds) since January 1st, 1970 at 12AM UTC. A moment in unix time is simply a number, usually referred to as a "timestamp".

One very convenient property of timestamps is that they are very easily comparable, since they are just numbers. The higher the number, the later the date.

[^1]: Fun fact about UTC: it doesn't actually stand for anything! The English name is "Coordinated Universal Time" and the French name is "Temps Universel Coordonné". So the compromise was to make the abbreviation be "UTC", which doesn't work for either.
