# Thrift

## Types

### Primitives
* bool
* byte
* i16, i32, i64
* float, double
* binary - array of bytes
* string - UTF-8

### Containers
* list<T>
* set<T>
* map<T1, T2>

containers can be nested: map<T1, map<T2, T2>>

### Initialization
```
bool field1 = true
string field2 = "value"

list<i32> list_field = [1, 2, 3, 4]
map<string, list<i32>> map_field = {
  "this" : [1, 2, 3],
  "that" : [4, 5, 6],
}
```

### Enums
```
enum Thing {
  THIS = 5,
  THAT = 3,
}
```


## Collections
collections of the form `id: [required|optional] type field_name `
ids are integers < 2^63 - 1
do not reuse ids

### Structs
```
struct Thing {
  1: i32 field1 = 5000,
  10: required string field2,
  100: optional list<string> field3,
}
```

Fields can be separated with a comma, semicolon, or just newline

### Exceptions
Same as struct

### Unions
Used when only one field will be set

### Default Values
* bool - false
* byte or ints - 0
* float or double - 0.0
* string - empty string
* container - empty container

avoid using default values for optional fields
do not change default values

### Modifiers
* required - must be set or it'll throw, always serialized, causes UB if null?
* optional - only serialized if set
* none - always serialized, even if unset, causes UB if null?


## Services

Services are sets of methods
* Each method:
    * has a unique name
    * may specify a list of inputs
        * Method are numbered like structs
        * It's better to have a request object instead of many inputs
        * New input fields can be added to a method, but it is better an input struct and add members, since the server can't distinguish missing arguments from default values
    * may return an output
    * may define exceptions that it throws
        * If the exceptions is a thrift exception, all members will be serialized with the exception
        * If not, a `TApplicationException` will be thrown, and no members will be serialized other than `message`
* `oneway` means the client doesn't wait for a response

```
struct Foo {
  1: i64 field1 = 101,
  2: string field2,
}

exception BarException {
  1: string message,
  2: i64 errorCode,
}

struct GetFoosRequest {
  1: list<i64> ids,
}

// service Bar extends Controller.ControllerService{
service Bar extends fb303.FacebookService {
  void ping() throws (1: BarException e),

  Foo getFoos(
    1: GetFoosRequest request
  )
  throws (1: BarException e),

  oneway void fireAndForget(),
}
```


## Constants

`const` keyword, can be used to specify defaults within thrift file
only usable on base types


## Typedef

```
typedef map<string, string> StringMap
```


## Include

```
include "common/example/if/Foo.thrift"

struct Baz {
  1: Foo.Bar field
}
```

Can't include multiple files with the same name (Foo.thrift)


## Namespaces

Specify namespaces for each language

```
namespace 2 foo.bar
namespace hack foo.bar
namespace java com.company.foo.bar
namespace py bar.foo
```
