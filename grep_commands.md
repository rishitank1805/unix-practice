# Grep Commands for Data Analysis

Here's a collection of grep commands that are actually useful when you're working with data files, logs, or sensor readings. These are the ones I use regularly.

## Basic Data Filtering

### Find Specific Sensor Data
```bash
grep "SENSOR_01" sensor_data.csv
```
Gets all readings from SENSOR_01. Simple but effective.

### Case Insensitive Search
```bash
grep -i "active" sensor_data.csv
```
Finds all lines with "active" or "ACTIVE" or "Active". Handy when your data isn't consistent.

### Show Line Numbers
```bash
grep -n "SENSOR_02" sensor_data.csv
```
Shows which line number each match is on. Useful when you need to reference specific entries.

### Count Matches
```bash
grep -c "ACTIVE" sensor_data.csv
```
Just tells you how many times "ACTIVE" appears. Quick way to see how many active sensors you have.

## Finding Specific Values

### Find High Temperature Readings
```bash
grep "2[5-9]\.[0-9]" sensor_data.csv
```
Finds temperatures in the 25-29 range. The regex `2[5-9]` means "2 followed by 5, 6, 7, 8, or 9".

### Find Specific Time Range
```bash
grep "09:1[0-9]" sensor_data.csv
```
Gets all readings from 09:10 to 09:19. Adjust the pattern for different time ranges.

### Find Exact Timestamp
```bash
grep "09:15" sensor_data.csv
```
Gets all sensor readings at exactly 09:15.

### Find High Humidity
```bash
grep -E "[6-9][0-9]\.[0-9]" sensor_data.csv | grep "Humidity"
```
Finds humidity values above 60%. The `-E` flag enables extended regex.

## Working with Status Fields

### Find Active Sensors Only
```bash
grep "ACTIVE" sensor_data.csv
```
Simple filter for active sensors.

### Find Inactive Sensors
```bash
grep "INACTIVE" sensor_data.csv
```
Shows all inactive sensor readings.

### Exclude Inactive Sensors
```bash
grep -v "INACTIVE" sensor_data.csv
```
Shows everything except inactive sensors. The `-v` flag inverts the match.

## Multiple Conditions

### Find Specific Sensor at Specific Time
```bash
grep "SENSOR_01" sensor_data.csv | grep "09:20"
```
Pipes two grep commands together to find SENSOR_01 readings at 09:20.

### Find Sensor with Multiple Conditions
```bash
grep "SENSOR_03" sensor_data.csv | grep "ACTIVE" | grep "2[6-9]\."
```
Finds SENSOR_03 that's active and has temperature between 26-29 degrees.

### Find Lines Matching Any Pattern
```bash
grep -E "SENSOR_01|SENSOR_02" sensor_data.csv
```
Finds readings from either SENSOR_01 or SENSOR_02. The `|` means "or".

## Context and Surrounding Data

### Show Context Around Matches
```bash
grep -A 2 -B 2 "INACTIVE" sensor_data.csv
```
Shows 2 lines before and after each INACTIVE match. Helps see what happened around that time.

### Show Lines After Match
```bash
grep -A 5 "09:00" sensor_data.csv
```
Shows 5 lines after each 09:00 timestamp. Useful for seeing the first few readings.

## Advanced Pattern Matching

### Find Pressure Values Above Threshold
```bash
grep -E "10[2-9][0-9]\.[0-9]" sensor_data.csv
```
Finds pressure values above 1020. Adjust the pattern for different thresholds.

### Find Specific Decimal Ranges
```bash
grep -E "2[2-4]\.[0-9]" sensor_data.csv | grep "Temperature"
```
Finds temperatures between 22.0 and 24.9 degrees.

### Match Whole Words Only
```bash
grep -w "ACTIVE" sensor_data.csv
```
Only matches "ACTIVE" as a complete word, not if it's part of "INACTIVE".

## File Operations

