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
