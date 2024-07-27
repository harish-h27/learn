

## Introduction
Shell scripts are interpreted, not compiled.

To list the different types of shells supported by your OS, use:

```sh
cat /etc/shells
```

The first line of every shell script should specify the interpreter:

```sh
#! /bin/bash
```


Steps to create and run a simple shell script:

1. Create a new script file:
```sh
touch hello.sh
```

2. Edit the script file to include:
```sh
echo "hello world"
```

3. Make the script executable:
```sh
chmod +x hello.sh
```

4. Run the script:
```sh
./hello.sh
```


## Using Variables and Comments

```sh
# This is a comment
```

## System Variables: Variables in capital letters are system or predefined variables.
```sh
echo $BASH
echo $BASH_VERSION
echo $HOME
echo $PWD
```

## User-Defined Variables:
```sh
name=mark
echo $name
echo "The name is $name abc"
```


## Reading User Input
Single Input:
```sh
echo "Enter name:"
read name
echo "Entered name: $name"
```


## Multiple Inputs:
```sh
echo "Enter names:"
read name1 name2 name3
echo "Names: $name1, $name2, $name3"
```



## Input on the Same Line:
```sh
read -p 'Username: ' user_var
echo "Our username is $user_var"
```




## Reading Password (hidden input):
```sh
read -sp 'Password: ' pass_var
echo "Password is $pass_var"
```

## Reading Array Input:
```sh
echo "Enter names:"
read -a names
echo "Names: ${names[0]}"
```

## Default Variable for Input:
```sh
echo "Enter name:"
read
echo "Name: $REPLY"
```


## Passing Arguments to a Bash Script
Accessing Arguments:
```sh
echo $1 $2 $3 $4
```
Running the script:
./hello.sh mark john ben king


## Script Name:
```sh
echo $0  # Stores the name of your shell script
```

## All Arguments:
```sh
echo "$@"
args=("$@")  # Convert all arguments to an array
echo ${args[0]} ${args[1]}
echo $@
echo $#  # Length of passed arguments
```


## Creating an Array
```sh
myArray=("cat" "dog" "mouse" "frog")
```


## If Statement
```sh
if [ condition ]
then
    statement
fi
```



## Integer Comparison Operators

```sh
# Is equal to
a=5
b=10
if [ "$a" -eq "$b" ]  # Checks if a is equal to b
then
    echo "a is equal to b"
else
    echo "a is not equal to b"
fi

# Is not equal to
a=5
b=10
if [ "$a" -ne "$b" ]  # Checks if a is not equal to b
then
    echo "a is not equal to b"
else
    echo "a is equal to b"
fi

# Is greater than
a=15
b=10
if [ "$a" -gt "$b" ]  # Checks if a is greater than b
then
    echo "a is greater than b"
else
    echo "a is not greater than b"
fi

# Is less than
a=5
b=10
if [ "$a" -lt "$b" ]  # Checks if a is less than b
then
    echo "a is less than b"
else
    echo "a is not less than b"
fi

# Is greater than or equal to
a=10
b=10
if [ "$a" -ge "$b" ]  # Checks if a is greater than or equal to b
then
    echo "a is greater than or equal to b"
else
    echo "a is less than b"
fi

# Is less than or equal to
a=5
b=10
if [ "$a" -le "$b" ]  # Checks if a is less than or equal to b
then
    echo "a is less than or equal to b"
else
    echo "a is greater than b"
fi

# Arithmetic comparison within (( ))
a=5
b=10
if (( a < b ))  # Checks if a is less than b
then
    echo "a is less than b"
else
    echo "a is not less than b"
fi

# Other comparisons within (( ))
if (( a > b ))  # Checks if a is greater than b
then
    echo "a is greater than b"
else
    echo "a is not greater than b"
fi

if (( a == b ))  # Checks if a is equal to b
then
    echo "a is equal to b"
else
    echo "a is not equal to b"
fi

if (( a != b ))  # Checks if a is not equal to b
then
    echo "a is not equal to b"
else
    echo "a is equal to b"
fi
```

### sample code
```sh
word=a
if [ "$word" -eq "b" ]
then
    echo "Condition b is true"
elif [ "$word" -eq "a" ]
then 
    echo "Condition a is true"
else 
    echo "Condition is false"
fi
```


## String Comparison Operators

