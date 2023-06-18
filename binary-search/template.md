# Template

```java
// Minimize x such that condition(x) is true
int binarySearchMinimize(int[] arr) {
    int lo = -1, hi = arr.length;
    while (lo + 1 < hi) {
        int mid = lo + (hi - lo) / 2;
        if (condition(arr, mid)) {
            hi = mid;
        } else {
            lo = mid;
        }
    }
    return hi;
}

// Maximize x such that condition(x) is true
int binarySearchMaximize(int[] arr) {
    int lo = -1, hi = arr.length;
    while (lo + 1 < hi) {
        int mid = lo + (hi - lo) / 2;
        if (condition(arr, mid)) {
            lo = mid;
        } else {
            hi = mid;
        }
    }
    return lo;
}

boolean condition(int[] arr, int idx) {
    // Some condition on arr[idx]
    // Return true or false
    return true;
}

```
