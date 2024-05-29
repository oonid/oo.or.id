---
title: "Learning Rust using Linux"
date: 2022-12-21T22:23:24+07:00
publishDate: 2022-12-21T22:23:24+07:00
image: "/images/2022/Rust_for_Linux_logo.webp"
url: "/content/learning-rust-using-linux"
tags: ["Linux", "Rust"]
draft: false
---

{{< figure src="/images/2022/Rust_for_Linux_logo.webp" title="Rust for Linux Logo" >}}

I think this is really the year to start learning Rust :crab:.
Rust has been named the most loved language for the seventh year in a row, with 87% of developers expressing a desire to continue using it[^1].

As quoted from fireflowers[^2]:
> Rust is a systems programming language that is perhaps most notable for being memory safe without garbage collection.

As a Linux enthusiast, I was thrilled to hear that a pull request for the implementation of Rust in Linux was approved by Torvalds for Linux 6.1[^3][^4].

Where should you start learning Rust?
I believe the official book, "The Rust Programming Language"[^5], available in the Rust documentation[^6], will answer all your questions :wink:.

Additionally, there's something special to note while writing this article: the release[^7] of the "Comprehensive Rust" book by the Android team at Google. You can read it online[^8].

Lastly, try running this code to get a joke from the Rust internal compiler :joy:.
```
fn main() {
break rust;
}
```


[^1]: https://survey.stackoverflow.co/2022#technology
[^2]: https://brson.github.io/fireflowers/
[^3]: https://en.wikipedia.org/wiki/Rust_for_Linux
[^4]: https://www.theregister.com/2022/10/05/rust_kernel_pull_request_pulled/
[^5]: https://doc.rust-lang.org/book/
[^6]: https://www.rust-lang.org/learn
[^7]: https://github.com/google/comprehensive-rust/commit/c212a473ba4d060b209b654c0dd403328d6e137f
[^8]: https://google.github.io/comprehensive-rust/
