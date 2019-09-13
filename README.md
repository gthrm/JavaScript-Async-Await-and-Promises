### ВВЕДЕНИЕ

- Любой, кто называет себя разработчиком JavaScript, в какой-то момент должен был работать с *функциями обратного вызова*, **Promises** или, с недавних пор, с синтаксисом **Async / Await**.
- Если вы "пробыли в игре достаточно долго", вы, вероятно, помните времена, когда функции обратного вызовы были единственным способом выполнения асинхронного JavaScript.
- Когда я впервые начал изучать и писать JavaScript, уже существовало миллиард руководств и руководств, объясняющих, как их использовать. Тем не менее, многие из них просто объясняли, как конвертировать обратные вызовы в **Promises** или Promises в **Async / Await**.
- В целом, для многих этого, вероятно, более чем достаточно, чтобы они могли писать код.
- Однако, если вы похожи на меня и действительно хотите понять асинхронное программирование (а не только синтаксис JavaScript!), То, возможно, вы согласитесь со мной, что существует нехватка контента, фактически объясняющего асинхронное программирование с нуля.

----------

# Что значит «асинхронный»?
![](http://blog.j7mbo.com/content/images/2018/08/async.jpg)

##### Как правило, ответы, которые вы получите, задавая этот вопрос, состоят из следующих строк:
- Несколько потоков, выполняющих код одновременно
- Более чем один бит кода выполняется одновременно
- Параллелизм (?)
- И все это будет в некотором роде в правильном направлении. Но вместо того, чтобы дать вам техническое определение, которое вы, вероятно, забудете позже, я приведу вам пример, который пятилетний ребенок мог понять.

---------

# Более человеческая аналогия

- Представь, что ты готовишь овощной суп. Для хорошей и простой аналогии предположим, что суп овощной, и состоит только из лука и моркови. Рецепт такого супа может быть следующим:
                
1. Нарезать морковь.
2. Нарезать лук.
3. Добавьте воду в кастрюлю, включите плиту и подождите, пока она закипит.
4. Добавьте морковь в кастрюлю и оставьте на 5 минут.
5. Добавить лук в кастрюлю и варить еще 10 минут.

- Эти инструкции просты и понятны, но если кто-то из вас, читая это, действительно готовит, вы поймете, что это не самый эффективный способ приготовления для тех из вас, кто не является опытным поваром, и вот почему:
    - Шаги 3, 4 и 5 фактически не требуют от вас как от шеф-повара ничего делать, кроме как наблюдать за потом и следить за временем.
    - Шаги 1 и 2 требуют, чтобы вы активно что-то делали.

Следовательно, более опытный повар может сделать следующее:
- Начать кипятить кастрюлю с водой
- В ожидании кипения кастрюли с водой начните измельчать морковь.
- К тому времени, когда вы закончите измельчать морковь, вода должна закипеть, поэтому добавьте морковь.
- Пока морковь готовится в кастрюле, нарежьте лук.
- Добавьте лук и готовьте еще 10 минут.
Несмотря на то, вы сделали то же самое, вы увидете, что это будет намного быстрее и эффективнее.

Это точно такая же концепция, что и асинхронное программирование: вам никогда не захочется сидеть сложа руки, просто ожидая чего-то, в то время как есть другие вещи, на которые вы можете потратить свои усилия.
И все мы знаем, что в программировании ожидание происходит довольно часто - будь то ожидание HTTP-ответа от сервера, файловая система ввода-вывода и т. Д. Но циклы выполнения вашего ЦП очень важны, и их всегда следует активно потратить на выполнение чего-либо, и не дожидаясь: отсюда асинхронное программирование.

---------

# Теперь давайте перейдем к реальному JavaScript, не так ли?

Итак, придерживаясь того же примера овощного супа, я определю несколько функций для действий, описанных выше.
Сначала давайте определим наши обычные синхронные функции, представляющие задачи, которые вообще не требуют ожидания.
Это ваши хорошие старые функции JavaScript, хотя обратите внимание, что я имитировал `chopCarrots` и `chopOnions` как задачи, требующие активной работы (и требующие времени), позволим им выполнять некоторые "дорогостоящие вычисления". Я закомментировал фактические реализации, поскольку они не имеют большого значения.

```javascript
function chopCarrots() {
  /**
   * Some long computational task here...
   */
  console.log("Done chopping carrots!");
}

function chopOnions() {
  /**
   * Some long computational task here...
   */
  console.log("Done chopping onions!");
}

function addOnions() {
  console.log('Add Onions to pot!');
}

function addCarrots() {
  console.log('Add Carrots to pot!');
}
```

Что касается асинхронных функций, сначала я быстро объясню, как система типов JavaScript обрабатывает асинхронность: **в основном все результаты (включая недействительные) от асинхронных операций должны быть заключены в тип Promise**.
Чтобы функция возвращала Promise, вы можете либо
- Явно вернуть Обещание, т.е. вернуть `new Promise(…..)`;
- Добавить асинхронную подпись функции
- или оба вышеперечисленных.
По причинам ² я не буду вдаваться в эту статью, но вы всегда должны использовать ключевое слово `async` в асинхронных функциях.
Итак, для наших асинхронных функций (представляющих шаги 3–5 приготовления овощного супа):

```javascript
async function letPotKeepBoiling(time) {
    return /* A promise to let the pot keep boilng for certain time */
}

async function boilPot() {
    return /* A promise to let the pot boil for time */
}
```

Опять же, я удалил детали реализации, но я опубликую их в конце, если вы хотите их увидеть.
Также важно знать, что для того, чтобы дождаться результата обещания, чтобы вы могли что-то с ним сделать, вы можете просто использовать ключевое слово `await`:

```javascript
async function asyncFunction() {
  /* Returns a promise... */
}

result = await asyncFunction();
```

```javascript
function makeSoup() {
    const pot = boilPot();
    chopCarrots();
    chopOnions();
    await pot;
    addCarrots();
    await letPotKeepBoiling(5);
    addOnions();
    await letPotKeepBoiling(10);
    console.log("Your vegetable soup is ready!");
}

makeSoup();
```

Но стоп! Это не работает! Вы получите SyntaxError: `await` действителен только в асинхронных функциях. Почему? Потому что, если вы не объявляете функцию как асинхронную, то по умолчанию JavaScript принимает это как означающее, что это синхронная функция - а синхронный означает отсутствие ожидания!³
(Это также означает, что вы не можете использовать `await` в скрипте верхнего уровня за пределами функции вообще).
Следовательно, мы просто добавляем ключевое слово `async` в функцию `makeSoup`:

```javascript
async function makeSoup() {
    const pot = boilPot();
    chopCarrots();
    chopOnions();
    await pot;
    addCarrots();
    await letPotKeepBoiling(5);
    addOnions();
    await letPotKeepBoiling(10);
    console.log("Your vegetable soup is ready!");
}

makeSoup();
```

И вуаля! Обратите внимание, что в строке ***2*** я вызываю асинхронную функцию `boilPot()` без ключевого слова `await`, потому что мы на самом деле не хотим ждать, пока горшок закипит, прежде чем начать измельчать морковь.
Мы ждем горшок **Promise(d)** только, прежде чем нам нужно будет положить в него морковь , поскольку мы не хотим делать это до того, как вода закипит.

Что происходит во время ожидания вызова? Ну, ничего ... вроде ...

В контексте функции `makeSoup` вы можете просто думать о ней как о том, что вы ожидаете, что что-то произойдет (или результат, который в конечном итоге будет возвращен).
Но помните: вы **(процессор)**, вы никогда не хотите просто сидеть и ждать чего-то, в то время как есть другие вещи, на которые вы можете потратить свои усилия.
Следовательно, вместо того, чтобы просто делать суп, мы можем делать что-то вроде этого:

```javascript
makeSoup();
makePasta();
```

Тогда, пока мы ждем `letPotKeepBoiling`, мы могли бы также готовить макароны.
Видите? **Async / Await** на самом деле довольно прост в использовании, если вы понимаете это, не так ли?

--------

# Как насчет явного Promise?

Хорошо, если вы настаиваете, я также перейду к использованию **Promise**. Имейте в виду, что методы **async / await** основаны на самих обещаниях и, следовательно, оба метода полностью совместимы.

Явные обещания, на мой взгляд, на полпути между использованием обратных вызовов старого стиля и новым **async/await** синтаксисом.
В качестве альтернативы, вы также можете думать **async/await** синтаксисе как о не более чем неявных обещаниях. В конце концов, **async / await** пришел после **Promises**, который сам пришел после обратных вызовов.

# Итак, поситим нашу машину времени, чтобы вернуться в ад… ⁴

```javascript
function callbackHell() {
  boilPot(() => {
    addCarrots();
    letPotKeepBoiling(
      () => {
        addOnions();
        letPotKeepBoiling(
          () => {
            console.log("Your vegetable soup is ready!");
          },
          1000,
        );
      },
      5000
    )
  },
  5000,
  chopCarrots(),
  chopOnions(),
  );
}
```

Я не буду врать, я написал это на лету, так как я просто писал эту статью, и мне понадобилось много времени, чтобы понять, что я сделал.
И многие из вас не будут знать, что вообще происходит. Мой дорогой друг, разве все эти обратные вызовы не ужасны? Пусть это будет уроком, чтобы никогда больше не трогать обратные вызовы ...
И, как и обещал (хе-хе), используя явные **Promise**:

```javascript
function makeSoup() {
  return Promise.all([
    new Promise((reject, resolve) => {
      chopCarrots();
      chopOnions();
      resolve();
    }),
    boilPot()
  ]).then(() => {
    addCarrots();
    return letPotKeepBoiling(5);
  }).then(() => {
    addOnions();
    return letPotKeepBoiling(10);
  }).then(() => {
    console.log("Vegetable soup done!");
  });
}
```

Как вы можете видеть, некоторые аспекты **Promise** все еще очень все еще очень похожи на callback.

Я не буду вдаваться в подробности, но в основном:
`.then` - это метод в **Promise**, который берет результат этого **Promise** и передает его в функцию аргумента (по сути, в функцию обратного вызова…)

Вы никогда не сможете развернуть результат **Promise** для использования вне контекста `.then` ⁵.

По сути, `.then` похож на `async` блок, который `await` (ожидает) результат, и затем передает его в обратный вызов.

Существуют также отклоненные обратные вызовы для обработки ошибок и метод `.catch` в **Promises** для обработки ошибок, но я не буду вдаваться в это, поскольку существует миллиард других учебных пособий **Promises**.

# Выводы

Я надеюсь, что вы получили некоторое представление об обещаниях и асинхронном программировании из этой статьи или, возможно, хотя бы узнали о хорошем способе объяснить это кому-то еще.

#### Итак, какой из них вы должны использовать? Явные **Promise** или **Async / Await**?

Ответ полностью зависит от вас - и я бы сказал, что смешивать их не так уж и плохо, так как оба полностью совместимы друг с другом.

Тем не менее, лично я нахожусь на 100% в **Async / Await**, так как для меня код намного яснее и более отражает истинную многозадачность асинхронного программирования.


[1]: Полный исходный код доступен [тут](https://gist.github.com/jackel119/b0599ff78e2a14b07439dd251dad464c#file-complete-js "тут").
[2]: Помсотри [тут](https://dev.to/mywebstuff_hq/async-function-vs-a-function-that-returns-a-promise-3lpo "тут").
[3]: Можно утверждать, что JavaScript, вероятно, может выводить **async/await** типы по телу функций и рекурсивной проверке, но JavaScript не был разработан для того, чтобы заботиться о безопасности статических типов во время компиляции, и не говоря уже о том, что он намного удобнее для разработчиков.
[4]: Я написал **async** функции, предполагая, что они работают под тем же интерфейсом обратного вызова, что и setTimeout. Обратите внимание, что обратные вызовы НЕ совместимы с Обещаниями и наоборот.
[5]: A Promise is a monad, for all you functional freaks out there!