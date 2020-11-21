# RFC 001: Iron Packages

This RFC introduces **packages** as an additional layer of abstraction that sits below Iron modules.

## Motivation

Iron should make it easy to design reusable software components and distribute them in a decentralized manner. Iron will use Git as its underlying version control system, but the specific details of how Iron modules are organized and versioned is not yet clear.

As a motivating example, consider a `mail` module that defines functionality for implementing clients and servers that communicate using IMAP and SMTP protocols. While such a module could be considered for inclusion in the Iron standard library, we will assume for now that this is an independent implementation. This is an important assumption, because I hope for the standard library to be driven largely by independent innovations and contributions.

Anyway, consider the (drastically simplified) structure of this hypothetical `mail` module.

```sh
mail/
├── imap/
│   └── module.fe
│
├── smtp/
│   └── module.fe
│
└── module.fe
```

If we drill into `mail/module.fe`, we will see that it makes the `imap` and `smtp` modules publicly available to consumers. It might define some additional types that could be shared across both submodules, but those are irrelevant for now.

```iron
public import (
	imap,
	smtp
)
```

That in mind, if you are writing a mail server in Iron, you will do something like:

```iron
import mail::imap

// do something with the imap module
```

**A critical question emerges: how is this dependency on this third-party `mail` module expressed?** We know that this module is defined in some Git repository somewhere, possibly hosted on GitHub or some other website. We also know that this module's Git repository likely has tags corresponding to module versions, as well as a primary branch (e.g. `main`) where the latest, and potentially unstable, code resides.

## Analysis of Prior Art

### Go: Modules

TODO

### Swift: Swift Package Manager

TODO

### Rust: Cargo

TODO

## Proposal

### Defining a package

The _package_ is the highest level of abstraction in Iron software systems. They are created using Forge:

```sh
# Create a new Iron package
forge new package <package-name>
```

An Iron package contains one or many _modules_ that are versioned and distributed together via Git. At the "package root," which is also the root of the Git repository, we find a new file: **`package.fe`**.

If our package depends on the third-party `mail` module, that can be expressed in this `package.fe` file like so:

```iron
import package::Package

public let package = Package{
	dependencies: [
		.package("https://github.com/ironlang/mail", from: "1.0.0")
	]
}
```

The `package` module, which contains the `Package` structure type, will be part of the standard library (or, standard package). With this information, Forge will be able to retrieve the `mail` package dependency that meets the version requirement described in our `package.fe` file.

### Dealing with package naming conflicts

TODO
