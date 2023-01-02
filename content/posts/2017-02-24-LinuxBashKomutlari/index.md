---
title: "Linux Bash Commands"
date: 2017-02-24T00:00:00+00:00
hero: blog10_bash.png
description: Linux Bash Commands
menu:
  sidebar:
    name: Linux Bash Commands
    identifier: linux-bash-commands
    weight: -20170224
tags: [bash]
categories: [bash]

author:
  name: Akif T.
---

![bash](blog10_bash.png "bash")<br>


- **Active folder:**
```
pwd
```

- **Appearance:**

```bash
ls
'Detailed View Parameters:
-l *detailed*
-lrt *detailed*
-d *folder*
-r *read*
-w *write*
-x *execute*'
```

- **Active Folder Change:**
```
cd <folder name>
```

- **File Creation:**
```
touch <filename>
```

- **Displayed:**

```bash
ctrl+k 'delete to the right'

echo "hello world"
echo "hello coders" > filename 'Send output to file'
echo $? 'Returns the result of the last operation (0 successful)'
cat filename 'prints file contents'
more filename 'prints file contents'
less filename 'prints file contents'
nano filename 'opens with nano text editor'
vim filename 'opens with vim text editor'
head -n 5 output-1.txt 'show first 5 lines'
tail -n 5 output-1.txt 'show last 5 lines'
tail -f log-file.txt 'prints the last lines and follows the new incoming lines'
```

- **Creating Folder:**
```
mkdir foldername 'Creates folder'
```

- **Run by Sending or Receiving Content:**

```bash
./prog < input-1.txt 'Executes the content of input-1 by sending it to the prog.'
./my-prog < input-1.txt > output-1.txt 'assigns output to output'
./my-prog 12 2> output-1.txt 'redirects input from stderr to output'
./my-prog < input-1.txt > all-output.txt 2>&1 'stderr, redirect to stdoutput'

echo "asdasd" >> log-file.txt 'add to end of file'
```

- **Compilation:**
```
gcc -o my-prog my-prog.c 'compiles with gcc.'
```

- **Search in Process:**
```
ps aux | grep my-prog 'do not search among running processes'
```

- **Process Operations:**

```bash
'All processes start with: 0:stdinput 1:stdoutput 2:stderror'
'The system proc folder where processes are kept.'
'
ctrl+c : interrupt (sigint)
		   (sigterm)
		   (sighup) -> updates the configuration
		   (sigkill) -> kills without question
ctrl+l : clear terminal does the action
ctrl+d : (end of stream) kills the active process running
'
kill -l			'shows list of signals for kill'
kill -INT 12334		'sends sigint signal'
kill -TERM 12334	'kills with sigterm signal'

cat cmdline   'running location'
cat environ   'process in environment variables'
cat limits    'process limits'

top    'shows transactions momentarily'
htop   'shows by updating'
```

- **Path Operations:**

```bash
echo $PATH		'path content'
export PATH=$PATH:/home/vm/deneme		'add new directory to end of path'
```

- **Device Transactions:**

```bash
ls /dev       'shows all devices'
ls -lrt /dev/null      'sends output to null'
```

- **Data transfer:**

```bash
curl http://akifmt.github.io  	'downloads'
curl http://akifmt.github.io > /dev/null  	'downloads results to null'
curl -s http://akifmt.github.io > /dev/null	'downloads in silent mode, sends results to null'
curl http://akifmt.github.io > /dev/null 2>&1	'forward also incoming from stderr'

curl -s https://demo.consul.io/v1/catalog/services 'download in silent mode'
curl -s https://demo.consul.io/v1/catalog/service/web?pretty 'downloads in silent mode'
curl -s https://demo.consul.io/v1/catalog/service/web?pretty | grep Address 'downloads in silent mode, finds Address lines'
curl -s https://demo.consul.io/v1/catalog/service/web?pretty | jq '.[0].Address' 'Retrieves the first of the address part'
curl -s https://demo.consul.io/v1/catalog/service/web?pretty | jq -r '.[].Address' 'Retrieves all Address parts'
curl -s https://demo.consul.io/v1/catalog/service/web?pretty | jq -r '.[].Address' | while read serverAddr; do curl -s $serverAddr > $serverAddr.txt; done
'Finds all Address lines, sends them to the right, creates a file based on the Address names'
```

- **File Operations:**

```bash
echo "asd,qwe,qwwww" | cut -d, -f2 'takes the 2nd word after'
echo "asd;qwe;qwwww" | cut -d\; -f2 '; takes the 2nd word after'
echo "asd,qwe,qwwww" | awk -F, '{print $2}' 'takes the 2nd word after'
ls -lrt | awk '{print $9}' '9. print columns'
ls -lrt | cat 'pipe outputs to the right one'
wc -l 'line counts'
ls -lrt | wc -l 'sends results from ls to the right and counts lines'
find . -name my-prog 'finds names starting with my-prog'
find . -type f 'finds files'
find . -name "*.txt" 'finds txt extension'
find -name "*.txt" | while read filename; do echo $filename; done 'finds txt, sends it to the server, prints it as filename'
find -name "*.txt" | while read filename; do rm $filename; done 'delete files found'
dd if=/dev/zero of=zero-file.txt bs=512 count=2 'Creates 2x512KB empty file. '
hexdump zero-file.txt 'Displays in hex and abbreviated repetitive parts.'
hexdump -v zero-file.txt 'shows all without abbreviations'
dd if=/dev/urandom of=random-file.txt bs=512 count=4 'creates 4x512KB file with random value'
```

- **Operations on CPU:**

```bash
dd if=/dev/zero of=null 'generates zero to send null to device (uncontrolled cpu load)'
stress-ng -c 1 -l 40 '40% load on 1 CPU'
stress-ng -c 0 -l 40 'loads 40% on all CPUs'
```

- **Network Transactions:**

```bash
ifconfig 'shows network devices and connections'
route -n 'shows all access'
sudo wondershaper ens33 512 512 'Limits downloading and sending 512Kpbs'
sudo wondershaper ens33 clear 'removes limits'
```

- **Satır Sonları (lineending):**

| OS | Desc | Code |
|---|---|---|
| Linux    | LF (line feed)   |  (\n) |
| Unix     |  CR (carriage return)| (\r) |
| Windows  | CRLF  (carriage return line feed) | \r\n |
