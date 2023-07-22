# GSoC Update - Week of June 23rd, 2023

Written by Felipe de Alcantara Tome (`@fda-tome`)

# DArray changes to the modern Eager API

The previous distributed array implementation of Dagger was still based in the legacy **`Lazy API`**, that is, the base routines were implemented using [`delayed`](https://juliaparallel.org/Dagger.jl/dev/api/functions/#Dagger.delayed) or the [`Thunk`](https://juliaparallel.org/Dagger.jl/dev/api/types/#Dagger.Thunk) constructor to create tasks, and [`compute`](https://juliaparallel.org/Dagger.jl/dev/api/functions/#Dagger.compute) or it's implicit calls embeded within **`collect`** to get results of tasks. The main effort during the last weeks was to mainly adapt the legacy array codebase, by directly changing the calls from the lazy API to the new API, substituting compute for fetch or delayed/Thunk for `spawn`, and adapting the legacy implementations to receive the modern version of Dagger, the result was [PR#396](https://github.com/JuliaParallel/Dagger.jl/pull/396). For further reference, access the [Lazy API](https://juliaparallel.org/Dagger.jl/dev/#Lazy-API) and [Eager API](https://juliaparallel.org/Dagger.jl/dev/#Eager-Execution) documentation. The code changes are denoted below, the files in the [array implementation folder](https://github.com/JuliaParallel/Dagger.jl/tree/master/src/array) that contain the changes are referenced for each major change. This aims to give general guidelines to developers trying to modernize other code that uses the legacy API.

### Changes to [darray.jl](https://github.com/JuliaParallel/Dagger.jl/blob/master/src/array/darray.jl)

The major changes done to adapt the current codebase were done to this file, first we've removed the need to specify a context since the modern API is cached all within the global context of computation, then we've implemented collect and fetch functions to accomodate all the data types other than the DArray struct that would be used by other operations, such as ArrayOp and Computation, which are types used by [sort.jl](https://github.com/JuliaParallel/Dagger.jl/blob/master/src/array/sort.jl) and [map-reduce.jl](https://github.com/JuliaParallel/Dagger.jl/blob/master/src/array/map-reduce.jl). Other than that, we've changed the thunkize function that was called when a compute of a DArray was performed, the function is built to find a thunk within a DArray execution tree and wrap it with the other elements of the DArray, transforming everything in a Thunk, now this is done eagerly by the fetch call for a DArray. The minor changes were either direct substitutions or changes to accomodate EagerThunks and other types on the definition of arrays such as `chunks::AbstractArray{Union{Chunk,Thunk}, N}` being changed to `chunks::AbstractArray{Any, N}`.

### Anonymous function pipeling

The use of `delayed` opened the opportunity of having a double call scheme, e.g,  `collect(treereduce(delayed(vcat), cs1))` present in [sort.jl](https://github.com/JuliaParallel/Dagger.jl/blob/master/src/array/sort.jl) , this sctructure presents `delayed(vcat)()` as a function that is yet to be called while performing a tree reduction with arguments present in `cs1`, this is not possible directly while using `spawn` and that is why an anonymous function was created when calling the reduce routine: `treereduce((cs...)->Dagger.spawn(vcat, cs...), cs1)`, this is the same case for [operators.jl](https://github.com/JuliaParallel/Dagger.jl/blob/master/src/array/operators.jl) anonymous function creation that is wrapped by spawn.

### Binary operations for matrix manipulation

Matrix operations defined in [matrix.jl](https://github.com/JuliaParallel/Dagger.jl/blob/master/src/array/matrix.jl) required the implementation of binary operations between matrices in the form of the `BinaryComputeOp{F}` struct and the inclusion of `AddComputeOp` and `MulComputeOp` that are basically specifications with `F = +` and ` F = *` respectively of the aforementioned struct.

### Other minor changes
 The other changes done to [alloc.jl](https://github.com/JuliaParallel/Dagger.jl/blob/master/src/array/alloc.jl), [map-reduce.jl](https://github.com/JuliaParallel/Dagger.jl/blob/master/src/array/map-reduce.jl), [getindex.jl](https://github.com/JuliaParallel/Dagger.jl/blob/master/src/array/getindex.jl), and[setindex.jl](https://github.com/JuliaParallel/Dagger.jl/blob/master/src/array/setindex.jl) were almost direct translations of code from `delayed` and `Thunk` to `spawn` and `@spawn` along with changing `compute` to `fetch`. The options are now wraped in a `Options` object as seen in [darray.jl](https://github.com/JuliaParallel/Dagger.jl/blob/master/src/array/darray.jl) `fetch(Dagger.spawn(Options(meta=true), thunks...)`. Apart from this, type changes from the lazy `Thunk` type to `EagerThunk`or to `Any` were made to assure correcteness and to broaden typing possibilites, along with removing the need to create pairs with `=>nothing`.



```
██████╗  █████╗  ██████╗  ██████╗ ███████╗██████╗         ██╗██╗
██╔══██╗██╔══██╗██╔════╝ ██╔════╝ ██╔════╝██╔══██╗        ██║██║
██║  ██║███████║██║  ███╗██║  ███╗█████╗  ██████╔╝        ██║██║
██║  ██║██╔══██║██║   ██║██║   ██║██╔══╝  ██╔══██╗   ██   ██║██║
██████╔╝██║  ██║╚██████╔╝╚██████╔╝███████╗██║  ██║██╗╚█████╔╝███████╗
╚═════╝ ╚═╝  ╚═╝ ╚═════╝  ╚═════╝ ╚══════╝╚═╝  ╚═╝╚═╝ ╚════╝ ╚══════╝



```
