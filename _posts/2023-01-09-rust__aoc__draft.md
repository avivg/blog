---
title: One and a Half Advent of Codes Using Rust
date: 2023-01-09 09:30 +02:00
tags: [Rust, AdventOfCode]
---
## Procrastination at its Best

[Advent of Code](https://adventofcode.com/) is a fun annual game, played in december, where each day a programming challenge is published and the compatitors try to solve it as fast as possible.

Many compatitors don't actually go for speed, but use it as an opportunity to sharpen their skills or learn a new language.

I found out about this game in the middle of Nov. 2022 and immediately realized I want to use it as a Rust beginners exercise. I started working on [2021's](https://github.com/avivg/aoc-2021) puzzles while waiting for [2022](https://github.com/avivg/aoc-2022) to start and then continue working in parallel until I ran out of time for both.

Each puzzle consists of 2 parts, where the 2nd part is only available after solving the first part. Each part solved awards a start. I scored a total of 76 stars at the time of writing this post. Currently I ran out of time to work on it, so for the forseeable future, this is my score.

## Thoughts about Rust

1. There are some serious adjustments for a C++ programmer, but it's very do-able and after investing a week or two, it feels very natural.
2. After said adjustment, it became much easier for me to implement the puzzle solutions I came up with using Rust, compared to implementing the same solutions with C++.
  * One reason is the true functional constructs, compared to my familiar C++17 with it's semi-functional-always-requires-begin-end concepts.
  * Another important reason is 'Cargo'. It is priceless to work with such a convenient and transparent package-manager/build-system.
  * The third reason is the rust-analyzer VSCode extension and the super friendly compiler errors of rustc.
3. After spending so much time writing rust, going back to work with C++17 felt like such a downgrade. I really feel after 2-3 weeks of rust that I have all the tools to build my next project with it. I started looking into contributing to FOSS projects just to keep the momentum, but life and work may not really permit that kind of time investment.
4. The performance difference between debug and release is enormous. That optimizer is really leaving up to its name.
5. The number of resources for learning and asking questions is also surprisingly large for a "novel" language such as this. I used Mastodon, [reddit](reddit.com/r/rust), [doc.rust-lang.org] and [Stack Overflow](https://stackoverflow.com/questions/tagged/rust), and it still feels like there are may other user forums and resources to learn and find answers.
6. I also encountered a lot of AoC participants that used Rust and published it on Mastodon. For a relativeley small network like mastodon, with the relatively marginal game such as AoC, I was surprised to find more rustaceans. Feels like this language is really up and coming. Here's hoping...
