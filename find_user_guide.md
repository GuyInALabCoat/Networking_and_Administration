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
        <br>
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
    <details>
        <summary>Necessary Option for Non-Unix-like Filesystems (CD-ROM, MS-DOS, AFS, etc.)</summary>
        <blockquote>
            <br>
            $\color{cyan}{\text{-noleaf}}$
            <br>
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

<details>
    <summary>Toggle warning messages</summary>
    <br>
        In the official documentation, there are a few <i>Positional Options</i> that rather than affecting the whole command, affect only the parts of the command that follow them. One is deprecated, and therefore will not be mentioned here. Two others affect only the tests that follow them, and therefore I feel it is best to mention them in the context of the tests they modify. 
        <br>
        <br>
        The two special outliers among these options are $\color{cyan}{\text{-warn (default), -nowarn}}$ 
        <br>
        <br>
        These toggle warning messages that occur in a few specific circumstances:
        <ul>
            <li>Using the deprecated Global Option $\color{cyan}{\text{-d}}$ for depth-first-search instead of $\color{cyan}{\text{-depth}}$.</li>
            <li>Specifying a Global Option elsewhere than directly after the list of starting points; for example after a test or action later on the command line.</li>
            <li>Using the $\color{cyan}{\text{-name}}$ or $\color{cyan}{\text{-iname}}$ test with a '/' in the pattern as both these options match against a file's basename which cannot contain a '/' character. The only exception to this rule is the basename of the root directory itself.</li>
        </ul>
        Any other warnings represent critical errors and therefore cannot be turned off.
</details>

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
    <br>
    <br>
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
            <td>Matches any one of the characters enclosed within the brackets. Inclusive character ranges can be given with a hyphen between the first and last character that will match any character within those ranges (e.g,  [a-z], [0-9], etc.). If the first character is '!' or '^' then it will match any character except those in the range.</td>
        </tr>
        <tr>
            <td>[:class:]</td>
            <td>Matches any character that belongs to that class. Valid classes include: <i>alnum</i>, <i>alpha</i>, <i>ascii</i>, <i>blank</i>, <i>cntrl</i>, <i>digit</i>, <i>graph</i>, <i>lower</i>, <i>print</i>, <i>punct</i>, <i>space</i>, <i>upper</i>, <i>word</i>, and <i>xdigit</i>.</td>
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
        <blockquote>
            <details>
            <summary>Match Relative Path Name using Shell Patterns</summary>
                <br>
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
                        <br>
                        <br>
                        <details>
                            <summary>Changing Regular Expression Syntax</summary>
                                <br>
                                <blockquote>
                                    The positional option $\color{cyan}{\text{-regextype \emph{name}}}$ changes the regular expression syntax for all following regular expressions. <code>-regextype emacs</code> is in effect by default, however the other possible arguments include: <code>posix-awk, posix-basic, posix-egrep, posix-extended</code>.
                            </blockquote>
                    </details>
                </blockquote>
            </details>
        </blockquote>
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

Allows the filtering of files based on their metadata.

