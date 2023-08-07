# CS 스터디 WEEK01

## JSON이란 ?

**Javascript 객체 문법**으로 구조화된 **데이터 교환 형식**, python, javascript, java 등 **여러 언어에서 데이터 교환형식**으로 쓰이며 객체문법 말고도 **단순 배열, 문자열도 표현** 가능

1. Javascript 객체 문법

   - 키와 값으로 구성됨 `{key: value}`
   - 이미 존재하는 키를 중복 선언하면 나중에 선언한 해당 키에 대응한 값이 덮어쓰이게 됨
     ⇒ 아래 name은 king이 됨

   ```json
   [
     {
       "name": "kundol",
       "name": "king",
       "age": 30
     },
     {
       "name": "yang",
       "age": 30
     }
   ]
   ```

   ```js
   const fs = require('fs');
   const path = require('path');
   const a = fs.readFileSync(path.join(_dirname, 'a.json'));
   const b = JSON.parse(a); // 여기서 json 객체를 javascript에서 쓸 수 있도록 변환
   // json 배열에 접근할 때 아래 두 가지 모두 가능
   console.log(b[0]['name']);
   console.log(b[0].name);
   ```

2. 데이터 + 교환 형식

   데이터는 추상적인 아이디어에서부터 시작해 구체적인 측정에 이르기까지 다양한 의미로 쓰임. 실험, 조사, 관찰 등으로부터 얻은 사실이나 자료 등을 의미

3. 여러 언어에서 쓰임

   객체, 해시테이블, 딕셔너리 등으로 변환되어 쓰임

   ‘독립적’ 여러가지 언어, 플랫폼에서 독립적으로 작동

4. 단순 배열이나 문자열 표현

   {} 형식이 아니라 [] 도 가능하고, “”도 가능함

5. JSON의 타입

   Javascript 객체와 유사하지만 undefined, 메서드을 포함하지 않는다.

   - 수(Number)
   - 문자열(String)
   - 참/거짓(Boolean)
   - 배열(Array)
   - 객체(Object)
   - null

6. JSON 직렬화, 역직렬화

   직렬화란 외부의 시스템에서도 이용할 수 있도록 바이트(byte) 형태로 데이터를 변환하는 기술이며 역직렬화는 반대를 의미

   역직렬화: JSON.parse()

   JS object → JSON(직렬화): JSON.stringify()

7. JSON의 활용

   JSON은 프로그래밍 언어와 프레임워크 등에 독립적이므로, 서로 다른 시스템 간에 데이터를 교환하기에 좋다.

   주로 API의 반환형태, 시스템을 구성하는 설정 파일에 활용

   ex) 업비트의 API, package.json

## XML

XML(Extensible Markup Language)은 **마크업 형태**를 쓰는 데이터 교환형식

⇒ 태그 등을 이용하여 문서나 데이터의 구조를 나타내는 방법 (속성 부여도 가능)

### 구성

1. 프롤로그: 버전, 인코딩
2. 루트 요소(단 하나만)
3. 하위 요소들

```xml
<?xml version="1.0" encoding="UTF-8"?> <!--프롤로그-->
<OSTList> <!-- 루트 요소 -->
	<OST>
		<name>마녀 배달부 키키</name> <song>따스함에 둘러쌓인다면</song>
	</OST>
</OSTList>
```

### HTML과 XML의 차이

1. HTML의 용도는 데이터를 표시 / XML은 데이터를 저장 및 전송
2. HTML에는 미리 저장된 태그가 있지만 사용자는 XML에서 고유한 태그를 만들고 정의 가능
3. XML은 대/소문자를 구분하지만 HTML은 구분하지 않음

### JSON과 XML의 차이

JSON과 비교했을 때 닫힌 태그가 들어가기 때문에 무거움

Javascript Object로 변환하기 위해서 JSON 보다 더 많은 단계가 필요함(JSON.parse())

### XML의 활용

sitemap.xml으로 쓰임

여러 언어에서도 독립적으로 쓰임
