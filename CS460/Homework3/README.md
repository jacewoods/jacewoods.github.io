# Homework 3
* [Assignment Page](http://www.wou.edu/~morses/classes/cs46x/assignments/HW3_1819.html)
* [Repository](https://github.com/jacewoods/CS460/tree/master/homework3)
* [Back to Homepage](https://jacewoods.github.io/)

The third assignment in CS460 included learning how to use the following things:
1. Visual Studio
1. C#
1. Merging branches with Git

# Visual Studio
The first thing to do to get a C# environment running was to download [Visual Studio IDE](https://visualstudio.microsoft.com/), specifically, the 2017 Community version. I've been using this specific version of Visual Studio for a few months now as my main code editor, so getting used to some of it's basic features like auto formatting (Ctrl+K+D) was not a problem. I have not used it to run C# code, however, so learning how to do this was the first step in my process for the homework.

The main thing that initially tripped me up with Visual Studio and C# was how code was ran and executed. When I built a simple 'Hello World' program, it did not appear to run when I clicked the green 'play' button. However, running it with the debugger (Ctrl+F5) fixed this issue, and I stuck with this method of running code during my tests when the actual java converted program was complete.

# C#
Converting the java code to C# code was a somewhat complicated but satisfying experience. I haven't worked with java much in the past as I first learned how to code primarily in C++. That said, I was surprised how much more similar C# was to java than to C++, which logically, given the names, C++ seems like it would be more similar. The code conversions were mostly 1-to-1, but there were a few errors that I found and had to do some research to discover the error and resolve the difference in syntax.

One example being the use of 'implements' in java code when declaring certain classes, whereas C# used a ':' to declare the equivalent meaning. Here is an example of the difference between the two with the same implementation of code:

Java:
```java
public class LinkedQueue<T> implements QueueInterface<T>
```
C#:
```c#
class LinkedQueue<T> : IQueueInterface<T>
```
The way I discovered how to fix this error was to do some research online. Eventually I discovered this [stackexchange response](https://stackoverflow.com/questions/513028/c-sharp-equivalent-of-java-implements-keyword) and was able to understand the difference and make the change to enable the code to work

I also got tripped up in the output of the code, where instead of
```c#
Console.Write(" ");
```
I wrote
```C#
Console.WriteLine(" ");
```
This resulted in correct yet wonkily spaced output code that caused me to look at the entirety of my function before discovering the simple mistake I had made. This was more of a learning experience where, in hindsight, I should have understood that my mistake was in the actual output rather than the code that built the binary, and could have saved myself some time if I checked on and identified the difference between 'Write' and 'WriteLine'.

Eventually I was able to resolve the small changes necessary to make in order to get the code working, which taught me a lot about how the C# code works (and java code, for that matter), and will hopefully prepare me for the future where I embrace a lot more C# coding! Below is an example of my output when the input is '12':

![image text](/CS460/Homework3/hw3output.PNG "Screenshot of my C# output")

# Merging branches with Git
This is more of a reiteration of learning how to branch and merge that was learned in Homework 2, but nonetheless, the steps I had written down and used to replicate the process in homework 3 were very helpful, and therefore I will repost the example step-by-step process that I take to set up a successful merge to master:

1. git branch hw2-main (creates the new branch)
1. git checkout hw2-main (goes to the new branch)
1. git branch (verify I am on the new branch)
1. vi homeworkfiles.html (create a file in new branch)
1. git add homeworkfiles.html
1. git commit -m "added homework files"
1. git checkout master (go back to master branch)
1. vi README.md (add change to make merge visible)
1. git add README.md
1. git commit -m "showing commit on master"
1. git merge hw2-main (merges hw2-main into master branch)
1. git log --oneline --graph (visual of hw2-main being merged into master branch, and to make sure it worked)
1. git push origin master
1. git push origin hw2-main
