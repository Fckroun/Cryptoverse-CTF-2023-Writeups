# Pwn/Acceptance

![Untitled](Pwn%20Acceptance%201b460d4be1b743738146333882584d35/Untitled.png)

I downloaded the file and checked it with checksec:

![Untitled](Pwn%20Acceptance%201b460d4be1b743738146333882584d35/Untitled%201.png)

I tried to execute the binary:

![Untitled](Pwn%20Acceptance%201b460d4be1b743738146333882584d35/Untitled%202.png)

It takes the input (**abcd**) and outputs a message which is not the flag obviously x)

It felt like debugging time so i fired up **pwndbg:**

![Untitled](Pwn%20Acceptance%201b460d4be1b743738146333882584d35/Untitled%203.png)

Disassembling and looking at the **main** function, i noticed :

- A declared **36(0x24) byte buffer**
- A **read** function call wich obviously handles our input
- A test eax, eax followed by a conditional **je**
- A print_flag function

Using radare2 i was able to better understand the code flow.

![Untitled](Pwn%20Acceptance%201b460d4be1b743738146333882584d35/Untitled%204.png)

The **test eax, eax** and the **je** instruction decides whether we get the **flag** or **“Arg! Why don't you help me :((”.**

It performs a bitwise logical operation between the value in the **eax** register and itself then **je** checks if the result is **0.** If it is then we jump to the **print_flag** function.

I added a breakpoint at this instruction to check the value of eax at the moment of the test.

![Untitled](Pwn%20Acceptance%201b460d4be1b743738146333882584d35/Untitled%205.png)

And passed a 36 bytes cyclic string as input 😈

When the breakpoint hits, RAX’s value seemed familiar ( a cyclic portion).

![Untitled](Pwn%20Acceptance%201b460d4be1b743738146333882584d35/Untitled%206.png)

After identifying the offset, i wrote a python script to send **32** byte string concatenated to **4 \xff bytes** to meet the condition.

![Untitled](Pwn%20Acceptance%201b460d4be1b743738146333882584d35/Untitled%207.png)

```python
from pwn import *

host = "20.169.252.240"
port = "4000"

pattern = cyclic(32)
payload = p32(0xffffffff)

send = pattern+payload
io = remote(host, port)
io.recvuntil('Help him: ')
io.send(send)
print(io.recvall())

"""
offset = cyclic_find(0x61616169)
print(offset)
"""
```

![Untitled](Pwn%20Acceptance%201b460d4be1b743738146333882584d35/Untitled%208.png)

Made By Fckroun with <3