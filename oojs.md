<h1>DBC Phase 2 Reference Sheet:<br/>
Object-Oriented JavaScript</h1>

<h4>Prepared by Jeff George for future DBC students<br/>
Based on the Phase 2 Helper Sheet compiled by Ryan Lesson</h4>

### Define a Class
```JS
var Panda = function(args) {
  this.num_paws = args.num_paws || 4;
  this.bamboos = [];
  this.cubs = [];
};
```

### Create a Class Method on a JS Class
_Note: this creates a separate copy of the method on every instance of the class. This takes a lot of space/memory, and means that the method on a specific individual will be limited to that individual, not the whole class._
```JS
Panda.all = function() {
  // do Javascript stuff to return a collection of all the pandas
};
```

### Create an Instance Method on a JS Class
_Note: this applies the same function to all instances of the class. This means that only a single copy of the method is saved, and changes to that method affect all instances of the class._
```JS
Panda.prototype.add_cub = function(cub) {
  this.cubs.push(cub);
};
```

### Methods that are useful with JavaScript Classes
_NOTE: These examples are not complete or exhaustive coverage of the functions;
they simply show applications which are applicable in common circumstances._
#### Array.prototype.map()
```JS
arr = [1,2,3,4];
arr.map( function(current_val, index) {
  return current_val * 2;
});
<- [2, 4, 6, 8]
```

#### Array.prototype.reduce()
```JS
arr = [1,2,3,4];
arr.reduce( function(total, current_val) {
  return total += current_val;
});
<- 10

arr.reduce( function(total, current_val) {
  return total += current_val;
}, 10);  // Value after the function (10, here) is the optional initial value
<- 20
```

#### Array.prototype.find()
```JS
arr = ["apple","banana","canteloupe"];
arr.find( function(current_val, index) {
  return current_val.slice(0,1) === "b";
});
<- "banana"
```

#### Array.prototype.filter()
```JS
arr = [1,2,3,4,5,6,7,8,9,10];
arr.filter( function(current_val, index) {
  return current_val % 3 === 0;
});
<- [3, 6, 9]
```
