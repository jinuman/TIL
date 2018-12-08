Merge Sort
--

- 시간 복잡도 : O(NlgN)
- 공간 복잡도 : O(N)

```Swift
import Foundation

func mergeSort(_ arr: inout [Int]) -> [Int] {
    mergeSort(arr: &arr, front: 0, rear: arr.count - 1)
    return arr
}
func mergeSort(arr: inout [Int], front: Int, rear: Int) {
    if front == rear {
        return
    }
    let mid: Int = (front + rear) / 2
    mergeSort(arr: &arr, front: front, rear: mid)
    mergeSort(arr: &arr, front: mid + 1, rear: rear)
    merge(arr: &arr, front: front, mid: mid, rear: rear)
}
func merge(arr: inout [Int], front: Int, mid: Int, rear: Int) {
    var tempArr: [Int] = arr

    var i: Int = front
    var j: Int = mid + 1
    var k: Int = front
    while i <= mid && j <= rear {
        if tempArr[i] <= tempArr[j] {
            arr[k] = tempArr[i]
            i += 1
        } else {
            arr[k] = tempArr[j]
            j += 1
        }
        k += 1
    }
    // 위에서 j > rear 이면 남은 것 저장
    while i <= mid {
        arr[k] = tempArr[i]
        i += 1
        k += 1
    }
    // 위에서 i > mid 이면 남은 것 저장
    while j <= rear {
        arr[k] = tempArr[j]
        j += 1
        k += 1
    }
}

var array: [Int] = [3, 5, 2, 9, 10, 14, 4, 8, 1, 15]
print(array)
print(mergeSort(&array))
```