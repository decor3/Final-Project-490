RSA Excel Final Project
- The purpose of this project is to portray the security that you get when using the RSA Algorithm.
I used Python to add from a template that i found to get started on. My contribution was changing most
of the code so that I could get it to register in blocks instead of just in regular character by character
which I found out is not very secure at all. My project can be a little slow to run but it might just be based
off of how fast your computer is or the amount of words that you type into the program.

To run this program.
1. Hit Run
2. Select whether you want to generate a new private or public key with y or n
3. Select whether you want to encrypt with e or d.
4. If encrypting type your sentence, if decrypting type the numbers you got from your encryption
5. If encrypting, choose whether you want to encrypt from my file or if you have your own file.
6. Write down your key and end the program.

*******************************************************************
import random

def gcd(a, b):
    while b != 0:
        a, b = b, a % b
    return a

def multiplicative_inverse(e, phi):
    d = 0
    x1 = 0
    x2 = 1
    y1 = 1
    temp_phi = phi

    while e > 0:
        temp1 = temp_phi//e
        temp2 = temp_phi - temp1 * e
        temp_phi = e
        e = temp2

        x = x2 - temp1 * x1
        y = d - temp1 * y1

        x2 = x1
        x1 = x
        d = y1
        y1 = y

    if temp_phi == 1:
        return d + phi


def is_prime(num):
    if num == 2:
        return True
    if num < 2 or num % 2 == 0:
        return False
    for n in range(3, int(num**0.5)+2, 2):
        if num % n == 0:
            return False
    return True


def generate_key_pair(p, q):
    if not (is_prime(p) and is_prime(q)):
        raise ValueError('Both numbers must be prime.')
    elif p == q:
        raise ValueError('p and q cannot be equal')
  
    n = p * q

    phi = (p-1) * (q-1)

    e = random.randrange(1, phi)

   
    g = gcd(e, phi)
    while g != 1:
        e = random.randrange(1, phi)
        g = gcd(e, phi)

 
    d = multiplicative_inverse(e, phi)


    return ((e, n), (d, n))


def encrypt(pk, plaintext):
    key, n = pk
    cipher = [pow(ord(str), key, n) for str in plaintext]
    return cipher


def decrypt(pk, ciphertext):
    key, n = pk
    aux = [str(pow(char, key, n)) for char in ciphertext]    
    plain = [chr(int(str)) for str in aux]
    return ''.join(plain)


if __name__ == '__main__':
    '''
    Detect if the script is being run directly by the user
    '''
  
    print("RSA Encryptor / Decrypter ")
    print(" ")

    p = int(input(" - Enter a prime number (17, 19, 23, etc): "))
    q = int(input(" - Enter another prime number (Not one you entered above): "))

    print(" - Generating your public / private key-pairs now . . .")

    public, private = generate_key_pair(p, q)

    print(" - Your public key is ", public, " and your private key is ", private)

    message = input(" - Enter a message to encrypt with your public key: ")
    encrypted_msg = encrypt(public, message)

    print(" - Your encrypted message is: ", ''.join(map(lambda x: str(x), encrypted_msg)))
    print(" - Decrypting message with private key ", private, " . . .")
    print(" - Your message is: ", decrypt(private, encrypted_msg))

    print(" ")
    print("============================================ END ==========================================================")
    print("===========================================================================================================")
    *****************************************************************************************
    This is the template code that I used for this Project. I had to look at many different sources to try and figure out a way to try and get it to encrpyt in blocks instead of characters.
