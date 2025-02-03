# Congress Self Organized Session to iOS Calendar

_Creates a calendar event from the content of the info page of a self organized session at the CCC_

At the moment, the Self Organized Sessions cannot be viewed directly in the Fahrplan app, as in: those cannot be bookmarked in any useful way. So this project came to be to help (at least iOS users) getting those into the own calendar.

[TOC]

## How to use it
1. Open the info page of a self organized session in Safari
2. Open the share menu and tap the Shortcut name
3. The Calendar app opens and shows you the newly created event

## How to install it
- [Via iCloud](https://www.icloud.com/shortcuts/acde3f799de84800aa08de88491b71f5)
- by downloading the the binary Shortcut file from Apple
    1. [Go to the JSON-description page of the shortcut](https://www.icloud.com/shortcuts/api/records/acde3f799de84800aa08de88491b71f5)
    2. Find the key "fields.shortcut.value.downloadURL" and download that URL
- by compiling the source Shortcut file yourself and installing it manually
    - there is the XML source: `C3 SoS to Calendar.xml`
    - there is the JSON source: `C3 SoS to Calendar.json`

### How the JSON and XML sources were generated
I used [this gist](https://gist.github.com/0xdevalias/27d9aea9529be7b6ce59055332a94477) for background info
1. Download the download URL
3. plutil -convert xml1 -e 'C3 SoS to Calendar.xml' -- 'C3 SoS to Calendar.plist'
4. plutil -convert json -e 'C3 SoS to Calendar.json' -- 'C3 SoS to Calendar.plist'

## You just came here to get the extraction logic to build your own helper?
- If you found this because you want to fix the software of the hub: add an ics Button to download an ICS file for the event. Also a JSON-LD document in the page source would be nice.
- If you found this because you want to fix the software of the Fahrplan: detect SOS pages as part of the schedule and allow bookmarking for those.

This is what you are looking for:
```javascript
// get starting time
var timeStartStr = document.querySelector(".hub-event-details__time").childNodes[0].nodeValue.trim();

// get ending time
var timeEndStr = document.querySelector(".hub-event-details__time").childNodes[4].nodeValue.trim();

// get day number
var dayStr = document.querySelector(".hub-event-details__day").textContent;
var day = parseInt(dayStr.replace(/^(Day|Tag) /i, ""));

// determine current year
var currentYear = parseInt(document.location.pathname.replace(/^\/congress\/([0-9]{4})\//i,"$1"));

// build date from day 0 + event day
var dateStart = new Date(`${currentYear}-12-26T00:00:00.000+01:00`);
dateStart.setDate(dateStart.getDate() + day);
var dateEnd = new Date(dateStart);

// change times of start date to actual starting time
var timeStartSplit = timeStartStr.split(":");
dateStart.setHours(timeStartSplit[0]);
dateStart.setMinutes(timeStartSplit[1]);

// change times of end date to actual ending time
var timeEndSplit = timeEndStr.split(":");
dateEnd.setHours(timeEndSplit[0]);
dateEnd.setMinutes(timeEndSplit[1]);

// get event title
var title = document.querySelector(".hub-head-main").textContent.trim();

// get event location
var location = document.querySelector("main.container.hub-content &gt; article &gt; div.hub-vlayout &gt; div.hub-row.hub-card &gt; div.hub-vlayout-l &gt; div &gt; h2.hub-section-title + div.hub-text").textContent.trim();

// get event description
var description = document.querySelector("main.container.hub-content &gt; article &gt; div.hub-vlayout &gt; div.hub-row.hub-card &gt; div.hub-vlayout-l &gt; div.hub-text &gt; section.hub-markdown").textContent.trim();

// return data to shortcut
completion({
    "title": title,
    "location": location,
    "start": dateStart.toUTCString(),
    "end": dateEnd.toUTCString(),
    "url": document.location.href,
    "notes": description
});
```

## Authors
* **Max Fuxjäger** - *Initial work* - [MaxValue](https://gitlab.com/MaxValue)

See also the list of [contributors](https://gitlab.com/MaxValue/c3-sos-icalender/-/graphs/main?ref_type=heads) who participated in this project.

## Project History
I was asked just before 38C3 to create a macro or similar to add SOS to the calendar. At that time I thought it had to be made with ics files and got lost. Some time after 38C3 i needed something to procrastinate with so I finally implemented this Shortcut.
