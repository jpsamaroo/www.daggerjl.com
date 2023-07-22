@def title = "Dagger.jl Home"
@def tags = ["parallel", "hpc", "julia"]

~~~
<img alt="Dagger.jl Logo" src="assets/logo-final-medium.jpg" style="width:100%;padding:0%" />
~~~

# Unleashing Parallelism with Dagger.jl

Making code run fast used to be difficult - no longer! Dagger.jl supercharges your code, letting it run in parallel on any and all hardware you have at your disposal.

Dagger doesn't expect you to learn about multithreading, distributed communication, GPU programming, atomics and memory models, etc. It could take years just to get up to speed on accelerating your code by hand! Instead, Dagger does that hard work for you - all you have to do is write your code!

Does this sound interesting? If so, keep reading to find out how to *make your code go fast* with **Dagger.jl**!

## Installing Dagger.jl

Once you've [installed Julia](https://julialang.org/downloads/), run the following to start using Dagger:

```julia-repl
(MyJuliaProject) pkg> add Dagger
julia> using Dagger
julia> mycode!() = println("I'm using Dagger!")
julia> fetch(Dagger.@spawn mycode!())
```

## Using Dagger.jl

See Dagger.jl's [documentation on GitHub](https://juliaparallel.org/Dagger.jl/dev/) to get going, or check out
any of our other excellent [recommended resources](https://github.com/JuliaParallel/Dagger.jl/#resources) for a
different learning experience!

## Why use Dagger.jl instead of <other library>?

Other libraries and frameworks make you work hard to get good performance:

- **dask** makes you choose between multithreaded and distributed acceleration
- **MPI** forces you to write your code in an SPMD style
- **OpenMP** expects your code to be statically compiled C, C++, or Fortran
- **Spark** tries to get you to pay for extra libraries to be productive
- **Hadoop** is slow and is painful to configure

## Why use Dagger.jl instead of accelerating by hand?

It's never a bad thing to learn how to write fast, parallel code, but it's also
time- and experience-intensive. Sometimes you just can't afford to do all the
learning and still be productive!

Don't get me wrong, Julia has great facilities for writing fast code:

- A state-of-the-art compiler with support for many CPU architectures
- Built-in multitasking and multithreading (`Threads.@spawn`)
- Easily accessible distributed computing with Distributed.jl
- Vendor-agnostic GPU computing with KernelAbstractions.jl
- Supercomputer-scale communication with MPI.jl

Each of these tools is amazing in isolation, and in many cases, can be
mixed-and-matched to gain the benefits of each tool. But as your code grows in
features, so too it grows in complexity - and that complexity is amplified by
the assumptions and quirks of each tool that your code relies on.

Dagger is no stranger to these tools - it uses them under the hood to make your
code fast! But Dagger frees you - the busy developer, scientist, engineer -
from having to learn how to best utilize these tools and maintain support for
them. Dagger gives you a small (but powerful!) set of tools that cooperate to
make your code fast, and keep you productive!

## Contributing to Dagger.jl

Dagger.jl is a Free and Open Source, [MIT-licensed](https://en.wikipedia.org/wiki/MIT_License) project, written entirely in the [Julia programming language](https://julialang.org), and hosted on [GitHub](https://github.com). We welcome users to [file issues or suggest improvements](https://github.com/JuliaParallel/Dagger.jl/issues), or if you have the time and comfort with Julia programming, we'd love to [review your contributions](https://github.com/JuliaParallel/Dagger.jl/pulls)!
