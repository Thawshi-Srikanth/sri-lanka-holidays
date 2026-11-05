# sri-lanka-holidays

A lightweight, zero-dependency npm package providing official **Sri Lanka Public, Bank, and Mercantile holidays** (including Full Moon Poya Days) for **2026** and **2027**, backed by clean JSON files for effortless future updates.

## Features

- **2026 & 2027 Official Holiday Calendars** included out of the box (`data/2026.json`, `data/2027.json`).
- **Full Holiday Classification**:
  - `Public` (Public Holidays)
  - `Bank` (Bank Holidays)
  - `Mercantile` (Mercantile / Shop & Office Holidays)
  - `isPoya` flag for Full Moon Poya Days
- **Easy JSON Extensibility**: Add future years (e.g., `data/2028.json`) or load custom JSON at runtime with `addHolidaysForYear()` / `loadHolidaysFromJson()`.
- **Dual Module Support**: Works with both CommonJS (`require`) and ES Modules (`import`), with full TypeScript declarations (`.d.ts`).

---

## Installation

```bash
npm install sri-lanka-holidays
```

---

## Quick Start

### ES Modules (`import`)

```js
import {
  HolidayType,
  getHolidays,
  getHolidaysByType,
  getPublicHolidays,
  getBankHolidays,
  getMercantileHolidays,
  getPoyaDays,
  getHolidayByDate,
  isHoliday,
  isMercantileHoliday
} from 'sri-lanka-holidays';

// 1. Get all holidays for 2026 or 2027
const holidays2026 = getHolidays(2026);
const holidays2027 = getHolidays(2027);

// 2. Filter by Holiday Type ('Public', 'Bank', 'Mercantile')
const mercantile2026 = getMercantileHolidays(2026);
const bank2027 = getBankHolidays(2027);
const public2027 = getPublicHolidays(2027);

// 3. Get only the 8 statutory (non-Poya) Mercantile holidays for 2027
const statutoryMercantile2027 = getHolidays({
  year: 2027,
  type: HolidayType.MERCANTILE,
  isPoya: false
});

// 4. Check a specific date
console.log(getHolidayByDate('2026-04-14'));
/*
{
  date: '2026-04-14',
  day: 'Tuesday',
  name: 'Sinhala & Tamil New Year Day',
  types: ['Public', 'Bank', 'Mercantile'],
  isPublic: true,
  isBank: true,
  isMercantile: true,
  isPoya: false
}
*/

// Good Friday (Public & Bank only, NOT Mercantile)
console.log(isHoliday('2026-04-03', 'Public'));     // true
console.log(isHoliday('2026-04-03', 'Bank'));       // true
console.log(isMercantileHoliday('2026-04-03'));     // false
```

### CommonJS (`require`)

```js
const slHolidays = require('sri-lanka-holidays');

console.log(slHolidays.getSupportedYears()); // [2026, 2027]
console.log(slHolidays.getHolidaysByMonth(2026, 4));
```

---

## Adding Future Years via JSON (e.g., 2028)

### Option 1: Drop a JSON file in the `data/` folder

Create `data/2028.json` following the same JSON structure:

```json
[
  {
    "date": "2028-01-15",
    "day": "Saturday",
    "name": "Tamil Thai Pongal Day",
    "types": ["Public", "Bank", "Mercantile"],
    "isPoya": false
  }
]
```

*(Note: `day` and `isPoya` are automatically computed if omitted in your JSON!)*

In Node.js, any `YYYY.json` file inside `data/` is discovered automatically. For browser bundlers, also export it in `data/index.js`.

### Option 2: Load JSON dynamically at runtime

```js
import { addHolidaysForYear, loadHolidaysFromJson } from 'sri-lanka-holidays';
import holidays2028 from './my-2028-holidays.json' with { type: 'json' };

// Register a single year
addHolidaysForYear(2028, holidays2028);

// Or register multiple years at once
loadHolidaysFromJson({
  2028: holidays2028
});
```

---

## API Reference

| Function | Description |
| :--- | :--- |
| `getHolidays(yearOrOptions?)` | Returns all holidays, holidays for a year (`2026`), or filtered by `{ year, month, type, isPoya }`. |
| `getHolidaysByYear(year, options?)` | Returns holidays for a specific year with optional `{ month, type, isPoya }` filters. |
| `getHolidaysByMonth(year, month, options?)` | Returns holidays for a given year and month (`1`–`12`). |
| `getHolidaysByType(type, year?)` | Filters holidays by `'Public'`, `'Bank'`, or `'Mercantile'` (also accepts `'P'`, `'B'`, `'M'`). |
| `getPublicHolidays(year?)` | Returns all `Public` holidays (optionally for a specific year). |
| `getBankHolidays(year?)` | Returns all `Bank` holidays (optionally for a specific year). |
| `getMercantileHolidays(year?)` | Returns all `Mercantile` holidays (optionally for a specific year). |
| `getPoyaDays(year?)` | Returns all Full Moon Poya Day holidays. |
| `getHolidayByDate(date)` | Returns the holiday object for `'YYYY-MM-DD'` or `Date`, or `null`. |
| `getUpcomingHolidays(fromDate?, options?)` | Returns upcoming holidays on/after `fromDate` with optional `{ limit, type, isPoya }`. |
| `isHoliday(date, type?)` | Returns `true` if `date` is a holiday (optionally of a specific `type`). |
| `isPublicHoliday(date)` | Returns `true` if `date` is a `Public` holiday. |
| `isBankHoliday(date)` | Returns `true` if `date` is a `Bank` holiday. |
| `isMercantileHoliday(date)` | Returns `true` if `date` is a `Mercantile` holiday. |
| `isPoyaDay(date)` | Returns `true` if `date` is a Full Moon Poya Day. |
| `getSupportedYears()` | Returns sorted array of loaded years (e.g., `[2026, 2027]`). |
| `addHolidaysForYear(year, jsonArray)` | Adds or replaces holiday JSON data for a given year. |
| `loadHolidaysFromJson(jsonInput)` | Loads an array of holidays or `{ [year]: holidays[] }` JSON map. |

## License

MIT

