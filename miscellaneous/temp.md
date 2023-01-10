# temp

#### Build

GCC options You may ask GCC to generate preprocessed source code using the -E option. Preprocessed code contains header file expansions and reduces the need to hop-skip through nested include files to expand multiple levels of macros. Here is a usage example to preprocess drivers/char/mydrv.c and produce expanded output in

mydrv.i:

bash> gcc -E drivers/char/mydrv.c -D**KERNEL** -Iinclude -Iinclude/asm-x86/mach-default > mydrv.i

The include paths specified using the -I option depend on the header files included by your code.

GCC generates assembly listings if you use the -S option. To generate an assembly listing in mydrv.s for drivers/char/mydrv.c,

do this:

bash> gcc -S drivers/char/mydrv.c -D**KERNEL** -Iinclude -Ianother/include/path



git

Cheatsheet

1. git log --pretty=oneline --abbrev-commit
2. Assuming we want to include all of our changes in one git commit, you can use git to add the changed file to the list of changes to be committed (the "staging area"):

```
git add <file>
If you run `git diff` again, you'll notice it doesn't list any changed files. That's because, by default, git diff only shows you the unstaged changes. If you run this command instead, you'll see the staged changes:
​
git diff --cached
That command will show you the changes to be committed.
```

1. You can also add parts of files to the staging area by using the following flag:

git add -p That will allow you to add hunks of the file to the staging area, or even edit hunks that you want to commit. This is useful, for instance, if you've made whitespace changes, and also made a camel-case variable name fix, but those changes are on the same line. You can edit the line to revert the camel-case name change, and just add the whitespace change to the staging area. Then when you commit, you will just be committing the whitespace change.
