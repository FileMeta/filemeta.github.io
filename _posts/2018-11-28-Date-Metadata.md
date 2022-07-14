---
title: Best Practice for Date and Time Metadata
date: 2018-11-28
labels: metadata; date
descriptions: Challenges with date and time metadata, best practices, and how to compensate when best practices aren't followed.
author: Brandt Redd
---
<img style="float: right; margin: 5;" src="/postimg/dateTime.jpg" alt="Calendar and Clock" width="300" />

The story is told about a museum guest looking at a dinosaur skeleton. She asks the guide, "When did this dinosaur live?"

The guide answers, "One hundred twenty million and eleven years ago."

"Wow!" says the guest, "How do you know so precisely?"

"Well, when I started working here, they told me that the skeleton was 120 million years old and that was eleven years ago."

Knowing the precision of a date is important. The [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) standard date format is commonly used in metadata. When used properly, it conveys timezone and precision information. But, too often the latter parts are lost. In this post I'll explain how that information is transmitted, why it's important, how it gets lost, and some strategies I use for detecting and preserving these features.

## Relevant Standards

The Dublin Core listing for [*date*](http://dublincore.org/documents/dcmi-terms/#terms-date) defines the property as "A point or period of time associated with an event in the lifecycle of the resource." By this definition, the property includes time information, not only date. This applies to derivative DMCI properties such as *dateSubmitted* and *dateAccepted*. So, when you encounter a "date" property in metadata, the time is usually included.

[DCMI](dublincore.org) further recommends that dates are stored according to the [W3CDTF profile of ISO 8601](https://www.w3.org/TR/NOTE-datetime). The W3CDTF format usually conveys *time zone* and *precision*. This information is often missing when date and time are parsed into or stored in binary formats and we have to find ways to compensate.

## Recommendations and Samples

The following practices will ensure that timezone and precision are recorded and preserved.

**When storing *date* properties in W3CDTF and ISO 8601 formats, use the local time relevant to where the event occurred and include timezone information.**

For example, an event in New York City (Eastern Time Zone) might be timestamped as "2018-11-28T13:25:04-05:00" This is parsed as 28 November 2018 at 1:25:04 PM Eastern Standard Time (UTC minus five hours).

Under most circumstances, humans care about when something happened in the timezone where the event occurred. For example, if I'm looking at a photo of New York's Central Park, I prefer to be told the photo was taken 5:00 PM in New York (so sun is getting low) even though that was 2:00 PM in California. The airline industry has proven that this works best for consumers. Takeoff and landing times are always given in the time local to the airport.

Including the timezone lets us convert from local to UTC and from UTC to another timezone when needed. For example, if you are plotting a set of events that originate from different timezones on a calendar or timeline you would want to convert all events to the same timezone. Likewise, if people from different timezones are collaborating on a project, each would want to see times rendered into their local timezone. Including timezone in the metadata ensures that the application can render the time in whatever zone makes the most sense to the context.

When the time is deliberately expressed in UTC then W3CDTF/ISO 8601 indicates that you use "Z" for the timezone designator. For example, "2018-11-28T18:25:04Z" would be the time of the event listed above. Some systems standardize on always using UTC for internal storage because its consistent worldwide. Indeed, that practice is helpful in indexes where it makes sorting and searching more convenient. However, unless you also store the timezone in a separate property, you lose information about the *local* time of the event. So, best practice *for metadata* is to use local time plus timezone. UTC and the "Z" time zone designator should principally be used for legacy data when the timezone of the event is not known.

When the time is local, but the timezone is unknown, then leave off the time zone designator like this, "2018-11-28T13:25:04".

For one more example, consider an event that happens in London. The timezone is Greenwich Mean Time (GMT) which has zero offset from UTC. You could express this event as "2018-12-25T07:05:01+00:00" or as "2018-12-25T07:05:01Z". Both express exactly the same time. However, the first instance is a "Local Time" indicating that the event occurred in the GMT time zone - somewhere in the UK or at the same longitude. The second instance is UTC meaning that the event could have happened anywhere in the world and the timezone is not expressed.

Here's a summary:
<table>
<tr><th>Condition</th><th>Samples</th></tr>
<tr><td>Local time with timezone<br/>(Best Practice)</td><td>2018-11-28T13:25:04-05:00<br/>2018-12-25T07:05:01+00:00</td></tr>
<tr><td>Local time with<br/>unstated timezone.</td><td>2018-11-28T13:25:04<br/>2018-12-25T07:05:01</td></tr>
<tr><td>UTC time with<br/>unstated timezone.</td><td>2018-11-28T18:25:04Z<br/>2018-12-25T07:05:01Z</td></tr>
</table>

**When storing and parsing *date* properties in W3CDTF and ISO 8601 formats, tolerate partial values and preserve precision.**

For example, "1976" is a valid date-time in W3DTF format. It means that the event occurred sometime in the year 1976. By the same token, "1976-07-04T21:05:02.319-05:00" is also a valid date-time in W3DTF format. In indicates that the event happened on July 4, 1976 at precisely 9:05 PM and 2.319 seconds.

Unfortunately, most parsers would either reject the year-only value or they would convert it to January 1 1976 at precisely midnight. The former case doesn't work and the latter case conveys more precision than was intended.

Precision can be expressed as significant digits according to the following table:

<table>
<tr><th>Significant<br/>Digits</th><th>Precision</th><th>Example</th></tr>
<tr><td>4</td><td>Year</td><td>1976</td></tr>
<tr><td>6</td><td>Month</td><td>1976-07</td></tr>
<tr><td>8</td><td>Day</td><td>1976-07-04</td></tr>
<tr><td>10</td><td>Hour</td><td>1976-07-04T21-05:00</td></tr>
<tr><td>12</td><td>Minute</td><td>1976-07-04T21:05-05:00</td></tr>
<tr><td>14</td><td>Second</td><td>1976-07-04T21:05:02-05:00</td></tr>
<tr><td>17</td><td>Millisecond</td><td>1976-07-04T21:05:02.319-05:00</td></tr>
</table>

Notice that when the precision is finer than one day, then timezone should be included. At coarser granularity (day and above) the timezone is no longer relevant. Again, this is according to the W3CDTF standard.

Parsers for W3CDTF formatted dates should include a precision information in their output so that the application can preserve and make use of that detail. Formatters should accept precision information and generate the string accordingly.

**When using existing metadata properties that don't include timezone or precision, add that data in custom metadata properties.**

Many existing metadata formats have date properties that don't include precision or timezone information. Because existing applications recognize the existing properties, it's best not to substitute your own properties that follow the practices above. Rather, continue to store the time in existing properties and then add new custom properties to express the timezone and precision information.

Timezone information should be expressed as the difference, in hours and minutes, between UTC and local time. For example, Eastern Standard Time (EST) is "-05:00". Most, but not all, timezones are offset by whole hours. For those cases, the minutes can be left off. Leading zeros are also not required. Thus, "-5" may be an acceptable shortened value for EST in a precision property. However, W3CDTF requires the full value - leading zeros and minutes.

Precison should be expressed in terms of significant digits when the date and time are rendered in ISO 8601 format. When counting digits, do not include the hyphen, colon, or "T" characters. The table above shows typical values.

## Example: Photo Management

To illustrate the problems that occur when these practices aren't followed, I'll use the photo management project I'm currently working on. I have a collection of more than 100,000 family photos and short videos. The photos are all in [JPEG (.jpg)](https://en.wikipedia.org/wiki/JPEG) format with [EXIF](https://en.wikipedia.org/wiki/Exif) metadata. Videos are in a mix of [Audio Video Interleave (.avi)](https://en.wikipedia.org/wiki/Audio_Video_Interleave), [QuickTime (.mov)](https://en.wikipedia.org/wiki/QuickTime_File_Format), and [MPEG-4 (.mp4)](https://en.wikipedia.org/wiki/MPEG-4_Part_14) formats.

For JPEG-EXIF images, the relevant date property is EXIF:DateTimeOriginal. According to the EXIF standard, the property is in ISO 8601 format but does not include the timezone suffix and should be rendered in local time - that is, in the time zone in which the photo was taken.

Both MP4 and Quicktime (.mov) video files use the [ISOM Format](https://en.wikipedia.org/wiki/ISO_base_media_file_format). For these files, the relevant property is *creation_time* which is stored internally in binary form. According [the ISOM specification](http://standards.iso.org/ittf/PubliclyAvailableStandards/c068960_ISO_IEC_14496-12_2015.zip), *creation_time* and other date properties are in UTC.

Neither of these formats include timezone information by default. EXIF defines an optional timezone property but I haven't found any file samples that include it. Many cameras don't have a timezone setting. For example, the Fuji camera I have only has a local time setting. That's no problem for JPEG files but for video files (in Quicktime .mov format) the Fuji camera fills in *creation_time* with the local time even though the property is supposed to be UTC. UTC is not possible because the camera doesn't have timezone information.

My Canon camera does have a timezone setting. For photos (in JPEG format) it fills in DateTimeOriginal with the local time, as expected. For videos (in .MP4 format) it fills in *creation_time* in UTC. In both cases, the Canon camera includes timezone information in a proprietary Canon property as part of the [*Makernote*](http://www.exiv2.org/makernote.html).

## The Timezone Problem

Consider the [slideshow program](https://github.com/FileMeta/FMSlideShow) I'm working on. It should display photos and videos interleaved in the order they were taken. If I use my Canon camera, and the pictures were taken near my home, then the timestamps on the photos and videos will be consistent.

Lets say I go on vacation to Florida; I live in Mountain time. If I fail to set the timezone correctly on my Canon camera (which is often the case) then the photos and videos will display in the right order when I get home. However, the timestamps I show will be off by two hours. On the other hand, if I set the timezone correctly on my camera then, when I get home, the photos and videos will not be consistent with each other. Let's say I take a photo at 11:00 (while in Florida) and I take a video at 11:01. When I return home to Mountain time, my computer will see the photo at 11:00 because it was stored in local time. The video will show as 9:01 local time. That's because the video was timestamped at 16:01 UTC (Florida's Eastern time zone is UTC-5) and in Mountain time (UTC-7) the timestamp is interpreted as 9:01 local time. So, the video is treated as if it was two hours before the photo instead of one minute after.

## Compensating for the Timezone Problem

These solutions are restricted to the photo and video case. For other media, other strategies may be required.

In most cases, the goal is to discover the timezone in effect at the time of the event and store that in a custom metadata property. Then use the existing metadata properties as they are defined - that is to use UTC or Local time according to the file format specification. When converting between UTC and local time, use the timezone from the metadata rather than the timezone setting of the computer. Doing this will result in consistent local or UTC values for both photos (which store local time in the metadata) and videos (which store UTC).

For Canon, and other cameras that store timezone in the Makernote, the timezone can be extracted using Phil Harvey's [ExifTool](https://sno.phy.queensu.ca/~phil/exiftool/). 

Most cameras store photos on flash media formatted using the [FAT](https://en.wikipedia.org/wiki/File_Allocation_Table) file system. Conveniently, FAT uses local time. So, by comparing the metadata time against the file system time, you can detect whether whether a video *creation_time* is in local or UTC. If it's in UTC then the timezone can be detected from the difference.

When comparing date-times you must take into account inconsistent resolution. On the FAT file system, Creation DateTime has a 10ms resolution while Modification DateTime has a 2 second resolution. ISOM DateTime values have a one-second resolution.

As files are copied from removable media to local drives, then synced to the cloud, and so forth, the file system times can be changed. Or, if the file is copied to a file system that stores UTC, and then the timezone of the computer is changed, then the timestamp will also shift. So, it's important detect timezone early and then store in a custom metadata property before that opportunity is lost. That's among the tasks of the [FMPhotoFinisher tool](https://github.com/FileMeta/FMPhotoFinisher).

## Compensating for the Precision Problem

Many of my photos come from the pre-digital era and I am slowly digitizing them. In the latter part of the 1990s, I had a camera with a data back that embossed the date on the photo. But the resolution of that information is one day. Meanwhile, the EXIF DateTimeOriginal property has second-level resolution. For earlier photos, it's even more difficult to determine when they were taken. Some of my slides are stamped with the year and month they were developed. For certain photos, all I can guess at are the year and the season.

For compatibility with existing applications that use date metadata, the date should be stored in the existing DateTimeOriginal property. Like Timezone, we add a custom "datePrecision" metadata property and use the number of significant digits from the table above. An application that recognizes the DatePrecision property ignores all detail below that level. When writing the value, use all zeros (or 1's for month and day) for sub-precision components. So, an event in February 2015 with month-level precision would be rendered as "date: 2015-02-01T00:00:00", "datePrecision: 6".

There is no reliable way to detect precision from existing file metadata. But when users enter date information the UI should enable them to leave out levels of detail and the precision would be determined from there.

## Wrapup

Many metadata properties were defined with an incomplete understanding of the use cases. When defining date properties, it seems reasonable to use ISO 8601 format out to the second level rendered into UTC. However, closer examination shows that doing so loses two critical pieces of information that careful use of ISO 8601 can preserve: precision and timezone.

The same basic principles can apply to other properties. When defining data elements, always consider the full set of information being conveyed and make sure that the format doesn't lose some of that information. 