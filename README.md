# ngalay-project2

Inside this repository lies the second Assembly Project.

Nathan Galay CS 2160 MW (12:15PM - 1:30PM)

**Part 1**

input: abcdefghijkmnlopqrstuvwxyz (26 byte long string)

the program crashes at address PC 0x238. 

This occurs because the program originally only allocates enough memory to store a 20 byte string. Since I entered a 26 byte long string, this causes the buffer to overflow. The overflow then causes the additional characters outside of the max 20 byte string to overflow over into memory addresses next to the string. This causes an overwrite in the original PC address; so from bytes [20:25] those last bytes overwrite the original PC address and causing the error "trying to jump to unaligned address 78777675" That number 78777675 is the address that had overwritten the original value in x1/ra. After trying to jump to that return address and control is being returned to PC, the program tries to jump to address 0x238 which is misaligned due to 78777675. 

**Part 2**

input: abcdefghijklmnopqrstèþÿ¿

The program crashes at both t1 and pc where t1 has a new address of 0xbfffff00 and pc crashes at 0xbffffeec 

When you overflow the buffer, the program will automatically create new space to hold the overflown values in continuous memory. When this happens, the original pc return address is changed and this is what causes it to have some vulnerabiltiies if an attacker is able to take control of it. Since we know the return address of the stack is 0xbffffee8 we can take advantage of this by overflowing the buffer and inputting that same exact address using extended ASCII characters. When this happens, it causes the PC to try to read from that return address but we changed it. When the string is inputted, the PC return to address 0xbffffeec which is the address PC + 4 from oxbffffee8. This showcases that the vulnerability of string attacks and buffer overflow can be seen as a common or an important thing to lookout for in security. 

If the buffer is not fully allocated for and taken care of, an attacker can take advantage of the buffer and overflow it to gain control of the return address that the program returns to so that they are able to change it to some destination that could potentially have some malicious intent.

**Part 3**

**What is the address of sekret_fn?**

0x0000323c

**What is the ASCII string that lets you encode that address?**

<2

**What is the input string you provide to cause main() to return to sekret_fn()?**

abcdefghijklmnopqrst<2

**Any idea what the sekret_data is?**

After a quick google search, the output of sekret_fn is SGVsbG8gV29ybGQ= and this a string that is encoded in Base64. After using a decoder, this string can be translated to "Hello World!"
