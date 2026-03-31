# Shell Scripting

## Flags

`-eq`   - equal to
`-ne`   - not equal to
`-gt`   - greater than
`-lt`   - lesser than
`-ge`   - greater than or equal to
`-le`   - lesser than or equal to

<!-- #!/bin/bash

a=10
b=20

if [ $a -eq $b ]; then
    echo "a is equal to b"
fi

if [ $a -ne $b ]; then
    echo "a is not equal to b"
fi

if [ $a -gt $b ]; then
    echo "a is greater than b"
else
    echo "a is not greater than b"
fi

if [ $a -lt $b ]; then
    echo "a is less than b"
fi

if [ $a -ge 10 ]; then
    echo "a is greater than or equal to 10"
fi

if [ $b -le 20 ]; then
    echo "b is less than or equal to 20"
fi -->

### String
`=`     - equal
`!=`    - not equal to
`-z`    - is empty
`-n`    - not empty

<!-- #!/bin/bash

str1="hello"
str2="world"
empty=""

if [ "$str1" = "$str2" ]; then
    echo "Strings are equal"
else
    echo "Strings are not equal"
fi

if [ "$str1" != "$str2" ]; then
    echo "Strings are different"
fi

if [ -z "$empty" ]; then
    echo "String is empty"
fi

if [ -n "$str1" ]; then
    echo "String is not empty"
fi -->

```
${VAR^^} : Converts everything to UPPERCASE.
${VAR,,} : Converts everything to lowercase.
${VAR^} : Capitalizes only the first letter.
${VAR,} : Lowercases only the first letter.
```

<!-- #!/bin/bash

VAR="hello world"

echo "Original: $VAR"

echo "Uppercase all: ${VAR^^}"
echo "Lowercase all: ${VAR,,}"
echo "Capitalize first letter: ${VAR^}"
echo "Lowercase first letter: ${VAR,}" -->


### Logical Operators

`-a`    - AND
`-o`    - OR
`!`     - NOT

<!-- #!/bin/bash

read -p "Enter your age: " age
read -p "Do you have ID? (yes/no): " id

# Condition using AND (-a), OR (-o), NOT (!)

if [ $age -ge 18 -a "$id" = "yes" ]; then
    echo "Access granted (Adult with ID)"

elif [ $age -lt 18 -o "$id" = "no" ]; then
    echo "Access denied (Minor OR No ID)"

fi

# Using NOT (!)
if [ ! "$id" = "yes" ]; then
    echo "Warning: ID is mandatory"
fi -->


<!-- if [[ $age -ge 18 && "$id" == "yes" ]]; then
    echo "Access granted"
fi

if [[ $age -lt 18 || "$id" == "no" ]]; then
    echo "Access denied"
fi

if [[ "$id" != "yes" ]]; then
    echo "ID is mandatory"
fi -->

### Loops

# For loop
for i in 1 2 3 4
do
  echo $i
done

# While loop
i=1
while [ $i -le 5 ]
do
  echo $i
  ((i++))
done

# Until loop
i=1
until [ $i -gt 5 ]
do
  echo $i
  ((i++))
done

### File Test Operators

`-e file`    - file exist
`-f file`    - regular file
`-d file`    - directory exist
`-r file`    - Readable
`-w file`    - Writable
`-x file`    - Executable
`-s file`    - File is not empty


<!-- #!/bin/bash

file="test.txt"
dir="sample_dir"

# Check if file or directory exists
if [ -e "$file" ]; then
    echo "$file exists"
fi

# Check if it's a regular file
if [ -f "$file" ]; then
    echo "$file is a regular file"
fi

# Check if it's a directory
if [ -d "$dir" ]; then
    echo "$dir is a directory"
fi

# Check permissions
if [ -r "$file" ]; then
    echo "$file is readable"
fi

if [ -w "$file" ]; then
    echo "$file is writable"
fi

if [ -x "$file" ]; then
    echo "$file is executable"
fi

# Check if file is not empty
if [ -s "$file" ]; then
    echo "$file is not empty"
else
    echo "$file is empty or does not exist"
fi -->

### Tasks

# COUNT LINES, WORDS, CHARACTERS

#!/bin/bash

file=$1

echo "Lines: $(wc -l < $file)"
echo "Words: $(wc -w < $file)"
echo "Characters: $(wc -c < $file)"

# PROCESS LOG FILE

#!/bin/bash

logfile="app.log"

echo "ERROR logs:"
grep "ERROR" $logfile

echo "WARNING logs:"
grep "WARNING" $logfile

echo "Logs from today:"
grep "$(date '+%Y-%m-%d')" $logfile


