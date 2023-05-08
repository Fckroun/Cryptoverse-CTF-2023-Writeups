# Crypto/Baby AES

![Untitled](Crypto%20Baby%20AES/Untitled.png)

The challenge provided us with a python script:

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
from secret import flag

KEY_LEN = 2
BS = 16
key = pad(open("/dev/urandom","rb").read(KEY_LEN), BS)
iv =  open("/dev/urandom","rb").read(BS)

cipher = AES.new(key, AES.MODE_CBC, iv)
ct = cipher.encrypt(pad(flag, 16))

print(f"iv = {iv.hex()}")
print(f"ct = {ct.hex()}")

# Output:
# iv = 1df49bc50bc2432bd336b4609f2104f7
# ct = a40c6502436e3a21dd63c1553e4816967a75dfc0c7b90328f00af93f0094ed62
```

This script encrypts the **whole** flag using AES with CBC mode and prints the iv and ct (the cipher text).

![Untitled](Crypto%20Baby%20AES/Untitled%201.png)

Normally to decrypt the cipher text, we need the iv and the used key. 

![Untitled](Crypto%20Baby%20AES/Untitled%202.png)

But in our case :

- The key is too short ( only 2 bytes in length ) ⇒ bruteforceable
- We know that wour flag starts with **cvctf{.**

 So, i decided to bruteforce the key**.**

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import itertools
ct = bytes.fromhex('a40c6502436e3a21dd63c1553e4816967a75dfc0c7b90328f00af93f0094ed62')
iv = bytes.fromhex('1df49bc50bc2432bd336b4609f2104f7')
target_plaintext = b'cvctf{'  # The plaintext you want to find
for key in itertools.product(range(256), repeat=2):
    key = bytes(key)
    cipher = AES.new(pad(key,16), AES.MODE_CBC, iv)
    pt = cipher.decrypt(ct)
    if pt.find(target_plaintext) != -1 :
        print(f"Found key: {key.hex()}")
        break
else:
    print("Key not found.")
```

![Untitled](Crypto%20Baby%20AES/Untitled%203.png)

I finally modified the script to decrypt ct and get the flag.

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import itertools
ct = bytes.fromhex('a40c6502436e3a21dd63c1553e4816967a75dfc0c7b90328f00af93f0094ed62')
iv = bytes.fromhex('1df49bc50bc2432bd336b4609f2104f7')
key = bytes.fromhex('a5cf')
cipher = AES.new(pad(key,16), AES.MODE_CBC, iv)
pt = cipher.decrypt(ct)
print(pt)
```

![Untitled](Crypto%20Baby%20AES/Untitled%204.png)

Made By Fckroun with <3