<details>
    <summary>Time</summary>
    <br>
    Each file records the time whenever the following operations are performed on the file:
    <ul>
        <li>access (a): The file's contents are read</li>
        <li>change (c): The file's metadata or attributes are changed</li>
        <li>modify (m): The file's contents are changed (and as a consequence, so too is its metadata)</li>
    </ul>
    <blockquote>
        Options that accept numbers as arguments can also accept ranges using $\color{cyan}{\pm\text{\emph{number}}}$ to indicate times that are older (+) or newer (-) than a given <i>number</i> of days/minutes. Integer and decimal numbers are accepted, however decimals should only be used with ranges as they represent the decimal fraction of a given time value.
    </blockquote>
    <details>
        <summary>Minutes</summary>
        <br>
        <blockquote>
            $\color{cyan}{\text{-amin }\pm\text{\emph{number}}}$
            <br>
            $\color{cyan}{\text{-cmin }\pm\text{\emph{number}}}$
            <br>
            $\color{cyan}{\text{-mmin }\pm\text{\emph{number}}}$
            <br>
            Returns true if the file was accessed (a), changed (c) or modified (m), more than (+), less than (-) or exactly <i>number</i> of minutes ago.
        </blockquote>
    </details>
    <details>
        <summary>Days</summary>
        <br>
        <blockquote>
            $\color{cyan}{\text{-atime }\pm\text{\emph{number}}}$
            <br>
            $\color{cyan}{\text{-ctime }\pm\text{\emph{number}}}$
            <br>
            $\color{cyan}{\text{-mtime }\pm\text{\emph{number}}}$
            <br>
            Returns true if the file was accessed (a), changed (c) or modified (m), more than (+), less than (-) or exactly <i>number</i> of days ($n * 24$ hours) ago.
        </blockquote>
    </details>
    As of version 4.9.0, there appears to be a <a href="https://savannah.gnu.org/bugs/?23065">known bug</a> such that $\color{cyan}{\text{-daystart}}$ sets the reference point <em>not</em> to the start of today, but rather the <em>start of tomorrow</em> (00:00:00 tomorrow). When $\color{cyan}{\text{-daystart}}$ is used together with $\color{cyan}{\text{-[acm]min}}$ or $\color{cyan}{\text{-[acm]time}}$, the tests behave as before, except they use the new reference time rather than the current time.
    <br>
    <br>
    <details>
        <summary>Comparing Timestamps</summary>
        <br>
        <blockquote>
            $\color{cyan}{\text{-newer[X][Y] \emph{reference\_file}}}$
            <br>
            Return true if timestamp 'X' of the file under examination is newer than timestamp 'Y' of the reference file, and returns false if timestamp 'X' is as old or older than than timestamp 'Y'.
            <br>
            <br>
            'X' and 'Y' must be any of the following letters, and the letters determine which timestamp they refer to:
            <ul>
                <li>access (a): The file's contents are read</li>
                <li>change (c): The file's metadata or attributes are changed</li>
                <li>modify (m): The file's contents are changed (and as a consequence, so too is its metadata)</li>
            </ul>
            <details>
                <summary style="color:#FFFFFF">Shortcuts commands to compare (access, change, or modification) time of the current file with the last modification time of the reference file</summary>
                <blockquote>
                    <br>
                    $\color{cyan}{\text{-anewer \emph{reference\_file}}}$ is equivalent to $\color{cyan}{\text{-neweram}}$
                    <br>
                    $\color{cyan}{\text{-cnewer \emph{reference\_file}}}$ is equivalent to $\color{cyan}{\text{-newercm}}$
                    <br>
                    $\color{cyan}{\text{-newer \emph{reference\_file}}}$ is equivalent to $\color{cyan}{\text{-newermm}}$
                </blockquote>
            </details>
            <details>
                <summary style="color:#FFFFFF">Compare using a time literal</summary>
                <blockquote>
                    <br>
                    If using $\color{cyan}{\text{-newer[X]t \emph{time}}}$ where 'X' can be any of the options presented previously, then you can provide a timestamp literal (see the <a href="https://www.gnu.org/software/findutils/manual/find.pdf#Date%20input%20formats">official documentation</a> for valid date syntax), instead of a reference file.
                </blockquote>
            </details>
        </blockquote>
    </details>
