# Lab Report 5 (11/19/2022)
David Wang

Here are the contents of my ```grade.sh``` file:
```
# Create your grading script here

CP=".:../lib/hamcrest-core-1.3.jar:../lib/junit-4.13.2.jar"

rm -rf student-submission
git clone $1 student-submission 2> /dev/null

cp TestListExamples.java student-submission

cd student-submission

if [ -e ListExamples.java ]
then
    javac -cp $CP *.java
    if [ $? -ne 0 ]
    then
        echo "Compiler Error! -> Fail"
        exit
    fi

    echo "Compiled"
    java -cp $CP org.junit.runner.JUnitCore TestListExamples > output.txt
    if [ $? -eq 0 ]
    then
        echo "100% of tests passed -> Pass"
        exit
    fi
    grep "failure" output.txt > result.txt
    score="$(grep 'failure' output.txt)"
    
    if [ "$score" = "There were 2 failures:" ]
    then
        echo "0% of tests passed- > Fail"
        exit
    elif [ "$score" = "There was 1 failure:" ]
    then
        echo "50% of tests passed -> Fail"
    fi
    exit
fi
echo "File was not found! -> Fail"
exit
```

I'll be testing it on 3 different submissions.

The first one is using https://github.com/ucsd-cse15l-f22/list-methods-lab3.

![Image](/images/lab5/test1.png)

The second one is using https://github.com/ucsd-cse15l-f22/list-methods-compile-error

![Image](/images/lab5/test2.png)

The third test is using https://github.com/ucsd-cse15l-f22/list-methods-corrected

![Image](/images/lab5/test3.png)

## Trace
---
I'll be tracing my third test (the one using https://github.com/ucsd-cse15l-f22/list-methods-corrected)

We'll be going over ```grade.sh``` step-by-step.

---
```
CP=".:../lib/hamcrest-core-1.3.jar:../lib/junit-4.13.2.jar"
```
This line sets the variable ```CP``` to the path containing the JUnit libraries needed for testing.

---
```
rm -rf student-submission
```
This line removes the directory named ```student-submission``` along with any of its contents, if it exists. Both the standard output and standard error are nothing, and the exit code is 0.

---
```
git clone $1 student-submission 2> /dev/null
```
This line clones the repository at the given link (the first parameter, given by ```$1```) into a directory called ```student-submission```. There is no standard output, and the standard error is redirected to ```/dev/null```. The standard error for this run would be:
```
Cloning into 'student-submission'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 695 bytes | 347.00 KiB/s, done.
```
The exit code is 0.

---
```
cp TestListExamples.java student-submission
```
This line copies over the file ```TestListExamples.java``` to ```student-submission```. There was no standard output or error, and the exit code was 0.

---
```
cd student-submission
```
This line navigates into the ```student-submission``` directory. There is no standard output or error in this run, and the exit code is 0.

---
```
if [ -e ListExamples.java ]
```
The command inside the if statement checks if a file named ```ListExamples.java``` exists in the current directory (```student-submission```). In this example, it does exist so it returns ```true```.

---
```
javac -cp $CP *.java
```
This command compiles all the java files using the JUnit libraries. For this run, the standard output and error are nothing, and the exit code is 0.

---
```
 if [ $? -ne 0 ]
```
This if statement returns ```true``` if the error code of the previous command (```$?```), which was the compiling command, has an exit code that is not zero. It returns ```false``` because, as described above, the exit code for the previous command was 0.

---
```
then
    echo "Compiler Error! -> Fail"
    exit
fi
```
These lines were inside the previous if block, and since the statement was false, these lines do not run.

---
```
echo "Compiled"
```
This line prints out "Compiled" to the standard output. There is no standard error, and the exit code is 0.

---
```
java -cp $CP org.junit.runner.JUnitCore TestListExamples > output.txt
```
This line runs all the file ```TestListExamples.class```. The standard output for this run, which is redirected to ```output.txt``` is:

```
JUnit version 4.13.2
..
Time: 0.011

OK (2 tests)
```
There is no standard error. The exit code is 0.

---
```
if [ $? -eq 0 ]
```
This if statement returns ```true``` because it checks if the return code (```$?```) for the last command (which in this case is the run command) is 0, and as described above, it is 0.

---
```
echo "100% of tests passed -> Pass"
```
This statement (inside of the if statement), has a standard output of "100% of tests passed -> Pass" and no standard error. The return code is 0.

---
```
exit
```
This line exits the script with no standard output or error and a return code of 0.

---
```
    grep "failure" output.txt > result.txt
    score="$(grep 'failure' output.txt)"
    
    if [ "$score" = "There were 2 failures:" ]
    then
        echo "0% of tests passed- > Fail"
        exit
    elif [ "$score" = "There was 1 failure:" ]
    then
        echo "50% of tests passed -> Fail"
    fi
    exit
fi
echo "File was not found! -> Fail"
exit
```
These are the remaining lines in the script, which do not run in this case because of the previous early exit command.