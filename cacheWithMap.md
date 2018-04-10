```javascript

let map = new Map();
const  factorial = (n) => (n===1) ? 1 : ((map.has(n)? map.get(n): map.set(n,n*factorial(n-1)), map.get(n) ));
// console.log(factorial(6));

const memorized = (fn, n) => {
  let map = new Map();
  if(!map.has(n))  map.set(n,fn(n));
  return map.get(n);
}

const fa = (n) => (n<2)?1:n* fa(n-1);
const fibo= (n) =>{
  if (n<3) return 1;
  else return fibo(n-1)+fibo(n-2);
}

console.log(memorized(fa,6));
console.log(memorized(fibo,7));

```
