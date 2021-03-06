# Events

## Браузерные события

Для реакции на действия посетителя и внутреннего взаимодействия скриптов существуют _события_.

_Событие_– это сигнал от браузера о том, что что-то произошло. Существует много видов событий. Посмотрим список самых часто используемых, пока просто для ознакомления:

**События мыши:**

* `click`– происходит, когда кликнули на элемент левой кнопкой мыши
* `contextmenu`– происходит, когда кликнули на элемент правой кнопкой мыши
* `mouseover`– возникает, когда на элемент наводится мышь
* `mousedown`и`mouseup`– когда кнопку мыши нажали или отжали
* `mousemove`– при движении мыши

**События на элементах управления:**

* `submit`– посетитель отправил форму`<form>`
* `focus`– посетитель фокусируется на элементе, например: нажимает на`<input>`

**Клавиатурные события:**

* `keydown`– когда посетитель нажимает клавишу
* `keyup`– когда посетитель отпускает клавишу

**События документа:**

* `DOMContentLoaded`– когда HTML загружен и обработан, DOM документа полностью построен и доступен.

**События CSS:**

* `transitionend`– когда CSS-анимация завершена.

### Назначение обработчиков событий

Событию можно назначить _обработчик_, то есть функцию, которая сработает, как только событие произошло.

Именно благодаря обработчикам JavaScript-код может реагировать на действия посетителя.

Есть несколько способов назначить событию обработчик. Сейчас мы их рассмотрим, начиная от самого простого.

#### Использование атрибута HTML

Обработчик может быть назначен прямо в разметке в атрибуте, который называется`on<событие>`.

Например, чтобы прикрепить`click`-событие к`input`кнопке, можно присвоить обработчик`onclick`таким образом:

```text
<input value="Нажми меня" onclick="alert('Клик!')" type="button">
```

При клике мышкой, на кнопке выполнится код, указанный в атрибуте`onclick`.

Обратите внимание: для содержимого атрибута`onclick`используются _одинарные кавычки_, так как сам атрибут находится в двойных.

Частая ошибка новичков в том, что они забывают, что код находится внутри атрибута. Запись вида`onclick="alert("Клик!")"` с двойными кавычками внутри не будет работать. Если вам действительно нужно использовать именно двойные кавычки, то это можно сделать, заменив их на`&quot;`, то есть так:`onclick="alert(&quot;Клик!&quot;)"`.

Однако обычно этого не требуется, так как прямо в разметке пишутся только очень простые обработчики. Если нужно сделать что-то сложное, то имеет смысл описать это в функции, и в обработчике вызвать уже её.

Следующий пример по клику запускает функцию`countRabbits()`.

```javascript
<!DOCTYPE HTML>
<html>
<head>
  <meta charset="utf-8">

  <script>
    function countRabbits() {
      for(var i=1; i<=3; i++) {
        alert("Кролик номер " + i);
      }
    }
  </script>
</head>
<body>
  <input type="button" onclick="countRabbits()" value="Считать кроликов!"/>
</body>
</html>
```

Как мы помним, атрибут HTML-тега не чувствителен к регистру, поэтому `ONCLICK` будет работать так же, как`onClick`или `onCLICK`… Но, как правило, атрибуты пишут в нижнем регистре:`onclick`.

#### Использование свойства DOM-объекта

Можно назначать обработчик, используя свойство DOM-элемента`on<событие>`.

Пример установки обработчика`click`:

```text
<input id="elem" type="button" value="Нажми меня" />
<script>
  elem.onclick = function() {
    alert( 'Спасибо' );
  };
</script>
```

Если обработчик задан через атрибут, то браузер читает HTML-разметку, создаёт новую функцию из содержимого атрибута и записывает в свойство`onclick`.

**Этот способ, по сути, аналогичен предыдущему.**

Обработчик хранится именно в DOM-свойстве, а атрибут – лишь один из способов его инициализации.

Эти два примера кода работают одинаково:

```javascript
<input type="button" onclick="alert('Клик!')" value="Кнопка"/>
```

HTML + JS:

```text
<input type="button" id="button" value="Кнопка" />
<script>
  button.onclick = function() {
    alert( 'Клик!' );
  };
</script>
```

