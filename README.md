Оригинальный репозиторий: [ryanmcdermott/clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)

# clean-code-javascript

## Содержание
  1. [Введение](https://github.com/maksugr/clean-code-javascript#Введение)
  2. [Переменные](https://github.com/maksugr/clean-code-javascript#Переменные)
  3. [Функции](https://github.com/maksugr/clean-code-javascript#Функции)
  4. [Объекты и структуры данных](https://github.com/maksugr/clean-code-javascript#Объекты-и-структуры-данных)
  5. [Классы](https://github.com/maksugr/clean-code-javascript#Классы)
  6. [Тестирование](https://github.com/maksugr/clean-code-javascript#Тестирование)
  7. [Параллелизм](https://github.com/maksugr/clean-code-javascript#Параллелизм)
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

И еще. Знание этих принципов не сразу сделает вас лучшее как разработчиков программного обеспечения, а их использование в течение многих лет не гарантирует, что вы перестанете совершать ошибки. Каждый фрагмент кода как первый набросок, как мокрая глина принимает свою форму постепенно. Наконец, все мы пронизаны несовершенством, когда наш код просматривают коллеги. Не истязайте себя за первые, нуждающиеся в улучшении, наброски. Вместо этого истязайте свой код!

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

### Используйте аргументы по умолчанию вместо логических операторов

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

Ноль аргументов - идеальный случай, один или два аргумента - хорошо, трех аргументов нужно избегать. Большее количество аргументов должно быть объединено. Как правило, если у вас более двух аргументов, ваша функция пытается сделать слишком много. Для большинства случаев, где это простительно, в качестве аргумента будет достаточно объекта верхнего уровня.

Поскольку JavaScript позволяет создавать объекты на лету, без классов в качестве основы, вы можете использовать их, если нуждаетесь во множестве аргументов.

**Плохо:**
```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**Хорошо:**
```javascript
function createMenu(config) {
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
function showList(employees) {
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
    vehicle.peddle(this.currentLocation, new Location('texas'));
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
JavaScript не имеет интерфейсов или типов и у нас нет ключевых слов `public` и `private`, поэтому этот паттерн труднореализуем. Тем не менее, использовать геттеры и сеттеры для доступа к данным объекта гораздо лучше, чем просто обращаться к его свойствам. Вы могли бы спросить: "Почему?". Ну, вот список причин, почему:

* Если вы хотите сделать больше, чем просто получить свойство объекта.
* Делает добавление валидации при выполнении `set` элементарным.
* Инкапсулирует внутреннее представление.
* При получении и добавлении легко внедрить логирование и обработку ошибок.
* Наследовав класс, вы можете переопределить функциональность по умолчанию.
* Вы можете использовать ленивую загрузку свойств вашего объекта, скажем, получая их с сервера.

**Плохо:**
```javascript
class BankAccount {
  constructor() {
    this.balance = 1000;
  }
}

const bankAccount = new BankAccount();

// Покупаем обувь...
bankAccount.balance -= 100;
```

**Хорошо:**
```javascript
class BankAccount {
  constructor(balance = 1000) {
    this._balance = balance;
  }

  // Не обязательно делать префикс `get` или `set` чтобы это был геттер/сеттер
  set balance(amount) {
    if (verifyIfAmountCanBeSetted(amount)) {
      this._balance = amount;
    }
  }

  get balance() {
    return this._balance;
  }

  verifyIfAmountCanBeSetted(val) {
    // ...
  }
}

const bankAccount = new BankAccount();

// Покупаем обувь...
bankAccount.balance -= shoesPrice;

// Получаем баланс
let balance = bankAccount.balance;

```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**


### Создавайте в объектах приватные поля
Это может быть достигнуто по средством замыканий (для версии ES5 и ниже).

**Плохо:**
```javascript

const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Имя сотрудника: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Имя сотрудника: undefined
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
### Single Responsibility Principle (SRP)
As stated in Clean Code, "There should never be more than one reason for a class
to change". It's tempting to jam-pack a class with a lot of functionality, like
when you can only take one suitcase on your flight. The issue with this is
that your class won't be conceptually cohesive and it will give it many reasons
to change. Minimizing the amount of times you need to change a class is important.
It's important because if too much functionality is in one class and you modify a piece of it,
it can be difficult to understand how that will affect other dependent modules in
your codebase.

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

### Open/Closed Principle (OCP)
As stated by Bertrand Meyer, "software entities (classes, modules, functions,
etc.) should be open for extension, but closed for modification." What does that
mean though? This principle basically states that you should allow users to
add new functionalities without changing existing code.

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
        // transform response and return
      });
    } else if (this.adapter.name === 'httpNodeAdapter') {
      return makeHttpCall(url).then((response) => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
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
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'nodeAdapter';
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then((response) => {
      // transform response and return
    });
  }
}
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**


### Liskov Substitution Principle (LSP)
This is a scary term for a very simple concept. It's formally defined as "If S
is a subtype of T, then objects of type T may be replaced with objects of type S
(i.e., objects of type S may substitute objects of type T) without altering any
of the desirable properties of that program (correctness, task performed,
etc.)." That's an even scarier definition.

The best explanation for this is if you have a parent class and a child class,
then the base class and child class can be used interchangeably without getting
incorrect results. This might still be confusing, so let's take a look at the
classic Square-Rectangle example. Mathematically, a square is a rectangle, but
if you model it using the "is-a" relationship via inheritance, you quickly
get into trouble.

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
    const area = rectangle.getArea(); // BAD: Will return 25 for Square. Should be 20.
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
  constructor() {
    super();
    this.width = 0;
    this.height = 0;
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

class Square extends Shape {
  constructor() {
    super();
    this.length = 0;
  }

  setLength(length) {
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach((shape) => {
    switch (shape.constructor.name) {
      case 'Square':
        shape.setLength(5);
        break;
      case 'Rectangle':
        shape.setWidth(4);
        shape.setHeight(5);
    }

    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(), new Rectangle(), new Square()];
renderLargeShapes(shapes);
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Interface Segregation Principle (ISP)
JavaScript doesn't have interfaces so this principle doesn't apply as strictly
as others. However, it's important and relevant even with JavaScript's lack of
type system.

ISP states that "Clients should not be forced to depend upon interfaces that
they do not use." Interfaces are implicit contracts in JavaScript because of
duck typing.

A good example to look at that demonstrates this principle in JavaScript is for
classes that require large settings objects. Not requiring clients to setup
huge amounts of options is beneficial, because most of the time they won't need
all of the settings. Making them optional helps prevent having a "fat interface".

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
  animationModule() {} // Most of the time, we won't need to animate when traversing.
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

### Dependency Inversion Principle (DIP)
This principle states two essential things:
1. High-level modules should not depend on low-level modules. Both should
depend on abstractions.
2. Abstractions should not depend upon details. Details should depend on
abstractions.

This can be hard to understand at first, but if you've worked with Angular.js,
you've seen an implementation of this principle in the form of Dependency
Injection (DI). While they are not identical concepts, DIP keeps high-level
modules from knowing the details of its low-level modules and setting them up.
It can accomplish this through DI. A huge benefit of this is that it reduces
the coupling between modules. Coupling is a very bad development pattern because
it makes your code hard to refactor.

As stated previously, JavaScript doesn't have interfaces so the abstractions
that are depended upon are implicit contracts. That is to say, the methods
and properties that an object/class exposes to another object/class. In the
example below, the implicit contract is that any Request module for an
`InventoryTracker` will have a `requestItems` method.

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

    // BAD: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
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

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(['apples', 'bananas'], new InventoryRequesterV2());
inventoryTracker.requestItems();
```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Prefer ES2015/ES6 classes over ES5 plain functions
It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

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


### Use method chaining
This pattern is very useful in JavaScript and you see it in many libraries such
as jQuery and Lodash. It allows your code to be expressive, and less verbose.
For that reason, I say, use method chaining and take a look at how clean your code
will be. In your class functions, simply return `this` at the end of every function,
and you can chain further class methods onto it.

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
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
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

### Prefer composition over inheritance
As stated famously in [*Design Patterns*](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four,
you should prefer composition over inheritance where you can. There are lots of
good reasons to use inheritance and lots of good reasons to use composition.
The main point for this maxim is that if your mind instinctively goes for
inheritance, try to think if composition could model your problem better. In some
cases it can.

You might be wondering then, "when should I use inheritance?" It
depends on your problem at hand, but this is a decent list of when inheritance
makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a"
relationship (Human->Animal vs. User->UserDetails).
2. You can reuse code from the base classes (Humans can move like all animals).
3. You want to make global changes to derived classes by changing a base class.
(Change the caloric expenditure of all animals when they move).

**Плохо:**
```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
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
Testing is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you
didn't break anything. Deciding on what constitutes an adequate amount is up
to your team, but having 100% coverage (all statements and branches) is how
you achieve very high confidence and developer peace of mind. This means that
in addition to having a great testing framework, you also need to use a
[good coverage tool](http://gotwarlost.github.io/istanbul/).

There's no excuse to not write tests. There's [plenty of good JS test frameworks]
(http://jstherightway.org/#testing-tools), so find one that your team prefers.
When you find one that works for your team, then aim to always write tests
for every new feature/module you introduce. If your preferred method is
Test Driven Development (TDD), that is great, but the main point is to just
make sure you are reaching your coverage goals before launching any feature,
or refactoring an existing one.

### Single concept per test

**Плохо:**
```javascript
const assert = require('assert');

describe('MakeMomentJSGreatAgain', () => {
  it('handles date boundaries', () => {
    let date;

    date = new MakeMomentJSGreatAgain('1/1/2015');
    date.addDays(30);
    date.shouldEqual('1/31/2015');

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
    date.shouldEqual('1/31/2015');
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

## **Параллелизм**
### Use Promises, not callbacks
Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6,
Promises are a built-in global type. Use them!

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

### Async/Await are even cleaner than Promises
Promises are a very clean alternative to callbacks, but ES2017/ES8 brings async and await
which offer an even cleaner solution. All you need is a function that is prefixed
in an `async` keyword, and then you can write your logic imperatively without
a `then` chain of functions. Use this if you can take advantage of ES2017/ES8 features
today!

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
Thrown errors are a good thing! They mean the runtime has successfully
identified when something in your program has gone wrong and it's letting
you know by stopping function execution on the current stack, killing the
process (in Node), and notifying you in the console with a stack trace.

### Don't ignore caught errors
Doing nothing with a caught error doesn't give you the ability to ever fix
or react to said error. Logging the error to the console (`console.log`)
isn't much better as often times it can get lost in a sea of things printed
to the console. If you wrap any bit of code in a `try/catch` it means you
think an error may occur there and therefore you should have a plan,
or create a code path, for when it occurs.

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
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

### Don't ignore rejected promises
For the same reason you shouldn't ignore caught errors
from `try/catch`.

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
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
});
```

**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**


## **Форматирование**
Formatting is subjective. Like many rules herein, there is no hard and fast
rule that you must follow. The main point is DO NOT ARGUE over formatting.
There are [tons of tools](http://standardjs.com/rules.html) to automate this.
Use one! It's a waste of time and money for engineers to argue over formatting.

For things that don't fall under the purview of automatic formatting
(indentation, tabs vs. spaces, double vs. single quotes, etc.) look here
for some guidance.

### Use consistent capitalization
JavaScript is untyped, so capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

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


### Function callers and callees should be close
If a function calls another, keep those functions vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

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
### Only comment things that have business logic complexity.
Comments are an apology, not a requirement. Good code *mostly* documents itself.

**Плохо:**
```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = ((hash << 5) - hash) + char;
    // Convert to 32-bit integer
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

    // Convert to 32-bit integer
    hash &= hash;
  }
}

```
**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**

### Don't leave commented out code in your codebase
Version control exists for a reason. Leave old code in your history.

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

### Don't have journal comments
Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

**Плохо:**
```javascript
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
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

### Avoid positional markers
They usually just add noise. Let the functions and variable names along with the
proper indentation and formatting give the visual structure to your code.

**Плохо:**
```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
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

This is also available in other languages:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese**: [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)

**[⬆ Назад к Содержанию](https://github.com/maksugr/clean-code-javascript#Содержание)**
