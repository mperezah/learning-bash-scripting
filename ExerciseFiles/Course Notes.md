# Course Notes for _Learning Bash Scripting_

This document contains links and code snippets for the LinkedIn Learning course _Learning Bash Scripting_ with Scott Simpson.

## Links and other information

### GitHub repository for this course

Download the files used in this course from [github.com/scottsimpson/learning-bash-scripting](https://github.com/scottsimpson/learning-bash-scripting).

### Related LinkedIn Learning courses

[Learning VirtualBox](https://www.linkedin.com/learning/learning-virtualbox-19862434)

[Learning Linux Command Line](https://www.linkedin.com/learning/learning-linux-command-line-14447912)

[Learning Windows Subsystem for Linux](https://www.linkedin.com/learning/learning-windows-subsystem-for-linux-16134127)

### GitHub Codespaces information

Learn more about [GitHub Codespaces](https://github.com/features/codespaces).

Learn more about [billing and usage](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-codespaces/about-billing-for-github-codespaces) for GitHub Codespaces.

## Commands used in the course

### 01_01 What's Bash?

```bash
bash --version
echo $SHELL
# change your user's default shell with chsh
echo $PATH
env
```
$PWD current working directory, $OLDPW previous working directory, $USER user accont name is stored in

### 01_02 Pipes and redirections

```bash
cat lorem.txt
clear
cat lorem.txt | less
cat lorem.txt | wc
ls > list.txt
cat list.txt
ls >> list.txt
clear
ls /notreal
ls /notreal 1> output.txt 2>error.txt
cat output.txt
cat error.txt
cat < list.txt
clear
cat << EndOfText
This is a
multiline
text string
EndOfText
```
Pipes (|) send output of one process to another. 
cat looks at content of a file. 
with | less breaks the text into pages, q to exit. 
| wc for lines, words, and characters in order.  
Streams: 0 is standard input, 1 is standard output, 2 is standard error. 
ls lists content of directory. 
The > is output redirection (truncate). >> is output redirection (append). 1 is assumed when working with redirection, but can use 2> for explicitly asking for error output. < is input redirection take information in file, making it seems like it was taken as the input. << is a here document. Using this after the << is the limit string, good for instructions. Adding - after << removes leading tabs.

### 01_03 Bash builtins and other commands

```bash
echo some text
printf hello
command echo hello
builtin echo hello
command -V echo
enable -n echo
command -V echo
enable -n
enable echo
help echo
help -d
```
echo outputs text, ends line with new line character. printf does not add new line character. 
command version vs builtin, just for explicitly saying. command -V followed by the name of the program will show whether it is builtin or command, but builtins will take precedent over commands. 
enable -n followed by command disables the builtin command. enable -n by itself shows disabled builtins.

### 01_05 Bash expansions and substitutions

```bash
echo ~
whoami
cd Solutions
cd
echo ~-
```
~ represents value of user's $HOME environment. command whoami show's user's name. 
cd followed by a directory is change directory to that directory. cd by itself goes back to home directory.
echo ~- shows oldworking directory.

### 01_06 Brace expansion

```bash
echo {1..10}
echo {10..1}
echo {01..10}
echo {01..100}
echo {a..z}
echo {Z..A}
echo {1..30..3}
echo {a..z..2}
touch file_{01..12}{a..d}
ls
clear
echo {cat,dog,fox}
echo {cat,dog,fox}_{1..5}
head -n1 {dir1,dir2,dir3}/lorem.txt
```
{} brace expansion creates sets or ranges. .. helps create sequential items, such as in {1..10} in order. {10..1} reverse.
Can pad with 0's as you'd like.
Can do with intervals by 3 in {1..30..3}, and can be done with letters as well.
{cat,dog,fox}_{1..5} can be useful for making directories. Will cycle 1-5 for the first string, then second etc.
head -n1 shows one line only, running lorem.txt in each of dir1, dir2, dir3

### 01_07 Parameter expansion

```bash
greeting="hello there!"
echo $greeting
echo ${greeting:6}
echo ${greeting:6:3}
echo ${greeting/there/everybody}
echo $greeting
echo ${greeting//e/_}
echo ${greeting/e/_}
echo $greeting:4:3
```
NOTE NO SPACE when assigning values to parameters. Parameter expansion manipulates the value
echo ${greeting:6:3} starting at ther 6th value, then 3 more values from there
echo ${greeting/there/everybody} replace there with everybody. / only replaces the first instance, while // replaces every instance such as with all e with _.

### 01_08 Command substitution

```bash
uname -r
echo "The kernel is $(uname -r)."
echo "The Python version is $(python3 -V)."
echo "Result: $(python3 -c 'print("Hello from Python!")' | tr [a-z] [A-Z])"
```
$(...) command substitution. Old way as `...`. uname -r for release version of the kernel. Command substitution can be used to add into text string.
So in "Result..." command substitution uses python to print the statement using python. This output is then piped (|) to tr to translate all lowercase ([a-z]) to uppercase ([A-Z]).

### 01_09 Arithmetic expansion

```bash
echo $(( 2 + 2 ))
echo $((4-2))
echo $(( 4*5 ))
echo $(( 4 / 5 ))
```
$[] is deprecated version. Use spaces in general so you don't have to remember when there are space sensitive cases. 
4 / 5 is 0.8, but here it is 0, since it can only do arithmetic with integers. 

### 02_01 Understanding Bash script syntax

```bash
nano myscript.sh

#!/usr/bin/env bash

# Tell the user hello!
echo Hello!

bash myscript.sh
chmod +x myscript.sh

./myscript.sh
```
Oneliners are useful for commands that are often used. Long commands can be split across multiple lines with line continuation characters (\).
Combining multiple lines is easier with bash scripts. A txt file with a series of commands.
Run with bash myscript.sh.
More common, make an executable bash script. Includes a shebang as the first line (#!/usr/bin/env bash).
Make executable with chmod +x myscript.

nano myscript.sh makes script. Here you would add the shebang #!/usr/bin/env bash, then whatever afterwards. Can make comments with #.
Ctrl + O saves the file, Ctrl + X leaves the script editor. Then can run with bash myscript.sh.
Make it executable with chmod +x(makes executible) myscript.sh(which script).
Now can run with ./(then the script, in this case)myscript.sh.
The script runs in a non-interactive shell, so not affected by other changes.

### 02_02 Displaying text with 'echo'

```bash
echo hello
echo hello world
worldsize=big
echo hello $worldsize world
echo "The kernel is $(uname -r)"
echo The kernel is $(uname -r)
echo The (kernel) is $(uname -r)
echo The \(kernel\) is $(uname -r)
echo 'The kernel is $(uname -r)'
echo "The (kernel) is $(uname -r)"
echo "The (kernel) is \$(uname -r)"
clear
echo
echo; echo "More space!"; echo
echo -n "No newline!"
echo -n "Part of"; echo -n " a statement"
echo -n "Part of"; echo " a statement"
```

### 02_03 Working with variables

```bash
mygreeting=Hello
mygreeting = Hello
mygreeting2="Good morning"
number=16
echo $mygreeting
echo $mygreeting2
echo $number
```

```bash
nano myscript.sh

#!/usr/bin/env bash

myvar="Hello!"
echo "The value of the myvar variable is: $myvar"
myvar="Bonjour!"
echo "The value of the myvar variable is: $myvar"

declare -r myname="Scott"
echo "The value of the myname variable is: $myname"
myname="Michael"
echo "The value of the myname variable is: $myname"

declare -l lowerstring="This is some TEXT!"
echo "The value of the lowerstring variable is: $lowerstring"
lowerstring="Let's CHANGE the VALUE!"
echo "The value of the lowerstring variable is: $lowerstring"

declare -u upperstring="This is some TEXT!"
echo "The value of the upperstring variable is: $upperstring"
upperstring="Let's CHANGE the VALUE!"
echo "The value of the upperstring variable is: $upperstring"

./myscript.sh
```

```bash
declare -p
echo $USER
```

### 02_04 Working with numbers

```bash
echo $(( 4 + 4 ))
echo $(( 8 - 5 ))
echo $(( 2 * 3 ))
echo $(( 8 / 4 ))
echo $(( (3+6) - 5 * (5*2) ))
a=3
((a+=3))
echo $a
((a++))
echo $a
((a++))
echo $a
((a--))
echo $a
(($a++))
clear
((a+=3))
((a-=3))
((a*=2))
((a/=2))
echo $a
a=$a+2
echo $a
declare -i b=3
b=$b+4
echo $b
echo $(( 1 / 3 ))
echo "1/3" | bc
echo "scale=3; 1/3" | bc
declare -i c=1
declare -i d=3
e=$(echo "scale=3; $c/$d" | bc)
echo $e
echo $RANDOM
echo $RANDOM
echo $RANDOM
echo $RANDOM
echo $RANDOM
echo $(( RANDOM % 10 ))
echo $(( 1 + RANDOM % 10 ))
echo $(( RANDOM % 2 ))
```

### 02_05 Comparing values with test

```bash
help test | less
[ -d ~ ]
echo $?
[ -d /bin/bash ]; echo $?
[ -d /bin ]; echo $?
[ "cat" = "dog" ]; echo $?
[ "cat" = "cat" ]; echo $?
pet=cat
[ $pet = "cat" ]; echo $?
pet=dog
[ $pet = "cat" ]; echo $?
clear
[ 4 -lt 5 ]; echo $?
[ 4 -lt 3 ]; echo $?
[ ! 4 -lt 3 ]; echo $?
(( 4 > 3 )); echo $?
[ $pet = "dog" -a -d ~ ]; echo $?
[ $pet = "dog" -o -d ~ ]; echo $?
[ $pet = "cat" -o -d ~ ]; echo $?
```

### 02_06 Comparing values with extended test

```bash
[[ 4 -lt 3 ]]; echo $?
[[ -d ~ && -a /bin/bash ]]; echo $?
[[ -d ~ && -a /bin/mash ]]; echo $?
[[ -d ~ || -a /bin/bash ]]; echo $?
[[ -d ~ ]] && echo ~ is a directory
[[ -d /bin/bash ]] && echo /bin/bash is a directory
ls && echo "listed the directory"
clear
true && echo "success!"
false && echo "success!"
[[ "cat" =~ c.* ]]; echo $?
[[ "bat" =~ c.* ]]; echo $?
```

### 02_07 Formatting and styling text output

```bash
echo -e "Name\t\tNumber"; echo -e "Scott\t\t123"
echo -e "This text\nbreaks over\nthree lines"
echo -e "\a"
```

```bash
nano myscript.sh

#!/usr/bin/env bash
echo -e "\033[33;44mColor Text\033[0m"
echo -e "\033[30;42mColor Text\033[0m"
echo -e "\033[41;105mColor Text"
echo "some text that shouldn't have styling"
echo -e "\033[0m"
echo "some text that shouldn't have styling"
echo -e "\033[4;31;40mERROR:\033[0m\033[31;40m Something went wrong.\033[0m"

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

ulinered="\033[4;31;40m"
red="\033[31;40m"
none="\033[0m"

echo -e $ulinered"ERROR:"$none$red" Something went wrong."$none

./myscript.sh
```

### 02_08 Formatting output with printf

```bash
echo "The results are: $(( 2 + 2 )) and $(( 3 / 1 ))"
printf "The results are: %d and %d\n" $(( 2 + 2 )) $(( 3 / 1 ))

printf "The results is: %d\n" $(( 2 + 2 )) $(( 3 / 1 ))
```

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "----10----| --5--"

echo "Right-aligned text and digits"
printf "%10s: %5d\n" "A Label" 123 "B Label" 456

echo "Left-aligned text, right-aligned digits"
printf "%-10s: %5d\n" "A Label" 123 "B Label" 456

echo "Left-aligned text and digits"
printf "%-10s: %-5d\n" "A Label" 123 "B Label" 456

echo "Left-aligned text, right-aligned and padded digits"
printf "%-10s: %05d\n" "A Label" 123 "B Label" 456

echo "----10----| --5--"

./myscript.sh
```

```bash
nano myscript.sh

printf "%(%Y-%m-%d %H:%M:%S)T\n" 1746020520
date +%s
date +%Y-%m-%d\ %H:%M:%S
printf "%(%Y-%m-%d %H:%M:%S)T\n" $(date +%s)
printf "%(%Y-%m-%d %H:%M:%S)T\n"
printf "%(%Y-%m-%d %H:%M:%S)T is %s\n" -1 "the time"

./myscript.sh
```

### 02_09 Working with arrays

```bash
snacks=("apple" "banana" "orange")
declare -a snacks=("apple" "banana" "orange")
echo ${snacks[2]}
echo ${snacks[1]}
echo ${snacks[0]}
snacks[5]="grapes"
snacks+=("mango")
echo ${snacks[@]}
for i in {0..6}; do echo "$i: ${snacks[$i]}"; done
echo ${snacks[@]: -1}
declare -A office
office[city]="San Francisco"
office["building name"]="HQ West"
echo ${office["building name"]} is in ${office[city]}
```

### 03_01 Conditional statements with the 'if' keyword

```bash
nano myscript.sh

#!/usr/bin/env bash

declare -i a=3

if [[ $a -gt 4 ]]
then
    echo "$a is greater than 4!"
else
    echo "$a is not greater than 4!"
fi

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

declare -i a=3

if (( $a > 4 ))
then
    echo "$a is greater than 4!"
else
    echo "$a is not greater than 4!"
fi

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

declare -i a=3

if (( $a > 4 ))
then
    echo "$a is greater than 4!"
elif (( $a > 2 ))
then
    echo "$a is greater than 2"
else
    echo "$a is not greater than 4!"
fi

./myscript.sh
```

### 03_02 Working with while and until loops

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "While Loop"
declare -i n=0
while ((n<10))
do
    echo "n:$n"
    ((n++))
done

echo -e "\nUntil Loop"
declare -i m=0
until ((m==10)); do
    echo "m:$m"
    ((m++))
done

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "While Loop"
declare -i n=0
while ((n<10))
do
    echo "n:$n"
    ((n++))
done

echo -e "\nUntil Loop"
declare -i m=0
until ((m>m)); do
    echo "m:$m"
    ((m++))
done

./myscript.sh
# Interrupt the program with Ctrl-C
```

### 03_03 Introducing 'for' loops

```bash
nany myscript.sh

#!/usr/bin/env bash

for i in 1 2 3
do
    echo $i
done

./myscript.sh
```

```bash
for i in 1 2 3; do echo $i; done
```

```bash
for i in {1..100}; do echo $i; done
```

```bash
nano myscript.sh

#!/usr/bin/env bash

for (( i=1; i<=10; i++ ))
do
    echo $i
done

./myscript.sh

clear
```

```bash
declare -a fruits=("apple" "banana" "cherry")
for i in ${fruits[@]}; do echo $i; done
```

```bash
nano myscript.sh

#!/usr/bin/env bash

declare -A arr
arr["name"]="scott"
arr["id"]="1234"

for i in "${!arr[@]}"
do
    echo $i: ${arr[$i]}"
done

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

for i in $(ls)
do
    echo "Found a file: $i"
done

./myscript.sh

ls
```

```bash
nano myscript.sh

#!/usr/bin/env bash

for i in *
do
    echo "Found a file: $i"
done

./myscript.sh
```

#### 03_04 Selecting behavior using 'case'

```bash
nano myscript.sh

#!/usr/bin/env bash

animal="dog"
case $animal in
    bird) echo "Avian";;
    dog|puppy) echo "Canine";;
    *) echo "No match!";;
esac

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

animal="bird"
case $animal in
    bird) echo "Avian";;
    dog|puppy) echo "Canine";;
    *) echo "No match!";;
esac

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

animal="elephant"
case $animal in
    bird) echo "Avian";;
    dog|puppy) echo "Canine";;
    *) echo "No match!";;
esac

./myscript.sh
```

### 03_05 Using functions

```bash
nano myscript.sh

#!/usr/bin/env bash

greet() {
    echo "Hi there!"
}

echo "And now, a greeting!"
greet

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

greet() {
    echo "Hi $1!"
}

echo "And now, a greeting!"
greet Scott

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

greet() {
    echo "Hi, $1! What a nice $2."
}

echo "And now, a greeting..."
greet Scott Morning
greet Everybody Evening

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

numberthings() {
    i=1
    for f in "$@"; do
        echo "$i: $f"
        ((i+=1))
    done
}

numberthings *
echo
numberthings pine birch maple spruce

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

numberthings() {
    i=1
    for f in "$@"; do
        echo "$i: $f"
        ((i+=1))
        echo "This output brought to you by $FUNCNAME!"
    done
}

numberthings *
echo
numberthings pine birch maple spruce

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

var1="I'm variable 1"

myfunction() {
    var2="I'm variable 2"
    local var3="I'm variable 3"
}
myfunction

echo $var1
echo $var2
echo $var3

./myscript.sh
```

### 03_06 Reading and writing text files

```bash
nano myscript.sh

#!/usr/bin/env bash

for i in 1 2 3 4 5
do
    echo "This is line $i" > ~/textfile.txt
done

./myscript.sh
cat textfile.txt
```

```bash
nano myscript.sh

#!/usr/bin/env bash

for i in 1 2 3 4 5
do
    echo "This is line $i" >> ~/textfile.txt
done

rm textfile.txt
./myscript.sh
cat textfile.txt
```

```bash
nano myscript.sh

#!/usr/bin/env bash

while read f
    do echo "I read a line an it says: $f"
done < ~/textfile.txt

./myscript.sh
```

### 04_01 Working with arguments

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "The $0 script got the argument: $1"

./myscript.sh Apple
```

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "The $0 script got the argument: $1"
echo "Argument 2 is $2"

./myscript.sh Apple Orange
./myscript.sh "Green Apple" Orange
```

```bash
nano myscript.sh

#!/usr/bin/env bash

for i in "$@"
do
    echo $i
done

echo "There were $# arguments."

./myscript.sh apple orange
./myscript.sh apple orange kiwi lemon
./myscript.sh "apple orange" kiwi lemon
```

### 04_02 Working with options

```bash
nano myscript.sh

#!/usr/bin/env bash

while getopts u:p: option; do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
    esac
done

echo "user: $user / pass: $pass"

./myscript.sh -u scott -p secret
./myscript.sh -p secret -u scott
```

```bash
nano myscript.sh

#!/usr/bin/env bash

while getopts u:p:ab option; do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
        a) echo "got the A flag";;
        b) echo "got the B flag";;
    esac
done

echo "user: $user / pass: $pass"

./myscript.sh -u scott -p secret
./myscript.sh -u scott -p secret -a
./myscript.sh -u scott -p secret -a -b
./myscript.sh -u scott -p secret -b
```

```bash
nano myscript.sh

#!/usr/bin/env bash

while getopts :u:p:ab option; do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
        a) echo "got the A flag";;
        b) echo "got the B flag";;
        ?) echo "I don't know what $OPTARG is!";;
    esac
done

echo "user: $user / pass: $pass"

./myscript.sh -u scott -p secret -b -z
```

### 04_03 Getting input during execution

```bash
nano myscript.sh

#!/usr/bin/env bash

echo "What is your name?"
read name
echo "What is your password?"
read -s pass

read -p "What's your favorite animal? " animal

echo "name: $name, pass: $pass, animal: $animal"

./myscript.sh

help read
```

```bash
nano myscript.sh

#!/usr/bin/env bash

select animal in "bird" "dog" "quit"
do
    echo "You selected $animal!"
    break
done

./myscript.sh
```

```bash
nano myscript.sh
#!/usr/bin/env bash

select animal in "bird" "dog" "quit"
do
    case $animal in
        bird) echo "Birds like to fly.":;
        dog) echo "Dogs like to play catch.";;
        quit) break;;
        *) echo "I'm not sure what that is.";;
    esac
done

./myscript.sh
```

### 04_04 Ensuring a response

```bash
nano myscript.sh

#!/usr/bin/env bash

read -ep "Favorite color? " -i "Blue" favcolor
echo $favcolor

./myscript.sh
./myscript.sh
./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

if (($#<3)); then
    echo "This command requires three arguments:"
    echo "username, userid, and favorite number."
else
    # the program goes here
    echo "username: $1"
    echo "userid: $2"
    echo "favorite number: $3"
fi

./myscript.sh
./myscript.sh scott 1234
./myscript.sh scott 1234 blue
```

```bash
nano myscript.sh

#!/usr/bin/env bash

read -p "Favorite animal? " fav
while [[ -z $fav ]]
do
        read -p "I need an answer! " fav
done

echo "$fav was selected."

./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

read -p "Favorite animal? [cat] " fav
if [[ -z $fav ]]; then
        fav="cat"
fi

echo "$fav was selected"

./myscript.sh
./myscript.sh
```

```bash
nano myscript.sh

#!/usr/bin/env bash

read -p "What year? [nnnn] " year
until [[ $year =~ ^[0-9]{4}$ ]]; do
        read -p "A year, please! [nnnn] " year
done
echo "Selected year: $year"

./myscript.sh
```
