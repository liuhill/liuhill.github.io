---
title: RSYNC TO EXFAT DRIVE
date: 2018-12-29 17:15:33
tags:
---
RSYNC TO EXFAT DRIVE
These options are friendly to sync to/from an ExFAT drive:
```
rsync -vrltD --progress --stats /source/a/ /dest/a
```
`-vrltD`
options from `-a` friendly with EXFAT.

non-ExFAT rsync:
If one uses the standard rsync options like:
```
rsync -a
```
they don’t work with an EXFAT drive. You’ll get errors like:
```
RSYNC: MKSTEMP … FAILED: FUNCTION NOT IMPLEMENTED (38)
```
because EXFAT doesn’t understand permissions, owners, or groups.

RSYNC PROGRESS INDICATOR
In general, rsync progress can be observed on a per-file basis using commands starting with:
```
rsync -av --progress
```
Overall rsync progress can be observed by:
```
rsync -a --info=progress2
```
