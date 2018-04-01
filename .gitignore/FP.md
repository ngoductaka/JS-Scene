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