```sh
# Is equal to
str1="hello"
str2="world"
if [ "$str1" = "$str2" ]  # Checks if str1 is equal to str2
then
    echo "str1 is equal to str2"
else
    echo "str1 is not equal to str2"
fi

# Is not equal to
str1="hello"
str2="world"
if [ "$str1" != "$str2" ]  # Checks if str1 is not equal to str2
then
    echo "str1 is not equal to str2"
else
    echo "str1 is equal to str2"
fi

# Is greater than (lexicographically)
str1="apple"
str2="banana"
if [[ "$str1" > "$str2" ]]  # Checks if str1 is greater than str2
then
    echo "str1 is greater than str2"
else
    echo "str1 is not greater than str2"
fi

# Is less than (lexicographically)
str1="apple"
str2="banana"
if [[ "$str1" < "$str2" ]]  # Checks if str1 is less than str2
then
    echo "str1 is less than str2"
else
    echo "str1 is not less than str2"
fi

# String is empty
str=""
if [ -z "$str" ]  # Checks if the string is empty
then
    echo "str is empty"
else
    echo "str is not empty"
fi

# String is not empty
str="hello"
if [ -n "$str" ]  # Checks if the string is not empty
then
    echo "str is not empty"
else
    echo "str is empty"
fi
```









## File Test Operators

```sh
# -e enables interpretation of backslash escapes
# \c suppresses the trailing newline
echo -e "Enter the name of the file: \c"
read file_name


# Check if file exists
if [ -e "$file_name" ]  # -e: Checks if the file exists
then
    echo "$file_name found"
else
    echo "$file_name not found"
fi

# Check if it's a regular file
if [ -f "$file_name" ]  # -f: Checks if the file is a regular file (not a directory or device file)
then
    echo "$file_name is a regular file"
else
    echo "$file_name is not a regular file"
fi

# Check if it's a directory
if [ -d "$file_name" ]  # -d: Checks if the file is a directory
then
    echo "$file_name is a directory"
else
    echo "$file_name is not a directory"
fi

# Check if it's a block special file
if [ -b "$file_name" ]  # -b: Checks if the file is a block special file (e.g., a hard drive)
then
    echo "$file_name is a block special file"
else
    echo "$file_name is not a block special file"
fi

# Check if it's a character special file
if [ -c "$file_name" ]  # -c: Checks if the file is a character special file (e.g., a keyboard or mouse)
then
    echo "$file_name is a character special file"
else
    echo "$file_name is not a character special file"
fi

# Check if the file is empty
if [ -s "$file_name" ]  # -s: Checks if the file is not empty
then
    echo "$file_name is not empty"
else
    echo "$file_name is empty"
fi

# Check if the file has read permission
if [ -r "$file_name" ]  # -r: Checks if the file has read permission
then
    echo "$file_name has read permission"
else
    echo "$file_name does not have read permission"
fi

# Check if the file has write permission
if [ -w "$file_name" ]  # -w: Checks if the file has write permission
then
    echo "$file_name has write permission"
else
    echo "$file_name does not have write permission"
fi

# Check if the file has execute permission
if [ -x "$file_name" ]  # -x: Checks if the file has execute permission
then
    echo "$file_name has execute permission"
else
    echo "$file_name does not have execute permission"
fi
```

## How to Append Output to the End of the File

```sh
# Prompt the user to enter the name of the file
echo -e "Enter the file name: \c"

# Read the file name input from the user
read file_name

# Check if the file exists
if [ -f "$file_name" ]
then 
    # Check if the file has write permission
    if [ -w "$file_name" ]
    then 
        # Prompt the user to type some text data and quit by pressing Ctrl + D
        echo "Type some text data to append to the file. To quit, press Ctrl + D."
        
        # Use cat with the append operator >> to add the user input to the file
        cat >> "$file_name" 
    else
        # Inform the user that the file does not have write permission
        echo "The file does not have write permission."
    fi 
else 
    # Inform the user that the file does not exist
    echo "$file_name does not exist."
fi
```





## Logical AND Operator

