# Modules

- Avoid the need for ``$GOPATH``

- Group packages versioned/released together

- Support semantic versioning and backwards compatability

- Provide in project dependency management

- Offer strong dependency security and availability

- Continue support of vendoring

- Work transparently across go ecosystems.

Go modules with proxying offers the value of vendoring without requiring the project to vendor all 3rd party code in your repo.

## Advantages

- Protects against risks like
    - Flaky repos
    - Package that disappears
    - Conflicting dependency versions
    - Surreptitious changes to public packages

> A little copying is better than a little dependancy - Go proverb

## Import compatibility rule

- If an old package and new package have the same import path , the new package must be backwards compatible with the old package

- An incompatible updated package should use a new URL(version)

```go
package hello

import(
    "github.com/x"
    x2 "github.com/x/v2"

)
```

- We can import both versions if necessary


## Control files

- The ``go.mod`` has the module name along with direct dependency requirements

```go
module hello

require github.com/x v1.1
go 1.13
```

- The ``go.sum`` file has checksums for all transitive dependencies

```go
github.com/x v1.1 h1:foo
github.com/y v0.2 h1:ffofdoo
```

## Environment variables

- Defaults for these

```shell
GOPROXY= https://proxy.golang.org,direct
GOSUMDB=sum.golang.org
```

- Private reops

```shell
GOPRIVATE
GONOSUMB
```
- We need to set access like SSH to private github repo to privat github repos

[Proxy inner working](../images/proxy-inner-working.png)

- We can use ``replace`` to point our local version [Go-work](https://go.dev/doc/tutorial/workspaces)

## Common commands

- Start a project with

```shell
$ go mod init <module-name> ## Create go.mod file
$ go build ## building the

```