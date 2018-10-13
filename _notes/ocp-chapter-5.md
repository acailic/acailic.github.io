---
title: OCP Review 5 - Dates,strings and localization 
layout: post
tags: [java, ocp, date, localization]
date: 2018-09-07
---
 

# Dates and Times in Java 8

---

- `LocalDate.now()`: date
- `LocalTime.now()`: time
- `LocalDateTime.now()`: date +time
- `ZonedDateTime.now()`: date + time + time zone

---

## Current Date


```java

LocalDate now = LocalDate.now();
System.out.println(now);

// 2015-11-08

```

- LocalDate: only date (not time)
- can't do `new LocalDate();`
- `import java.time.LocalDate;`


---

## Date with year, month, day

```java

LocalDate firstOfJanuary2015 = LocalDate.of(2015, Month.JANUARY, 1);
System.out.println(firstOfJanuary2015);
```

---

## LocalDate inmutable!

```java
LocalDate _12Oct1492_ = LocalDate.of(1492, Month.OCTOBER, 12);
_12Oct1492_.plusDays(800);
System.out.println(_12Oct1492_);								// 1492-10-12
LocalDate other = _12Oct1492_.plusDays(800);
System.out.println(other);										// 1494-12-21
```

---

## LocalTime

```java
LocalTime timeNow = LocalTime.now();
System.out.println(timeNow);									// 13:51:53.382

System.out.println(LocalTime.of(10, 10, 10));					// 10:10:10
```

- LocalTime: sólo tiempo
- `LocalDateTime`: fecha y hora

---


## LocalTime inmutable!

```java
LocalTime timeNow = LocalTime.now();
System.out.println(timeNow);

timeNow.plusHours(1);
System.out.println(timeNow);

// 19:57:24.995
// 19:57:24.995

```

- plusMinutes / minusMinutes
- plusSeconds / minus

---

## Intervals

```java
LocalDate start = LocalDate.of(2015, Month.APRIL, 10);
LocalDate end = LocalDate.of(2015, Month.APRIL, 20);
while (start.isBefore(end)) {
	System.out.println(start);
	start = start.plusDays(1);
}

2015-04-10
2015-04-11
2015-04-12
2015-04-13
2015-04-14
2015-04-15
2015-04-16
2015-04-17
2015-04-18
2015-04-19

```

---

## Periods

- Create task every week

```java

Period everyWeek = Period.ofWeeks(1);
start = LocalDate.now();
end = LocalDate.of(2016, Month.APRIL, 10);
while (start.isBefore(end)) {
	System.out.println(start);
	start = start.plus(everyWeek);
}

``` 
---

## Periods

Period.of(year, month, days)

```java
System.out.println(Period.of(3, 2, 1));
// P3Y2M1D
// P 3Y 2M 1D
```

---

## Duration

- like Periods, but for days, hours, minutes, seconds, milliseconds, nanoseconds

Duration.ofDays()

---

## Parse dates

```java

DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
LocalDate date = LocalDate.parse("2010/10/40", formatter);
System.out.println(date);

```
- can throw `java.time.format.DateTimeParseException` (Runtime)


---

## TimeZones

```java
System.out.println(LocalTime.now());
System.out.println(LocalTime.now(ZoneId.of("US/Pacific")));
System.out.println(LocalTime.now(ZoneId.of("Zulu")));
System.out.println(LocalTime.now(ZoneId.of("UTC")));
// console output

20:05:25.170
11:05:25.172
19:05:25.173
19:05:25.173
```

---

## UTC

The official abbreviation for Coordinated Universal Time is UTC. It came about as a compromise between English and French speakers.
- Coordinated Universal Time in English would normally be abbreviated CUT.
- Temps Universel Coordonné in French would normally be abbreviated TUC.

---

## GMT & UTC

Negative <-- GMT 0 == UTC 0 --> Positive


```java
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println(localDateTime);

ZonedDateTime zonedDateTimeUSPacific = ZonedDateTime.now(ZoneId.of("US/Pacific"));
System.out.println(zonedDateTimeUSPacific);

ZonedDateTime zonedDateTimeMadrid = ZonedDateTime.now(ZoneId.of("Europe/Madrid"));
System.out.println(zonedDateTimeMadrid);
```

---

## Print all timezones

```java
ZoneId.getAvailableZoneIds().stream().sorted().forEach(System.out::println);
```

---

## Daylight saving time

- last Sunday of March, we move from 1:59 AM -> 3:00 AM

```java
LocalDate change2018 = LocalDate.of(2018, Month.MARCH, 25);
LocalTime changeTime = LocalTime.of(1, 30);
ZoneId zone = ZoneId.of("Europe/Madrid");

ZonedDateTime time = ZonedDateTime.of(change2018, changeTime, zone);

System.out.println(time); 				// 2018-03-25T01:30+01:00[Europe/Madrid]
System.out.println(time.plusHours(1));  // 2018-03-25T03:30+02:00[Europe/Madrid]
// we add 1 h, but went from 01:30 --> 03:30!
```

