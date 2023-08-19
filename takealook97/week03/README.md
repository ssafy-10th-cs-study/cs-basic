디자인 패턴
=

- 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 ‘규약’ 형태로 만들어 놓은 것을 의미한다.
- 디자인 패턴은 라이브러리나 프레임워크를 만드는데 기초적인 원리가 되며 지금도 많은 라이브러리, 프레임워크가 어떠한 디자인패턴을 기반으로 만들어지고 있다.
- 이를 기반으로 문제를 해결하는데 있어서 영감을 받을 수 도 있고 빠른 의사소통을 통해 문제를 해결해 나갈 수 있다.

### 종류

- 생성 패턴
    - 클래스로부터 객체를 만들 때 어떻게 만들 것인지
    - 객체 생성 방법이 들어간 디자인 패턴
    - 싱글톤, 팩토리, 추상팩토리, 빌더, 프로토타입 등
- 구조 패턴
    - 객체를 기반으로 어떤 구조를 만들어서 처리할 것인지
    - 객체, 클래스 등으로 큰 구조를 만들 때 유연하고 효율적으로 만드는 방법이 들어간 디자인 패턴
    - 프록시, 어댑터, 브리지, 복합체, 데코레이터, 퍼사드, 플라이웨이트 등
- 행동 패턴
    - 객체들을 어떻게 순회할 것인지, 객체를 기반으로 알고리즘이 구현이 되는데 이를 어떻게 처리할 것인지
    - 객체나 클래스 간의 알고리즘, 책임 할당에 관한 디자인 패턴
    - 이터레이터, 옵저버, 전략, 책임연쇄, 커맨드, 중재자, 메멘토, 상태, 템플릿메서드, 비지터 등

### 라이브러리와 프레임워크의 차이

- 라이브러리
    - 공통으로 사용될 수 있는 특정한 기능들을 모듈화한 것
    - 폴더명, 파일명 등에 대한 규칙이 없고 프레임워크에 비해 자유롭다.
    - 자유도가 높지만 제공되는 것들이 많지는 않다.
- 프레임워크
    - 공통으로 사용될 수 있는 특정한 기능들을 모듈화한 것
    - 폴더명, 파일명 등에 대한 규칙이 있으며 라이브러리에 비해 좀 더 엄격하다.
    - 자유도가 낮지만 제공되는 것들이 많다.

---

# 싱글톤 패턴

- 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
- 홀더 방식이 가장 많이 사용된다.
    - 실제로 사용할 때 까지 실행 시점을 미룰 수 있다.
    - 정적 초기화로 생성 되므로 쓰레드 세이프하다.
- 장점
    - 하나의 인스턴스를 기반으로 해당 인스턴스를 다른 모듈들이 공유하여 사용하기 때문에 인스턴스 생성 시 발생하는 비용이 줄어든다.
    - 그렇기 때문에 인스턴스 생성에 많은 비용이 발생하는 I/O 바운드 작업에 많이 사용한다.
- 단점
    - 의존성이 높아진다.
        - 이는 DI(의존성 주입)을 통해 모듈 간 결합을 느슨하게 함으로써 해결할 수 있다.
        - 그러나 클래스 수가 늘어나 복잡성이 증가될 수 있고, 약간의 런타임 페널티가 생기기도 한다.
    - TDD(Test Driven Development)를 할 때 걸림돌이 된다.
        - TDD는 단위테스트로 주로 이루어지는데 이 때 서로 독립적이어야하며 테스트를 어떤 순서로든 실행할 수 있어야 한다.
        - 그러나 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이기에 각 테스트마다 ‘독립적인’ 인스턴스를 만들기가 어렵다.

# 싱글톤 패턴을 구현하는 7가지 방법

## 1. 단순한 메서드 호출

- 싱글톤 패턴 생성 여부를 확인하고 싱글톤이 없으면 새로 만들고 있다면 만들어진 인스턴스를 반환
- 그러나 이는 메서드의 원자성이 결여되어 있다. 멀티스레드 환경에서는 싱글톤 인스턴스를 2개 이상 만들 수 있다.(쓰레드 세이프 하지 않다)

    ```java
    public class Singleton {
    
    	private static Singleton instance;
    	
    	private Singleton() {
    
    	}
    	
    	public static Singleton getInstance() {
    		if (instance == null) {
    			instance = new Singleton(); //객체 생성 과정
    		}
    		return instance; 
    	}
    }
    
    ```

- 자바는 멀티 쓰레드 언어이다. → 여러 쓰레드로 발생할 수 있는 상황을 고려해야 한다.
- 만약 2개의 쓰레드에서 각자 `getInstance()` 메서드를 호출했을 때 두 개의 인스턴스가 만들어진다면 이는 싱글톤 패턴이 아니다.

## 2. `synchronized`

- 인스턴스를 반환하기 전까지 격리 공간에 놓기 위해 `synchronized` 키워드로 잠금을 할 수 있다.
    - 1번에 대한 해결방법
