## Hello World

Обычно программа «Hello world!» является первым шагом при изучении нового языка. Все знают, что это простая однострочная программа, которая выводит на экран фразу приветствия.

В мире GPU рисование текста - слишком сложная задача для первого шага. Вместо этого мы выберем яркий, жизнерадостный цвет!

<div class="codeAndCanvas" data="hello_world.frag"></div>

С кодом выше можно взаимодействовать, если вы читаете книгу в браузере. Это означает, что вы можете изменить любую часть кода по вашему желанию. Благодаря архитектуре GPU, которая компилирует и запускает шейдеры *на лету*, вы увидите изменения незамедлительно. Попробуйте изменить значения в строке 8.

Эти несколько строчек кода не похожи на нормальную, зрелую программу, но мы можем извлечь из них кое-какие знания:

1. Язык шейдеров содержит функцию `main`, которая возвращает цвет по окончании работы. Это напоминает C.

2. Конечный цвет пикселя записывается в зарезервированную переменную `gl_FragColor`.

3. В этом C-подобном языке есть встроенные *переменные* (такие как `gl_FragColor`), *функции* и *типы*. В этом примере мы только что познакомились с типом `vec4`, то есть четырёхкомпонентным вектором значений с плавающей точкой. Ниже мы встретим такие типы, как `vec3` и `vec2`, а так же более популярные `float`, `int` и `bool`.

4. Присмотревшись к типу `vec4`, можно догадаться, что его компоненты соответствуют красному, зелёному, синему и альфа каналам. Так же видно, что эти значения нормализованы, то есть лежат в диапазоне от `0.0` до `1.0`. Позднее мы увидим как нормализация значений позволяет проще преобразовывать значения между переменными.

5. Этот пример так же демонстрирует наличие макросов препроцессора - ещё одно важное свойство, пришедшее из языка C. Макросы раскрываются перед компиляцией. С их помощью можно определить глобальные значения (`#define`) и выполнить простые условные операции, используя `#ifdef` и `#endif`. Все макрокоманды начинаются с решётки (`#`). Препроцессор работает непосредственно перед компиляцией, подставляя определения из директив `#define` и проверяя условия `#ifdef` (если определено) и `#ifndef` (если не определено). Так, в примере выше строка 2 вставляется в код, только если определено `GL_ES`. Это как правило случается на мобильных платформах и браузерах.

6. Типы чисел с плавающей точкой играют ключевую роль в шейдерах, поэтому важно помнить о *точности*. Более низкая точность позволяет выиграть в скорости работы за счёт качества. Если вы достаточно дотошны, вы можете указывать точность каждой переменной. Во второй строке примера мы установили по умолчанию среднюю точность для всех чисел с плавающей точкой (`precision mediump float;`). Так же можно установить низкую (`precision lowp float;`) или высокую (`precision highp float;`) точность.

7. Последняя, и возможно, важнейшая деталь: спецификация GLSL не гарантирует автоматического приведения типов. Что это означает? Производители оборудования используют различные подходы для ускорения работы видеокарт, но они вынуждены обеспечивать соответствие какому-то минимальному набору требований. Автоматическое приведение в этот набор не входит. В нашем примере тип `vec4` содержит значения с плавающей точкой, поэтому он должен быть инициализирован соответствующими числами. Если вы хотите писать хороший, целостный код и не тратить многие часы на отладку белых экранов, возьмите себе за правило использовать точку (`.`) в значениях с плавающей точкой. Следующий код не везде будет работать корректно:

```glsl
void main() {
    gl_FragColor = vec4(1,0,0,1);	// ОШИБКА
}
```

К этому моменту мы описали основные элементы программы «hello world!», а значит, теперь самое время нажать на блок кода и начать применять полученные знания. При возникновении ошибок программа не скомпилируется и покажет белый экран. Попробуйте проделать следующее:

* Замените числа с плавающей точкой целыми. Ваша графическая карта может безошибочно воспринять такое поведение, а может и нет.

* Попробуйте закомментировать шестую строку, чтобы не присваивать никакое значение цвету пикселя.

* Объявите отдельную функцию, которая возвращает заданный цвет, и используйте её внутри `main()`. Например вот это код функции, которая возвращает красный:

```glsl
vec4 red(){
    return vec4(1.0,0.0,0.0,1.0);
}
```

* Есть много способов конструирования значений типа `vec4`. Попробуйте найти другие способы. Например, вот один из них:

```glsl
vec4 color = vec4(vec3(1.0,0.0,1.0),1.0);
```

Очевидно, это не самый крутой шейдер. Это всего лишь базовый пример, который красит все пиксели экрана в один цвет. В следующей главе мы покажем как изменять цвет пикселя в зависимости от двух типов входных данных: пространственных (положение пикселя на экране) и временных (количество секунд с момента загрузки страницы).