</details>
<details>
    <summary>Size</summary>
    <br>
    $\color{cyan}{\text{-size }\pm\text{number\emph{[bckwMG]}}}$
    <br>
    <blockquote>
        <br>
        Returns true if the file uses <i>number</i> units of space, rounded up. The one-character suffix determines the size of the memory block:
        <br>
        <ul>
            <li>b (default) : 512-byte blocks</li>
            <li>c : bytes</li>
            <li>w : 2-byte words</li>
            <li>k : Kibibytes (KiB, units of 1024 bytes)</li>
            <li>M : Mebibytes (MiB, units of 1024<sup>2</sup> bytes</li>
            <li>G : Gibibytes (GiB, units of 1024<sup>3</sup> bytes</li>
        </ul>
        Note that +<i>number</i> matches files $\ge$ the given size, while -<i>number</i> matches files $\le$ a given size - 1. Therefore as shown in the official documentation <code>-size -1M</code> will only match files that are 0M in size (i.e., empty files), while <code>-size -1048576c</code> will match files from 1048575 bytes to 0 bytes in size.
    </blockquote>
    $\color{cyan}{\text{-empty}}$
    <br>
    Returns true if the candidate file is empty and either a regular file or a directory.
</details>
<details>
    <summary>Type</summary>
    <br>
    $\color{cyan}{\text{-type \emph{c}}}$
    <br>
    Returns true if the candidate file is of type <i>c</i>:
    <blockquote>
        <br>
        <ul>
            <li>b : block device file (buffered)</li>
            <li>c : character device file (unbuffered)</li>
            <li>d : directory</li>
            <li>P : named pipe (FIFO)</li>
            <li>f : regular file</li>
            <li>l : symbolic link (with -L, true only for broken links)</li>
            <li>s : socket</li>
        </ul>
    </blockquote>
    Note: multiple file types can be listed together as a comma separated list and the expression will return true if the file matches any of the types (e.x., <code>-type f,d,l</code> matches all regular files, directories, and symbolic links.
    <br>
    <br>
    <details>
        <summary>$\color{cyan}{\text{-xtype \emph{c}}}$ as a counterpoint to <code>-type</code></summary>
        <br>
        $\color{cyan}{\text{-xtype \emph{c}}}$ behaves the same as <code>-type</code>, unless the file is a symbolic link, in which case it will examine the file the <code>-type</code> does not check. This case be combined with the link deference options <code>-P</code> (never dereference links) and <code>-L</code> (always dereference links) to search for broken links.
        <br>
        <br>
        For example:
        <br>
        <blockquote>
            $\color{cyan}{\text{-P -xtype l}}$ returns true if the symbolic link is broken
            <br>
            $\color{cyan}{\text{-P -xtype X}}$ returns true if the target file is of type 'X
            <br>
            $\color{cyan}{\text{-L -xtype l}}$ always true 
            <br>
            $\color{cyan}{\text{-L -xtype X}}$ this is actually an <a href="https://savannah.gnu.org/bugs/?65349">issue pointed out by another user</a>, where because the symbolic link is dereferenced this expression always returns false. On healthy links <code>-xtype X</code> returns false because the link is not of type 'X', and on broken links <code>find</code> tries to resolve the broken link, fails, and reverts to the optional behaviour of <code>-L</code> where it examines the link itself, which also returns false because the link is not of type 'X'. Therefore this expression behaves almost identically to <code>find -L -type X</code>, except that it will ignore all symmetric links.
        </blockquote>
    </details>
</details>
<details>
    <summary>Owner</summary>
    <br>
    <blockquote>
        $\color{cyan}{\text{-user \emph{username}}}$
        <br>
        $\color{cyan}{\text{-group \emph{groupname}}}$
        <br>
        Returns true if the file is owned by <i>username</i>, or by <i>groupname</i>. 
    </blockquote>
    <blockquote>
        $\color{cyan}{\text{-uid }\pm\text{\emph{n}}}$
        <br>
        $\color{cyan}{\text{-gid }\pm\text{\emph{n}}}$
        <br>
        Returns true if the file's numberic <i>userid</i>, or by <i>groupid</i> is $\pm n$. While the official documentation states that <code>-user</code> and <code>-group</code> can also take numeric IDs, <code>-uid</code> and <code>-gid</code> are better suited as they also support ranges (unlike the first two).
    </blockquote>
    <blockquote>
        $\color{cyan}{\text{-nouser}}$
        <br>
        $\color{cyan}{\text{-nogroup}}$
        <br>
        Returns true if the file's user ID (or group ID) does not correspond to any user (or group).
    </blockquote>
</details>
<details>
    <summary>File Permissions</summary>
    <br>
    Users should be cautious when using these options as they may return false reports due to additional restrictions outside the scope of these system calls; for example, restrictions on NFS servers, race conditions, and available memory.
    <br>
    <details>
        <summary>Test file permissions via <code>access</code> system call</summary>
        <br>
        <blockquote>
            $\color{cyan}{\text{-readable}}$
            <br>
            $\color{cyan}{\text{-writable}}$
            <br>
            $\color{cyan}{\text{-executable}}$
            <br>
            Returns true if the current user can read/write/execute the candidate file.
        </blockquote>
    </details>
    <details>
        <summary>Test file permissions via mode bits</summary>
        <br>
        $\color{cyan}{\text{-perm \emph{pmode}}}$ where <i>pmode</i> is either the symbolic or numeric form of the mode and may be optionally prefixed by '-' or '/'.
        <br>
        <blockquote>
            <br>
            If the <i>pmode</i> starts with '-', then the test returns true if <em>at least <b>all</b></em> the file's mode bits match the given <i>pmode</i>. (i.e., <code>pmode -554</code> matches files with modes 0554, 0555, 0654, 0777, etc.)
            <br>
            <br>
            If the <i>pmode</i> starts with '-/', then the test returns true if <em><b>any</b></em> of the file's mode bit match the given <i>pmode</i>. (i.e., <code>pmode /022</code> returns true if the file is writable by the group, other, or both.)
            <br>
            <br>
            And if there is no prefix, then the test returns true <em>only if</em> all the file's mode bits match exactly.
        </blockquote>
    </details>
</details>
<details>
    <summary>Filesystem</summary>
    <br>
    <blockquote>
        $\color{cyan}{\text{-fstype \emph{type}}}$
        <br>
        Returns true if the file is on a filesystem of the given <i>type</i>. Can be used together with <code>-prune</code> to avoid searching other filesystems (however, <code>{ -xdev | -mount }</code> are better suited.
    </blockquote>
</details>

### Actions

By default, if no action is explicitly given, <code>-print</code> is implemented by default for all files that make the whole expression true.

#### Print

<details>
    <summary>Printing filenames to <code>stdout</code></summary>
    <br>
    <blockquote>
        $\color{cyan}{\text{-print, -print0}}$
        <br>
        <code>-print</code> prints the whole relative path name to <code>stdout</code>, followed by a newline character. <code>-print0</code> also prints the whole relative path name, however it terminates the string with a null instead of a newline character. Therefore, if it is at all possible that filenames contain newlines, then <code>-print0</code> should be used instead.
    </blockquote>
</details>
<details>
    <summary>Printing filenames to a file</summary>
    <br>
    <blockquote>
        $\color{cyan}{\text{-fprint \emph{file}, -fprint0 \emph{file}}}$
        <br>
        Much like -print, <code>-fprint</code> also prints the whole relative path name, followed by a newline, however the output is writen to the given <i>file</i> instead of <code>stdout</code>. 
        <br>
        <br>
        Note: an empty file is created in all circumstances (even when no output is produced), and therefore an existing file will have its contents written over by this output. Likewise, as with -print, if there is any chance that filenames may contain newlines, then <code>-fprint0</code> should be used.
    </blockquote>
</details>
<details>
    <summary>Printing file information</summary>
    <br>
    <blockquote>
        $\color{cyan}{\text{-ls}}$
        <br>
        Prints the file metadata in <code>ls -dils</code> format to <code>stdout</code>.
        <br>
        <br>
        $\color{cyan}{\text{-fls \emph{file}}}$
        <br>
        Writes the same output as <code>-ls</code> to the given file. If the given file already exists, then it is overwritten, else a new file is always created, even if no text is output.
    </blockquote>
</details>
<details>
    <summary>Printing using <code>-printf</code> format</summary>
    <br>
        <blockquote>
            $\color{cyan}{\text{-printf \emph{format}, -fprintf \emph{file format}}}$
            <br>
            Prints the output in a format similar to the <code>printf</code> C function. There are various directives that enable the printing of both file names and metadata, however it is fairly extensive and therefore it's encouraged to consult the <a href="https://www.gnu.org/software/findutils/manual/find.pdf#Format%20Directives">official documentation</a> for a complete list of details
        </blockquote>
</details>

#### Arbitrary Command Execution on Files

<details>
    <summary>Secure Command Execution</summary>
    <blockquote>
        <details>
            <summary>Process Files One at a Time</summary>
            <br>
            <blockquote>
                $\color{cyan}{\text{-execdir \emph{command} ';'}}$
                <br>
                Parses all arguments between <code>-execdir</code> and the first ';' (semicolon) as a single command. Filenames passed into the command can be expanded using the <code>{}</code> construction, where the two curly brackets are replaced with the current file's basename (not the relative path name) during execution. Each filename is prepended with './' and therefore a file called <code>temp.txt</code> found in directory <code>foo/bar/</code> will be evaluated as <code>./temp.txt</code>. However as './' represent special characters in the shell, the whole construction must be quoted as <code>'./'</code> to prevent their expansion by the shell. Likewise the terminal semicolon should be sanitised, which is why it is quoted in the above construction.
                <br>
                <br>
                When using <code>-execdir</code>, you must ensure that the PATH variable contains only absolute directory names, and that there is no empty directory element either at the beginning, end or in the middle of the list.
                <br>
                <br>
                Additionally, passing untrusted data (such as file names) to commands (i.e., 'sh') which interpret arguments as commands to be further interpreted should be avoided wherever possible. If untrusted data must be passed along, then it can be more safely handled using <code>-execdir sh -c 'command "$@"' sh {} ';'</code> where the arguments are expanded using the "$@" construct.
                <br>
                <details>
                    <summary>Execute with Confirmation</summary>
                    <blockquote>
                        $\color{cyan}{\text{-okdir \emph{command} ';'}}$
                        <br>
                        Execute in the same manner as <code>-execdir</code>, except that the user is queried first to confirm before execution can proceed.
                        <br>
                        <br>
                        Cannot be implemented together with the <code>-files0-from</code> option.
                    </blockquote>
                </details>
            </blockquote>
        </details>
        <details>
            <summary>Process Multiple Files at Once</summary>
            <br>
            <blockquote>
                $\color{cyan}{\text{-execdir \emph{command} '{}' +}}$
                <br>
                Similar execution as the command to process files one at a time, except that only one <code>'{}'</code> construct is allowed and it must appear at the end, immediately before the '+'. 
                <br>
                <br>
                As in the sequential file processing command, each file name is prepended with './', in contrast however, the <code>'{}' +</code> construct expands to the full list of matching file names and the whole expression is evaluated as one entire command. When multiple files can be processed at once this provides much greater efficiency over the serial <code>-execdir</code> as the command is called only once rather than repeatedly for each file.
            </blockquote>
        </details>
    </blockquote>
</details>
<details>
    <summary>Insecure Command Execution</summary>
    <blockquote>
        <details>
            <summary>Process Files One at a Time</summary>
            <br>
            <blockquote>
                $\color{cyan}{\text{-exec \emph{command} ';'}}$
                <br>
                Parses all arguments between <code>-exec</code> and the first ';' (semicolon) as a single command. Filenames passed into the command can be expanded using the <code>{}</code> construction. In contrast to the secure <code>-execdir</code> command, the '{}' construct expands to the relative pathname rooted in the directory from where <code>find</code> was called. Each file name is <b>not</b> prepended with './' and therefore a file called <code>temp.txt</code> found in directory <code>foo/bar/</code> will be evaluated as <code>foo/bar/temp.txt</code>. 
                <br>
                <br>
                This presents security concerns as a file name beginning with a '-' will be interpreted as a command option and not a file. Alternatively, if in the time between when a file is first matched and the command is executed, the file is changed to a symbolic link then the command may be executed in a location that a user did not intend. Both of these concerns highlight why <code>-execdir</code> should be used over <code>-exec</code> wherever possible.
                <br>
                <br>
                <details>
                    <summary>Execute with Confirmation</summary>
                    <br>
                    <blockquote>
                        $\color{cyan}{\text{-ok \emph{command} ';'}}$
                        <br>
                        Execute in the same manner as <code>-exec</code>, except that the user is queried first to confirm before execution can proceed.
                        <br>
                        <br>
                        Cannot be implemented together with the <code>-files0-from</code> option.
                    </blockquote>
                </details>
            </blockquote>
        </details>
        <details>
            <summary>Process Multiple Files at Once</summary>
            <br>
            <blockquote>
                $\color{cyan}{\text{-exec \emph{command} '{}' +}}$
                <br>
                Similar execution as the command to process files one at a time, except that only one <code>'{}'</code> construct is allowed and it must appear at the end, immediately before the '+'. 
                <br>
                <br>
                In contrast to sequential execution, the <code>'{}' +</code> construct expands to the full list of matching relative path names beginning in one of the starting directories and the whole expression is evaluated as one entire command.
            </blockquote>
        </details>
    </blockquote>
</details>

#### Delete

<details>
    <summary>Deleting Matched Files</summary>
    <blockquote>
        $\color{cyan}{\text{-delete}}$
        <br>
        Deletes the file or directory under consideration. Returns true if the removal was successful, else an error message will be printed and <code>find</code> will terminate with a nonzero exit status.
        <br>
        <br>
        When implemented, the <code>-depth</code> option will be activated, and the <code>-prune</code> option will be deactivated.
    </blockquote>
</details>

### Copyright
Copyright © 2026 Dmytro Politov \
Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Document License, Version 3 or any later version published by the Free Software Foundation. [GNU General Pulic License](https://www.gnu.org/licenses/gpl-3.0.html)