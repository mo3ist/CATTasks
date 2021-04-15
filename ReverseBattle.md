## reversetaks.exe
First off, I ran **file** on the program

![](Attachments/Pasted%20image%2020210415025718.png)

How is this useful? idk :')

#### Step 0: Run the program
It asks for an array-of-integer's length then the elements of that array and quits.

![](Attachments/Pasted%20image%2020210415031751.png)

#### Step 1: Ghidra, a first look.
I went to the export and found the main entry `_main`, and I think Ghidra had a stroke analyzing this file :''

![](Attachments/Pasted%20image%2020210415030459.png)

There're many weird pointers and a lot of `undefined4` so I got lost.

#### Step 2: Quit Reverse Engineering.
shit's hard

#### Step 2: A look at the Function Call Graph.
There was that function `_test`.

![](Attachments/Pasted%20image%2020210415031002.png)

There was no functions to indicate a success or a failure.

#### Step 3: The strings.

![](Attachments/Pasted%20image%2020210415031305.png)

So it appears that there's no checks, we have to control the value of some variable to be equal to 1 to succeed.

#### Step 4: `_test`, a closer look.

![](Attachments/Pasted%20image%2020210415031940.png)

The function looks decent with the exception of line 13.

`local_18` appears to be an index or something (line 22). It's multiplied by 4, since we know that the program takes an array of integer as an input, maybe `param_1` is a `int *`. 
`param_2` can also be the length of that array (since you have to keep track of the size if we're dealing with pointers)

![](Attachments/Pasted%20image%2020210415032937.png)

Way cleaner!

So for this to be true (or 1), `local_14` must equal to 5. 
To get this value we need to fire `else` five times. But there's a catch: With every time it's fired, it will always be skipped the next time (cause of `bVar1`).

With all of that in mind, the final correct input should have a length of 10 with five fives!

After setting a breakpoint and running the program in IDA, this was the output

![](Attachments/Pasted%20image%2020210415033616.png)

Nice.
