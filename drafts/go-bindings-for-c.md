---
layout: post
title:  "Fighting with cgo: bindings for audio streaming library"
date:   2020-04-06 21:06:20 +0100
categories: programming
tags: go, cgo, bindings, roc
---
- I wanted a tshirt, so I signed up for hacktoberfest. While my other 3 commits were just adding icons to treemacs or fixing minor bugs, one of them was writing go bindings for `roc`, a real-time audio streaming library.
- Trying to research the ways of creating bindings, I stumbled upron c-for-go - an automatic C-Go Bindings Generator. It seemed to be the most advanced generator of this kind that existed and we decided to give it a shot.

- nice, but not for our purposes
2) hand-written bindings
   2a) unions
how to debug:
1. Use gdb
2. Build test : `go test -c`
2. Turn off optimization: `go test -c ./roc -gcflags '-N -l'`



# Side-note: c-for-go usage example

Given: headers and library written in c, `roc.yml` - configuration file for c-for-go with type hints etc.
One will generate: folder of the same name that `*.yml` file is, in our case: `roc/` with all types and function signatures converted to go.

the main thing is writing that roc.yml file, this is ours:

```
---
GENERATOR:
  PackageName: roc
  PackageDescription: "Package roc provides Go bindings for libroc"
  PackageLicense: "MIT"
  Options:
    SafeStrings: true
  Includes: ["roc/receiver.h", "roc/sender.h", "roc/log.h"]
  FlagGroups:
    - {name: "LDFLAGS", flags: [
      "-lroc",
    ]}

PARSER:
  SourcesPaths:
    - /usr/include/roc/address.h
    - /usr/include/roc/config.h
    - /usr/include/roc/context.h
    - /usr/include/roc/frame.h
    - /usr/include/roc/log.h
    - /usr/include/roc/platform.h
    - /usr/include/roc/receiver.h
    - /usr/include/roc/sender.h

TRANSLATOR:
  PtrTips:
    function:
      - {target: "^roc_address_init$", tips: [ref,0,ref,0]}
      - {target: "^roc_address_family$", tips: [ref]}
      - {target: "^roc_address_ip$", tips: [ref,arr]}
      - {target: "^roc_context", tips: [ref]}
      - {target: "^roc_receiver_open$", tips: [ref, ref]}
      - {target: "^roc_receiver_bind$", tips: [ref, 0, 0, ref]}
      - {target: "^roc_receiver_read$", tips: [ref, ref]}
      - {target: "^roc_receiver_close$", tips: [ref]}
      - {target: "^roc_sender_open$", tips: [ref, ref]}
      - {target: "^roc_sender_bind$", tips: [ref, ref]}
      - {target: "^roc_sender_connect$", tips: [ref, 0, 0, ref]}
      - {target: "^roc_sender_write$", tips: [ref, ref]}
      - {target: "^roc_sender_close$", tips: [ref]}
      - {target: "^roc_", tips: [sref, sref, sref, sref, sref]}
  ConstRules:
    defines: expand
  Rules:
    const:
      - {action: ignore, from: ROC_ADDRESS_SIZE}
      - {action: accept, from: "^ROC_"}
      - {action: replace, from: "^ROC_", to: }
      - {transform: lower}
      - {load: snakecase}
      - {transform: export}
    type:
      - {action: accept, from: "^roc_"}
      - {action: replace, from: "^roc_", to: }
      - {transform: lower}
      - {load: snakecase}
      - {transform: export}
      - {action: replace, from: "^Frame$", to: frame}
    function:
      - {action: accept, from: "^roc_"}
      - {action: replace, from: "^roc_", to: }
      - {transform: lower}
      - {load: snakecase}
      - {transform: unexport}
    private:
      - {transform: unexport}

```
generator and parser sections seem intuitive enough, but you may seem puzzled, why there are "Includes" and "SourcesPaths" botb in generator and parser that seeemingly contain the same headers. The thing is that in 'generator' section you specify includes that will be straight up copied to the `#cgo` section of your go file, like this:
```
/*
#cgo LDFLAGS: -lroc
#include "roc/receiver.h"
#include "roc/sender.h"
#include "roc/log.h"
#include <stdlib.h>
#include "cgo_helpers.h" // added automatically
*/
import "C"

// go code 
```

