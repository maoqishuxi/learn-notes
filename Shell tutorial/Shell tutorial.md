# Shell tutorial

Example:

```
#!/bin/bash
#!/bin/csh
```

## Variables

A variable can contain a number, a character or a string of characters. Variable name is case sensitive and can consist of a combination of letters and the underscore “_”. Value assignment is done using the “=” sign. Note that no space permitted no either side of = sign when initializing variables.

```
PRICE_PER_APPLE=5
MyFirstLetters=ABC
greeting='Hello        world!'
```

A backslash “\” is used to escape special character meaning

```
PRICE_PER_APPLE=5
echo "The price of an Apple today is: \$HK $PRICE_PER_APPLE"
```

Encapsulating the variable name with ${} is used to avoid ambiguity

```
MyFirstLetters=ABC
echo "The first 10 letters in the alphabet are: ${MyFirstLetters}DEFGHIJ"
```

Encapsulating the variable name with “” will preserve any white space values

```
greeting='Hello        world!'
echo $greeting" now with spaces: $greeting"
```

Variables can be assigned with the value of a command output. This is referred to as substitution. Substitution can be done by encapsulating the command with `` (known as back-ticks) or with $()

```
FILELIST=`ls`
FileWithTimeStamp=/tmp/my-dir/file_$(/bin/date +%Y-%m-%d).txt
```

## Arrays

An array can hold several values under one name. Array naming is the same as variables naming. An array is initialized by assign space-delimited values enclosed in ()

```
my_array=(apple banana "Fruit Basket" orange)
new_array[2]=apricot
```

