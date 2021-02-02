---
theme: gaia
_class: lead
paginate: false
size: 30px
color: #fff
backgroundColor: #101d27
marp: true;
---
<style>
    h2 {
        font-size: 45px;
    }
</style>
<style scoped>
    h1 {
        font-size: 100px
    }
</style>

# Enumerated

---
## 목차
- 열거형
- 개선안
- 객체
- 추상화
- 시연
- 결론
---
<style scoped>
    section {
        text-align: center
    }
    h2 {
        margin-top: 21%;
        font-size: 100px
    } 
</style>
## 열거형
---
## 열거형 (Enumerated type)
`열거하다: 어떤 목록들을 순서에 상관없이 늘어놓다.`

```markdown
# 열거형이란 무엇일까.

# 열거형은 왜 만들어졌을까. 

# 열거형은 어떻게 사용하면 좋을까.
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 초기의 상태 값
복잡한 여러가지 상태들을 단순한 값으로 치환하여 사용할 수 있었다.
```typescript
/**
 *  0: 사과
 *  1: 오렌지
 *  2: 바나나
 */
function f (type: number) {
  switch (type) {
    case 0: 
      // code apple.
      break
    case 1: 
      // code orange.
      break
    case 2:
      // code banana
      break
  }
}
f(0)
```
```markdown
# 코드와 주석이 동시에 유지보수 될 가능성은 낮다. 
# 주석이 변질되거나 유실 될 경우 값에 대한 명확한 의도를 알기 어렵다.
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 상수를 사용
상수를 사용하여 의미를 부여하여 주석을 사용하지 않아도 맥락을 파악할 수 있게 되었다.
```typescript
const APPLE = 0
const ORANGE = 1
const BANANA = 2
function f (type: number) {
  switch (type) {
    case APPLE:
      // code apple.
      break
    case ORANGE: 
      // code orange.
      break
    case BANANA:
      // code banana
      break
  }
}
f(APPLE)
```
```markdown
# namespace의 충돌을 해결할 수 없다.
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 네임스페이스의 충돌
같은 namespace에 같은 값과 다른 의미를 가진 상태가 존재할 수 있다. 
```typescript
const FRUITE_APPLE = 0 // 과일
const FRUITE_ORANGE = 1
const FRUITE_BANANA = 2

const COMPANY_APPLE = 0 // 회사
const COMPANY_GOOGLE = 1
const COMPANY_FACEBOOK = 3

f(FRUITE_APPLE)  // do apple code.
f(COMPANY_APPLE) // do apple code.
```
```markdown
# 컴파일러는 이를 오류라고 판단하지 않는다.
# 이를 해결하기 위해 과도한 prefix가 붙게 된다.
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 타입의 불안정성
```typescript
const FRUITE_APPLE = 0
const COMPANY_APPLE = 0

FRUITE_APPLE === COMPANY_APPLE // true
```
```markdown
# 값은 메모리에 적재된 bit를 직접 비교하기 때문에, 값을 사용하는 프로그램은 깨지기 쉽다.
# 정수 상수들은 단순히 값이기 때문에 디버깅하기 어렵고, 이를 문자열로 출력하기 까다롭다.
```

```typescript
const APPLE = 'APPLE'
const ORANGE = 'ORANGE' 
const BANANA = 'BANANA'
f('APPLEE')
```
```markdown
# 출력하여 값을 확인하는 부분은 개선되어 좋으나, 사용하는 쪽의 코드에 문자열을 하드코딩하게 만든다.
# 하드코딩 된 문자열은 컴파일 시점에 알 수 없다. 
# 이로인해 반드시 런타임 에러를 발생시키며 발견하기 무척 어렵다.
# 문자열 비교 연산은 대체로 비싸다.
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 열거형 (Enumerated type)
개별적인 namespace를 갖게 하여 추상적인 대상을 명확하게 표현한다.
```typescript
enum Fruite {
  APPLE,
  ORANGE,
  BANANA
}
enum Company {
  APPLE = 'APPLE',
  GOOGLE = 'GOOGLE',
  FACEBOOK = 'FACEBOOK'
}
console.log(Fruite[0] === 'APPLE') // true
console.log(Fruite['APPLE'] === 0) // true
console.log(Fruite.APPLE === 0) // true

