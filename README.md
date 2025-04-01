# ngalay-project2

Inside this repository lies the second Assembly Project.

Nathan Galay CS 2160 MW (12:15PM - 1:30PM)

**Part 1**

input: abcdefghijkmnlopqrstuvwxyz (26 byte long string)

the program crashes at address PC 0x238. 

This occurs because the program originally only allocates enough memory to store a 20 byte string. Since I entered a 26 byte long string, this causes the buffer to overflow. The overflow then causes the additional characters outside of the max 20 byte string to overflow over into memory addresses next to the string. This causes an overwrite in the original PC address; so from bytes [20:25] those last bytes overwrite the original PC address and causing the error "trying to jump to unaligned address 78777675" That number 78777675 is the address that had overwritten the original value in x1/ra. After trying to jump to that return address and control is being returned to PC, the program tries to jump to address 0x238 which is misaligned due to 78777675. 
