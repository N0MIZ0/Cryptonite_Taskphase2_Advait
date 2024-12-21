***Flag:*** <br>
flag
<br>
***Approaching the challenge:*** <br>
Encryption code:
```
from random import randint
import sys


def generator(g, x, p):
    return pow(g, x) % p


def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher


def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True


def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text


def test(plain_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = randint(p-10, p)
    b = randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    semi_cipher = dynamic_xor_encrypt(plain_text, text_key)
    cipher = encrypt(semi_cipher, shared_key)
    print(f'cipher is: {cipher}')


if __name__ == "__main__":
    message = sys.argv[1]
    test(message, "trudeau")
```
With the help of AI and internet searching I was made to understand that the text was encrypted using 2 methods, the Diffie-Hellman Key Exchange in which 2 numbers (a and b)
are used as private keys to encrypt the data, and XOR operation. <br>
The above code has a **generator** function which uses the formula g^x mod p. <br>
Another function isPrime function which tests if the numbers are prime or not, has 2 fixed variables **p** and **g** (97 and 31). <br>
Using the genrator function, u and v are defined and using them shared_key and b_key is also calculated. <br>
After this text is reversed using XOR and key as "trudeau". <br>
Finally the present encrypted data is going through the **encrypt** function where the text is now converted to numbers(ascii) which are then multiplied to 311 and key.
<br>
Cipher tex provided:
```
a = 90
b = 26
cipher is: [61578, 109472, 437888, 6842, 0, 20526, 129998, 526834, 478940, 287364, 0, 567886, 143682, 34210, 465256, 0, 150524, 588412, 6842, 424204, 164208, 184734, 41052, 41052, 116314, 41052, 177892, 348942, 218944, 335258, 177892, 47894, 82104, 116314]
```
So to begin we shall divide them with ```311*key``` to get ascii values which can be converted to texts. <br>
After converting to text we have use XOR on them with the key "trudeau" in reverse. Basically reverse XOR. <br>
Since we know ```a``` and ```b``` values, we can compute the key easily. <br>
With the help of AI, made a function to calculate the key and then use it to find the flag. <br>
Decryption code:
```
def compute_shared_key(a, b, g, p):

    u = pow(g, a, p) 
    v = pow(g, b, p) 


    shared_key = pow(v, a, p)  # v^a % p
    return shared_key

a = 90
b = 26
g = 31
p = 97

shared_key = compute_shared_key(a, b, g, p)
print("Shared key:", shared_key)
cipher = [] # we shall input as per the given file
text_key = "trudeau"

decrypted_message = decrypt(cipher, shared_key, text_key)
print("Decrypted message:", decrypted_message)

```
But this code was not working and had some error in it.  So instead of me modifying anything, I tried the exact code suggested by AI:
```

def compute_shared_key(a, b, g, p):
   
    u = pow(g, a, p) 
    v = pow(g, b, p)  

   
    shared_key = pow(v, a, p)  
    return shared_key


def decrypt(cipher, a, b, g, p, text_key):
   
    shared_key = compute_shared_key(a, b, g, p)
    print(f"Shared key: {shared_key}")


    original_ascii = [num // (shared_key * 311) for num in cipher]

   
    semi_cipher = ''.join(chr(num) for num in original_ascii)

  
    decrypted_text = ""
    key_length = len(text_key)
    for i, char in enumerate(semi_cipher[::-1]): 
        key_char = text_key[i % key_length]
        original_char = chr(ord(char) ^ ord(key_char))  
        decrypted_text += original_char

    return decrypted_text

cipher = [] #input as per the file
a = 90  
b = 26
g = 31  
p = 97  
text_key = "trudeau"  

decrypted_message = decrypt(cipher, a, b, g, p, text_key)
print(f"Decrypted message: {decrypted_message}")

```
