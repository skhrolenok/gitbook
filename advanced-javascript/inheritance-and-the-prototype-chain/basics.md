# Basics

## Наследование свойств {#Наследование_свойств}

Объекты в JavaScript — динамические "контейнеры", наполненные свойствами \(называемыми **собственными свойствами**\). Каждый объект содержит ссылку на свой объект-прототип.  
При попытке получить доступ к какому-либо свойству объекта, свойство вначале выискивается в самом объекте, затем — в прототипе объекта, после чего — в прототипе прототипа, и так далее. Поиск ведется до тех пор, пока не найдено свойство с совпадающим именем или не достигнут конец цепочки прототипов.

```javascript
// В этом примере someObject.[[Prototype]] означает прототип someObject. 
// Это упрощённая нотация (описанная в стандарте ECMAScript). 
// Она не может быть использована в реальных скриптах.

// Допустим, у нас есть объект 'o' с собственными свойствами a и b
// {a:1, b:2} 

// o.[[Prototype]] имеет свойства b и с
// {b:3, c:4}

// Далее, o.[[Prototype]].[[Prototype]] является null
// null - это окончание в цепочке прототипов 
// по определению, null не имеет свойства [[Prototype]]

// В итоге полная цепочка прототипов выглядит так:
// {a:1, b:2} ---> {b:3, c:4} ---> null

console.log(o.a); // 1
// Есть ли у объекта 'o' собственное свойство 'a'? 
// Да, и его значение равно 1

console.log(o.b); // 2
// Есть ли у объекта 'o' собственное свойство 'b'? 
// Да, и его значение равно 2.
// У прототипа o.[[Prototype]] также есть свойство 'b', 
// но обращения к нему в данном случае не происходит. 
// Это и называется "property shadowing" (затенение свойства)

console.log(o.c); // 4
// Есть ли у объекта 'o' собственное свойство 'с'? 
// Нет, тогда поищем его в прототипе.
// Есть ли у объекта o.[[Prototype]] собственное свойство 'с'? 
// Да, оно равно 4

console.log(o.d); // undefined
// Есть ли у объекта 'o' собственное свойство 'd'? 
// Нет, тогда поищем его в прототипе.
// Есть ли у объекта o.[[Prototype]] собственное свойство 'd'? 
// Нет, продолжаем поиск по цепочке прототипов.
// o.[[Prototype]].[[Prototype]] равно null, прекращаем поиск, 
// свойство не найдено, возвращаем undefined
```

При добавлении к объекту нового свойства, создаётся новое собственное свойство \(own property\). Единственным исключением из этого правила являются наследуемые свойства, имеющие [getter или setter](https://developer.mozilla.org/en/docs/JavaScript/Guide/Working_with_Objects?redirectlocale=en-US&redirectslug=Core_JavaScript_1.5_Guide%2FWorking_with_Objects#Defining_getters_and_setters).

## Наследование "методов" {#Наследование_методов}

JavaScript не имеет "методов" в смысле, принятом в классической модели ООП. В JavaScript любая функция может быть добавлена к объекту в виде его свойства. Унаследованная функция ведёт себя точно так же, как любое другое свойство объекта, в том числе и в плане "затенения свойств" \(property shadowing\), как показано в примере выше \(в данном конкретном случае это форма _переопределения метода - method overriding_\).

В области видимости унаследованной функции ссылка[`this`](https://developer.mozilla.org/en/JavaScript/Reference/Operators/this)указывает на наследующий объект \(на наследника\), а не на прототип, в котором данная функция является собственным свойством.

```javascript
var o = {
  a: 2,
  m: function(){
    return this.a + 1;
  }
};

console.log(o.m()); // 3
// в этом случае при вызове 'o.m' this указывает на 'o'

var p = Object.create(o);
// 'p' - наследник 'o'

p.a = 12; // создаст собственное свойство 'a' объекта 'p'
console.log(p.m()); // 13
// при вызове 'p.m' this указывает на 'p'.
// т.е. когда 'p' наследует функцию 'm' объекта 'o', 
// this.a означает 'p.a', собственное свойство 'a' объекта 'p'
```

