# [#31844](https://github.com/bitcoin/bitcoin/pull/31844) cmake: add a component for each binary

PR by @theuni

## Context

Bitcoin Core doesn't support the installation of a specific binary when running `cmake --install build`. This can be accomplished by making use of the `COMPONENT` option of CMake's `install()` command. The existing implementation maintains a list of `installable_targets` and installs them all together in one `install()` command in an elegant attempt to reduce repeated code. This is problematic for assigning targets to a component, as each would classically require its own `install()` command.

The PR defines a new CMake module which is passed a component name. It wraps the `install()` command for the target and defines the `COMPONENT` option for that of the same name. Additionally, this PR defines its own option `HAS_MANPAGE` which is set if the given target also has documentation that should be installed along with it. This cleverly solves the adjacent issues [#31762](https://github.com/bitcoin/bitcoin/issues/31762) and [#31765](https://github.com/bitcoin/bitcoin/pull/31765), where Bitcoin Core installs manpages for every target, even if they haven't been built (empty!).

The only existing install target that has a `COMPONENT` defined is `libbitcoinkernel`. That component is named `Kernel` and following that naming convention pattern of Pascal case would mean a somewhat disjointed developer experience of trying to install `bitcoin-cli` by calling for the named component `BitcoinCli`. The PR correctly reverses that convention and renames the `Kernel` to `bitcoinkernel`.

This PR is an elegant solution to a handful of (the above) discussions that have been floating around. It maintains the succinct install one-liner and avoids code reuse but offers more flexibility for defining install targets.

## Personal

This was my first attempt at performing a review in Bitcoin Core. Previously spotted [#31745](https://github.com/bitcoin/bitcoin/issues/31745) and attempted a PR [#31847](https://github.com/bitcoin/bitcoin/pull/31847) for that issue, but was promptly informed that this PR already existed. Somewhat disappointed that I didn't spot it, and happy to close my PR in favor of this more elegant, abstracted solution using a CMake module, and addressing the documentation issue.

Starting to get a feel for the structure and cadence of the PR review process. My initial glance at the code was quite light. I spotted that it was a superior implementation, took note of the module and support for coupled documentation, and that was it. Unfortuantely, I offered  my first `ACK` on the wrong commit, which was a bit embarassing, but hopefully my signal that the PR should also close issue #31745 was somewhat helpful in improving the issue management and making sure anyone else looking to tackle that issue didn't double their efforts.

As the other experienced reviewers weighed in, I followed their comments and put more time and scrutiny into the work. Again, near the apparent end of the review lifecycle, I spotted a minor nit regarding a naming convention for the function arguments prefix. Somewhat insecure, I debated whether to post the comment. After I did, the PR was merged a few minutes later. Slightly embarassing, but happy to observe a reasonable improvment make its way in.

I have a ton of learning to do about the code base, build system and developer workflow, but I think the latter will come quickly. Perhaps one day, I'll circle back and submit a PR for this nit when I feel more settled in the process.
