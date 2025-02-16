# [#31860](https://github.com/bitcoin/bitcoin/pull/31869) cmake: add libbitcoinkernel target

PR by @hebasto

## Context

This PR is a easy follow up on [#31844](https://github.com/bitcoin/bitcoin/pull/31844). It utilizes CMake's `add_custom_target()` and `add_dependencies()` to essentially provide an alias to the `bitcoinkernel` target. As I understand, because `bitcoinkernel` is declared as a library, CMake automatically prefixes the resultant output with `lib`, concluding in `libbitcoinkernel`. While sensible, this disallows us from matching the name of it as a component intuitively, as `libbitcoinkernel` when calling `cmake --install build --component bitcoinkernel`. By adding a target alias, the PR allows the use of the intuitive name as a component.

## Personal

As I reviewed the prior PR, and this one was a rather simple addition, it was pretty easy to test and weigh in. I learned a little more about CMake in that my test using the build flag provided in the description didn't pick up. I learned that I need to delete the `./build` directory or pass `--fresh` to the `cmake -B build` command in order to overwrite the build system cache stored in `./build/CMakeCache.txt`. Simple thing, but good to know as I learn more about CMake.
