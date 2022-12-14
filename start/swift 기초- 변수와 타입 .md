# swift 기초- 변수와 타입

1-1) **변수(variable)**: 데이터(자료)를 담을 수 있는 공간(바구니)

```swift
var a = 3
```

1-2) **상수(constants)**: **변하지 않는** 데이터(자료)를 담을 수 있는 공간(바구니)

```swift
let a = 3
```

위에서 a 는 변수의 이름(식별자)이며 한 영역에서 유일하게 하나의 이름만 사용해야 합니다.

**변수의 이름은 대부분 소문자로 시작하는것이 대부분입니다.** 숫자는 처음들어가는게 아닌이상 어디든지 들어가도 됩니다. Camel 문법을 주로 사용하고 언더바(_)는 잘 사용하지 않습니다.

변수를 문자열 안에서 사용하기위해서는 문자열 보간법을 사용합니다.

```swift
print(“저의 이름은 \(name)입니다. 나이는 \(age)살 이고, \(address)에 살고 있습니다.”)
```

### 2)Type의 종류

2-1)**정수(Integer)형 데이터 타입 : Int**
스위프트는 **모든 타입**의 첫글자에 **대문자**를 사용합니다(변수명과 대비)

2-2)**실수(Floating-point Number)형 데이터 타입 : Float, Double**

**Float**: 실수/부동소수 타입(4바이트 단위이며 6자리까지 표현)

**Double**: 실수타입(8바이트 단위이며 15자리까지 표현 

2-3)**문자형 데이터 타입(2가지): Character,String (쌍따옴표”” 동반해야함)**

**Character: 하나의 문자(한 글자 단위, 공백문 표현가능)**

**String: 문자열 단위**

2-4) 참/거짓을 다루는 데이터형

Bool

### **3)변수와 타입**

타입주석: 타입을 명시함

```swift
var name: String = "이지훈"
```

타입추론: 타입을 명시하지 않아도 컴파일러가 타입을 추론(찾아보니 자바랑 코틀린도 있더라)

```swift
var name = "이지훈"
```

타입 안정성:  소수 + 정수를 더 할 수 없다. "안녕" + 5 를 더할 수 없다.

⇒ 떄문에 타입(형) 변환을 이용해야 한다.

### 4) ****타입 변환(Conversion)****

기존에 메모리에 저장된 값을 다른 형식으로 변환하여, 새로운 값을 저장하여 또다른 메모리 공간에 저장하는 방식(데이터 유실이나 데이터 변환실패 가능성도 염두해야함)

```swift
let str2 = "123"
let number1 = Int(str2)

print(number1)

>>>optional(123)
```

5)****타입 애일리어스(Type Alias)****
원래의 이름 대신에 가명을 지정 하고 싶을 때 사용합니다.(수학에서의 치환과 비슷한 개념)

```swift
// 왼쪽에 치환된 별칭이 위치
typealias Work = String

// Work타입이 의미하는 것은 String과 완전히 동일
let name: Work = "홍길동"
//즉 String을 Work로 치환하여 사용하는것
```

이 애일리어스는 문자가 길어질때 유용하게 사용할 수 있습니다.

```swift
typealias Something = (Int) -> String

func someFunction(completionHandler: (Int) -> String) {
}

func someFunction2(completionHandler: Something) {
}
```

1-1) **변수(variable)**: 데이터(자료)를 담을 수 있는 공간(바구니)

```swift
var a = 3
```

1-2) **상수(constants)**: **변하지 않는** 데이터(자료)를 담을 수 있는 공간(바구니)

```swift
let a = 3
```

위에서 a 는 변수의 이름(식별자)이며 한 영역에서 유일하게 하나의 이름만 사용해야 합니다.

**변수의 이름은 대부분 소문자로 시작하는것이 대부분입니다.** 숫자는 처음들어가는게 아닌이상 어디든지 들어가도 됩니다. Camel 문법을 주로 사용하고 언더바(_)는 잘 사용하지 않습니다.

변수를 문자열 안에서 사용하기위해서는 문자열 보간법을 사용합니다.

```swift
print(“저의 이름은 \(name)입니다. 나이는 \(age)살 이고, \(address)에 살고 있습니다.”)
```

### 2)Type의 종류

2-1)**정수(Integer)형 데이터 타입 : Int**
스위프트는 **모든 타입**의 첫글자에 **대문자**를 사용합니다(변수명과 대비)

2-2)**실수(Floating-point Number)형 데이터 타입 : Float, Double**

**Float**: 실수/부동소수 타입(4바이트 단위이며 6자리까지 표현)

**Double**: 실수타입(8바이트 단위이며 15자리까지 표현 

2-3)**문자형 데이터 타입(2가지): Character,String (쌍따옴표”” 동반해야함)**

**Character: 하나의 문자(한 글자 단위, 공백문 표현가능)**

**String: 문자열 단위**

2-4) 참/거짓을 다루는 데이터형

Bool

### **3)변수와 타입**

타입주석: 타입을 명시함

```swift
var name: String = "이지훈"
```

타입추론: 타입을 명시하지 않아도 컴파일러가 타입을 추론(찾아보니 자바랑 코틀린도 있더라)

```swift
var name = "이지훈"
```

타입 안정성:  소수 + 정수를 더 할 수 없다. "안녕" + 5 를 더할 수 없다.

⇒ 떄문에 타입(형) 변환을 이용해야 한다.

### 4) ****타입 변환(Conversion)****

기존에 메모리에 저장된 값을 다른 형식으로 변환하여, 새로운 값을 저장하여 또다른 메모리 공간에 저장하는 방식(데이터 유실이나 데이터 변환실패 가능성도 염두해야함)

```swift
let str2 = "123"
let number1 = Int(str2)

print(number1)

>>>optional(123)
```

5)****타입 애일리어스(Type Alias)****
원래의 이름 대신에 가명을 지정 하고 싶을 때 사용합니다.(수학에서의 치환과 비슷한 개념)

```swift
// 왼쪽에 치환된 별칭이 위치
typealias Work = String

// Work타입이 의미하는 것은 String과 완전히 동일
let name: Work = "홍길동"
//즉 String을 Work로 치환하여 사용하는것
```

이 애일리어스는 문자가 길어질때 유용하게 사용할 수 있습니다.

```swift
typealias Something = (Int) -> String

func someFunction(completionHandler: (Int) -> String) {
}

func someFunction2(completionHandler: Something) {
}
```