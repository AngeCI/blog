---
date: "2026-08-11"
type: "post"
draft: true
title: "曆法轉換腳本開發日誌（之一）"
translationKey: "calendar-conversion-script-1"
categories:
  - "Astronomy and Calendars 天文與曆法"
---

<!--
> [!IMPORTANT] 想立刻體驗嗎？
> - 範例：[點我](https://gh.ltgc.cc/astro/)
> - 開發文檔：[點我](https://kb.ltgc.cc/astro/)
-->

# 簡單週期
## UNIX 時間戳及儒略日
[UNIX 時間戳](https://zh.wikipedia.org/wiki/UNIX%E6%97%B6%E9%97%B4)的定義是自 1970 年 1 月 1 日以來所累積的秒數，在 JavaScript 中可以直接透過 [`Date.prototype.getTime()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime) 結合 [`Date.prototype.getTimezoneOffset()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getTimezoneOffset) 提取。

[儒略日](https://zh.wikipedia.org/wiki/%E5%84%92%E7%95%A5%E6%97%A5)（Julian day）的定義是自逆推儒略曆公元前 4713 年 1 月 1 日以來所累積的日數。這項資訊基本上只對天文學家和程式員有用，日常生活並不會用的到。這項數值與 UNIX 時間戳有簡單的線性關係：

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

## 試算表日期
```js
function jdToSpreadsheet (jd) {
  return jd - 2415019.5 + (jd >= 2415079.5);
}
function spreadsheetToJd (d) {
  return d + 2415019.5 - (d > 60);
}
```

## 星期數
```js
/** wday (jd)
 * Calculate the day of week from the given Julian day. 0 = Sunday.
 * @param jd Julian day.
 */
function wday (jd){
  return Math.floor(jd + 1.5) % 7;
}
```

## 干支
### 年干支
```js
const tmgj = "甲乙丙丁戊己庚辛壬癸";
const divi = "子丑寅卯辰巳午未申酉戌亥";

let yearNum = Math.floor(((new Date(date).getTime() - new Date().getTimezoneOffset() * 60000) / 31557600000 - 14)) % 60;
if (yearNum < 0)
  yearNum += 60;

let nmgj = tmgj[yearNum % 10];
let nmvi = divi[yearNum % 12];

function jdToYearSexagenary (jd) {
  const a = Math.floor((jd << 2) / 1461 - 4) % 60;
  return a > 0 ? a : a + 60;
}
```

<!-- ### 月干支 -->
### 日干支
```js
const tmgj = "甲乙丙丁戊己庚辛壬癸";
const divi = "子丑寅卯辰巳午未申酉戌亥";

let dayNum = Math.floor(((new Date(date).getTime() - new Date().getTimezoneOffset() * 60000) / 86400000 + 17)) % 60;
if (dayNum < 0)
  dayNum += 60;

let rigj = tmgj[dayNum % 10];
let rivi = divi[dayNum % 12];

function jdToDaySexagenary (jd) {
  const a = Math.floor(jd - 10.5) % 60;
  return a > 0 ? a : a + 60;
}
```

### 時干支
```js
const tmgj = "甲乙丙丁戊己庚辛壬癸";
const divi = "子丑寅卯辰巳午未申酉戌亥";

let timeNum = Math.floor(((new Date(date).getTime() - new Date().getTimezoneOffset() * 60000) / 7200000 + 24.5)) % 60;
if (timeNum < 0)
  timeNum += 60;

let uigj = tmgj[dayNum % 10];
let uivi = divi[dayNum % 12];

function jdToTimeSexagenary (jd) {
  const a = Math.floor(jd - 3.0416666666666666) % 60
  return a > 0 ? a : a + 60;
}
```

有些流派認為古時沒有法定時區，計算時辰干支時應以[視太陽時](https://zh.wikipedia.org/wiki/%E5%A4%AA%E9%98%B3%E6%97%A5#%E8%A6%96%E5%A4%AA%E9%99%BD%E6%97%A5)為準。有關視太陽時的計算可見[下方章節](#視太陽時)。

# 簡單曆法
## 公曆、儒略曆
```js
/** isLeapGreg (y)
 * Determine whether the given year is a leap year in Gregorian calendar.
 * @param y Gregorian year.
 */
function isLeapGreg (y) {
  return ((y & 3) == 0) && (!(((y % 100) == 0) && ((y % 400) != 0)));
}

/** isLeapJulianOnly (y)
 * Determine whether the given year is a leap year in Julian calendar while not being one in Gregorian.
 * @param y Year.
 */
function isLeapJulianOnly (y) {
  return ((y % 100) == 0) && ((y % 400) != 0);
}
```

### 每月日數
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
  else {
    const a = 501381 * m - 996208;
    return (a >> 14) - (a - 501381 >> 14);
  }
}
```

### 序數日期
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
  else return (501381 * m - 1497589 >> 14) + 59 + d + isLeapGreg(y);
}
```

## ISO 週日曆與漢克亨利萬年曆
[ISO 週日曆](https://zh.wikipedia.org/wiki/ISO%E9%80%B1%E6%97%A5%E6%9B%86)將一年分割成 52 或 53 個星期，ISO 對於星期的定義是從週一開結、週日結束。在年份邊界的時份，以星期四所在的年份作為整個星期所屬的年份。

[漢克亨利萬年曆](https://zh.wikipedia.org/wiki/%E6%BC%A2%E5%85%8B%E4%BA%A8%E5%88%A9%E8%90%AC%E5%B9%B4%E6%9B%86)（Hanke–Henry Permanent Calendar, HHPC）是一種把曆年與 ISO 週日曆對齊的曆法，每年有 364 或 371 天。

```js
/** jdToIsoWeekday (jd)
 * Calculate the ISO 8601 weekday from the given Julian day. 
 * @param jd Julian day.
 * @return An array [y, w, d] representing the year, week and day respectively. 0 = Sunday.
 */
function jdToIsoWeekday (jd) {
  jd = Math.floor(jd + 0.5);
  let y = jdToGreg(jd - 3)[0];
  if (jd >= isoWeekdayToJd(year + 1, 1, 1))
    y++;

  return [
    y,
    Math.floor((jd - isoWeekdayToJd(y, 1, 1)) / 7) + 1,
    wday(jd)
  ];
}

function isoWeekdayToJd (y, w, d) {
  if (y === 0)
    y = 7;

  let j = w * 7;
  const yearStart = gregToJd(y - 1, 12, 28);

  if (w > 0) {
    j += yearStart - 1 - wday(yearStart - 1); // Previous Sunday
  } else {
    j += yearStart + 7 - wday(yearStart + 7); // Next Sunday
  };

  return d + j;
}
```

```js
function jdToHhpc (jd) {
  jd = Math.floor(jd + 0.5);
  let y = jdToGreg(jd - 3)[0];
  if (jd >= isoWeekdayToJd(year + 1, 1, 1))
    y++;

  const date = jd - isoWeekdayToJd(y, 1, 1);
  const m = Math.floor(date / 30) + Math.floor(date / 90);

  return [
    y,
    m + 1,
    date - Math.floor((m - 1) * 91 / 3);
  ];
}

function hhpcToJd (y, m, d) {
  const yearStartLastest = gregToJd(y, 1, 3);
  return yearStartLastest - wday(yearStartLastest) + Math.floor((m - 1) * 91 / 3) + d;
}

function hhpcOldToJd (y, m, d) {
  const yearStartLastest = gregToJd(y, 1, 2);
  return yearStartLastest - wday(yearStartLastest - 6) + Math.floor((m - 1) * 91 / 3) + d;
}
```

## 科普特曆與埃塞俄比亞曆
[科普特曆](https://en.wikipedia.org/wiki/Coptic_calendar)與[埃塞俄比亞曆](https://zh.wikipedia.org/wiki/%E5%9F%83%E5%A1%9E%E4%BF%84%E6%AF%94%E4%BA%9E%E6%9B%86)基本上和儒略曆有線性對應關係，只是紀年法以及年首的位置不同而已。

```js
function jdToCoptic (jd) {
  let wjd = Math.floor(jd - 1824663.5); // Counting from 0283-08-29
  let year = Math.floor(wjd / 365.25);
  year %= 1460; // Substract multiples of 365 / 0.25 = 1460
  let days = Math.floor(wjd - year * 365.25);

  return [
    year,
    Math.floor(days / 30) + 1,
    days % 30 + 1
  ];
}

function jdToEthiopian (jd) {
  let wjd = Math.floor(jd - 1723854.5); // Counting from 0007-08-27
  let year = Math.floor(wjd / 365.25);
  year %= 1460; // Substract multiples of 365 / 0.25 = 1460
  let days = Math.floor(wjd - year * 365.25);

  return [
    year,
    Math.floor(days / 30) + 1,
    days % 30 + 1
  ];
}
```

# 瑪雅曆
```js
/** jdToMayanShort (jd)
 * Convert a Julian day into a date in Mayan “short” calendars (tzolkʼin and haabʼ).
 * Tzolkʼin day names and haabʼ month names are 0 indexed, the other two elements are 1 indexed.
 * @return An array [tzolkin_name, tzolkin_num, haab_month, haab_day].
 */
function jdToMayanShort (jd) {
  let d = Math.floor(jd - 584282.5); // Counting from -3113-08-11
  let day = (d + 348) % 365;
  return [
    d % 20,
    (d + 3) % 13 + 1,
    Math.floor(day / 20),
    day % 20
  ];
}

/** jdToMayanLong (jd)
 * Convert a Julian day into a date in Mayan long count.
 * @return An array [baktun, katun, tun, uinal, kin].
 */
function jdToMayanLong (jd) {
  let d = Math.floor(jd - 584282.5); // Counting from -3113-08-11

  let baktun = Math.floor(d / 144000);
  d -= baktun * 144000;
  let katun = Math.floor(d / 7200);
  d -= katun * 7200;
  let tun = Math.floor(d / 360);
  d -= tun * 360;
  let uinal = Math.floor(d / 20);

  return [baktun, katun, tun, uinal, d - uinal * 20];
}

/** mayanLongToJd (baktun, katun, tun, uinal, kin)
 * Convert a date in Mayan long count into a Julian day.
 */
function mayanLongToJd (baktun, katun, tun, uinal, kin) {
  return 584282.5 +
    baktun * 144000 +
    katun  *   7200 +
    tun    *    360 +
    uinal  *     20 +
    kin;
}
```

# 基本概念
## 恆星時
```js
/** jdToGmst (jd)
 * Convert a Julian day into the corresponding (approximate) Greenwich mean sidereal time (GMST) in degrees.
 * @param jd Julian day.
 * @return Greenwich mean sidereal time (GMST) in degrees.
 */
function jdToGmst (jd) {
  return 241.654320 + 360.9856473840 * (jd - 2440000.5);
}
```

## 平太陽時與視太陽時
平太陽時的計算很簡單，只需要知道觀測點的經度就可以了。

而[視太陽時](https://zh.wikipedia.org/wiki/%E5%A4%AA%E9%98%B3%E6%97%A5#%E8%A6%96%E5%A4%AA%E9%99%BD%E6%97%A5)除了要知道觀測點經度之外，還得知道輸入的時間是一年之中的哪一個日子。

```js
/** equationOfTimeFast (jd)
 * Compute the equation of time for a given moment, but with a faster (but less accurate) algorithm.
 * @param jd Julian day.
 * @return Equation of time in minutes.
 */
function equationOfTimeFast (jd) {
  let d = 6.24004077 + 0.01720197 * (jd - 2451545);
  return 9.863 * Math.sin(d * 2 + 3.5932) - 7.659 * Math.sin(d);
}
```

## 月相
## 分點和至點

# 複雜曆法
## 法國共和曆
[法國共和曆](https://zh.wikipedia.org/wiki/%E6%B3%95%E5%9C%8B%E5%85%B1%E5%92%8C%E6%9B%86)的原始版本因為涉及到計算每年秋分的準確日期，因此也歸類進複雜曆法當中。但除此以外法國共和曆結構上與古埃及曆法基本同出一轍。

```js
function jdToFrench (jd) {
  jd = Math.floor(jd + 0.5);

  let days = jd - equinox;

  return [
    y,
    Math.floor(days / 30) + 1,
    Math.floor((days % 30) / 10) + 1,
    days % 10 + 1
  ];
}
```

## 農曆
精確的農曆編算非常複雜，加上古代實曆的觀測精度沒有現代這麼準，大多數轉換程式用的底層方法還是查表。

```js
function jdToChinese (jd) {

}
```
