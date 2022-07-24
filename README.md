* 타입스크립트 강의 연습장

```ts
// 타입 명시

type Name = string;
type Age = number;

type Player = {
    name : Name,
    age? : Age
}

const nico : Player = {
    name: "nico"
}

const lynn : Player = {
    name: "lynn",
    age : 12
}

function playerMaker(name:string) : Player {
    return {
        name
    }
}

const playerMaker = (name:string) : Player => ({name})

//[case] readonly 속성

type Player = {
    readonly name : Name,
    age? : Age
}

const numbers: readonly number[] = [1,2,3,4]

numbers.push(1) //readonly 속성으로 인한 error

//[case] Tuple - 최소한의 길이를 가지고 특정 위치에 특정 타입이어야 함

const player: [string, number, boolean] = ["nico", 1, true]

// readonly까지 합친 경우

const player: readonly [string, number, boolean] = ["nico", 1, true]

//[case] any 는 타입스크립트를 벗어나게 해줌

const a : any[] = [1,2,3,4]
const b : any = true

a + b // error 안뜸

//[case] unknown - 변수의 타입을 미리 알지 못 할 때 사용
let a : unknown;

if(typeof a === 'number') {
    let b = a + 1
}

if(typeof a === 'string') {
    let b = a.toUpperCase();
}

//[case] void - 아무것도 return 하지 않는 함수를 대상으로 사용


//[case] never - 함수가 절대 return하지 않을 때 발생

function hello(name: string|number) {
    if(typeof name === "string") {
        name //type string
    } else if (typeof name === "number") {
        name //type number
    } else {
        name //type never
    }
}

```

```ts
// function

function add(a : number,b : number /* 명시 안해주면 타입스크립트에선 오류 */) {
    return a + b
}

// call Signatures 사용한 경우
type Add = (a: number, b: number) => number;

const add: Add = (a,b) => a + b

// overloading 예시

type Config = {
    path: string,
    state: object
}

type Push = {
    (path: string): void
    (config: Config): void
}

const push: Push = (config) => {
    if (typeof config === "string") { console.log(config)} else {
        console.log(config.path)
    }
}

type Add = {
    (a: number, b: number) : number
    (a: number, b: number, c: number) : number 
}

const add: Add = (a, b, c?:number) => {
    if(c) return a + b + c
    return a + b
}


// Polymorphism 예시

type SuperPrint = {
    (arr: number[]):void
    (arr: boolean[]):void
    // generic 예시- 어떤 타입이 들어올지 애매할때
    <TypePlaceholder>(arr: TypePlaceholder[]):TypePlaceholder
}

const superPrint: SuperPrint = (arr) => {
    arr.forEach(i => console.log(i)) 
}

superPrint([1,2,3,4])
superPrint([true, false, true])
superPrint(["a", "b", "c"]) /* error */

```