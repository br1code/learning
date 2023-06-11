# NodaTime

https://www.nuget.org/packages/NodaTime

## Simple Explanation

NodaTime is a .NET library that provides an alternative to the built-in DateTime and DateTimeOffset types. It aims to make working with dates and times in .NET easier and more reliable by providing a comprehensive set of types and operations that reflect the complexities of real-world time-keeping.

## Deep Explanation

NodaTime is an open-source .NET library that provides an alternative to the built-in types for working with dates and times. It was developed by Jon Skeet and other contributors, and is based on the Java library JodaTime.

One of the key goals of NodaTime is to provide more accurate and reliable types for working with dates and times than the built-in DateTime and DateTimeOffset types. These types have some limitations and quirks that can make them difficult to work with, especially when dealing with time zones, daylight saving time, and other complexities of real-world time-keeping.

NodaTime provides a comprehensive set of types for working with dates, times, and time zones, including:

- LocalDate: Represents a date without a time or time zone.

- LocalTime: Represents a time without a date or time zone.

- LocalDateTime: Represents a date and time without a time zone.

- Instant: Represents a point in time, with no concept of time zone or calendar.

- ZonedDateTime: Represents a date and time in a specific time zone.

- OffsetDateTime: Represents a date and time with an offset from UTC.

- Duration: Represents a length of time.

- Period: Represents a length of time in years, months, and days.

- Interval: Represents a range of time between two instants.

NodaTime also provides a rich set of operations and utilities for working with these types, including arithmetic operations, parsing and formatting, time zone conversions, and more.

## Examples

Creating a LocalDate:

```C#
LocalDate date = new LocalDate(2022, 4, 20);
```

---

Creating a LocalTime:

```C#
LocalTime time = new LocalTime(13, 30, 0);
```

---

Creating a LocalDateTime:

```C#
LocalDateTime dateTime = new LocalDateTime(2022, 4, 20, 13, 30, 0);
```

---

Creating an Instant:

```C#
Instant instant = Instant.FromUtc(2022, 4, 20, 13, 30, 0);
```

---

Creating a ZonedDateTime:

```C#
DateTimeZone tz = DateTimeZoneProviders.Tzdb["America/New_York"];
ZonedDateTime zonedDateTime = new ZonedDateTime(new LocalDateTime(2022, 4, 20, 13, 30, 0), tz);
```

---

Adding a duration to a LocalDateTime:

```C#
Duration duration = Duration.FromHours(1);
LocalDateTime newDateTime = dateTime.Plus(duration);
```