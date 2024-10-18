## Static Analysis

- Static means the program isn't running (compile time)

- Static analysis transfers the effort from people to tools
    - Mental effort while coding
    - Code review effort

- Static analysis improves code hygiene
     - correctness
     - efficiency
     - readbility
     - maintainability

- Static analysis allows us to find many issues beyond compiler

- If our code compiles & passes static analysis , we can have a lot of confidence it even before ``running tests``

## gofmt and goimports

- ``gofmt`` will put your code in standard form (spacing,indentation)

- ``goimports`` will do that and also update import lists

- Having a canonical codeformat is an important part of SDE

- The standard practice is to rune one or other on every save in your IDE

- They can also run as ``pre-commit`` hook in your local

## golint

- ``golint`` will check for non-format style issues

- exported names should have comments for godoc

- names shoud'nt have ``under_scores`` or be in ``ALLCAPS``

- ``panic`` should'nt be used for normal error handling

- the error flow should be indented , the happy path not

- variable declaration should'nt have redundant type info

## govet

- ``go vet`` will find some issues the compiler wont

- suspicious ``printf`` format strings

- accidentally copying a ``mutex`` type

- possibly invalid integer shifts

- possibly invalid atomic assignments

- possibly invalid struct tag

- unreachable code

no static analysis tools can find all possible errors

## Other tools

- ``goconst`` finds literals that should be declared with ``const``

- ``gosec`` looks for possible security issues

- ``ineffassign`` finds assignments that are ineffcective

- ``deadcode``, ``unused`` and ``varcheck`` find unused/dead code

- ``unconvert`` finds redundant type conversions

## ineffassign

- The first assignment is ineffective because it is overwritten without being read

```go
prices , err := r.prices(region ,...)
regularPrices , err = r.regularPrices(region,...) // probably add later

if err != nil {
    return nil ,nil
}

// ineffectual assignment to err
```

## govet 

The format string is mismatched

```go
fmt.Sprintf("%d",foo)
```


- Go officially handles all of this
[Go extension](https://code.visualstudio.com/docs/languages/go)

## Further reading

[Further reading](https://go.dev/wiki/CodeReviewComments)