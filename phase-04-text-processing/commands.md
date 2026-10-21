# Phase 04 — Linux Text Processing & Log Analysis

## Linux for DevOps — Complete Commands Reference

**Operating System:** CentOS Stream 9  
**Level:** Beginner to Intermediate  
**Project:** Spring Boot Application Log Analysis

## 1. Objective

This document covers Linux text-processing commands used for:

- Searching application logs
- Identifying ERROR and WARN messages
- Investigating HTTP 500 and 503 errors
- Extracting API endpoints and HTTP status codes
- Calculating response times
- Filtering and transforming log data
- Generating incident investigation reports
- Automating basic log analysis

---

## 2. Lab Environment

Create the project directory:

```bash
mkdir -p ~/linux-for-devops/phase-04/{logs,output,backup,scripts}
```

Navigate into the project:

```bash
cd ~/linux-for-devops/phase-04
```

Verify:

```bash
pwd
ls -lah
```

All examples below assume commands are executed from this directory.

### Sample Application Logs

Create the file:

```bash
cat > logs/application.log <<'EOF'
2026-10-21 10:00:01 INFO GET /api/users 200 45ms
2026-10-21 10:00:02 INFO GET /api/products 200 72ms
2026-10-21 10:00:03 ERROR GET /api/orders 500 340ms
2026-10-21 10:00:04 WARN GET /api/users/99 404 55ms
2026-10-21 10:00:05 ERROR POST /api/orders 500 620ms
2026-10-21 10:00:06 INFO GET /api/users 200 35ms
2026-10-21 10:00:07 WARN GET /api/products/99 404 60ms
2026-10-21 10:00:08 ERROR GET /api/orders 500 780ms
2026-10-21 10:00:09 INFO POST /api/users 201 120ms
2026-10-21 10:00:10 INFO GET /api/products 200 48ms
2026-10-21 10:00:11 ERROR POST /api/orders 500 910ms
2026-10-21 10:00:12 WARN GET /api/orders/99 404 52ms
2026-10-21 10:00:13 INFO GET /api/users 200 42ms
2026-10-21 10:00:14 ERROR GET /api/payments 503 1100ms
2026-10-21 10:00:15 INFO GET /api/products 200 67ms
2026-10-21 10:00:16 WARN GET /api/users/100 404 49ms
2026-10-21 10:00:17 ERROR POST /api/payments 500 1500ms
2026-10-21 10:00:18 INFO GET /api/users 200 39ms
2026-10-21 10:00:19 ERROR GET /api/orders 500 870ms
2026-10-21 10:00:20 INFO GET /api/products 200 55ms
EOF
```

### Log Format

```text
DATE TIME LEVEL METHOD ENDPOINT STATUS LATENCY
```

Example:

```text
2026-10-21 10:00:03 ERROR GET /api/orders 500 340ms
```

| Field | Value | Meaning |
|---|---|---|
| $1 | 2026-10-21 | Date |
| $2 | 10:00:03 | Time |
| $3 | ERROR | Log level |
| $4 | GET | HTTP method |
| $5 | /api/orders | API endpoint |
| $6 | 500 | HTTP status |
| $7 | 340ms | Response time |

These field positions apply to our simulated log format. Real Spring Boot logs may use a different structure.

---

# 3. cat — Concatenate and Display Files

**Full form:** Concatenate

### Purpose

Displays file contents and can combine multiple files.

### Syntax

```bash
cat filename
```

### Example 1 — Display application logs

```bash
cat logs/application.log
```

Displays the complete file.

### Example 2 — Display multiple files

```bash
cat logs/application.log output/errors.log
```

Displays the contents of both files in sequence, provided both exist.

### Example 3 — Display line numbers

```bash
cat -n logs/application.log
```

`-n` numbers all output lines.

### Example 4 — Create a file

```bash
cat > output/test.txt
```

Type some text and press `Ctrl+D` to finish.

### DevOps Use Case

Quickly inspect small configuration files, deployment scripts, or application logs.

For large logs, `less`, `tail`, and targeted filtering are generally more practical.

