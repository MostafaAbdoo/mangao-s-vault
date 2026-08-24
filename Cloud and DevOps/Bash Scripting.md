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
# to return data from a function use echo command
```