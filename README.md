Оригинальный репозиторий: [ryanmcdermott/clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)

`Актуализировано по меньшей мере по состоянию на 4 февраля 2017 года`

# clean-code-javascript

## Содержание
  1. [Введение](https://github.com/maksugr/clean-code-javascript#Введение)
  2. [Переменные](https://github.com/maksugr/clean-code-javascript#Переменные)
  3. [Функции](https://github.com/maksugr/clean-code-javascript#Функции)
  4. [Объекты и структуры данных](https://github.com/maksugr/clean-code-javascript#Объекты-и-структуры-данных)
  5. [Классы](https://github.com/maksugr/clean-code-javascript#Классы)
  6. [Тестирование](https://github.com/maksugr/clean-code-javascript#Тестирование)
  7. [Асинхронность](https://github.com/maksugr/clean-code-javascript#Асинхронность)
  8. [Отлавливание ошибок](https://github.com/maksugr/clean-code-javascript#Отлавливание-ошибок)
  9. [Форматирование](https://github.com/maksugr/clean-code-javascript#Форматирование)
  10. [Комментарии](https://github.com/maksugr/clean-code-javascript#Комментарии)
  11. [Переводы](https://github.com/maksugr/clean-code-javascript#Переводы)

## Введение
![Юмористическое изображение оценки качества программного обеспечения: в качестве единицы измерения используется количество ругательства, выкрикиваемых во время чтения кода](http://www.osnews.com/images/comics/wtfm.jpg)

Адаптация для JavaScript принципов разработки программного обеспечения из книги Роберта Мартина [*Чистый код*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882). Это не руководство по стилю. Это руководство по написанию читабельного и пригодного для переиспользования и рефакторинга программного обеспечения на JavaScript.

Не каждый принцип здесь должен строго соблюдаться и еще меньше получит всеобщее признание. Это принципы и ничего более, но они накапливались в течение многих лет коллективным опытом авторов *Чистого кода*.

Искусству написания программного обеспечения немногим более пятидесяти лет, и мы
все еще многому учимся. Когда программная архитектура постареет до возраста самой архитектуры, быть может тогда у нас появятся жесткие правила, которым необходимо следовать. А сейчас пусть эти принципы служат критерием оценка качества JavaScript-кода, создаваемого вами и вашей командой.

И еще. Знание этих принципов не сразу сделает вас лучше как разработчиков программного обеспечения, а их использование в течение многих лет не гарантирует, что вы перестанете совершать ошибки. Каждый фрагмент кода как первый набросок, как мокрая глина принимает свою форму постепенно. Наконец, все мы пронизаны несовершенством, когда наш код просматривают коллеги. Не истязайте себя за первые, нуждающиеся в улучшении, наброски. Вместо этого истязайте свой код!

## **Переменные**
### Используйте выразительные и произносимые имена переменных

**Плохо:**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Хорошо:**
```javascript
const currentDate = moment().format('YYYY/MM/DD');
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Используйте один и тот же лексикон для того же типа переменной

**Плохо:**
```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Хорошо:**
```javascript
getUser();
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Используйте имена, доступные для поиска
Мы прочитаем больше кода, чем когда-либо напишем. Это важно, чтобы код, который мы создаем, был читабельным и доступным для поиска. Невыразительно названные переменные ухудшают понимание наших программ и оскорбляют наших читателей. Делайте ваши обозначения доступными для поиска. Инструменты вроде [buddy.js](https://github.com/danielstjules/buddy.js) и [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) помогут найти неназванные константы.

**Плохо:**
```javascript
// Что такое 86400000?
setTimeout(blastOff, 86400000);

```

**Хорошо:**
```javascript
// Объявляйте их как глобальный `const` со всеми буквами в заглавном регистре.
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);

```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Используйте объясняющие переменные

**Плохо:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(address.match(cityZipCodeRegex)[1], address.match(cityZipCodeRegex)[2]);
```

**Хорошо:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Избегайте мысленных связей
Явное лучше, чем неявное.

**Плохо:**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Стойте. Еще раз, что такое `l`?
  dispatch(l);
});
```

**Хорошо:**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((location) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Не добавляйте ненужный контекст
Если ваше имя класса/объекта говорит за себя, не повторяйте его в имени переменной.

**Плохо:**
```javascript
const Car = {
  carMake: 'Honda',
  carModel: 'Accord',
  carColor: 'Blue'
};

function paintCar(car) {
  car.carColor = 'Red';
}
```

**Хорошо:**
```javascript
const Car = {
  make: 'Honda',
  model: 'Accord',
  color: 'Blue'
};

function paintCar(car) {
  car.color = 'Red';
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Используйте аргументы по умолчанию вместо короткой схемы вычисления
Аргументы по умолчанию часто чище, чем короткая схема вычисления. Имейте в виду, что если вы используете ее, ваша функция задает значения по умолчанию только для `undefined` аргументов. Другие "falsy" значения, такие как `''`, `""`, `false`, `null`, `0` и `NaN`, не будут заменены значением по умолчанию.

**Плохо:**
```javascript
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.';
  // ...
}

```

**Хорошо:**
```javascript
function createMicrobrewery(breweryName = 'Hipster Brew Co.') {
  // ...
}

```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

## **Функции**
### Аргументы функции (идеально два или меньше)
Ограничение количества параметров функции невероятно важно, потому что это упрощает тестирование. Более трех входных данных приводят к комбинаторному взрыву, где вы должны протестировать множество различных вариантов с каждым отдельным аргументом.

Один аргумент или два - идеальный случай, трех аргументов нужно избегать. Большее количество аргументов должно быть объединено. Как правило, если у вас более двух аргументов, ваша функция пытается сделать слишком много. Для большинства случаев, где это простительно, в качестве аргумента будет достаточно объекта верхнего уровня.

Поскольку JavaScript позволяет создавать объекты на лету, без классов в качестве основы, вы можете использовать их, если нуждаетесь во множестве аргументов.

Для того, чтобы сделать очевидным, какие свойства функция ожидает на входе, вы можете использовать синтаксис деструкции из ES2015/ES6. Он имеет несколько преимуществ:

1. Когда вы смотрите на сигнатуру функции, то сразу понятно, какие свойства используются.
2. Деструкция клонирует примитивные значения аргумента-объекта, переданного в функцию. Это предотвращает побочные эффекты. Примечание: объекты и массивы, которые деструктурированы из аргумента-объекта, НЕ клонируются.
3. Линтер может предупредить вас о неиспользуемых свойствах, что было бы невозможно без деструктурирования.

**Плохо:**
```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**Хорошо:**
```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**


### Функции должны делать одно действие
Это, безусловно, самое важное правило в разработке программного обеспечения. Когда функции делают больше чем одну вещь, их труднее объединять, тестировать и анализировать. Если вы сможете изолировать функцию так, чтобы она производила только одно действие, в дальнейшем она легко может быть переработана, а ваш код будет гораздо чище. Даже если из данного руководства вы не почерпнете ничего, кроме этого принципа, то вы уже оставите позади многих разработчиков.

**Плохо:**
```javascript
function emailClients(clients) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Хорошо:**
```javascript
function emailClients(clients) {
  clients
    .filter(isClientActive)
    .forEach(email);
}

function isClientActive(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Имена функций должны говорить, что они делают

**Плохо:**
```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// Из имени функции сложно понять, что она добавляет
addToDate(date, 1);
```

**Хорошо:**
```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Функции должны иметь один уровень абстракции
Если у вас есть более чем один уровень абстракции, ваша функция, как правило, делает слишком много. Разделение функций приводит к возможности переиспользования и простоте тестирования.

**Плохо:**
```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // анализируем...
  });

  ast.forEach((node) => {
    // разбираем...
  });
}
```

**Хорошо:**
```javascript
function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

function lexer(tokens) {
  const ast = [];
  tokens.forEach((token) => {
    ast.push( /* ... */ );
  });

  return ast;
}

function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const ast = lexer(tokens);
  ast.forEach((node) => {
    // разбираем...
  });
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Удаление повторяющегося кода
Делайте все, чтобы избежать дублирования кода. Повторяющийся код плох тем, что если придется править вашу логику, ее придется править в нескольких местах.

Представьте себе, вы открыли ресторан и ведете учет продуктов (всех ваших помидоров, лука, чеснока, специй и т.д.). Если у вас несколько списков с продуктами, то, когда у вас закажут томатный суп, вам придется обновить их все. Если список у вас только один, то обновлять придется только его!

Часто вы дублируете код, потому что у вас есть несколько логических участков, которые во многом схожи, но их различия заставляют вас иметь несколько функций, делающих много одинаковых операций. Удаление повторяющегося кода означает создание абстракции, обрабатывающей эту разную логику с помощью всего одной функции/модуля/класса.

Получение абстракции имеет решающее значение, поэтому вы должны следовать принципам проектирования классов (SOLID-принципам), изложенным в разделе *Классы*. Плохие абстракции могут оказаться хуже, чем повторяющийся код, так что будьте осторожны! Попросту говоря, если вы можете сделать хорошую абстракцию, делайте! Не повторяйте себя, в противном случае в любом момент вы можете обнаружить себя, вносящим изменения в несколько мест ради изменения одной единственной логики.

**Плохо:**
```javascript
function showDeveloperList(developers) {
  developers.forEach((developer) => {
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
  managers.forEach((manager) => {
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

**Хорошо:**
```javascript
function showEmployeeList(employees) {
  employees.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    let portfolio = employee.getGithubLink();

    if (employee.type === 'manager') {
      portfolio = employee.getMBAProjects();
    }

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Значения по умолчанию полей объекта устанавливайте с помощью Object.assign

**Плохо:**
```javascript
const menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable === undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Хорошо:**
```javascript
const menuConfig = {
  title: 'Order',
  // Пользователь не добавил поле 'body'
  buttonText: 'Send',
  cancellable: true
};

function createMenu(config) {
  config = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // config теперь равен: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**


### Не используйте флаги в качестве параметров функции
Флаги говорят о том, что эта функция делает больше чем одну вещь. Функции должны делать только одно. Разделите вашу функцию, если, основываясь на логическом параметре, она исполняет различный код.

**Плохо:**
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Хорошо:**
```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Избегайте побочных эффектов (часть первая)
Функция производит побочный эффект, если она делает что-либо, кроме как принимает значение и возвращает другое значение. Побочным эффектом может быть запись в файл, изменение глобальной переменной или случайный перевод всех своих денег на незнакомца.

Однако в определенных случаях вам потребуются побочные эффекты. Как и в предыдущем примере, вам может понадобиться запись в файл. В таком случае, вам нужно централизовать, где вы будете это делать. Не нужно создавать несколько функций и классов для записи в конкретные файлы. Должен быть один сервис, делающий это. Один и только один.

Главное здесь - избежать распространенных ошибок: разделения состояния между объектами без какой-либо структуры, использование изменяемых типов данных, которые могут быть переопределены кем угодно, отсутствие централизованного места возникновения ваших побочных эффектов. Если вы сможете сделать это, вы станете счастливее, чем подавляющее большинство других программистов.

**Плохо:**
```javascript
// Глобальную переменную использует следующая за ней функция.
// Функция превращает переменную в массив и если бы у нас была еще одна функция, использующая эту же переменную, то это могло бы привести к ее поломке.
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Хорошо:**
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Избегайте побочных эффектов (часть вторая)
В JavaScript примитивы передаются по значению, а объекты/массивы передаются по ссылке. В случае объектов и массивов, если ваша функция вносит изменения в массив корзины покупок, например, путем добавления товара, то любая другая функция, которая использует этот массив `cart`, будет зависеть от этого дополнения. Это в равной мере может быть хорошо и плохо. Давайте представим себе плохой сценарий.

Пользователь нажимает на "Купить" - кнопку, вызывающую функцию `purchase`, которая создает сетевой запрос и отправляет массив `cart` на сервер. Из-за плохого подключения к сети функция `purchase` вынуждена повторно пытаться отправить запрос. А что если в то же время пользователь случайно нажимает кнопку "Добавить в корзину" на товаре, который он не хотел покупать до того, как начался сетевой запрос? Если это произойдет, а сетевой запрос уже начался, то функция покупки пошлет на сервер случайно добавленный товар, поскольку она имеет ссылку на массив корзины покупок, который функция `addItemToCart` модифицировала путем добавления нежелательного товара.

Отличным решением было бы, чтобы функция `addItemToCart` всегда клонировала массив `cart`, редактировала его и возвращала отредактированный клон. Это бы гарантировало, что никакие другие функции, использующие ссылку на массив корзины покупок, не будут затронуты какими-либо изменениями.

Два предостережения:
1. Могут быть случаи, когда вы на самом деле хотите изменить входящий объект, но когда вы привыкнете к такому подходу программирования, то обнаружите, что эти случаи довольно редки. Большую часть логики можно переделать так, чтобы побочных эффектов не было совсем!
2. Клонирование больших объектов может быть очень дорогими с точки зрения производительности. К счастью,
это не является большой проблемой на практике, потому что существуют [отличные библиотеки] (https://facebook.github.io/immutable-js/), которые позволяют такому подходу программирования быть быстрым и не настолько затратным по памяти, как это было бы если бы вы вручную клонировали объекты и массивы.

**Плохо:**
```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Хорошо:**
```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date : Date.now() }];
};
```

**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Не записывайте в глобальные объекты
Загрязнение глобального объекта - плохая практика в JavaScript, потому что вы можете начать конфликтовать с другой библиотекой, а пользователь вашего API останется в замешательстве даже после получения исключения в режиме production.
Давайте поразмышляем о примере: что делать, если вы хотите расширить глобальный объект `Array`, чтобы он имел метод `diff`, показывающий различие между двумя массивами? Вы могли бы написать новый метод к `Array.prototype`, но он может начать конфликтовать с другой библиотекой, пытающейся сделать то же самое. Что, если эта другая библиотека с помощью `diff` показывает различие не между двумя массивами, а между первым и последним элементами массива? Вот почему было бы гораздо лучше использовать ES2015/ES6 классы и расширить глобальный объект `Array`.

**Плохо:**
```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Хорошо:**
```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Функциональное программирование предпочтительней императивного
JavaScript не такой функциональный язык как Haskell, но у него есть предрасположенность к этому. Функциональные языки чище и их проще тестировать. Предпочитайте этот стиль программирования, когда можете.

**Плохо:**
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**Хорошо:**
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

const INITIAL_VALUE = 0;

const totalOutput = programmerOutput
  .map((programmer) => programmer.linesOfCode)
  .reduce((acc, linesOfCode) => acc + linesOfCode, INITIAL_VALUE);
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Инкапсулируйте условия

**Плохо:**
```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  // ...
}
```

**Хорошо:**
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Избегайте негативных условий

**Плохо:**
```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Хорошо:**
```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Избегайте условий
Это кажется невозможной задачей. Большинство людей, впервые услышав это, говорят: "Как я должен делать что-либо без выражения `if`?". Ответ в том, что во многих случаях для достижения той же цели вы можете использовать полиморфизм. Как правило, далее следует вопрос: "Хорошо, это здорово, но почему я должен следовать этому?". Ответ в одном из предыдущих принципах Чистого кода: функция должна делать только одну вещь. Если у вас есть классы и функции, которые имеют выражения `if`, вы признаетесь своему пользователю, что ваша функция делает больше, чем одну вещь. Запомните, делать только одну вещь.

**Плохо:**
```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**Хорошо:**
```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Избегайте проверки типов (часть первая)
JavaScript - слабо типизированный язык программирования - ваши функции могут принимать аргументы любого типа. Иногда такая свобода играет против вас и велик соблаз ввести в функции проверку типов. Есть много способов избежать этого. Первый - уплотнить API.

**Плохо:**
```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}
```

**Хорошо:**
```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Избегайте проверки типов (часть вторая)
Если вы работаете с базовыми примитивными значениями, как строки, числа и массивы, и вы не можете использовать полиморфизм, но вы все еще нуждаетесь в проверке типов, вы должны задуматься об использовании TypeScript. Это отличная альтернатива обычному JavaScript, так как он предоставляет вам статическую типизацию поверх стандартного JavaScript
синтаксиса. Проблема ручной проверки типов JavaScript в том, что, если делать ее хорошо, она излишне многословна и получаемая вами безопасность не компенсирует потерянную читаемость. Держите JavaScript в чистоте, пишите хорошие тесты и проводите качественное рецензирование кода. В противном случае, делайте все необходимые проверки, но с TypeScript (который, как я уже сказал, - отличная альтернатива!).

**Плохо:**
```javascript
function combine(val1, val2) {
  if (typeof val1 === 'number' && typeof val2 === 'number' ||
      typeof val1 === 'string' && typeof val2 === 'string') {
    return val1 + val2;
  }

  throw new Error('Must be of type String or Number');
}
```

**Хорошо:**
```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Не оптимизируйте чрезмерно
Под капотом современные браузеры осуществляют большой объем оптимизации во время выполнения кода. В большинстве случаев, если вы занимались оптимизацией, вы попусту потратили свое время. [Есть хорошие ресурсы] (https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) для обнаружения нехватки оптимизации. Используйте их до того момента, пока ситуация не изменится.

**Плохо:**
```javascript

// В старых браузерах каждая итерация с незакешированным `list.length` - дорогостоящая.
// Дело в постоянном перерасчете `list.length`. В современных браузерах это оптимизировано.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Хорошо:**
```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Удаляйте мертвый код
Мертвый код - так же плохо, как повторяющийся код. Нет никаких оснований продолжать хранить его в кодовой базе. Если он не используется, избавьтесь от него! В случае надобности, его всегда можно найти в истории версий.

**Плохо:**
```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');

```

**Хорошо:**
```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

## **Объекты и структуры данных**
### Используйте геттеры и сеттеры
Использовать геттеры и сеттеры для доступа к данным объекта гораздо лучше, чем напрямую обращаться к его свойствам. Почему? Вот список причин:

* Если вы хотите сделать больше, чем просто получить свойство объекта.
* Делает добавление валидации при выполнении `set` элементарным.
* Инкапсулирует внутреннее представление.
* При получении и добавлении легко внедрить логирование и обработку ошибок.
* Вы можете использовать ленивую загрузку свойств вашего объекта, скажем, получая их с сервера.

**Плохо:**
```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0,
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**Хорошо:**
```javascript
function makeBankAccount() {
  // приватная переменная
  let balance = 0;

  // геттер является публичным, так как возвращается в объекте ниже
  function getBalance() {
    return balance;
  }

  // сеттер является публичным, так как возвращается в объекте ниже
  function setBalance(amount) {
    // ... валидация перед обновлением баланса
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance,
  };
}

const account = makeBankAccount();
account.setBalance(100);
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**


### Создавайте в объектах приватные поля
Это может быть достигнуто по средством замыканий (для версии ES5 и ниже).

**Плохо:**
```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    },
  };
}

const employee = makeEmployee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**Хорошо:**
```javascript
const Employee = function (name) {
  this.getName = function getName() {
    return name;
  };
};

const employee = new Employee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Имя сотрудника: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Имя сотрудника: John Doe
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**


## **Классы**
### Принцип единственной ответственности (SRP)
Чистый код декларирует: "Не должно быть более чем одной причины для изменения класса". Заманчиво представить себе класс, переполненный большим количеством функционала, словно в поездку вам позволили взять всего один чемодан. Проблема в том, что ваш класс не будет концептуально единым и это даст ему множество причин для изменения. Имеет огромное значение свести к минимуму количество таких причин. Если сосредоточить слишком много функциональности в одном классе, а затем попытаться изменить его часть, то спрогнозировать, как это может сказаться на других модулях системы, станет крайне сложно.

**Плохо:**
```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Хорошо:**
```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}


class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Принцип открытости/закрытости (OCP)
Как заявил Бертран Мейер, программные сущности (классы, модули, функции и т.д.) должны оставаться открытыми для расширения, но закрытыми для модификации. Что это означает на практике? Принцип закрепляет, что вы должны позволить пользователям добавлять новые функциональные возможности, но без изменения существующего кода.

**Плохо:**
```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'ajaxAdapter';
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'nodeAdapter';
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === 'ajaxAdapter') {
      return makeAjaxCall(url).then((response) => {
        // трансформируем ответ и возвращаем
      });
    } else if (this.adapter.name === 'httpNodeAdapter') {
      return makeHttpCall(url).then((response) => {
        // трансформируем ответ и возвращаем
      });
    }
  }
}

function makeAjaxCall(url) {
  // делаем запрос и возвращаем Промис
}

function makeHttpCall(url) {
  // делаем запрос и возвращаем Промис
}
```

**Хорошо:**
```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'ajaxAdapter';
  }

  request(url) {
    // делаем запрос и возвращаем Промис
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'nodeAdapter';
  }

  request(url) {
    // делаем запрос и возвращаем Промис
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then((response) => {
      // трансформируем ответ и возвращаем
    });
  }
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**


### Принцип подстановки Барбары Лисков (LSP)
Это страшный термин для очень простой концепции. Формальным языком он звучит следующим образом: "Если S является подтипом T, то объекты типа Т могут быть заменены на объекты типа S (то есть, объекты типа S могут заменить объекты типа Т) без влияния на важные свойства программы (корректность, пригодность для выполнения задач и т.д.). И да, в итоге определение получилось еще страшней.

Лучшее объяснение заключается в том, что если у вас есть родительский и дочерний классы, то они могут использоваться как взаимозаменяемые, не приводя при этом к некорректным результатам. Это по-прежнему может сбивать с толку, так что давайте взглянем на классический пример квадрата-прямоугольника. Математически квадрат представляет собой прямоугольник, но если вы смоделируете их отношения через наследование ("является разновидностью"), вы быстро наткнетесь на неприятности.

**Плохо:**
```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach((rectangle) => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // ПЛОХО: Вернет 25 для Квадрата. Должно быть 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Хорошо:**
```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach((shape) => {
      const area = shape.getArea();
      shape.render(area);
    });
  }

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Принцип разделения интерфейса (ISP)
JavaScript не имеет интерфейсов, так что этот принцип не применяется так строго, как другие. Тем не менее, это важно и актуально даже в виду их отсутствия. Принцип утверждает, что клиенты не должны зависеть от интерфейсов, которые они не используют. Из-за утиной типизации, в JavaScript интерфейсы - это неявные контракты.

Хорошим примером станут классы, предусматривающие множество настроек для объектов. Полезно не требовать от клиентов проставления их всех, потому что большую часть времени все они не требуются. Создание опциональных настроек помогает предотвратить разбухание интерфейса.

**Плохо:**
```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  animationModule() {} // В большинстве случаев анимация нам не пригодится.
  // ...
});

```

**Хорошо:**
```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  options: {
    animationModule() {}
  }
});
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Принцип инверсии зависимостей (DIP)
Этот принцип закрепляет две важные вещи:
1. Модули верхнего уровня не должны зависеть от модулей низкого уровня. И те, и другие должны зависеть от абстракций.
2. Абстракции не должны зависеть от деталей. Детали должны зависеть от абстракций.

На первый взгляд это кажется трудным, но если вы работали с Angular.js, вы видели реализацию этого принципа в виде внедрения зависимостей (Dependency Injection - DI). Несмотря на то, что DIP и DI - понятия не идентичные, DIP оберегает модули верхнего уровня от деталей модулей низкого уровня, а сделать это он может через DI. Огромное преимущество DIP в уменьшении взаимосвязей между модулями. Переплетение модулей - это антипаттерн, потому что оно делает рефакторинг вашего кода гораздо более трудоемким.

Как было сказано выше, JavaScript не имеет интерфейсов, так что абстракции зависят от неявных контрактов. То есть, от методов и свойств, которые объект/класс предоставляет другому объекту/классу. В приведенном ниже примере, неявный контракт в том, что любой модуль запроса для `InventoryTracker` будет иметь метод `requestItems`.

**Плохо:**
```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // ПЛОХО: Мы создали зависимость от конкретной реализации запроса.
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(['apples', 'bananas']);
inventoryTracker.requestItems();
```

**Хорошо:**
```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ['WS'];
  }

  requestItem(item) {
    // ...
  }
}

// Построив наши зависимости извне и внедряя их, мы можем легко
// заменить наш модуль запроса на модный, например, использующий вебсокеты.
const inventoryTracker = new InventoryTracker(['apples', 'bananas'], new InventoryRequesterV2());
inventoryTracker.requestItems();
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Классы ES2015/ES6 предпочтительней функций ES5
При классическом подходе к классам в ES5 очень трудно добиться читаемого наследования, конструктора и определения методов. Если вам нужно наследование (будьте уверены, что вероятней всего нет), то лучше отдать предпочтение классам ES2015/ES6. Тем не менее, предпочитайте маленькие функции перед классами, пока вы не столкнетесь с необходимостью в более крупных и сложных объектах.

**Плохо:**
```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error('Instantiate Animal with `new`');
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error('Instantiate Mammal with `new`');
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error('Instantiate Human with `new`');
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**Хорошо:**
```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() { /* ... */ }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() { /* ... */ }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() { /* ... */ }
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**


### Используйте цепочки методов
Это очень полезный паттерн в JavaScript и вы встретите его во многих библиотеках, например, JQuery и Lodash. Он делает ваш код выразительным и менее многословным. Стройте цепочки методов и вы увидете, на сколько чище становится ваш код. Метод вашего класса должен просто возвращать `this` в конце своего вызова и вы сможете присоединить к нему вызов следующего метода.

**Плохо:**
```javascript
class Car {
  constructor() {
    this.make = 'Honda';
    this.model = 'Accord';
    this.color = 'white';
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car();
car.setColor('pink');
car.setMake('Ford');
car.setModel('F-150');
car.save();
```

**Хорошо:**
```javascript
class Car {
  constructor() {
    this.make = 'Honda';
    this.model = 'Accord';
    this.color = 'white';
  }

  setMake(make) {
    this.make = make;
    // ПРИМЕЧАНИЕ: Возвращаем this для построения цепочки
    return this;
  }

  setModel(model) {
    this.model = model;
    // ПРИМЕЧАНИЕ: Возвращаем this для построения цепочки
    return this;
  }

  setColor(color) {
    this.color = color;
    // ПРИМЕЧАНИЕ: Возвращаем this для построения цепочки
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // ПРИМЕЧАНИЕ: Возвращаем this для построения цепочки
    return this;
  }
}

const car = new Car()
  .setColor('pink')
  .setMake('Ford')
  .setModel('F-150')
  .save();
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Предпочитайте композицию перед наследованием
Как верно замечено в [*Design Patterns*](https://ru.wikipedia.org/wiki/Design_Patterns) под авторством Банды Четырех (Gang of Four), когда это возможно, вы должны отдавать предпочтение композиции перед наследованием. Есть много хороших причин для использования наследования и много хороших причин для применения композиции. Главное, что если ваш ум инстинктивно идет путем наследования, подумайте, может быть композиция способна решить вашу проблему лучше. В ряде случаев это именно так.

Возможно вы задаетесь вопросом: "Когда я должен использовать наследование?". Это зависит от вашей проблемы, но вот конкретный список, когда наследование имеет больше смысла, чем композиция:

1. Ваше наследование представляет собой отношения "является разновидностью", а не "включает в себя" (Человек->Животное против Пользователь->Детали_Пользователя).
2. Вы можете повторно использовать код из базовых классов (люди могут двигаться как и все животные).
3. Вы хотите сделать глобальные изменения в производных классах путем изменения базового класса (изменение расхода калорий всех животных во время движения).

**Плохо:**
```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Плохо, потому что Employee (Сотрудник) "имеет" налоговые данные.
// EmployeeTaxData (Налоговые_данные_сотрудника) не являются типом Employee (Сотрудника).
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Хорошо:**
```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

## **Тестирование**
Тестирование важнее деплоя. Если у вас нет тестов или их мало, то каждый раз при выкладке кода на боевые сервера у вас нет уверенности, что ничего не сломалось. Решение о достаточном количестве тестов остается на совести вашей команды, но 100% покрытие тестами всех выражений и ветвлений обеспечивает высокое доверие к вашему коду и спокойствие всех разработчиков. Из этого следует, что в дополнение к отличному фреймворку для тестирования, необходимо также использовать [хороший инструмент покрытия](http://gotwarlost.github.io/istanbul/).

Нет оправдания не писать тесты. В JavaScript cуществует [множество хороших тестовых фреймворков](http://jstherightway.org/#testing-tools), так что найдите подходящий для вас. А когда найдете, то стремитесь писать тесты для каждой новой фичи или нового модуля. Замечательно, если вы предпочитаете метод Разработки через тестирование (TDD), но главное убедиться, что перед запуском любой новой фичи или рефакторинга существующей вы достигаете достаточного уровня покрытия тестами.

### Один кейс на один тест

**Плохо:**
```javascript
const assert = require('assert');

describe('MakeMomentJSGreatAgain', () => {
  it('handles date boundaries', () => {
    let date;

    date = new MakeMomentJSGreatAgain('1/1/2015');
    date.addDays(30);
    assert.equal('1/31/2015', date);

    date = new MakeMomentJSGreatAgain('2/1/2016');
    date.addDays(28);
    assert.equal('02/29/2016', date);

    date = new MakeMomentJSGreatAgain('2/1/2015');
    date.addDays(28);
    assert.equal('03/01/2015', date);
  });
});
```

**Хорошо:**
```javascript
const assert = require('assert');

describe('MakeMomentJSGreatAgain', () => {
  it('handles 30-day months', () => {
    const date = new MakeMomentJSGreatAgain('1/1/2015');
    date.addDays(30);
    assert.equal('1/31/2015', date);
  });

  it('handles leap year', () => {
    const date = new MakeMomentJSGreatAgain('2/1/2016');
    date.addDays(28);
    assert.equal('02/29/2016', date);
  });

  it('handles non-leap year', () => {
    const date = new MakeMomentJSGreatAgain('2/1/2015');
    date.addDays(28);
    assert.equal('03/01/2015', date);
  });
});
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

## **Асинхронность**
### Используйте Промисы, а не Callback-функции
Callback-функции ухудшают читаемость и приводят к чрезмерному количеству вложенности. В ES2015/ES6 Промисы - встроенный глобальный тип. Используйте их!

**Плохо:**
```javascript
require('request').get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', (requestErr, response) => {
  if (requestErr) {
    console.error(requestErr);
  } else {
    require('fs').writeFile('article.html', response.body, (writeErr) => {
      if (writeErr) {
        console.error(writeErr);
      } else {
        console.log('File written');
      }
    });
  }
});

```

**Хорошо:**
```javascript
require('request-promise').get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((response) => {
    return require('fs-promise').writeFile('article.html', response);
  })
  .then(() => {
    console.log('File written');
  })
  .catch((err) => {
    console.error(err);
  });

```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Async/Await даже чище, чем Промисы
Промисы - очень чистая альтернатива callback-функциям, но ES2017/ES8 привносит async и await с еще более чистым решением. Все, что вам нужно, это функция с ключевым словом `async`, после чего вы можете писать логику императивно - без цепочек `then`. Если уже сегодня вы можете внедрить фичи ES2017/ES8, используйте `async/await`!

**Плохо:**
```javascript
require('request-promise').get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((response) => {
    return require('fs-promise').writeFile('article.html', response);
  })
  .then(() => {
    console.log('File written');
  })
  .catch((err) => {
    console.error(err);
  });

```

**Хорошо:**
```javascript
async function getCleanCodeArticle() {
  try {
    const response = await require('request-promise').get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin');
    await require('fs-promise').writeFile('article.html', response);
    console.log('File written');
  } catch(err) {
    console.error(err);
  }
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

## **Отлавливание ошибок**
Выброшенные ошибки - отличная штука! Они означают, что когда во время выполнения вашей программы что-то пошло не так, это было успешно зафиксировано и донесено до вас путем остановки выполнения функции, убийства процесса и уведомления в консоль с трассировкой стека.

### Не игнорируйте пойманные ошибки
Игнорирование пойманной ошибки не дает вам возможности исправить или каким-либо образом отреагировать на ее появление. Логирование ошибок в консоль (`console.log`) не намного лучше, так как зачастую оно может потеряться в море консольных записей. Оборачивание куска кода в `try/catch` означает, что вы предполагаете возможность появления ошибки и имеете на этот случай четкий план.

**Плохо:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Хорошо:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // Один из вариантов (более навязчивый, чем console.log):
  console.error(error);
  // Другой вариант:
  notifyUserOfError(error);
  // Другой вариант:
  reportErrorToService(error);
  // Или используйте все три!
}
```

### Не игнорируйте выполненные с ошибкой (rejected) Промисы
Вы не должны игнорировать ошибки в Промисах по той же причине, что и в `try/catch`.

**Плохо:**
```javascript
getdata()
.then((data) => {
  functionThatMightThrow(data);
})
.catch((error) => {
  console.log(error);
});
```

**Хорошо:**
```javascript
getdata()
.then((data) => {
  functionThatMightThrow(data);
})
.catch((error) => {
  // Один из вариантов (более навязчивый, чем console.log):
  console.error(error);
  // Другой вариант:
  notifyUserOfError(error);
  // Другой вариант:
  reportErrorToService(error);
  // Или используйте все три!
});
```

**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

## **Форматирование**
Форматирование носит субъективный характер. Как и во многом собранном здесь, в вопросе форматирования нет жестких правил, которым вы обязаны следовать. Главное, не тратить время на споры о нем. Есть [тонны инструментов](http://standardjs.com/rules.html) для автоматизации этого процесса. Выберите один! Споры о форматировании - пустая трата времени и денег.

Для случаев, не подходящих для автоматического форматирования (отступы, табуляция или пробелы, двойные кавычки против одинарных и т.д.), в данном руководстве содержатся лучшие практики.

### Будьте последовательны в капитализации
JavaScript - нетипизированный язык, поэтому капитализация говорит вам многое о ваших переменных, функциях и т.д. Правила носят субъективный характер, ваша команда может выбрать любые. Главное, независимо от того, что вы выбрали, быть последовательными.

**Плохо:**
```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Хорошо:**
```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Вызывающая и вызываемая функции должны быть рядом
Если функция вызывает другую, сохраняйте эти функции вертикально рядом в исходном файле. В идеале, держите вызывающую функцию прямо над вызываемой. Мы склонны читать код сверху-внизу, как газету. Поэтому подготавливайте ваш код для восприятия таким образом.

**Плохо:**
```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers');
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(user);
review.perfReview();
```

**Хорошо:**
```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers');
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

## **Комментарии**
### Комментируйте только бизнес-логику
Комментарии - оправдания и не являются обязательным требованием. Хороший код *в основном* документирует себя сам.

**Плохо:**
```javascript
function hashIt(data) {
  // Хэш
  let hash = 0;

  // Длина строки
  const length = data.length;

  // Цикл через каждый символ в данных
  for (let i = 0; i < length; i++) {
    // Получаем код символа
    const char = data.charCodeAt(i);
    // Создаем хэш
    hash = ((hash << 5) - hash) + char;
    // Преобразуем в 32-битное целое число
    hash &= hash;
  }
}
```

**Хорошо:**
```javascript

function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = ((hash << 5) - hash) + char;

    // Преобразуем в 32-битное целое число
    hash &= hash;
  }
}

```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Не оставляйте закомментированный код в вашей кодовой базе
Системы контроля версий существуют не зря. Оставьте старый код в истории.

**Плохо:**
```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Хорошо:**
```javascript
doStuff();
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Не заводите журнальных комментариев
Не забывайте использовать системы контроля версий! Нет необходимости в мертвом коде, закоментированном коде и особенно в журнальных комментариях. Используйте `git log`, чтобы получить историю!

**Плохо:**
```javascript
/**
 * 2016-12-20: Удалены монады, не понимал их (RM)
 * 2016-10-01: Улучшено использование специальных монады (JP)
 * 2016-02-03: Исключена проверка типов (LI)
 * 2015-03-14: Добавлен combine с проверкой типов (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**Хорошо:**
```javascript
function combine(a, b) {
  return a + b;
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Избегайте маркеров позиционирования
Они, как правило, просто добавляют шум. Пусть функции и имена переменных вместе с правильными отступами и форматированием задают визуальную структуру кода.

**Плохо:**
```javascript
////////////////////////////////////////////////////////////////////////////////
// Создание объекта
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

////////////////////////////////////////////////////////////////////////////////
// Установка экшена
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**Хорошо:**
```javascript
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

const actions = function() {
  // ...
};
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

## Переводы

Данное руководство также доступно на других языках:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Бразильский португальский**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Китайский**: [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **Немецкий**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Корейский**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)

**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**
