![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.9` · мод `0.2.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Данные | Пред.: [Timeline](09_timeline_ru.md) | След.: [Button](../10-overlays/01_button_ru.md) | [English](10_organizationchart_en.md)

---

# OrganizationChart

Иерархическая схема: карточки узлов, соединительные линии, сворачивание поддеревьев и выбор узла.
Растёт вниз (классическая оргструктура) или вправо.

`RimUI.Components.OrganizationChart : UiElement`

## Отличие от Tree

`Tree` — это **список** строк с отступом по уровню: высота растёт, ширина фиксирована.
`OrganizationChart` — **двумерная** раскладка: уровни идут поперёк, братья — вдоль. Ширина растёт
быстро, поэтому схему почти всегда заворачивают в `ScrollBox` с горизонтальной прокруткой.

Компонент **сам не скроллит**: он честно возвращает свой полный размер, а обрезкой и прокруткой
занимается `ScrollBox` — иначе два скролла дрались бы за колесо.

## Пример

```csharp
var boss = new OrgChartNode("Губернатор") { Icon = Icons.Gear };
var supply = new OrgChartNode("Снабжение");
supply.Sub(new OrgChartNode("Повар")).Sub(new OrgChartNode("Фермер"));
boss.Sub(supply).Sub(new OrgChartNode("Оборона"));

var chart = new OrganizationChart { OnSelect = n => Log.Message(n.Text) };
chart.Roots.Add(boss);

var box = new ScrollBox(chart) { Horizontal = true, Style = { Height = 230f } };
```

Своя карточка через шаблон:

```csharp
new OrganizationChart
{
    NodeTemplate = n =>
    {
        var col = new FlexBox(Axis.Column) { Style = { Gap = 2f, AlignItems = AlignItems.Center } };
        col.Add(new Text(n.Text));
        col.Add(new Text(n.Value as string) { Style = { Text = new TextStyle { Size = FontSize.Tiny } } });
        return col;
    }
};
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Roots` | `List<OrgChartNode>` (readonly) | пусто | Корни схемы. Их может быть несколько — лес рисуется в один ряд. |
| `Orientation` | `OrgChartOrientation` | `Vertical` | Направление роста: `Vertical` (вниз) или `Horizontal` (вправо). |
| `NodeTemplate` | `Func<OrgChartNode,UiElement>` | `null` | Шаблон карточки. Вызывается **один раз на узел**, результат кэшируется по ключу. |
| `Selectable` | `bool` | `true` | Выбор целиком: `false` = карточки не подсвечиваются и не реагируют на клик. |
| `MultiSelect` | `bool` | `false` | Множественный выбор: клик добавляет/снимает узел. |
| `SelectChildren` | `bool` | `false` | Клик выбирает узел и всё его поддерево. Требует `MultiSelect`. |
| `SiblingGap` | `float` | `14` | Зазор между соседними поддеревьями. |
| `LevelGap` | `float` | `34` | «Коридор» между уровнями, в котором идут линии. |
| `NodeMinWidth` | `float` | `74` | Минимальная ширина карточки. |
| `OnSelect` | `Action<OrgChartNode>` | `null` | Выбран узел. |
| `OnToggle` | `Action<OrgChartNode,bool>` | `null` | Свёрнуто/развёрнуто поддерево (второй аргумент — свёрнуто ли теперь). |
| `Disabled` | `bool` | `false` | Отключённое состояние. |

### OrgChartNode

| Поле | Тип | Описание |
|---|---|---|
| `Key` | `string` | Стабильный ключ узла (`null` = путь по текстам). |
| `Text` | `string` | Подпись карточки. |
| `Icon` | `int` | Иконка перед подписью (`-1` = без иконки). |
| `Content` | `UiElement` | Произвольное содержимое карточки (приоритетнее `Text` и `NodeTemplate`). |
| `Collapsible` | `bool` | Показывать кнопку сворачивания (по умолчанию `true`). |
| `Selectable` | `bool` | Можно ли выбрать этот узел (по умолчанию `true`). |
| `Children` | `List<OrgChartNode>` | Дочерние узлы; добавлять удобнее методом `Sub`. |
| `Value` | `object` | Полезная нагрузка. |

## Поведение

**Раскладка в два прохода.** Сначала считается протяжённость поддерева — максимум из «своей
карточки» и «суммы поддеревьев детей с зазорами». Затем детям раздаётся место по порядку, а
родитель центрируется **между первым и последним ребёнком**, а не по середине всего поддерева: так
узел стоит ровно над своей группой, даже если поддеревья детей сильно разной величины.

**Размер уровня** — по самой крупной карточке на этом уровне, поэтому линии между уровнями всегда
одной длины.

**Хранится свёрнутость, а не раскрытость.** По умолчанию схема развёрнута целиком, и пустое
множество означает ровно это.

**`SelectChildren` требует `MultiSelect`.** Без него выбранным может быть только один узел, и
«выбрать поддерево» просто некуда записать — флаг молча ничего не делает.

**Кнопка сворачивания** — кружок на нижней кромке карточки. Её зона клика шире рисунка: попасть
мышью в 16-пиксельный кружок иначе тяжело.

## Стили

| Слот | Назначение |
|---|---|
| `orgchart/node` | Карточка узла. |
| `orgchart/node_hover` | Под курсором. |
| `orgchart/node_selected` | Выбранная карточка. |
| `orgchart/line` | Цвет соединительных линий (берётся только цвет). |
| `orgchart/toggle` | Кружок сворачивания: `Background`/`BorderColor` и `Text.Color` для шеврона. |

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `GetSelected()` | `OrgChartNode` | Последний выбранный узел (одиночный режим) либо последний переключённый. |
| `GetSelectedKeys(ctx)` | `IEnumerable<string>` | Ключи выбранных узлов. В одиночном режиме — ключ выбранного, если он есть. |
| `ClearSelection(ctx)` | — | Снять выбор целиком. |
| `SetCollapsed(ctx, nodeKey, collapsed)` | — | Свернуть/развернуть узел программно, по тому же ключу, что и клик. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnSelect` | `Action<OrgChartNode>` | узел | Клик по карточке. При `SelectChildren` — один раз, по корню поддерева. |
| `OnToggle` | `Action<OrgChartNode,bool>` | узел, свёрнут ли | Клик по кружку сворачивания. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedValue` | То же, что `OnSelect`, в общем виде. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы схемы. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по схеме. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Смена `Disabled`. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слоты карточки, линий и кнопки сворачивания. |
| Спрайт-рамка | Да | Через слот `orgchart/node`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Клики и подсветка отключаются. |
| Hover-fade | Нет | Подсветка карточки переключается мгновенно. |
| Курсор | Да | Рука над карточками и кнопкой сворачивания. |


---

[Оглавление](../index_ru.md) - Данные | Пред.: [Timeline](09_timeline_ru.md) | След.: [Button](../10-overlays/01_button_ru.md) | [English](10_organizationchart_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
