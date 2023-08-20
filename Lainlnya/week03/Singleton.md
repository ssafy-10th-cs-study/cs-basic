# 싱글턴패턴을 만드는 7가지 방법

## 1. 단순한 메서드 호출

싱글톤 패턴 생성 여부를 확인하고 싱글톤이 없으면 새로 만들고 있다면 만들어진 인스턴스를 반환

```java
public class Singleton{
	private static Singleton instance;

	private Singleton(){}

	public static Singleton getInstance() {
		if (instance == null) instance = new Singleton();
	}
	return instance;
}
```

자바는 멀티스레드 언어이기 때문에 여러 실행 환경을 고려해야 함

**멀티스레드 환경에서 스레드를 여러 개 생성하게 되면 인스턴스가 여러 개 생길 가능성**이 있음

## 2. synchronized

인스턴스를 반환하기 전까지 격리 공간에 놓기 위해 **synchronized 키워드로 잠금 가능**

최초로 접근한 스레드가 해당 메서드 호출 시에 다른 스레드가 접근하지 못하도록 lock을 걸어줌 ⇒ getInstance() 를 호출할 때마다 lock이 걸려 성능 저하가 됨

```java
public class Singleton{
	private static Singleton instance;

	private Singleton(){}

	public static synchronized Singleton getInstance() {
		if (instance == null) instance = new Singleton();
	}
	return instance;
}
```

## 3. 정적 멤버

런타임이 아니라 최초에 JVM이 클래스 로딩 시 모든 클래스를 로드할 때 미리 인스턴스를 생성하는데 이를 이용한 방법

클래스 로딩과 동시에 싱글톤 인스턴스를 만듦 ⇒ 싱글톤을 만들지 않을 경우 불필요한 자원낭비라는 단점

```java
public class Singleton{
	// 정적 멤버 확인
	private final static Singleton instance = new Singleton();

	private Singleton(){}

	public static Singleton getInstance() {
		return instance;
}
```

## 4. 정적 블록

```java
public class Singleton{
	private final static Singleton INSTANCE = null;

	// 정적 블록 확인
	static {
		INSTANCE = new Singleton();
	}

	private Singleton(){}

	public static Singleton getInstance() {
		return INSTANCE;
}
```

## 5. 정적 멤버와 Lazy Holder(중첩 클래스) \*\* 추천

singleInstanceHolder라는 내부클래스를 하나 더 만듦으로써 Singleton클래스가 최초에 로딩되더라도 함께 초기화되지 않고, getInstance()가 호출될 때 singleInstanceHolder 클래스가 로딩되어 인스턴스 생성

```java
class Singleton {
	private static class singleInstanceHolder {
		private static final Singleton INSTANCE = new Singleton();
	}

	public static Singleton getInstance() {
		return singleInstanceHolder.INSTANCE;
	}
}
```

## 6. 이중 확인 잠금(DCL)

인스턴스 생성 여부를 싱글톤 패턴 잠금 전에 한 번, 객체를 생성하기 전에 한 번 2번 체크하면 인스턴스가 존재하지 않을 때만 잠금을 걸 수 있기 때문에 앞서 생겼던 문제점을 해결 할 수 있음

```java
public class Singleton{
	private volatile static Singleton instance;

	private Singleton(){}

	public static Singleton getInstance() {
		if (instance == null) { // 1번 체크
			// 위에서 synchronized 하는 것이 아닌 인스턴스가 null일 때 생성하며 lock
			synchronized (Singleton.class) {
				if (instance == null) { // 2번 체크
					instance = new Singleton();
				}
			}
	}
	return instance;
}
```

### volatile

Java에서는 스레드 2개가 열리면 변수를 메인 메모리(RAM)으로부터 가져오는 것이 아니라 캐시메모리에서 각각의 캐시메모리를 기반으로 가져 옴

volatile 키워드를 쓰면 메인 메모리로부터 변수를 기반으로 가져와 변수 공유가 가능

## 7. enum \*\* 추천

enum은 기본적으로 스레드 세이프(thread safe)한 점이 보장되기 때문에 이를 통해 생성할 수 있음

```java
public enum SingletonEnum {
	INSTANCE;
	public void oortCloud(){};
}
```
