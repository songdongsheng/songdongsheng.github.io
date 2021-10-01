---
title: Java Offset DateTime Formatter
excerpt: Java Offset DateTime Formatter
date: 2020-01-11 22:17:27
tags:
  - Java
categories: [Programming, Java]
---

# Java Offset DateTime Formatter

```java
import java.time.Instant;
import java.time.OffsetDateTime;
import java.time.ZoneId;
import java.time.ZoneOffset;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.List;

public final class Bootstrap {
    public static void main(String[] argv) {
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SSSZ");
        Instant now = Instant.now();
        List<OffsetDateTime> dts = new ArrayList<>(3);

        OffsetDateTime current = OffsetDateTime.ofInstant(now, ZoneId.systemDefault());
        dts.add(current);

        current = OffsetDateTime.ofInstant(now, ZoneId.ofOffset("UTC", ZoneOffset.ofHours(5)));
        dts.add(current);

        current = OffsetDateTime.ofInstant(now, ZoneId.ofOffset("UTC", ZoneOffset.ofHours(0)));
        dts.add(current);

        for (OffsetDateTime dt : dts) {
            String str = formatter.format(dt);
            System.out.println(str);
        }
    }
}
```

```
2020-01-11T13:05:21.621+0800
2020-01-11T10:05:21.621+0500
2020-01-11T05:05:21.621+0000
```
