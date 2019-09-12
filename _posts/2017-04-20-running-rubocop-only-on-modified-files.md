---
title: Running Rubocop Only On Modified Files
layout: post
date: '2017-04-20 22:44:00'
image: "/assets/images/rubocop.jpg"
tag:
- Git
- Github
- Rubocop
- Refactoring
- Software Engineering
star: false
description: A better way to use Rubocop
category: blog
---

![Rubocop]({{ site.url }}/{{ site.rubocop }})

If you are using Rubocop as a code analyzer and following its guidelines as a standard for your project, then like me you also might have faced the annoyance of running Rubocop on each and every modified file individually. Its boring and a little time consuming as well. Hence, its lot more productive to run it only on the updated files and for once.
There are two ways to run Rubocop on modified files:

**[1] Before committing**

To view the list of files that have been modified, you can do:

```
git ls-files -m
```

This lists all the modified files regardless of the extensions as well as the deleted files. To exclude deleted files you can do:

```
git ls-files -m | xargs ls -1 2>/dev/null
```

Again, if you wish to view only the modified files with .rb extension, you can do:

```
git ls-files -m | xargs ls -1 2>/dev/null | grep '\.rb$'
```

Hence, the final command to run Rubocop for modified files is:

```
git ls-files -m | xargs ls -1 2>/dev/null | grep '\.rb$' | xargs rubocop
```

**[2] After committing**

There are two ways to view the list of changed files after you have committed. First is to compare the changes with master or you can compare it with the current upstream.

To compare with master:

```
git diff-tree -r --no-commit-id --name-only head origin/master
```

To compare with current upstream:

```
git diff-tree -r --no-commit-id --name-only @\{u\} head
```

*Note: This won’t work if you haven’t yet set up a remote branch.*

Hence, the file command to run Rubocop for modified files is:

```
git diff-tree -r --no-commit-id --name-only head origin/master | xargs rubocop
```

or

```
git diff-tree -r --no-commit-id --name-only @\{u\} head | xargs rubocop
```

With this, you will be saving quite a time. To top if off, you can also create aliases for these.