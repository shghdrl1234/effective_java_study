item 06. 불필요한 객체 생성을 피하라
=============

### 1. String s = new String("hi"); 보다는 String s = "hi";
> 새로운 인스턴스를 매번 만들지 말고 하나의 인스턴스를 재사용하자.

### 2. Boolean(String) 보다는 Boolean.valueOr(String)
> 정적 팩터리 메서드를 사용해 불필요한 객체 생성을 피할 수 있다.   
> 생성자는 호출할 때마다 새로운 객체를 만들지만 팩터리 메서드는 그렇지 않다.   
> > 팩터리 메서드란?
> > -  객체를 생성하기 위해 인터페이스를 정의하지만, 어떤 클래스의 인스턴스를 생성할지에 대한 결정은 서브클래스가 내리도록 하는 패턴


### 3. 생성 비용이 비싼 객체는 캐싱하자
#### 생성 비용이 비싼 객체란?
- 크기가 아주 큰 Array
- Database Connection
- I/O 작업을 필요로 하는 Object
- Pattern


```java
static boolean isRomanNumeral(String s){
	return s.matches("^(?=.)M*(C[MD]|d?C{0,3})" + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
}
```
String.matehes는 성능이 중요한 상황에서 반복적으로 사용하기에 적합하지x

```java
public class RomanNumerals {
	private static final Pattern ROMAN = Pattern.compile("^(?=.)M*(C[MD]|d?C{0,3})" + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
	
static boolean isRomanNumeral(String s){
		return ROMAN.matcher(s).matches();
	}
}
```

> 클래스 초기화 과정에서 인스턴스를 직접 생성해 캐싱해두고, 나중에 메서드가 호출할때 이 인스턴스를 재사용   
> 빈번하게 호출되는 상황에서 성능 개선 뿐만 아니라 코드도 명확해짐

### 4. 오토박싱을 유의하자
오토박싱이란 기본 타입과 박싱된 기본 타입을 섞어 쓸때 자동으로 상호 변환해주는 기술
> 박싱된 기본 타입보다는 기본타입을 사용하고, 의도치 않은 오토박싱을 유의하자