---

# 4. head — Display Beginning of File

### Purpose

Displays the first lines of a file.

### Syntax

```bash
head [options] filename
```

### Example 1

```bash
head logs/application.log
```

Displays the first 10 lines by default.

### Example 2

```bash
head -n 5 logs/application.log
```

Displays the first five lines.

### Example 3

```bash
head -c 50 logs/application.log
```

Displays the first 50 bytes.

### Options

| Option | Explanation |
|---|---|
| -n | Number of lines |
| -c | Number of bytes |
| -q | Suppress filename headers when reading multiple files |
| -v | Always display filename headers |

### DevOps Use Case

Inspect the beginning of a generated report or confirm a file's structure.

---

# 5. tail — Display End of File

### Purpose

Displays the last lines of a file.

### Example 1

```bash
tail logs/application.log
```

Displays the last 10 lines.

### Example 2

```bash
tail -n 5 logs/application.log
```

Displays the last five lines.

### Example 3 — Follow logs

```bash
tail -f logs/application.log
```

`-f` follows newly appended content.

Stop using:

```text
Ctrl+C
```

### Example 4 — Follow log rotation

```bash
tail -F logs/application.log
```

`-F` follows the filename and retries if the file is replaced or temporarily unavailable.

### Options

| Option | Meaning |
|---|---|
| -n | Display last N lines |
| -f | Follow appended content |
| -F | Follow by filename and retry |
| -c | Display last N bytes |

### DevOps Use Case

Monitor application logs during deployments and restarts.

---

# 6. less — View Large Files

### Purpose

Opens a file in a scrollable terminal viewer.

```bash
less logs/application.log
```

### Keyboard Shortcuts

| Key | Action |
|---|---|
| Space | Next page |
| b | Previous page |
| g | Beginning of file |
| G | End of file |
| /ERROR | Search forward |
| n | Next match |
| N | Previous match |
| q | Quit |

### Example

```bash
less +G logs/application.log
```

Opens the file near the end.

### DevOps Use Case

Inspect large logs without printing the entire file to the terminal.

---

# 7. grep — Search Text

**Name origin:** Historically derived from a command meaning global regular-expression print.

### Purpose

Searches text for matching patterns.

### Syntax

```bash
grep [options] "pattern" filename
```

### Example 1 — Find ERROR

```bash
grep "ERROR" logs/application.log
```

Expected: 7 matching lines.

### Example 2 — Find WARN

```bash
grep "WARN" logs/application.log
```

Expected: 4 matching lines.

### Example 3 — Ignore case

```bash
grep -i "error" logs/application.log
```

`-i` ignores case differences.

### Example 4 — Display line numbers

```bash
grep -n "ERROR" logs/application.log
```

`-n` shows line numbers.

### Example 5 — Count matching lines

```bash
grep -c "ERROR" logs/application.log
```

Expected:

```text
7
```

### Example 6 — Exclude INFO

```bash
grep -v "INFO" logs/application.log
```

`-v` inverts the match.

### Example 7 — Search recursively

```bash
grep -rn "ERROR" logs/
```

- `-r` searches recursively.
- `-n` displays line numbers.

### Example 8 — Show matching filenames

```bash
grep -l "ERROR" logs/*.log
```

`-l` displays filenames containing at least one match.

### Example 9 — Exact fixed string

```bash
grep -F "/api/orders" logs/application.log
```

`-F` treats the search pattern as a fixed string rather than a regular expression.

### Example 10 — Whole-word matching

```bash
grep -w "ERROR" logs/application.log
```

`-w` matches whole words.

### Example 11 — Before and after context

```bash
grep -B 2 -A 2 "503" logs/application.log
```

- `-B 2`: Two lines before the match.
- `-A 2`: Two lines after the match.

### Options Summary

| Option | Purpose |
|---|---|
| -i | Ignore case |
| -n | Show line numbers |
| -c | Count matching lines |
| -v | Invert match |
| -r | Recursive search |
| -l | Show matching filenames |
| -w | Match whole words |
| -F | Fixed-string matching |
| -A N | N lines after |
| -B N | N lines before |
| -C N | N lines before and after |

