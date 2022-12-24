# URLSession 기본정리

# URLSession API 호출 개념 정리

출처: [link][https://github.com/timojaask/URLSession-Cheat-Sheet/blob/master/README.md](https://github.com/timojaask/URLSession-Cheat-Sheet/blob/master/README.md)

## Simple GET request

```swift
// URL(일반적인거), URLRequest(완성되어있는거) 형태로 호출이 가능 
URLSession.shared.dataTask(with: url) { (data, response, error) in
    guard let statusCode = (response as? HTTPURLResponse)?.statusCode else {
        // request failed
        return
    }
    // handle status code
}.resume()
```

## Query parameters

```swift
var urlComponents = URLComponents(string: "https://www.google.com")!
urlComponents.queryItems = [
    URLQueryItem(name: "foo1", value: "bar"),
    URLQueryItem(name: "foo2", value: "baz")
]
let url = urlComponents.url // https://www.google.com?foo1=bar&foo2=baz
```

### Extension example

```swift
`extension URL {
    init?(baseUrl: String, queryItems: [String: String]) {
        guard var urlComponents = URLComponents(string: baseUrl) else { return nil }
        urlComponents.queryItems = queryItems.map { URLQueryItem(name: $0.key, value: $0.value) }
        guard let finalUrlString = urlComponents.url?.absoluteString else { return nil }
        self.init(string: finalUrlString)
    }
}`
```

## Method, body and headers

```swift
var request = URLRequest(url: url)
request.httpMethod = "POST" // "GET", "PUT", "DELETE"
request.httpBody = data // json 요청 형태 혹은 form 데이터 형태
request.addValue("application/json", forHTTPHeaderField: "Content-Type")
request.addValue("application/json", forHTTPHeaderField: "accept") // accept: application/json
```

### Extension example

```swift
extension URLRequest {
    static func request(url: URL, headers: [String: String] = [:], method: String = "GET", data: Data? = nil) -> URLRequest {
        var request = URLRequest(url: url)
        request.httpMethod = method
        request.httpBody = data
        headers.forEach { header in
            request.addValue(header.value, forHTTPHeaderField: header.key)
        }
        return request
    }
}
```

## JSON

### From Dictionary to JSON object - 중요 : 보통 post 방식으로 json을 요청 데이터로 넣을때 사용 httpBody

```swift
guard let data = try? JSONSerialization.data(withJSONObject: dictionary, options: [.prettyPrinted]) else {
    // JSON serialization failed
    return
}
```

출처: [link][https://github.com/timojaask/URLSession-Cheat-Sheet/blob/master/README.md](https://github.com/alexpaul/iOS-UIKit/blob/main/URLSession-Cheatsheet.md)

# URLSession Cheatsheet 2 번째

## Vocabulary

- JSON
- endpoint
- RESTFul API
- URLSession
- URLSession.shared
- (중요)JSONDecoder, JSONEncoder
- URL - 제일 심플한 형태
- URLRequest - URL에 플러스로 추가 세팅이 되어 있는 것
- URLResponse
- HTTPURLResponse
- Status Code - HTTP 상태 코드 200번 대가 OK이다
- Data
- Codable = Encodable & Decodable
- Encodable -> Class 나 Struct 를 JSON으로 만드는 것
- Decodable -> JSON을 Class 나 Struct로 만드는 것
- HTTP methods: GET, POST, PUT, DELETE, PATCH, UPDATE
- Asynchronous - Closure, Rx Observable, Combine Publisher, Async await
- Result - Enum 클래스 : Result<성공데이터, 커스텀에러>
- @escaping closures
- capture list e.g [weak self], [unowned self]
- weak vs unowned

## Using a closure to capture the `Result` of the asynchronous network request

`Result` type is an `enum` type that has two arguments, a `success` state and an `failure` state.

```swift
func fetchWebData(completion: @escaping (Result<[ModelObject], Error>) -> ()) {
 // netowrking code here
}`
```

## Perform a GET request using `URLSession`

`URLSession.shared` is a singleton instance on `URLSession` with basic networking configurations.

```swift
let dataTask = URLSession.shared.dataTask(with: url) { (data, response, error) in
  // networking code here
}
dataTask.resume()`
```

## Check that the `HTTPURLResponse` status code is within the valid range of `200...299` indicating a successful response

```swift
guard let httpResponse = response as? HTTPURLResponse,
      (200...299).contains(httpResponse.statusCode) else {
  print("bad status code")
  return
}
```

## Converting `JSON` data to Swift objects

```swift
do {
   // 데이터 파싱
  let topLevelModel = try JSONDecoder().decode(TopLevelModel.self, from: jsonData)
  let modelObjects = topLevelModel.modelObjects
  completion(.success(modelObjects))
} catch {
  // decoding error : JSON -> 우리가 사용하는 데이터 모델 class,struct
  completion(.failure(error))
}
```

## Using the `Codable` protocol to parse JSON to Swift model(s)

```swift
struct ApiResponse: Decodable & Endocodable {
   let data: Todo?
   let message: String?
}

struct Todo: Decodable & Endocodable {
    let id: Int?
    let title: String?
    let isDone: Boolean?
  
  // CodingKeys allows us to rename properties
  enum CodingKeys: String, CodingKey {
    case id = "id"case title = "title"case isDone = "is_done"
  }
}`

`struct CovidCountriesWrapper: Codable {
  let countries: [CountrySummary]
  
  // CodingKeys allows us to rename properties
  enum CodingKeys: String, CodingKey {
    case countries = "Countries"
  }
}

struct CountrySummary: Codable {
  let country: String
  let totalConfirmed: Int
  let totalRecovered: Int
  
  enum CodingKeys: String, CodingKey {
    case country = "Country"case totalConfirmed = "TotalConfirmed"case totalRecovered = "TotalRecovered"
  }
}
```

## The `CodingKeys` built-in `enum` type allows us to change JSON property names to our own custom names

In this example we change `Countries` to a more Swift naming conventional name `countries`.

```swift
struct CovidCountriesWrapper: Codable {
  let countries: [CountrySummary]
  
  // CodingKeys allows us to rename properties
  enum CodingKeys: String, CodingKey {
    case countries = "Countries"
  }
}
```

## Completed API Client to fetch web data

```swift
func fetchWebData(completion: @escaping (Result<[ModelObject], Error>) -> ()) {
    // 1. - endpoint URL string
    let endpointURLString = "https://........"
    
    // 2. - convert the string to an URL
    guard let url = URL(string: endpointURLString) else {
      print("bad url")
      return
    }
    
    // URL vs URLRequest
    
    // 3. - make the request using URLSession
    // .shared is an singleton instance on URLSession comes with basic configuration needed for most requests
    let dataTask = URLSession.shared.dataTask(with: url) { (data, response, error) in
      if let error = error {
        return completion(.failure(error))
      }
      
      // first we have to type cast URLResponse to HTTPURLRepsonse to get access to the status code
      // we verify the that status code is in the 200 range which signals all went well with the GET request
      guard let httpResponse = response as? HTTPURLResponse,
            (200...299).contains(httpResponse.statusCode) else {
        print("bad status code")
        return
      }
      
      if let jsonData = data {
        // convert data to our swift model
        do {
          let topLevelModel = try JSONDecoder().decode(TopLevelModel.self, from: jsonData)
          let modelObjects = topLevelModel.modelObjects
          completion(.success(modelObjects))
        } catch {
          // decoding error
          completion(.failure(error))
        }
      }
    }
    dataTask.resume()
  }
```

## dictionary -> URLEncoded 처리

```swift
extension URLRequest {
  
  private func percentEscapeString(_ string: String) -> String {
    var characterSet = CharacterSet.alphanumerics
    characterSet.insert(charactersIn: "-._* ")
    
    return string
      .addingPercentEncoding(withAllowedCharacters: characterSet)!
      .replacingOccurrences(of: " ", with: "+")
      .replacingOccurrences(of: " ", with: "+", options: [], range: nil)
  }
  
  mutating func encodeParameters(parameters: [String : String]) {
    httpMethod = "POST"
    
    let parameterArray = parameters.map { (arg) -> String in
      let (key, value) = arg
      return "\(key)=\(self.percentEscapeString(value))"
    }
    
    httpBody = parameterArray.joined(separator: "&").data(using: String.Encoding.utf8)
  }
}
```