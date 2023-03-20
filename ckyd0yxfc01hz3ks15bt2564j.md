---
title: "Mathematics of RSA Encryption Algorithm"
seoTitle: "RSA Encryption"
seoDescription: "RSA Explained in Brief."
datePublished: Thu Jan 13 2022 13:46:11 GMT+0000 (Coordinated Universal Time)
cuid: ckyd0yxfc01hz3ks15bt2564j
slug: rsa-encryption-algorithm
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/YriuRFYUP0I/upload/009b857b851c71c709e0067ed35b05ca.jpeg
tags: algorithms, encryption, cryptography, mathematics

---

### What is RSA?
The Rivest-Shamir-Adleman (RSA) is an asymmetric encryption algorithm. Asymmetric Encryption is when a box(data) can be locked by one key and unlocked by other. locking key cannot unlock the box and vice-versa. The locking key is the public key and the unlocking key is the private key, Public key is called so, because it is publicly distributed, so anyone/everyone would send the user an encrypted msg, and the user can unlock it in private with the private key.
By keeping private to the user-self, no hacker can get a method of unlocking it.

****

### How Asymmetric Encryption is achieved?
We need a mathematical function such that
><center> `f`: public key function , `g`: private key function
<br>
> `f`<sup>`-1`</sup>`(f(msg)) != msg`
<br>
> `g(g`<sup>`-1`</sup>`(msg)) != msg`
<br>
> `g(f(msg)) == msg`</center>

****

### Signing Document via Asymmetric Encryption:
Digital Signature is the by-product of Asymmetric Encryption, In most asymmetric encryption algorithms (including RSA), public and private are just names, one can simply interchange them for locking and unlocking. Here is how to sign a document:

**How to sign:**
- Step 1: Calculate hash of the document using an unbreakable and popular hashing algorithm (for eg. SHA-256)
- Step 2: Make `digest` by performing the decryption function with the private key on that hash.
- Step 3: Send the document, digest, and hashing algorithm to the world.

**How to Verify:**
- Step 1: Calculate the hash of the document using the provided hashing algorithm, let's call it the "calculated hash".
- Step 2: Get the hash by performing the encryption function with the user's public key on that digest,  let's call it the "given hash".
- Step 3: If "calculated hash" matches with the "given hash" then the document is authentic else not.

****

### Mathematics of RSA:
for `g` and `f` should be such that `g` must relatively harder to perform so that it could not be cracked by brute force.

First, let's get this straight. Every ~~string~~ data can be represented as numbers or chunks of numbers, which can happen via ASCII or change it to binary then to decimal. So mathematical operation can be used on any and every data.

Consider `m` as the message. say m is 10, now we have to find 2 functions, one that will change it some else and the other that can change back!


> Multiplication/Division:
>> <center>let `f(key, m) = key * m`</center>
> so now we have to distribute `f` and `key` but same key can be used to change it back!

> Addition/Subtraction:
>> <center>let `f(key, m) = key + m`</center>
> so now we have to distribute `f` and `key` but same key can be used to change it back!

> Trigonometry:
>> <center> let `f(..., m) = sin(... m)` </center>
> Here the problem is the decimal nature of output, can have different result at different machines!

<br>

> Modulus:
> For those who are new to modulus function, it is the reminder of integer division.
>> <center> `x mod y` => `ky - x` where `ky` > `x` </center>
> for example: `7/5 = 1` and `2 = 7 mod 5`

> Here is the catch, for
>> <center>`2 = m mod 5`
>> `m` can be `2, 7, 12... 5*(x) + 2`</center>
>> So a hacker with `2` (encrypted msg) and `5` cannot decode the `m`, but so can't we.

> Let us try one more trick
>> <center> `3`<sup>`m`</sup>` mod 5 = x`<br>
>> for if `m` is `0, 1, 2, 3, 4 ...` x is `1, 3, 4, 2, 1 ...` respectively, `m` is uniformly distributed in 1 to 4.</center>
>> So a hacker with `x` (encrypted msg), `5` and `3` cannot decode the `m`, but still so can't we. Another major point to be noted is, if `3 < 5` exists, then `x == 3` for some `m` and `x < 5` exists always.

> Now Let us merge both tricks.
>> <center> `m`<sup>`e`</sup>` mod n = x`, `m < n`    --> eq 1<br>
>> `m`: message, `e`, `n`: some constants, `x`: encrypted message</center>
>> Let there be another equation
>> <center> `x`<sup>`d`</sup>` mod n = m`    --> eq 2<br>
>> `x`: encrypted message, `d`, `n`: some constants, `m`: message</center><br>
>> Let us substitute `x` in `eq 2` with `eq 1`<br>
>> <center> `(m`<sup>`d`</sup>` mod n)`<sup>`e`</sup>` mod n = m`<br>
>> `m`<sup>`e * d`</sup>` mod n = m`<br>
>> `m`: message, `e`, `d`, `n`: some constants, `x`: encrypted message</center>
>> in other words: `e` -> `public_key` and `d` -> `private_key`
>> All we need is do is define those constants and we are good to go! 


> Now Lets take a dive into history,
>> Introducing the `Totient` function aka Euler's phi function
> <center> `phi(n) = m`, `m` is the <b>count</b> of integers b/w `[1, n]`, which are coprime(relatively prime) to `n`</center><br>
> for example, `phi(8) = 2` as, `gcd(6, n) == 1` holds true only for `1` and `5`.
> - Interestingly enough, `phi(prime)` is always `prime - 1`, as all the numbers before prime gave their `gcd` as `1`, given the definition of prime is exactly that.
> - Another Observation is `phi(p1 * p2) = phi(p1) * phi(p2)`

> Let's cook with the ingredients we have.
>> - `phi(n) = Set of all the co-primes`
>> - `3`<sup>`e`</sup>` mod 5 = x`,<br> for if `e` is `0, 1, 2, 3, 4 ...` x is `1, 3, 4, 2, 1 ...` respectively, `e` is **uniformly distributed** in 1 to 4.
>> - Let there be `a`<sup>`k`</sup>, such that `a`<sup>`k`</sup>` = 1 mod n`
>> - Since, is `k` one of the uniformly distributed power, it must be within `phi(n)`, therefore `phi(n) = kc`, there `c` is some constant.
>> - Now if raise both side with power for `c`
>> - It becomes: `a`<sup>`kc`</sup>` = a`<sup>`phi(n)`</sup>` = 1 mod n`
>> This is `Fermat–Euler Theorem`

> Merge both the discoveries:
>> <center> `a * a`<sup>`phi(n)`</sup>` mod n = a`<sup>`ed`</sup>` mod n` <br>
>> `a * a`<sup>`phi(n) * c`</sup>` = a`<sup>`ed`</sup>` mod n`<br>
>> `(phi(n) * c) + 1 = e * d`</center><br>
>> if `n` is product of big prime number then phi(n) easy to calculate and `n` itself hard to find (prime factorization would take a lot of time)
>> <center> `d = (c * phi(n) + 1) / e`<br>
>> where `c` is calculated such that `d` becomes an integer
>> `(n, e)` : public key, `(n, d)`: private key</center>  

****

### Bibliography / Resources:

**RSA Brief:** https://www.youtube.com/watch?v=wXB-V_Keiu8

**Khan Academy:** https://www.khanacademy.org/computing/computer-science/cryptography/modern-crypt/v/intro-to-rsa-encryption

**Proof of Fermat–Euler Theorem:** https://www.youtube.com/watch?v=5pswKNgVZSg

**RSA SIgning:** https://www.cs.cornell.edu/courses/cs5430/2015sp/notes/rsa_sign_vs_dec.php