- 최초로 접근한 쓰레드가 해당 메서드 호출 시에 다른 쓰레드가 접근하지 못하도록 잠금(lock)을 걸어준다.
- 이 때 `getInstance()` 메서드를 호출할 때 마다 lock이 걸려 성능 저하

```java
public class Singleton {

	private static Singleton instance;
	
	private Singleton() {
	}
	
	public static synchronized Singleton getInstance() {
		if (instance == null) {
			instance = new Singleton(); //객체 생성 과정
		}
		return instance; 
	}
}

```

## 3. 정적 멤버

- 정적(static) 멤버나 블록은 런타임이 아니라 최초에 JVM이 클래스 로딩 시 모든 클래스들을 로드할 때 미리 인스턴스를 생성하는 데 이를 이용한 방법
- 클래스 로딩과 동시에 싱글톤 인스턴스를 만든다. 그렇기 때문에 모듈들이 싱글톤 인스턴스를 요청할 때 그냥 만들어진 인스턴스를 반환하면 된다.
- 그러나 이는 불필요한 자원낭비라는 문제점이 있다. 싱글톤 인스턴스가 필요없는 경우에도 무조건 싱글톤 클래스를 호출해 인스턴스를 만들어야 하기 때문이다.
- 정적 멤버 활용

    ```java
    public class Singleton {
    
    	private final static Singleton instance = new Singleton();
    
    	private Singleton() {
    	}
    
    	public static Singleton getInstance() { 
    	    return instance;
    	} 
    }
    
    ```


## 4. 정적 블록

- 정적 블록 활용 (정적 블록에는 주로 클래스 변수를 초기화시키는 코드를 둔다.)

    ```java
    public class Singleton {
    
    	private static Singleton instance = null;
    
    	static {
    		instance = new Singleton();
    	}
    
    	private Singleton() {
    	}
    
    	public static Singleton getInstance() {
    		return instance;
    	}
    }
    
    ```


## 5. 정적 멤버와 LazyHolder(중첩 클래스)

- 기존의 방식에서는 싱글톤 인스턴스를 호출하지 않아도 무조건 초기에 자원을 할당해야하는 문제점이 있었다.
    - 이를 해결하기 위해 모듈이 필요로 할 때에만 정적 멤버로 선언을 하는 것 (자원낭비 문제 해결)
- `SingleInstanceHolder`라는 내부 클래스를 하나 더 만듦으로써, Singleton클래스가 최초에 로딩되더라도 함께 초기화가 되지 않고, `getInstance()`가 호출될 때 `SingleInstanceHolder` 클래스가 로딩되어 인스턴스를 생성하게 된다.

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

- 이중 확인 잠금(Double Checked Locking)은 인스턴스 생성 여부를 싱글톤 패턴 lock 전에 한 번, 객체를 생성하기 전에 한 번 총 2번 체크한다.
- 인스턴스가 존재하지 않을 때에만 잠금을 걸 수 있기 때문에 앞서 생겼던 lock으로 인한 성능저하를 어느정도 극복 가능하다.

    ```java
    public class Singleton {
    
    	private volatile Singleton instance;
    
    	private Singleton() {
    	}
    
    	public Singleton getInstance() {
    		if (instance == null) {//인스턴스가 없을 때
    			synchronized (Singleton.class) {//lock
    				if (instance == null) {//인스턴스가 없을 때
    					instance = new Singleton();//객체 생성
    				}
    			}
    		}
    		return instance;
    	}
    }
    
    ```


### `volatile` 키워드

- L1, L2, L3, L4 → 메모리 위의 CPU 캐시 메모리

  ![image](https://github.com/takealook97/cs-basic/assets/118447769/d1417cf0-3c8e-4027-8332-b770cf813e94)  

- 자바에서는 멀티쓰레드 환경에서 Task를 수행하는 동안 성능 향상을 위해 메인 메모리에서 읽은 변수 값을 CPU 캐시에 저장하게 된다.
- 이렇게 되면 쓰레드가 변수값을 읽어올 때 변수를 메인 메모리(RAM)으로부터 가져오는 것이 아니라 각각의 CPU 캐시에서 가져오게 된다. → 변수 값 불일치 문제 발생
- `volatile` 키워드를 통해서 캐시메모리가 아닌 메인메모리를 기반으로 저장하고 읽어오기 때문에 이 문제를 해결할 수 있다.
- 이는 멀티 쓰레드 환경에서 하나의 쓰레드만 read & write을 하고 나머지 쓰레드가 read하는 상황에서 가장 최신의 값을 보장한다.

## 7.  enum

- enum의 인스턴스는 기본적으로 쓰레드 세이프한 점이 보장되기 때문에 이를 통해 생성할 수 있다.

    ```java
    public enum SingletonEnum {
    	INSTANCE;
    	public void oortCloud() {
    	}
    }
    
    ```


### 추천하는 방법

- 5번(정적 멤버와 LazyHolder)
    - 가장 널리 쓰이는 방식
- 7번(enum)
    - 이펙티브 자바를 쓴 조슈아 블로크가 추천한 방법\
    - “A single-element enum type is the best way to implement a singleton.”