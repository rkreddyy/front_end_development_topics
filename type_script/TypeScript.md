# TypeScript
- TYpescript types
  - Built-in Type Primitives: boolean, string, number, undefined, null, any, unknown, never, void, bigint, symbol 
- [TypeScript Cheat Sheets](https://www.typescriptlang.org/cheatsheets)
  - Type
    - Primitive Type
    - Object Literal Type
    - Tuple Type
    - Union Type
    - Intersection Types
    - Type Indexing
    - Type from Value
    - Type from Func Return
    - Type from Module
  - Interface
  - Class
    - A private field is declared by appending the field with `#`.
    - Private access is just to this class, protected allows to subclasses. Only used for type checking, public is the default. Private and protected methods can be declared as 
    ```
     private makeRequest() {...}
     protected handleRequest() {...}
    ```
    - Abstract Classes: A class can be declared as not implementable, but as existing to be subclassed in the type system. As can members of the class.
    ```
    abstract class Animal {
     abstract getName(): string;
     printName() {
      console.log("Hello, " + this.getName());
     }
    }
    class Dog extends Animal { getName(): { ... } }
    ```
  - Nullable Types
  - Generics
    - Declare a type which can change in your class methods.
     ```
     class Box<Type>{
      contents: Type
      constructor(value: Type){
        this.contents = value;
      }
     }

     const stringBox = new Box("a package");
     const intBox = new Box<number>(1);
     ```
  - [Decorators and Attributes](https://www.typescriptlang.org/docs/handbook/decorators.html): 
    A Decorator is a special kind of declaration that can be attached to a class declaration, method, accessor, property, or parameter. Decorators use the form @expression, where expression must evaluate to a function that will be called at runtime with information about the decorated declaration.
    ```
    // The following is an example of a class decorator (@sealed) applied to a BugReport class:
    @sealed
    class BugReport {
      type = "report";
      title: string;

      constructor(t: string) {
        this.title = t;
      }
    }
    
    // We can define the @sealed decorator using the following function declaration:
    function sealed(constructor: Function) {
      Object.seal(constructor);
      Object.seal(constructor.prototype);
    }
    ```
  - Mixins
  - any vs unknown
- Type narrowing Example(Union Types)
```
function kgToLbs(weight: number | string): number{
  // Type Narrowing
  if(typeof weight == 'number') return weight * 2.2;
  else return parseInt(weight) * 2.2;
}

console.log( kgToLbs(10));
console.log(kgToLbs('10Kg'));
```
- Type Intersection Example
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
- Interfaces are merged, so multiple declarations will add new fields to the type definition.
```
interface APICall {
 data: Response
}

interface APICall {
 error?: Error
}
```

- Discriminated Unions. All members of the union have the same property name, CFA can discriminate on that.
```
type Responses = 
 | { status: 200, data: any }
 | { status: 301, to: string }
 | { status: 400, error: Error }
 
// Usage
const response = getResponse()
switch(response.status) {
 case 200: return response.data
 case 301: return redirect(response.to)
 case 400: return response.error
}
```

