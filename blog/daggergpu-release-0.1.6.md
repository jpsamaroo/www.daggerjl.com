# DaggerGPU 0.1.6 Release Notes

Summary of key changes:
- Package extensions and ROCm/AMD revamp ([#18](https://github.com/JuliaGPU/DaggerGPU.jl/pull/18))

While only consisting of a single PR, this release is a big step towards making DaggerGPU suitable for more use cases. First and foremost, this release adds proper support for computing with AMDGPU.jl, by making use of the new per-task synchronization API, and by updating to the latest version of KernelAbstractions.jl.

Additionally, DaggerGPU now uses package extensions on Julia 1.9+, which makes it possible to not have to load all of the GPU backends just to use DaggerGPU with a single backend. We still keep Requires around for 1.7 and 1.8 support, but I would expect that to go away in the not-too-distant future.

Along the way, I also updated the tests to be a bit more thorough and correct across all the backends, which should help ensure good coverage of our supported backends.

That's it for this release, but I expect to be doing some work focused on KernelAbstractions support in the near future, to make it easier to port GPU kernel code to Dagger directly without extra boilerplate.