# SYSTEM INFORMATION SCRIPT

#!/bin/bash

echo "Kernel Version:"
uname -r

echo "Memory Usage:"
free -h

echo "Running Processes:"
ps aux | head -10

echo "Installed Packages (Ubuntu):"
dpkg -l | head -10
















### SUPERVISOR

`sudo apt update`

`sudo apt install supervisor`

`supervisord --version`


#### Create a "Dummy" Script
Create a simple script that prints the date every second. 

Command: `nano ~/ticktock.sh`
Content:
bash
#!/bin/bash
while true; do
  echo "Tick: $(date)"
  sleep 1
done

#### Create the Supervisor Config

Supervisor looks for configuration files in `/etc/supervisor/conf.d/`.

Command: `sudo nano /etc/supervisor/conf.d/ticktock.conf`

CONTENT:

[program:ticktock]
command=/home/alphamale/super/ticktock.sh
autostart=true
autorestart=true  
stderr_logfile=/var/log/ticktock.err.log
stdout_logfile=/var/log/ticktock.out.log


### Activate and Manage

Reread configs: `sudo supervisorctl reread` (Finds the new file)
Update: `sudo supervisorctl update` (Starts the new process)
Check Status: `sudo supervisorctl status`
The Log Test:  `sudo tail -f /var/log/ticktock.out.log`
The Crash Test: `sudo supervisorctl status`
kill it manually: `sudo kill -9 <PID>`
The Control Test: `sudo supervisorctl`
Reload Supervisor : `sudo supervisorctl reread`
`sudo supervisorctl update`


##### Inside supervisor 
```
To start: `start ticktock` 
To stop: `stop ticktock`
To get log: `tail -f ticktock`  
```

e.g.

sudo supervisorctl start test
sudo supervisorctl stop test
sudo supervisorctl restart test
sudo supervisorctl status

##### Logs

cat /var/log/test.out.log
cat /var/log/test.err.log


# Monit

`sudo apt install monit -y`

| Command | Meaning | output |
| :--- | :--- | :--- |
| `dpkg -l \| grep monit` |  |<pre><code>i  iotop                           0.6-42-ga14256a-0.2build1               amd64        simple top-like I/O monitor <br>ii  monit                           1:5.33.0-2build2                        amd64        utility for monitoring and managing daemons or similar programs </code></pre>|
| `monit --version` | To get avilability |  |
| `monit -V` | To get avilability |  |
| `sudo systemctl status monit` | To get the status |  |
| `sudo systemctl disable monit` | To disable in startup |  |
| `sudo systemctl enable monit` | To enable in startup |  |
| `cd /etc/monit` | FILES OF MONIT | <pre><code>drwxr-xr-x  6 root root  4096 Mar 18 10:30 ./ <br>drwxr-xr-x 93 root root  4096 Mar 18 10:29 ../<br>drwxr-xr-x  2 root root  4096 Mar 18 10:30 conf-available/<br>drwxr-xr-x  2 root root  4096 Apr  1  2024 conf-enabled/<br>drwxr-xr-x  2 root root  4096 Apr  1  2024 conf.d/<br>-rw-------  1 root root 13503 Apr  1  2024monitrc  <br>drwxr-xr-x  2 root root  4096 Mar 18 10:30 templates/</code></pre>|
| `sudo cat monitrc` | file to configure monit |  |
| ` sudo monit reload` | To reload monit | Reinitializing monit daemon |
| `sudo systemctl reload monit` | To reload monit |  |
| `sudo systemctl status monit` | To get the status |  |
| `sudo monit status` | to get the status |  |


The default credentials for the Monit web interface are:
Username: admin
Password: monit 

```
set httpd port 2812 and
    use address 0.0.0.0
    allow 0.0.0.0/0
    allow admin:monit
```

To open the monit UI : `http://localhost:2812`

To get WSL host name : `hostname -I`

If above does not work : `http://<YOUR_WSL_IP>:2812`

<br>
<br>


















# Loops

## FOR LOOP
e.g.

```
for variable in list
do
  commands
done
```

#### Example 1: Print numbers
```
#!/bin/bash

for i in 1 2 3 4 5
do
  echo "Number: $i"
done
```

#### Example 2: Loop using range
```
#!/bin/bash

for i in {1..5}
do
  echo "Count: $i"
done
```

#### Example 3: Loop through files
```
#!/bin/bash

for file in *.log
do
  echo "Processing $file"
done
```

## WHILE LOOP

#### Syntax
```
while condition
do
  commands
done
```