### DevOps Use Case

Search logs for exceptions, error codes, failed deployments, and application messages.

---

# 8. grep -E — Extended Regular Expressions

### Purpose

Searches using extended regular expressions.

### Example 1 — ERROR or WARN

```bash
grep -E "ERROR|WARN" logs/application.log
```

The `|` inside the pattern means OR.

### Example 2 — HTTP 500 or 503

```bash
grep -E " (500|503) " logs/application.log
```

### Example 3 — Find all 5xx status codes

```bash
grep -E " 5[0-9]{2} " logs/application.log
```

Explanation:

- `5`: Status starts with 5.
- `[0-9]`: Any digit.
- `{2}`: Exactly two occurrences.

### Example 4 — Beginning of line

```bash
grep -E "^2026-10-21" logs/application.log
```

`^` anchors the pattern to the beginning.

### Example 5 — End of line

```bash
grep -E "[0-9]+ms$" logs/application.log
```

`$` anchors the pattern to the end.

### Regex Reference

| Pattern | Meaning |
|---|---|
| ^ | Beginning of line |
| $ | End of line |
| . | Any single character |
| * | Zero or more repetitions |
| + | One or more repetitions |
| ? | Optional occurrence |
| [0-9] | Any digit |
| [a-z] | Lowercase letter |
| {2} | Exactly two repetitions |
| \| | OR in extended regex |

### DevOps Use Case

Identify patterns across HTTP status codes, timestamps, and structured logs.

---

# 9. cut — Extract Columns

### Purpose

Extracts fields or character ranges from lines.

### Syntax

```bash
cut -d 'delimiter' -f field_number filename
```

### Example 1 — Extract dates

```bash
cut -d ' ' -f 1 logs/application.log
```

### Example 2 — Extract HTTP methods

```bash
cut -d ' ' -f 4 logs/application.log
```

### Example 3 — Extract endpoints

```bash
cut -d ' ' -f 5 logs/application.log
```

### Example 4 — Extract HTTP status codes

```bash
cut -d ' ' -f 6 logs/application.log
```

### Example 5 — Extract multiple fields

```bash
cut -d ' ' -f 4,5,6 logs/application.log
```

### Example 6 — Extract character positions

```bash
cut -c 1-10 logs/application.log
```

Displays the first ten characters of each line.

### Options

| Option | Purpose |
|---|---|
| -d | Delimiter |
| -f | Field numbers |
| -c | Character positions |
| -b | Byte positions |
| --complement | Select fields outside the specified selection |

### DevOps Use Case

Extract status codes, endpoint names, timestamps, and other fields from consistently formatted logs.

---

# 10. awk — Pattern Scanning and Processing

### Purpose

`awk` is a text-processing language used for filtering, field extraction, calculations, and reporting.

### Basic Syntax

```bash
awk 'condition { action }' filename
```

### Example 1 — Print a field

```bash
awk '{print $5}' logs/application.log
```

Displays API endpoints.

### Example 2 — Print multiple fields

```bash
awk '{print $4, $5, $6}' logs/application.log
```

Displays method, endpoint, and status.

### Example 3 — Find HTTP 500

```bash
awk '$6 == 500' logs/application.log
```

### Example 4 — Find HTTP 5xx

```bash
awk '$6 >= 500 && $6 < 600' logs/application.log
```

### Example 5 — Find successful HTTP responses

```bash
awk '$6 >= 200 && $6 < 300' logs/application.log
```

### Example 6 — Count HTTP 500

```bash
awk '$6 == 500 {count++} END {print count+0}' logs/application.log
```

Expected:

```text
6
```

### Example 7 — Count all 5xx

```bash
awk '$6 >= 500 && $6 < 600 {count++} END {print count+0}' logs/application.log
```

Expected:

```text
7
```

### Example 8 — Find slow requests

```bash
awk '$7+0 > 500' logs/application.log
```

The `+0` converts the leading numeric portion of values such as `620ms` into a number.

### Example 9 — Average latency

