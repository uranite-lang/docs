# uranite.datetime.timezone

## Table of Contents

- [Imports](#imports)
- [const `UTC_OFFSET`](#const-utc-offset)
- [const `GMT_OFFSET`](#const-gmt-offset)
- [const `EST_OFFSET`](#const-est-offset)
- [const `EDT_OFFSET`](#const-edt-offset)
- [const `CST_OFFSET`](#const-cst-offset)
- [const `CDT_OFFSET`](#const-cdt-offset)
- [const `MST_OFFSET`](#const-mst-offset)
- [const `MDT_OFFSET`](#const-mdt-offset)
- [const `PST_OFFSET`](#const-pst-offset)
- [const `PDT_OFFSET`](#const-pdt-offset)
- [const `AKST_OFFSET`](#const-akst-offset)
- [const `AKDT_OFFSET`](#const-akdt-offset)
- [const `HST_OFFSET`](#const-hst-offset)
- [const `CET_OFFSET`](#const-cet-offset)
- [const `CEST_OFFSET`](#const-cest-offset)
- [const `EET_OFFSET`](#const-eet-offset)
- [const `EEST_OFFSET`](#const-eest-offset)
- [const `IST_OFFSET`](#const-ist-offset)
- [const `CST_CHINA_OFFSET`](#const-cst-china-offset)
- [const `JST_OFFSET`](#const-jst-offset)
- [const `KST_OFFSET`](#const-kst-offset)
- [const `AEST_OFFSET`](#const-aest-offset)
- [const `AEDT_OFFSET`](#const-aedt-offset)
- [const `NZST_OFFSET`](#const-nzst-offset)
- [const `NZDT_OFFSET`](#const-nzdt-offset)
- [const `WIB_OFFSET`](#const-wib-offset)
- [class `Timezone`](#class-timezone)
  - [`Timezone()`](#Timezone)
  - [`abbreviation()`](#abbreviation)
  - [`offsetHours()`](#offsetHours)
  - [`offsetMinutes()`](#offsetMinutes)
  - [`formatOffset()`](#formatOffset)
  - [`convertFrom()`](#convertFrom)
- [function `utcTimezone`](#function-utctimezone)
  - [`utcTimezone()`](#utcTimezone)
- [function `gmtTimezone`](#function-gmttimezone)
  - [`gmtTimezone()`](#gmtTimezone)
- [function `estTimezone`](#function-esttimezone)
  - [`estTimezone()`](#estTimezone)
- [function `edtTimezone`](#function-edttimezone)
  - [`edtTimezone()`](#edtTimezone)
- [function `cstTimezone`](#function-csttimezone)
  - [`cstTimezone()`](#cstTimezone)
- [function `cdtTimezone`](#function-cdttimezone)
  - [`cdtTimezone()`](#cdtTimezone)
- [function `mstTimezone`](#function-msttimezone)
  - [`mstTimezone()`](#mstTimezone)
- [function `mdtTimezone`](#function-mdttimezone)
  - [`mdtTimezone()`](#mdtTimezone)
- [function `pstTimezone`](#function-psttimezone)
  - [`pstTimezone()`](#pstTimezone)
- [function `pdtTimezone`](#function-pdttimezone)
  - [`pdtTimezone()`](#pdtTimezone)
- [function `cetTimezone`](#function-cettimezone)
  - [`cetTimezone()`](#cetTimezone)
- [function `cestTimezone`](#function-cesttimezone)
  - [`cestTimezone()`](#cestTimezone)
- [function `eetTimezone`](#function-eettimezone)
  - [`eetTimezone()`](#eetTimezone)
- [function `eestTimezone`](#function-eesttimezone)
  - [`eestTimezone()`](#eestTimezone)
- [function `istTimezone`](#function-isttimezone)
  - [`istTimezone()`](#istTimezone)
- [function `jstTimezone`](#function-jsttimezone)
  - [`jstTimezone()`](#jstTimezone)
- [function `kstTimezone`](#function-ksttimezone)
  - [`kstTimezone()`](#kstTimezone)
- [function `aestTimezone`](#function-aesttimezone)
  - [`aestTimezone()`](#aestTimezone)
- [function `nzstTimezone`](#function-nzsttimezone)
  - [`nzstTimezone()`](#nzstTimezone)
- [function `wibTimezone`](#function-wibtimezone)
  - [`wibTimezone()`](#wibTimezone)
- [function `fromOffset`](#function-fromoffset)
  - [`fromOffset()`](#fromOffset)
- [function `fromHoursOffset`](#function-fromhoursoffset)
  - [`fromHoursOffset()`](#fromHoursOffset)
- [function `fromHoursMinutesOffset`](#function-fromhoursminutesoffset)
  - [`fromHoursMinutesOffset()`](#fromHoursMinutesOffset)
- [function `lookupTimezone`](#function-lookuptimezone)
  - [`lookupTimezone()`](#lookupTimezone)

## Imports

- `uranite.convert.convert`
  - `intToString`
- `uranite.datetime.datetime`
  - `DateTime`
  - `fromTimestamp`
- `uranite.datetime.errors`
  - `DateTimeError`

## const `UTC_OFFSET`

UTC offset in seconds (0). 

## const `GMT_OFFSET`

GMT offset in seconds (0). 

## const `EST_OFFSET`

US Eastern Standard Time offset in seconds (-5 hours). 

## const `EDT_OFFSET`

US Eastern Daylight Time offset in seconds (-4 hours). 

## const `CST_OFFSET`

US Central Standard Time offset in seconds (-6 hours). 

## const `CDT_OFFSET`

US Central Daylight Time offset in seconds (-5 hours). 

## const `MST_OFFSET`

US Mountain Standard Time offset in seconds (-7 hours). 

## const `MDT_OFFSET`

US Mountain Daylight Time offset in seconds (-6 hours). 

## const `PST_OFFSET`

US Pacific Standard Time offset in seconds (-8 hours). 

## const `PDT_OFFSET`

US Pacific Daylight Time offset in seconds (-7 hours). 

## const `AKST_OFFSET`

Alaska Standard Time offset in seconds (-9 hours). 

## const `AKDT_OFFSET`

Alaska Daylight Time offset in seconds (-8 hours). 

## const `HST_OFFSET`

Hawaii Standard Time offset in seconds (-10 hours). 

## const `CET_OFFSET`

Central European Time offset in seconds (+1 hour). 

## const `CEST_OFFSET`

Central European Summer Time offset in seconds (+2 hours). 

## const `EET_OFFSET`

Eastern European Time offset in seconds (+2 hours). 

## const `EEST_OFFSET`

Eastern European Summer Time offset in seconds (+3 hours). 

## const `IST_OFFSET`

India Standard Time offset in seconds (+5:30 hours). 

## const `CST_CHINA_OFFSET`

China Standard Time offset in seconds (+8 hours). 

## const `JST_OFFSET`

Japan Standard Time offset in seconds (+9 hours). 

## const `KST_OFFSET`

Korea Standard Time offset in seconds (+9 hours). 

## const `AEST_OFFSET`

Australian Eastern Standard Time offset in seconds (+10 hours). 

## const `AEDT_OFFSET`

Australian Eastern Daylight Time offset in seconds (+11 hours). 

## const `NZST_OFFSET`

New Zealand Standard Time offset in seconds (+12 hours). 

## const `NZDT_OFFSET`

New Zealand Daylight Time offset in seconds (+13 hours). 

## const `WIB_OFFSET`

Western Indonesia Time offset in seconds (+7 hours). 

## class `Timezone`

Represents a timezone as a named abbreviation paired with a fixed UTC offset in seconds. Provides methods for formatting the offset, extracting hour and minute components, and converting DateTimes between timezones.

### Fields

| Name | Type | Access |
|------|------|--------|
| `name` | `String` | public |
| `offset` | `I64` | public |

### Methods

#### `function Timezone( self, String name, I64 offset ) -> Void`

Create a new Timezone with the given abbreviation and UTC offset.

**Parameters**:

- `name` (`String`)
- `The timezone abbreviation.`
- `offset` (`I64`)
- `The offset from UTC in seconds.`

#### `function abbreviation( self ) -> String`

Return the timezone abbreviation.

**Returns**: — The timezone name string (e.g. "UTC", "PST").

#### `function offsetHours( self ) -> I64`

Return the whole hours component of the UTC offset.

**Returns**: — The offset divided by 3600, truncated toward zero.

#### `function offsetMinutes( self ) -> I64`

Return the remaining minutes component of the UTC offset after removing the whole hours. Always returns a non-negative value.

**Returns**: — The absolute value of the minutes remainder (0-59).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function formatOffset( self ) -> String`

Format the UTC offset as a string in the form "+HH:MM" or "-HH:MM". Returns "+00:00" for zero offsets.

**Returns**: — A formatted offset string such as "+05:30" or "-08:00".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function convertFrom( self, DateTime dt ) -> DateTime`

Convert a DateTime from its current timezone to this timezone by extracting the UTC timestamp and reinterpreting it with this timezone's offset.

**Parameters**:

- `dt` (`DateTime`)
- `The source DateTime to convert.`

**Returns**: — A new DateTime adjusted to this timezone's offset.

## function `utcTimezone`

Create a Timezone representing Coordinated Universal Time (UTC+00:00).

**Returns**: — A Timezone with abbreviation "UTC" and zero offset.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function utcTimezone(  ) -> Timezone`

Create a Timezone representing Coordinated Universal Time (UTC+00:00).

**Returns**: — A Timezone with abbreviation "UTC" and zero offset.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `gmtTimezone`

Create a Timezone representing Greenwich Mean Time (GMT+00:00).

**Returns**: — A Timezone with abbreviation "GMT" and zero offset.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function gmtTimezone(  ) -> Timezone`

Create a Timezone representing Greenwich Mean Time (GMT+00:00).

**Returns**: — A Timezone with abbreviation "GMT" and zero offset.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `estTimezone`

Create a Timezone representing US Eastern Standard Time (UTC-05:00).

**Returns**: — A Timezone with abbreviation "EST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function estTimezone(  ) -> Timezone`

Create a Timezone representing US Eastern Standard Time (UTC-05:00).

**Returns**: — A Timezone with abbreviation "EST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `edtTimezone`

Create a Timezone representing US Eastern Daylight Time (UTC-04:00).

**Returns**: — A Timezone with abbreviation "EDT".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function edtTimezone(  ) -> Timezone`

Create a Timezone representing US Eastern Daylight Time (UTC-04:00).

**Returns**: — A Timezone with abbreviation "EDT".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `cstTimezone`

Create a Timezone representing US Central Standard Time (UTC-06:00).

**Returns**: — A Timezone with abbreviation "CST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function cstTimezone(  ) -> Timezone`

Create a Timezone representing US Central Standard Time (UTC-06:00).

**Returns**: — A Timezone with abbreviation "CST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `cdtTimezone`

Create a Timezone representing US Central Daylight Time (UTC-05:00).

**Returns**: — A Timezone with abbreviation "CDT".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function cdtTimezone(  ) -> Timezone`

Create a Timezone representing US Central Daylight Time (UTC-05:00).

**Returns**: — A Timezone with abbreviation "CDT".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `mstTimezone`

Create a Timezone representing US Mountain Standard Time (UTC-07:00).

**Returns**: — A Timezone with abbreviation "MST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function mstTimezone(  ) -> Timezone`

Create a Timezone representing US Mountain Standard Time (UTC-07:00).

**Returns**: — A Timezone with abbreviation "MST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `mdtTimezone`

Create a Timezone representing US Mountain Daylight Time (UTC-06:00).

**Returns**: — A Timezone with abbreviation "MDT".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function mdtTimezone(  ) -> Timezone`

Create a Timezone representing US Mountain Daylight Time (UTC-06:00).

**Returns**: — A Timezone with abbreviation "MDT".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `pstTimezone`

Create a Timezone representing US Pacific Standard Time (UTC-08:00).

**Returns**: — A Timezone with abbreviation "PST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function pstTimezone(  ) -> Timezone`

Create a Timezone representing US Pacific Standard Time (UTC-08:00).

**Returns**: — A Timezone with abbreviation "PST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `pdtTimezone`

Create a Timezone representing US Pacific Daylight Time (UTC-07:00).

**Returns**: — A Timezone with abbreviation "PDT".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function pdtTimezone(  ) -> Timezone`

Create a Timezone representing US Pacific Daylight Time (UTC-07:00).

**Returns**: — A Timezone with abbreviation "PDT".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `cetTimezone`

Create a Timezone representing Central European Time (UTC+01:00).

**Returns**: — A Timezone with abbreviation "CET".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function cetTimezone(  ) -> Timezone`

Create a Timezone representing Central European Time (UTC+01:00).

**Returns**: — A Timezone with abbreviation "CET".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `cestTimezone`

Create a Timezone representing Central European Summer Time (UTC+02:00).

**Returns**: — A Timezone with abbreviation "CEST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function cestTimezone(  ) -> Timezone`

Create a Timezone representing Central European Summer Time (UTC+02:00).

**Returns**: — A Timezone with abbreviation "CEST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `eetTimezone`

Create a Timezone representing Eastern European Time (UTC+02:00).

**Returns**: — A Timezone with abbreviation "EET".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function eetTimezone(  ) -> Timezone`

Create a Timezone representing Eastern European Time (UTC+02:00).

**Returns**: — A Timezone with abbreviation "EET".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `eestTimezone`

Create a Timezone representing Eastern European Summer Time (UTC+03:00).

**Returns**: — A Timezone with abbreviation "EEST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function eestTimezone(  ) -> Timezone`

Create a Timezone representing Eastern European Summer Time (UTC+03:00).

**Returns**: — A Timezone with abbreviation "EEST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `istTimezone`

Create a Timezone representing India Standard Time (UTC+05:30).

**Returns**: — A Timezone with abbreviation "IST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function istTimezone(  ) -> Timezone`

Create a Timezone representing India Standard Time (UTC+05:30).

**Returns**: — A Timezone with abbreviation "IST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `jstTimezone`

Create a Timezone representing Japan Standard Time (UTC+09:00).

**Returns**: — A Timezone with abbreviation "JST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function jstTimezone(  ) -> Timezone`

Create a Timezone representing Japan Standard Time (UTC+09:00).

**Returns**: — A Timezone with abbreviation "JST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `kstTimezone`

Create a Timezone representing Korea Standard Time (UTC+09:00).

**Returns**: — A Timezone with abbreviation "KST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function kstTimezone(  ) -> Timezone`

Create a Timezone representing Korea Standard Time (UTC+09:00).

**Returns**: — A Timezone with abbreviation "KST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `aestTimezone`

Create a Timezone representing Australian Eastern Standard Time (UTC+10:00).

**Returns**: — A Timezone with abbreviation "AEST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function aestTimezone(  ) -> Timezone`

Create a Timezone representing Australian Eastern Standard Time (UTC+10:00).

**Returns**: — A Timezone with abbreviation "AEST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `nzstTimezone`

Create a Timezone representing New Zealand Standard Time (UTC+12:00).

**Returns**: — A Timezone with abbreviation "NZST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function nzstTimezone(  ) -> Timezone`

Create a Timezone representing New Zealand Standard Time (UTC+12:00).

**Returns**: — A Timezone with abbreviation "NZST".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `wibTimezone`

Create a Timezone representing Western Indonesia Time (UTC+07:00).

**Returns**: — A Timezone with abbreviation "WIB".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function wibTimezone(  ) -> Timezone`

Create a Timezone representing Western Indonesia Time (UTC+07:00).

**Returns**: — A Timezone with abbreviation "WIB".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fromOffset`

Create a custom Timezone from an abbreviation and an offset specified in seconds.

**Parameters**:

- `name` (`String`)
- `The timezone abbreviation.`
- `offsetSeconds` (`I64`)
- `The offset from UTC in seconds.`

**Returns**: — A new Timezone with the given name and offset.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fromOffset( String name, I64 offsetSeconds ) -> Timezone`

Create a custom Timezone from an abbreviation and an offset specified in seconds.

**Parameters**:

- `name` (`String`)
- `The timezone abbreviation.`
- `offsetSeconds` (`I64`)
- `The offset from UTC in seconds.`

**Returns**: — A new Timezone with the given name and offset.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fromHoursOffset`

Create a custom Timezone from an abbreviation and an offset specified in whole hours.

**Parameters**:

- `name` (`String`)
- `The timezone abbreviation.`
- `hours` (`I64`)
- `The offset from UTC in hours.`

**Returns**: — A new Timezone with the given name and the hours converted
to seconds.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fromHoursOffset( String name, I64 hours ) -> Timezone`

Create a custom Timezone from an abbreviation and an offset specified in whole hours.

**Parameters**:

- `name` (`String`)
- `The timezone abbreviation.`
- `hours` (`I64`)
- `The offset from UTC in hours.`

**Returns**: — A new Timezone with the given name and the hours converted
to seconds.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fromHoursMinutesOffset`

Create a custom Timezone from an abbreviation and an offset specified in hours and minutes. For negative offsets, the minutes are subtracted from the hour component.

**Parameters**:

- `name` (`String`)
- `The timezone abbreviation.`
- `hours` (`I64`)
- `The hours component of the offset` (`negative for west of UTC`)
- `minutes` (`I64`)
- `The minutes component of the offset` (`always non-negative`)

**Returns**: — A new Timezone with the computed offset in seconds.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fromHoursMinutesOffset( String name, I64 hours, I64 minutes ) -> Timezone`

Create a custom Timezone from an abbreviation and an offset specified in hours and minutes. For negative offsets, the minutes are subtracted from the hour component.

**Parameters**:

- `name` (`String`)
- `The timezone abbreviation.`
- `hours` (`I64`)
- `The hours component of the offset` (`negative for west of UTC`)
- `minutes` (`I64`)
- `The minutes component of the offset` (`always non-negative`)

**Returns**: — A new Timezone with the computed offset in seconds.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `lookupTimezone`

Look up a timezone by its standard abbreviation. Supports common abbreviations including UTC, GMT, EST, EDT, CST, CDT, MST, MDT, PST, PDT, CET, CEST, EET, EEST, IST, JST, KST, AEST, NZST, and WIB.

**Parameters**:

- `name` (`String`)
- `The timezone abbreviation to look up.`

**Returns**: — A Timezone matching the given abbreviation.

**Raises**:

- `DateTimeError` → `Error` — If the abbreviation is not recognized.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function lookupTimezone( String name ) -> Timezone`

Look up a timezone by its standard abbreviation. Supports common abbreviations including UTC, GMT, EST, EDT, CST, CDT, MST, MDT, PST, PDT, CET, CEST, EET, EEST, IST, JST, KST, AEST, NZST, and WIB.

**Parameters**:

- `name` (`String`)
- `The timezone abbreviation to look up.`

**Returns**: — A Timezone matching the given abbreviation.

**Raises**:

- `DateTimeError` → `Error` — If the abbreviation is not recognized.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

