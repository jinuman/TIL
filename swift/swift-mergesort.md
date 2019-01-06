# Merge Sort
> 분할 정복 방법을 통해 리스트를 정렬

- 시간 복잡도 : O(NlgN)
- 공간 복잡도 : O(N)

```Swift
import Foundation

func mergeSort(_ arr: inout [Int]) -> [Int] {
    mergeSort(arr: &arr, start: 0, end: arr.count - 1)
    return arr
}

func mergeSort(arr: inout [Int], start: Int, end: Int) {
    if start == end {
        return
    }
    let mid: Int = (start + end) / 2
    mergeSort(arr: &arr, start: start, end: mid)
    mergeSort(arr: &arr, start: mid + 1, end: end)
    merge(arr: &arr, start: start, mid: mid, end: end)
}

func merge(arr: inout [Int], start: Int, mid: Int, end: Int) {
    var tempArr: [Int] = arr
    
    var i: Int = start
    var j: Int = mid + 1
    var k: Int = start
    while i <= mid && j <= end {
        if tempArr[i] <= tempArr[j] {
            arr[k] = tempArr[i]
            i += 1
        } else {
            arr[k] = tempArr[j]
            j += 1
        }
        k += 1
    }
    // 위에서 j > end 이면 남은 것 저장
    while i <= mid {
        arr[k] = tempArr[i]
        i += 1
        k += 1
    }
    // 위에서 i > mid 이면 남은 것 저장
    while j <= end {
        arr[k] = tempArr[j]
        j += 1
        k += 1
    }
}

var array: [Int] = [3, 5, 2, 9, 10, 14, 4, 8, 1, 15]
print(mergeSort(&array))    // [1, 2, 3, 4, 5, 8, 9, 10, 14, 15]

```