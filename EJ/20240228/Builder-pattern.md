## Builder pattern 이란

`const office = new Office(‘Banpodero’, 4, true, true);`

만약, 해당 Office 클래스에서 일부 속성을 추가해 클래스를 확장하는 작업이 있다면, 어떤 문제가 있는가?

Builder is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code. reference: https://refactoring.guru/design-patterns/builder

즉, Builder Pattern은 빌더 패턴이란 객체의 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에 서로 다른 표현 결과를 만들 수 있게 하는 패턴이다.

아래 코드를 살펴보자.

빌더 패턴의 정의는 복잡한 객체의 구성과 표현을 분리하는 것이다. Builder 클래스는 단계별로 최종 객체를 구축한다. 이런 방식으로 Builder는 다른 개체와 독립적이다.

`const office = new Office(‘Banpodero’, 4, true, true);` 코드에 Builder pattern을 적용하면 아래와 같이 작성 가능하다.

`const office = new Office('Banpodero').setFloor(4).makeParkinglot().makePantry().build();`

## Builder pattern의 장점

— 생성자의 매개변수가 훨씬 더 읽기 쉬운 방식으로 축소되고 제공되므로 선택적 매개변수에 대해 생성자에 null을 전달할 필요가 없다.

## Office.ts

앞서 언급했듯이 Office 클래스에는 다음 속성이 포함되어 있다.
- address(string)
- FloorNumber(number)
- isHavingParkinglot(boolean)
- isHavingPantry(boolean)

```.ts
import { OfficeBuilder } from './OfficeBuilder';

export class Office {
    address: string;
    floorNumber: number;
    isHavingParkinglot: boolean;
    isHavingPantry: boolean;
    constructor(officeBuilder: OfficeBuilder) {
        this.address = officeBuilder.address;
        this.floorNumber = officeBuilder.floorNumber;
        this.isHavingParkinglot = officeBuilder.isHavingParkinglot;
        this.isHavingPantry = officeBuilder.isHavingPantry;
    }
}
```

## OfficeBuilder.ts
```.ts
import { Office } from './Office';

export class OfficeBuilder {

    private readonly _address: string;
    private _floorNumber: number = 0;
    private _isHavingParkinglot: boolean = false;
    private _isHavingPantry: boolean = false;

    constructor(address: string) {
        this._address = address;
    }

    setFloor(floor: number) {
        this._floorNumber = floor;
        return this;
    }

    makeParkinglot() {
        this._isHavingParkinglot = true;
        return this;
    }

    makePantry() {
        this._isHavingPantry = true;
        return this;
    }

    build() {
        return new House(this);
    }

    get isHavingParkinglot() {
        return this._isHavingParkinglot;
    }

    get isHavingPantry() {
        return this._isHavingPantry;
    }

    get address() {
        return this._address;
    }

    get floorNumber() {
        return this._floorNumber;
    }
}
```

## OfficeBuilder 사용하기
```.ts
import { OfficeBuilder } from './OfficeBuilder';
const office = new OfficeBuilder('some-address').setFloor(3).makeParkinglot().makePantry().build();
```
