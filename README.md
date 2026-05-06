Md5>
import hashlib

 
text = "Hello World"

 
text_encoded = text.encode()

 
md5_hash = hashlib.md5(text_encoded)

 
result = md5_hash.hexdigest()

print(f"Text: {text}")
print(f"MD5 Hash: {result}")





import struct
import math

 
def left_rotate(x, c):
    return ((x << c) | (x >> (32 - c))) & 0xFFFFFFFF


def md5(message):
 
    message = bytearray(message, 'utf-8')
    original_len_bits = (8 * len(message)) & 0xffffffffffffffff

     
    message.append(0x80)
    while (len(message) * 8) % 512 != 448:
        message.append(0)

    
    message += struct.pack('<Q', original_len_bits)

     
    A = 0x67452301
    B = 0xefcdab89
    C = 0x98badcfe
    D = 0x10325476

    
    K = [int(abs(math.sin(i + 1)) * (2**32)) & 0xFFFFFFFF for i in range(64)]

    
    s = [
        7,12,17,22]*4 + \
        [5,9,14,20]*4 + \
        [4,11,16,23]*4 + \
        [6,10,15,21]*4

    
    for i in range(0, len(message), 64):
        chunk = message[i:i+64]
        M = list(struct.unpack('<16I', chunk))

        a, b, c, d = A, B, C, D

        
        for j in range(64):

            if j < 16:
                F = (b & c) | (~b & d)
                g = j
            elif j < 32:
                F = (d & b) | (~d & c)
                g = (5*j + 1) % 16
            elif j < 48:
                F = b ^ c ^ d
                g = (3*j + 5) % 16
            else:
                F = c ^ (b | ~d)
                g = (7*j) % 16

            temp = (a + F + K[j] + M[g]) & 0xFFFFFFFF
            temp = (left_rotate(temp, s[j]) + b) & 0xFFFFFFFF
 
            a, b, c, d = d, temp, b, c
 
        A = (A + a) & 0xFFFFFFFF
        B = (B + b) & 0xFFFFFFFF
        C = (C + c) & 0xFFFFFFFF
        D = (D + d) & 0xFFFFFFFF
 
    return ''.join('{:02x}'.format(x) for x in struct.pack('<4I', A, B, C, D))


 
msg = input("Enter message: ")
print("MD5:", md5(msg))




-------------------------------------------------
----------------------------------------------

# RSA : user enters only p and q

def is_prime(num):
    if num <= 1:
        return False
    
    for i in range(2, num):
        if num % i == 0:
            return False
    
    return True


# find e automatically gcd(e,ϕ(n))=1
def find_e(phi):
    for e in range(2, phi):
        ok = True
        
        for i in range(2, min(e, phi) + 1):
            if e % i == 0 and phi % i == 0:
                ok = False
                break
        
        if ok:
            return e


# find d automatically
def find_d(e, phi):
    for d in range(1, phi):
        if (d * e) % phi == 1:
            return d


# user input
p = int(input("Enter first prime number: "))
q = int(input("Enter second prime number: "))

if not is_prime(p):
    print("First number is Not Prime")

elif not is_prime(q):
    print("Second number is Not Prime")

else:
    n = p * q
    phi = (p - 1) * (q - 1)

    # auto calculate e and d
    e = find_e(phi)
    d = find_d(e, phi)

    m = int(input("Enter message: "))

    # encryption
    c = (m ** e) % n

    # decryption
    plain = (c ** d) % n

    print("Public Key:", (e, n))
    print("Private Key:", (d, n))
    print("Encrypted Message:", c)
    print("Decrypted Message:", plain)
>>>>
>>>>
I want you to use this code as a reference for your solution.
