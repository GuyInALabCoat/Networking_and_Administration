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
            In the official GNU documentation, $\color{cyan}{\text{--help}}$ and $\color{cyan}{\text{--version}}$ options are placed under the $\color{red}{\text{\emph{GLOBAL OPTIONS}}}$  category, however I feel this obscures the purpose of the global options and their place in the syntax of `find` commands. Unlike other global options, these options can be placed anywhere in a command and they will execute first and exit without a warning message before any other part of the command can execute.
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
                When <code>find</code> encounters a symbolic link, it will only examine the symbolic link itself and not whatever file it points to.
            </blockquote>
        </details>
    </blockquote>
    <blockquote>
        $\color{cyan}{\text{-L}}$
        <details>
            <summary>Dereference Symbolic Links Where Possible</summary>
            <br>
            <blockquote>
                If <code>find</code> encounters a symbolic link, it will examine the target of the symbolic link unless the symbolic link is broken, in which case it will then examine the symbolic link.
            </blockquote>
        </details>
    </blockquote>
    <blockquote>
        $\color{cyan}{\text{-H}}$
        <details>
            <summary>Do Not Dereference Symbolic Links, except if they are given as command line arguments</summary>
            <br>
            <blockquote>
                If <code>find</code> encounters a symbolic link, it will function identically as the $\color{cyan}{\text{-P}}$ option, unless that symbolic link is given as one of the $\color{red}{\text{\emph{Starting-Points}}}$ on the command line, or it is in the starting point file following the <code>-files0-from</code> $\color{red}{\text{\emph{Global Option}}}$. In these cases where a symbolic link is given as a command line argument, it is dereferenced (where possible) and the target of the symbolic link will be examined instead (as in the $\color{cyan}{\text{-L}}$ option).
            </blockquote>
        </details>
    </blockquote>
</details>

## Debug Options

$\color{cyan}{\text{-D \emph{options}}}$
<details>
<br>
    <blockquote>
        Prints diagnostic information to help troubleshoot the behaviour of the <code>find</code> command. Run <code>find -D help</code> to see a complete list of valid debug options, or consult the official documentation.
    </blockquote>
</details>

## Optimisation

$\color{cyan}{\text{-O\emph{level}}}$
<details>
<br>
    <blockquote>
    Enables query optimisation, specified using <code>-O<i>level</i></code> , where <i>level</i> can be a value from 0 to 3 inclusive. Opimisation is outside the scope of this guide and users should refer to the official documentation for further details.
    </blockquote>
</details>

## Starting Point

<details>
    <summary>Root of the Directory Tree to search. If no starting point is specified, the current working directory '.' is used by default.</summary>
    <br>
    <blockquote>
        Given a directory, <code>find</code> will evaluate the given expression from left to right for every file it encounters using short-circuit evaluation, after which it will move on to the next file. 
        <br>
        <br>
        Multiple directories can be listed so long as they are listed before the first expression argument (<code>find</code> reads until it encounters a '-' to determine the start of the expression). However, if using wildcard globbing to give starting point arguments and a file happens to begin with '-', then <code>find</code> may mistakenly take that file name as an expression argument. Therefore it is safer to prefix wildcards with './' or use absolute path names.
        <br>
        <br>
        Alternatively, instead of listing directories as command arguments, a ASCII NUL separated list of starting points can be given using <code>-files0-from {file | -}</code>. If giving a filename, the file must contain a list separated by single ASCII NUL characters, else it will terminate with an error. If giving '-' as an argument, <code>find</code> will read the list of starting points from <code>stdin</code>.
    </blockquote>
</details>

## Global Options

