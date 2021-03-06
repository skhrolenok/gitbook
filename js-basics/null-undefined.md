# Null, Undefined

## Значение null

Ключевое слово null в JavaScript имеет специальный смысл. Обычно считается, что у значения null объектный тип, а это говорит об отсутствии объекта. Значение null уникально и отличается от любых других. Если переменная равна null, следовательно, в ней не содержится допустимого объекта, массива, числа, строки или логического значения.

Когда значение null используется в логическом контексте, оно преобразуется в значение false, в числовом контексте оно преобразуется в значение 0, а в строковом контексте — в строку "null".

## Значение undefined

Еще одно специальное значение, иногда используемое в JavaScript, – undefined. Оно возвращается при обращении либо к переменной, которая была объявлена, но которой никогда не присваивалось значение, либо к свойству объекта, которое не существует. Заметьте, что специальное значение undefined – это не то же самое, что null.

Хотя значения null и undefined не эквивалентны друг другу, оператор эквивалентности == считает их равными. Рассмотрим следующее выражение:

```javascript
my.prop == null
```

Это сравнение истинно, если свойство my.prop не существует, либо если оно существует, но содержит значение null. Поскольку значение null и undefined обозначают отсутствие значения, это равенство часто оказывается тем, что нам нужно. Однако когда действительно требуется отличить значение null от значения undefined, нужен оператор идентичности === или оператор typeof.

Объявив, но не инициализировав переменную, вы гарантируете, что переменная имеет значение undefined. Оператор void предоставляет еще один способ получения значения undefined. Когда значение undefined используется в логическом контексте, оно преобразуется в значение false. В числовом контексте – в значение NaN, а в строковом – в строку "undefined".

