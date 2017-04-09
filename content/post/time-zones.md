+++
date = "2016-01-16T11:01:05+02:00"
title = "Time zones for Developers"
draft = true

+++
I spend more time than I ever wanted over the last decade dealing with time zone related problems. Most of them were caused by bugs introduced by developers (including myself) who didn't know enough about it.

This post is here to help with that: It's not a complete coverage of time zones or an introduction. It's based on my experience what the most common mistakes are and what is least understood. I will not cover specific languages or libraries: These are general informations, which apply to almost any environment.


##### Time zones

Every location on earth has a time zone associated with it.

For example Berlin has "Europe/Berlin" and San Francisco has "America/Los_Angeles".

Every time zone has an offset, which describes how one time zone relates to another.
The offset of the time zone is normally expressed as difference to "UTC" (Greenwich). For example "America/Los_Angeles" has the offset "UTC-8" in the winter. 


There is no global authority or formal process how a time zone is assigned. Every country decides for itself.
But there is a time zone database maintained by the IANA, which documents all timezones: https://www.iana.org/time-zones. This database is the source for almost all libraries dealing with time zones.

One warning about the format: Time zones in the format `Etc/GMT` are especially nasty: `Etc/GMT+10` is not `UTC+10` but `UTC-10`!

Because time zone rules can change at any time, the list of time zones should be updated every now and then (by updating the language/library used). But to be honest: The practical consequences of out-dated time zones rules are limited to use cases with rather exotic time zones. The most important time zones (US, Europe, China, India, Japan etc) don't change very often.

##### Time zone vs offset

One common misconception is that the time zone is 'UTC-XX'. Technically this might cause no bugs currently for some zones, which don't use daylight saving time, but it's still wrong.

For example San Francisco has "UTC-8" in the winter, but "UTC-7" in the summer. Using a fix offset instead of the name is wrong and will probably cause bugs.

Also just because "UTC+8" is the current fix offset for Beijing, it's not guaranteed to be true tomorrow. As mentioned above, time zone rules are political decisions and can be changed at any time.

**So please use a full time zone name whenever possible.**

##### Time representations

The two most common ways to express a time is the ISO-8601 format or the unix timestamp: (And of course there a countless custom variations)

[ISO-8601](https://en.wikipedia.org/wiki/ISO_8601): `2016-01-04T19:55:16Z` 

Unix timestamp (in seconds:) `1451937316` 

A few things about that:

- It's just a representation: Changing the offset for the ISO-8601 string doesn't change the actual time. For example `2016-01-24T10:00:00Z` is the same time as `2016-01-24T:15:00:00+05:00`

- The offset of the the ISO-8601 string is not a real time zone. As mentioned before, an offset is not enough. Just looking at such a representation doesn't say anything about any time zone. 

- Again about the offset: To represent a specific point in time you need an offset. There is no real concept of time without an offset to UTC in practice (even if it is just offset `+0` = `UTC`). But in theory time has nothing todo with our notion of offset. This is just how we choose to communicate. 

- Often time representation and UI view concerns are mixed up: For example when a web app should display a time, it's expected that the time, which comes from server, is represented with the offset in which it should be shown. This is not good: The information which exact time is meant is something completely different from how the time is shown to the user. The time zone in which a time is shown should always be stored separately.

- The Unix timestamp includes a time zone: It's the seconds (or milliseconds) elapsed since `01.01.1970` in UTC. Using the unix format won't fix magically time zone problems. I have heard it more than once: Just use "unix time". No ... just because the time zone is not directly visible, does not mean you can ignore it.

- I recommend to use the ISO-8601 format over the unix timestamp. Because it's human readable and includes a explicit time zone info, which makes it harder to ignore.

##### A time without an offset is not a real time

This might be the single biggest cause for time zone related bugs: Developers just forgetting about the time zone.

Technically a date or time without an offset is not a real time. With "real" time I mean a specific point in time (past or future). For example for `2018-11-20` it's not clear if this is the day in Tokyo or in Paris. You have to add a offset.

What happens a lot is that developers just ignore or forget the think about the time zone. But then the library used to construct a date (or to print a date) assumes some kind of default time zone. Normally this the default time zone of the browser or the default time zone of the server. 

I really think this a big API design flaw: There should never be a implicit time zone. I highly recommend always be explicit about the time zone, even if the default one is the right one. In pseudo code: `printNow(TimeZone.getDefault())` is just a lot better than `printNow()`.


##### When you really don't want a time zone  

Sometimes a time zone is really not relevant. What this means is that it's not important what specific point in time it really is. This is sometimes called a "Local Time" or "Local Date". 

For example a date of birth often does not need a time zone. It's enough to know that somebody is born on `20.07.1980` and if this is `20.07.1980` in Spain or Japan is simply not relevant.

This concept should not be confused with "print something in local time". For example the arrival time of a flight is always shown in local time, but it's still a specific point in time and not a "Local Time".

One important consequence is that a "Local Time" can't be compared to a real time. The question if a "Local Time" is before or after an real time can't be answered because it's not the same thing. If such a comparison is needed, it's not a local time.

The problems arise when it's treated as a real time. Even if there are UI elements for the user (date/time pickers) to make it seem like a real time it should not saved in the same way. Instead I recommend to save a local time as a String. 

If a real date is needed it should be converted locally and explicitly for every case and String should be used everywhere else including transportation and persistence.

If the persistence layer really needs a real date it also should be converted just before and after saving/loading. But it's better to save a String if possible.

I have seen a lot of hacks and workarounds to fix time zone problems with something like "convert this here now to UTC" which where all caused by the mistake to save a local date as a real date. 

Some libraries/languages have special "Local Date" support (e.g. Joda-Time in Java or Java 8 built-in). They can be useful but I still prefer to use a String. Because it's likely that at one point it needs to be converted into JSON or saved in a DB or serialized in some format and than it will be converted into a real date object with some kind of default time zone without thinking about it.


##### Debugging time zone problems

Of course every environment or system is too unique to have concrete rules for finding time zone problems.

But the general pattern is to look at every point where a conversion is happening. 
For example: A user is entering a date in text field, a JavaScript date object is created (1.), send to the server (2.), received at the server (3.), mapped (4.), saved in the DB (5.) and then the whole way back in reverse. 
Most of the time there are half a dozen or more places to check. The tricky part is not to miss a single one and to understand every single one. Very often these conversions happen internally in some libraries with some default time zone. 

This all adds up and it can happen that it takes days to analyse and fix a time zone problem.

##### Conclusion

Time zone bugs are very hard to fix and should be avoided in the first place. 
**The most important rule to achieve that is to make everything involving time zones very explicit.**

Often the root cause is not enough attention and/or not enough experience/knowledge. I hope this text helps in with that.

Please comment on this article or reach out to me on twitter if you have feedback: [@andimarek](https://twitter.com/andimarek).


