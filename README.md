# From zero to bash hero

It's all about the tools you use.
Let me ask you this:
Would you take a builder seriously
if he turned up for a job without a hammer?
Yeah, me neither.
I would say a programmer without a solid knowledge of basic tools is like a builder without a hammer.
Learning just a couple of tricks and bash features is enough to speed up your daily workflow significantly
and by the end of this course you'll wonder how you could have worked without them.

### Day 1
1. Strictly bash
   1. Dotfiles
   1. REPL vs files
   1. Writing to the screen
   1. Aritmetic operations
   1. Variables
   1. Reading from input
   1. Script arguments
   1. Functions
   1. Conditional statements
   1. Loops
   1. Useful commands
   1. Job management
   1. Pipes
   1. Output redirection
   1. Loops and pipes
   1. Writing to stderr
   1. git part 1
1. Productivity supercharged
   1. tools
      1. fzf - https://github.com/junegunn/fzf

### Day 2
1. Dotfiles update
1. Aliases
1. Binding shortcuts
1. Brace expansion
1. Star expansion
1. Displaying directory structure
1. Pipes continued
2. Redirecting
1. git part 2
1. trap
1. diff and <()
1. tools
  1. sshrc
  1. tmux - https://formulae.brew.sh/formula/tmux
  1. tmuxinator - https://github.com/tmuxinator/tmuxinator
  1. autojump / z - https://github.com/wting/autojump
  1. functional shells
  1. asciinema - https://asciinema.org/
1. Big project
1. Present your improved setup

## Dotfiles

Moving from one machine to another is always a pain in the butt,
but you can minimize that using a repo for your configuration files.

My recommendation is to create a repo for all config files called dotfiles.

The suggested structure is:

```
.
├── .bash
│   └── prompt.bash
├── .bashrc
├── .gitignore
├── .oh-my.zsh
├── .tmux.conf
├── .zsh
│   ├── aliases.zsh
│   ├── config.zsh
│   ├── extras.zsh
│   ├── path.zsh
│   ├── secrets.zsh
│   └── vimode.zsh
├── .zshrc
└── readme.md
```

You install it by symlinking each file to you home folder.

```
ln -sv ~/projects/dotfiles/.zsh ~
```

Start gradually by moving just one config file: `.bashrc` - for example
```
cd ~/projects/dotfiles/
mv ~/.bashrc .
```

**REMEMBER - REVIEW YOUR CHANGES CAREFULLY,
THERE MIGHT BE SOME SECRETS IN YOUR CONFIG THAT YOU DO NOT WANT TO EXPOSE**

Exercise - start working on your `dotfiles` repository.
Create a repo on github, rewrite your current setup,
start working on other files like `.vimrc`, `.tmux.conf` etc.

## REPL vs files

You can write bash scripts in your terminal (REPL - read, eval, print loop) or save it in files.
It's customary to place a shebang line at the beginning of a file to indicate which shell language the script is written in.

We will use:

```bash
#!/bin/bash
```

Save this in a file called `script.sh` add executable permission `chmod +x`  and run it using:

```
./script.sh
```

You can also call bash with a script as a parameter like this:
```
bash ./script.sh
```

## Writing to the screen

Stdout:

```bash
echo "Hello world!"
```

Exercise: write a script that prints "Hello shell!"

## Aritmetic operations

```
echo $((5 + 6))
```

## Variables

Assigning a value:

```bash
a=42
```

Rembember to never add space between `=` and value.

To use variable value you need to add `$` before it.

```bash
a=42
echo $a
```

Exercise: Write a script that assigns "world" to a variable and print "Hello world!" using that variable.

## Reading from input

You can use command `read` to save input to variable

```bash
read a
echo "$a"
```

Exercise: write a script that reads a number and ads 5 to it.

### Reading secure values like password

```bash
read -s password
echo "Length of your password is: ${#password}"
```

## Script arguments

You can pass arguments to your script when you execute like this:

```
./script.sh world
```

The argument `world` is visible as `$1` inside your script.
You can use it like this:

```bash
echo "Hello $1!"
```

## Functions

Declaration:

```bash
a() {
  echo "a"
}
```

Execution:

```bash
a
```

Changing global variables:

```bash
a() {
  result="some result"
}

a
echo $result
```

Local variables exist:

```bash
a() {
  local my_result="some result"
}

a
echo $my_result
```

There is no "real" return value of bash functions.
You can print something and catch that using:

```bash
a() {
  echo "a"
}

b="$(a)"
echo $b
```

## Conditional statements

The overall structure of an if statement is:

```bash
if <condition>; then
<commands>
fi
```

There also is an extended version with "else if" and "else":

```
if <condition>; then
  <commands>
elif <condition>; then
  <commands>
else
  <commands>
fi
```

There are a couple of syntax options for conditions.
One is single bracket `[]`, the other double bracket `[[]]`.
In this course we will use the simplest one `(())` for arithmetic

Example:

```bash
if ((1 > 0)); then
  echo "yes"
fi
```

Exercise: Write a script that accepts a parameter and based on that parameter prints "yes" if it's larger than 0 and prints "no" when it's not.

## Loops

### For loop

```bash
for i in {1..5}; do echo $i ; done;
```

Exercise: Write a loop from 10 to 1.

### While loop

```bash
while <condition>
do
  <commands>
done
```

Example:
```bash
i=0
while (( $i < 5 ))
do
  echo $i
  i=$(( i + 1 ))
done
```

Exercise: Write a loop from 0 to 10 showing only even numbers.

## Useful commands

### Cat

Print file contents:

a.txt:
```
line 1
line 2
line 3
line 4
```

```bash
cat a.txt
```

### Head

Get's first `N` lines from a file.

Example:

a.txt:
```
line 1
line 2
line 3
line 4
```

```bash
head -n 2 a.txt
```

### Tail

Get's last `N` lines from a file.

Example:

a.txt:
```
line 1
line 2
line 3
line 4
```

```bash
tail -n 2 a.txt
```

### Grep

a.txt:
```
line 1
line 2
line 3
line 4
```

Searching using regular expressions.

Syntax:

```
grep WHAT FILE
```

grep 2 a.txt

### Curl

Downloading a page and printing it.

```
curl -s <website>
```

Downloading a page and saving it as a file it.

```
curl -s <website> -o <file>
```