#### Example 1: Counter
```
#!/bin/bash

i=1

while [ $i -le 5 ]
do
  echo "Value: $i"
  ((i++))
done
```

#### Example 2: Read file line by line
```
#!/bin/bash

while read line
do
  echo "$line"
done < file.txt
```


## UNTIL LOOP

#### Syntax
```
until condition
do
  commands
done
```

#### E.g
```
#!/bin/bash

i=1

until [ $i -gt 5 ]
do
  echo "Value: $i"
  ((i++))
done
```


## Loop Control Statements

#### break (stop loop)
```
for i in {1..10}
do
  if [ $i -eq 5 ]
  then
    break
  fi
  echo $i
done
```

#### continue (skip iteration)
```
for i in {1..5}
do
  if [ $i -eq 3 ]
  then
    continue
  fi
  echo $i
done
```

<br>

# Conditions

## IF Statement
```
if [ condition ]
then
  commands
fi
```

#### E.g.
```
#!/bin/bash

num=10

if [ $num -gt 5 ]
then
  echo "Number is greater than 5"
fi
```

## IF-ELSE

#### E.g.
```
#!/bin/bash

num=3

if [ $num -gt 5 ]
then
  echo "Greater"
else
  echo "Smaller"
fi
```

## IF-ELIF-ELSE

#### E.g.
```
#!/bin/bash

num=5

if [ $num -gt 5 ]
then
  echo "Greater"
elif [ $num -eq 5 ]
then
  echo "Equal"
else
  echo "Smaller"
fi
```


## Comparison Operators

#### Numeric

| Operator | Meaning          |
| :--- | :--- |
| `-eq`    | equal            |
| `-ne`    | not equal        |
| `-gt`    | greater than     |
| `-lt`    | less than        |
| `-ge`    | greater or equal |
| `-le`    | less or equal    |

#### String

| Operator | Meaning      |
| :--- | :--- |
| `=`      | equal        |
| `!=`     | not equal    |
| `-z`     | empty string |
| `-n`     | not empty    |


#### File Conditions

| Condition | Meaning          |
| :--- | :--- |
| `-f`      | file exists      |
| `-d`      | directory exists |
| `-r`      | readable         |
| `-w`      | writable         |
| `-x`      | executable       |


## Examples

#### Example 1: Check File Exists

```
#!/bin/bash

if [ -f "test.txt" ]
then
  echo "File exists"
else
  echo "File not found"
fi
```

####  Example 2: Check Service Running
```
#!/bin/bash

if pgrep nginx > /dev/null
then
  echo "Nginx is running"
else
  echo "Nginx is stopped"
fi
```

#### Example 3: Disk Usage Alert
```
#!/bin/bash

usage=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')

if [ $usage -gt 80 ]
then
  echo "Disk usage is high: $usage%"
else
  echo "Disk is normal"
fi
```

## Logical Operators

#### AND (&&)
if [ $a -gt 5 ] && [ $b -lt 10 ]
#### OR (||)
if [ $a -gt 5 ] || [ $b -lt 10 ]
#### NOT (!)
if [ ! -f file.txt ]
## Nested IF
```
#!/bin/bash

num=10

if [ $num -gt 5 ]
then
  if [ $num -lt 20 ]
  then
    echo "Between 5 and 20"
  fi
fi
```
### Short Form (One-Line IF)
```
[ -f file.txt ] && echo "Exists" || echo "Not exists"
```

## Linux Commands

#### grep → Search Text

`grep "ERROR" app.log`

`grep -c "ERROR" app.log`

`grep -i "error" app.log`

`grep -n "ERROR" app.log`

#### awk → Data Processing Tool

`df -h | awk '{print $1, $5}'`

`df -h | awk 'NR==2 {print $5}'`

#### sed → Stream Editor

`sed 's/error/ERROR/g' file.txt`

`echo "disk=80%" | sed 's/%//'`

#### cut → Extract Columns

`cut -d':' -f1 /etc/passwd`

#### sort → Sort Data

`sort file.txt`

#### Numeric sort

`sort -n numbers.txt`

#### uniq → Remove Duplicates

`sort file.txt | uniq`

#### Count duplicates

`sort file.txt | uniq -c`

#### wc → Count Data

`wc -l file.txt`   # lines
`wc -w file.txt`   # words
`wc -c file.txt`   # characters

#### head & tail

head file.txt

head file.txt

tail -f app.log

#### tr → Translate Characters

`echo "hello" | tr 'a-z' 'A-Z'`


## NOTE
```
Your App (Python / Node / Script)
        ↓
Supervisor (start, stop, restart)
        ↓
Monit (checks health, CPU, memory, alerts)
```