# Макротасок не существует.

Один из самых частых вопросов на собеседованиях для frontend разработчиков: Расскажите про событийный цикл? как выполняются таски? что такое микротаски и макротаски?

В архитектуре event loop вообще нет такого слова как макротаски(macrotasks). Я вообще не смог найти ни одной спецификации, где было бы написано слово macrotask. Кроме [Promises/A+](https://promisesaplus.com/). Так в чем же разница между Promise и setTimeout? Почему Promise всегда(невсегда) будут исполняться в приоритете?

Браузер имеет несколько очередей задач (task queues) для разных типов тасок. Таска - это любой javascript код, запланированный стандартными механизмами, такие как запуск программы, запуск события или коллбэки. Помимо этого вы можете создать таску с помощью API, например WindowTimers(setTimeout, setInterval). Микротаски же в свою очередь такие же конструкции javascript, которые позволяют выполнять операции не дожидаясь запуска нового цикла event loop (process.nextTick, Promises, queueMicrotask). Так вот, так как setTimeout, setInterval относятся к браузерному API, то очередь микротасок, таких как Promise и т.д. всегда будет в приоритете выполнения, перед браузерным API.

При этом стоит учитывать, что браузерные API исполняют таски в разные очереди и по разному, например [MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver) среагировавший после того, как в очередь микротасок попал Promise.resolve() от функции fetch, будет выполнен раньше. То есть вставка в очередь тасок может быть не только как push.

#### Полезные материалы
1. [W3](https://www.w3.org/TR/2011/WD-html5-20110525/webappapis.html#task-queue)
2. [MDN Event Loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)
3. [Tasks, microtasks, queues and schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
4. [Филипп Робертс: Что за чертовщина такая event loop? | JSConf EU 2014](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
5. [Джейк Арчибальд. В цикле - JSConf.Asia](https://www.youtube.com/watch?v=cCOL7MC4Pl0)