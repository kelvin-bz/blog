---
title: The beauty of "else if" in different programming languages

categories: [tech]
date: 2024-07-03 00:00:00
tags: [javascript]
image: "/assets/images/the-beauty-of-else-if-in-different-programming-languages.jpg
"
---

Throughout my career, I have had the opportunity to work with multiple programming languages, each with its unique syntax and nuances. From JavaScript and Java to Python, Solidity, Ruby, Bash, and Go, I've navigated various coding environments. However, there are moments when my brain freezes, especially when writing else if statement.

### JavaScript (ES6+)

```javascript
let num = 10;

if (num > 10) {
    console.log("Greater than 10");
} else if (num === 10) {
    console.log("Equal to 10");
} else {
    console.log("Less than 10");
}
```

### Java

```java
int num = 10;

if (num > 10) {
    System.out.println("Greater than 10");
} else if (num == 10) {
    System.out.println("Equal to 10");
} else {
    System.out.println("Less than 10");
}
```

### C

```c
#include <stdio.h>

int main() {
    int num = 10;

    if (num > 10) {
        printf("Greater than 10\n");
    } else if (num == 10) {
        printf("Equal to 10\n");
    } else {
        printf("Less than 10\n");
    }

    return 0;
}
```


### Solidity

```js
pragma solidity ^0.8.0;

contract ConditionalExample {
    function checkNumber(uint num) public pure returns (string memory) {
        if (num > 10) {
            return "Greater than 10";
        } else if (num == 10) {
            return "Equal to 10";
        } else {
            return "Less than 10";
        }
    }
}
```


### Python

```python
num = 10

if num > 10:
    print("Greater than 10")
elif num == 10:
    print("Equal to 10")
else:
    print("Less than 10")
```

### Ruby

```ruby
num = 10

if num > 10
  puts "Greater than 10"
elsif num == 10
  puts "Equal to 10"
else
  puts "Less than 10"
end
```

### Bash

```bash
num=10

if [ "$num" -gt 10 ]; then
    echo "Greater than 10"
elif [ "$num" -eq 10 ]; then
    echo "Equal to 10"
else
    echo "Less than 10"
fi
```

### Go

```go
package main

import "fmt"

func main() {
    num := 10

    if num > 10 {
        fmt.Println("Greater than 10")
    } else if num == 10 {
        fmt.Println("Equal to 10")
    } else {
        fmt.Println("Less than 10")
    }
}
```