```sh
# Logical AND with Numbers

num1=10
num2=20

if [ "$num1" -eq 10 ] && [ "$num2" -eq 20 ]  # Checks if num1 equals 10 AND num2 equals 20
then
    echo "Both conditions are true"
else
    echo "One or both conditions are false"
fi

# Logical AND with Strings

str1="hello"
str2="world"

if [ "$str1" = "hello" ] && [ "$str2" = "world" ]  # Checks if str1 equals "hello" AND str2 equals "world"
then
    echo "Both string conditions are true"
else
    echo "One or both string conditions are false"
fi

# Logical AND with Mixed Types

num1=10
str1="hello"

if [ "$num1" -eq 10 ] && [ "$str1" = "hello" ]  # Checks if num1 equals 10 AND str1 equals "hello"
then
    echo "Both number and string conditions are true"
else
    echo "One or both conditions are false"
fi

# Using Double Brackets for Logical AND

if [[ "$num1" -eq 10 && "$str1" = "hello" ]]  # Another way to use logical AND with double brackets
then
    echo "Both conditions are true with double brackets"
else
    echo "One or both conditions are false with double brackets"
fi
```


## Logical OR Operator

```sh
# Logical OR with Numbers

num1=10
num2=20

if [ "$num1" -eq 10 ] || [ "$num2" -eq 30 ]  # Checks if num1 equals 10 OR num2 equals 30
then
    echo "At least one condition is true"
else
    echo "Both conditions are false"
fi

# Logical OR with Strings

str1="hello"
str2="world"

if [ "$str1" = "hello" ] || [ "$str2" = "planet" ]  # Checks if str1 equals "hello" OR str2 equals "planet"
then
    echo "At least one string condition is true"
else
    echo "Both string conditions are false"
fi

# Logical OR with Mixed Types

num1=10
str1="hello"

if [ "$num1" -eq 5 ] || [ "$str1" = "hello" ]  # Checks if num1 equals 5 OR str1 equals "hello"
then
    echo "At least one number and string condition is true"
else
    echo "Both conditions are false"
fi

# Using Double Brackets for Logical OR

if [[ "$num1" -eq 10 || "$str1" = "world" ]]  # Another way to use logical OR with double brackets
then
    echo "At least one condition is true with double brackets"
else
    echo "Both conditions are false with double brackets"
fi
```

## Performing Arithmetic Operations

```sh
# Define two numbers
num1=20
num2=5

# wrong way 
echo $num1 + $num2 # dont do this 

# Addition
result=$((num1 + num2))  # Adds num1 and num2
echo "Addition: $num1 + $num2 = $result"

# Subtraction
result=$((num1 - num2))  # Subtracts num2 from num1
echo "Subtraction: $num1 - $num2 = $result"

# Multiplication
result=$((num1 * num2))  # Multiplies num1 by num2
echo "Multiplication: $num1 * $num2 = $result"

# Division
result=$((num1 / num2))  # Divides num1 by num2
echo "Division: $num1 / $num2 = $result"

# Modulus
result=$((num1 % num2))  # Finds the remainder of num1 divided by num2
echo "Modulus: $num1 % $num2 = $result"

# Increment
num1=$((num1 + 1))  # Increments num1 by 1
echo "Increment: num1 incremented by 1 = $num1"

# Decrement
num2=$((num2 - 1))  # Decrements num2 by 1
echo "Decrement: num2 decremented by 1 = $num2"

# Using expr for arithmetic operations
result=$(expr $num1 + $num2)  # Adds num1 and num2 using expr
echo "Addition with expr: $num1 + $num2 = $result"

result=$(expr $num1 - $num2)  # Subtracts num2 from num1 using expr
echo "Subtraction with expr: $num1 - $num2 = $result"

result=$(expr $num1 \* $num2)  # Multiplies num1 by num2 using expr (note the escape character)
echo "Multiplication with expr: $num1 * $num2 = $result"

result=$(expr $num1 / $num2)  # Divides num1 by num2 using expr
echo "Division with expr: $num1 / $num2 = $result"

result=$(expr $num1 % $num2)  # Finds the remainder of num1 divided by num2 using expr
echo "Modulus with expr: $num1 % $num2 = $result"
```








## Performing Floating-Point Arithmetic Operations

