# Quick Sort
> 분할 정복 방법을 통해 리스트를 정렬

### 시간 복잡도

- 평균적으로 O(NlgN)
- pivot이 가장 작거나 가장 큰 최악의 경우 O(N^2)

```Swift
import Foundation

func quickSort(_ arr: inout [Int]) -> [Int] {
    quickSort(arr: &arr, start: 0, end: arr.count - 1)
    return arr
}

func quickSort(arr: inout [Int], start: Int, end: Int) {
	// partition에서 오른쪽 part의 첫번째 값의 인덱스 이용
    let rightPartFirst = partition(arr: &arr, start: start, end: end)
    if start < rightPartFirst - 1 {
        quickSort(arr: &arr, start: start, end: rightPartFirst - 1)
    }
    if rightPartFirst < end {
        quickSort(arr: &arr, start: rightPartFirst, end: end)
    }
}

func partition(arr: inout [Int], start: Int, end: Int) -> Int {
    let pivot: Int = arr[(start + end) / 2]
    var i: Int = start
    var j: Int = end
    while i <= j {
        while arr[i] < pivot {
            i += 1
        }
        while arr[j] > pivot {
            j -= 1
        }
        if i <= j {
            arr.swapAt(i, j)
            i += 1
            j -= 1
        }
    }
    return i
}

var array: [Int] = [3, 5, 2, 9, 10, 14, 0, 8, 1, 15]
print(quickSort(&array))    // [0, 1, 2, 3, 5, 8, 9, 10, 14, 15]
```