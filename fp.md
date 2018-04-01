# Composition 
composition là gì ?
## "  The act of combining parts or elements to form a whole"

cssence (tinh hoa):  bẻ chương trình lớn, phức tạp thành các chương trình con 
đơn giản và ghép chúng lại 
1. sử dụng 1 function làm đầu vào cho chương trình khác 
* trong toàn học đó là (f*g)(x) = f(g(x));
trong js :
sử dụng kết quả 1 hàm làm param hàm khác
```javascript
const f=n=>n+1;
const g=n=>n*2;

const doStuffBetter=x=>f(g(x));
doStuffBetter(20);// return 40
```
hoặc 
```javascript
const g = n => n + 1;
const f = n => n * 2;
const wait = time => new Promise(
  (resolve, reject) => setTimeout(
    resolve,
    time
  )
);
wait(300)
  .then(() => 20)
  .then(g)
  .then(f)
  .then(value => console.log(value)) // 42
```
## => "if you chaning, you'are composition"
thủ thuật lodash 
```javascipt 
import pipe from 'lodash/fp/flow';
const doStuffBetter = pipe(
  g,
  trace('after g'),
  f,
  trace('after f')
);
doStuffBetter(20); // =>
/*
"after g: 21"
"after f: 42"
*/
```
hoặc 
```javascipt
// pipe(...fns: [...Function]) => x => y
const pipe = (...fns) => x => fns.reduce((y, f) => f(y), x);
```
# vấn đề của hướng đối tượng 
* **The tight coupling problem:** Because child classes are dependent on the implementation of the parent class, class inheritance is the tightest coupling available in object oriented design.
* **The fragile base class problem:** Due to tight coupling, changes to the base class can potentially break a large number of descendant classes — potentially in code managed by third parties. The author could break code they’re not aware of.
* **The inflexible hierarchy problem:** With single ancestor taxonomies, given enough time and evolution, all class taxonomies are eventually wrong for new use-cases.
* **The duplication by necessity problem:** Due to inflexible hierarchies, new use cases are often implemented by duplication, rather than extension, leading to similar classes which are unexpectedly divergent. Once duplication sets in, it’s not obvious which class new classes should descend from, or why.
* **The gorilla/banana problem**: “…the problem with object-oriented languages is they’ve got all this implicit environment that they carry around with them. You wanted a banana but what you got was a gorilla holding the banana and the entire jungle.” ~ Joe Armstrong, “Coders at Work”

` giải pháp cho thừa kế trong hướng đối tượng không dùng this / class`
```javascipt
const a = {
  a: 'a'
};
const b = {
  b: 'b'
};
const c = {...a, ...b}; // {a: 'a', b: 'b'}
```
`hoặc dùng closesure thay cho this`
```javascript
const Person =  ()=> {
    let firstName, lastName;
    return {
        getName: () =>{
            console.log(firstName, lastName);
            return firstName,lastName;
        },
        setName:(first,last)=>{
            firstName = first;
            lastName= last;
        }
    };
};

const dnd = Person();
dnd.setName('ngoc','duc');
dnd.getName();
const dnd1 = Object.create(dnd);
dnd1.eat = (food)=>`eat ${food}`;
const eat = dnd1.eat('com');
console.log(eat);

```
# phần 2
## sử dụng var, let, const :
sử dụng theo thứ tự 
1. lựa chọn const: biến không thể gán lại ! chỉ gám 1 lần khi khai báo => hiểu đúng
2. khi biến cần thay đổi như biến đếm => dùng let 
## type 
```javascipt 
const b = 'b';
const oB = { b };
// khi trùng key nó sẽ ghi đè
const c = {...oA, ...oB};
```
## Destructuring
có thể trích xuất (extract values) và gán vào biến:
```javascipt
const [t, u] = ['a', 'b'];
t; // 'a'
u; // 'b'
const blep = {
  blop: 'blop'
};

// The following is equivalent to:
// const blop = blep.blop;
const { blop } = blep;
blop; // 'blop'
```
## function 
trong toán học f(x)=2x <br>
f của x là 2x =>> f của 2 là 4 <br>
## Arguments
```javascript
const createUser = (
    {name = 'Anonymous',avatarThumbnail = '/avatars/anonymous.png'},
    [a='a1',b,c='a3'],
    n='n'
) => ({
    name,
    avatarThumbnail,
    a,b,c,n
});

const george = createUser(
    { name: 'George', avatarThumbnail: 'avatars/shades-emoji.png'},
    [,'ar2',undefined],
    undefined
);
console.log(george);
```
# Rest and Spread
1. ***Rest***
tập hợp các thành phần còn lịa của arguments trong function lại thành 1 array 
2. ***Spread***
làm điều ngược lại là chia (phân tách )các cá thể trong 1 array thành các phần tử)
```javascipt
const shiftToLast = (head, ...tail) => [...tail, head];
shiftToLast(1, 2, 3); // [2, 3, 1]
```
* trong array có iterator khi dung toán tử `spread` nó copy từng giá trị theo thứ tự đến hết vào array mới được khởi tạo