```bash
awk '{sum += $7+0; count++} END {if(count) print sum/count; else print "N/A"}' logs/application.log
```

Expected:

```text
343
```

The result is in milliseconds.

### Example 10 — Calculate error percentage

```bash
awk '{total++; if($6 >= 500 && $6 < 600) errors++} END {if(total) printf "%.2f%%\n", errors*100/total}' logs/application.log
```

Expected:

```text
35.00%
```

### Example 11 — Count HTTP statuses

```bash
awk '{count[$6]++} END {for(status in count) print status, count[status]}' logs/application.log | sort -n
```

Expected:

```text
200 8
201 1
404 4
500 6
503 1
```

### Example 12 — Count failing endpoints

```bash
awk '$6 >= 500 && $6 < 600 {count[$5]++} END {for(endpoint in count) print count[endpoint], endpoint}' logs/application.log | sort -nr
```

Expected:

```text
5 /api/orders
2 /api/payments
```

### Important awk Variables

| Variable | Meaning |
|---|---|
| $0 | Entire current line |
| $1 | First field |
| $2 | Second field |
| $NF | Last field |
| NF | Number of fields |
| NR | Number of records processed |
| FS | Input field separator |
| OFS | Output field separator |

### Example — Last field

```bash
awk '{print $NF}' logs/application.log
```

Displays response times.

### Example — Line number

```bash
awk '{print NR, $0}' logs/application.log
```

Displays line numbers with the log contents.

### DevOps Use Case

Generate HTTP error reports, calculate latency metrics, and identify failing endpoints directly from logs.

---

# 11. sed — Stream Editor

### Purpose

Processes and transforms text streams.

### Syntax

```bash
sed 'expression' filename
```

### Example 1 — Replace ERROR

```bash
sed 's/ERROR/CRITICAL/' logs/application.log
```

This changes the displayed output without modifying the original file.

### Example 2 — Global replacement

```bash
sed 's/ERROR/CRITICAL/g' logs/application.log
```

`g` replaces every match on each line.

### Example 3 — Print selected lines

```bash
sed -n '1,5p' logs/application.log
```

Displays lines 1–5.

### Example 4 — Delete INFO lines from output

```bash
sed '/INFO/d' logs/application.log
```

### Example 5 — Save modified output

```bash
sed 's/ERROR/CRITICAL/g' logs/application.log > output/critical.log
```

### Example 6 — In-place editing with backup

```bash
cp logs/application.log backup/application.log
sed -i.bak 's/ERROR/CRITICAL/g' output/critical.log
```

`-i.bak` edits the target file in place and creates a backup of its previous contents.

### Options

| Option | Meaning |
|---|---|
| -n | Suppress automatic output |
| -i | Edit file in place |
| -E | Extended regular expressions |
| s | Substitute |
| p | Print |
| d | Delete matching lines from the processing stream |
| g | Replace all matches per line |

### DevOps Use Case

Transform generated configuration files, extract log sections, and prepare text reports.

Avoid modifying live production logs in place.

---

# 12. sort — Sort Lines

### Purpose

Sorts lines alphabetically, numerically, or by selected fields.

### Example 1 — Alphabetical sort

```bash
awk '{print $5}' logs/application.log | sort
```

### Example 2 — Numeric sort

```bash
awk '{print $6}' logs/application.log | sort -n
```

### Example 3 — Reverse sort

```bash
awk '{print $6}' logs/application.log | sort -nr
```

### Example 4 — Unique sorted values

```bash
awk '{print $6}' logs/application.log | sort -u
```

### Options

| Option | Meaning |
|---|---|
| -n | Numeric sort |
| -r | Reverse order |
| -u | Unique lines |
| -k | Sort using a selected field |
| -t | Specify field separator |

### DevOps Use Case

Rank response times, HTTP status codes, and request frequencies.

---

# 13. uniq — Remove or Count Adjacent Duplicates

### Purpose

Processes adjacent identical lines.

### Example 1 — Unique HTTP status codes

```bash
awk '{print $6}' logs/application.log | sort | uniq
```

