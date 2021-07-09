---
title: 'NFS mount.nfs: access denied by server while mountin'
date: 2017-12-28 14:44:51
tags:
---
setup NFS but when trying to mount getting the following errors

    mount.nfs: access denied by server while mounting <server ip>:/exports

The

    showmount -e <server ip>

command returns the following

    /exports <client ip>