# Currying
là function nhận vào nhiều param và xư lý cho đến hết <br> ví dụ rất hay :
```javascript
const highpass = cutoff => n => n >= cutoff;
const gt4 = highpass(4); // highpass() returns a new function
```
hoặc 
```javascript
import curry from 'lodash/curry';
const add3 = curry((a, b, c) => a + b + c);
add3(1, 2, 3); // 6
add3(1, 2)(3); // 6
add3(1)(2, 3); // 6
add3(1)(2)(3); // 6
```
# array function
```javascript
[1,2,3].map(e=>e*2);
```
array được coi như 1 object map là 1 method khi được gọi nó nhận function làm pram và trã về 1 array mới là kết quả của function trã về 
<br>
e là giá của từng phần tử trong array . dùng `this` để trỏ vào đối tượng mang nó đang gọi 
# chaining function 
```javascript
[2, 4, 6].filter(gt4).map(double);// [8, 12]
[1,2,3].map(e=>2*e).filter(e=>e>=4);
```
# Higher Order Functions `
`'A higher order function is a function that takes a function as an argument, or returns a function'`

```javascript
const censor = words => {
  const filtered = [];
  for (let i = 0, { length } = words; i < length; i++) {
    const word = words[i];
    if (word.length !== 4) filtered.push(word);
  }
  return filtered;
};
censor(['oops', 'gasp', 'shout', 'sun']);
// [ 'shout', 'sun' ]
```
=> lặp đi lặp lại nhiều => nếu cần lọc các ký tự có bắt đầu  bằng 's' thì viết tương tự => giải pháp thông minhg hơn
``` javascript
const reduce = (reducer, initial, arr) => {
  // shared stuff
  let acc = initial;
  for (let i = 0, { length } = arr; i < length; i++) {
    // unique stuff in reducer() call
    acc = reducer(acc, arr[i]);
  // more shared stuff
  }
  return acc;
};
reduce((acc, curr) => acc + curr, 0, [1,2,3]); // 6
```
### and the best 
```javascript
//reduce: (acc, curr) => fn(curr) ?acc.concat([curr]) :acc
//initial: []
//arr: ['oops', 'gasp', 'shout', 'sun'];
const reduce = (reducer, initial, arr) => {
    let acc = initial;//[]
    for (let i = 0, { length } = arr; i < length; i++)   acc = reducer(acc, arr[i]);
    //reducer: (acc, curr) => fn(curr) ?acc.concat([curr]) :acc // acc = []// arr = ['oops', 'gasp', 'shout', 'sun'];
    // reducer: ([], 'oops') => fn(curr) ?acc.concat([curr]) :acc//fn =word => word.length !== 4(false) ->acc
    // word => word.length !== 4
    return acc;
};
// console.log(reduce((acc, curr) => acc + curr, 0, [1,2,3]) ); // 6
// fn: word => word.length !== 4
// words: ['oops', 'gasp', 'shout', 'sun'];
const filter = (fn, arr) => reduce( (acc, curr) => fn(curr) ?acc.concat([curr]) :acc,  [], arr);
const _filter = (fn, arr)=>{
    let result=[];
    for(let i=0, {length}=arr; i<length; i++) if(fn(arr[i]))   result.push(arr[i]);
    return result;
}
const arrInput = ['oops', 'gasp', 'shout', 'sun'];
const censor = words => _filter(word => word.length !== 4, words);
console.log(censor(arrInput));

```
# Phần 3 : Reduce (Composing Software)
```javascript
const summingReducer = (acc, n) => acc + n;
[2, 4, 6].reduce(summingReducer, 0); // 12
// acc : accumulated  nó bắt đầu từ giá trị đầu vào mặc định ( trường hợp này là 0, nếu ko có thì lấy ar[0]) sau đó lấy ra trị return 
// của function 
// n là giá trị tiếp theo của mảng 
```
***map***
```javascript
const map = (fn, arr) => arr.reduce((acc, item, index, arr) => {
  return acc.concat(fn(item, index, arr));
}, []);
```
***Filter***
```javascript
const map = (fn, arr) => arr.reduce((acc, item, index, arr) => {
  return fn(item, index, arr)?acc.concat(arr[index]):acc;
}, []);
```
***and more***
```
const compose = (...func)=>x=>func.reduceRight((v, f) => f(v), x);
const add1 = n => n + 1;
const double = n => n * 2;
const add1ThenDouble = compose(double, add1);
console.log(add1ThenDouble(2)); // 6
// ((2 + 1 = 3) * 2 = 6)
```
*** hà nội tower **
```javascript
const compose = (...func)=>x=>func.reduceRight((v, f) => f(v), x);
const add1 = n => n + 1;
const double = n => n * 2;
const add1ThenDouble=compose(add1, double);
let count = 0;
const a= (x)=>{
    console.log(x);
    if (count++>1) return x;
    else return a(add1ThenDouble(x));//x=1->count=0->a(3)//x=3->count=1->a(7)//x= 7->count=2->return 7
}
console.log('...',a(1)); 
```
# A Word on Redux - react
* nguyên lý hoạt động của redux là dùng reduce để quản lý state : nó nhận vaò state hiện tại và 1 action và trã về state mới : <br>
`reducer(state: Any, action: { type: String, payload: Any}) => newState: Any`<br>
* rude reduce in dedux 
1. A reducer called with no parameters should return its valid initial state.
2. If the reducer isn’t going to handle the action type, it still needs to return the state.
3. Redux reducers must be pure functions.
* ví dụ 
```javascript
const ADD_VALUE = 'ADD_VALUE';
const summingReducer = (state = 0, action = {}) => {
  const { type, payload } = action;
  switch (type) {
    case ADD_VALUE:
      return state + payload.value;
    default: return state;
  }
};

const actions = [
  { type: 'ADD_VALUE', payload: { value: 1 } },
  { type: 'ADD_VALUE', payload: { value: 1 } },
  { type: 'ADD_VALUE', payload: { value: 1 } },
];
actions.reduce(summingReducer, 0); // 3
```
## chốt : reduce là công cụ tuyệt vời cho fp !
















