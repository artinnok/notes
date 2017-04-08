# Заметки по JavaScript


## Определение функций
* Function Expression:
```
var foo = function() {
  console.log('foo');
};
```

* Function Declaration:
```
function foo() {
  console.log('foo');
};
```
* Разница между FE и FD в том, что интерпретатор ищет Function Declaration перед запуском кода и исполняет их. В силу этого факта, FD можно вызывать до объявления:
```
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