console.log(Company.APPLE === 'APPLE') // true
```
```markdown
# 타입스크립트의 enum은 객체이고 기본적으로 정수값과 문자열을 동시에 갖는다.
# 선택적으로 문자열만을 할당할 수 있지만 여전히 앞서 말했던 값을 상수로 사용하는 것의 문제점은 발생한다.
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 유지보수(Maintenance)의 어려움
열거형을 그대로 사용하면 값을 사용하여 상태를 판단하기 때문에 수정의 여파가 어플리케이션 전체가 될 수 있다.
```typescript
type fruite = string;

// A.tsx
import Fruite from './fruite'
if (fruite === Fruite.APPLE) {/* do apple.. */}
else if (fruite === Fruite.ORANGE) {/* do orange.. */}
else {/* do banana */}

// B.tsx
import Fruite from './fruite'
if (fruite === Fruite.APPLE) {/* do apple.. */}
else if (fruite === Fruite.ORANGE) {/* do orange.. */}
else {/* do banana */}

// There will be able about lots of the same modules below.
// ...
```
```markdown
# 망고라는 상태가 추가 된다면?
# 애플망고라는 종이 추가되어, 기존의 사과와 망고가 둘다 애플 망고의 카테고리에 종속 된다면?
# 개발자가 수정하기 쉽지 않고 수정의 여파를 파악하기 쉽지 않음.
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 맥락(Context)을 파악하기 어려움
값을 비교하는 연산을 수행하기 때문에 코드의 의도를 읽고 파악하는 것이 쉽지 않다.
```typescript
if (fruite === Fruite.APPLE) {/* do apple.. */}
```
```markdown 
# fruite에 대한 정보와 Fruite.APPLE에 대한 정보를 전부 알아야 한다.
# fruite와 APPLE이 같다는 것이 무엇을 의미하는지를 파악해야 한다.
# 열거형이 특정 값(expresion)과 똑같다라는 코드의 맥락을 파악하기는 쉽지 않다.
```

조건이 중첩되면 더 까다로워 진다.
```typescript
if ((fruite === Fruite.APPLE && ripe === Ripe.ENOUGH && sweet === Sweet.ENOUGH)
  || (fruite === Fruite.MANGO && ripe === Ripe.NOT_ENOUGH && sweet !== Sweet.ENOUGH)) {
  /* do something.. */
}
```
```markdown
# 조건을 파악하는데 너무 많은 정보를 알아야 한다.
# 무엇을 위한 스펙인지 파악하기 어렵다.
# 만약 위에 사용 된 열거형 중 하나라도 기능이 추가되거나 사용 조건이 변경된다면 어떨까? 
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 높은 결합도(Coupling)와 낮은 응집도(Cohesion)
`enum을 값으로 사용했기 때문에 사용하는 모듈과의 높은 결합도, 낮은 응집도가 형성.`
<br/>

```text
                            Enum
           A.tsx            B.tsx             C.tsx         ...etc
    (A1.tsx) (A2.tsx) (B1.tsx) (B2.tsx) (C1.tsx) (C2.tsx)   ...etc 
    ...
```

```markdown
# 많은 의존도를 받고있는 열거형 모듈은 무거워져 수정하거나 변경하기 쉽지 않다.

# 열거형을 수정하는 순간 의존하고 있는 모듈 및 하위 모듈들은 영향을 받게 되므로 광범위한 테스트를 수행.

# 유지보수 비용이 증가하고 변화에 대응하기 어려움.
```
---
<style scoped>
    section {
        text-align: center
    }
    h2 {
        margin-top: 21%;
        font-size: 100px
    } 
</style>
## 개선안
---

<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 중복 제거 및 응집도 향상
`# enum과 함수를 같은 곳에 선언하여 중복을 제거하고 관련 코드를 한곳으로 모아 응집도를 높인다.`

```typescript
export const enum Fruite {
  APPLE = 'APPLE',
  ORANGE = 'ORANGE',
  BANANA = 'BANANA'
}
export const isApple = (fruite: keyof typeof Fruite) => fruite === Fruite.APPLE
export const isOrange = (fruite: keyof typeof Fruite) => fruite === Fruite.ORANGE
export const isBanana = (fruite: keyof typeof Fruite) => fruite === Fruite.BANANA
```
`# 사용하는 쪽에서는 직접 조건을 작성하지 않고 호출만 한다.`
```typescript
import {Frute, isApple, isOrange} from './fruite'
// A.tsx
import Fruite from './fruite'
if (isApple(fruite)) {/* do apple.. */}

// B.tsx
import Fruite from './fruite'
if (isApple(fruite)) {/* do apple.. */}
```

