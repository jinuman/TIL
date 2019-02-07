# Timer
> Timer 클래스는 일정한 시간 간격이 지나면 지정된 메세지를 특정 객체로 전달하는 기능을 제공  
> Objective-C : NSTimer

- 매 초마다 여러 개의 작업이 동시에 수행되어야 할 때 유용하다. 
ex. 0.01초마다 전체 재생시간에 대한 현재 시간을 UILabel에 출력해주고, UISlider에도 똑같이 반영해야 하는 경우

- main 쓰레드에 작은 딜레이를 주는 코드는 그냥 GCD(Grand Central Dispatch)를 쓰자..훨씬 편하다.

### 특징

- run loop에서 작동한다.
- 타이머를 생성할 때, 반복 여부를 지정한다.
  - 비 반복 타이머 : 한 번 실행된 다음 자동으로 무효화 된다.
  - 반복 타이머 : 동일한 런 루프에서 특정 TimeInterval 간격으로 실행된다. 반복되는 타이머 기능을 정지하려면 invalidate() 메서드를 호출해 무효화한다.

### Run loop

- Imagine run loop like a _ferris wheel_
- You can schedule a task on a run loop simular to people can enter a passenger car and get moved around by the wheel.
- Run loop keeps "looping" and executing tasks. When there are no tasks to execute, the run loop waits or quits.

#### If main thread is busy..like app is interacting with UI..  
add below line
```Swift
let timer = Timer(timeInterval: 1.0, target: self, selector: #selector(fire), userInfo: nil, repeats: true)

RunLoop.current.add(timer, forMode: .commonModes)  // add this line
```

### 주요 프로퍼티

1. var isValid: Bool - 타이머가 현재 유효한지 아닌지 여부
2. var fireDate: Date - 다음에 타이머가 실행될 시각
3. var timeInterval: TimeInterval - 타이머의 실행 시간 간격(초 단위)


#### 현재 시각을 매 초마다 출력, 10초 후 종료하는 예제
##### 1. 일반 버전
```Swift
extension DateFormatter {
    static var timerFormatter: DateFormatter = {
        let df = DateFormatter()
        df.locale = Locale(identifier: "ko_KR")
        df.dateFormat = "hh:mm:ss a"
        return df
    }()
}

class Alarm {
    private var cnt: Int = 0
    @objc func printCurrentTimeBySecond(_ timer: Timer) {
        if cnt == 10 {
            timer.invalidate()
            print("10초 후 끝 >.<")
            return
        }
        print("현재 시각: \(DateFormatter.timerFormatter.string(from: timer.fireDate))")
        cnt += 1
    }
}

let timer = Timer.scheduledTimer(timeInterval: 1.0,
                                 target: Alarm(),
                                 selector: #selector(Alarm.printCurrentTimeBySecond(_:)),
                                 userInfo: nil,
                                 repeats: true)
```

##### 2. closure 버전
```Swift
var cnt: Int = 0
Timer.scheduledTimer(withTimeInterval: 1.0, repeats: true) {
    if cnt == 10 {
        $0.invalidate()
        print("Done.")
        return
    }
    print("현재 시각: \(DateFormatter.timerFormatter.string(from: $0.fireDate))")
    cnt += 1
}
```

### 참고

## TimeInterval

#### TimeInterval이란? Double형의 초단위  

예를 들어 time: 1.59609977324263 와 같은 형태      
여기에서 millisecond는 59
```Swift
typealias TimeInterval = Double
```

- 초 단위 시간을 minute : second : millisecond 형태로 변환하는 코드
```Swift
func updateTimeLabelText(time: TimeInterval) {
    let minute: Int = Int(time / 60)
    let second: Int = Int(time.truncatingRemainder(dividingBy: 60))
    let milisecond: Int = Int(time.truncatingRemainder(dividingBy: 1) * 100)

    let timeText: String = String(format: "%02ld:%02ld:%02ld", minute, second, milisecond)
    self.timeLabel.text = timeText
}
```