```sh
# Define two floating-point numbers
num1=20.5
num2=5.3

# Addition
result=$(echo "$num1 + $num2" | bc)  # Adds num1 and num2 using bc
echo "Addition: $num1 + $num2 = $result"

# Subtraction
result=$(echo "$num1 - $num2" | bc)  # Subtracts num2 from num1 using bc
echo "Subtraction: $num1 - $num2 = $result"

# Multiplication
result=$(echo "$num1 * $num2" | bc)  # Multiplies num1 by num2 using bc
echo "Multiplication: $num1 * $num2 = $result"

# Division
result=$(echo "scale=2; $num1 / $num2" | bc)  # Divides num1 by num2 using bc with scale set to 2 decimal places
echo "Division: $num1 / $num2 = $result"

# Modulus (bc does not support modulus operation directly for floating-point numbers)
int_num1=${num1%.*}  # Extracts integer part of num1
int_num2=${num2%.*}  # Extracts integer part of num2
result=$(echo "$int_num1 % $int_num2" | bc)  # Finds the remainder of int_num1 divided by int_num2 using bc
echo "Modulus: $int_num1 % $int_num2 = $result"

# Exponentiation
result=$(echo "$num1 ^ $num2" | bc)  # Raises num1 to the power of num2 using bc
echo "Exponentiation: $num1 ^ $num2 = $result"

# Square root
result=$(echo "scale=2; sqrt($num1)" | bc)  # Calculates the square root of num1 using bc with scale set to 2 decimal places
echo "Square root of $num1 = $result"

# Increment
num1=$(echo "$num1 + 1" | bc)  # Increments num1 by 1 using bc
echo "Increment: num1 incremented by 1 = $num1"

# Decrement
num2=$(echo "$num2 - 1" | bc)  # Decrements num2 by 1 using bc
echo "Decrement: num2 decremented by 1 = $num2"
```


## The Case Statement

```sh
# Prompt the user to enter a value
echo -e "Enter a value: \c"
read value

# Use the case statement to match the entered value
case $value in
    # Matches the number 1
    1)
        echo "You entered number 1"
        ;;
    # Matches the number 2
    2)
        echo "You entered number 2"
        ;;
    # Matches the string 'hello'
    "hello")
        echo "You entered 'hello'"
        ;;
    # Matches the string 'world'
    "world")
        echo "You entered 'world'"
        ;;
    # Matches any other value
    *)
        echo "You entered an unknown value"
        ;;
esac
```


## simple case program

```sh
echo -e "enter some character: \c"
read value

case $value in
    [a-z])
        echo "user entered $vaue a to z";;
    [0-9])
        echo "user entered $vaue 0 to 9";;
    ?)
        echo "user entered $vaue special character";;
    *) 
        echo "unknonw input";;
esac
    
```



## Arrays of Numbers and Strings

```sh
# Array of Numbers

# Define an array of numbers
numbers=(1 2 3 4 5)

# Access and print elements of the numbers array
echo "First number: ${numbers[0]}"
echo "Second number: ${numbers[1]}"
echo "All numbers: ${numbers[@]}"

# Loop through the numbers array
echo "Looping through numbers array:"
for num in "${numbers[@]}"
do
    echo "$num"
done

# Array of Strings

# Define an array of strings
strings=("hello" "world" "bash" "scripting")

# Access and print elements of the strings array
echo "First string: ${strings[0]}"
echo "Second string: ${strings[1]}"
echo "All strings: ${strings[@]}"

# Loop through the strings array
echo "Looping through strings array:"
for str in "${strings[@]}"
do
    echo "$str"
done

# Adding an element to an array
numbers+=(6)  # Adds the number 6 to the numbers array
strings+=("tutorial")  # Adds the string "tutorial" to the strings array

# Print updated arrays
echo "Updated numbers array: ${numbers[@]}"
echo "Updated strings array: ${strings[@]}"

# Length of the arrays
echo "Length of numbers array: ${#numbers[@]}"
echo "Length of strings array: ${#strings[@]}"

# Removing an element from an array
unset numbers[2]  # Removes the third element from the numbers array
unset strings[1]  # Removes the second element from the strings array

# Print arrays after removal
echo "Numbers array after removal: ${numbers[@]}"
echo "Strings array after removal: ${strings[@]}"
```



## Using While Loops

```sh
# Basic While Loop

count=1

# Loop while the count is less than or equal to 5
while [ $count -le 5 ]
do
    echo "Count: $count"
    count=$((count + 1))  # Increment the count
done

# Using Sleep in a While Loop

counter=1

# Loop with sleep to pause execution
while [ $counter -le 5 ]
do
    echo "Counter: $counter"
    sleep 1  # Pause for 1 second
    counter=$((counter + 1))  # Increment the counter
done

# Open a Terminal Window with a While Loop

# Loop to open multiple terminal windows (example with gnome-terminal)
# Adjust the command according to your terminal emulator
while true
do
    gnome-terminal -- bash -c "echo 'Hello from new terminal'; exec bash" # you can use any terminal you want like xterm
    sleep 10  # Wait 10 seconds before opening another terminal
done

# Reading File Content Line by Line Using a While Loop

# File to read (replace 'file.txt' with your filename)
file="file.txt"

# Check if the file exists
if [ -f "$file" ]
then
    # Read the file line by line
    while IFS= read -r line
    do
        echo "Line: $line"
    done < "$file"
else
    echo "File $file does not exist."
fi
```




