# Заметки по JavaScript

Содержание:
1. [Определение функций](https://github.com/artinnok/notes/blob/master/javascript.md#Определение-функций)
2. [Объекты](https://github.com/artinnok/notes/blob/master/javascript.md#Объекты)
3. [Конструкторы](https://github.com/artinnok/notes/blob/master/javascript.md#Конструкторы)
4. [Прототипы](https://github.com/artinnok/notes/blob/master/javascript.md#Прототипы)


## Определение функций
* Function Expression:
```javascript
var foo = function() {
  console.log('foo');
}
```

* Function Declaration:
```javascript
function foo() {
  console.log('foo');
}
```
* Разница между **FE** и **FD** в том, что интерпретатор ищет **Function Declaration** перед запуском кода и исполняет их. В силу этого факта, **Function Declaration** можно вызывать до объявления:
```javascript
// Function Declaration
foo(); // foo

function foo() {
  console.log('foo');
}

// Function Expression
foo(); // ошибка

var foo = function() {
  console.log('foo');
}
```

## Объекты
* Объекты определяются как словари в Python, и ассоциативные массивы в других языках:
```javascript
var foo = {
  a: 10,
  b: 20 
};

console.log(foo.b); // 20
```

* Атрибуты можно определять также через кавычки:
```javascript
var foo = {
  'a': 10, 
  'b': 20
};

console.log(foo.b); // 20
```

* Доступ к атрибутам можно получить двумя способами: доступ через квадратную скобку `[]` и через точку `.`:
```javascript
foo["b"]; // 20
foo.a; // 10
```


* Объекты могут содержать в качестве атрибутов функции, которые могут получать доступ к объекту через `this`:
```javascript
var foo = {
  a: 10,
  b: 20,
  c: function() {
    return this.b;
  }
};

foo.c(); // 20
```
* `this` используется только внутри функции, но не в теле объекта:
```javascript
var foo = {
  this.a = 10; // ошибка
};

var bar = {
  a: 10,
  b: function() {
    return this.a;
  }
};

foo.a; // ошибка
bar.b(); // 10
```
* При определении объектов в конце строки пишется запятая `,`, а не точка с запятой `;`. Также в конце последней строки не ставится `,`:
```javascript
var foo = {
  a: 10;  // ошибка
  b: 20;  // ошибка
};

var bar = {
  a: 10,
  b: 20, // ошибка
};

var spam = {
  a: 10,
  b: 20
};
```

## Конструкторы
* Конструктором является любая функция, вызванная с помощью ключевого слова `new`. Новый объект доступен внутри функции при помощи `this`:
```javascript
function Foo() {
  this.a = 10;
  this.b = 20;
}

var foo = new Foo();
console.log(foo.a); // 10
```
* Все свойства, указанные через `this` внутри конструктора будут доступны **только новым** объектам созданным через `new`. Чтобы задать атрибут конструктору его надо монки патчить:
```javascript
function Foo() {
  this.a = 10;
}

console.log(Foo.a); // undefined
console.log(new Foo().a); // 10

Foo.a = 30;
console.log(Foo.a); // 30
console.log(new Foo().a); // 10
```

* По-умолчанию конструктор ничего не возвращает - его задача создать новый объект. Но если есть явный `return`:
  * Если `return` с объектом, то возвращается объект
  * Если `return` с любым примитивом, то примитив отбрасывается и вернется `this` - созданный объект 
  
```javascript
// пример с объектом
function Foo() {
  this.a = 10;
  
  return { a: 20 };
}

console.log(new Foo()); // Object {a: 20}

// пример с примитивом
function Bar() {
  this.b = 30;
  
  return "hello!";
}

console.log(new Bar()); // Bar {b: 30}
```

* В конструктор при вызове можно передать аргументы, которые могут учавствовать при инициализации:
```javascript
function Foo(a, b) {
  this.a = a;
  this.b = b;
}

var foo = new Foo(100, 200);
console.log(foo.a, foo.b); // 100 200
```
* Конструктор может получить методы другого конструктора - "наследовать" его методы и свойства:
```javascript
function Foo() {
  var a = 10;
  this.hello = function() {
    return a;
  }
}

function Bar() {
  // "наследуем"
  Foo.call(this);
  this.b = 20;
}

console.log(new Bar().hello()); // 10
```
* Но при такой реализции у наследника нет доступ к приватным свойствам родителя - чтобы появился доступ к свойствам, их необходимо явно прописать в `this` родителя:
```javascript
// пример с ошибкой
function Foo() {
  var a = 10;
  this.hello = function() {
    return a;
  }
}

function Bar() {
  Foo.call(this);
  this.b = 20;
}

console.log(new Bar().a); // ошибка 

// правильный пример
function Foo() {
  this.a = 10;
  this.hello = function() {
    return this.a;
  }
}

function Bar() {
  Foo.call(this);
  this.b = 20;
}

console.log(new Bar().a); // 10
```


## Прототипы
* Прототип служит резервным хранилищем свойств и методов для объекта - если атрибута нет у самого объекта, поиск переходит к прототипам. Грубый аналог `ChainMap` в Python
* Прототип указывается через свойство `__proto__`:
```javascript
var foo = {
  a: 10
};

var bar = {
  b: 20
};

bar.__proto__ = foo;
console.log(bar.b); // 20
console.log(bar.a); // 10
```
* Прототип используется только при чтении - т.е. если мы перепишем значение в самом объекте, это повлияет только на объект, но не на прототип:
```javascript
bar.a = 100; 

console.log(bar.a); // 100
console.log(bar.__proto__.a); // 10
```
* Прототип можно указать при определении конструктора:
```javascript
var foo = {
  a: 10
};

function Bar() {
  this.b = 20;
  // определение прототипа внутри конструктора
  this.__proto__ = foo;
}

console.log(new Bar().a); // 10
```
* Прототип при указании через свойство `__proto__` может быть объектом или экземпляром конструктора. Это логично, т.к. экземпляр конструктора, в конечном счете, является объектом:
```javascript
// прототип в виде объекта
var foo = {
  a: 10
};

function Bar() {
  this.b = 20;
  this.__proto__ = foo;
}

console.log(new Bar().a); // 10

// прототип в виде экземпляра конструктора
function Foo() {
  this.a = 10;
}

function Bar() {
  this.b = 20;
  this.__proto__ = new Foo();
}

console.log(new Bar().a); // 10
```
* Указание прототипа через `__proto__` является плохой практикой, обычно конструктору ставят свойство `prototype`. `prototype` при вызове конструктора с ключевым словом `new` ставит свойство `__proto__` только что созданному объекту:
```javascript
var foo = {
  a: 10
};

function Bar() {
  this.b = 20;
}

Foo.prototype = foo;

console.log(new Foo().a); // 10
```
* Свойством `__proto__` считается системным и есть у всех объектов, а `prototype` можно выставить любому объекту, но особый смысл оно будет иметь только у конструктора.
* Прототип при указании через свойство `prototype` конструктора может быть объектом или экземпляром конструктора:
```javascript
// прототип в виде объекта
var foo = {
  a: 10
};

function Bar() {
  this.b = 20;
}

Bar.prototype = foo;

console.log(new Bar().a); // 10

// прототип в виде экземпляра конструктора
function Foo() {
  this.a = 10;
}

function Bar() {
  this.b = 20;
}

Bar.prototype = new Foo();

console.log(new Bar().a); // 10
```
* Можно реализовать цепочку прототипов - некое подобие "наследования":
```javascript
function Foo() {
  this.a = 10;
}

function Bar() {
  this.b = 20;
}

Bar.prototype = new Foo();

function Spam() {
  this.c = 30;
}

Spam.prototype = new Bar();

var spam = new Spam();

console.log(spam.a, spam.b, spam.c); // 10 20 30
```
* Наследование в прототипном стиле также можно реализовать через `Object.create` - методы определяются в прототипе. Но это не наследует свойств:
```javascript
function Foo() {
  this.a = 10; 
}

Foo.prototype.hello = function() {
    return this.b;
}

function Bar() {
  this.b = 20;
}

Bar.prototype = Object.create(Foo.prototype);

console.log(new Bar().hello()); // 20
```
