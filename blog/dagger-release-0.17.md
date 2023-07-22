# Dagger 0.17 Release Notes

Summary of key changes:
- Added keyword argument support to all APIs ([#394](https://github.com/JuliaParallel/Dagger.jl/pull/394))
- Added cross-thread work stealing ([#373](https://github.com/JuliaParallel/Dagger.jl/pull/373))
- Deprecated `proclist` and `single` options, added `Dagger.scope` helper ([#374](https://github.com/JuliaParallel/Dagger.jl/pull/374))
- Added support for broadcast to `@spawn` ([#372](https://github.com/JuliaParallel/Dagger.jl/pull/372))

This release was primarily focused on usability, with the additions of kwarg support, broadcast in `@spawn`, and the `Dagger.scope` helper. Additionally, with assistance from Przemysław Szufel (`@pszufe`), we were able to greatly improve multithreaded performance thanks to the addition of same-worker work stealing. Cross-worker work stealing was skipped as the performance implications of that are quite complex, whereas same-worker work stealing is basically always a net win for performance.

It's also important to note the deprecation of `proclist` and `single` options, which was done because their semantics were not entirely clear (especially when mixed with the more powerful scope system), and because they weren't expressive enough for many use cases. `scope` is the successor option to these, and with the new `Dagger.scope` helper, it should be an easy upgrade for most codes. Please file an issue if you run into missing functionality with this helper!
