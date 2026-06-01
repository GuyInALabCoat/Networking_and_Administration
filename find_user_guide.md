# Introduction
The `find` utility that is a part of GNU findutils package is an incredibly powerful tool for discovering files (and other *file-like* objects, such as directories, symbolic links, and other directory nodes), filtering them, and then performing some operation on each of them.

However, I have found much of the official documentation on this utility difficult to use as they introduce concepts and their various options out of order, such that it is difficult to piece them together into more complex commands.

Here I intend to create a user-guide of sorts that introduces modifiers to the `find` command in order of decreasing scope, so that even complex commands can be built-up piece by piece in a clear and concise way.

Each option category will be separated into their own section with further explanation within. Options that are surrounded by square brackets '[]' are optional, while options that are surrounded by curly braces '{}' implies that one of several options must be chosen. My option categories are slightly different from GNU's official documentation, however I will make reference to these names where my classification differs. Deprecated options will not be listed here in this guide.

Much of this guide is based on the official documentation provided by GNU, the Free Software Foundation, Inc. and the official Linux Documentation [find(1)-Linux manual page](https://man7.org/linux/man-pages/man1/find.1.html#COPYRIGHT) and  [GNU Findutils](https://www.gnu.org/software/findutils/manual/find.pdf) and users should always refer to these documents in troubleshooting their issues.

## Overview

The structure of the `find` command is as follows:

`find [Info] [Link Options] [Debug Options] [Optimisation] [Starting-Point] [Global Options] { Tests | Actions } [Operators]`

## Info

<details> \
$\color{cyan}{\text{-help, --help}}$ \
Prints a overview of find usage options and syntax, and then exits.

$\color{cyan}{\text{-version, --version}}$ \
Prints the find version information, and then exits.

<details>

<summary>Why this is different than the official documentation</summary>

In the official GNU documentation, `--help` and `--version` options are placed under the *GLOBAL OPTIONS* category, however I feel this obscures the purpose of the global options and their place in the syntax of `find` commands. Unlike other global options, these options can be placed anywhere in a command and they will execute first and exit without a warning message before any other part of the command can execute.

Although this is rather minor, I've decided to place them in their own category where they can stand alone, as opposed to the other global options that are particular about their place in the syntax and which will print warning messages if they are not before the first test, positional option or action.

</details>
</details>

## Link Options

<details> \
Controls the treatment of symbolic links. (If more than one option is specified, the last one specified will take effect)

$\color{cyan}{\text{-P}}$ (Default)
<details>
<summary>Never Dereference Symbolic Links</summary>

When `find` encounters a symbolic link, it will only examine the symbolic link itself and not whatever file it points to.
</details>

$\color{cyan}{\text{-L}}$
<details>
<summary>Dereference Symbolic Links Where Possible</summary>

If `find` encounters a symbolic link, it will examine the target of the symbolic link unless the symbolic link is broken, in which case it will then examine the symbolic link.
</details>

$\color{cyan}{\text{-H}}$
<details>
<summary>Do Not Dereference Symbolic Links, except if they are given as command line arguments</summary>

If `find` encounters a symbolic link, it will function identically as the -P option, unless that symbolic link is given as one of the *Starting Points* on the command line, or it is in the starting point file following the `-files0-from` *Global Option*. In these cases where a symbolic link is given as a command line argument, it is dereferenced (where possible) and the target of the symbolic link will be examined instead (as in the -L option).

</details>
</details>

## Debug Options

$\color{cyan}{\text{-D \emph{options}}}$

<details>

Prints diagnostic information to help troubleshoot the behaviour of the `find` command. Run `find -D help` to see a complete list of valid debug options, or consult the official documentation.

</details>

## Optimisation

$\color{cyan}{\text{-O\emph{level}}}$

<details>

Enables query optimisation, specified using `-O*level*` , where *level* can be a value from 0 to 3 inclusive. Opimisation is outside the scope of this guide and users should refer to the official documentation for further details.

</details>

### Copyright
Copyright © 2026 Dmytro Politov \
Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Document License, Version 3 or any later version published by the Free Software Foundation. [GNU General Pulic License](https://www.gnu.org/licenses/gpl-3.0.html)