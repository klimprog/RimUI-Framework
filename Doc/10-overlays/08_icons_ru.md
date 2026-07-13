![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — ядро `0.6.7` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [Icon](07_icon_ru.md) | След.: [Text](09_text_ru.md) | [English](08_icons_en.md)

---

# Icons

Встроенный лист иконок фреймворка (сетка 48×48, 8 колонок, 1px разрывы между ячейками) и фабрика
готовых `Icon`-элементов по индексу. Это статический хелпер, а не элемент интерфейса — см.
страницу «Icon» для самого элемента.

`RimUI.Elements.Icons` (static class)

## Пример

```csharp
var ic = Icons.Get(Icons.Check, 16f);
ic.Tint = ColorRGBA.White;

var rotated = Icons.Get(Icons.ChevronRight, 20f);
rotated.Rotation = 45f;
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `DefaultAtlas` (const) | `string` | `"RimUI/Icons"` | Ключ встроенного листа. |
| `DefaultCell` (const) | `int` | `48` | Размер ячейки в пикселях. |
| `DefaultColumns` (const) | `int` | `8` | Число колонок листа. |
| `Gap` (const) | `int` | `1` | Разрыв между ячейками, px. |
| `AtlasKey` | `string` | `DefaultAtlas` | Активный лист (тема может подменить). |
| `Cell` | `int` | `DefaultCell` | Активный размер ячейки. |
| `Columns` | `int` | `DefaultColumns` | Активное число колонок. |

Именованные индексы: `Close=0, Hamburger=1, Check=2, ChevronDown=3, ChevronRight=4,
ChevronUp=5, ChevronLeft=6, Dot=7, Plus=8, Minus=9, SpinnerRing=10, Info=11, Warning=12,
Error=13, ChevronDoubleUp=14, ChevronDoubleDown=15, ChevronDoubleRight=16, ChevronDoubleLeft=17,
Gear=18`.

### Точная формула `Rect(index)`

```
col = index % Columns
row = index / Columns
x = col * (Cell + Gap)
y = row * (Cell + Gap)
Rect = (x, y, Cell, Cell)
```

Индексация с нуля, слева-направо и сверху-вниз. Шаг ячейки (расстояние между соседними
началами) — `Cell + Gap` (49px при дефолтах), но размер самого возвращаемого прямоугольника —
ровно `Cell × Cell` (48×48): зазор `Gap` только сдвигает начало следующей ячейки, в размер
иконки не входит. Координаты — в пикселях от верхнего-левого угла листа (как в графическом
редакторе); перевод в нормализованные Unity-UV координаты (с переворотом оси Y, «снизу — 0»)
происходит уже внутри адаптера при отрисовке, а не в этом классе.

## Стили

Не применимо — это статическая утилита, не элемент со `Style`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `static Get(int index, float size = 0f)` | `Icon` | Готовый `Icon` с `Source = Rect(index)`; при `size > 0` сразу ставит точный размер. |
| `static Rect(int index)` | `RectF` | Пиксельный прямоугольник иконки по индексу. |
| `static Configure(string atlasKey, int cell, int columns)` | `void` | Подменить лист иконок целиком (вызывается `ThemeManager` при загрузке темы). |
| `static ResetConfig()` | `void` | Сброс к дефолтному листу. |

## События

Не применимо.

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Своя тема иконок | Да | `Configure(...)` меняет весь лист (например, из JSON темы). |
| Спрайт/анимация/disabled/hover-fade | См. страницу «Icon» | Все эти механики относятся к возвращаемому `Icon`-элементу, не к самому статическому классу. |


---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [Icon](07_icon_ru.md) | След.: [Text](09_text_ru.md) | [English](08_icons_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