## Using Until Loops and Double Brackets

```sh
# Until Loop

count=1

# Loop until the count is greater than 5
until [ $count -gt 5 ]
do
    echo "Count: $count"
    count=$((count + 1))  # Increment the count
done

# Until Loop with Sleep

counter=1

# Loop with sleep and a condition
until [ $counter -gt 5 ]
do
    echo "Counter: $counter"
    sleep 2  # Pause for 2 seconds
    counter=$((counter + 1))  # Increment the counter
done

# if you use these doube round brackets like until (($n > 10)) inside of any loop you cant use keywords 
# like -ge , -gt, -eq you must use symbols like this >, <
```





## Using For Loops and For-In Loops

```sh
# Basic For Loop

# Loop from 1 to 5
for i in {1..5}
do
    echo "Number: $i"
done

# For Loop with a List of Values

# Define a list of values
for fruit in apple banana cherry
do
    echo "Fruit: $fruit"
done

# For Loop with C-style Syntax

# Initialize variables
for ((i = 1; i <= 5; i++))
do
    echo "Number: $i"
done

# For-In Loop with Array

# Define an array
colors=("red" "green" "blue")

# Loop through the array elements
for color in "${colors[@]}"
do
    echo "Color: $color"
done

# For-In Loop with Command Output

# Loop through the output of a command
for file in $(ls)
do
    echo "File: $file"
done

# loop through the output of multiple command
for command in ls pwd date
do
    echo "$command"
done

# loop over every file & directory in the current directory
for item in * 
do
    if [ -d $item ]
    then
        echo $item
    fi
done
# * means everything in the current directory

# For Loop with Sequence

# Loop with a sequence of numbers
for num in $(seq 1 5)
do
    echo "Number: $num"
done
```


## Using Select Loop

```sh
#!/bin/bash

# Define a list of options
PS3="Please select an option: "  # Prompt for user input

select option in "Option 1" "Option 2" "Option 3" "Quit"
do
    case $option in
        "Option 1")
            echo "You selected Option 1"
            ;;
        "Option 2")
            echo "You selected Option 2"
            ;;
        "Option 3")
            echo "You selected Option 3"
            ;;
        "Quit")
            echo "Exiting..."
            break  # Exit the select loop
            ;;
        *)
            echo "Invalid option. Please try again."
            ;;
    esac
done

# The select loop in shell scripting is used to create a simple menu-driven interface. It allows users to select from a list of options,
# which is especially useful  for interactive scripts.
```







## Using Break and Continue Statements

```sh
#!/bin/bash

# Break Statement in a For Loop

for i in {1..10}
do
    if [[ $i -eq 5 ]]
    then
        break  # Exit the loop when i equals 5
    fi
    echo "Number: $i"
done

echo "Exited the loop after encountering break at i = 5"

# Continue Statement in a For Loop

for j in {1..10}
do
    if [[ $j -eq 5 ]]
    then
        continue  # Skip the rest of the loop when j equals 5
    fi
    echo "Number: $j"
done

echo "Skipped printing the number 5 using continue"

# Break Statement in a While Loop

count=1

while [[ $count -le 10 ]]
do
    if (( count == 5 ))
    then
        break  # Exit the loop when count equals 5
    fi
    echo "Count: $count"
    ((count++))  # Increment count
done

echo "Exited the while loop after encountering break at count = 5"

# Continue Statement in a While Loop

counter=1

while [[ $counter -le 10 ]]
do
    if (( counter == 5 ))
    then
        ((counter++))  # Increment counter to avoid infinite loop
        continue  # Skip the rest of the loop when counter equals 5
    fi
    echo "Counter: $counter"
    ((counter++))  # Increment counter
done

echo "Skipped printing the number 5 using continue in while loop"
```



## Using Functions, Functions with Arguments, and Local Variables

