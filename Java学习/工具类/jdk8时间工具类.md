## Java 8 新的时间工具类

```java
package com.df.dkcs.diversion.core.uitls;

import java.time.LocalDateTime;
import java.time.ZoneId;
import java.util.Date;

/**
 * @author: zhaihongwei
 * @date: 2018/8/9 17:42
 * @Description: jdk 1.8 时间工具类
 */
public class DateTimeUtils {

    /**
     * Date转换为LocalDateTime
     *
     * @param date
     * @return
     */
    public static LocalDateTime convertDateToLDT(Date date) {
        return LocalDateTime.ofInstant(date.toInstant(), ZoneId.systemDefault());
    }

    /**
     * LocalDateTime转换为Date
     *
     * @param time
     * @return
     */
    public static Date convertLDTToDate(LocalDateTime time) {
        return Date.from(time.atZone(ZoneId.systemDefault()).toInstant());
    }

    /**
     * 获取指定日期的毫秒
     *
     * @param time
     * @return
     */
    public static Long getMilliByTime(LocalDateTime time) {
        return time.atZone(ZoneId.systemDefault()).toInstant().toEpochMilli();
    }

    /**
     * 获取指定日期的秒
     *
     * @param time
     * @return
     */
    public static Long getSecondsByTime(LocalDateTime time) {
        return time.atZone(ZoneId.systemDefault()).toInstant().getEpochSecond();
    }

    /**
     * 获取一天的开始时间
     *
     * @param time
     * @return
     */
    public static LocalDateTime getDayStart(LocalDateTime time) {
        return time.withHour(0)
                .withMinute(0)
                .withSecond(0)
                .withNano(0);
    }

    /**
     * 获取一天的结束时间
     *
     * @param time
     * @return
     */
    public static LocalDateTime getDayEnd(LocalDateTime time) {
        return time.withHour(23)
                .withMinute(59)
                .withSecond(59)
                .withNano(999999999);
    }

    /**
     * 获取当天剩余的秒
     *
     * @param time
     * @return
     */
    public static Long getDayLaveSeconds(LocalDateTime time) {
        return (getSecondsByTime(getDayEnd(time)) - getSecondsByTime(time));
    }

    /**
     * 获取当天剩余的毫秒
     *
     * @param time
     * @return
     */
    public static Long getDayLaveMilli(LocalDateTime time) {
        return (getMilliByTime(getDayEnd(time)) - getMilliByTime(time));
    }
}

```

