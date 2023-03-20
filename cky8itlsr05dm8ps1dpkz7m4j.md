---
title: "Random Prime Number Generator (C++)"
seoTitle: "namespace ra :: Random Prime Generator"
datePublished: Mon Jan 10 2022 10:07:05 GMT+0000 (Coordinated Universal Time)
cuid: cky8itlsr05dm8ps1dpkz7m4j
slug: random-prime-generator
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/iar-afB0QQw/upload/a5845259dc02d788f47a821fb6eaa09f.jpeg
tags: tutorial, cpp, opensource, showhashnode

---

### Aim: 
To create a random number engine that generates prime number on the go! Here the link [GitHub](https://github.com/ra101/Random-Prime-Number-Generator-Engine-cpp) repo


### Plan of Attack:
- Since finding prime number in constant time is impossible by the math at the moment, So We are going to use a database-based approach. Lets store all the prime number in `primeDB` file
- Since `C++` already have [random engines](https://en.cppreference.com/w/cpp/numeric/random), Let us create a wrapper around that using [templates](https://en.cppreference.com/w/cpp/language/templates)

### Objective:
- [Container](https://en.cppreference.com/w/cpp/container) that takes in engine class and override all the engine functions, also takes in filepath for database
- Functions to override
  -  `()` -> returns random prime number
  -  `min()` -> returns min prime number
  -  `max()` -> returns max prime number
  -  `<<` -> save the state to `std::stringstream`
  -  `>>` -> restores the state from `std::stringstream`

### Code:

- Private Members:
 - `template <typename Engine> Engine eng;`
 - `int min_prime;`
 - `int max_prime;`
 - `char const *db_filepath;`


- Public Members:
 - `Constructor`
 - `int operator()();`
 - `friend stringstream &operator<<`
 - `friend stringstream &operator>>`

#### Constructor: takes in `seed` and `filepath`

```C++
#define DB_FILEPATH "primeDB"

template <typename Engine>
random_prime_engine<Engine>::random_prime_engine(unsigned long rand_seed, char const *db_filepath=DB_FILEPATH)
{
    // Get DB path
    this->db_filepath = db_filepath;
    eng.seed(rand_seed);
}
```
```C++
// Usage

ra::random_prime_engine<std::default_random_engine> rpe_2(rand_seed);
```


#### `()` operator,
```C++
#define DB_SIZE 10000

template <typename Engine>
int random_prime_engine<Engine>::operator()()
{
    int idx = eng() % DB_SIZE; // to get a index within file
    int rand_prime;

    // Open the file
    std::ifstream file;
    file.open(db_filepath);

    // update rand_prime till it gets the value at index
    for (int i = 0; i <= idx; i++)
        file >> rand_prime;

    file.close();
    return (rand_prime);
}
```
```C++
// Usage

std::cout << rpe()
```

#### min and max
```python
# pseudo code
# initialize min/max with 0, for lazy initialization

{
    # return min or max if once found
    if bool(min/max):
        return min/max

    min = file.first_line
    max = file.last_line

    return min/max
}
```

#### `>>` and `<<`
```C++
// Friend Functions
#define stringstream SS
function SS &operator<>(SS&in/out, rpe){
    in/out >> rpe.eng;
    return in/out
}
```
```C++
// Usage

std::stringstream state;
state << rpe; //save current state
state >> rpe; //restore old state
```

All the code can be found at the below link:
https://github.com/ra101/Random-Prime-Number-Generator-Engine-cpp