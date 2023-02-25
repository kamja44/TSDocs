# 타입 추론(Types by Inference)

- TS는 js 언어를 알고 있으며, 대부분의 경우 타입을 생성해준다.

# 타입 정의하기 (Defining Types)

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

타입 구성

유니언(Unions)

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

제네릭 (Generics)

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

구조적 타입 시스템(Structural Type System)

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
