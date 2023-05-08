# Reverse/Simple Checkin

![Untitled](Reverse%20Simple%20Checkin/Untitled.png)

The file is a dynamically linked ELF executable:

![Untitled](Reverse%20Simple%20Checkin/Untitled%201.png)

Downloaded the file and executed it:

It takes my input and throws a ‘Try gain!’ at me 😞

![Untitled](Reverse%20Simple%20Checkin/Untitled%202.png)

It felt like reverse time so i fired Ghidra up.

After decompiling the binary, i found this section to be the most interesting for now, it contains the **Try again!** which is the FAIL message and **Good job!** which is the winnning message.

![Untitled](Reverse%20Simple%20Checkin/Untitled%203.png)

After changing some variable names to make it clearer:

![Untitled](Reverse%20Simple%20Checkin/Untitled%204.png)

I noticed that for the function to print the Good job message, the local_74 variable should no be equal to 0.

Since local_74 is initialized with 1, the XOR condition shouldn’t be met so local_74 remains unmodified.

For that to happen str1 XOR str2 should be equal to str3.

I noticed another section in the decompiled code, containing the initialization of str1 and str3.

![Untitled](Reverse%20Simple%20Checkin/Untitled%205.png)

**…**

![Untitled](Reverse%20Simple%20Checkin/Untitled%206.png)

I decided to reconstruct str1 and str3, XOR them to get str2. For that reason i created this python script:

 

```python
str1 = [125, 172, 168, 5, 239, 30, 215, 140, 148, 31, 235, 155, 42, 182, 128, 58, 57, 57, 95, 147, 95, 112, 70, 225, 185, 60, 59, 118, 44, 72, 174, 106, 215, 75, 162, 191, 193, 238, 186, 100, 187, 187, 183, 182, 198, 73, 224, 179, 35, 133, 88, 161, 13, 243, 33, 170, 204, 74, 2, 46, 176, 212, 140, 73, 61, 165, 126, 142, 21, 217, 65, 152, 239, 144, 52, 169, 15, 124, 76, 69, 123, 110, 149, 58, 128, 205, 123, 226, 159, 60, 204, 106, 248, 103, 119, 243, 168, 85, 101, 21, 167, 135, 203, 137, 29, 207, 209, 27, 121, 231, 125, 92, 240, 251, 9, 74, 69, 7, 116, 205, 127, 152, 11, 170, 4, 14, 112, 34, 17, 156, 114, 62, 124, 134, 28, 33, 110, 24, 78, 48, 59, 15, 42, 38, 111, 184, 142, 76, 127, 34, 239, 99, 163, 194, 200, 99, 216, 2, 136, 25, 91, 120, 122, 57, 140, 191, 195, 25, 93, 135, 154, 217, 162, 234, 127, 253, 208, 249, 182, 247, 110, 173, 175, 93, 216, 202, 41, 143, 181, 15, 189, 206, 248, 157, 203, 24, 160, 143, 206, 48, 119, 95, 138, 235, 10, 115, 218, 158, 97, 127, 81, 32, 105, 16, 105, 241, 38, 158, 125, 247, 6, 10, 176, 95, 34]
str2 = [11, 207, 220, 99, 148, 119, 136, 237, 228, 112, 135, 244, 77, 223, 250, 95, 102, 95, 48, 225, 0, 3, 51, 130, 209, 99, 90, 41, 64, 39, 192, 13, 136, 56, 214, 205, 168, 128, 221, 59, 210, 213, 232, 194, 174, 32, 147, 236, 64, 237, 61, 194, 102, 154, 79, 245, 175, 34, 99, 66, 220, 177, 226, 46, 88, 137, 28, 251, 97, 134, 40, 236, 176, 253, 93, 206, 103, 8, 19, 39, 30, 49, 244, 101, 231, 162, 20, 134, 192, 72, 165, 7, 157, 56, 3, 156, 247, 57, 0, 116, 213, 233, 148, 232, 127, 160, 164, 111, 38, 134, 8, 40, 159, 150, 104, 62, 44, 105, 19, 146, 11, 240, 98, 217, 91, 126, 2, 77, 114, 249, 1, 77, 67, 223, 115, 84, 49, 117, 39, 87, 83, 123, 117, 72, 10, 221, 234, 19, 11, 77, 176, 7, 204, 157, 161, 23, 135, 96, 237, 122, 58, 13, 9, 92, 211, 215, 166, 107, 56, 216, 243, 170, 253, 139, 32, 141, 177, 144, 216, 145, 27, 193, 240, 53, 189, 178, 19, 188, 135, 110, 140, 248, 154, 174, 170, 47, 197, 234, 168, 8, 19, 58, 187, 217, 60, 64, 226, 175, 83, 81, 20, 78, 3, 127, 16, 217, 73, 236, 34, 153, 105, 126, 153, 126, 95]

def xor_arrays(arr1, arr2):
    str3 = []
    for num1, num2 in zip(arr1, arr2):
        result = num1 ^ num2
        str3.append(result)
    string_value = bytes(str3).decode()
    return string_value

print(xor_arrays(str1,str2))
```

And boom flag!!

![Untitled](Reverse%20Simple%20Checkin/Untitled%207.png)

Made By Fckroun with <3