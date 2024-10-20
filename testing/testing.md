## Go test features

- Go has standard tools and conventions for testing

- Test files end with ``_test.go`` and have ``TestXXX`` functions
(they can be in package directory on in aseerate directory)

- We can run tests with go test

```
go test ./...

ok xyz/test 56.841s
ok xyz/pkg/acedb (cached)
```

Tests are not run if the source wasnt changed since the last test

## Common things to test for

- extreme values

- input validation

- race conditions

- error conditions

- boundary conditions

- pre and post conditions

- randomzied data

- configuration & deployment

- interfaces to other software

**Unit testing** - Unit testing is self contained and doesn't need any external
dependencies to be included (database ,microservice), just need to test whether the parts that are in our control work correctly and handled correctly.

**Integration testing** - Typically runs in CI/CD and communicates with MS

## Test functions

Test functions have the same signature using ``testing.T``

```go
func TestCrypto(t*testing.T){
    uuid := "foo"
    key :="bar"
    key2 := "foo"

    ct , err := secrets.MakeAppKey(key,uuid)
    if err != nil {
        t.Errorf("make failed: %s",err)
    }
}
```
- Errors are reported through param t and fail the test

## Table driven tests

 Tests just in a range of values

## Testing culture

We should assume code doesn't work unless

- We have tests (unit,integration etc)

- They work correctly

- You run them , They pass

- The basic code hygeine **start clean , stay clean**

- In general , **developers test to show that things are done and working** ,according to understanding of the problem and solution.

- Most difficulties in SDE development are **failures of imagination**

## Program correctness

- It compiles [and passes static analysis]

- It has no bugs that can be found just running the program

- It works for some handpicked data

- It works for reasonable input

- It works with test data chosen to be difficult

- It works for all input that follows specification

- It works on all input

## Distinct types of erros

- errors in undetstanding the problem requirements.

- errors in understanding the programming language.

- errors in understanding the underlying algorithm

-  errors where we know better but slipped

## Developer testing is necessary

Should aim for ``75-85%`` coverage
- Unit tests

- Integration tests

- Post deployment sanity checks


![Layers of Testing](../images/layers-of-testing.png)