---

## Localisation

- You need to know how a ResourceBundle is prepared for a particular locale when multiple properties files are available.
 The following JavaDoc API decription contains all you need to know: http://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.html#getBundle-java.lang.String-java.util.Locale-java.lang.ClassLoader-
  You may try executing the given code and see how the values are loaded. Keep the properties files in the base folder of your classpath. For example, if your classpath contains c:\javatest\classes, then keep the properties files in c:\javatest\classes.
- Keys are always Strings. So you cannot use an int to get value for a key.
- ResourceBundle is retrieved, changing the Locale will not affect the ResourceBundle. You have to retrieve a new ResourceBundle by passing in the new Locale and then assign it to the variable

- EX. : mymsgs.properties is the base file for this resource bundle. Therefore, it will be loaded first. Since the language and region specific file is also present (_en_UK), it will also be loaded and the values in this file will be superimposed on the values of the base file.

        Remember that if there were another properties file named mymsgs_en.properties also present, then that file would have been loaded before mymsgs_en_UK.properties.


-  In order for a program to load a resource bundle, the resource bundle properties file must be in the CLASSPATH.
      The JVM attempts to find a "resource" with this name using ClassLoader.getResource. (Note that a "resource" in the sense of getResource has nothing to do with the contents of a resource bundle, it is just a container of data, such as a file.) If it finds a "resource", it attempts to create a new PropertyResourceBundle instance from its contents. If successful, this instance becomes the result resource bundle. 

- A resource bundle file could be a properties file or a class file.  The abstract class ResourceBundle has two subclasses: PropertyResourceBundle and ListResourceBundle.  
    1. A PropertyResourceBundle is backed by a properties file. A properties file is a plain-text file that contains translatable text. Properties files are not part of the Java source code, and they can contain values for String objects only. If you need to store other types of objects, use a ListResourceBundle instead. 
    2. The ListResourceBundle class manages resources with a convenient list. Each ListResourceBundle is backed by a class file. You can store any locale-specific object in a ListResourceBundle. To add support for an additional Locale, you create another source file and compile it into a class file.  The ResourceBundle class is flexible. If you first put your locale-specific String objects in a PropertyResourceBundle and then later decided to use ListResourceBundle instead, there is no impact on your code. For example, the following call to getBundle will retrieve a ResourceBundle for the appropriate Locale, whether ButtonLabel is backed up by a class or by a properties file:  ResourceBundle introLabels = ResourceBundle.getBundle("ButtonLabel", currentLocale);


### Links Dates, Strings, and Localization

- [Dates and Times](https://github.com/acailic/java8-learning/blob/master/Java-8/src/datesStringsLocalization/DatesAndTimes.java) <br />
- [Formatting Dates and Times](https://github.com/acailic/java8-learning/blob/master/Java-8/src/datesStringsLocalization/FormattingDatesAndTimes.java) <br />
- [Formatting Numbers](https://github.com/acailic/java8-learning/blob/master/Java-8/src/datesStringsLocalization/FormattingNumbers.java) <br />
- [Internationalization and Localization](https://github.com/acailic/java8-learning/blob/master/Java-8/src/datesStringsLocalization/InternationalizationLocalization.java) <br />


## Question

- public static Duration between(Temporal startInclusive, Temporal endExclusive)
  1. Duration.between method computes the duration between two temporal objects.  If the objects are of different types, then the duration is calculated based on the type of the first object. For example, if the first argument is a LocalTime then the second argument is converted to a LocalTime. (Not the case here because both arguments are of ZonedDateTime).
  2. In this case, the difference between the two zones is 3 hours therefore the resulting duration will contain 3 hours. 
  3. The result of Duration.between method can be a negative period if the end is before the start. This is NOT the case here. Therefore, the Duration.between will return be plus 3 hrs and not minus 3 hours.

-  toString method of java.time.Duration works -  It generates a string representation of the duration object using ISO-8601 seconds based representation, such as PT8H6M12.345S. 
  1. Duration.between method computes the duration between two temporal objects. If the objects are of different types, then the duration is calculated based on the type of the first object. For example, in this case, since the first argument is an OffsetDateTime then the second argument will be converted to a OffsetDateTime. Now, remember that ZonedDateTime adds the functionality of Day light savings on top of OffsetDateTime. Thus, it is possible to convert a ZonedDateTime into an OffsetDateTime. The reverse is also possible. (If you try to do Duration.between(nyOdt, localDateTime);, it will throw java.time.DateTimeException: Unable to obtain ZonedDateTime from TemporalAccessor, at run time.)
  
  2. In this case, since DST is ON on 2nd Jun, the ZonedDateTime will be 1 hour ahead of the OffsetDateTime. Thus, the difference between the two is 1 hour.
  
  3. The result of Duration.between method can be a negative period if the end is before the start. This is indeed the case here. The offset of OffsetDateTime is -5, while the offset of ZonedDateTime is -4 (due to DST). Therefore, the Duration.between will return be -1H.