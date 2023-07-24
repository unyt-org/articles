<!--
	{
		description: "An introduction",
		preview: "res/uix_banner.png",
		date: ~2023-04-26~,
		tag: "Developer",
		author: "unyt.org",
		authorRef: https://unyt.org
	};
-->


# Building the Future: Distributed Computing with DATEX

With DATEX, we want to establish a new, standardized, easy-to-use way to create the next generation of distributed applications.

## 1. Everything is DATEX

The approach for the Supranet architecture could be described as a combination of 
*keep it incredibly simple* and *make it as complicated as necessary* (unfortunatly not very good for nice acronyms)

On a macro scale, everything is *kept incredibly simple*. Nearly every aspect
in the supranet can be broken down to DATEX binary blocks that are sent between endpoints, read, modified, stored, added to the blockchain or executed.

DATEX itself on the other hand has evolved from a simple JSON-like format to an extensive protocol.
In our opinion, it is still more straightforward than most of the protocol stacks used today - but it is no HTTP.

## 2. DATEX as a network-native language

*In the following paragraphs, we're mostly talking about the paradigms of the DATEX Script language, which has some subtle differences to the binary format to which it gets compiled.*

## Remote code execution

In the DATEX language paradigm, the supranet is a core part of the system.
As consequence, calling a function on a remote endpoint or keeping data synchronized between multiple endpoints is a simple as calling a function locally or accessing data on a single machine with different threads.

```datex
result = complexFormula(); // calling the 'complexFormula' on the current endpoint
result = @+my-remote-computer :: complexFormula(); // calling the 'complexFormula' on a remote endpoint
```

Variables can also just be passed from scope of the local endpoint to a remote scope:
```datex
const PI   = 3.1415;
ref radius = 10;
result     = @+my-remote-computer :: calculateCircleArea(PI, radius);
```

As you can see, DATEX completely abstracts away any functional differences between local and remote execution.

## Compatibility with the web

Although we want to establish a completely new global network with the Supranet, we do not want to neglect the importance of the conventional web.

This is why DATEX also comes with first-class support for URLs and associated protocols.
The contents of both `file` and `http(s)` URLs can be directly accessed and modified via DATEX:
```datex
const url = https://unyt.org/example.txt;
const example = get url; // get the content as type text/plain
```
```datex
const json = get https://unyt.org/example.json; // get the content as a parsed JSON value
``````

```datex
const image = get file:///home/ubuntu/example.png; // get the content as a type image/png
file:///home/ubuntu/example2.png = image/png `FEFA123...` // store the image value in the file
```

DATEX maps all [mime types](https://www.iana.org/assignments/media-types/media-types.xhtml) to primitive types, e.g. `text/plain`, `text/html` or `image/png`. This allows for a direct mapping between web resources and DATEX values, as well as files on a file system.

## 3. Permission Management

**With great power comes great responsibility**. This is especially true for DATEX remote code execution.
Although all execution happens in a closed sandbox environment, it is still
important that read/write and execution access for any value can be restricted to specific endpoints.

The DATEX permission system is a core part of the language and allows both white- and blacklists and can be fine-tuned for any usecase. 

## 4. Pointer synchronization




## 5. Garbage collection


