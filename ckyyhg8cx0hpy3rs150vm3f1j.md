---
title: "Implementing RSA Cryptography (C++)"
seoTitle: "namespace ra :: RSA Cryptography"
seoDescription: "Creating working RSA code. Here is the link GitHub repo!"
datePublished: Fri Jan 28 2022 14:10:41 GMT+0000 (Coordinated Universal Time)
cuid: ckyyhg8cx0hpy3rs150vm3f1j
slug: rsa-cpp
canonical: https://ra101.github.io/RSA-cpp/
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/FnA5pAzqhMM/upload/a29d85147226ce679bbef4ac034006c2.jpeg
tags: cpp, opensource, cryptography, showhashnode

---

### Aim:

To create a working RSA code, Using [`namespace ra::random_prime_engine`](https://ra101.hashnode.dev/random-prime-generator), Here the link [GitHub](https://github.com/ra101/RSA-cpp) repo!

### What is RSA?

The Rivest-Shamir-Adleman (RSA) is an asymmetric encryption algorithm. Asymmetric Encryption is when a box(data) can be locked by one key and unlocked by other. locking key cannot unlock the box and vice-versa. The locking key is the public key and the unlocking key is the private key, Public key is called so because we publicly distribute it, so anyone/everyone would send the user an encrypted msg, and the user can unlock it in private with the private key.
By keeping private to the user-self, no hacker can get a method of unlocking it.

#### How RSA Works?

https://ra101.hashnode.dev/rsa-encryption-algorithm


### Code:

- Private Members:
 - `private_key`
 - `public_key`
 - `create_key(long seed)`


- Public Members:
 - `Constructor` & `Constructor(seed)`
 - `std::size_t decrypt(std::size_t);`
 - `std::size_t decrypt_with_padding(std::string);`
 - `Key Getters`
 - `std::size_t sign(Message message)`


- Other Function within `namespace` but Outside `class` _(All the functions utilizing public key)_:
 - `std::size_t encrypt(std::size_t digest, public_key);`
 - `std::string encrypt_with_padding(std::size_t digest, public_key);`
 - `bool verify(Message message, size_t digest, public_key)`
 - `string padding(string)`


#### Constructors:

The seed in the constructor is used in `random prime engine` and the logic of the seed argument is as follow,

```python
# pseudo code implemented in python
def constructor(seed=None):
    if not seed:
        seed = timestamp()
    else:
        seed = hash(seed)
   return create_key(seed)
```

```C++
// Usage

ra::rsa_key_pair k1, k2("127.0.0.1");
```


#### `create_key(seed)` function,

I will not get into the mathematics of it all, but in the end, using 2 primary numbers we get
- public key: (n, e)
- private key: (n, d)

(n, e, d) all are integers.

To concatenate these two numbers (n, e/d) let us define a big number `BIG_NO`, such that 
- n = key / BIG_NO
- e = public_key % BIG_NO
- d = private_key % BIG_NO

```C++
#define BIG_NO 100000000000

public_key = n * BIG_NO + e;
private_key = n * BIG_NO + d;
```

#### `encrypt(digest, key)` function,
```python
return (digest ** e) % n
```

#### `decrypt(encrypted_data)` function,
```python
return (encrypted_data ** d) % n
```

#### `padding(string str)` function,
```C++
#define PAD_SIZE 10

// 12345 -> 0000012345
while (str.length() < PAD_SIZE)
{
    str = '0' + str; // add 0 in front of no.
}
return str;
```


#### `encrypt_with_padding(digest, key)` function,
```python
# encrypt each digit
for each_digit in digest:
    output = padding(encrypt(each_digit)) + output
return output
```

#### `decrypt_with_padding(encrypted_data)` function,
```python
# cut the input string in chunks of length PAD_SIZE
# decrypt each chunk to digit and then form a output
for i_chunk in len(encrypted_data)/PAD_SIZE:
    i = i_chunk * PAD_SIZE
    digit = decrypt(int(encrypted_data[i: i + PAD_SIZE]))
    output = digit + output * 10
return output
```

#### `sign(Message message)` function,
```python
return decrypt(hash(message))
```

#### `verify(Message message, digest, others_public_key)` function,
```python
return ( hash(message) == encrypt(digest, others_public_key) )
```

All the code can be found at the below link: https://github.com/ra101/RSA-cpp