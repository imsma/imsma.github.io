---
layout: post
title:  "How to run multiple terminal commands in one line in Ubuntu / Linux?"
author: sma
categories: [ Linux, Ubuntu ]
image: assets/images/posts/bridge-buildings-business-2019059.jpg
description: "How to run multiple terminal commands in one line in Ubuntu / Linux?"
---

Writing multiple commands together is known as *Command Chaining*, in this  article I will show you different methods to combine multiple commands on terminal / shell.

## Using Semicolon (;)

If you combine multiple commands using semicolon (;) operator, commands will execute even if the previous command in chain succeeds or fails.

In following example `command2` will execute regardless of the `command1`'s success.

```
command1;command2
```

Here is a more practical example.

```
ping google.com -c 3; whoami; pwd
```

Above chain of commands  will:

* `ping`: ping `google.com`  3 times.
* `whoami`: print *user name* of currently logged in user.
* `pwd`: print current working directory.

---

## Using AND Operator (&&)

Chaining multiple commands using AND (&&) operator will execute the command only and only if preceding command executes successfully.

`command2` from following snippet will only execute if `command1` runs successfully.

```
command1 && command2
```

From following listing the `cd newDirectory` command will only execute if `mkdir newDirectory` command was successful.

```
mkdir newDirectory && cd newDirectory
```

## Using OR Operator (||)

Using OR (||) operator makes sure that the following command executes only and only if preceding command fails.

`command2` from following snippet will only execute if `command1` fails to execute.

```
command1 || command2
```

From following example the `echo` command will only execute if `ping` command fails which means you are facing internet connectivity issue.

```
ping -c 3 google.com  || echo "You are not connected to internet"
```

## Using NOT Operator (!)

You can use `NOT (!)` operator  to exclude some results, following is an example command to delete all files in a directory except with `.text` extension. 

**Important Note**: if you are trying this example, please do it in a practice / temporary  directory to avoid deletion of actual / important work files.

```
rm -r !(*.text)
```

## Combining AND (&&) and OR (||) operators

You can combine `AND (&&)` and `OR (||)` operators to simulate `if ... else` scenario, let's rewrite the *OR (||) operator* example to print both *SUCCESS* and *FAILURE* message for internet connectivity check.

```
ping -c 3 google.com1 && echo "Your internet connection is OK"  || echo "You are not connected to internet"
```

That's it, hope you enjoyed it. This was part one of *using operators on linux command line  shell /terminal*, in next part of this article I will show more operators to enhance your productivity while working with *linux shell commands*. You like this article, have some questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!

    


