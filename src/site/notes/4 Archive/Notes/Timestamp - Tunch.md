---
{"dg-publish":true,"permalink":"/4-archive/notes/timestamp-tunch/"}
---

tags:: #output/presentation [[3 Resources/Programming\|Programming]] [[4 Archive/Notes/Timezones\|Timezones]] [[4 Archive/Lab900\|Lab900]]
[[0 Inbox/Timestamp - Tunch - Brainstorm\|Timestamp - Tunch - Brainstorm]]

# Ceci n'est pas un(e) timestamp

---

# Once upon a time

```typescript
const oneDay = 1000 * 60 * 60 * 24;
const oneWeek = oneDay * 7;

const oneWeekFromNow = new Date(
  Date.now() + oneWeek
);
```

---

# wat

```typescript
const start = new Date(2022, 2, 25);

start; // Sun Mar 27 2022 00:00:00

const end = new Date(
  start.getTime() + oneDay
);

end // Mon Mar 28 2022 01:00:00
```

---

![Pasted image 20221007092510.png](/img/user/0%20Inbox/Pasted%20image%2020221007092510.png)

---

```typescript
const start = new Date(2022, 9, 30);

start // Sun Oct 30 2022

const end = new Date(
  start.getTime() + oneDay
)

end // Sun Oct 30 2022
```

---

...and we're not even talking about different time zones, or when we *do* care about time.

![](http://i1.kym-cdn.com/photos/images/original/000/251/734/cfb.jpg)

---

### Our lord and savior Tom Scott?

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/-5wpm-gesOY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## Meeting our mentor

![understanding-timezones.jpg](/img/user/0%20Inbox/understanding-timezones.jpg)

---

## Let's do this shit

![](https://preview.redd.it/10zutid8u6651.jpg?auto=webp&s=e53fd087e06d35092edb7e97463d04a0fa5f78d8)

## Everything is UTC

---

Demo: picking a UTC date in different time zones

---

### What could possibly go wrong?
- Britain starts DST earlier
- Southern hemisphere DST is swapped
- Samoa skipped December 30, 2011
- Countries change timezone quite often
- Libya cancelled DST in 2013
- Britain: until 16th century New years was on March 25th
- West Bank: time zone depends on political affiliation
- Leap seconds

---

## Two Realizations

Clock time ≠ moment in time. *This is not a timestamp.*

Length of second/day/month/year depends on time zone.

---

## oh btw

every country wants a different date format

12h / 24h

month first / day first

is a comma really a comma?

plurals?

currencies?

---

Do I really have to do this?

---

<video data-autoplay controls><source src="just-let-go.mp4" type="video/mp4"></video>

---

## Java

```java
java.util.Date; // bad
java.time.* // good
```

[The Date Time API Overview - dev.java](https://dev.java/learn/date-time/intro/) often forces you to think before you do stupid things.

- Instant: Storage
- LocalDateTime: When only "wall clock" time counts
- OffsetDateTime: Okay for single point in time
- ZonedDateTime: For correct calculations

---

## JavaScript

`Date` is being replaced by `Temporal`. Similar API to the Java one.

Basically: show it in the right format and zone. Angular mostly takes care of this.

[Angular's Pipes](https://angular.io/guide/i18n-common-format-data-locale)
- DatePipe
- CurrencyPipe
- DecimalPipe
- PercentPipe

Angular locale data is not always correct 😭

---

# Localization in the browser
Locale negotiation.

Locale ≠ Language.

---

## Plain JavaScript: Intl

[Intl](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl)
- Locale
- [DateTimeFormat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat/DateTimeFormat) (nl-NL vs nl-BE)
- [NumberFormat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat/NumberFormat)
- PluralRules
- RelativeTimeFormat

---

## Angular: LOCALE_ID

```typescript
// app.module.ts
import '@angular/common/locales/global/en';  
import '@angular/common/locales/global/nl';

providers: [
  { provide: LOCALE_ID, useValue: 'nl-BE' },
]
```

Or `useFactory: () => navigator.language`.

Or do your own negotiation.

---

# Conclusions

- Use the built-in packages, Luke
- Care about time
- Calculate time only with a zone
- Use `Intl` to format numbers, dates, plurals