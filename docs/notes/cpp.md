# C++: containers and standard algorithms

Prefer a standard container and an algorithm that expresses the operation clearly.
This example uses C++17.

```cpp title="sum.cpp" linenums="1"
#include <iostream>
#include <numeric>
#include <vector>

int main() {
    const std::vector<int> values{1, 2, 3, 4, 5};
    const int total = std::accumulate(values.begin(), values.end(), 0);
    std::cout << total << '\n';  // 15
}
```

Compile and run:

```sh
c++ -std=c++17 -Wall -Wextra -pedantic sum.cpp -o sum
./sum
```

`std::accumulate` visits each element once: $O(n)$ time and $O(1)$ auxiliary space.
The initial value determines the accumulator type; use `0LL` when a wider integer
sum is needed.
