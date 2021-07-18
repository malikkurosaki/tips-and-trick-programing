# create multiple array in simple way

```js
console.log(Array.from(Array(10), (x, i) => {
   return {
       "nama": "malik"+ (i + 1)
   }
}));
```

