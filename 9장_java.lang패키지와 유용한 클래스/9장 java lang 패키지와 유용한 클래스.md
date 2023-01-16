# 9장 java.lang 패키지와 유용한 클래스

### 1장 Object 클래스

| Object 클래스의 메서드 | 설명 |
| --- | --- |
| protected Object clone() |  |
| public boolean equals(Object obj) |  |
|  |  |

### equals(Object obj)

### hashcode()

### System.identityHashCode()

### **toString()**

### clone()

1. **Clonable 인터페이스**를 구현해야함
2. 참조형 인스턴스 변수일 시 **얕은 복사**가 이루어짐
3. 접근제어자가 protected에서 public으로

**공변 반환타입 (JDK 1.5, covariant return type)**

오버라이딩할 때 조상 메서드의 반환타입을 자손 클래스의 타입으로 변경을 허용하는 것이다.

```java
public Point clone(){
	Object obj=null;
	try{
		obj=super.clone();
	}catch(CloneNotSupportedException e){}
	return (Point) obj;
}
```

### getClass()

> Class 객체를 반환하는 메서드이다.
> 

**Class 객체**

1. 클래스의 모든 정보를 담고 있다
2. 클래스 당 1개만 존재한다
3. 클래스 파일이 클래스 로더에 의해서 메모리에 올라갈 때 자동으로 생성된다
4. 클래스 로더는 실행 시에 필요한 클래스를 동적으로 메모리에 로드하는 역할을 한다
    1. 먼저 기존에 생성된 클래스 객체가 메모리에 있는 지 확인
    2. 있으면 객체의 참조를 반환
    3. 없으면 클래스 패스에 지정된 경로를 따라 클래스 파일을 찾는다
        1. 못 찾으면 ClassNotFoundException이 발생
        2. 찾으면 해당 클래스 파일을 읽어서 Class 객체로 변환한다

**Class 객체를 얻는 방법**

```java
Class cobj=new Card().getClass(); // 이미 생성된 객체로부터 얻는 방법
Class cobj=Card.class; // 클래스 리터럴로부터 얻는 방법
Class cobj=Class.forName("Card"); //클래스 이름으로부터 얻는 방법
```

3번째 방법은 특정 클래스 파일, 예를 들어 DB 드라이버를 메모리에 올릴 때 주로 사용한다.

Class 객체를 이용하면 클래스에 정의된 멤버의 이름이나 개수 등, 클래스에 대한 모든 정보를 얻을 수 있기에 Class 객체를 통해 객체를 생성하고, 메서드를 호출하는 등의 보다 동적인 코드를 작성할 수 있겠다.

### 2장 String 클래스

1. **변경 불가능한(immutable) 클래스**
    1. String 인스턴스가 갖고 있는 문자열은 read only, not write
    2. 문자열끼리의 결합 추출등의 연산이 필요하면 String 클래스가 아니라 StringBuffer를 사용
2. **문자열의 비교**
    1. 문자열을 만드는 방법
        1. 문자열 리터럴을 지정하는 방법
        2. String 클래스의 생성자를 사용해서 만드는 방법
3. **문자열 리터럴**
    1. “AAA”라고 만들면 다른 “AAA”도 같은 객체를 참조
    2. 문자열 리터럴은 컴파일 시에 클래스 파일에 저장됨
    3. 클래스 파일에는 소스파일에 포함된 모든 리터럴의 목록이 있음
    4. 클래스 로더에 의해 클래스 파일이 로드될 때 리터럴 목록들이 JVM 내의 상수 저장소에 저장된다.
4. **빈 문자열**
    1. 빈 문자열은 가능하다
    2. 길이가 0인 배열
5. **String 클래스의 생성자와 메서드**
    
    
    | char charAt(int index) |  |
    | --- | --- |
    | int compareTo(String str) |  |
    | String concat(String str) |  |
    | boolean contains(CharSequence s) |  |
    | boolean endsWith(String suffix) |  |
    | boolean equals(Object obj) |  |
    | boolean equalsIgnoreCase(Object obj) |  |
    | int indexOf(int ch) |  |
    | int indexOf(int ch, int pos) |  |
    | int indexOf(String str) |  |
    | String intern() |  |
    | int lastIndexOf(int ch) |  |
    | int lastIndexOf(String str) |  |
    | int length() |  |
    | String replace(char old, char nw) |  |
    | String replace(CharSequence old, CharSequence nw) |  |
    | String replaceAll(String regex, String replacement) |  |
    | String replaceFirst(String regex, String replacement) |  |
    | String[] split(String regex) |  |
    | String[] split(String regex, int limit) |  |
    | boolean startsWith(String prefix) |  |
    | String substring(int begin) |  |
    | String substring(int begin, int end) |  |
    | String toLowerCase() |  |
    | String toString() |  |
    | String toUpperCase() |  |
    | String trim() |  |
    | static String valueOf() |  |
6. **join()과 StringJoiner**
    1. `join()` 은 여러 문자열 사이에 구분자를 넣어서 결합한다
    2. `spilit()` 과 반대의 작업을 한다
    3. `StringJoiner` 예제
        
        ```java
        StringJoiner sj=new StringJoiner(",","[","]");
        String[] strArr={"aaa","bbb","ccc"};
        
        for(String s:strArr){
        	sj.add(s.toUpperCase());
        } 
        
        System.out.println(sj.toString());
        
        // 출력 [AAA,BBB,CCC]
        
        //join()과 StringJoiner는 jdk 1.8부터 추가되었다
        ```
        
7. **유니코드의 보충문자**
    1. 메서드 중 매개변수 타입이 int인 것도 있고 char 타입인 것도 있다.
    2. 이는 유니코드가 원래 16bit였는데, 이것으로 부족하여 20bit까지 확장하게 되었다.
    3. 따라서 하나의 문자를 char타입으로 다루지 못하는 문제가 발생하게 되어서 int 타입으로 다루는 것이다.
    4. 이 확장으로 인해 추가된 문자를 보충문자라고 하고 이를 지원하는지에 대한 여부는 메서드의 매개변수가 int인지 char인지 확인하면 된다.
8. **문자 인코딩 변환**
    1. `getBytes(String charsetName)` 를 사용하면 문자열의 문자 인코딩을 다른 인코딩으로 변경할 수 있다.
    2. 자바는 UTF-16 사용, 한글 윈도우는 CP949 사용
    3. UTF-8은 한글 하나에 3바이트
    4. CP949는 한글 하나에 2바이트
9. **String.format()**
    1. printf()와 동일
10. **기본형 값을 String으로 변환**
    1. `+`로 하는 방법
    2. `valueOf()`으로 하는 방법
11. **String을 기본형 값으로 전환**
    1. 래퍼 클래스의 `parse타입()` 을 사용하기
    2. 래퍼 클래스의 `valueOf()` 를 사용하기
        1. 사실 이것도 결국 `parseInt()` 를 내부 호출하는 것이다.
    3. 문자열을 기본형으로 전환할 때의 오류를 고려하기
        1. 좌우 공백 → `trim()`
        2. 형변환 오류

### 3장 StringBuffer 클래스와 StringBuilder 클래스

### 4장 Math 클래스

### 5장 래퍼 (wrapper) 클래스

# 유용한 클래스

### 1장 java.util.Objects 클래스

### 2장 java.util.Random 클래스

### 3장 정규표현식 java.util.regex 패키지

### 4장 java.util.Scanner 클래스

### 5장 java.util.StringTokenizer 클래스

### 6장 java.math.BigInteger 클래스

### 7장 java.math.BigDecimal 클래스