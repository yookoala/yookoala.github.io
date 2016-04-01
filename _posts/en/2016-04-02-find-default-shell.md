---
layout: post
title:  "HOWTO: Find out the current user default shell"
date:   2016-04-02 13:01:00 +0800
categories: shell
---

A snippet to get the current user's default shell:

```
getent passwd $LOGNAME | cut -d: -f7
```

This is the only method to have extract the default shell setting.

Some would suggest to use `echo $SHELL`. The command only show your
**current shell**. If you open another shell in it, the `$SHELL` changes
accordingly.
