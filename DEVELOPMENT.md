# Development Environment

This is a [Rust][rust] project, built via [Cargo][cargo].
Additionally, every part of the project can be built using [Nix][nix].

> [!IMPORTANT]
> This project relies on the [Nix flakes][flakes] feature, which might not be enabled by default.
> To enable it, set `extra-experimental-features = nix-command flakes` in your Nix config.

## Build

With Nix installed the following starts a build and displays build logs:

```
nix build -L
```

Implicitly, the `.#default` flake output is built, which builds ddnnife.
There are different flake outputs: Kotlin bindings, Python bindings, the container image and more.
Run the following to get a list of all available flake outputs per supported system:

```
nix flake show
```

## Formatting

This project uses [`treefmt`][treefmt] for formatting the Nix and Rust code.
To format the code, simply run:

```
nix fmt
```

The configuration is done in `nix/treefmt.nix`.

## Check

A Nix check will run format and linter checks:

```
nix flake check -L
```

This will error on any problems and show the full output of each check.

## Development Shell

For manual builds and IDE setup, a Nix development shell can be used.
It configures the environment to pull in everything needed for a build.
To enter a development shell, use the following:

```
nix develop
```

## Updates

There are four different set of dependencies: Nix, Rust, Gradle and GitHub actions.
All but Gradle are taken care of by Renovate.

The Nix dependencies are tracked in `flake.nix` and locked in `flake.lock`.
The following will update the lockfile with the latest available versions that fit the specifications in `flake.nix`:

```
nix flake update
```

Similarly, Rust dependencies are tracked in `Cargo.toml` and locked in `Cargo.lock`.
We track all dependencies of the workspace in the root `Cargo.toml` to ensure the same version of any given crate is used across the workspace.
The following will update `Cargo.lock` as specified by `Cargo.toml`:

```
cargo update
```

Gradle does unfortunately not offer a lockfile that works for Nix.
Therefore, Nix provides its own lockfile for Gradle dependencies and injects them at build time.
The dependencies are specified in `bindings/kotlin/build.gradle.kts` and locked by Nix in `nix/kotlin-dependencies.json`.
This lockfile can be update by running:

```
nix build .#kotlin.mitmCache.updateScript
./result
```

The GitHub Actions lockfile is at `.github/workflows/actions.lock` and can be updated using [`gh`][gh] (requires the [`actions-lock`][actions-lock] extension):

```
gh actions-lock --relock
```

## Cache

The CI uses a Nix cache which lives at [Cachix][cachix].
It can also be used locally so that Nix automatically uses cached builds when possible.
The Cachix cache has the name `practical-kc`.
Nix will ask to add it when using the flake in this repository.
Alternatively, it can be enable manually:

```
nix run nixpkgs#cachix use practical-kc
```

[rust]: https://rust-lang.org
[cargo]: https://doc.rust-lang.org/cargo
[nix]: https://nixos.org
[flakes]: https://wiki.nixos.org/wiki/Flakes
[treefmt]: https://treefmt.com
[gh]: https://cli.github.com
[actions-lock]: https://github.com/github/gh-actions-lock
[cachix]: https://www.cachix.org
