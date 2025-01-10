# Implementing Cryptosystems

## Project Description
This project demonstrates fundamental cryptographic techniques with detailed examples and explanations.

### Cryptosystems Included:
- Caesar Cipher
- Substitution Cipher
- Diffie-Hellman Key Exchange
- RSA Cryptosystem

[View the Project on GitHub](https://github.com/ethereumvd/Implementing_Cryptosystems)

# Implementing Cryptosystems
## Caesar cipher
The caesar cipher is a type of substitution cipher where the letters of the ciphertext alphabet are the plaintext alphabet shifted by an amount, called the key. For example, with a shift of $3$, `D` becomes `A`, `E` becomes `B`, etc.

When encoding a message, every letter of the plain text alphabet is shifted by the key to get the corresponding letter in the ciphertext alphabet. If a letter is not in the plaintext alphabet in a message we are trying to encode, we pass it as is. (for example: digits and spaces)

Let's illustrate this with an example: Consider the message ``HELLO WORLD`` and suppose we choose the key as 3. The corresponding ciphertext is ``KHOOR ZRUOG``.

Decoding is done by just shifting the letters of the ciphertext in the opposite sense. For example: `A` becomes `X`, `B` becomes `Y`, etc. Following this rule, decoding ``KHOOR ZRUOG`` we get our message ``HELLO WORLD`` back.

The shifting can be shown as $E_n(x) \equiv (x + n) \mod{26}$ where $x$ is the index of the character to be encoded and n is the key. Decoding is very similar, just instead of adding $n$, we subtract it. $D_n(x) \equiv (x - n) \mod{26}$

## Substituition cipher
Substituion cipher is a simple encryption scheme where units of the plaintext alphabet are interchanged with the corresponding units in their ciphertext alphabet. For example:

```
plaintext alphabet:   ABCDEFGHIJKLMNOPQRSTUVWXYZ
ciphertext alphabet:  ZEBRASCDFGHIJKLMNOPQTUVWXY
```

Encoding a done by substituting each letter of the message with the corresponding letter in the ciphertext alphabet. For example, ``HELLO WORLD`` will be encoded as ``DAIIL VLOIR``. Decoding is done in the reverse way, by substituting the character in the ciphertext alphabet with the corresponding letter in the plaintext alphabet.

## Diffie Hellman Key Exchange
Diffie–Hellman key exchange is a mathematical method of securely exchanging cryptographic keys over a public channel and was one of the first public-key protocols as conceived by Ralph Merkle and named after Whitfield Diffie and Martin Hellman.

In the Diffie-Hellman key exchange, both parties agree on a large prime number and a base. Each party then selects a private key and calculates a public key based on the agreed prime and base. The public keys are exchanged, and the shared secret key is independently computed using the other party's public key and one's own private key. 

For example, suppose Alice and Bob want to have a shared secret between them. They publicly choose a prime number $p = 23$ and it's base ( a primitive root ) $g = 5$.

Alice chooses a randomly generated secret integer $a$ such that $1 \leq a < p$. Let us take $a=3$ for this example. She sends Bob the value of $A = g^a \mod{p} = 10$.

Bob chooses a randomly generated secret integer $b$ such that $1 \leq b < p$. Again suppose $b=4$ for this example. He sends Alice the value of $B = g^b \mod{p} = 4$.

Alice computes $s = B^a \mod{p} = 18$.<br>
Bob computes $s = A^b \mod{p} = 18$.

Alice and Bob now share a common secret, i.e, the integer $18$.

## RSA Cryptosystem
The RSA cryptosystem, named after its inventors Ron Rivest, Adi Shamir, and Leonard Adleman, is a widely used public-key cryptography algorithm. Introduced in 1977, RSA is known for its security based on the practical difficulty of factoring the product of two large prime numbers.

In RSA, each participant generates a pair of keys: a public key used for encryption and a private key used for decryption. The public key can be freely distributed, allowing anyone to encrypt messages for the key holder, while the private key is kept secret. The security of RSA relies on the complexity of factoring the product of two large prime numbers, making it computationally infeasible to deduce the private key from the public key.

The RSA algorithm involves four steps: key generation, key distribution, encryption, and decryption.

### Key Generation
1. Choose two large prime numbers $p$ and $q$
   - $p$ and $q$ should be chosen at random, be both large (at least 1024 bits) and have a large difference. For choosing them the standard method is to choose random integers and use a primality test (commonly Miller-Rabin) until two primes are found.
   - $p$ and $q$ are kept secret.
2. Compute the modulus $n = pq$
   - The length of $n$ in bits is called the key length
   - $n$ is released as part of the public key.
3. Compute $\phi(n)=(p-1)\cdot(q-1)$
   - $\phi(n)$ is called the Euler's Totient Function and is defined as the number of positive integers which are less than co-prime to $n$.
   - $\phi(n)$ must be kept secret.
4. An integer $e$ is chosen such that $\gcd(e,\phi(n)) = 1$ and $2 < e < \phi(n)$.
   - Most commonly a value of $e = 65537$ is chosen.
   - It is released as part of the public key.
5. $d = e^{-1} \mod{\phi(n)}$ is computed using the Extended Euclidean algorithm.
   - $d$ is kept secret as part of the private key.

The public key consists of the pair of integers $(e,n)$ and the private key consists of the integers $(d,n)$. $p$, $q$ and $\phi(n)$ must all ke kept secret, and can even be discarded after $d$ has been calculated. 

### Key Distribution
Suppose that Bob wants to send information to Alice. If they decide to use RSA, Bob must know Alice's public key to encrypt the message, and Alice must use her private key to decrypt the message.

To enable Bob to send his encrypted messages, Alice transmits her public key $(e, n)$ to Bob via a reliable, but not necessarily secret, route. Alice's private key $d$ is never distributed.

### Encryption
After Bob obtains Alice's public key, he can send a message $m$ to Alice. 
To do this, he computes the following: $$c = m^e \mod{n}$$
where $e$ is the public exponent in Alice's public key. This is the ciphertext.

### Decryption
Upon receiving this message by Bob, Alice uses her private exponent $d$, to decrypt it.
$$c^d \equiv \left(m^{e}\right)^{d} \equiv m^{e \cdot d} \equiv m \mod{n}$$ 

### Example
1. Choose $p=31$ and $q=29$
2. Compute $n=pq=31\cdot 29=899$
3. Compute $\phi(n) = (p-q)\cdot (q-1) = 840$
4. Choose any number $e$ such that $2 < e < \phi(n)$, suppose $e = 23$.
5. Compute $d$, the modular multiplicative inverse using the Extended Euclidean Algorithm. $d = 767$
6. Suppose we want to encrypt $m = 45$, $c = m^e = 45^{23} \equiv 111 \mod{899}$
7. To decrypt, $c^d \equiv 111^{767} \equiv 45 \mod{899}$

