- Union Types and Type narrowing
```
function kgToLbs(weight: number | string): number{
  // Type Narrowing
  if(typeof weight == 'number') return weight * 2.2;
  else return parseInt(weight) * 2.2;
}

console.log( kgToLbs(10));
console.log(kgToLbs('10Kg'));
```
- Type Intersection
```
type Draggable = {
  drag: () => void;
}

type Resizable = {
  resize: () => void;
}

type UiWidget = Draggable & Resizable;

let textBox: UiWidget = {
  drag: () => {},
  resize: () => {}
}
```
- Literal Types
- Nullable Types
- Tuples
- Generics
- Decorator
- Mixins
- any vs unknown
