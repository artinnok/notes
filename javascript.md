# Заметки по JavaScript


## Определение функций
* Function Expression:
```javascript
var foo = function() {
  console.log('foo');
};
```

* Function Declaration:
```javascript
function foo() {
  console.log('foo');
};
```
* Разница между **FE** и **FD** в том, что интерпретатор ищет **Function Declaration** перед запуском кода и исполняет их. В силу этого факта, **Function Declaration** можно вызывать до объявления:
```javascript
// Function Declaration
foo(); // foo

function() {
  console.log('foo');
};

// Function Expression
foo(); // ошибка

function() {
  console.log('foo');
};
```

## Объекты
* Объекты определяются как словари в Python, и ассоциативные массивы в других языках:
```javascript
var foo = {
  a: 10,
  b: 20 
}

console.log(foo.b); // 20
```

* Атрибуты можно определять также через кавычки:
```javascript
var foo = {
  'a': 10, 
  'b': 20
}

console.log(foo.b) // 20
```

* Доступ к атрибутам можно получить двумя способами: доступ через квадратную скобку `[]` и через точку `.`:
```javascript
foo["b"] // 20
foo.a // 10
```


* Объекты могут содержать в качестве атрибутов функции, которые могут получать доступ к объекту через `this`:
```javascript
var foo = {
  a: 10,
  b: 20,
  c: function() {
    return this.b;
  }
}

foo.c(); // 20
```
* `this` используется только внутри функции, но не в теле объекта:
```javascript
var foo = {
  this.a = 10; // ошибка
}

var bar = {
  a: 10,
  b: function() {
    return this.a;
  }
}

foo.a // ошибка
bar.b() // 10
```
* При определении объектов в конце строки пишется запятая `,`, а не точка с запятой `;`. Также в конце последней строки не ставится `,`:
```javascript
var foo = {
  a: 10;  // ошибка
  b: 20;  // ошибка
}

var bar = {
  a: 10,
  b: 20, // ошибка
}

var spam = {
  a: 10,
  b: 20
}
```

## Конструкторы
* Конструктором является любая функция, вызванная с помощью ключевого слова `new`. Новый объект доступен внутри функции при помощи `this`:
```javascript
function Foo() {
  this.a = 10;
  this.b = 20;
}

foo = new Foo();
console.log(foo.a); // 10
```
* По-умолчанию конструктор ничего не возвращает - его задача создать новый объект. Но если есть явный `return`:
  * Если `return` с объектом, то возвращается объект
  * Если `return` с любым примитивом, то примитив отбрасывается
  
```javascript
// пример с объектом
function Foo() {
  this.a = 10;
  
  return { a: 20 };
}

console.log(new Foo().a); // 20

// пример с примитивом
function Bar() {
  this.b = 30;
  
  return "hello!";
}

console.log(new Bar().b); // 30
```

* В конструктор при вызове можно передать аргументы, которые могут учавствовать при инициализации:
```javascript
function Foo(a, b) {
  this.a = a;
  this.b = b;
}

foo = new Foo(100, 200);
console.log(foo.a, foo.b); // 100 200
```

## Прототипы
* Прототип служит резервным хранилищем свойств и методов для объекта - если атрибута нет у самого объекта, поиск переходит к прототипам. Грубый аналог `ChainMap` в Python
* Прототип указывается через свойство `__proto__`:
```javascript
// присвоение __proto__ во время исполнения
var foo = {
  a: 10
}

var bar = {
  b: 20
}

bar.__proto__ = foo;
console.log(bar.b); // 20
console.log(bar.a); // 10

// присвоение __proto__ при определении
var foo = {
  a: 10
}

var bar = {
  b: 20,
  __proto__: foo
}

console.log(bar.b); // 20
console.log(bar.a); // 10
```
* Прототип используется только при чтении - т.е. если мы перепишем значение в самом объекте, это повлияет только на объект:
```javascript
bar.a = 100; 

console.log(bar.a) // 100
console.log(bar.__proto__.a) // 10
```
