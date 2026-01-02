# Grep Commands - From Basics to Advanced

So you want to learn grep? Good call. It's one of those tools that seems simple but can do way more than you'd think. Let me walk you through it from the basics to the stuff that'll make you look like a terminal wizard.

## The Basics

### Just Finding Stuff
```bash
grep "search term" file.txt
```
This is the simplest thing. It just looks for "search term" in file.txt and shows you every line that has it.

### Case Insensitive Search
```bash
grep -i "error" logfile.txt
```
The `-i` flag makes it ignore case. So "Error", "ERROR", and "error" all match. Super useful when you're not sure how something was capitalized.

### Show Line Numbers
```bash
grep -n "pattern" file.txt
```
The `-n` flag shows you the line number where it found the match. Really helpful when you need to go back and edit something.

### Invert the Match
```bash
grep -v "pattern" file.txt
```
The `-v` flag shows you everything EXCEPT lines matching the pattern. Want to see all lines that don't have "debug" in them? This is your friend.

### Count Matches
```bash
grep -c "pattern" file.txt
```
Instead of showing all the matching lines, `-c` just tells you how many times it found the pattern. Quick way to see if something appears a lot or not at all.

## Working with Multiple Files

### Search in Multiple Files
```bash
grep "pattern" file1.txt file2.txt file3.txt
```
Just list multiple files. Grep will search through all of them and tell you which file each match came from.

### Recursive Search
```bash
grep -r "pattern" /path/to/directory
```
The `-r` flag (or `-R`) searches through all files in a directory and all subdirectories. Super handy when you're looking for something but don't know where it might be.

### Show Only Filenames
```bash
grep -l "pattern" *.txt
```
The `-l` flag only shows you which files contain the pattern, not the actual matching lines. Useful when you just need to know "does this file have it or not?"

## Regular Expressions (The Fun Part)

### Basic Patterns
```bash
grep "^start" file.txt        # Lines starting with "start"
grep "end$" file.txt          # Lines ending with "end"
grep "^$" file.txt            # Empty lines
```

The `^` means "start of line" and `$` means "end of line". So `^start` finds lines that begin with "start", and `end$` finds lines that end with "end".

### Match Any Character
```bash
grep "a.b" file.txt
```
The `.` matches any single character. So "a.b" matches "aab", "a1b", "a b", etc.

### Character Classes
```bash
grep "[0-9]" file.txt         # Any digit
grep "[a-z]" file.txt         # Any lowercase letter
grep "[A-Z]" file.txt         # Any uppercase letter
grep "[0-9a-f]" file.txt      # Hex digits
```

Square brackets let you match a set of characters. `[0-9]` means "any digit from 0 to 9".

### Match Zero or More
```bash
grep "ab*c" file.txt
```
The `*` means "zero or more of the previous thing". So "ab*c" matches "ac", "abc", "abbc", "abbbc", etc.

### Match One or More
```bash
grep "ab+c" file.txt
```
The `+` means "one or more". So "ab+c" matches "abc", "abbc", "abbbc", but NOT "ac". You need at least one 'b'.

### Match Zero or One
```bash
grep "ab?c" file.txt
```
The `?` means "zero or one". So "ab?c" matches "ac" or "abc", but not "abbc".

### Escaping Special Characters
```bash
grep "file\.txt" file.txt
```
If you want to search for a literal period (or other special character), escape it with a backslash. So `\.` means "an actual period" not "any character".

## More Useful Flags

### Show Context Around Matches
```bash
grep -A 3 "pattern" file.txt  # 3 lines after
grep -B 3 "pattern" file.txt  # 3 lines before
grep -C 3 "pattern" file.txt  # 3 lines before and after
```

Sometimes you need to see what's around the match. `-A` shows lines after, `-B` shows lines before, and `-C` shows both. Really useful when debugging.

### Extended Regular Expressions
```bash
grep -E "pattern1|pattern2" file.txt
```
The `-E` flag lets you use extended regex. The `|` means "or", so this finds lines matching either pattern1 OR pattern2.

### Match Whole Words Only
```bash
grep -w "word" file.txt
```
The `-w` flag only matches whole words. So searching for "word" won't match "password" or "sword". It has to be a complete word.

### Color the Matches
```bash
grep --color=auto "pattern" file.txt
```
Makes the matching text stand out. Most modern terminals do this automatically, but you can force it with this flag.