```sh
#!/bin/bash

# Defining a simple function

function greet() {
    echo "Hello, World!"
}

# Calling the function
greet

# Defining a function with arguments

function greet_person() {
    echo "Hello, $1! You are $2 years old."
}

# Calling the function with arguments
greet_person "John" 25

# Defining a function with local variables

function add_numbers() {
    local num1=$1  # Local variable num1
    local num2=$2  # Local variable num2
    local sum=$((num1 + num2))  # Local variable sum
    echo "The sum of $num1 and $num2 is: $sum"
}


# defining a function
# every variables even if it is inside the function it will be global variable
# the variable name is available to global access only when the print function is called 
# to escape this feature we use local varialbe
function print() {
    name=dante
}
print
echo $name

# Calling the function with arguments
add_numbers 5 10

# Demonstrating scope of local variables

function show_scope() {
    local var="I am local"  # Local variable var
    echo "Inside function: $var"
}

# Calling the function to see the local variable
show_scope

# Trying to access the local variable outside the function
echo "Outside function: $var"  # This will not print the local variable value
```




## Using readonly Command and Creating Constants

```sh
#!/bin/bash

# Defining a variable
var="I am a variable"
echo "Before making readonly: $var"

# Making the variable readonly
readonly var

# Attempting to change the readonly variable (this will cause an error)
var="Attempt to change"
echo "After attempting to change: $var"

# Defining a constant
readonly PI=3.14159

# Using the constant
echo "The value of PI is: $PI"

# Attempting to change the constant (this will cause an error)
PI=3.14
echo "After attempting to change: $PI"

# Using a readonly function
function myFunction() {
    echo "This is a readonly function"
}

# Making the function readonly
readonly -f myFunction

# Attempting to redefine the readonly function (this will cause an error)
function myFunction() {
    echo "Attempt to change"
}

# Calling the function to see if it changed
myFunction
```

## Signals, Traps, PID, Kill Command, and man 7 signal

```sh
#!/bin/bash

# Signals and Traps

# Define a function to handle the signal
function handle_signal() {
    echo "Signal caught: $1"
    exit 0
}

# The trap command allows you to execute a command when a signal is received. In the example, the error_handler function 
# is defined to handle errors. The trap 'error_handler $LINENO' ERR command sets this function to be called whenever an error occurs (ERR signal).
# The function prints an error message and exits the script.

# Trap the SIGINT (Ctrl+C) signal
trap 'handle_signal SIGINT' SIGINT

# Trap the SIGTERM signal
trap 'handle_signal SIGTERM' SIGTERM

# Simulate a long-running process
echo "Running... (Press Ctrl+C to interrupt)"
while true
do
    sleep 1
done

# Getting the PID of the Current Script

# Get the current script's PID
current_pid=$$
echo "The PID of this script is: $current_pid"

# Kill Command

# Simulate a background process
sleep 300 &
background_pid=$!
echo "Started background process with PID: $background_pid"

# Killing the background process
kill $background_pid
echo "Background process with PID $background_pid has been killed"

# Man 7 Signal

# To see detailed information about signals, run the following command in your terminal:
# man 7 signal
# This will display the manual page for signals, including a list of standard signals and their descriptions.
```



## Error Handling in Shell Scripting

```sh
#!/bin/bash

# Error Handling Basics

# Using exit status
# Every command in a shell script returns an exit status.
# A status of 0 means the command was successful.
# A non-zero status indicates an error.

# Example: Checking the exit status of a command
ls /nonexistent_directory
if [ $? -ne 0 ]; then
    echo "Error: Directory does not exist"
fi

# Setting an error handler with trap
# The trap command allows you to execute a command when a signal is received.
# trap 'commands' SIGNAL

# Define an error handler function
function error_handler() {
    echo "An error occurred on line $1"
    exit 1
}

# Trap errors (ERR signal)
trap 'error_handler $LINENO' ERR

# Demonstrating error handling with a failing command
echo "Trying to list a nonexistent directory..."
ls /nonexistent_directory

# Using set -e
# The set -e command causes the script to exit immediately if a command returns a non-zero status.
# This is useful for stopping the script when an error occurs.

set -e

# Demonstrating set -e with a failing command
echo "Trying to list a nonexistent directory with set -e..."
ls /nonexistent_directory

# This line will not be executed because set -e causes the script to exit on the previous error
echo "This will not be printed"
```

## Debugging with set -x and set +x

```sh
#!/bin/bash

# Enable debugging
set -x
# The set -x command enables debugging mode. When debugging mode is enabled,
# each command and its arguments are printed to the terminal as they are executed. This is useful
# for understanding the flow of the script and identifying where errors occur.
# Sample script with debugging enabled
echo "Debugging is enabled"
a=5
b=10
result=$((a + b))
echo "The result of a + b is: $result"

# Disable debugging
set +x

# Sample script with debugging disabled
echo "Debugging is disabled"
c=15
d=20
result=$((c + d))
echo "The result of c + d is: $result"
```