### Search Multiple Files
```bash
grep "SENSOR_01" *.csv
```
Searches for SENSOR_01 across all CSV files in the current directory.

### Recursive Search
```bash
grep -r "ACTIVE" /path/to/sensor/logs
```
Searches recursively through all files in a directory.

### Show Only Filenames
```bash
grep -l "SENSOR_05" *.csv
```
Just shows which files contain SENSOR_05, not the actual matches.

## Data Extraction

### Extract Only Matching Part
```bash
grep -o "SENSOR_[0-9][0-9]" sensor_data.csv
```
Only shows the sensor ID part, not the whole line. Good for extracting specific values.

### Extract Temperature Values
```bash
grep -o "[0-9][0-9]\.[0-9]" sensor_data.csv | head -20
```
Extracts temperature-like values. Not perfect but works for basic extraction.

## Combining with Other Tools

### Count Unique Sensors
```bash
grep -o "SENSOR_[0-9][0-9]" sensor_data.csv | sort | uniq
```
Extracts sensor IDs, sorts them, and shows unique values.

### Find and Sort
```bash
grep "ACTIVE" sensor_data.csv | sort
```
Finds active sensors and sorts the results.

### Find and Count Occurrences
```bash
grep -o "SENSOR_[0-9][0-9]" sensor_data.csv | sort | uniq -c
```
Shows how many readings each sensor has.

### Find High Values and Save
```bash
grep -E "2[7-9]\.[0-9]" sensor_data.csv > high_temp_readings.txt
```
Finds high temperatures and saves them to a file.

## Practical Examples for Sensor Data

### Find All Readings at 9 AM
```bash
grep "^09:" sensor_data.csv
```
The `^` means start of line, so this finds all lines starting with "09:".

### Find Last 10 Minutes of Data
```bash
grep -E "09:[2-9][0-9]" sensor_data.csv
```
Finds readings from 09:20 to 09:29.

### Find Sensors with Issues
```bash
grep -E "INACTIVE|ERROR|FAIL" sensor_data.csv
```
Finds any line with INACTIVE, ERROR, or FAIL status.

### Find Temperature Anomalies
```bash
grep -E "3[0-9]\.[0-9]|1[0-5]\.[0-9]" sensor_data.csv
```
Finds temperatures above 30 or below 15 (assuming normal range is 15-30).

### Find Specific Sensor at Specific Conditions
```bash
grep "SENSOR_03" sensor_data.csv | grep "ACTIVE" | grep "2[7-9]\."
```
Finds SENSOR_03 that's active with temperature 27-29 degrees.

### Count Readings Per Sensor
```bash
for sensor in SENSOR_01 SENSOR_02 SENSOR_03 SENSOR_04 SENSOR_05; do
  echo "$sensor: $(grep -c "$sensor" sensor_data.csv)"
done
```
Loops through sensors and counts readings for each.

## Tips for Working with CSV Data

1. **Header row**: Remember that `grep` will also match the header. Use `grep -v "^Timestamp"` to exclude it if needed.

2. **Comma-separated values**: If you need to match a specific column, you might need to combine grep with `awk` or `cut` for better results.

3. **Multiple patterns**: Use `-E` for extended regex when you need `|` (OR) or `+` (one or more).

4. **Case sensitivity**: Use `-i` when you're not sure about capitalization in your data.

5. **Context**: Use `-A` and `-B` flags to see surrounding data when debugging sensor issues.

## Common Patterns for Sensor Data

- `grep "SENSOR_0[1-5]"` - Find sensors 01 through 05
- `grep -E "[0-9]{2}:[0-9]{2}"` - Match timestamp format
- `grep "[0-9]\.[0-9]"` - Match decimal numbers
- `grep "^[0-9]"` - Lines starting with a digit
- `grep "[0-9]$"` - Lines ending with a digit

These commands should cover most of what you need when working with sensor data or any structured data files. Start simple, then combine patterns as you get more comfortable.