**Так как DOM-свойство**`onclick`**, в итоге одно, то назначить более одного обработчика нельзя.**

В примере ниже назначение через JavaScript перезапишет обработчик из атрибута:

```text
<input type="button" id="elem" onclick="alert('До')" value="Нажми меня" />
<script>
  elem.onclick = function() { // перезапишет существующий обработчик
    alert( 'После' ); // выведется только это
  };
</script>
```

Кстати, обработчиком можно назначить и уже существующую функцию:

```javascript
function sayThanks() {
  alert( 'Спасибо!' );
}

elem.onclick = sayThanks;
```

#### Доступ к элементу через this

Внутри обработчика события`this`ссылается на текущий элемент, то есть на тот, на котором он сработал.

Это можно использовать, чтобы получить свойства или изменить элемент.

В коде ниже`button`выводит свое содержимое, используя`this.innerHTML`:

```text
<button onclick="alert(this.innerHTML)">Нажми меня</button>
```

#### Недостаток назначения через свойство

Фундаментальный недостаток описанных выше способов назначения обработчика – невозможность повесить _несколько _обработчиков на одно событие.

Например, одна часть кода хочет при клике на кнопку делать ее подсвеченной, а другая – выдавать сообщение. Нужно в разных местах два обработчика повесить.

При этом новый обработчик будет затирать предыдущий. Например, следующий код на самом деле назначает один обработчик – последний:

```javascript
input.onclick = function() { alert(1); }
// ...
input.onclick = function() { alert(2); } // заменит предыдущий обработчик
```

Разработчики стандартов достаточно давно это поняли и предложили альтернативный способ назначения обработчиков при помощи специальных методов, которые свободны от указанного недостатка.

#### addEventListener и removeEventListener

Методы`addEventListener`и`removeEventListener`являются современным способом назначить или удалить обработчик, и при этом позволяют использовать сколько угодно любых обработчиков.

Назначение обработчика осуществляется вызовом`addEventListener`с тремя аргументами:

```javascript
element.addEventListener(event, handler[, phase]);
```

`event`

Имя события, например`click`

`handler`

Ссылка на функцию, которую надо поставить обработчиком.

`phase`

Необязательный аргумент, «фаза», на которой обработчик должен сработать.

Удаление обработчика осуществляется вызовом`removeEventListener`:

```javascript
// передать те же аргументы, что были у addEventListener
element.removeEventListener(event, handler[, phase]);
```

Метод`addEventListener`позволяет добавлять несколько обработчиков на одно событие одного элемента, например:

```text
<input id="elem" type="button" value="Нажми меня"/>

<script>
  function handler1() {
    alert('Спасибо!');
  };

  function handler2() {
    alert('Спасибо ещё раз!');
  }

  elem.onclick = function() { alert("Привет"); };
  elem.addEventListener("click", handler1); // Спасибо!
  elem.addEventListener("click", handler2); // Спасибо ещё раз!
</script>
```

Как видно из примера выше, можно одновременно назначать обработчики и через DOM-свойство, и через`addEventListener`. Однако во избежание путаницы, рекомендуется выбрать один способ.

## Порядок обработки событий

События могут возникать не только по очереди, но и «пачкой» по много сразу. Возможно и такое, что во время обработки одного события возникают другие, например пока выполнялся код для`onclick`– посетитель нажал кнопку на клавиатуре \(событие`keydown`\).

### Главный поток

В каждом окне выполняется только один _главный _поток, который занимается выполнением JavaScript, отрисовкой и работой с DOM.

Он выполняет команды последовательно, может делать только одно дело одновременно и блокируется при выводе модальных окон, таких как`alert`.

### Очередь событий

Произошло одновременно несколько событий или во время работы одного случилось другое – как главному потоку обработать это?

Если главный поток прямо сейчас занят, то он не может срочно выйти из середины одной функции и прыгнуть в другую. А потом третью. Отладка при этом могла бы превратиться в кошмар, потому что пришлось бы разбираться с совместным состоянием нескольких функций сразу.

**Когда происходит событие, оно попадает в очередь.**

Внутри браузера непрерывно работает «главный внутренний цикл», который следит за состоянием очереди и обрабатывает события, запускает соответствующие обработчики и т.п.

