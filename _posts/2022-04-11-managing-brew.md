---
layout: post
title:  "Managing Multiple Homebrew Versions on an M1 Mac"
date:   2022-04-10 18:20:49 +0100
categories: programming knowledge
---

In this blog post, I’ll quickly explain how homebrew works on M1 Macs, then show how to configure homebrew such that you can manage both Arm-based and x86-based software.

<!--end_of_excerpt-->

## Two versions of Homebrew

What nobody will tell you is that there are two very different versions of Homebrew you can install on an M1 Mac. The first is the new, Arm-based version designed specifically for the M1 Mac, managing software running on the Arm architecture. The second is the older version for Intel-based macs, managing software running on the x86 architecture. M1 Macs can run x86 programs with Rosetta, which translates x86 instructions to Arm64. Confusingly, while both package managers have the name “brew”, they are NOT interchangeable! X86 brew cannot install Arm64 software, and Arm64 brew can only install x86 binaries if they have been translated to the Arm64 architecture. 

This means that if you’re working on a codebase with software that doesn’t run natively on Arm, it’s best to download x86 brew. For example, if you’re working on a Python 3.7.x project, and you wanted to install its dependencies with brew, you’d better use the x86 version so you’re guaranteed to install all required dependencies. Since some dependencies likely haven’t been translated to Arm64, Arm brew will be unable to install them, generating a host of compatibility issues. 

Of course, if you’re only going to work with x86 software, what’s the point of using an M1 Mac at all? If you plan on downloading your favorite code editor with brew, and there’s a version native for Arm64 (as for most code editors), you won’t be able to download it with x86 brew, and will instead have to settle with a less optimized version To solve this, I’ll share how to download both versions of brew and switch between them to get the best of both worlds.

## Setting up Homebrew on a Mac M1

To install x86 homebrew, run

```
➜  ~ arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

By default, this version of homebrew is stored in `/usr/local/homebrew`.

To install arm64 homebrew, run

```
➜  ~ arch -arm64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

By default, this version of homebrew is stored in `/opt/homebrew`.

Of course, since they’re both stored as “brew”, you can’t use them both just yet. One solution is to alias them with different names, but my preferred solution is to automatically change the brew type based on the current default architecture by configuring `.zshrc`.

```zsh
# Multiple Homebrews on Mac M1
if [ "$(arch)" = "arm64" ]; then
    eval "$(/opt/homebrew/bin/brew shellenv)"
else
    eval "$(/usr/local/bin/brew shellenv)"
fi
```

We can verify that this works!

```
➜  ~ arch -x86_64 zsh
➜  ~ arch
i386
➜  ~ which brew
/usr/local/bin/brew
➜  ~ arch -arm64 zsh
➜  ~ arch
arm64
➜  ~ which brew
/opt/homebrew/bin/brew
```

Lastly, to switch from one default architecture to another, I like to set some aliases in my `.zshrc`.

```
alias tox86="arch -x86_64 zsh"
alias toa64="arch -arm64 zsh"
```

One last problem is that the shell will draw on different pools of software based on the current version of brew. So, if I install neovim on the Arm brew for the performance gains, then switch to an x86 architecture, my shell won’t know where neovim is!

```
➜  ~ arch
i386
➜  ~ nvim
zsh: command not found: nvim
➜  ~ toa64
➜  ~ which nvim
/opt/homebrew/bin/nvim
```

The only solution I’ve found to this is to alias the editor path in the `.zshrc`. 

```zsh
alias nvim="/opt/homebrew/bin/nvim"
```

This is a good approach if you plan on working in an x86 environment with the aid of only a few Arm64 applications. Otherwise, it might be best to just switch from one environment to another.

