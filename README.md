# Coral Preamble

I love [Cobra](https://github.com/spf13/cobra) and I love [Viper](https://github.com/spf13/viper).
They are great projects, incredibly useful and outstandingly important for the
Go community. But sometimes, just sometimes, don't you wish you could use Cobra
without the entire dependency chain Viper drags in?

This is what `Coral` is: a Cobra fork without any of the Viper dependencies and
features. This will hopefully be a soft fork with only the minimal changes
required to decouple Viper from Cobra. The aim is to follow upstream Cobra
development as closely as possible.

Coral has only four direct dependencies:

- github.com/spf13/pflag
- github.com/cpuguy83/go-md2man/v2
- github.com/inconshreveable/mousetrap
- gopkg.in/yaml.v2

# Migrating existing projects to Coral

It's easy to use Coral as a drop-in replacement for Cobra in existing projects,
just let `gofmt` do the heavy lifting for you:

```sh
gofmt -w -r '"github.com/spf13/cobra" -> "github.com/muesli/coral"' .
gofmt -w -r '"github.com/spf13/cobra/doc" -> "github.com/muesli/coral/doc"' .
gofmt -w -r 'cobra -> coral' .
go mod tidy
```

As a result, you should typically see a lot of indirect dependencies removed
from the project's `go.mod` and `go.sum` files.

# Cobra 1.4.0

Recent Cobra versions, v1.4.0 and onward, removed the dependency on Viper as well.
You might switch back to Cobra using `gofmt`:

```sh
gofmt -w -r '"github.com/muesli/coral" -> "github.com/spf13/cobra"' .
gofmt -w -r '"github.com/muesli/coral/doc" -> "github.com/spf13/cobra/doc"' .
gofmt -w -r 'coral -> cobra' .

go get -u github.com/spf13/cobra
go mod tidy
```

If you use [mango][] too, also do this:

```sh
gofmt -w -r '"github.com/muesli/mango-coral" -> "github.com/muesli/mango-cobra"' .
gofmt -w -r 'mcoral -> mcobra' .

go get -u github.com/muesli/mango-cobra
go mod tidy
```

More info: https://github.com/spf13/cobra/releases/tag/v1.4.0

[mango]: https://github.com/muesli/mango

# Upstream Cobra README

![cobra logo](https://cloud.githubusercontent.com/assets/173412/10886352/ad566232-814f-11e5-9cd0-aa101788c117.png)

Cobra is both a library for creating powerful modern CLI applications as well as a program to generate applications and command files.

Cobra is used in many Go projects such as [Kubernetes](http://kubernetes.io/),
[Hugo](https://gohugo.io), and [Github CLI](https://github.com/cli/cli) to
name a few. [This list](./projects_using_cobra.md) contains a more extensive list of projects using Cobra.

[![](https://img.shields.io/github/workflow/status/spf13/cobra/Test?longCache=tru&label=Test&logo=github%20actions&logoColor=fff)](https://github.com/spf13/cobra/actions?query=workflow%3ATest)
[![GoDoc](https://godoc.org/github.com/spf13/cobra?status.svg)](https://godoc.org/github.com/spf13/cobra)
[![Go Report Card](https://goreportcard.com/badge/github.com/spf13/cobra)](https://goreportcard.com/report/github.com/spf13/cobra)
[![Slack](https://img.shields.io/badge/Slack-cobra-brightgreen)](https://gophers.slack.com/archives/CD3LP1199)

# Overview

Cobra is a library providing a simple interface to create powerful modern CLI
interfaces similar to git & go tools.

Cobra is also an application that will generate your application scaffolding to rapidly
develop a Cobra-based application.

Cobra provides:
* Easy subcommand-based CLIs: `app server`, `app fetch`, etc.
* Fully POSIX-compliant flags (including short & long versions)
* Nested subcommands
* Global, local and cascading flags
* Easy generation of applications & commands with `cobra init` & `cobra add cmdname`
* Intelligent suggestions (`app srver`... did you mean `app server`?)
* Automatic help generation for commands and flags
* Automatic help flag recognition of `-h`, `--help`, etc.
* Automatically generated shell autocomplete for your application (bash, zsh, fish, powershell)
* Automatically generated man pages for your application
* Command aliases so you can change things without breaking them
* The flexibility to define your own help, usage, etc.
* Optional seamless integration with [viper](http://github.com/spf13/viper) for 12-factor apps

# Concepts

Cobra is built on a structure of commands, arguments & flags.

**Commands** represent actions, **Args** are things and **Flags** are modifiers for those actions.

The best applications read like sentences when used, and as a result, users
intuitively know how to interact with them.

The pattern to follow is
`APPNAME VERB NOUN --ADJECTIVE.`
    or
`APPNAME COMMAND ARG --FLAG`

A few good real world examples may better illustrate this point.

In the following example, 'server' is a command, and 'port' is a flag:

    hugo server --port=1313

In this command we are telling Git to clone the url bare.

    git clone URL --bare

## Commands

Command is the central point of the application. Each interaction that
the application supports will be contained in a Command. A command can
have children commands and optionally run an action.

In the example above, 'server' is the command.

[More about cobra.Command](https://pkg.go.dev/github.com/spf13/cobra#Command)

## Flags

A flag is a way to modify the behavior of a command. Cobra supports
fully POSIX-compliant flags as well as the Go [flag package](https://golang.org/pkg/flag/).
A Cobra command can define flags that persist through to children commands
and flags that are only available to that command.

In the example above, 'port' is the flag.

Flag functionality is provided by the [pflag
library](https://github.com/spf13/pflag), a fork of the flag standard library
which maintains the same interface while adding POSIX compliance.

# Installing
Using Cobra is easy. First, use `go get` to install the latest version
of the library. This command will install the `cobra` generator executable
along with the library and its dependencies:

    go get -u github.com/spf13/cobra

Next, include Cobra in your application:

```go
import "github.com/spf13/cobra"
```

# Usage
Cobra provides its own program that will create your application and add any
commands you want. It's the easiest way to incorporate Cobra into your application.

For complete details on using the Cobra generator, please read [The Cobra Generator README](https://github.com/spf13/cobra/blob/master/cobra/README.md)

For complete details on using the Cobra library, please read the [The Cobra User Guide](user_guide.md).

# License

Cobra is released under the Apache 2.0 license. See [LICENSE.txt](https://github.com/spf13/cobra/blob/master/LICENSE.txt)
