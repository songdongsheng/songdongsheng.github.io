---
title: Time of Haskell
excerpt: Time of Haskell
date: 2021-04-04 11:18:47
tags:
    - Programming
    - Haskell
categories: [Programming, Haskell]
---

# Time of Haskell

Handling timezones correctly, along with the distinction between timestamps and calendar dates is an endless source of bugs. Being precise about which *exact type* of time data your program takes in and spits out can completely obviate that problem.

The de-facto standard date and time library in Haskell, [time](https://hackage.haskell.org/package/time), however, can be a little obtuse to get started with. Here’s a cheatsheet for the most common use cases for the time library.

## Importing and using time library

Add to your *package.yam*l file:

```yaml
dependencies:
  - time
```

In modules where you need to work with date/time data:

```yaml
import Data.Time
```

In GHCi:

```shell
Prelude> import Data.Time
```

The **primitive** type in the time library is [UTCTime](https://hackage.haskell.org/package/time/docs/Data-Time-Clock.html#t:UTCTime). If you need to do anything involving the current time, it will most likely involve some conversion from/to this type. As the name suggests, it’s a timestamp in the UTC timezone.

## Get time resolution

```shell
Prelude> import Data.Time
Prelude Data.Time> getTime_resolution
0.0000001s
```

## Getting the POSIX time

```shell
Prelude> import Data.Time.Clock.POSIX
Prelude Data.Time.Clock.POSIX> getPOSIXTime
1628354752.8489337s
```

## Getting the current time

```shell
Prelude> import Data.Time

Prelude Data.Time> getCurrentTime
2021-04-03 16:22:38.674402 UTC
```

## Getting the system time

```shell
Prelude> import Data.Time.Clock.System
Prelude Data.Time.Clock.System> getSystemTime
MkSystemTime {systemSeconds = 1628354836, systemNanoseconds = 564557100}
```

## Working with time

```shell
Prelude> import Data.Time
Prelude Data.Time> fromGregorian 2020 2 31
2020-02-29

Prelude Data.Time> fromGregorianValid 2020 2 31
Nothing

Prelude Data.Time> fromGregorianValid 2019 10 7
Just 2019-10-07

Prelude Data.Time> start <- getCurrentTime
Prelude Data.Time> now <- getCurrentTime
Prelude Data.Time> diffUTCTime now start
6.6531334s

Prelude Data.Time> now
2021-04-03 16:37:51.7175715 UTC
Prelude Data.Time> addUTCTime 5 now
2021-04-03 16:37:56.7175715 UTC
```

## Formatting dates and times

```shell
Prelude> import Data.Time
Prelude Data.Time> now <- getCurrentTime
Prelude Data.Time> now
2021-04-03 16:32:43.4961349 UTC
Prelude Data.Time> formatTime defaultTimeLocale "%_Y-%m-%dT%H:%M:%S.%q%z" now
"2021-04-03T16:32:43.496134900000+0000"
```