---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 유지보수성 향상
`# 타입이 추가 되어도 문제가 없다.`
```typescript
export const enum Fruite {
  HONEY_APPLE = 'HONEY_APPLE'
}
export const isHoneyApple = (fruite: keyof typeof Fruite) => fruite === Fruite.HONEY_APPLE
```
`# 사과를 판단하는 조건이 복잡하게 변경되더라도, isApple 함수의 내부의 조건만 변경할 수 있다.`
```typescript
export const isApple = (fruite: keyof typeof Fruite) => fruite === Fruite.APPLE || isHoneyApple(fruite)
```
`# 사용하는 쪽에서는 호출만 하기 때문에 수정의 여파가 없다.`
```typescript
import {Frute, isApple, isOrange} from './fruite'
// A.tsx
import Fruite from './fruite'
if (isApple(fruite)) {/* do apple.. */}

// B.tsx
import Fruite from './fruite'
if (isApple(fruite)) {/* do apple.. */}
```
---
<style scoped>
    section {
        text-align: center
    }
    h2 {
        margin-top: 21%;
        font-size: 100px
    } 
</style>
## 객체
---

<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 상태와 행위의 분리
`# 아래 존재하는 모든 함수는 Fruite를 위해 만들어진 행위들이다.`
```typescript
export const enum Fruite {
  APPLE = 'APPLE',
  ORANGE = 'ORANGE',
  BANANA = 'BANANA'
}
export const isApple = (fruite: keyof typeof Fruite) => fruite === Fruite.APPLE
export const isOrange = (fruite: keyof typeof Fruite) => fruite === Fruite.ORANGE
export const isBanana = (fruite: keyof typeof Fruite) => fruite === Fruite.BANANA
```
```markdown
# 얼핏 보면 같은 모듈에 작성되어 같은 맥락인 것처럼 보인다.

# 하지만 enum Fruite와 아래의 함수들은 코드상으로는 연결점이 없다.

# 그렇기 때문에 어떤 상태 값에 종속적인 행위를 표현하기엔 부족하고, 쉽게 다른 조건들이 추가되거나 함수자체가 다른 곳으로 분리되어진다.
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 상태와 행위를 하나로
```text
# Javascript는 ES6 이후로 권한과 책임을 하나로 표현하기에 충분한 class 문법을 제공한다.

# public 접근 제한자를 사용하여 접근 권한을 명시한다.

# static 필드를 사용하면 전역 변수를 선언할 수 있다.

# readonly 속성을 사용하여 내부 필드의 불변성을 유지한다.

# 자바스크립트의 객체는 싱글톤 이므로 타입에 안전하다. 
```
```typescript
class Fruite {
  public static APPLE = new Fruite()
  public static ORANGE = new Fruite()
  public static BANANA = new Fruite()
}
Fruite.APPLE === 'APPLE'        // false
Fruite.APPLE === 0              // false
Fruite.APPLE === Fruite.ORANGE  // false
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 불변성
```text
# readonly 속성을 사용하여 내부 필드의 불변성을 유지한다.

# 생성자의 접근권한을 private으로 막아 외부에서 변경할 가능성을 제거하여 불변성을 유지한다. 
```

```typescript
class Fruite {
  public static readonly APPLE = new Fruite()
  public static readonly ORANGE = new Fruite()
  public static readonly BANANA = new Fruite()
  
  private constructor () {}
}

class Child extends Fruite {} // not allowed
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 권한 부여
`# 값으로 사용할 상수를 선언한다.`
```typescript
const enum Kind {
  APPLE = 'APPLE',
  ORANGE = 'ORANGE',
  BANANA = 'BANANA'
}
```
```
# 객체에 필요한 상태를 할당하고, 상태를 관리할 권한을 부여한다.

# 상태 fruite를 추가한다.
```
```typescript
class Fruite {
  public static readonly APPLE = new Fruite(Kind.APPLE)
  public static readonly ORANGE = new Fruite(Kind.ORANGE)
  public static readonly BANANA = new Fruite(Kind.BANANA)

  private constructor (private _fruite: Kind) {}
}
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 책임 할당
`# 객체가 수행해야할 책임을 추가한다.`
```typescript
class Fruite {
  // ...
  public isApple () {
    return this.fruite === Kind.APPLE
  }

  public isOrange () {
    return this.fruite === Kind.ORANGE
  }

  public isBanana () {
    return this.fruite === Kind.BANANA
  }

  private get fruite () {
    return this._fruite
  }
}
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 코드로 표현된 권한과 책임
`# 값과 함수가 아니라, 상태와 행위이기 때문에 코드는 분리되지 않고 연관성을 갖는다.`
```typescript
export default class Fruite {
  public static readonly APPLE = new Fruite(Kind.APPLE)
  public static readonly ORANGE = new Fruite(Kind.ORANGE)
  public static readonly BANANA = new Fruite(Kind.BANANA)

  private constructor (private _fruite: Kind) {}

  public isApple () {
    return this.fruite === Kind.APPLE
  }

  public isOrange () {
    return this.fruite === Kind.ORANGE
  }

  public isBanana () {
    return this.fruite === Kind.BANANA
  }

  private get fruite () {
    return this._fruite
  }
}
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 상수의 사용
```typescript
import Fruite from './fruite'

