1. Ограничение доступа к переменным и свойствам. 

Задача состоит в том, чтобы сократить время доступа к переменным и свойствам объектов в приложениях.
Причина заключается в том, что при каждом обращении процессору необходимо получить доступ к элементу в памяти, чтобы вычислить его результаты. Следовательно, это действие нужно выполнять как можно реже.
Например, при наличии цикла не нужно писать следующее:

```
for (let i = 0; i < arr.length; i++) {}
 ```

Лучше использовать данный вариант:
```
let length = arr.length;
for (let i = 0; i < length; i++) {}
```
Таким образом, на `arr.length` ссылаются один раз за цикл, а не обращаются к нему на каждой итерации.

2. Не объявляйте ненужные переменные

При каждом объявлении переменных, браузер должен выделить пространство памяти для них. Следовательно, чтобы уменьшить использование памяти, необходимо сократить количество объявления переменных.
Для примера возьмем следующий HTML-код:

```
<div id='foo'>
  <p>

  </p>
</div>
 ```
 
Чтобы установить текстовое содержимое элемента `p`, не нужно писать ничего вроде:

```
const foo = document.querySelector('#foo');
const p = foo.querySelector('p');
p.textContent = 'foo';
```
поскольку у нас есть 2 переменные. Это означает, что компьютер должен хранить значения еще для 2 переменных JavaScript.
Вместо этого можно сократить объявления переменных с помощью:
```
document.querySelector('#foo p').textContent = 'foo';
```
Используйте метод `querySelector`, а также связанный метод `querySelectorAll`, для выбора элементов, поскольку они оба применяют CSS-селекторы для выбора любого узла элемента HTML.

3. Значимые имена

Убедитесь, что ваши переменные названы осмысленно. Это уменьшит необходимость в дополнительных комментариях, так как ваш код говорит сам за себя.

❌ плохо:
```
cosnt ddmmyyyy = new Date();
```

✅ хорошо:
```
const date = new Date();
 ```

4. Не добавлять нежелательный контекст

Если имя класса или объекта говорит вам, что это такое, не включайте это в имя переменной.
❌ плохо:
```
const Book = {
 bookName: "Programming with JavaScript",
 bookPublisher: "Penguin",
 bookColour: "Yellow"
};
function wrapBook(book) {
 book.bookColour = "Brown";
}
```

✅ хорошо:
```
const Book = {
 name: "Programming with JavaScript",
 publisher: "Penguin",
 colour: "Yellow"
};
function wrapBook(book) {
 book.colour = "Brown";
}
```

5. Используйте строгую проверку типа

Используйте `===` вместо `==` . Это поможет избежать всяких ненужных проблем в дальнейшем. Если не делать проверки должным образом, то это может существенно повлиять на логику программы.

```
0 == false // true
 0 === false // false
 2 == "2" // true
 2 === "2" // false
```

6. Функции

Используйте описательные имена, которые говорят сами за себя. Рассматривая функции, которые представляют определенное поведение, имя функции должно быть глаголом или словосочетанием, полностью раскрывающим намерение, стоящее за ним, а также намерение аргументов. Их имена должны говорить о том, что они делают.

❌ плохо:
```
function sMail(user){
 //........
}
```

✅ хорошо:
```
function sendEmail(emailAddress){
 //.........
}
```

7. Функции должны делать что-то одно

Это одно из важнейших правил в программировании. Если функция выполняет более чем одну вещь, ee сложнее проверить и отладить. Если функция изолированна и делает что-то одно, ее проще отрефакторить. 

❌ плохо:
```
function notifyListeners(listeners) {
 listeners.forEach(listener => {
  const listenerRecord = database.lookup(listener);
  if (listenerRecord.isActive()) {
   notify(listener);
  }
 });
} 
```

✅ хорошо:
```
function notifyActiveListeners(listeners) {
  listeners.filter(isListenerActive).forEach(notify);
}

function isListenerActive(listener) {
  const listenerRecord = database.lookup(listener);
  return listenerRecord.isActive();
}
```

8. Удалять дубликат кода

Делайте все возможное, чтобы избежать дублирования кода. Писать один и тот же код более одного раза не только бесполезно, но и еще приведет к проблемам при рефакторинге кода. Вместо того, чтобы одно изменение повлияло на все соответствующие модули, вы должны найти все дубликаты модулей и повторить это изменение.
Часто дублирование в коде происходит потому, что два или более модуля имеют небольшие различия, так как у вас есть две или более немного отличающихся сущностей, которые имеют много общего.
Небольшие различия заставляют вас иметь очень похожие модули. Удаление дублирующего кода означает создание абстракции, которая может обрабатывать этот набор различных сущностей с помощью одной функции/модуля/класса.

❌ плохо:
```
function showDeveloperList(developers) {
  developers.forEach(developer => {
  const expectedSalary = developer.calculateExpectedSalary();
  const experience = developer.getExperience();
  const githubLink = developer.getGithubLink();
  const data = {
    expectedSalary,
    experience,
    githubLink
  };
  render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };
    render(data);
  });
}
```

✅ хорошо:
```
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const data = {
      expectedSalary,
      experience
    };
    switch (employee.type) {
      case "manager":
      data.portfolio = employee.getMBAProjects();
      break;
      case "developer":
      data.githubLink = employee.getGithubLink();
      break;
    }
    render(data);
  });
}
```

9. По-минимуму количество параметров функции

В идеале следует избегать большого количества параметров. Уменьшение количества параметров функции облегчило бы ее тестирование.
Один или два аргумента - идеальный вариант, а три следует по возможности избегать. Все, что больше этого, должно быть сведено воедино. Обычно, если у вас более двух аргументов, то ваша функция пытается сделать слишком много. В тех случаях, когда это не так, в большинстве случаев в качестве аргумента будет достаточно высокоуровненового объекта.

❌ плохо:
```
function createMenu(title, body, buttonText, cancellable) {
 // ...
}
createMenu("Foo", "Bar", "Baz", true);
```

✅ хорошо:
```
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}
createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

10. Отложенная загрузка сценариев

Загрузка файлов JavaScript — дорогостоящая операция. Браузер должен загрузить файл, проанализировать содержимое, а затем преобразовать его в машинный код и запустить.
Браузер загружает один файл построчно, не допуская выполнения других операций. Следовательно, нам нужно отложить эту операцию. Для этого разместите тег `script` в конце кода. Также можно использовать атрибут `defer` тега `script`. Кроме того, сценарии можно запускать после загрузки страницы, создавая элементы `script` и добавляя их следующим образом:

```
window.onload = () => {
  const element = document.createElement("script");
  element.src = "https://code.jquery.com/jquery-1.12.4.min.js";
  document.body.appendChild(element);
};
```

Все элементы, загружаемые после загрузки страницы, могут использовать этот метод.