**Иногда события добавляются в очередь сразу пачкой.**

Например, при клике на элементе генерируется несколько событий:

Сначала

1. `mousedown`– нажата кнопка мыши.
2. Затем`mouseup`– кнопка мыши отпущена, и так как это было над одним элементом, то дополнительно генерируется`click`\(два события сразу\).

```text
<textarea rows="8" cols="40" id="area">Кликни меня
</textarea>

<script>
  area.onmousedown = function(e) { this.value += "mousedown\n"; this.scrollTop = this.scrollHeight; };
  area.onmouseup = function(e) { this.value += "mouseup\n"; this.scrollTop = this.scrollHeight; };
  area.onclick = function(e) { this.value += "click\n"; this.scrollTop = this.scrollHeight; };
</script>
```

Таким образом, при нажатии кнопки мыши в очередь попадёт событие`mousedown`, а при отпускании – сразу два события:`mouseup`и`click`. Браузер обработает их строго одно за другим:`mousedown`→`mouseup`→`click`.

При этом каждое событие из очереди обрабатывается полностью отдельно от других.

### Вложенные \(синхронные\) события

Обычно возникающие события «становятся в очередь».

Но в тех случаях, когда событие инициируется не посетителем, а кодом, то оно, как правило, обрабатывается синхронно, то есть прямо сейчас.

Рассмотрим в качестве примера событие`onfocus`.

#### Пример: событие onfocus

Когда посетитель фокусируется на элементе, возникает событие`onfocus`. Обычно оно происходит, когда посетитель кликает на поле ввода, например:

```text
<p>При фокусе на поле оно изменит значение.</p>
<input type="text" onfocus="this.value = 'Фокус!'" value="Кликни меня">
```

При фокусе на поле, оно изменит значение.

Но ту же фокусировку можно вызвать и явно, вызовом метода`elem.focus()`:

```text
<input type="text" id="elem" onfocus="this.value = 'Фокус!'">

<script>
  // сфокусируется на input и вызовет обработчик onfocus
  elem.focus();
</script>
```

При этом обработчик`onclick`вызовет метод`focus()`на текстовом поле`text`. Код обработчика`onfocus`, который при этом запустится, сработает синхронно, прямо сейчас, до завершения`onclick`.

```text
<input type="button" id="button" value="Нажми меня">
<input type="text" id="text" size="60">

<script>

  button.onclick = function() {
    text.value += ' ->в onclick ';

    text.focus(); // вызов инициирует событие onfocus

    text.value += ' из onclick-> ';
  };

  text.onfocus = function() {
    text.value += ' !focus! ';
  };
</script>
```

## Объект события

Чтобы хорошо обработать событие, недостаточно знать о том, что это – «клик» или «нажатие клавиши». Могут понадобиться детали: координаты курсора, введенный символ и другие, в зависимости от события.

Детали произошедшего браузер записывает в «объект события», который передается первым аргументом в обработчик.

### Свойства объекта события

Пример ниже демонстрирует использование объекта события:

```text
<input type="button" value="Нажми меня" id="elem">

<script>
  elem.onclick = function(event) {
    // вывести тип события, элемент и координаты клика
    alert(event.type + " на " + event.currentTarget);
    alert(event.clientX + ":" + event.clientY);
  }
</script>
```

Свойства объекта`event`:

`event.type`

Тип события, в данном случае`click`

`event.currentTarget`

Элемент, на котором сработал обработчик. Значение – в точности такое же, как и у`this`, но бывают ситуации, когда обработчик является методом объекта, и его`this`при помощи`bind`привязан к этому объекту, тогда мы можем использовать`event.currentTarget`.

`event.clientX / event.clientY`

Координаты курсора в момент клика \(относительно окна\).

## Всплытие и перехват

Основной принцип всплытия:

**При наступлении события, обработчики сначала срабатывают на самом вложенном элементе, затем на его родителе, затем выше и так далее, вверх по цепочке вложенности.**

Например, есть 3 вложенных элемента`FORM > DIV > P` с обработчиком на каждом:

