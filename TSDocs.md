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
