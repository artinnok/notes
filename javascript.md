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
* Разница между FE и FD в том, что интерпретатор ищет Function Declaration перед запуском кода и исполняет их. В силу этого факта, FD можно вызывать до объявления:
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
  
  return { b: 40 };
}

console.log(new.Bar().b); // 04
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
