```bash
#!/usr/bin/env bash

# server_health_check.sh
# A DevOps capstone project script to check the health
# of multiple remote servers via SSH.
# Usage: ./server_health_check.sh -f <server_list_file> -u <remote_user>

# --- Part 1: "Strict Mode" ---
# set -e: exit immediately if any command fails
# set -u: exit if an undefined variable is used
# set -o pipefail: if any command in a pipeline fails, treat the whole pipeline as failed
set -euo pipefail

# --- Global Constants ---
LOG_FILE=$(mktemp /tmp/server_health.XXXXXX)
readonly LOG_FILE

# --- Function Definitions ---

# Log an informational message to both screen and log file
log_info() {
    echo "[INFO] $1" | tee -a "$LOG_FILE"
}

# Log an error message to stderr and to the log file
log_error() {
    echo "[ERROR] $1" | tee -a "$LOG_FILE" >&2
}

# Print script usage
print_usage() {
    echo "Usage: $0 -f <server_list_file> -u <remote_user>"
    echo "  -f: Path to a file containing a list of servers (one per line)."
    echo "  -u: The remote SSH user to connect as."
    echo "  -h: Display this help message."
}

# Cleanup temporary log file on exit
cleanup() {
    echo "Cleaning up temporary log file: $LOG_FILE"
    rm -f "$LOG_FILE"
}

trap cleanup EXIT INT TERM

echo "Script started. Log file created at: $LOG_FILE"

# Performs the actual health check on a remote server
check_server() {
    local server="$1"
    local user="$2"

    log_info "--- Checking Server: $server ---"

    # Use an SSH here-document to send a batch of commands in one connection
    ssh -n -o ConnectTimeout=5 "${user}@${server}" << 'EOF'
# 1. Uptime Check
echo "--- System Uptime ---"
uptime

# 2. Disk Check (Root partition)
echo "--- Disk Usage (Root /) ---"
df -h / | awk 'NR==2 {print "Used: " $5 " (" $3 "/" $2 ")"}'

# 3. Memory Check
echo "--- Memory Usage ---"
free -m | awk 'NR==2 {
    printf "Used: %sMB / Total: %sMB (%.2f%%)\n", $3, $2, ($3/$2)*100
}'

# 4. Security Check (SSH brute-force failures)
echo "--- Security (SSH) ---"
AUTH_LOG="/var/log/auth.log"
if [ -f "$AUTH_LOG" ]; then
    count=$(grep -c "Failed password" "$AUTH_LOG")
    echo "Failed SSH Attempts: $count"
else
    echo "Failed SSH Attempts: auth log not found."
fi
EOF

    log_info "--- Finished Check: $server ---"
}

# Main function: core logic of the script
main() {
    local server_file=""
    local remote_user=""

    # --- Argument Parsing (using getopts) ---
    while getopts ":f:u:h" opt; do
        case "$opt" in
            f)
                server_file="$OPTARG"
                ;;
            u)
                remote_user="$OPTARG"
                ;;
            h)
                print_usage
                exit 0
                ;;
            \?)
                log_error "Invalid option: -$OPTARG"
                print_usage
                exit 1
                ;;
            :)
                log_error "Option -$OPTARG requires an argument."
                print_usage
                exit 1
                ;;
        esac
    done

    # --- Input Validation ---
    if [ -z "$server_file" ] || [ -z "$remote_user" ]; then
        log_error "Missing required arguments."
        print_usage
        exit 1
    fi

    if [ ! -f "$server_file" ]; then
        log_error "Server file not found: $server_file"
        exit 1
    fi

    declare -a servers=()

    # Read the server file line by line safely
    while IFS= read -r line || [ -n "$line" ]; do
        # Skip empty lines and comment lines
        if [[ -z "$line" || "$line" == \#* ]]; then
            continue
        fi
        servers+=("$line")
    done < "$server_file"

    if [ ${#servers[@]} -eq 0 ]; then
        log_error "No servers found in $server_file. Exiting."
        exit 1
    fi

    log_info "Configuration valid. Starting health checks..."
    log_info "Found ${#servers[@]} servers to check. Starting..."

    for server_host in "${servers[@]}"; do
        check_server "$server_host" "$remote_user"
    done

    log_info "All checks completed."
}

# --- Execution ---
main "$@"
```