## Advanced Stuff

### Combining Multiple Patterns
```bash
grep -E "error|warning|critical" logfile.txt
```
Find lines that have any of these words. The `-E` is needed for the `|` operator.

### Excluding Multiple Patterns
```bash
grep -v -E "debug|test|temp" file.txt
```
Show lines that don't have debug, test, or temp. Combine `-v` with `-E` to exclude multiple things.

### Search for Lines with Multiple Patterns
```bash
grep "pattern1" file.txt | grep "pattern2"
```
Pipe grep into grep to find lines that match both patterns. This is a common trick.

### Find Lines That Don't Match Either Pattern
```bash
grep -v "pattern1" file.txt | grep -v "pattern2"
```
Chain `-v` flags to exclude multiple patterns.

### Search Binary Files as Text
```bash
grep -a "pattern" binaryfile
```
The `-a` flag treats binary files as text. Sometimes you need to search through files that aren't plain text.

### Suppress Error Messages
```bash
grep -s "pattern" file.txt
```
The `-s` flag suppresses error messages about files that don't exist or can't be read. Useful in scripts.

### Only Show the Matching Part
```bash
grep -o "pattern" file.txt
```
The `-o` flag only shows the part that matched, not the whole line. Handy when you're extracting specific values.

## Real-World Examples

### Find All TODO Comments
```bash
grep -rn "TODO" .
```
Recursive search with line numbers for all TODO comments in your project.

### Find All IP Addresses
```bash
grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" file.txt
```
This regex finds IP addresses. The `{1,3}` means "between 1 and 3 times".

### Find All Email Addresses
```bash
grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file.txt
```
A basic email regex. Not perfect but catches most common formats.

### Find All Function Definitions
```bash
grep -E "^[a-zA-Z_][a-zA-Z0-9_]*\s*\([^)]*\)\s*{" file.txt
```
This looks for function definitions. Adjust based on your language's syntax.

### Count Errors in Log Files
```bash
grep -c "ERROR" *.log
```
Quick way to see how many errors are in each log file.

### Find Files Containing a Pattern
```bash
grep -rl "database_password" /path/to/config
```
Recursive search that only shows filenames. Great for finding which config files have a specific setting.

### Show Context for Errors
```bash
grep -A 5 -B 5 "ERROR" logfile.txt
```
See 5 lines before and after each error. Helps understand what led to the error.

### Find Lines with Numbers
```bash
grep "[0-9]" file.txt
```
Simple way to find any line that contains a number.

### Find Empty Lines
```bash
grep "^$" file.txt
```
Finds completely empty lines (useful for cleaning up files).

### Find Non-Empty Lines
```bash
grep -v "^$" file.txt
```
Shows all lines except empty ones.

## Pro Tips

1. **Combine with other commands**: `cat file.txt | grep "pattern" | sort | uniq` - chain commands together for powerful filtering.

2. **Use with find**: `find . -name "*.py" -exec grep -l "import os" {} \;` - find Python files that import os.

3. **Save results**: `grep "pattern" file.txt > results.txt` - save your matches to a file.

4. **Search in command output**: `ps aux | grep "python"` - find running Python processes.

5. **Multiple patterns with files**: `grep -E "pattern1|pattern2" file1.txt file2.txt` - search for multiple patterns across multiple files.

6. **Exclude directories**: `grep -r --exclude-dir=node_modules "pattern" .` - search recursively but skip certain directories.

7. **Include only certain file types**: `grep -r --include="*.py" "pattern" .` - only search in Python files.

## Common Gotchas

- **Quotes matter**: `grep pattern file.txt` vs `grep "pattern" file.txt` - use quotes if your pattern has spaces or special characters.

- **Regex vs literal**: By default, some characters are special. Use `-F` for literal matching if you don't want regex.

- **File vs stdin**: `grep pattern file.txt` searches a file, but you can also pipe: `echo "text" | grep pattern`.

- **Case sensitivity**: Remember `-i` for case-insensitive searches. It's easy to forget and wonder why nothing matches.

That's grep in a nutshell. Start with the basics, get comfortable, then move to the advanced stuff. Once you get the hang of it, you'll find yourself using it constantly. It's one of those tools that just becomes part of your workflow.

