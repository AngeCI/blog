---
date: "2026-08-11"
type: "post"
draft: true
title: "曆法轉換腳本開發日誌（一）"
translationKey: "calendar-conversion-script-1"
categories:
  - "Astronomy and Calendars 天文與曆法"
---

# 儒略日
儒略日（Julian day）的定義是自逆推儒略曆公元前 4713 年 1 月 1 日以來所累積的日數。這項資訊基本上只對天文學家和程式員有用，日常生活並不會用的到。
```js
/** jdToUnix (jd)
 * Convert a Julian day into a UNIX timestamp.
 * @param jd Julian day.
 */
function jdToUnix (jd) {
  return Math.floor((jd - 2440587.5) * 86400);
}

/** unixToJd (unix)
 * Convert a UNIX timestamp into a Julian day.
 * @param unix UNIX timestamp.
 */
function unixToJd (unix) {
  return unix / 86400 + 2440587.5;
}
```

# 公曆、儒略曆
## 每月日數
```js
/** numberOfDays (y, m)
 * Calculate the number of days in a given Gregorian month.
 * @param y Gregorian year.
 * @param m Gregorian month.
 */
function numberOfDays (y, m) {
  if (m == 1) return 31;
  else if (m == 2) return isLeapGreg(y) ? 29 : 28;
  // else return Math.floor((153 * m - 304) / 5) - Math.floor((153 * m - 457) / 5);
  // else return (((153 * m - 304) * 3277) >> 14) - (((153 * m - 457) * 3277) >> 14);
  // else return ((501381 * m - 996208) >> 14) - ((501381 * m - 1497589) >> 14);
  else {
    let a = 501381 * m - 996208;
    return (a >> 14) - ((a - 501381) >> 14);
  }
}
```

## 序數日期

```js
/** gregOrdinalDate (y, m, d)
 * Calculate the ordinal date from a given Gregorian date.
 * @param y Gregorian year.
 * @param m Gregorian month.
 * @param d Gregorian day.
 * @return Ordinal date, with 1st January = 1.
 */
function gregOrdinalDate(y, m, d) {
  if (m < 3) return (m - 1) * 31 + d;
  // else return Math.floor((153 * m - 457) / 5) + 59 + d + isLeapGreg(y);
  else return ((501381 * m - 1497589) >> 14) + 59 + d + isLeapGreg(y);
}
/** julianOrdinalDate (y, m, d)
 * Calculate the ordinal date from a given Julian date.
 * @param y Julian year.
 * @param m Julian month.
 * @param d Julian day.
 * @return Ordinal date, with 1st January = 1.
 */
function julianOrdinalDate(y, m, d) {
  if (m < 3) return (m - 1) * 31 + d;
  // else return Math.floor((153 * m - 457) / 5) + 59 + d + ((y & 3) == 0);
  else return ((501381 * m - 1497589) >> 14) + 59 + d + ((y & 3) == 0);
}
```

# 農曆
精確的農曆編算非常複雜，加上古代實曆的觀測精度沒有現代這麼準，大多數轉換程式用的還是查表。