The total number of elements in the array is referenced by ${#arrayname[@]}

```
my_array=(apple banana "Fruit Basket" orange)
echo  ${#my_array[@]}                   # 4
```

The array elements can be accessed with their numeric index. The index of the first element is 0.

```
my_array=(apple banana "Fruit Basket" orange)
echo ${my_array[3]}                     # orange - note that curly brackets are needed
# adding another array element
my_array[4]="carrot"                    # value assignment without a $ and curly brackets
echo ${#my_array[@]}                    # 5
echo ${my_array[${#my_array[@]}-1]}     # carrot
```

## Basic Operators

Simple `arithmetics` on variables can be done using the arithmetic expression: $((expression))

```
A=3
B=$((100 * $A + 5)) # 305
```

The basic operators are:

**a + b** addition (a plus b)

**a - b** `substraction` (a minus b)

**a * b** multiplication (a times b)

**a / b** division (integer) (a divided by b)

**a % b** modulo (the integer remainder of a divided by b) 

**a \** b** exponentiation (a to the power of b) 

## Basic String Operations

String Length

```
#       1234567890123456
STRING="this is a string"
echo ${#STRING}            # 16
```

Index

Find the numerical position in $STRING of any single character in $SUBSTRING that matches. Note that the ‘expr’ command is used in this case.

```
STRING="this is a string"
SUBSTRING="hat"
expr index "$STRING" "$SUBSTRING"     # 1 is the position of the first 't' in $STRING
```

Substring Extraction

Extract substring of length $LEN from $STRING starting after position $POS. Note that first position is 0.

```
STRING="this is a string"
POS=1
LEN=3
echo ${STRING:$POS:$LEN}   # his
```

If :$LEN is omitted, extract substring from $POS to end of line

```
STRING="this is a string"
echo ${STRING:1}           # $STRING contents without leading character
echo ${STRING:12}          # ring
```

Simple data extraction example:

```
# Code to extract the First name from the data record
DATARECORD="last=Clifford,first=Johnny Boy,state=CA"
COMMA1=`expr index "$DATARECORD" ','`  # 14 position of first comma
CHOP1FIELD=${DATARECORD:$COMMA1}       #
COMMA2=`expr index "$CHOP1FIELD" ','`
LENGTH=`expr $COMMA2 - 6 - 1`
FIRSTNAME=${CHOP1FIELD:6:$LENGTH}      # Johnny Boy
echo $FIRSTNAME
```

Substring Replacement

```
STRING="to be or not to be"
```

Replace first occurrence of substring with replacement

```
STRING="to be or not to be"
echo ${STRING[@]/be/eat}        # to eat or not to be
```

Replace all occurrences of substring

```
STRING="to be or not to be"
echo ${STRING[@]//be/eat}        # to eat or not to eat
```

Delete all occurrences of substring (replace with empty string)

```
STRING="to be or not to be"
echo ${STRING[@]// not/}        # to be or to be
```

Replace occurrence of substring if at the beginning of $STRIGN

```
STRING="to be or not to be"
echo ${STRING[@]/#to be/eat now}    # eat now or not to be
```

Replace occurrence of substring if at the end of $STRING

```
STRING="to be or not to be"
echo ${STRING[@]/%be/eat}        # to be or not to eat
```

replace occurrence of substring with shell command output

```
STRING="to be or not to be"
echo ${STRING[@]/%be/be on $(date +%Y-%m-%d)}    # to be or not to be on 2012-06-14
```

## Decision Making

The basic conditional decision making construct is:

**if [expression]; then**

code if ‘expression’ is true

**fi**

```
NAME="John"
if [ "$NAME" = "John" ]; then
	echo "True - my name is indeed John"
fi
```

it can be expanded with ‘else’

```
NAME="Bill"
if [ "$NAME" = "John" ]; then
	echo "True - my name is indeed John"
else
	echo "False"
	echo "You must mistaken me for $NAME"
fi
```

it can be expanded with ‘`elif`’ (else-if)

```
NAME="George"
if [ "$NAME" = "John" ]; then
	echo "John Lennon"
elif [ "$NAME" = "George" ]; then
	echo "George Harrison"
else
	echo "This leaves us with Paul and Ringo"
fi
```

Types of numeric comparisons

```
comparison    Evaluated to true when
$a -lt $b    $a < $b
$a -gt $b    $a > $b
$a -le $b    $a <= $b
$a -ge $b    $a >= $b
$a -eq $b    $a is equal to $b
$a -ne $b    $a is not equal to $b
```

Types of string comparisons

```
comparison    Evaluated to true when
"$a" = "$b"     $a is the same as $b
"$a" == "$b"    $a is the same as $b
"$a" != "$b"    $a is different from $b
-z "$a"         $a is empty
```

- note1: whitespace around = is required
- note2: use “” around string variables to avoid shell expansion of special characters as *

Logical combinations

```
if [[ $VAR_A[0] -eq 1 && ($VAR_B = "bee" || $VAR_T = "tee" ]]; then
	command...
fi
```

case structure

```
case "$variable" in
    "$condition1" )
        command...
    ;;
    "$condition2" )
        command...
    ;;
esac
```

simple case bash structure

```
mycase=1
case $mycase in
    1) echo "You selected bash";;
    2) echo "You selected perl";;
    3) echo "You selected phyton";;
    4) echo "You selected c++";;
    5) exit
esac
```

## Loops

bash for loop

```
# basic construct
for arg in [list]
do
 command(s)...
done
```

For each pass through the loop, `arg` takes on the value of each successive value in the list. Then the command(s) are executed.

```
# loop on array member
NAMES=(Joe Jenny Sara Tony)
for N in ${NAMES[@]} ; do
	echo "My name is $N"
done

#loop on command output results
for f in $( ls prog.sh /etc/localtime ) ; do
	echo "File is: $f"
done
```

 bash while loop

```
# basic construct
while [ condition ]
do
	command(s)...
done
```

The while construct tests for a condition, and if true, executes commands. It keeps looping as long as the  condition is true.

```
COUNT=4
while [ $COUNT -gt 0 ]; do
	echo "Value of count is: $COUNT"
	COUNT=$(($COUNT - 1))
done
```

bash until loop

```
# basic construct
until [ condition ]
do
	command(s)...
done
```

The until construct tests for a condition, and if false, executes commands. It keeps looping as the condition is false (opposite of while construct)

```
COUNT=1
until [ $COUNT -gt 5 ]; do
	echo "Value of count is: $COUNT"
	COUNT=$(($COUNT + 1))
done
```

“break” and “continue” statements

break and continue can be used to control the loop execution of for, while and until constructs. continue is used to skip the rest of a particular loop iteration, whereas break is used to skip the entire rest of loop. A few examples:

```
# Prints out 0,1,2,3,4

COUNT=0
while [ $COUNT -ge 0 ]; do
	echo "Value of COUNT is: $COUNT"
	COUNT=$((COUNT+1))
	if [ $COUNT -ge 5] ; then
		break
	fi
done

# Prints out only odd numbers - 1,3,5,7,9
COUNT=0
while [ $COUNT -lt 10 ]; do
	COUNT=$((COUNT+1))
	# Check if COUNT is even
	if [ $(($COUNT % 2)) = 0 ]; then
		continue
	fi
	echo $COUNT
done
```

## Array-Comparison

```
# basic construct
# array=(value1 value2 ... valueN)
array=(23 45 34 1 2 3)
#To refer to a particular value (e.g. : to refer 3rd value)
echo ${array[2]}

#To refer to all the array values
echo ${array[@]}

#To evaluate the number of elements in an array
echo ${#array[@]}
```

## Shell Functions

```
# basic construct
function function_name {
	command...
}
```

```
function function_B {
	echo "Function B."
}
function function_A {
	echo "$1"
}

# FUNCTION CALLS
# Pass parameter to function A
function_A "Function A."     # Function A.
function_B                   # Function B.
# Pass two parameters to function adder
adder 12 56                  # 68
```