Exercise: look for all lines containing word "Mieszko" on Poland wikipedia page (https://pl.wikipedia.org/wiki/Polska)

### Counting

Counting lines:

```
wc -l
```

Counting characters:
```
wc -c
```

### Text processing:

Uppercase / lowercase

```
awk '{print toupper($0)}' a.txt
```

Awk is a much bigger program, you can check out more options online.
For now we will use only basic things and if something new pops up it will be explained.

### Cuting pieces of file

a.txt:
```
a-1
b-2
c-3
d-4
```

```bash
cut -d '-' -f 1 a.txt
```

```bash
cut -d '-' -f 2 a.txt
```

### Moving around file system

Go into directory

```
cd directory
```

Go out of a directory
```
cd ..
```

Go to home directory
```
cd ~
```

### History of commands

```
history
```

### Manual pages

If you do not know how to use a particular command you can always use `man` to show instructions for it.

```bash
man curl
```

## Job management

When you run a program it is by default run in the foreground.
You can change that by using `&` at the end.
That task goes to a list of currently running tasks, e.g.

```bash
# krzysztof.slonka @ slonka-macbook in ~/projects/dotfiles on git:master x [15:42:28]
$ vim

[1]  + 6121 suspended  vim

# krzysztof.slonka @ slonka-macbook in ~/projects/dotfiles on git:master x [15:42:51]
$ nano &
[2] 6163
[2]  + 6163 suspended (tty output)  nano

# krzysztof.slonka @ slonka-macbook in ~/projects/dotfiles on git:master x [15:42:54]
$ jobs
[1]  - suspended  vim
[2]  + suspended (tty output)  nano
```

### Chaining executions

You can chain multiple executions using `&&`.
The execution will continue only when the first one completed with a success.

Example:
```
cat a.txt && echo "finish"
```

When a file `a.txt` exists this command will print "finish". If it doesn't it will not.

To chain execution even if the previous command does not succeed use `||`.

Example:
```
cat does_not_exist.txt || echo "finish"
```

### Suspended tty - additional

The `suspended (tty output)` - means that the job is blocked on outputing to the terminal.
Usually we do not want to mix output from multiple background jobs,
you can change that by setting

```bash
stty tostop
```

To reenable:

```bash
stty -tostop
```

To bring a job to the foreground you would use `fg %1` (where 1 is the number of the job), to run it in the background use `bg %1`.

## Pipes

Pipes are used to communicate between processes.
They are a way to connect input and output of one program to another.

```bash
echo "abc" | awk '{print toupper($0)}' | rev
```

Will print out "CBA"

The first one connects STDOUT of `echo` to the STDIN of `awk`.
After awk is done the "output" is "ABC".
The second one connects STDOUT of `awk` to STDIN of `rev`.
The output here is reversed, so it prints "CBA".
You can connect as many of these as you want.

The pipes used above are called *unnamed pipes*
(we did not specify a name).
There are also *named pipes*, more commonly known as FIFOs.
Named pipes differ from unnamed pipes in couple of aspects:
- named pipes are bidirectional, you can read and write to them
- unnamed pipes disappear as soon as the processes that created them exit

Exercise: count the characters in string "Eyjafjallajökull".

## Output redirection

Saving to a file:

```bash
echo "a" > ./a.txt
```

Appending to a file:

```bash
echo "b" >> ./a.txt
```

Reading from a file:

```bash
read b < a.txt
echo $b
```

## Loops and pipes

How does a loop differ from java loop?

```java
for (int i = 1; i <= 100; i++) {
  System.out.println(i);
}
```

vs

```bash
for i in {1..100}
  do echo "$i"
done
```

<details>
  <summary>Click to see the answer</summary>

Every command in bash has STDIN and STDOUT so you can do:

```bash
for i in {1..100}
  do echo "$i"
done | sort -nr
```

or

```bash
printf "1\n 2\n 3\n" | while read NUMBER; do echo "i like $NUMBER"; done
```

```zsh
echo "1\n 2\n 3" | while read NUMBER
do
  echo "I like $NUMBER"
done
```

```fish
#!/usr/local/bin/fish

echo "1" | while read x
  echo $x
end
```

And so on.

</details>

## Update your dofiles repository and clean up things

Remember to move your `.zshrc` file to `dotfiles` repository.

See what changes you made to `.bashrc`, review them, commit and push to the repository.

## Aliases

Aliases are a quick way to define a new version of a function.

You can define a new alias by doing:

```bash
alias ll='ls -lh'
```

Now every time you run `ll` it will actually run `ls -lh`.

Exercise: write an alias that lists files using their size in decreasing order, size should be human readable.

## Binding shortcuts

```bash
bind '"\C-f": "echo aaa\n"'
```

## Brace expansion

As seen in the "for loops" section you can use `{}` to expand values.

For example:

```bash
echo {0..5}
```

Will show numbers from 0 to 5. You can also use other modes of expansion for example `,`. It does not interpolate the result but only puts the interpolated value in place.

```bash
mkdir backup_{one,two,three}
```

Will create folders `backup_one`, `backup_two`, `backup_three`.

Exercise: create 24 folders with the format `YEAR-MONTH` for years 2019, 2020 and every month (from 1 to 12).

Exercise 2:
Make the same 24 directories with a structure `YEAR/MONTH` (where month is in a subfolder).

Hint: `mkdir -p`

## Star expansion

You can use `*` to match any characters in your command. For example:

```bash
ls ./*/03
```

Executed in the directory from a previous exercise will show contents of all subfolders called "03".

```
$ ls ./*/03
./2019/03:

./2020/03:
```

## Displaying directory structure

You can use `tree` command to see a hierarchical tree of directories.

## Pipes continued

How can I see my unnamed pipe?
You can use `lsof` to list all files opened by a process.

```bash
# priv @ slonka-macbook in ~/projects/from-zero-to-bash-hero on git:master x [11:03:37]
$ echo "abc" | sleep 1000 &
[1] 20250 20251

# priv @ slonka-macbook in ~/projects/from-zero-to-bash-hero on git:master x [11:09:48]
$ lsof -p 20251 | awk 'NR==1{print};/PIPE/{print}'
COMMAND   PID USER   FD   TYPE             DEVICE   SIZE/OFF     NODE NAME
sleep   20251 priv    0   PIPE 0xa2b5a10dd9a580f7      16384
```

To create a named pipe (fifo) you can use `mkfifo` command.
You can write to a fifo and then read from it.
Open two terminals, in the first type:

```bash
cd /tmp
mkfifo a
echo "abc" > a
```

In the second terminal type:

```bash
cd /tmp
cat a
```

You can see that the first one blocks on `echo`.
This is the nature of pipes.
You can reed it in the manual `mkfifo(3)`

> However, it has to be open at both ends simultaneously before you can proceed to do any input or output operations on it. Opening a FIFO for reading normally blocks until some other process opens the same FIFO for writing, and vice versa.

Exercise - use named pipes to solve "uppercase and reverse" problem shown before.

### Redirecting

To discard the output you can redirect it to virtual device `/dev/null`

```bash
echo "aaa" > /dev/null
```

Sometimes commands do not use standard output but error output to display text.

This will not work:
```
cat not_existing > /dev/null
```

But this will:
```
cat not_existing 2>/dev/null
```

## Git

To combine all the things we learned so far we're going to create a statistic.
Suppose you're working on a project where you use a tool called `Envoy`.
You want to upgrade to a new version but you do not know how dangerous it is.
Fortunately Envoy maintainers keep "Risk Level" description in every pull request, e.g. https://github.com/envoyproxy/envoy/pull/10285.
Write a script that counts how many times a "high risk" and "medium risk" pull request has been merged from a version you are currently using `v1.13.0` to the version `v1.13.1`.

Useful commands:

```
git log --oneline
```

### Additional

To combine all the things we learned so far we're going to create a simple chart.
The chart will show 10 commits in 5 commit interval starting from the oldest,
the time it took to run tests on that revision
and a number of "#" characters corresponding to the time it took.

An example chart:

```
############# 13.13 f6f42da6607a426fdc5278232ef4ef6f31e63486
############ 12.23 503e3267c606868bb81b3f545f9232201e318331
############# 13.59 5e2a51bad8ec0fff3fe1f771ca5b97f1ad79ffcd
############# 13.16 10da9ef9073ca2632047bdeb37ca13e59a5df121
############### 15.02 e0e64f8fd712a595e7c0ed928e05646024984f29
############# 13.89 161cc091d32ca4a303eeb6534871dfe6fdd68dcd
############# 13.26 0aa039bf49c372194369ceed06e680efadaed390
############## 14.24 584a576765eac89bae83a23eb0d26e8594ef11c7
############ 12.21 eb32cff78b324dfebaf5044bb53ab7a12a9d0de0
####### 7.15 37b3f7ea7b93d7085affbbdec82aa8979b138640
```

As an example repo we can use [this repo](https://github.com/slonka/node-worker-nodes).
Or you can use whatever repo you want.

Useful commands: `git rev-list`, `git checkout -q`, `seq`

Gradle exiting the loop after first iteration fix (you need to redirect dev null to stdin):

```
./gradlew unitTest 2>/dev/null < /dev/null
```

Now we know that the improvement happened somewhere between `eb32cff78b324dfebaf5044bb53ab7a12a9d0de0`
and `37b3f7ea7b93d7085affbbdec82aa8979b138640`
but we don't know where exactly.
What if tests took a lot of time and we did not want to go through all of them?

Let's abuse `git bisect` to find the commit that introduced the change.
This command takes two commits in a history tree
and using binary search finds the one that matches our search criteria.

<details>
<summary>Bisect script</summary>

```bash
git bisect start eb32cff78b324dfebaf5044bb53ab7a12a9d0de0 37b3f7ea7b93d7085affbbdec82aa8979b138640
```

```bash
#!/bin/zsh

npm i &> /dev/null && git clean -f &> /dev/null && git reset --hard &> /dev/null && echo $(sh -c "time -p npm test &> /dev/null" 2>&1) | head -1 | awk '{print $2}' | read testtime

if (($testtime < 10)); then
  exit 1
else
  exit 0
fi
```

</details>

## trap

There might be situations when you don't want users of your scripts to exit untimely using keyboard abort sequences,
for example because input has to be provided or cleanup has to be done.
The trap statement catches these sequences and can be programmed to execute a list of commands upon catching those signals.

```bash
trap "echo Boom! && exit 1" SIGINT SIGTERM
echo "pid is $$"

sleep 60	# This script is not really doing anything.
```

Exercise - write a script that runs 5 istances of a python server
and kills all of them when you SIGINT (ctrl - c) the main script.

Starting a python server:

```bash
python -m SimpleHTTPServer 8080
```

## diff and <()

Diff works as the name suggest, it diffs two files together showing you their difference.
But there is an imposed limit here.
What if you wanted to diff something different than a file?
A command ouput for example?

For that we're going to use something that's called `Process Substitution`,
it executes a command and puts a pipe with results in it's place.

For example:

```bash
diff <(ls ~/projects/alle-envoy-control) <(ls ~/projects/conwatch/)
```

To see that they are actually pipes and not something different run:

```bash
# krzysztof.slonka @ slonka-macbook in ~/projects/rafting-for-fun-and-profit on git:master o [7:19:50]
$ file <(echo 1) <(echo 2)
/dev/fd/11: fifo (named pipe)
/dev/fd/12: fifo (named pipe)
```

Exercise - write a command that diffs the polish and english version of google.

Exercise (optional) - write a command that diffs the production version of your website with a local development version.

## sshrc

Sshrc is a great script that enables you to bring your config with you when you ssh into a server.
You can find it on https://github.com/ikuwow/sshrc.
It's in homebrew so osx install is pretty easy:

```
brew install ikuwow/ikuwow-sshrc/sshrc
```

After that you can use it like you would `ssh`.

For example:

```
sshrc slonka@s37.mydevil.net
```

You can test your setup in a docker container.
Run an ubuntu container with a port redirect `2200` to `22`:

```bash
docker run -p 2200:22 -it ubuntu
```

Below is a one liner function that installs ssh
and allows logging in as `root` with password `my_password`.
It takes one argument - container name.

```
function docker-sshrcize() {
  docker exec $1 /bin/sh -c "apt-get update -y && apt-get install -y python openssh-server && echo "root:my_password" | chpasswd && echo 'PermitRootLogin yes' > /etc/ssh/sshd_config && service ssh start"
}
```

Example usage:

```bash
docker-sshrcize competent_zhukovsky
```

And after that you can login using:

```
sshrc root@localhost -p 2200
```

## functional shell

If you like functional style programming, you can use [shell-functools](https://github.com/sharkdp/shell-functools).
It is a collection of functional style operators for your shell.

We can rewrite the example of checking out commits using rev-list:

```bash
git rev-list --max-count=50 --reverse master | while read commit; do git checkout -q $commit; done
```

Into more functional style:

```bash
git rev-list --max-count=50 master | map duplicate | map -c1 format "checkout" | map run git
```

Exercise (optional) - rewrite yesterday's time measuring script into functional style.

## recording a session

There is a nifty tool called [asciinema](https://asciinema.org/) that allows you to record you terminal sessions
and share them with your friends. **No more 50-MB gifs of your terminal.**

Here is an example recording:

[![asciicast](https://asciinema.org/a/hSNwrBhqdWYszdPNbX0FT6550.png)](https://asciinema.org/a/hSNwrBhqdWYszdPNbX0FT6550)

There is also a way to record a whole tmux session - [instructions here](https://github.com/asciinema/asciinema/wiki/Recording-tmux-session).


## Big project

Work on something that will improve your productivity.
It might be:
- something from your `sshrc` config, like making your favourite programs portable (fzf for example)
- writing a script that synchronizes command history
- tmuxinator config for fast ssh-ing into multiple machines
