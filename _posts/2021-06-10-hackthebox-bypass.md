---
published: true
title: Hackthebox - Bypass
author: f3dai
category: writeup
image: 'https://i.imgur.com/ltlchP7.png'
---
The challenge provides a 32 bit windows executable. 

Since this is .NET c#, I will use a tool called dnSpy (32 bit version, must be the same as the binary).

I will be doing the reverse engineering on a **Windows 10 AWS workspace**.

Import / load the executable into [**dnSpy**](https://github.com/dnSpy/dnSpy). It looks like the original name for this is **HTBChallenge.exe**. Open this tree and then the classes / functions. You should see this:

![tree](https://i.imgur.com/QcTHJ67.png)

Take a look at the first class "0":

![class 0](https://i.imgur.com/7WLqgeC.png)

You are looking at the code in *public class 0()*. The function from line 7 is setting a variable called flag and flag2. The program is then checking if it's set (to True).

<pre>
if (flag2)
{
	global::0.2();
}
-
if (flag)
{
	Console.Write(5.5 + global::0.2 + 5.6);
}
</pre>

The next function *public static bool 1()* on line 23 represents the program asking for my username and password. But for some reason it returns false regardless, so the user and pass prompts seem useless. Here is the code:

<pre>
Console.Write(5.1);
string text = Console.ReadLine();
Console.Write(5.2);
string text2 = Console.ReadLine();
return false;
</pre>

We can use breakpoints to provide a vector to pause execution flow and modify variables.

Add breakpoint (right click or click on the margin) to the if statements (line 11 and 39).

Enter garbage values to the program. When it hits the breakpoint - set the flag and flag2 variables to true and continue. 

![breakpoint 1](https://i.imgur.com/JVoozlo.png)

We are prompted for a secret key - enter anything again and let's change the flag variable to true again and continue.

![breakpoint 2](https://i.imgur.com/yqR2pSc.png)

It seemed like nothing happened, but I think it quit before I wanted to. Add another breakpoint at the end of the function, on line 48 so you can see the result. 

Finito :)


