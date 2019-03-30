# UrlSession
> HTTP/HTTPS 를 통해 콘텐츠를 업로드하고 다운로드하기 위한, Apple 이 제공하는 네트워킹 API

## 특징
- iOS 7 이상부터 사용 가능하다.
- Authentication 을 지원하는 많은 delegate 메소드들을 가지고 있다.
- 앱이 정지되거나 종료되어도 백그라운드에서 다운로드를 계속 할 수 있는 기능이 있다.

## What is session?
- __방문자가 서버에 접속해 있는 상태를 하나의 단위로 보고 세션이라고 말한다.__
- 예를 들어, 웹 브라우저를 사용 중인 경우 `탭 당 하나의 세션`을 만들 수 있다.
- 각 세션 내에서 애플리케이션은 작업을 추가하고, 각 작업은 특정 URL에 대한 요청을 나타낸다.

## 유형

- 3가지 유형의 세션을 제공한다.
- URLSession configuration 프로퍼티로 결정할 수 있다.
  - ex. URLSession(configuration: .default)

#### 1) Default
- 추가적으로 property 설정하지 않았다면, __`shared` 와 유사하게 동작__한다.
- 디스크에 저장하는 방식이다.

#### 2) Ephemeral (임시)
- 기본 세션과 유사하지만, 디스크에 어떤 데이터도 저장하지 않고, 메모리에 올려 세션과 연결한다.
- 따라서 애플리케이션이 세션을 만료시키면 세션과 관련한 데이터가 사라진다.

#### 3) Background
- 백그라운드 세션은 별도의 프로세스가 모든 데이터 전송을 처리한다는 점을 제외하고는 기본 세션과 유사하다.
- 앱이 suspend, not running 일 때도 background 에서 콘텐츠 업로드 및 다운로드가 가능하다.

### URLSessionTask
> 세션 작업 하나를 나타내는 추상 클래스이다. 3가지 작업 유형이 있다.

#### 1) URLSessionDataTask

- HTTP의 각종 메서드를 이용해 서버로부터 응답 데이터를 받아서 Data 객체를 가져오는 작업을 수행한다.

#### 2) URLsessionUploadTask

- 애플리케이션에서 웹 서버로 `Data 객체` 또는 파일 데이터를 업로드하는 작업을 수행한다.
- 주로 HTTP의 POST 혹은 PUT 메서드를 이용한다.

#### 3) URLSessionDownloadTask

- <u>__서버로부터 데이터를 다운로드 받아서 파일의 형태로 저장하는 작업을 수행한다.__</u>
- 애플리케이션의 상태가 대기 중이거나 실행 중이 아니라면 background 상태에서도 다운로드가 가능합니다.


#### _tip_ : default request method is GET.  

- POST, PUT, DELETE 을 해야한다면
  - URL -> URLRequest
  - data task를 URL 대신 URLRequest로 만들면 된다.

```Swift
func dataTask(with request: URLRequest, 
              completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void) -> URLSessionDataTask
```

## For Download

### URLSessionDownloadDelegate
> handles task-level events specific to download tasks.

- 만약 다운로드 상태를 모니터링, 업데이트하고 싶으면 `URLSessionDownloadDelegate`를 구현해야 한다.

## 참고

### URLComponents
- Swift에서 제공하는 url 파싱할 때 편리한 struct

### defer
- `defer block` is called at the end of each loop(scope).