## Using the `export` Command

```sh
#!/bin/bash

# Setting a local variable
my_var="Hello, World!"

# Displaying the local variable
echo "Local variable my_var: $my_var"

# Exporting the variable to make it an environment variable
export my_var

# Running a child process to show that it inherits the environment variable
bash -c 'echo "Environment variable my_var in child process: $my_var"'

# Unsetting the environment variable
unset my_var

# Running another child process to show that the variable is no longer available
bash -c 'echo "Environment variable my_var after unset: $my_var"'
```


## Using the `alias` Command

```sh
#!/bin/bash

# The alias command in shell scripting is used to create shortcuts for longer commands or command sequences.

# Creating Aliases

# Create an alias for the 'ls -la' command
alias ll='ls -la'

# Create an alias for a command with arguments
alias greet='echo "Hello, World!"'

# Create an alias for a long command sequence
alias update='sudo apt update && sudo apt upgrade'

# Display all currently defined aliases
alias

# Example usage of the defined aliases

# Using the 'll' alias to list files in long format with details
ll

# Using the 'greet' alias to print a greeting message
greet

# Using the 'update' alias to update and upgrade the system packages
update

# Unaliasing a command to remove the alias
unalias ll

# Trying to use the removed alias 'll'
ll
```



## Using `awk` for Text Processing

```sh
#!/bin/bash

# Sample data file (data.txt):
# name,age,location
# Alice,30,New York
# Bob,25,Los Angeles
# Carol,35,Chicago

# 1. Print the entire file
echo "1. Print the entire file:"
awk '{print}' data.txt  # Prints every line of the file

# 2. Print the first column
echo -e "\n2. Print the first column (name):"
awk -F, '{print $1}' data.txt  # Uses comma as field separator and prints the first column

# 3. Print the second column
echo -e "\n3. Print the second column (age):"
awk -F, '{print $2}' data.txt  # Uses comma as field separator and prints the second column

# 4. Print the name and age columns
echo -e "\n4. Print the name and age columns:"
awk -F, '{print $1, $2}' data.txt  # Uses comma as field separator and prints the name and age columns

# 5. Print lines where age is greater than 30
echo -e "\n5. Print lines where age is greater than 30:"
awk -F, '$2 > 30' data.txt  # Filters and prints lines where the value in the second column (age) is greater than 30

# 6. Print lines with custom delimiter (tab)
echo -e "\n6. Print lines with custom delimiter (tab):"
awk -F'\t' '{print $1, $2}' data.txt  # Uses tab as field separator and prints the first two columns

# 7. Use awk to calculate the sum of ages
echo -e "\n7. Calculate the sum of ages:"
awk -F, '{sum += $2} END {print "Sum of ages:", sum}' data.txt  # Calculates the sum of values in the second column and prints the result

# 8. Format output with header
echo -e "\n8. Format output with header:"
awk -F, 'BEGIN {print "Name\tAge\tLocation"} {print $1 "\t" $2 "\t" $3}' data.txt  # Prints a custom header and formats the output with tabs between columns

# 9. Print only lines that match a pattern
echo -e "\n9. Print lines where location is 'Chicago':"
awk -F, '$3 == "Chicago"' data.txt  # Filters and prints lines where the value in the third column (location) is "Chicago"
```


## Using `sed` for Text Processing