### Example 2 — Count each status

```bash
awk '{print $6}' logs/application.log | sort | uniq -c
```

Expected counts:

```text
8 200
1 201
4 404
6 500
1 503
```

### Example 3 — Show only repeated values

```bash
awk '{print $6}' logs/application.log | sort | uniq -d
```

### Example 4 — Show values appearing once

```bash
awk '{print $6}' logs/application.log | sort | uniq -u
```

### Options

| Option | Meaning |
|---|---|
| -c | Count occurrences |
| -d | Display duplicate groups |
| -u | Display unique groups |

**Important:** `uniq` compares adjacent lines. Use `sort` first when you need to group identical values from the entire file.

---

# 14. wc — Word Count

### Purpose

Counts lines, words, bytes, and characters.

### Example 1 — Count lines

```bash
wc -l logs/application.log
```

Expected: 20.

### Example 2 — Count words

```bash
wc -w logs/application.log
```

### Example 3 — Count bytes

```bash
wc -c logs/application.log
```

### Example 4 — Count characters

```bash
wc -m logs/application.log
```

### Example 5 — Count ERROR entries

```bash
grep "ERROR" logs/application.log | wc -l
```

Expected:

```text
7
```

### Options

| Option | Meaning |
|---|---|
| -l | Lines |
| -w | Words |
| -c | Bytes |
| -m | Characters |
| -L | Length of longest line |

---

# 15. Pipes and Redirection

## Pipe — `|`

Connects one command's standard output to another command's standard input.

```bash
grep "ERROR" logs/application.log | wc -l
```

## Output Redirection — `>`

Overwrites a file with standard output.

```bash
grep "ERROR" logs/application.log > output/errors.log
```

## Append Redirection — `>>`

Appends standard output.

```bash
echo "Investigation completed" >> output/errors.log
```

## Error Redirection — `2>`

Redirects standard error.

```bash
cat missing.log 2> output/errors.txt
```

## Append Standard Error — `2>>`

```bash
cat missing.log 2>> output/errors.txt
```

## Redirect Standard Output and Error

```bash
cat missing.log > output/result.txt 2>&1
```

`2>&1` sends standard error to the current destination of standard output.

### Common Operators

| Operator | Meaning |
|---|---|
| \| | Pipe |
| > | Overwrite stdout |
| >> | Append stdout |
| 2> | Redirect stderr |
| 2>> | Append stderr |
| 2>&1 | Send stderr to stdout's destination |
| < | Redirect file to stdin |

---

# 16. tee — Display and Save Output

### Purpose

Writes input to standard output and one or more files.

### Example

```bash
grep "ERROR" logs/application.log | tee output/error-report.log
```

### Append instead of overwrite

```bash
grep "WARN" logs/application.log | tee -a output/error-report.log
```

`-a` appends to the file.

### DevOps Use Case

Display deployment output while saving it for troubleshooting or auditing.

---

# 17. diff — Compare Files

### Purpose

Displays differences between files.

Create examples:

```bash
printf 'server.port=8080\nlogging.level=INFO\n' > backup/old.properties
printf 'server.port=9090\nlogging.level=DEBUG\n' > backup/new.properties
```

Compare:

```bash
diff backup/old.properties backup/new.properties
```

### Unified diff

```bash
diff -u backup/old.properties backup/new.properties
```

### Ignore whitespace changes

```bash
diff -w backup/old.properties backup/new.properties
```

### Options

| Option | Meaning |
|---|---|
| -u | Unified diff |
| -w | Ignore whitespace differences |
| -q | Report whether files differ |
| -r | Compare directories recursively |

### DevOps Use Case

Compare configuration changes before and after deployment.

---

# 18. comm — Compare Sorted Files

### Purpose

Compares two sorted files and displays three columns:

1. Lines only in the first file
2. Lines only in the second file
3. Lines common to both files

### Example

```bash
sort backup/old.properties > output/old-sorted.txt
sort backup/new.properties > output/new-sorted.txt
comm output/old-sorted.txt output/new-sorted.txt
```

### Show only common lines