<details>
    <summary>Options that affect the operation of all tests and actions specified on any part of the command line, and as such should be specified directly after the list of starting points. Global options placed elsewhere will issue a warning message.</summary>
    <br>
    $\color{cyan}{\text{-depth}}$ 
    <blockquote>
        Processes a directory's contents before the directory itself. Effectively performing depth-first-search on a given directory structure.
    </blockquote>
    $\color{cyan}{\text{-mount | -xdev}}$ 
    <blockquote>
        Don't descend into directories on other filesystem. Both <code>-mount</code> and <code>-xdev</code> are equivalent.
    </blockquote>
    $\color{cyan}{\text{-mindepth \emph{levels}}}$ 
    <blockquote>
        Do not apply any tests or actions for any files <i>above</i> a certain depth (level, integer >= 0). Using <code>-mindepth 1</code> evaluates all files except the list of starting points.
    </blockquote>
    $\color{cyan}{\text{-maxdepth \emph{levels}}}$ 
    <blockquote>
        Limits the depth (level, integer >= 0) to which <code>find</code> will descend within each starting point. Using <code>-maxdepth 0</code> means tests and actions will be applied only to the starting points themselves.
    </blockquote>
    <details>
        <summary>Enable or Disable Warnings Caused by Race Conditions. (Useful when examining directories that frequently change)</summary>
        $\color{cyan}{\text{-ignore\_readdir\_race}}$ 
        <blockquote>
            To read from a directory <code>find</code> issues a system call to the kernel using <code>readdir()</code> (or modern <code>getdents()</code>) which returns the basename of all the directory entries. To obtain each file's metadata, it then calls <code>stat()</code> on each filename. 
            <br>
            <br>
            Normally, if this <code>stat()</code> call fails (i.e., if a file is deleted between these two system calls) an error message will be printed. However with this option enabled, <em>no error message will be issued</em>. 
            <br>
            <br>
            This option is useful when examining filesystem directories that change frequently (i.e., mail queues, temporary directories, etc.)
        </blockquote>
        $\color{cyan}{\text{-noignore\_readdir\_race (default)}}$ 
        <blockquote>
            Reverses the effect of the <code>-ignore_readdir_race</code> option.
        </blockquote>
    </details>
    $\color{cyan}{\text{-noleaf}}$
    <details>
        <summary>Necessary Option for Non-Unix-like Filesystems (CD-ROM, MS-DOS, AFS, etc.)</summary>
        <blockquote>
            On Unix filesystems, each directory has 2 + N number of hard links to it (where N is the number of immediate subdirectories). A directory with no subdirectories has only 2 hard links: 
            <ol>
                <li>its own name inside its parent directory.</li>
                <li>the inner '.' directory that references itself.</li>
            </ol> 
            Normally, <code>find</code> optimizes its search such that when it has called <code>stat()</code> on 'A' number of subdirectories (where 'A' is a directory's number of hard links - 2), then it assumes that the remaining entries are <em>not</em> directories, significantly speeding up execution when metadata is not needed.
            <br>
            <br>
            On filesystems that do follow this convention this option is required, otherwise <code>find</code> may skip over some subdirectories.
        </blockquote>
    </details>
</details>

### Positional Options

In the official documentation, there are a few *Positional Options* that rather than affecting the whole command, affect only the parts of the command that follow them. One is deprecated, and therefore will not be mentioned here. Two others affect only the tests that follow them, and therefore I feel it is best to mention them in the context of the tests they modify. 

The two special outliers among these options are $\color{cyan}{\text{-warn (default), -nowarn}}$ 

These toggle warning messages that occur in a few specific circumstances:
- Using the deprecated Global Option $\color{cyan}{\text{-d}}$ for depth-first-search instead of $\color{cyan}{\text{-depth}}$.
- Specifying a Global Option elsewhere than directly after the list of starting points; for example after a test or action later on the command line.
- Using the $\color{cyan}{\text{-name}}$ or $\color{cyan}{\text{-iname}}$ test with a '/' in the pattern as both these options match against a file's basename which cannot contain a '/' character. The only exception to this rule is the basename of the root directory itself.

Any other warnings represent critical errors and therefore cannot be turned off.

## Tests | Actions

These form the core of the `find` utility expressions and define how files are matched, and what actions are performed on each matched file. Through the use of logical operators, longer and more complex expressions can be created by chaining together individual tests or actions.

Logical expressions are evaluated from left to right, grouped by operator precedence (see section *OPERATORS*) and evaluated using short-circuit evaluation. Unless another action is used, the <em>default</em> and <em>implicit</em> action $\color{cyan}{\text{-print}}$ is performed on all files for which the whole expression is true. The only action for which this default <code>-print</code> is <i>not</i> disabled, is for $\color{cyan}{\text{-prune}}$.

### Tests

When evaluating a file within a starting-point directory, return a true or false value according to the result of the test criteria.

To ignore a whole directory tree, use `[Test] (directory to exclude)` $\color{cyan}{\text{-prune}}$. (See *Actions* section for more details)

#### File Name Matching

<details>
    <summary>Matches files based on their basename, or relative path name</summary>
    <br>
    Except when explicitly matching files using Regular Expressions (using $\color{cyan}{\text{-regex}}$ or $\color{cyan}{\text{-iregex}}$), all matching is done using <i>shell patterns</i>. A shell pattern is a string that may contain regular characters alongside the following <i>wildcard characters</i> and as such it must be surrounded by quotes ' or ", in order to prevent the shell itself from expanding those characters.
    <table>
        <tr>
            <td>*</td>
            <td>Matches any characters zero or more times</td>
        </tr>
        <tr>
            <td>?</td>
            <td>Matches any single character</td>
        </tr>
        <tr>
            <td>[...]</td>
            <td>Matches any one of the characters enclosed within the brackets. Inclusive character ranges can be given with a hyphen between the first and last character that will match any character within those ranges. If the first character is '!' or '^' then it will match any character except those in the range.</td>
        </tr>
        <tr>
            <td>[:class:]</td>
            <td>Matches any character that belongs to that class. Valid classes include: alnum, alpha, ascii, blank, cntrl, digit, graph, lower, print, punct, space, upper, word, and xdigit.</td>
        </tr>
        <tr>
            <td>\</td>
            <td>Removes the special meaning of the character that follows it, allowing it to be interpreted as it is.</td>
        </tr>
    </table>
    <details>
        <summary>Basename</summary>
        <br>
        <blockquote>
            $\color{cyan}{\text{-name \emph{pattern}}}$
            <br>
            $\color{cyan}{\text{-iname \emph{pattern}}}$ 
            <br>
            Returns true if the basename of the file matches the pattern. $\color{cyan}{\text{-iname}}$ matches in a case-insensitive manner. 
        </blockquote>
    </details>
    <details>
        <summary>Relative Path Name</summary>
        <br>
        <details>
        <summary>Match Relative Path Name using Shell Patterns</summary>
            <blockquote>
                $\color{cyan}{\text{-path \emph{pattern}}}$
                <br>
                $\color{cyan}{\text{-ipath \emph{pattern}}}$
                <br>
                $\color{cyan}{\text{-wholename \emph{pattern}}}$
                <br>
                $\color{cyan}{\text{-iwholename \emph{pattern}}}$
                <br>
                Returns true if the relative path name beginning from the starting point directory to the basename of the file, matches the given shell pattern. As no file basename ends with a '/', any pattern ending in a '/' will not match anything. $\color{cyan}{\text{-path}}$ and $\color{cyan}{\text{-wholename}}$ are synonyms, and likewise $\color{cyan}{\text{-ipath}}$ and $\color{cyan}{\text{-iwholename}}$ match in a case-insensitive manner.    
            </blockquote>
        </details>
        <details>
            <summary>Match Relative Path Name using Regular Expressions</summary>
            <br>
            <blockquote>
                $\color{cyan}{\text{-regex \emph{expr}}}$
                <br>
                $\color{cyan}{\text{-iregex \emph{expr}}}$
                <br>
                Returns true if the relative path name beginning from the starting point directory to the file basename, matches the given regular expression. As with $\color{cyan}{\text{-path}}$, a regular expression matching a terminal slash '/' will not match any file. Likewise, $\color{cyan}{\text{-iregex}}$ matches in a case-insensitive manner.
                <details>
                    <summary>Changing Regular Expression Syntax</summary>
                    <br>
                    <blockquote>
                        The positional option $\color{cyan}{\text{-regextype \emph{name}}}$ changes the regular expression syntax for all following regular expressions. <code>-regextype emacs</code> is in effect by default, however the other possible arguments include: <code>posix-awk, posix-basic, posix-egrep, posix-extended</code>.
                    </blockquote>
                </details>
            </blockquote>
        </details>
    </details>
</details>

#### Link Matching

Depending on which *Link Options* (if any) are selected, the files to be examined may or may not include the symbolic links themselves. Please verify that your intended option is in effect before applying these tests.

<details>
    <summary>Symbolic Links</summary>
    <br>
    <blockquote>
        $\color{cyan}{\text{-lname \emph{pattern}}}$
        <br>
        $\color{cyan}{\text{-ilname \emph{pattern}}}$
        <br>
        Returns true if the file under examination is a symbolic link $\color{red}{\text{\emph{AND}}}$ it points to a file who's name matches the given pattern. $\color{cyan}{\text{-ilname}}$ matches the $\color{red}{\text{\emph{target file's}}}$ name (and not name of the link itself) in a case-insensitive manner.
    </blockquote>
</details>

<details>
    <summary>Hard Links</summary>
    <br>
    <blockquote>
        $\color{cyan}{\text{-samefile \emph{name}}}$
        <br>
        Returns true if the file under examination is a hard link to the same inode as the given <i>filename</i>. If a hard link is found, then it must be on the same filesystem as the given file.
        <br>
        <br>
        You can also test if a file matches a given inode number using $\color{cyan}{\text{-inum \emph{number}}}$, however it is often easier to use the <code>-samefile</code> option above than this option.
    </blockquote>
    <blockquote>
        $\color{cyan}{\text{-links }\pm\text{\emph{number}}}$
        <br>
        Returns true if the file under examination has greater than (+), fewer than (-) or an equal <i>number</i> of hard links to it. 
    </blockquote>
</details>

#### File Filtering

Allows the filtering of files based on their metadata provided by system calls to `stat()`.

<details>
    <summary>Time</summary>
    <br>
</details>

### Copyright
Copyright © 2026 Dmytro Politov \
Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Document License, Version 3 or any later version published by the Free Software Foundation. [GNU General Pulic License](https://www.gnu.org/licenses/gpl-3.0.html)