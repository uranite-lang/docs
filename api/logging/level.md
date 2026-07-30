# uranite.logging.level

## Table of Contents

- [enum `LogLevel`](#enum-loglevel)

## enum `LogLevel`

Log severity levels ordered from least to most severe.

Use variant.name to get the string representation (e.g., LogLevel.Info.name returns "Info"). Use variant.value to get the backing I64 value (e.g., LogLevel.Info.value returns 2). Comparisons use backing values: LogLevel.Error >= LogLevel.Info is True.