```sh
#!/bin/bash

# Sample data file (data.txt):
# Hello world
# This is a test
# Welcome to the sed tutorial
# Goodbye world

# 1. Print the contents of the file
echo "1. Print the contents of the file:"
sed '' data.txt  # Prints the entire file (no changes)

# 2. Substitute text
echo -e "\n2. Substitute 'world' with 'universe':"
sed 's/world/universe/' data.txt  # Replaces the first occurrence of 'world' with 'universe' in each line

# 3. Substitute text globally (all occurrences)
echo -e "\n3. Substitute 'world' with 'universe' globally:"
sed 's/world/universe/g' data.txt  # Replaces all occurrences of 'world' with 'universe' in each line

# 4. Delete lines containing a pattern
echo -e "\n4. Delete lines containing 'test':"
sed '/test/d' data.txt  # Deletes all lines that contain the pattern 'test'

# 5. Print specific lines (e.g., line 2)
echo -e "\n5. Print line 2:"
sed -n '2p' data.txt  # Prints only the second line of the file

# 6. Insert text before a specific line
echo -e "\n6. Insert text before line 2:"
sed '2i\This is a new line' data.txt  # Inserts 'This is a new line' before line 2

# 7. Append text after a specific line
echo -e "\n7. Append text after line 2:"
sed '2a\This is an appended line' data.txt  # Appends 'This is an appended line' after line 2

# 8. Replace text only on a specific line
echo -e "\n8. Replace 'Goodbye' with 'Farewell' only on line 4:"
sed '4s/Goodbye/Farewell/' data.txt  # Replaces 'Goodbye' with 'Farewell' only on line 4

# 9. Use a different delimiter for substitution
echo -e "\n9. Use a different delimiter for substitution:"
sed 's|world|universe|' data.txt  # Uses '|' as the delimiter instead of '/' for the substitution

# 10. Delete a specific range of lines (e.g., lines 2 to 4)
echo -e "\n10. Delete lines 2 to 4:"
sed '2,4d' data.txt  # Deletes lines 2 through 4

# 11. Replace multiple patterns
echo -e "\n11. Replace 'Hello' with 'Hi' and 'Goodbye' with 'Farewell':"
sed -e 's/Hello/Hi/' -e 's/Goodbye/Farewell/' data.txt  # Performs multiple substitutions
```


## Using `awk` and `sed` Together for Text Processing

```sh
#!/bin/bash

# Sample data file (data.txt):
# Alice,30,New York
# Bob,25,Los Angeles
# Carol,35,Chicago

# 1. Use `awk` to print the second column, then use `sed` to replace '30' with '31'
echo "1. Use awk to print the second column, then sed to replace '30' with '31':"
awk -F, '{print $2}' data.txt | sed 's/30/31/'  # Extracts the second column (age) using `awk` and then replaces '30' with '31' using `sed`

# 2. Use `sed` to replace 'Los Angeles' with 'LA' and then use `awk` to print the third column
echo -e "\n2. Use sed to replace 'Los Angeles' with 'LA', then awk to print the third column:"
sed 's/Los Angeles/LA/' data.txt | awk -F, '{print $3}'  # Replaces 'Los Angeles' with 'LA' using `sed` and then prints the third column (location) using `awk`

# 3. Use `awk` to print lines where age is greater than 30, then use `sed` to remove the age column
echo -e "\n3. Use awk to filter lines where age > 30, then sed to remove the age column:"
awk -F, '$2 > 30' data.txt | sed 's/,[0-9]*//'  # Filters lines with age > 30 using `awk` and then removes the age column using `sed`

# 4. Use `sed` to add a header line, then use `awk` to print the first column with the header
echo -e "\n4. Use sed to add a header line, then awk to print the first column:"
sed '1i\Name,Age,Location' data.txt | awk -F, '{print $1}'  # Adds a header line to the file using `sed` and then prints the first column (name) using `awk`

# 5. Use `awk` to print lines with 'New York', then use `sed` to replace 'New York' with 'NY'
echo -e "\n5. Use awk to filter lines with 'New York', then sed to replace 'New York' with 'NY':"
awk -F, '$3 == "New York"' data.txt | sed 's/New York/NY/'  # Filters lines with 'New York' using `awk` and then replaces 'New York' with 'NY' using `sed`

# 6. Use `awk` to add line numbers, then use `sed` to remove lines with numbers greater than 1
echo -e "\n6. Use awk to add line numbers, then sed to remove lines with numbers greater than 1:"
awk '{print NR ": " $0}' data.txt | sed '/^[2-9][0-9]*: /d'  # Adds line numbers using `awk` and then removes lines with numbers greater than 1 using `sed`

# 7. Use `sed` to remove all commas, then use `awk` to print lines with 'Alice'
echo -e "\n7. Use sed to remove all commas, then awk to print lines with 'Alice':"
sed 's/,/ /g' data.txt | awk '/Alice/'  # Replaces all commas with spaces using `sed` and then prints lines containing 'Alice' using `awk`

# 8. Use `awk` to extract names, then use `sed` to convert all names to uppercase
echo -e "\n8. Use awk to extract names, then sed to convert names to uppercase:"
awk -F, '{print $1}' data.txt | sed 's/.*/\U&/'  # Extracts names using `awk` and then converts names to uppercase using `sed`
```


## PENDING
getting --arguments just like fabric 2.0 scripts     
retry mechanism (until a function fails 3 times we will execute it back to back)    