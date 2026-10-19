```bash
#!/usr/bin/env bash
today=$(date + "%y - %m - %y")
file="/tmp/$today"
file2="/var/log/syslog"
# check if file already exists
if [[ -f "$file" ]]; then
	echo "file $file already exist"
else
	touch $file
	echo "File created"
fi
# check if file2 exist and readable
if [[ ! -f "$file2" || ! -r "$file2" ]]; then
	echo "Error: $file2 does not exist or is not readable"
	exit 1
fi
```

```bash
#!/usr/bin/env bash
# $1 is the first argument passed to the script
# Example: ./service.sh start
ACTION="$1"
# Check if the user provided an action
if [ -z "$ACTION" ]; then
    echo "Error: No action provided."
    echo "Usage: $0 [start|stop|restart]"
    exit 1
fi
case "$ACTION" in
    start)
        echo "Starting the service..."
        ;;
    stop)
        echo "Stopping the service..."
        ;;
    restart)
        echo "Restarting the service..."
        ;;
    *)
        echo "Error: Unknown action '$ACTION'."
        echo "Usage: $0 [start|stop|restart]"
        exit 1
        ;;
esac
echo "Done."
```

```bash
for fruit in mango apple
do
echo "i like $fruit"
done
# i like mango
# i like apple
```

```bash
#!/usr/bin/env bash
echo "--- 1. Unquoted \$* ---"
echo "Result: Arguments are split on spaces (WRONG)"
echo "Loop count depends on word-splitting"
for arg in $*; do
    echo "Argument: '$arg'"
done
echo
echo "--- 2. Quoted \"\$*\" ---"
echo "Result: Loop runs 1 time (WRONG)"
echo "\"\$*\" expands to ONE string: all args joined by spaces"
for arg in "$*"; do
    echo "Argument: '$arg'"
done
echo
echo "--- 3. Unquoted \$@ ---"
echo "Result: Arguments are split on spaces (WRONG)"
echo "Same problem as \$*: word splitting"
for arg in $@; do
    echo "Argument: '$arg'"
done
echo
echo "--- 4. Quoted \"\$@\" (Correct way) ---"
echo "Result: Loop runs once per REAL argument (CORRECT)"
echo "\"\$@\" preserves each argument as a separate item"
for arg in "$@"; do
    echo "Argument: '$arg'"
done
```

```bash
for file in *.jpeg; do  # changes the .jpeg extension
    mv "$file" "${file%.jpeg}.jpg"   # % strips .jpeg part and append file to .jpg
done
```

```bash
#!/usr/bin/env bash
FILENAME="my_file.txt"
# The canonical way to read a file line by line -> Low Memory Usage
while IFS= read -r line; do
    echo "LINE: $line"
done < "$FILENAME"
# `IFS=` (Preserves Whitespace) / `-r` (Prevents Backslash Escapes)
```

```bash
#!/usr/bin/env bash
echo "Waiting for service on port 8080 to be ready..."
# -s (silent)
# -f (fail fast, don't output error)
until curl -s -f http://localhost:8080/health; do
    echo "Service is not ready yet. Retrying in 5 seconds..."
    sleep 5
done
echo "SERVICE IS READY! Continuing deployment..."
```

```bash
#!/usr/bin/env bash
# 1. Define the function
get_formatted_date() {
    # $1 is the first argument passed to the function
    local name="$1"
    local date
    date=$(date +"%Y-%m-%d")

    # "Return" the string by echoing it
    echo "$name (Date: $date)"
}
# 2. Call the function and capture the output
user_string=$(get_formatted_date "Ahmed")
report_string=$(get_formatted_date "Report_Backup")
echo "Result 1: $user_string"
echo "Result 2: $report_string"
```

```bash
#!/usr/bin/env bash
LOG_DIR="/var/log/my-app"
FOUND_FILE=""
# Loop through all files ending with .log
for log_file in "$LOG_DIR"/*.log; do

    echo "Scanning file: $log_file"
    # -q (quiet) makes grep silent; we only care about the exit code
    if grep -q "FATAL_ERROR" "$log_file"; then
        echo "Found error in: $log_file"
        FOUND_FILE="$log_file"

        # Stop scanning once we find the first corrupted file
        break # or continue
    fi
done
# The script continues here after the break
if [ -n "$FOUND_FILE" ]; then
    echo "The first corrupted file is: $FOUND_FILE"
    # You can send an email or alert here
else
    echo "No corrupted files found."
fi
```


```bash
#!/usr/bin/env bash
# 1. Define the associative array
declare -A error_counts
LOG_FILE="app.log"
# 2. Make sure the log file exists
if [[ ! -f "$LOG_FILE" ]]; then
    echo "Error: Log file not found: $LOG_FILE"
    exit 1
fi
# 3. Read the file line by line
# We use awk with ':' as a delimiter to extract the first field (e.g., ERROR, INFO, WARN)
while IFS= read -r line; do
    # Extract the part before the first colon
    log_type=$(echo "$line" | awk -F: '{print $1}')
    # Skip empty lines or lines where log_type is empty
    if [[ -z "$log_type" ]]; then
        continue
    fi
    # 4. Increment the counter for this log type
    # If the key doesn't exist, Bash treats its value as 0 in arithmetic context
    error_counts["$log_type"]=$(( ${error_counts["$log_type"]} + 1 ))
done < "$LOG_FILE"
# 5. Print the report (looping over keys)
echo "--- Log Analysis Report ---"
for type in "${!error_counts[@]}"; do
    echo "$type: ${error_counts[$type]}"
done
```