---
title: Булев
order: 30
---

Булев имеет тип `bool` и может принимать значения `true` или `false`. Основные операции:

- `&&`: логическое И
- `||`: логическое ИЛИ
- `not`: логическое НЕ. **Заметьте что символ ! зарезервирован для другой операции**
- `<=`, `>=`, `<`, `>`
- `==`: физическое сравнение, сравнивает структуры рекурсивно: `(1, 2) == (1, 2)` равно `true`. Удобно, но
 используйте аккуратно
- `===`: сравнение по ссылке. `(1, 2) === (1, 2)` равно `false`. `let myTuple = (1, 2); myTuple === myTuple` равно `true`.
- `!=`: физическое неравенство
- `!==`: неравенство по ссылке

### Использование

**Важно: BuckleScript предоставляет биндинги в JavaScript** `true` и `false`, которые
[не то же самое, что и Reason/OCaml `true` и `false`](http://bucklescript.github.io/bucklescript/Manual.html#_boolean)!
Не используйте из взаимозаменяемо без преобразования (`Js.to_bool` и `Js.Boolean.to_js_boolean`).

### Советы и трюки

**Используйте физическое сравнение аккуратно**. Это удобно, но вы можете случайно сравнить две глубоко
вложенные структуры данных и нанести большой урон производительности. Также не всегда явно, что считается
"равным". Например, часть данных `foo` равна ленивой `foo`? В идеале это должно быть настраиваемо.
В будущем такие изменения могут появится в языке. Если вам интересно, посмотрите [этот раздел](https://www.reddit.com/r/ocaml/comments/2vyk10/modular_implicits/).

### Дизайн решения

_Этот раздел предполагает знание [вариантов](../../guide/language/variant). Если вы читаете документацию первый
раз, то пропустите и вернитесь позже_!

Булев тип, это просто специальный случай варианта: `type bool = True | False`. Конструктивно, это элегантно
удаляет необходимость иметь булев тип в системе типов. Недостатком является, что конструкторы
[комплилируются в менее читаемое, но более быстрое представление](https://bucklescript.github.io/bucklescript/js-demo/?gist=fa7c72e81d7ac31977da1500ee4fa6d4).
Вот почему BuckleScript не хватает информации для компиляции Reason true/false в JavaScript true/false.