```bash
comm -12 output/old-sorted.txt output/new-sorted.txt
```

`-1` suppresses the first column, and `-2` suppresses the second.

### DevOps Use Case

Compare sorted inventories, package lists, and configuration entries.

---

# 19. Real-World Production Troubleshooting

## Scenario — Spring Boot HTTP 500 Errors

**Application:** Order Service  
**Issue:** Increased server errors  
**Impact:** Order creation failures

### Step 1 — Count total requests

```bash
wc -l logs/application.log
```

Expected: 20.

### Step 2 — Count HTTP 500

```bash
awk '$6 == 500' logs/application.log | wc -l
```

Expected: 6.

### Step 3 — Count all 5xx

```bash
awk '$6 >= 500 && $6 < 600' logs/application.log | wc -l
```

Expected: 7.

### Step 4 — Calculate server error rate

```bash
awk '{total++; if($6 >= 500 && $6 < 600) errors++} END {if(total) printf "%.2f%%\n", errors*100/total}' logs/application.log
```

Expected:

```text
35.00%
```

### Step 5 — Find affected endpoints

```bash
awk '$6 >= 500 && $6 < 600 {print $5}' logs/application.log | sort | uniq -c | sort -nr
```

Expected:

```text
5 /api/orders
2 /api/payments
```

### Step 6 — Find slow requests

```bash
awk '$7+0 > 500' logs/application.log
```

### Step 7 — Calculate average latency

```bash
awk '{sum += $7+0; count++} END {if(count) printf "%.2f ms\n", sum/count}' logs/application.log
```

Expected:

```text
343.00 ms
```

### Step 8 — Generate an error report

```bash
awk '$6 >= 500 && $6 < 600' logs/application.log > output/server-errors.log
```

### Step 9 — Save investigation results

```bash
awk '$6 >= 500 && $6 < 600' logs/application.log | tee output/investigation.log
```

### Findings

| Metric | Value |
|---|---|
| Total requests | 20 |
| HTTP 200 | 8 |
| HTTP 201 | 1 |
| HTTP 404 | 4 |
| HTTP 500 | 6 |
| HTTP 503 | 1 |
| Total server errors | 7 |
| Server error rate | 35% |
| Average latency | 343 ms |
| Most affected endpoint | /api/orders |

**Important:** These findings identify symptoms. A production root-cause analysis would require exception details, dependency health, deployment history, and application metrics.

---

# 20. Quick Command Reference

| Command | Purpose |
|---|---|
| cat | Display or concatenate files |
| head | Display beginning of file |
| tail | Display end of file |
| tail -f | Follow appended log data |
| tail -F | Follow log filename across replacement |
| less | Interactive file viewer |
| grep | Search text |
| grep -i | Ignore case |
| grep -n | Display line numbers |
| grep -c | Count matching lines |
| grep -v | Invert match |
| grep -r | Recursive search |
| grep -E | Extended regex |
| cut | Extract fields |
| awk | Filter and process structured text |
| sed | Stream editing |
| sort | Sort lines |
| uniq | Process adjacent duplicates |
| wc | Count lines, words, and bytes |
| tee | Display and save output |
| diff | Compare files |
| comm | Compare sorted files |
| \| | Pipe |
| > | Overwrite output file |
| >> | Append output |
| 2> | Redirect errors |

---

# 21. Skills Learned

After completing Phase 4, I will be able to:

- Analyze Linux application logs
- Search ERROR and WARN messages
- Use regular expressions
- Extract fields from structured logs
- Filter HTTP status codes
- Calculate API response times
- Identify slow endpoints
- Calculate server error rates
- Compare configuration files
- Generate incident investigation reports
- Combine commands using pipes
- Save analysis results for documentation

## Next Phase

**Phase 05 — Linux Process Management**

Topics:

- `ps`
- `top`
- `htop`
- `pgrep`
- `pidof`
- `kill`
- `pkill`
- `nice`
- `renice`
- `jobs`
- `bg`
- `fg`
- `nohup`
- Process states
- Zombie processes
- High CPU troubleshooting