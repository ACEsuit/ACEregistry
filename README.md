# ACE Registry 

This is the Julia package registry for ACEsuit packages that are still 
under heavy development. Mature packages suitable for public use will 
generally be moved into the `General` registry. 

To install open a Julia REPL and type
```{julia}
] registry add https://github.com/ACEsuit/ACEregistry.git
```
Afterwards use the normal package manager commands to add, remove, dev, etc the packages in this registry.

On Windows, there might be problems due to stalled `git clone` command. One workaround that can be
used is by calling systems's git. This can be achieved by setting environment variable
`JULIA_PKG_USE_CLI_GIT` to `true`:
```julia
ENV["JULIA_PKG_USE_CLI_GIT"] = true
```
This can be added to Julia's startup.jl or call this before any Pkg commands that
requires `git clone`. This will work as along as `git clone` command works (not
blocked by Windows Defender or any other programs).


## Registering a Package or a new version

Both registering a new package or a new version uses
[`LocalRegistry`](https://github.com/GunnarFarneback/LocalRegistry.jl).
This needs to be installed first:
```julia
] add LocalRegistry
```

Suppose now that we want to register a new version of `JuLIP.jl`:
1. `] dev JuLIP`; we will assume this puts `JuLIP` into  `~/.julia/dev/JuLIP/`.
2. make the changes to the package
3. Bump the version number, by manually editing `Project.toml`, commit and push.
4. Now the following sequence of commands will register the new version in the `ACE` registry.
   ```julia
   using LocalRegistry
   using JuLIP
   register(JuLIP; regsitry="ACE")
   ```
   This edits the registry files and commits the changes and then pushes the registry to the git server.
   If the package has already been registered before (i.e. this is a version update),
   then a simple `register(<packagename>)` is sufficient.

## Remarks

* If you don't have push access or are unsure about merging the updated registry, then make a PR. (Maybe this step should be automated at some point.)
* Sometimes, I found that the main `v1.x` environment is incompatible with a package, and e.g. `using IPFitting` will fail. In that case, one first activates the package's environment.
