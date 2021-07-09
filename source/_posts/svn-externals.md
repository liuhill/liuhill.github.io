---
title: svn externals
date: 2016-09-09 14:32:47
tags:
---
# Externals Definitions
Sometimes it is useful to construct a working copy that is made out of a number of different checkouts. For example, you may want different subdirectories to come from different locations in a repository, or perhaps from different repositories altogether. You could certainly setup such a scenario by hand—using `svn checkout` to create the sort of nested working copy structure you are trying to achieve. But if this layout is important for everyone who uses your repository, every other user will need to perform the same checkout operations that you did.

Fortunately, Subversion provides support for externals definitions. An externals definition is a mapping of a local directory to the URL—and possibly a particular revision—of a versioned resource. In Subversion, you declare externals definitions in groups using the `svn:externals` property. You can create or modify this property using `svn propset` or `svn propedit` (see [the section called “Why Properties?”](http://svnbook.red-bean.com/en/1.0/ch07s02.html#svn-ch-7-sect-2.1)). It can be set on any versioned directory, and its value is a multi-line table of subdirectories (relative to the versioned directory on which the property is set) and fully qualified, absolute Subversion repository URLs.

```
      $ svn propget svn:externals calc
      third-party/sounds             http://sounds.red-bean.com/repos
      third-party/skins              http://skins.red-bean.com/repositories/skinproj
      third-party/skins/toolkit -r21 http://svn.red-bean.com/repos/skin-maker
```

he convenience of the svn:externals property is that once it is set on a versioned directory, everyone who checks out a working copy with that directory also gets the benefit of the externals definition. In other words, once one person has made the effort to define those nested working copy checkouts, no one else has to bother—Subversion will, upon checkout of the original working copy, also checkout the external working copies.

Note the previous externals definition example. When someone checks out a working copy of the calc directory, Subversion also continues to checkout the items found in its externals definition.


```
$ svn checkout http://svn.example.com/repos/calc
A  calc
A  calc/Makefile
A  calc/integer.c
A  calc/button.c
Checked out revision 148.

Fetching external item into calc/third-party/sounds
A  calc/third-party/sounds/ding.ogg
A  calc/third-party/sounds/dong.ogg
A  calc/third-party/sounds/clang.ogg
…
A  calc/third-party/sounds/bang.ogg
A  calc/third-party/sounds/twang.ogg
Checked out revision 14.

Fetching external item into calc/third-party/skins
…

```
If you need to change the externals definition, you can do so using the regular property modification subcommands. When you commit a change to the `svn:externals` property, Subversion will synchronize the checked-out items against the changed externals definition when you next run `svn update`. The same thing will happen when others update their working copies and receive your changes to the externals definition.

The `svn status` command also recognizes externals definitions, displaying a status code of X for the disjoint subdirectories into which externals are checked out, and then recursing into those subdirectories to display the status of the external items themselves.

The support that exists for externals definitions in Subversion today can be a little misleading, though. First, an externals definition can only point to directories, not files. Second, the externals definition cannot point to relative paths (paths like ../../skins/myskin). Third, the working copies created via the externals definition support are still disconnected from the primary working copy (on whose versioned directories the `svn:externals` property was actually set). And Subversion still only truly operates on non-disjoint working copies. So, for example, if you want to commit changes that you've made in one or more of those external working copies, you must run `svn commit` explicitly on those working copies—committing on the primary working copy will not recurse into any external ones.

Also, since the definitions themselves use absolute URLs, moving or copying a directory to which they are attached will not affect what gets checked out as an external (though the relative local target subdirectory will, of course, move with renamed directory). This can be confusing—even frustrating—in certain situations. For example, if you use externals definitions on a directory in your /trunk development line which point to other areas of that same line, and then you use `svn copy` to branch that line to some new location /branches/my-branch, the externals definitions on items in your new branch will still refer to versioned resources in /trunk. Also, be aware that if you need to re-parent your working copy (using svn switch --relocate), externals definitions will not also be re-parented.

# delete
```
svn propdel -R svn:externals
```

```
svn propdel PROPNAME
```