```text
<style>
  body * {
    margin: 10px;
    border: 1px solid blue;
  }
</style>

<form onclick="alert('form')">FORM
  <div onclick="alert('div')">DIV
    <p onclick="alert('p')">P</p>
  </div>
</form>
```

Всплытие гарантирует, что клик по внутреннему`<p>`вызовет обработчик`onclick`\(если есть\) сначала на самом`<p>`, затем на элементе`<div>,`далее на элементе`<form>`и так далее вверх по цепочке родителей до самого`document`.

![](../.gitbook/assets/event-phase.png)

Поэтому если в примере выше кликнуть на`P`, то последовательно выведутся`alert`:`p`→`div`→`form`.

Этот процесс называется _всплытием_, потому что события «всплывают» от внутреннего элемента вверх через родителей.

### Целевой элемент event.target

На каком бы элементе мы ни поймали событие, всегда можно узнать, где конкретно оно произошло.

**Самый глубокий элемент, который вызывает событие, называется **_**«целевым» **_**или **_**«исходным» **_**элементом и доступен как **`event.target`**.**

Отличия от`this`\(=`event.currentTarget`\):

* `event.target`– это **исходный элемент**, на котором произошло событие, в процессе всплытия он неизменен.
* `this`– это **текущий элемент**, до которого дошло всплытие, на нем сейчас выполняется обработчик.

Например, если стоит только один обработчик`form.onclick`, то он «поймает» все клики внутри формы. Где бы ни был клик внутри – он всплывет до элемента`<form>`, на котором сработает обработчик.

При этом:

* `this`\(`=event.currentTarget`\) всегда будет сама форма, так как обработчик сработал на ней.
* `event.target`будет содержать ссылку на конкретный элемент внутри формы, самый вложенный, на котором произошел клик.

Возможна и ситуация, когда`event.target`и`this`– один и тот же элемент, например, если в форме нет других тегов и клик был на самом элементе`<form>`.

### Прекращение всплытия

Всплытие идет прямо наверх. Обычно событие будет всплывать вверх и вверх, до элемента`<html>`, а затем до`document`, а иногда даже до`window`, вызывая все обработчики на своем пути.

**Но любой промежуточный обработчик может решить, что событие полностью обработано, и остановить всплытие.**

Для остановки всплытия нужно вызвать метод`event.stopPropagation()`.

Например, здесь при клике на кнопку обработчик`body.onclick`не сработает:

```text
<body onclick="alert('сюда обработка не дойдёт')">
  <button onclick="event.stopPropagation()">Кликни меня</button>
</body>
```

### Погружение

В современном стандарте, кроме «всплытия» событий, предусмотрено еще и «погружение».

Оно гораздо менее востребовано, но иногда, очень редко, знание о нем может быть полезным.

Строго говоря, стандарт выделяет целых три стадии прохода события:

Событие сначала идет сверху вниз. Эта стадия называется

1. _«стадия перехвата»_\(capturing stage\).
2. Событие достигло целевого элемента. Это –_«стадия цели»_\(target stage\).
3. После этого событие начинает всплывать. Это –_«стадия всплытия»_\(bubbling stage\).

В [стандарте DOM Events 3](http://www.w3.org/TR/DOM-Level-3-Events/) это продемонстрировано так:

![](../.gitbook/assets/event-capturing.png)

То есть, при клике на`TD,`событие путешествует по цепочке родителей сначала вниз к элементу \(«погружается»\), а потом наверх \(«всплывает»\), задействуя обработчики.

Обработчики, добавленные через`on...`-свойство, ничего не знают о стадии перехвата, а начинают работать со всплытия.

Чтобы поймать событие на стадии перехвата, нужно использовать третий аргумент`addEventListener`:

Если аргумент

* `true`, то событие будет перехвачено по дороге вниз.
* Если аргумент`false`, то событие будет поймано при всплытии.

## Делегирование событий

Всплытие событий позволяет реализовать один из самых важных приемов разработки –_делегирование_.

Он заключается в том, что если у нас есть много элементов, события на которых нужно обрабатывать похожим образом, то вместо того, чтобы назначать обработчик каждому – мы ставим один обработчик на их общего предка. Из него можно получить целевой элемент`event.target`, понять, на каком именно потомке произошло событие, и обработать его.

