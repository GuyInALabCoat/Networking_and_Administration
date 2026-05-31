# Introduction
The `find` utility that is a part of GNU findutils package is an incredibly powerful tool for discovering files (and other *file-like* objects, such as directories, symbolic links, and other directory nodes), filtering them, and then performing some operation on each of them.

However, I have found much of the official documentation on this utility difficult to use as they introduce concepts and their various options out of order, such that it is difficult to piece them together into more complex commands.

Here I intend to create a user-guide of sorts that introduces modifiers to the `find` command in order of decreasing scope, so that even complex commands can be built-up piece by piece in a clear and concise way.

Much of this guide is based on the official documentation provided by GNU, the Free Software Foundation, Inc. and the official Linux Documentation [find(1)-Linux manual page](https://man7.org/linux/man-pages/man1/find.1.html#COPYRIGHT) and  [GNU Findutils](https://www.gnu.org/software/findutils/manual/find.pdf) and users should always refer to these documents in troubleshooting their issues.

### Copyright
Copyright © 2026 Dmytro Politov
Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Document License, Version 3 or any later version published by the Free Software Foundation. [GNU General Pulic License](https://www.gnu.org/licenses/gpl-3.0.html)