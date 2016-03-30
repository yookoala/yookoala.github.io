---
layout: post
title:  "syscall.Utsname [65]int8 to string problem"
date:   2016-03-30 10:30:00 +0800
categories: golang
---

As you might have noticed, [syscall.Uname](https://golang.org/pkg/syscall/#Uname)
sets the variables into a [*syscall.Utsname](https://golang.org/pkg/syscall/#Utsname)
struct with fields in type `[65]int`.

It is not difficult to display the output, but the code is too tiny for a
library of any sort. Just share the code snippet here:

```go
// UtsnameStr reads [65]int8 into string
func UtsnameStr(in []int8) string {
	i, out := 0, make([]byte, 0, len(in))
	for ; i < len(in); i++ {
		if in[i] == 0 {
			break
		}
		out = append(out, byte(in[i]))
	}
	return string(out)
}

// PrintUtsname prints the current system info in uname call
func PrintUtsname() {
	utsname := new(syscall.Utsname)
	err := syscall.Uname(utsname)
	if err != nil {
		panic(fmt.Sprintf("unable to run uname (%s)", err))
	}
	fmt.Printf(
		"OS=%s\nNodename=%s\nRelease=%s\nVersion=%s\nMachine=%s\n",
		UtsnameStr(utsname.Sysname[:]),
		UtsnameStr(utsname.Nodename[:]),
		UtsnameStr(utsname.Release[:]),
		UtsnameStr(utsname.Version[:]),
		UtsnameStr(utsname.Machine[:]))
}
```