while in parser section you specify where to find definitions of all types found while parsing your c code, this list is just a "reference" for c-for-go compiler and is not copied anywhere, just used during compilation.

The trickiest part of `roc.yml` was the "translator" rules. First subsection PtrTips is here to give c-for-go tips on datatypes accepted in function signatures. The first hit in regex will be accepted.
- {target: "^roc_sender_close$", tips: [ref]}
 In this example you specify a regex that will match functions with `target`: this regex will match only one funciton - roc_sender_close, and gives tips for its only argument - it's a pointer (`ref`). There are 4 possible kinds of PtrTip: ref, sref, arr and 0 (for scalar types). See more [here](https://github.com/xlab/c-for-go/wiki/Translator-config-section#ptrtips).
 ConstRules are for specifying how to treat defines and enums, it's pretty straightforward and we didn't have much troubles with it.
 The trickiest part of generator section is its Rules subsection. Here you define rules on what's exported and unexported in your newly created bindings, but it also can be done right with the right combination of `      - {transform: unexport}` in each subsection.
What you get as a result of generation is code like this:

|--------------------------------------------|--------------------------------------------------------------------------------------|
| c code                                     | golang code                                                                          |
|--------------------------------------------|--------------------------------------------------------------------------------------|
| `typedef struct roc_address {`             |                                                                                      |
| `#ifndef ROC_DOXYGEN`                      |                                                                                      |
| `/* Private data. Do not use directly. */` |                                                                                      |
| `union {`                                  |                                                                                      |
| `unsigned long align;`                     |                                                                                      |
| `char payload[ROC_ADDRESS_SIZE];`          |                                                                                      |
| `} private_data;`                          |                                                                                      |
| `#endif /* ROC_DOXYGEN */`                 |                                                                                      |
| `} roc_address;`                           |                                                                                      |
|                                            | `// Address as declared in roc/address.h:59`                                         |
|                                            | `type Address struct {`                                                              |
|                                            | `ref94efe9d7    *C.roc_address`                                                      |
|                                            | `allocs94efe9d7 interface{}`                                                         |
|                                            | `}`                                                                                  |
|                                            |                                                                                      |
|--------------------------------------------|--------------------------------------------------------------------------------------|
| `int roc_address_init(     `               |                                                                                      |
| `roc_address* address,     `               |                                                                                      |
| `roc_family family,        `               |                                                                                      |
| `const char* ip, int port);`               |                                                                                      |
|                                            | `// addressInit function as declared in roc/address.h:86`                            |
|                                            | `func addressInit(address *Address, family Family, ip string, port int32) int32 {  ` |
|                                            | `	caddress, _ := address.PassRef()                                              ` |
|                                            | `	cfamily, _ := (C.roc_family)(family), cgoAllocsUnknown                        ` |
|                                            | `	ip = safeString(ip)                                                           ` |
|                                            | `	cip, _ := unpackPCharString(ip)                                               ` |
|                                            | `	cport, _ := (C.int)(port), cgoAllocsUnknown`                                    |
|                                            | `	__ret := C.roc_address_init(caddress, cfamily, cip, cport)                    ` |
|                                            | `	runtime.KeepAlive(ip)                                                         ` |
|                                            | `	__v := (int32)(__ret)                                                         ` |
|                                            | `	return __v                                                                    ` |
|                                            | `}`                                                                                  |
|--------------------------------------------|--------------------------------------------------------------------------------------|

as you can see in go code, there are a lot of allocations, which is not ideal for a real-time audio streaming library. 

