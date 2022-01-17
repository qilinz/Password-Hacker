# Password Hacker
Create a password hacker using brute force and dictionary-based brute force.

## How to use
- Run [hacker.py](https://github.com/qilinz/Password-Hacker/blob/main/hacker.py) with arguments of which server to connect. e.g. `python hacker.py localhost 8080`

## How the program works
1. Find the correct username using [logins.txt](https://github.com/qilinz/Password-Hacker/blob/main/logins.txt).
   1. If username is correct, the response info will be "Wrong password".
   2. If username is wrong, the response info will be "Wrong login"
2. Find the correct password through brute force. 
   1. If the first character of the password is correct, the response will have a delay, which means the execution time will be much longer. Therefore, if a delay is detected, a correct character is found.
   2. The server to hack only uses numbers and alphabets(lowercase and uppercase) for password
   
## Other ways to find password
Define character available:
```
NUMBERS = "0123456789"
CHARS_LOWER = "abcdefghijklmnopqrstuvwxyz"
CHARS_UPPER = CHARS_LOWER.upper()
```
### Brute force
``` 
CHARACTERS = CHARS_LOWER + NUMBERS
LEN_CHAR = len(CHARACTERS)


def brute_force():
    for i in range(LEN_CHAR):
        pw_iter = itertools.product(CHARACTERS, repeat=i+1)

        for pw in pw_iter:
            pw_string = "".join(list(pw))
            
```
The outer loop tests all possible length of passwords. Once the length is decided, an iterator is created using `itertools.product()`. The inner loop tests all the possible combination in the iterator, changing a list of characters to a string.
### Dictionary-based brute force
In this method, a dictionary of the most common passwords included in [passwords.txt](https://github.com/qilinz/Password-Hacker/blob/main/passwords.txt) is used.

Passwords in the dictionary are checked one by one with different combinations of lowercase and uppercase. 
```
def dict_brute_force():
    with open("passwords.txt") as file:
        passwords = file.read().splitlines()
        for password in passwords:
            pw_iter = itertools.product(*([letter.lower(), letter.upper()] for letter in password))
            for pw in pw_iter:
                pw_string = "".join(pw)

```
The idea of the iterator `pw_iter` is to create all the different combinations of uppercase and lowercase of a word.
- Take `apple` as an example. 
- `([letter.lower(), letter.upper()] for letter in password)` will return `(["a", "A"], ["p", "P"], ,["p", "P"], ["l", "L"], ["e", "E"])`
- By placing a `*` before it, the contents in the tuple will be returned without "(" and ")".
- Thus, `itertools.product(["a", "A"], ["p", "P"], ,["p", "P"], ["l", "L"], ["e", "E"])` will find all the possible combinations.

Disclaimer: The original project idea is from [JetBrains Academy](https://hyperskill.org/projects/80?track=2). All codes were written by myself.