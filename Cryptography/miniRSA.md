***Flag:*** <br>
picoCTF{n33d_a_lArg3r_e_606ce004} <br>
***Approaching the challenge:*** <br>
Ciphertext provided:
```

N: 29331922499794985782735976045591164936683059380558950386560160105740343201513369939006307531165922708949619162698623675349030430859547825708994708321803705309459438099340427770580064400911431856656901982789948285309956111848686906152664473350940486507451771223435835260168971210087470894448460745593956840586530527915802541450092946574694809584880896601317519794442862977471129319781313161842056501715040555964011899589002863730868679527184420789010551475067862907739054966183120621407246398518098981106431219207697870293412176440482900183550467375190239898455201170831410460483829448603477361305838743852756938687673
e: 3

ciphertext (c): 2205316413931134031074603746928247799030155221252519872649649212867614751848436763801274360463406171277838056821437115883619169702963504606017565783537203207707757768473109845162808575425972525116337319108047893250549462147185741761825125 
```
After going through the hint (wikipedia page of RSA) I have understood the following about RSA cryptosystem.
RSA is a cryptosystem where 2 random numbers (large distinct prime) are taken, after few calculations we end up with 2 keys - public and private key. <br>
The message which has to be encrypted uses the formula CipherText = M^e (mod n)
<br>
Hence respectively to decrypt it we shall use M = C^d (mod n) <br>
**e** and **d** are public and private keys respectively. <br>
**N (n)** is the multiple of 2 distinct prime numbers. <br>
The second hint emphasizes on the small size of **e**. <br>
After browsing more I found out that if **e** is very small the C = M^e, hence cub root of C is the message.
<br> So I wrote a simple cube root program to find the cube root of the cipher text:
```
def integer_cube_root(c):
    """
    Find the integer cube root of c using binary search.
    """
    low, high = 0, c
    while low < high:
        mid = (low + high) // 2
        if mid**3 < c:
            low = mid + 1
        else:
            high = mid
    return low

C=2205316413931134031074603746928247799030155221252519872649649212867614751848436763801274360463406171277838056821437115883619169702963504606017565783537203207707757768473109845162808575425972525116337319108047893250549462147185741761825125
M = integer_cube_root(C)
print("Cube root of C:", M)
```
The cuberoot of ciphertext came out as:
```
13016382529449106065894479374027604750406953699090365388202874238148389207291005
```
With the help of AI, I figured out that this cuberoot was encoded in base 256 format (group of 8 bits represent one character), after extracting these bytes we need to 
convert them to their respective ascii for appropriate flag.
<br>
Using following code:
```
def integer_to_string(m):
  
    result = bytearray()
    while m > 0:
        result.append(m % 256) 
        m //= 256               
    
    # Convert bytearray to string
    try:
        return bytes(result[::-1]).decode('utf-8') 
    except UnicodeDecodeError:
        return "Error " + result[::-1].hex()


M = 13016382529449106065894479374027604750406953699090365388202874238148389207291005 # This is the cube root which we figured out in the previous step


plaintext = integer_to_string(M)
print("Plaintext (flag):", plaintext)
```
As expected and gussed this was the right approach and I got the flag!