const example1 = (fruite: Fruite) => {
  if (fruite.isApple()) {} 
  else {}
}
const example2 = (fruite: Fruite) => {
  switch (fruite) {
    case Fruite.APPLE:
      break
    case Fruite.ORANGE:
      break
    case Fruite.BANANA:
      break
  }
}
const example3 = (fruite: Fruite) => {
  switch (true) {
    case fruite.isApple():
      break
    case fruite.isOrange():
      break
    case fruite.isBanana():
      break
  }
}
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 손쉬운 변경 - 1
```text
# 여러가지 조건이 추가 되는 경우에도 손쉬운 변경이 가능하다.

# 껍질이라는 조건을 추가해보자.
```
```typescript
const enum Peel {
  EXISTED = 'EXISTED',
  NOT_EXISTED = 'NOT_EXISTED'
}
```
`# 상태를 추가하고, 권한을 부여한다.`
```typescript
export default class Fruite {
  public static readonly APPLE = new Fruite(Kind.APPLE, Peel.EXISTED)
  public static readonly ORANGE = new Fruite(Kind.ORANGE, Peel.EXISTED)
  public static readonly BANANA = new Fruite(Kind.BANANA, Peel.EXISTED)

  private constructor (private _fruite: Kind, private _peel: Peel) {}
}
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 손쉬운 변경 - 2
`# 책임을 할당한다.`
```typescript
export default class Fruite {
  // ...
  public isNeededKnife () {
    return this.peel === Peel.EXISTED
  }
  
  private get peel () {
    return this._peel
  }
}
```
`# 사과를 식별하는 조건에 껍질 여부가 추가 된다 하더라도, 수정의 여파는 Fruite에 그친다.`
```typescript
export default class Fruite {
  // ...
  public isApple () {
    return this.fruite === Kind.APPLE && this.isNeededKnife()
  }
}
```
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 상태 저장소에 저장
```text
# API에서 전달된 문자열로 상수 객체를 찾아 상태 저장소에 저장한다.

# 값을 반환하는 values를 구현 한다.
```
```typescript
class Fruite {
  // ...
  public static values () {
    return Object.values(this)
  }

  public static from (fruite: string) {
    return this.values().find(t => t.fruite === fruite)
  }
}
```
`# 직렬화를 위해 toJSON을 구현한다.`
```typescript
class Fruite {
  // ...
  public toJSON () {
    return {
      fruite: this.fruite
    }
  }
}
```
---
<style scoped>
    section {
        text-align: center
    }
    h2 {
        margin-top: 21%;
        font-size: 100px
    } 
</style>
## 추상화
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 공통된 로직을 추상화
```text
# 추상 클래스를 선언하고, 공통된 로직을 추상화 한다.

# 모든 구상객체가 구현해야 하는 직렬화 로직은 추상 메소드로 분리하여 DIP를 달성한다.
```
```typescript
abstract class Enum {
  public static values () {
    return Object.values(this)
  }

  public abstract toJSON(): object;
}
```
---
<style scoped>
    section {
        text-align: center
    }
    h2 {
        margin-top: 21%;
        font-size: 100px
    } 
</style>
## 시연
---
<style scoped>
    section {
        text-align: center
    }
    h2 {
        margin-top: 21%;
        font-size: 100px
    } 
</style>
## 결론
---
<style scoped>
    p {
        font-size: 20px;
    }
    code {
        font-size: 15px;
    }
    pre {
        margin-top: 15px;
    }
</style>
## 결론
```text
# 열거형이란 표현하고 싶은 상태를 네임스페이스별로 추상화하는 도구이다.

# 복잡한 상태를 단순한 값으로 표현하기 위해 등장했다.

# 타입스크립트에서는 값의 형태로 사용한다.

# 중복되는 값의 비교 로직은 최소한 함수로 분리한다.

# 복합적인 상태를 갖는다면 객체를 상수로 만들어 사용하는 것이 좋다.

# 낮은 결합도와, 높은 응집도를 추구하여 변화에 쉽게 대응할 수 있도록 한다.
```
---
<style scoped>
    section {
        text-align: center
    }
    h2 {
        margin-top: 21%;
        font-size: 100px
    } 
</style>
## 끗
---