
This technique should be your go-to when you see a problem that involves searching for two (or more) elements in an array that meet a certain criteria.

Useful for problems where we deal with sorted lists and need to find a set of elements that fulfill certain constraints

An implementation using this technique runs in `0(n)` time complexity

### Example: find sum of pairs in array
Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.

Since the array is sorted, we can just move inwards from the ends of the array, comparing the sums to the `target` until we find the match. We could alternatively brute-force this by using a nested for-loop and iterating over each element twice, but since the array is sorted, we can leverage this fact to be smart about which comparisons we make
![](/assets/images/2021-10-10-11-45-01.png)

```js
function findSumPairs(arr, target) {
    let frontIndex = 0
    let backIndex = arr.length - 1

    let tempSum
    while (frontIndex !== backIndex) {
        tempSum = arr[frontIndex] + arr[backIndex]
        if (tempSum < target) {
            frontIndex++
        } else if (tempSum > target) {
            backIndex--
        } else {
            return true
        }
    }
    return false
}
```
