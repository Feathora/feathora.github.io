---
layout: post
title:  "Advent of Code"
summary: "An advent calendar, 25 days before Christmas, with two puzzles per day, only solvable with code. Incredibly fun, and a great opportunity to challenge yourself in new ways, for example by using a language you've never used before."
author: Vincent
date: '2019-05-22 14:35:23 +0530'
category: personal
thumbnail: /assets/img/posts/adventofcode.webp
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes
permalink: /projects/personal/advent-of-code/
usemathjax: true
---


The [Advent of Code](https://adventofcode.com) is a fun little challenge I like to participate in when I have time. It always runs from December 1st until Christmas Day, providing you with two puzzles per day. You're allowed to use any language or framework you want, but you cannot solve the puzzles by hand, due to the sheer size of the puzzle input. Each pair of puzzles requires a certain technique, for example some kind of pathfinding algorithm. You can often get away with using some kind of brute-force technique on the first puzzle. But then for the second one, they might for example ask you to run a billion iterations, forcing you to really find a proper solution and optimise your code.

The further you get, the more time it tends to take to solve the puzzles, so sadly with December already being a very busy month, I've only been able to finish the whole Advent of Code once. But it's fun to try anyway.

# 2021

In 2021, I wanted to focus on writing short, clean C#, and to hopefully experiment a little with some new features in the language. You can find the source code for this one on my [Github](https://github.com/Feathora/AdventOfCode2021)

# 2020

Word had been going around about a new language called Rust, so I figured the Advent of Code would be a great way to try it out. You can find it on my [Github](https://github.com/Feathora/AdventOfCode2020)

It's a very interesting language. It's a low-level language that can achieve the same performance and memory-efficiency as C++, but it is completely memory-safe and thread-safe, due to its ownership system in which the lifetime and ownership of variables is determined at compile-time. 

Working in Rust on these puzzles was definitely interesting, because it forced me to think about my code in a different way. For example, in most languages, variables are exactly that, variable. Meaning they can change. In Rust, they're 'const' by default, you need to explicitly declare them as 'mutable' if you want to be able to change them.

```rust
fn puzzle1(passwords:&Vec<Password>) -> u32
{
    let mut result = 0;
    for password in passwords
    {
        let count = password.pass.matches(password.req).count();
        if count >= password.min.try_into().unwrap() && count <= password.max.try_into().unwrap()
        {
            result += 1;
        }
    }

    return result;
}
```

# 2017, 2018 and 2019

For small puzzles and scripts such as these, Python is my go-to language, because it allows me to focus on the challenge itself, instead of having to think about classes and variable declaration and memory management, for example. As soon as the code gets too long however, or split into multiple files, I like to switch to a language with a stronger type system such as C#.

As a small example, here's my solution to the second puzzle of day 10, 2017.

```python
def round(hash, lengths, position, skip):
    for length in lengths:

        for i in range(length // 2):
            left = (position + i) % len(hash)
            right = (position + length - i - 1) % len(hash)

            temp = hash[left]
            hash[left] = hash[right]
            hash[right] = temp

        position = (position + length + skip) % len(hash)
        skip += 1

    return hash, position, skip

def solve(input):
    lengths = []
    for c in input:
        lengths.append(ord(c))

    lengths += [17, 31, 73, 47, 23]

    hash = list(range(256))
    position = 0
    skip = 0

    for i in range(64):
        hash, position, skip = round(hash, lengths, position, skip)

    result = []
    for i in range(0, len(hash), 16):
        char = 0
        for x in range(i, i + 16):
            char ^= hash[x]

        result.append(char)

    output = ""
    for i in result:
        output += "%0.2x" % i

    print(output)

solve("34,88,2,222,254,93,150,0,199,255,39,32,137,136,1,167")
```

The code for these three years can be found on my [Github](https://github.com/Feathora/AdventOfCode). I managed to solve all the puzzles in 2017, but sadly not in 2018 or 2019, due to time limitations.

