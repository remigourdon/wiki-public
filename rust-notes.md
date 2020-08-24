# Rust notes

Some notes on Rust about advantages/disadvantages and tradeoffs when considering Rust for production.

## My notes

+ built to last
+ fast
+ safe (less debugging)
+ reduce costs (memory usage, developer time)
+ increase developer morale -> increase productivity
+ need for several people to be trained on (contigencies, multiple persons should be able to maintain)
+ recruiting directly through the communicty could be invaluable (Reddit, Discord)
+ enthusiasm (this is important in a company!)
  + it's like giving a tech bonus to make developers happy instead of a cash one
  + https://stackoverflow.blog/2020/06/05/why-the-developers-who-use-rust-love-it-so-much/
  + https://www.reddit.com/r/rust/comments/hj66pc/convincing_my_company_to_consider_rust_in_the/fwm1zjz?utm_source=share&utm_medium=web2x
+ people don't want to learn a new language, but we don't have people yet!
+ interoperable with C (FFI) and C++ to some extent (cpp and cxx crates)
+ great package and dependency management (C, C++ don't have that)
+ great build tools
+ great documentation tools
+ unlike GC based languages you get memory safety AND performance
+ shorter development time
  + reduced debugging time
  + lots of issues are solved at compile time such as reference issues, synchronization issues, etc
  + Rust is slower dev than Python but because it forces you to fix issues up front
  + you get faster to a solid piece of software
+ provides a general understanding of memory management for someone who doesn't have a systems programming background
+ recruiting pool could decrease in size but could increase in quality (back that up with facts)
  + https://www.reddit.com/r/rust/comments/gz9qam/are_you_using_rust_on_the_job_if_yes_howd_you/ftgms81?utm_source=share&utm_medium=web2x
  + https://www.reddit.com/r/rust/comments/gz9qam/are_you_using_rust_on_the_job_if_yes_howd_you/ftg9ivb?utm_source=share&utm_medium=web2x

## Jeremy Fitzhardinge (Facebook)

https://www.youtube.com/watch?v=kylqq8pEgRs

By Jeremy Fitzhardinge who introduced Rust at Facebook on the [Monoke](https://github.com/facebookexperimental/eden) source control backend.

+ introducing a new language is not just a technical matter
  + intrinsically risky (long term bet that might not pay off)
  + most people just want to get the work done and are not interested in a new language
+ making the case
  + needs to be **much** better at something that incumbent languages
  + and at least on par for everything else

Rust's 10x advantage:
+ detects large classes of serious bugs at **compile time** (memory corruption, race conditions)
+ cost of a bug at compile time is **orders of magnitude less** than in production

Unlike code reviews and static analysis, Rust solves large classes of errors in the "inner loop" (at compile time) while the developer is still in the development phase, not later or in production.

But then how do you quantify the cost of bugs that didn't happen?
No real answer there.

Benefits of this include:
+ code reviews can now be higher level and focus on the why instead of the how
+ reviewers don't need to be concerned with low-level details
+ code maintenance has a lower cost
  + dropping a fix somewhere probably won't break everything
  + the compiler will catch it because the constraints are encoded in the types and the lifetimes

New Rustaceans:
+ ramp-up time is a universal concern
+ typically they have seen:
  + 2-3 weeks to get to write little programs while fighting with the compilers
  + 3-5 weeks to be able to work out a way to get things done
  + 8+ weeks to start being consistently idiomatic
+ this is dependent on language background:
  + functional: Rust's constraints are easy to conceptualize
  + C++: lifetimes make sense
  + Go/Java: harder to migrates from GC language
+ no need for much internal training material
  + at the begining because of enthusiasm
  + different with onboarding of new developers

Building a community:
+ local community extending the external Rust community
+ bootstrap a review culture
+ active learning group
+ solid documentation (hybrid of external and internal)

Rust is not just for fans:
+ early adopters are very excited
+ need to reach out to people who just want to get their job done
+ arguments for it include:
  + undedected bugs are expensive
  + intrinsic security (especially for untrusted inputs) is valuable
  + attractng talents is easy (people want to use Rust)
  + learning curve is steep but you get higher faster
  + async is really nice
+ general observations:
  + every team who has evaluated Rust has ended up using it (so far)
  + non of the adopters has regretted it

Rust at Facebook now:
+ actively used for several high-value projects
+ many standard APIs available in Rust
+ lots of ambient enthusiasm
+ not the default language for any domain yet (although it would make sense for security critical code bases)
+ rough edges and missing functionality
+ journey is 1% finished

## Ashley Williams (NPM)

https://www.youtube.com/watch?v=GCsxYAxw3JQ

+ making Rust fast is hard
+ making Rust correct is pretty simple
+ have a problem to solve
+ learning curves are a blessing in disguise
  + learning something new can be energizing
  + if it is hard it is probably better

## Ryan Levick (Microsoft)

https://www.youtube.com/watch?v=NQBVUjdkLAA

> Our systems are **more interconnected** and performing **more important** tasks.

> Our systems are **more vulnerable**, and attackers have **more incentive to attack**.

How much does this cost (CVEs):
+ $150,000 per incident (the figure is from 2004 adjusted for inflation)
+ extremely conservative estimate
+ only direct mitigation costs (not reputation loss, cost to customers, etc)
+ in 2018, 468 issues
+ can the cost be even higher?
  + yes in the past some issues have cost a lot more
  + Wannacry malware infected 200,000 computers
  + napkin estimate is $4B for Microsoft and customers
  + and this is **with a patch put in place** (so basically people who didn't update)
  + NHS in the UK estimated the cost for them to fix it to 92 million pounds

The core of the problem is **memory safety** issues, which account for most CVEs:
+ use after free
+ uninitialized memory
+ double free
+ buffer overruns
+ but also:
  + data races

70% of CVEs are related to memory safety, and the number is not going down, despite **massive** efforts.

At the core of this is C++, because even though it is a great language, it is not a safe one.
Recent versions have provided ways to protect against some of these issues, but this is not enough.
Concerning because C and C++ are used to build the core of the Microsoft systems and "run the world".

Lots of great ideas have been used but they're not hollistic so they're not enough:
+ we need better programmers
  + training is good
  + but there is 0 evidence that training of C/C++ developers will fix the issue in any significant way
  + and Microsoft does a lot of training!
  + people feel personally attacked in these situations
    + but really its not an individual programmer's problem
    + 2 very well written pieces of software that work great in isolation will fail when composed because the underlying language is not safe and assumptions about memory management will be different
+ we need better analysis tools (static analysis)
  + can get you pretty far
  + but this is not on by default
  + need to include it in build system and run it on every build request
  + it slows the development process
  + there is a lot of overhead
  + it is **limited** because the memory unsafe language won't provide all the annotations needed to figure out if the program is really correct
+ we need to make these issues impossible
  + you need something in the language to make sure everything is caught
  + the cost of a bug at compile time is **orders of magnitude less costly** that one in production

How to make these issues impossible?
+ runtime checking contracts
  + require a lot of discipline in memory unsafe languages
  + programmers need to know about the constructs and use them at all times
+ garbage collection
  + works great
  + not acceptable for systems software (low level, just above assembler)

Microsoft Research Security Center (MSRC) formed the Safe Systems Programming Languages initiative.
+ making C++ safer: limited returns and the wall has been reached
+ developing new programming languages / constructs for systems programming in a safe way
  + release early version of the Verona research project
+ adopting the industry's best chance to address the issue -> Rust

Rust:
+ performance
  + on par with C/C++
  + first rewrites at Microsoft confirm that (and seem to lean toward Rust being faster)
+ correctness
  + a lot less runtime debugging
  + compile time checks mean programs tend to run correctly when they compile
  + no nulls
  + no exceptions, you know from function signature if it can fail
+ productivity
  + steep learning curve
  + but once you're beyond it that pays immensely
  + modern tooling (package manager, test framework)
  + systems programmers can have nice things too
  + adopting Rust is not a pain, people tend to enjoy it
+ safety
  + completely memory safe by default
  + free of data races
  + with minimum runtime checks (only where necessary)
  + wraps unsafe constructs in safe APIs

Rust's 10x improvement: at least 10 times better that what we currently have.
+ with C/C++, 100% of the code needs to be audited for memory safety.
+ with Rust it's much smaller (no exact figures but fundamental Rust packages are around 1%)

The issue: for security critical software, C++ **is no longer acceptable**.
Rust allows to write those safely and with performance.

The challenge: integrate Rust instead of rewriting the entire world.

Second half of the presentation is about this challenge at Microsoft and questions.

For interoperability:
+ bindgen and cbindgen work great for simple C APIs
+ not as straightforward for complex C++ APIs

Humans:
+ convincing others
+ training others

Use in industry:
+ Microsoft: aiming to be at the forefront of adoption
+ Facebook
+ Amazon (Lambda, stuff inside EC2)
+ Google
+ Dropbox (file sync engine)
+ Intel and ARM
+ CloudFlare
+ Mozilla
+ Discord

The industry needs to give this language an honest shot because it provides a solution to a problem that still hasn't been solved.

Verona:
+ research project
+ can't tell where it's going to go
  + become its own language?
  + go nowhere?
  + provide ideas and constructs that feed back into Rust?

What features make Rust safe?
+ borrow checker that checks ownership of values (memory) and when to destroy it (RAII from C++)
  + can ensure you don't drop a value when it still has references (borrows) to it
  + ensure either multiple immutable references or single mutable reference at all times (readers/writer paradigm)
  + all done at compile time

At Microsoft:
+ Krustlet (Kubernetes tooling)
+ Azure IoT Edge Security Daemon (on devices)
+ some minimal VS Code stuff (installer, regular expression engine)

Go comparison:
+ Go has garbage collection
+ Rust has a much smaller standard library (by design)
+ Go compiles much faster
+ Go doesn't have generics or as rich of a type system
+ Go is easier to learn
+ Go currently beats Rust for quick micro-services for instance

## Alexandra Clifford (Lincoln Laboratory)

https://drive.google.com/file/d/1-WqGKbksNlyXCg5a6mz4RX7YKJNbF4hG/view?usp=sharing

https://apps.dtic.mil/sti/pdfs/AD1087142.pdf

### C++

Pros:
+ fast, small footprint
+ large existing ecosystem
+ large community of developers
+ toolchain support for almost any platform

Cons:
+ weak type system permits developer errors that create exploitable vulnerabilities
+ bolt-on security is incomplete and incurrs high overhead

### Rust

Pros:
+ C-like performance
+ memory and concurrency safety
+ industry support and growing ecosystem
+ lots of toolchain support

Cons:
+ fewer developers

Dual benefit:
+ memory and type safety protects from benign bugs
  + no invalid memory accesses
  + no information leakage
  + no arbitrary code execution
+ highly optimized for speed and performance
+ safe concurrency built into the language
+ also offers:
  + zero-cost abstractions
  + threads without data races
  + efficient C bindings
  + safe memory space allocation
  + smart pointers (prevent memory mismanagement)
  + a lot more!

**Use Rust whenever possible for embedded system development**

More content in the report but nothing about Rust per se.

## Advanced Space

https://www.youtube.com/watch?v=ET1QdkAK-_U

C is great but:

+ difficult to develop in
+ difficult to debug
+ difficult to verify
+ difficult to validate

## Ryan Plauche (Kubos)

https://www.youtube.com/watch?v=y5Yd3FC-kh8

Customers in aerospace are mostly electrical engineers and aerospace people, not really software.

After moving to Linux (from FreeRTOS) because it made sense for the customers, this opened up the door to new language.

C was the easiest to pick up (and was used on FreeRTOS) but is it the safest choice for the customers. They would get very pointed questions in conferences where people were asking about the safety of C and how to guarantee safety for the customers.

ADA is another popular choice in aerospace industry but is very much behind closed source bareers and made it impossible for their open source software.
Not the same community either.

GraphQL support was a big decision factor.

Good stuff:

+ High level tools, low level performance
+ Compiler protections
+ Compiled documentation with tests is big!
+ Tooling

Challenges (true in aerospace and in fringe industries):

+ Compiled binary sizes for software updates
+ User learning curve
  + But great community and resources
+ Language maturity
  + Haven't faced it really
  + But a lot of institutional inertia
+ Toolchain maturity (for embedded)
+ Ecosystem maturity (options, maintenance, etc)
