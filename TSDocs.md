목차

1. [TS 맛보기](#ts-맛보기)<br>
   &nbsp;&nbsp; 1-1. [타입 추론(Types by Inference)](#타입-추론types-by-inference)<br>
   &nbsp;&nbsp; 1-2.[타입 정의하기 (Defining Types)](#타입-정의하기-defining-types)<br>
   &nbsp;&nbsp; 1-3 [타입 구성](#타입-구성)<br>
   &nbsp;&nbsp;&nbsp;&nbsp; 1-3-1. [유니언(Unions)](#유니언unions)<br>
   &nbsp;&nbsp;&nbsp;&nbsp; 1-3-2. [제네릭(Generics)](#제네릭-generics)<br>
   &nbsp;&nbsp; 1-4 [구조적 타입 시스템(Structural Type System)](#구조적-타입-시스템structural-type-system)<br>
2. [Handbook Basic](#handbook-basic)<br>
   &nbsp;&nbsp; 2-1. [TS는 JS 프로그램의 정적 타입 검사자의 역할](#ts는-js-프로그램의-정적-타입-검사자의-역할)<br>
   &nbsp;&nbsp; 2-2. [정적 타입 검사](#정적-타입-검사)<br>
   &nbsp;&nbsp; 2-3. [런타임 오류](#런타임-오류)<br>
   &nbsp;&nbsp; 2-4. [프로그래밍 도구로서의 타입](#프로그래밍-도구로서의-타입)<br>
   &nbsp;&nbsp; 2-5. [TS 컴파일러 설치방법](#ts-컴파일러-설치방법)<br>
   &nbsp;&nbsp; 2-6. [명시적 타입](#명시적-타입)<br>
   &nbsp;&nbsp; 2-7. [다운레벨링](#다운레벨링)<br>
   &nbsp;&nbsp; 2-8. [엄격도](#엄격도)
3. [Handbook Every Types](#everyday-types)<br>
   &nbsp;&nbsp; 3-1. [원시타입](#원시타입)<br>
   &nbsp;&nbsp; 3-2. [배열](#배열)<br>
   &nbsp;&nbsp; 3-3. [any 타입](#any-타입)<br>
   &nbsp;&nbsp; 3-4. [변수에 대한 타입 표기](#변수에-대한-타입-표기)<br>
   &nbsp;&nbsp; 3-5. [함수](#함수)<br>
   &nbsp;&nbsp; 3-6. [익명함수](#익명함수)<br>
   &nbsp;&nbsp; 3-7. [객체 타입](#객체-타입)<br>
   &nbsp;&nbsp; 3-8. [옵셔널 프로퍼티](#옵셔널-프로퍼티)<br>
   &nbsp;&nbsp; 3-9. [유니언 타입](#유니언-타입)<br>
   &nbsp;&nbsp; 3-10. [타입 별칭](#타입-별칭)<br>
   &nbsp;&nbsp; 3-11. [인터페이스](#인터페이스)<br>
   &nbsp;&nbsp;&nbsp;&nbsp; 3-11-1. [타입 별칭과 인터페이스의 차이점](#타입-별칭과-인터페이스의-차이점)<br>
   &nbsp;&nbsp; 3-12. [타입 단언](#타입-단언)<br>
   &nbsp;&nbsp;&nbsp;&nbsp; 3-12-1. [타입 단언을 사용하여 타입을 강제 변환](#타입-단언을-사용하여-타입을-강제-변환)<br>
   &nbsp;&nbsp; 3-13. [리터럴 타입](#리터럴-타입)<br>
   &nbsp;&nbsp; 3-14. [리터럴 추론](#리터럴-추론)<br>
   &nbsp;&nbsp; 3-15. [null과 undefined](#null과-undefined)<br>
   &nbsp;&nbsp;&nbsp;&nbsp; 3-15-1. [strictNullChecks가 설정되지 않았을 때](#strictnullchecks가-설정되지-않았을-때)<br>
   &nbsp;&nbsp;&nbsp;&nbsp; 3-15-2. [strictNullChecks가 설정되었을 때](#strictnullchecks가-설정되었을-때)<br>
   &nbsp;&nbsp;&nbsp;&nbsp; 3-15-3. [Null 아님 단언 언산자](#null-아님-단언-연산자)<br>
   &nbsp;&nbsp; 3-16-1. [열거형](#열거형)<br>

# TS 맛보기

## 타입 추론(Types by Inference)

- TS는 js 언어를 알고 있으며, 대부분의 경우 타입을 생성해준다.

## 타입 정의하기 (Defining Types)

- JS는 다양한 디자인 패턴을 가능하게 하는 동적 언어이다.
- 몇몇 디자인 패턴은 자동으로 타입을 제공하기 힘들 수 있다.(동적 프로그래밍을 사용하고 있을 경우)
  - 이러한 경우 TS는 TS에게 타입이 무엇이 되어야 하는지 명시 가능한 JS 언어의 확장을 지원한다.

```js
// name : string과 id : number을 포함하는 추론 타입을 가진 객체를 생성한다.
const user = {
  name: "kamja",
  id: 44,
};
// user 객체의 형태를 명시적으로 나타내기 위해 interface로 선언한다.
interface User {
  name: string;
  id: number;
}
// 변수 선언 뒤에 :TypeName의 구문을 사용하여 JS 객체가 새로운 interface의 형태를 따르고 있음을 선언할 수 있다.
const user2: User = {
  name: "kamja",
  id: 44,
};
// 해당 인터페이스에 맞지 않는 객체를 생성하면 TS는 경고를 준다.
```

JS는 클래스와 객체 지향 프로그래밍을 지원하기에, TS도 클래스와 객체 지향 프로그래밍을 지원한다.

- 인터페이스는 클래스로도 선언할 수 있다.

```js
interface User {
  name: string;
  id: number;
}
class UserAccount {
  name: string;
  id: number;

  constructor(name: string, id: number) {
    this.name = name;
    this.id = id;
  }
}
const user: User = new UserAccount("kamaj", 44);
```

인터페이스는 함수에서 매개변수와 리턴 값을 명시하는데 사용되기도 한다.

```js

interface User{
    name : string;
    id : number;
}
function getAdminUser() : User{
    //...
}
functin deleteUser(user : User){
    //...
}
```

JS에서 사용할 수 있는 타입

- boolean, bigint, null, number, string, symbol, object, undefined

TS에서 사용할 수 있는 타입

- JS에서 사용할 수 있는 타입, any(무엇이든 허용), unknown(이 타입을 사용하는 사람이 타입이 무엇인지 선언했는지 확인), never(이 타입은 발생될 수 없다.), void(undefined를 리턴하거나, 리턴값이 없는 함수)

타입을 정의하는 2가지 방법

- 타입을 구축할 때는 interface 방법과 type 방법이 있다.
  - `interface의 사용을 우선적`으로 하고 `특정 기능이 필요할 때 type을 사용`한다.

## 타입 구성

### 유니언(Unions)

- 유니언은 타입이 여러 타입 중 하나일 수 있음을 선언하는 방법이다.
- booelan 타입을 true or false로 설명할 수 있다.

```js
type MyBool = true | false;
// MyBool위에 마우스를 올리면, boolean으로 분류된다.
```

- 유니언 타입이 사용되는 예

```js
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

- 유니언이 array 또는 string을 받는 함수

```js
function getLength(obj: string | string[]) {
  return obj.length;
}
```

Ts는 코드가 시간에 따라 변수가 변경되는 방식을 이해하며, 이러한 방식을 통해 타입을 걸러낼 수 있다.
|Type|Predicate|
|:---:|:---:|
|string| typeof s === "string"|
|number| typeof n === "number"|
|boolean| typeof b === "boolean"|
|undefined| typeof undefined === "undefined"|
|function| typeof f === "function"|
|array|Array.isArray(a)|

### 제네릭 (Generics)

- 제네릭은 타입에 변수를 제공하는 방법이다.
- 배열이 일반적인 예시이며, 제네릭이 없는 배열은 어떤 것이든 포함할 수 있다.
  - 제네릭이 있는 배열은 배열 안의 값을 설명할 수 있따.

```js
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```

- 제네릭을 사용하는 경우 고유 타입을 선언할 수 있다.

```js
// @errors: 2345
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}

// 이 줄은 TypeScript에 `backpack`이라는 상수가 있음을 알리는 지름길이며
// const backpack: Backpack<string>이 어디서 왔는지 걱정할 필요가 없습니다.
declare const backpack: Backpack<string>;

// 위에서 Backpack의 변수 부분으로 선언해서, object는 string입니다.
const object = backpack.get();

// backpack 변수가 string이므로, add 함수에 number를 전달할 수 없습니다.
backpack.add(23);
```

## 구조적 타입 시스템(Structural Type System)

- TS의 핵심 원칙 중 하나는 타입 검사가 값이 있는 형태에 집중한다는 것이다.
- 이를 덕 타이핑 or 구조적 타이핑 이라 한다.
- 구조적 타입 시스템에서 두 객체가 같은 형태를 가지면 같은 것으로 간주된다.

```js
interface Point {
  x: number;
  y: number;
}
function pointPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
// 12, 26을 출력한다.
const point = { x: 12, y: 26 };
printPoint(point);
```

- point 변수는 Point 타입으로 선언된 적이 없지만, TS는 타입 검사에서 point의 형태와 Point의 형태를 비교한다.
- 둘 다 같은 형태이기 때문에 통과한다.
- 형태 일치에는 일치시킬 객체의 필드의 하위 집합만 필요하다.

```js
interface Point {
  x: number;
  y: number;
}
function printPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
const point = { x: 12, y: 26, z: 89 };
printPoint(point); // 12, 26
const rect = { x: 33, y: 3, width: 30, height: 80 };
printPoint(rect); // 33, 3
const color = { hex: "#187ABF" };
printPoint(color);
//Argument of type '{ hex: string; }' is not assignable to parameter of type 'Point'. Type '{ hex: string; }' is missing the following properties from type 'Point': x, y
```

- 구조적으로 클래스와 객체가 형태를 따르는 방법에는 차이가 없다.

```js
interface Point {
  x: number;
  y: number;
}
function printPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
class VirtualPoint {
  x: number;
  y: number;
  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}
const newVPoint = new VirtualPoint(13, 56);
printPoint(newVPoint); // 13, 56
```

- 객체 또는 클래스에 필요한 모든 속성이 존재한다면, TS는 구현 세부 정보에 관계없이 일치하게 본다.

# Handbook Basic

## TS는 JS 프로그램의 정적 타입 검사자의 역할

- 즉, 코드가 실행되기 전에 실행하고(정적), 프로그램 타입이 정확한지 검사하는 도구(타입 검사)이다.

JS 런타임은 코드가 실행될 때 자신이 무엇을 해야 할지 결정하기 위하여 값의 타입 즉, 해당 값이 어떤 동작과 능력을 가지고 있는지 확인한다.

- 이것이 TypeError가 암시하는 바이다.

string, number와 같은 원시 타입의 값의 경우 typeof 연산자를 사용하면 각 값들의 타입을 실행 시점에 알 수 있다.
하지만, 그 밖의 값들 예를 들어 함수값의 경우 값의 타입을 실행 시점에 알 수 없다.

```js
const message = "Hello World";
typeof message; // string

function fn(x) {
  return x.flip();
}
typeof fn; // function
```

- fn 함수를 보면, 인자로 전달된 객체가 호출 가능한 프로퍼티인 flip을 가져야만 fn 함수가 잘 작동할 것이라는 것을 코드를 읽음으로 알 수 있다.
  - 하지만, JS는 이러한 정보를 코드가 실행되는 동안 알지 못한다.
    - JS에서 함수 fn이 특정 값과 어떤 동작을 수행하는지 알 수 있는 유일한 방법은 호출하고 무슨 일이 일어나는지 보는 것이다.
      - 이와 같은 동작은 코드 실행 전 예측을 어렵게 한다.
        - 즉, 코드가 어떤 동작 결과를 보일지 코드를 작성하는 동안에는 알기 어렵다.
      - 이러한 측면에서 볼 때 `타입`이란 `어떤 값이 함수 fn으로 전달될 수 있고, 어떤 값은 실행에 실패할 것임을 설명하는 개념이다.`
      - `JS는 오직 동적 타입만을 제공하며, 코드를 실행해야만 어떤 일이 벌어지는지 비로소 확인할 수 있다.`
        - 이에 대한 대안은 `정적 타입 시스템을 사용하여 코드가 실행되기 전 코드에 대하여 예측하는 것이다.`

## 정적 타입 검사

- TS와 같은 정적 타입 검사기의 역할
  - 코드를 실행하기 전 버그를 미리 발견할 수 있게 도와준다.
- 정적 타입 시스템은 개발자가 작성한 프로그램에서 사용된 값들의 형태와 동작을 설명한다.
  - TS와 같은 타입 검사기는 이 정보를 활용하여 프로그램이 제대로 작동하지 않을 때 개발자에게 알려준다.

```js
const message = "hello";

message();
// this expression is not callable. Type 'String has no call signatures
```

- 위의 코드를 TS로 실행하면, 코드가 실행되기 전 오류 메시지를 확인하게 된다.

## 런타임 오류

- JS 런타임이 무언가 이상하다고 개발자에게 직접 말해주는 오류
- 이러한 오륜느 예기치 못한 문제가 발생했을 때 JS가 어떻게 대응해야 하는지 ECMAScript 명세에서 절차를 제공하기 때문에 발생한다.
  - ex) 호출 가능하지 않은 것에 대하여 호출을 시도할 경우
    - JS에서 객체에 존재하지 않는 프로퍼티에 접근할 경우 오류를 발생시키지 않고 undefined를 반환한다.
    - TS에서 객체에 존재하지 않는 프로퍼티에 접근할 경우 오류를 발생시킨다.

```js
const user = {
  name: "kamja",
  age: 24,
};
// JS인 경우
user.location; // undefined를 반환한다.

// TS인 경우
user.location; // Property "location" does not exist on type "{name : string; age : number; }"
```

TS가 잡아낼 수 있는 겉으로 드러나지 않는 오류들

```js
// 오타로 인한 에러
const announcement = "Hello World!";

announcement.toLocaleLowerCase(); // 오타로 인한 에러 발생

// 호출되지 않은 함수로 인한 에러
function flipCoin() {
  return Math.random < 0.5; // Operator "<" cannot be applied to types "() => number" and "number"
}

// 기본적인 논리 오류
const value = Math.random() < 0.5 ? "a" : "b";
if (value !== "a") {
  // ...
} else if (value === "b") {
  // This condition will always return false since the types "a" and "b" have no overlap
  // 즉, 이 블록은 실행되지 않는다.
}
```

## 프로그래밍 도구로서의 타입

- TS는 개발자가 코드 상에서 실수를 저질렀을 때 버그를 잡아준다.(실수를 저지르는 그 순간)
- 타입 검사기는 개발자가 변수 또는 다른 프로퍼티 상의 올바른 프로퍼티에 접근하고 있는지 여부를 검사할 수 있도록 관련 정보를 가지고 있다.
  - 즉, TS는 코드 수정에 활용될 수 있고, 개발자가 코드를 입력할 때 오류 메시지를 제공하거나 코드 완성 기능(자동 완성)을 제공할 수 있다.

## TS 컴파일러 설치방법

TS 컴파일러를 설치하기 위해선 다음과 같은 명령어를 사용한다.
`npm i -g typescript`

- 위 코드를 실행하면 TS 컴파일러 tsc가 전역 설치된다.
  - tsc를 로컬 node_modules 패키지로부터 실행하고자 한다면 npx 또는 유사한 도구를 사용하면 된다.

## 명시적 타입

- ex) TS에게 person이 string 타입이고, date가 Date 객체여야 한다는 것을 명시적으로 알려준다. 또한 date의 toDateString() 메서드를 사용한다.

```js
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}
```

- person과 date에 대하여 타입 표기를 수행하며 greet가 호출될 때 함께 사용될 수 있는 값들의 타입을 설명했다.
  - 즉, greet 함수는 string 타입의 person과 Date 타입의 date를 가진다고 해석할 수 있다.
- 이 기능을 사용하면 TS는 개발자가 해당 함수를 올바르게 사용하지 않았을 경우 이를 알려줄 수 있다.
- ex)

```js
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}

greet("kamja", Date());
// Argument of type "string" is not assignable to parameter of type "Date"
```

에러가 발생한 이유

- JS에서 Date()를 호출하면 string을 반환한다.
  - 즉, new Date()를 사용하여 Date 타입을 생성해야 비로소 처음 기대했던 결과를 반환받을 수 있게 된다.
    - 즉, 다음과 같이 호출해야 한다.

```js
greet("kamja", new Date());
```

`TS는 타입을 추론할 수 있기에 명시적인 타입 표기를 항상 작성해야 할 필요는 없다.`

```js
let msg = "hello"; // let msg : string
// msg 변수이름 위에 마우스 오버시 TS가 추론한 타입 확인 가능
```

위에서 작성한 greet 함수는 tsc 컴파일러로 컴파일하여 JS로 출력을 얻으면 다음과 같다.

```js
"use strict";
function greet(person, date) {
  console.log(
    "Hello ".concat(person, ", today is ").concat(date.toDateString(), "!")
  );
}
greet("Maddison", new Date());
```

알 수 있는 내용은 다음과 같다.

- person과 date 인자는 더 이상 타입 표기를 가지지 않는다.
  - 타입 표기는 JS의 일부가 아니므로, TS를 수정 없이 그대로 실행할 수 있는 브라우저나 런타임은 현재 존재하지 않는다. -이것이 TS를 사용하고자 할 때 컴파일러가 필요한 이유이다.
    - 즉, TS 전용 코드(타입 표기 등)를 제거하거나 변환하여 실행할 수 있도록 만드는 방법이 필요하다.
      - 대부분의 TS 전용 코드는 제거되며, 타입 표기 또한 완전히 지워진다.
      - 즉, `타입 표기는 프로그램의 런타임을 전혀 수정하지 않는다.`
- 템플릿 리터럴을 사용하여 작성된 문장은 일반 문자열로 변환된다.

## 다운레벨링

템플릿 리터럴이 구형 코드로 변환된 이유

```js
console.log(`Hello ${person}, today is ${date.toDateString()}!`);
// 위의 코드가 아래의 코드로 변환된 이유
console.log(
  "Hello ".concat(person, ", today is ").concat(date.toDateString(), "!")
);
```

- 템플릿 리터럴은 ES6에서 등장한 개념이다.
  - TS는 새 버전의 ECMAScript 코드를 ES3, ES5와 같은 예전 버전의 코드들로 다시 작성해준다.
    - 이러한 과정을 다운레벨링이라고 한다.
  - --target 플래그를 설정하면 변경할 코드들의 버전을 설정할 수 있다.
    - --target es2015를 실행하면 ES5로 동작한다.
      - 즉, ES5가 지원되는 런타임이기만 하면 해당 코드가 실행될 수 있도록 변환된다는 의미이다.

## 엄격도

- 타입 검사는 선택사항이며, 타입 추론은 가장 관대한 기준으로 이루어지고, 잠재적인 null/undefined 값에 대한 검사는 이루어지지 않는다.
- CLI에서 `--strict` 플래그를 설정하거나 tsconfig.json에 "strict' : true를 추가하면 모든 플래그를 활성화하게 되지만, 각각의 플래스를 개별적으로 끌 수 있다.
  - 반드시 알아야 하는 두 가지 가장 중요한 옵션은 noImplicitAny와 strictNullChecks이다.
- noImplicitAny
  - 몇몇 경우에서 TS는 값의 타입을 추론하지 않고 가장 관대한 타입인 any로 간주한다.
    - any타입을 사용하면 TS를 사용하는 이유가 없다.
    - noImplicitAny 플래그를 활성화하면 타입이 any로 암묵적으로 추론되는 변수에 대하여 오류를 발생시킨다.
- strictNullChecks
  - null과 undefined를 보다 명시적으로 처리하며, null 및 undefined 처리를 잊었는지 여부를 확인시켜준다.

# Everyday Types

## 원시타입

string

- 문자열 값을 나타낸다.

number

- 숫자를 나타낸다.
- JS는 정수를 위한 런타임 값을 별도로 가지지 않으므로, int 또는 float과 같은 것은 존재하지 않는다.
  - 즉, 모든 수는 number이다.

boolean

- true와 false라는 두 가지 값만 가진다.

## 배열

- 배열의 타입을 지정할 때 number[] 구문을 사용할 수 있다.
  - 이 구문은 모든 타입에서 사용할 수 있다.
    - ex) string[]은 문자열 배열이다.
  - 이 타입은 Array<number>와 같은 형태로 적을 수 있으며, number[]와 동일한 의미를 가진다.
    - Array<number>은 제네릭 문법이다.
  - [number]는 전혀 다른 의미를 가진다.

## any 타입

- 특정 값으로 인하여 타입 검사 오류가 발생하는 것을 원하지 않을 때 사용한다.
- 어떤 값의 타입이 any이면, 해당 값에 대하여 임의의 속성에 접근할 수 있고(이때 반환되는 타입도 any), 함수인 것처럼 호출할 수 있고, 다른 임의 타입의 값에 할당하거나(받거나), 그 밖에도 구문적으로 유효한 것이라면 무엇이든 할 수 있다.

```js
let obj: any = { x: 0 };
// 아래 코드들은 모두 오류없이 정상적으로 실행된다.
// any 타입을 사용하면 추가적인 타입 검사가 비활성화된다.
obj.foo();
obj();
obj.bar = 100;
obj = "hello";
const n: number = obj;
```

- any 타입은 타입을 새로 정의하고 싶지 않을 때 유용하게 사용할 수 있다.
  - 하지만, any 타입은 타임 검사를 하지 않기에 사용을 지양해야 한다.
    - 컴파일러 플래그 noImplicitAny를 사용하면 암묵적으로 any로 간주하는 모든 경우에 오류를 발생시킨다.

## 변수에 대한 타입 표기

```js
let myName: string = "kamja";
```

- 변수의 타입을 명시적으로 지정하기 위해 타입 표기를 추가할 수 있으며 이는 선택사항이다.
- `TS에선 타입의 표기를 항상 타입의 대상 뒤쪽에 위치한다.`

## 함수

- 함수를 선언할 때, 함수가 허용할 매개변수 타입을 선언하기 위하여 각 매개변수 뒤에 타입을 표기할 수 있다.
- 매개변수 타입은 매개변수 이름 뒤에 표기한다.

```js
function greet(name: string): number {
  console.log(`Hi ${name.toUpperCase()}!!`);
  return 44;
}
// 매개변수에 타입이 표기되었다면, 해당 함수에 대한 인자는 검사가 이뤄진다.
greet(44); // Argument of type 'number' is not assignable to parameter of type 'string'.
```

- 반환 타입 표기
  - 반환 타입은 매개변수 목록 뒤에 표기한다.
    - TS가 해당 함수에 들어있는 return 문을 바탕으로 반환 타입을 추론하기에 반환 타입은 잘 표기하지 않는다.

## 익명함수

- 함수가 코드상에서 위치한 곳을 보고 해당 함수가 어떻게 호출될지 알아낼 수 있다면, TS는 해당 함수의 매개변수에 자동으로 타입을 부여한다.

```js
// 아래 코드에느 타입 표기가 전혀 없지만, TS는 오류를 감지할 수 있다.
const names = ["kamja", "kokuma"];

// 함수에 대한 문맥적 타입 부여
names.forEach(function (item) {
  console.log(item.toUppercase());
  // Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
});

// 화살표 함수에도 문맥적 타입 부여는 적용된다.
names.forEach((item) => {
  console.log(item.toUppercase());
  // Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
});
```

- 매개변수 item에는 타입이 표기되지 않았음에도 불구하고, TS는 item의 타입을 알아내기 위해 배열의 추론된 타입과 더불어 forEach 함수의 타입을 활용한다.
  - 이 과정을 문맥적 타입 부여라고 한다.
    - 함수가 실행되는 문맥을 통해 해당 함수가 가져야 하는 타입을 알 수 있기 때문

## 객체 타입

- 원시 타입을 제외하고 가장 많이 마주치는 타입이다.
- 객체는 프로퍼티를 가지는 JS값을 말한다.
- 객체 타입을 정의하기 위해선 해당 객체의 프로퍼티들과 각 프로퍼티의 타입을 나열하기만 하면 된다.
- 객체의 각 프로퍼티의 타입 표기 또한 선택사항이다.
  - 타입을 표기하지 않으면 표기하지 않은 프로퍼티는 any 타입으로 간주한다.

```js
// 매개 변수의 타입은 객체로 표기되고 있습니다.
function printCoord(pt: { x: number, y }) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```

## 옵셔널 프로퍼티

- 객체 타입을 사용할 때 일부 또는 모든 프로퍼티의 타입을 선택적인 타입 즉, 옵셔널로 지정할 수 있다.
  - 프로퍼티 이름 뒤에 ?를 붙이면 된다.

```js
function printName(obj: { first: string, last?: string }) {
  // ....
}
// 둘 다 가능
printName({ first: "Bob" });
printName({ first: "Alice", last: "Alisson" });
```

- JS에서 존재하지 않는 프로퍼티에 접근했을 때, 런타임 오류가 발생하지 않고 undefined 값을 얻게 된다.
  - 즉, 옵셔널 프로퍼티를 읽었을 때 해당 값을 사용하기에 앞서 undefined인지 여부를 확인해야 한다.

```js
function printName(obj: { first: string, last?: string }) {
  // obj.last의 값이 제공되지 않는다면 오류가 발생한다.
  console.log(obj.last.toUpperCase()); // Object is possibly "undefined"
  if (obj.last !== undefined) {
    // obj.last의 값이 존재할때 실행할 코드
    console.log(obj.last.toUpperCase());
  }
  // 최신 JS 문법을 사용하였을 때 또다른 안전한 코드
  console.log(obj.last?.toUpperCase());
}
```

## 유니언 타입

- 유니언 타입은 서로 다른 두 개 이상의 타입을을 사용하여 만드는 것이다.
- 유니언 타입의 값은 타입 조합에 사용된 타입 중 무엇이든 하나를 타입으로 가질 수 있다.
  - 조합에 사용된 각 타입을 유니언 타입의 멤버라고 부른다.
    문자열 or 숫자를 받을 수 있는 유니언 타입

```js
function printId(id: number | string) {
  console.log(`Your id is ${id}`);
}
// 가능
printId(101);
printId("202");
// 오류 발생
printId({ myId: 22342 }); // Argument of type "{myId : number;}" is not assignable to parameter of type "string \ number"
```

- 유니언 타입을 사용할때 유니언 타입의 모든 멤버에 대하여 유효한 작업일 때만 허용된다.
  - 즉, string | number라는 유니언 타입의 경우, string 타입에만 유효한 메서드는 사용할 수 없다.

```js
function printId(id: number | string) {
  console.log(id.toUpperCase());
  // Property 'toUpperCase' does not exist on type 'string | number'. Property 'toUpperCase' does not exist on type 'number'.
}

function welcomePeople(x: string[] | string) {
  if (Array.isArray(x)) {
    // 여기에서 'x'는 'string[]' 타입입니다
    console.log("Hello, " + x.join(" and "));
  } else {
    // 여기에서 'x'는 'string' 타입입니다
    console.log("Welcome lone traveler " + x);
  }
}
```

- 유니언의 모든 멤버가 어떤 프로퍼티를 공통으로 가진다면, 좁히기 없이도 해당 프로퍼티를 사용할 수 있게 된다.

```js
// 반환 타입은 "number[] | string"으로 추론된다.
function getFirstThree(x: number[] | string) {
  return x.slice(0, 3);
}
```

`유니언은 의미상 합집합을 뜻하는데, 실제로는 유니언 타입이 프로퍼티들의 교집합을 가리키는 것처럼 보여 헷갈리게 느낄 수 있다. 이는 유니언이라는 명칭이 타입 이론에서 비롯되었기 때문이다. number | string 유니언 타입은 각각의 타입을 가지는 값들에 대하여 합집합을 취하여 구성된다. 두 집합과 각각의 집합에 대한 특성이 주어졌을 때, 두 집합의 유니언에는 각각의 특성들의 교집합만이 적용된다는 점에 유의해야한다.  예를 들어, 한 방에는 모자를 쓰고 키가 큰 사람들이 있고 다른 방에는 모자를 쓰고 스페인어를 사용하는 사람들이 있다고 가정하고, 이때 두 방을 합친다면, 모든 사람들에 대하여 우리가 알 수 있는 사실은 바로 누구나 반드시 모자를 쓰고 있다는 것이다.`

## 타입 별칭

- 똑같은 타입을 한 번 이상 재사용하거나 또 다른 이름으로 부르고 싶을 때 타입 별칭을 사용한다.
  - 타입 별칭은 타입을위한 이름을 제공한다.

```js
type Point = {
  x: number,
  y: number,
};

function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100 });
```

- 타입 별칭을 사용하면 단지 객체 타입뿐이 아니라 모든 타입에 대하여 새로운 이름을 부여할 수 있다.
  - 유니언 타입에 대하여 타입 별칭을 부여한 예

```js
type ID = number | string;
```

## 인터페이스

- 인터페이스 선언은 객체 타입을 만드는 또 다른 방법이다.

```js
interface Point {
  x: number;
  y: number;
}

function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100 });
```

- 타입 별칭을 사용한 경우와 마찬가지로, 위 예시 코드는 마치 타입이 없는 임의의 익명 객체를 사용하는 것처럼 동작한다.
- TS는 오직 printCoord에 전달된 값의 구조에만 관심을 갖는다.
  - 즉, 예측된 프로퍼티를 가졌는지 여부만 따진다.
  - 이처럼 타입이 가지는 구조와 능력에만 관심을 가진다는 점이 TS가 구조적 타입 시스템이라고 불리는 이유이다.

### 타입 별칭과 인터페이스의 차이점

- 인터페이스가 가지는 대부분의 기능은 타입 별칭에서도 동일하게 사용 가능하다.
- 단, 타입 별칭은 새 프로퍼티를 추가하도록 개방될 수 없는 반면, 인터페이스의 경우 항상 확장이 가능하다.
- 인터페이스는 항상 오류 메시지에 이름이 나타난다.
- 타입 별칭은 선언 병합에 포함될 수 없지만, 인터페이스는 포함될 수 있다.
- 인터페이스는 오직 객체의 모양을 선언하는 데만 사용되며, 기존 원시 타입에 별칭을 부여하는 데에는 사용할 수 없다.
  - 즉, `interface를 우선적으로 사용하고 문제 발생 시 타입 별칭을사용한다.`

인터페이스 확장

```js
interface Animal {
  name: string;
}
interface Bear extends Animal {
  hondy: boolean;
}
const bear = getBear();
bear.name;
bear.honey;
```

기존의 인터페이스에 새 필드 추가하기

```js
interface Window {
  title: string;
}
interface Window {
  ts: TypeScriptAPI;
}
const src = `const a = "Hello World"`;
window.ts.transpileModule(src, {});
```

## 타입 단언

- TS보다 개발자가 어떤 값의 타입에 대한 정보를 더 잘 아는 경우
- ex) 코드상에서 `document.getElementById`가 사용되는 경우, TS는 이때 `HTMLElement` 중 무언가가 반환된다는 것만을 알 수 있는 반면, 개발자는 펭지 상에서 사용되는 IDㄹㅗ는 언제나 `HTMLCanvasElement`가 반환된다는 사실을 이미 알고 있을 수 있다.
  - 이러한 경우 타입 단언을 사용하여 타입을 좀 더 구체적으로 명시한다.

```js
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
// 타입 표기와 마찬가지로, 타입 단언은 컴파일러에 의해 제거되며 코드의 런타임에는 영향을 미치지 않는다.
// 코드가 .tsx 파일이 아닌경우 <>괄호를 사용하는 것 또한 가능하며, 동일한 의미를 지닌다.
const myCanvas2 = <HTMLCanvasElement>document.getElementById("main_canvas");
```

- 타입 단언은 컴파일 시간에 제거되므로, 타입 단언에 관련된 검사는 런타임 중에 이뤄지지 않는다.
  - 즉, 타입 단언이 틀리더라도 예외나 null이 발생하지 않는다.
- 타입 단언은 구체적인 or 덜 구체적인 버전의 타입으로 변환하는 타입 단언만 허용된다.
  - 즉, 불가능한 강제 변환을 방지한다.

```js
const x = "hello" as number; // Conversion of type 'string' to type 'number' may be a mistake because neither type sufficiently overlaps with the other. If this was intentional, convert the expression to 'unknown' first.
```

### 타입 단언을 사용하여 타입을 강제 변환

- 타입 단언을 2번 사용한다.
  - any 타입으로 변환 후 원하는 타입으로 변환한다.

```js
const a = (변수명 as any) as string;
```

## 리터럴 타입

- string과 number와 같은 일반적인 타입 이외에도, 구체적인 문자열과 숫자 값을 타입 위치에 지정할 수 있다.

```js
let chaningString = "Hello world!"; // let chaningString : string
// 변수 chaningString은 어떤 문자열이든 모두 나타낼 수 있다.
// 이는 TS의 타입 시스템에서 문자열 타입 변수를 다루는 방식과 동일하다.

const constString = "Hello world!"; // const constString : string
// 변수 constString은 오직 단 한 종류의 문자열만 나타낼 수 있으며 이는 리터럴 타입의 표현 방식이다.
```

- 리터럴 타입만사용한다면 유의미하지 않다.
  - 리터럴 타입과 유니언 타입을 함꼐 사용하면 유용한 개념들을 표현할 수 있다.
    - ex) 특정 종류의 값들만을 인자로 받을 수 있는 함수를 정의할 수 있다.

```js
function printText(s: string, alignment: "left" | "right" | "center") {
  // ....
}
printText("Hello, world", "left");
printText("G'day, mate", "centre"); // Argument of type "center" is not assignable to parameter of type "left" | "right" | "center";
```

- 숫자 리터럴 타입 또한 같은 방식으로 사용할 수 있다.

```js
function compare(a: string, b: string): -1 | 0 | 1 {
  return a === b ? 0 : a > b ? 1 : -1;
}
```

- 리터럴이 아닌 타입과 함께 사용이 가능하다.

```js
interface Options {
  width: number;
}
function configure(x: Options | "auto") {
  // ...
}
configure({ width: 100 });
configure("auto");
configure("automatic");
// Argument of type "automatic" is not assignable to parameter of type "Options | 'auto' "
```

## 리터럴 추론

- 객체를 사용하여 변수를 초기화하면 TS는 해당 객체의 프로퍼티는 이후에 그 값이 변화할 수 있다고 가정한다.

```js
const obj = { counter: 0 };
if (someCondition) {
  obj.counter = 1;
}
```

- 기존 값이 0이였던 필드에 1을 대입했을 때 TS는 이를 오류로 간주하지 안흔다.
  - 이를 달리 말하면 obj.counter는 반드시 number 타입을 가져야 하며, 0 리터럴 타입을 가질 수 없다는 의미이다.
    - 타입은 읽기 및 쓰기 두 동작을 결정하는데 사용되기 때문
    - 이와 같은 내용이 문자열에도 적용된다.

```js
const req = { url: "https://example.com", method: "GET" };
handleRequest(req.url, req.method);
// Argument of type "string" is not assignable to parameter of type "GET" | "POST".
```

- 위의 코드에선 req.method는 string으로 추론되며, "GET"으로 추론되지 않는다.
  - req의 생성 시점과 handleRequest의 호출시점 사이에도 얼마든지 코드 평가가 발생할 수 있고, 이때 req.method에 "GUESS"와 같은 새로운 문자열이 대입될 수 있으므로, TS는 위 코드에 오류가 있다고 판단한다.
- 해결법 1
  - 둘 중 한 위치에 타입단언을 추가하여 추론 방식을 변경한다.

```js
// 수정 1
const req = { url: "https://example.com", method: "GET" as "GET" };
// 수정 2
handleRequest(req.url, req.method as "GET");
```

- 수정 1은 req.method가 항상 리터럴 타입 "GET"이기를 의도하며, 이에 따라 "GUESS"와 같은 값이 대입되는 경우를 미연에 방지한다는 의미이다.
- 수정 2는 어떤 이유인지 req.method가 "GET"을 값으로 가진다는 사실을 알고 있다라는 것을 의미한다.

- 해결법 2
  - as const를 사용하여 객체 전체를 리터럴 타입으로 변경할 수 있다.

```js
const req = { url: "https://example.com", method: "GET" } as const;
handleRequest(req.url, req.method);
```

- as const 접미사는 일반적으로 const와 유사하게 동작하는데, 해당 객체의 모든 프로퍼티에 string 또는 number와 같ㅇ느 보다 일반적인 타입이 아닌 리터럴 타입의 값이 대입되도록 보장한다.

## null과 undefined

- JS에선 null과 undefined를 이용하여 빈 값 또는 초기화되지 않은 값을 가리킨다.
- TS에선 각 값에 대응하는 동일한 이름의 두 가지 타입이 존재한다.
  - 각 타입의 동작 방식은 strictNullChecks 옵션의 설정 여부에 따라 달라진다.

### strictNullChecks가 설정되지 않았을 때

- strictNullChecks가 설정되지 않았다면, 어떤 값이 null 또는 undefined일 수 있더라도 해당 값에 접근할 수 있으며, null과 undefined는 모든 타입의 변수에 대입될 수 있다.
  - 이는 Null 검사를 하지 않는 언어의 동작 방식과 유사하다.
    - C#, Java 등
  - Null 검사의 부재는 버그의 주요 원인이 된다.
    - 특별한 이유가 없다면, 코드 전반에 걸쳐 strictNullChecks 옵션을 설정하는 것을 권장한다.

### strictNullChecks가 설정되었을 때

- strictNullChecks가 설정되었다면, 어떤 값이 null 또는 undefined일 때, 해당 값과 함께 메서드 또는 프로퍼티를 사용하기에 앞서 해당 값을 테스트해야 한다.
  - 옵셔널 프로퍼티를 사용하기에 앞서 undefined 여부를 검사하는 것과 마찬가지로, 좁히기를 통하여 null일 수 있는 값에 대한 검사를 수행할 수 있다.

```js
function doSomething(x: string | undefined) {
  if (x === undefined) {
    // 아무것도 안함
  } else {
    console.log(`Hello, ${x.toUpperCase()}`);
  }
}
```

### Null 아님 단언 연산자

- TS에서는 명시적인 검사를 하지 않고도 타입에서 null과 undefined를 제거할 수 있는 특별한 구문을 제공한다.
  - 접미사 !로 사용한다.
- 표현식 뒤에 !를 작성하면 해당 값이 null 또는 undefined가 아니라고 타입을 단언한다.

```js
function liveDangerously(x?: number | undefined){
  // 오류없음
  console.log(x!.toFixed()); // x의 값이 null 또는 undefined가 아니라고 단언
}
```

## 열거형

- 열거형은 TS가 JS에 추가하는 기능이다.
- 어떤 값이 이름이 있는 상수 집합에 속한 값 중 하나일 수 있도록 제한하는 기능이다.
- 언어와 런타임 수준에 추가되는 기능이다.
  - 즉, 사용법을 모르면 사용하지 않는것이 좋다.
