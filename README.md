# ngalay-project2

Inside this repository lies the second Assembly Project.

Nathan Galay CS 2160 MW (12:15PM - 1:30PM)

**Part 1**

input: abcdefghijkmnlopqrstuvwxyz (26 byte long string)

the program crashes at address 78777675. 

This occurs because the program originally only allocates enough memory to store a 20 byte string. Since I entered a 26 byte long string, this causes the buffer to overflow. The overflow then causes the additional characters outside of the max 20 byte string to overflow over into memory addresses next to the string since strings are stored in contiguous memory. This causes an overwrite in the original PC address; so from bytes [20:23] those last bytes overwrite the original PC address and causing the error "trying to jump to unaligned address 78777675" That number 78777675 is the address that had overwritten the original value in x1/ra. After trying to jump to that return address and control is being returned to PC, the program tries to jump to address 0x238 which is misaligned due to the address 78777675. 

**Part 2**

input: abcdefghijklmnopqrstèþÿ¿

The program crashes at pc 0xbffffeec 

When you overflow the buffer, the program will automatically create new space to hold the overflown values in continuous memory. When this happens, the original return address is changed and this is what causes it to have some vulnerabiltiies if an attacker is able to take control of it. Since we know the return address that we want to return to is 0xbffffee8 we can take advantage of this by overflowing the buffer and inputting that same exact address using extended ASCII characters. When I inputted the string abcdefghijklmnopqrstèþÿ¿, after the program finishes and tries to return to main it will attempt to jump to 0xbffffee8 decided by the extended ASCII characters. Since this points to a specific location of the buffer, the program will try to execute it by unconditionally jumping to this location, since there is nothing there, this leads to the crash of the pc at 0xbffffeec.

If the buffer is not fully allocated for and taken care of, an attacker can take advantage of the buffer and overflow it to gain control of the return address that the program returns to so that they are able to change it to some destination that could potentially have some malicious intent. The key takeaway is that proper coding and use of functions are important so that an attacker cannot take control of the buffer and control the flow of the program and potentially point addresses to malicious ones.

**Part 3**

**What is the address of sekret_fn?**

0x0000323c

**What is the ASCII string that lets you encode that address?**

<2

**What is the input string you provide to cause main() to return to sekret_fn()?**

abcdefghijklmnopqrst<2

**What does sekret_fn() do?**

it prints out SGVsbG8gV29ybGQ= infinitely as long as you keep clicking the run button

**Any idea what the sekret_data is?**

After a quick google search, the output of sekret_fn is SGVsbG8gV29ybGQ= and this a string that is encoded in Base64. After using a decoder, this string can be translated to "Hello World!"

**REPORTING AND LOGGING**

**challenges I faced during this assignment**

I am happy to say that I breezed through this project fairly easily. 

The hardest part of the project was finding the address of sekret_fn. My first approach to finding the address was copying the sekret_fn function into another program and running it and seeing where the address jumps to. This does not work because the address is calcualted during run-time and not the actual address of where the function actually lies. I ended up plugging in a lot of random addresses and values that did not work. In the QtRV simulator there is a section in machine where you can go look at symbol. When I opened it, I was able to find sekret_fn's address which was 0x0000323c and I shoved that into a hex to text converter.


Another issue that I faced is in task 2. In my github I have provided screenshots of it. I have different addresses pointed to whenever I input the string abcdefghijklmnopqrstèþÿ¿. Sometimes I would have different addresses that I would return too and I am not too sure why. From my understanding, what is happening is that since the buffer is overflowed and were forcefull changing the return address, I am trying to jump to this address that I stated unconditionally because I know that the program is going to try to access that memory space that was overflowed from the buffer.

**Approximately how much time you think you spent on the project?**

I would say 6-8 hours.

**Documentation of functionality, bugs, etc. of your final submitted version**

Since this is a continuation of project 1, I did not have many bugs, just things that I had to fix. I believe I fixed those bugs from project 1 and should be adjusted in both my first github repository for project 1 and fixed version of the code in project 2.

**Sources**

https://www.digitalocean.com/community/tutorials/how-to-encode-and-decode-strings-with-base64-in-javascript
