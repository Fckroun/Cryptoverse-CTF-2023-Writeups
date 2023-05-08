# Misc/Baby calculator

![Untitled](Misc%20Baby%20calculator/Untitled.png)

Connecting with netcat on the challenge’s port, i was welcomed with a small description.

![Untitled](Misc%20Baby%20calculator/Untitled%201.png)

As i understood from the description, i should solve 40 intergrals in one minute with a precision of 1e-6. (It was dropped later to 1e-3) 

I first made a function that takes the full line:

```python
Evaluate the integral of: 6/x-3*sin(x)+8*x-3/x+8/x from 1 to 8.
```

parses it and extracts:

- The equation
- The lower limit
- The upper limit

```python

import numpy as np
from scipy.integrate import quad
import re
from numpy import cos,sin

def calc(string):
    def evaluate_integral(expression):
        pattern = r'Evaluate the integral of: (.*) from (\d+) to (\d+)\.'
        matches = re.search(pattern, expression)  
        if matches:
            integrand_str = matches.group(1)
            lower_limit = float(matches.group(2))
            upper_limit = float(matches.group(3))
        else:
            return None
        def integrand(x):
            return eval(integrand_str)
        integral_value, error = quad(integrand, lower_limit, upper_limit) 
        return integral_value
    expression = string
    result = evaluate_integral(expression)
    return result

print(calc("Evaluate the integral of: -10/x-5*x-6/x from 3 to 7."))
```

I tested it and it’s working perfectly.

![Untitled](Misc%20Baby%20calculator/Untitled%202.png)

Now i should automate it with socket or pwntools

```python
from pwn import *
import numpy as np
from scipy.integrate import quad
import re
from numpy import cos,sin

def calc(string):
    def evaluate_integral(expression):
        pattern = r'Evaluate the integral of: (.*) from (\d+) to (\d+)\.'
        matches = re.search(pattern, expression)  
        if matches:
            integrand_str = matches.group(1)
            lower_limit = float(matches.group(2))
            upper_limit = float(matches.group(3))
        else:
            return None
        def integrand(x):
            return eval(integrand_str)
        integral_value, error = quad(integrand, lower_limit, upper_limit) 
        return integral_value
    expression = string
    result = evaluate_integral(expression)
    if result is not None:
        print("The integral value is:", result)
    else:
        print("Invalid expression.")
    return result

def send_and_receive(host, port, repetitions):
    r = remote(host, port)
    for _ in range(repetitions):
        received_output =  r.recv(1024)
        print('Received:', received_output)
        string = [i for i in received_output.decode('utf-8').split('\n') if 'Evaluate' in i][0]
        x = str(str(calc(string))+'\n').encode('utf-8')
        print('THIIIIIIIIS',x)
        r.send(x)
    r.close()

host = '20.169.252.240'
port = 4200
repetitions = 41

send_and_receive(host, port, repetitions)
#print(calc("Evaluate the integral of: -10/x-5*x-6/x from 3 to 7."))
```

After 40 successefull answers, i got the flag:

![Untitled](Misc%20Baby%20calculator/Untitled%203.png)

Made By Fckroun with <3