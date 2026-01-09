# surl - Sortable URL package

A fork of Go's standard library `net/url` package with custom query parameter ordering support.

## Overview

This package is based on Go's `net/url` package and provides the same URL parsing and query escaping functionality, with one key enhancement: **custom query parameter ordering**.

## Installation

```bash
go get github.com/nukilabs/surl
```

## Key Feature: Custom Query Parameter Ordering

The standard `url.Values.Encode()` method sorts query parameters lexicographically. This package adds support for custom ordering via the special `OrderKey` constant.

### Usage

```go
package main

import "github.com/nukilabs/surl"

func main() {
    v := url.Values{
        "zebra":  {"1"},
        "apple":  {"2"},
        "banana": {"3"},
        // Define custom order using OrderKey
        url.OrderKey: {"banana", "zebra", "apple"},
    }
    
    println(v.Encode())
    // Output: banana=3&zebra=1&apple=2
    
    // Without OrderKey, output would be: apple=2&banana=3&zebra=1
}
```

### How It Works

- **With `OrderKey`**: Parameters are ordered according to the slice provided in `url.OrderKey`
  - Keys defined in the order slice appear first, in the specified order
  - Keys not defined in the order slice appear last, sorted lexicographically
  - The `OrderKey` itself is not included in the encoded output
  
- **Without `OrderKey`**: Parameters are sorted lexicographically (standard behavior)

### Use Cases

This feature is particularly useful when:
- Interfacing with APIs that require specific parameter ordering for signature generation
- Working with legacy systems that expect parameters in a particular order
- Debugging or maintaining consistent output formatting
- Generating canonical URLs for caching or comparison

## API Compatibility

This package maintains full API compatibility with Go's standard `net/url` package. You can use it as a drop-in replacement:

```go
import url "github.com/nukilabs/surl"
```

All standard functionality remains unchanged, including:
- URL parsing and construction
- Query parameter encoding/decoding
- Path escaping
- RFC 3986 compliance

## License

BSD-style license (same as Go's standard library)

## Credits

Based on Go's `net/url` package:
- Copyright 2009 The Go Authors
- Follows RFC 3986 and RFC 6874 (IPv6 zone literals)
