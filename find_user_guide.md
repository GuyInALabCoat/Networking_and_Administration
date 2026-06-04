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

<details> 
    <summary>Getting Help and Version Information</summary>
    <br>
    <blockquote>
        $\color{cyan}{\text{-help, --help}}$ <br>
        Prints a overview of find usage options and syntax, and then exits.
    </blockquote>
    <blockquote>
        $\color{cyan}{\text{-version, --version}}$ <br>
        Prints the find version information, and then exits.
    </blockquote>
    <details>
        <summary>Why this is different than the official documentation</summary>
        <br>
        <blockquote>
            In the official GNU documentation, `--help` and `--version` options are placed under the *GLOBAL OPTIONS*  category, however I feel this obscures the purpose of the global options and their place in the syntax of `find` commands. Unlike other global options, these options can be placed anywhere in a command and they will execute first and exit without a warning message before any other part of the command can execute.
            <br>
            <br>
            Although this is rather minor, I've decided to place them in their own category where they can stand alone, as opposed to the other global options that are particular about their place in the syntax and which will print warning messages if they are not before the first test, positional option or action.
        </blockquote>
    </details>
</details>

Under the official documentation, *Link Options*, *Debug Options*, and *Optimisation* are listed together under *OPTIONS*.

## Link Options

<details> 
    <summary>Controls the treatment of symbolic links. (If more than one option is specified, the last one specified will take effect)</summary>
    <br>
    <blockquote>
        $\color{cyan}{\text{-P}}$ (Default)
        <details>
            <summary>Never Dereference Symbolic Links</summary>
            <br>
            <blockquote>
                When `find` encounters a symbolic link, it will only examine the symbolic link itself and not whatever file it points to.
            </blockquote>
        </details>
    </blockquote>
    <blockquote>
        $\color{cyan}{\text{-L}}$
        <details>
            <summary>Dereference Symbolic Links Where Possible</summary>
            <br>
            <blockquote>
                If `find` encounters a symbolic link, it will examine the target of the symbolic link unless the symbolic link is broken, in which case it will then examine the symbolic link.
            </blockquote>
        </details>
    </blockquote>
    <blockquote>
        $\color{cyan}{\text{-H}}$
        <details>
            <summary>Do Not Dereference Symbolic Links, except if they are given as command line arguments</summary>
            <br>
            <blockquote>
                If `find` encounters a symbolic link, it will function identically as the -P option, unless that symbolic link is given as one of the *Starting Points* on the command line, or it is in the starting point file following the `-files0-from` *Global Option*. In these cases where a symbolic link is given as a command line argument, it is dereferenced (where possible) and the target of the symbolic link will be examined instead (as in the -L option).
            </blockquote>
        </details>
    </blockquote>
</details>

## Debug Options

$\color{cyan}{\text{-D \emph{options}}}$
<details>
<br>
    <blockquote>
        Prints diagnostic information to help troubleshoot the behaviour of the `find` command. Run `find -D help` to see a complete list of valid debug options, or consult the official documentation.
    </blockquote>
</details>

## Optimisation

$\color{cyan}{\text{-O\emph{level}}}$
<details>
<br>
    <blockquote>
    Enables query optimisation, specified using `-O*level*` , where *level* can be a value from 0 to 3 inclusive. Opimisation is outside the scope of this guide and users should refer to the official documentation for further details.
    </blockquote>
</details>

## Starting Point

<details>
    <summary>Root of the Directory Tree to search. If no starting point is specified, the current working directory '.' is used by default.</summary>
    <br>
    <blockquote>
        Given a directory, `find` will evaluate the given expression from left to right for every file it encounters using short-circuit evaluation, after which it will move on to the next file. 
        <br>
        <br>
        Multiple directories can be listed so long as they are listed before the first expression argument (`find` reads until it encounters a '-' to determine the start of the expression). However, if using wildcard globbing to give starting point arguments and a file happens to begin with '-', then `find` may mistakenly take that file name as an expression argument. Therefore it is safer to prefix wildcards with './' or use absolute path names.
        <br>
        <br>
        Alternatively, instead of listing directories as command arguments, a ASCII NUL separated list of starting points can be given using `-files0-from {file | -}`. If giving a filename, the file must contain a list separated by single ASCII NUL characters, else it will terminate with an error. If giving '-' as an argument, `find` will read the list of starting points from `stdin`.
    </blockquote>
</details>

## Global Options

<details>
    <summary>Options that affect the operation of all tests and actions specified on any part of the command line, and as such should be specified directly after the list of starting points. Global options placed elsewhere will issue a warning message.</summary>
    <br>
    <blockquote>
        $\color{cyan}{\text{-d | -depth}}$ 
        <br>
        Processes a directory's contents before the directory itself. Effectively performing depth-first-search on a given directory structure.
        <br>
        $\color{cyan}{\text{-mount | -xdev}}$ 
        <br>
        Don't descend into directories on other filesystem. Both `-mount` and `-xdev` are equivalent.
        <br>
        $\color{cyan}{\text{-middepth \emph{levels}}}$ 
        <br>
        Do not apply any tests or actions for any files *above* a certain depth (level, integer >= 0). Using `-mindepth 1` evaluates all files except the list of starting points.
        <br>
        $\color{cyan}{\text{-maxdepth \emph{levels}}}$ 
        <br>
        Limits the depth (level, integer >= 0) to which `find` will descend within each starting point. Using `-maxdepth 0` means tests and actions will be applied only to the starting points themselves.
        <br>
        $\color{cyan}{\text{-ignore\_readdir\_race}}$ 
        <br>
        To read from a directory `find` issues a system call to the kernel using `readdir()` (or modern `getdents()`) which returns the basename of all the directory entries. To obtain each file's metadata, it then calls `stat()` on each filename. 
        <br>
        Normally, if this `stat()` call fails (i.e., if a file is deleted between these two system calls) an error message will be printed. However with this option enabled, *no error message will be issued*. 
        <br>
        This option is useful when examining filesystem directories that change frequently (i.e., mail queues, temporary directories, etc.)
        <br>
        $\color{cyan}{\text{-noignore\_readdir\_race (default)}}$ 
        <br>
        Reverses the effect of the `-ignore_readdir_race` option.
    </blockquote>
    
$\color{cyan}{\text{-noleaf}}$
<details>
    <summary>Necessary Option for Non-Unix-like Filesystems (CD-ROM, MS-DOS, AFS, etc.)</summary>
    <br>
    <blockquote>
        On Unix filesystems, each directory has 2 + N number of hard links to it (where N is the number of immediate subdirectories). A directory with no subdirectories has only 2 hard links (1. its own name inside its parent directory, 2. the inner '.' directory that references itself.) Normally, `find` optimizes its search such that when it has called `stat()` on 'A' number of subdirectories (where 'A' is a directory's number of hard links - 2), then it assumes that the remaining entries are *not* directories, significantly speeding up execution when metadata is not needed.
        <br>
        On filesystems that do follow this convention this option is required, otherwise `find` may skip over some subdirectories.
    </blockquote>
</details>
</details>

### Copyright
Copyright © 2026 Dmytro Politov \
Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Document License, Version 3 or any later version published by the Free Software Foundation. [GNU General Pulic License](https://www.gnu.org/licenses/gpl-3.0.html)