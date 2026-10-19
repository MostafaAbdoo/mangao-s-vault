```bash
#! /usr/bin/env bash   -> indicates start of a script and excutes bash regardless of it's location  
# -> the extension is .sh and to run it -> ./$NAME.sh but first -> chmod +x $NAME
MY_NAME="Ahmed" # -> sets a shell variable ((No Spaces))
echo 'Hello, $MY_NAME' # -> Hello, $MY_NAME  -> literal printing
echo "Hello, $MY_NAME" # -> Hello, Ahmed
# use bachslash for special character, name (local) shell variables in small letters to avoid crashing with system variables
TODAY=$(date) # -> excutes the inner command first and assign it

sum=$(($x+$y)) # for mathematical calculations
echo "sum of $x and $y is $sum" # -> sum of 5 and 10 is 15

# if conditions ((BEWARE THE SPACES))
if [[ condition ]]; then
... some commands...
else
... some commands...
fi


read -p "Enter text" text  # stores input in text variable
exit 1 # to exit with an error
id -u # return user id, note that root's id is zero
&& and || exists too

# string comparison
# u can use == / != / > / < between "$string1" and "$string2"
# u can use == for pattern also like this -> "string.txt" == *.txt -> true
# "string.txt" == "*.txt" -> false
-z "$string" # returns true if string is empty
-n "$string" # returns true if string is non-empty

# integer comparison
# between $int1 and $int2 -> -eq / -ne / -gt / -ge / -lt / -le

# file tests
-e "$path" # returns true if path exits for a file or a directory
-f "$path" # returns true if path exits for a file
-d "$path" # returns true if path exits for a directory
-s "$path" # reurns size of a file if not empty
-r "$path" # return true if readable
-w "$path" # returns true if writable
-x "$path" # returns true if executable

# case statement
case "$variable" in
	PATTERN_1)
		# command
	;;
	PATTERM_2)
		# command
	;;
	*)
		# command for default case
	;;
esac
arg=$1 # first argument after command
com=$0 # command itself

# for loop
for $VARIABLE in $LIST; do
# commands
done

# $# -> number of arguments passed
# $* and $@ both bring the arguments passed to the script

# while loop is used untill the condition is false but the untill loop is used untill the condition is true
# break or continue keyword can have a number after them to indicate how many loops to escape (like if u r in nested loops)

# functions
fun_name(){
# command
}
# return keyword returns exit code not data, u can use exit codes from 1 to 255 for your own meanings, exit 0 is done with no problem
# to return data from a function use echo command and capture it using $(script)

# arrays
servers=("web-01" "web-02" "db-01" "monitoring")
echo "The database server is: ${servers[0]}"
${#servers[@]}  -> # returns length of the array
servers+=$NEW_ELEMENT
for items in "${servers[@]}"  # to loop an array

# maps
declare -A $NAME
$NAME[$KEY]="$VALUE"
# or u can do all of this in one line
declare -A $NAME=([$KEY2]="$VALUE1" $KEY2]="$VALUE1")
${$NAME[$KEY]}  # to access it
# to loop over a map
for server in "${!servers[@]}"; do
echo"Server: $server ---- IP:${servers[$server]}  # first one is key second is value
```

```bash
# grep
-i # ignore case (captial or small)
-v # invert match (find all line that doesn't contain this word)
-c # count
-r # recursive
-E # extended regex -> grep -E "ERROR|FATAL|CRITICAL" /var/log/syslog -> more that one word to search for
-o # only prints the matching part not all of the line
-A, -B, -C # after, before, context   -> put a number after for how many lines to print (after or before or both)

# sed  -> to edit stream data
sed 's/FIND_THIS/REPLACE_WITH_THIS/g'  # s for substituite and g for global
# u can replace / with anything as a seperator
sed -i 's/FIND_THIS/REPLACE_WITH_THIS/g' filename.txt  # to edit on the original file

# awk   -> filter, manipulate, and extract structured text data—such as logs
awk 'pattern { action }' file.txt
# $0 -> The entire current line/record
# $1, $2, $3 -> The 1st, 2nd, and 3rd field on the current line
# NF -> Number of Fields
# NR -> Number of Records
# FS -> Input Field Separator (defaults to whitespace)
# OFS -> Output Field Separator (defaults to space)

# BEGIN { ... }: Executes **before** any input text is read. Ideal for defining variables or field separators.
    
# END { ... }: Executes **after** all input lines have been processed. Ideal for printing totals, averages, or final summaries.
# Use `-F` to define a custom field separator
awk -F':' '{ print $1, $6 }' /etc/passwd
awk -F':' '$3 >= 1000 { print $1 }' /etc/passwd # Print the username ($1) for accounts with UID ($3) greater than or equal to 1000
awk '/ERROR/ { print $0 }' log.txt # Print lines containing the word `ERROR`
awk '{ sum += $2 } END { print "Total:", sum }' numbers.txt # Sum the values in column 2 and output the total at the end
```