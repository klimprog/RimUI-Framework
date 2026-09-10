![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.31` · мод `0.3.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - NodeCanvas | Пред.: [HeatmapChart](../14-charts/07_heatmapchart_ru.md) | След.: [Архитектура: как фреймворк устроен внутри](../16-internals/01_architecture_ru.md) | [English](01_nodecanvas_en.md)

---

# NodeCanvas

Поле блоков и связей: блоки свободно перетаскиваются мышью (при перекрытии рисуется тот, у кого
выше Z-индекс), связи — плавные кривые от выхода блока (справа) ко входу другого (слева) с
настраиваемыми лимитами по типу блока. Всё состояние поля можно сохранить и загрузить в виде
JSON.

`RimUI.Components.NodeCanvas : UiElement`

## Пример

```csharp
var canvas = new NodeCanvas { Style = { Height = 300f } };

canvas.LinkTypes.Add(new CanvasLinkType("Поток", new ColorRGBA(0.75f, 0.62f, 0.39f, 1f)));
canvas.LinkTypes.Add(new CanvasLinkType("Сигнал", Blue));

string flow = "Поток", signal = "Сигнал";
canvas.BlockTypes.Add(new CanvasBlockType("Источник").Allow(flow, 0, -1).Allow(signal, 0, 1));
canvas.BlockTypes.Add(new CanvasBlockType("Процесс").Allow(flow, -1, -1).Allow(signal, 1, 1));
canvas.BlockTypes.Add(new CanvasBlockType("Сток").Allow(flow, -1, 0).Allow(signal, 1, 0));

canvas.Groups.Add(new CanvasGroup("Основные", new ColorRGBA(0.75f, 0.62f, 0.39f, 1f)));

canvas.Nodes.Add(new CanvasNode { Id = "n1", Title = "Источник", Type = "Источник",
    Group = "Основные", Pos = new Vec2(30f, 40f) });
canvas.Nodes.Add(new CanvasNode { Id = "n2", Title = "Процесс", Type = "Процесс",
    Group = "Основные", Pos = new Vec2(260f, 110f) });
canvas.Links.Add(new CanvasLink { Type = flow, From = "n1", To = "n2" });

canvas.ConfirmDelete = (node, accept) =>
    ConfirmWindow.OfText("Удалить блок?", "Удалить блок «" + node.Title + "»?", accept).Show();
canvas.OnConfigure = node => OpenNodeConfig(canvas, node);

var export = Button.Make("Экспорт JSON");
export.OnClick = () => GUIUtility.systemCopyBuffer = canvas.Serialize();
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `BlockTypes` | `List<CanvasBlockType>` | пусто | Типы блоков и правила связей для них. |
| `LinkTypes` | `List<CanvasLinkType>` | пусто | Типы связей (имя + цвет). |
| `Groups` | `List<CanvasGroup>` | пусто | Группы блоков (имя + цвет бордюра). |
| `AllowCreate` | `bool` | `true` | Разрешено ли создавать блоки правым кликом по пустому месту. |
| `Nodes` | `List<CanvasNode>` | пусто | Рабочее состояние — узлы поля (мутируется компонентом). |
| `Links` | `List<CanvasLink>` | пусто | Рабочее состояние — связи поля (мутируется компонентом). |
| `NodeHeight` | `float` | `30` | Высота блока. |
| `LinkThickness` | `float` | `1.5` | Толщина линии связи. |
| `ArrowLength` | `float` | `10` | Длина стрелки на середине кривой связи; `0` = без стрелок (рисуется только при доступном GL-рендеринге). |
| `PortRadius` | `float` | `3.5` | Радиус точки крепления связи; `0` = без портов. |
| `AddBlockLabel` | `string` | `"+"` | Локализуемая подпись пункта меню «добавить блок». |
| `DeleteLinkLabel` | `string` | `"x"` | Локализуемая подпись пункта меню «удалить связь». |
| `NewBlockTitle` | `string` | `"block"` | Заголовок нового блока по умолчанию. |

### Типы данных

`CanvasBlockType(string name)`: `ColorRGBA Color`; метод `Allow(string linkType, int maxIn, int
maxOut)` (fluent; `-1` = без ограничения, `0` = нельзя) добавляет правило; метод
`RuleFor(linkType)` для чтения правила.

`CanvasLinkType(string name, ColorRGBA color)` — тип связи.

`CanvasGroup(string name, ColorRGBA color)` — цвет бордюра блоков этой группы.

`CanvasNode` (данные узла): `string Id`, `Title = ""`, `Type` (имя `CanvasBlockType`, `null` =
связи недоступны), `Group` (имя `CanvasGroup`, `null` = серый бордюр по умолчанию), `ColorRGBA
TitleColor`, `Vec2 Pos`, `int Z` (порядок отрисовки при перекрытии).

`CanvasLink` (данные связи): `string Type`, `From`, `To` (выход `From` — правое ребро блока, вход
`To` — левое).

### Точная геометрия портов и кривой связи

**Порты** — точные координаты относительно блока: выход (`From`) — правая грань блока
(`node.Pos.X + NodeWidth`) по центру его высоты (`node.Pos.Y + NodeHeight/2`); вход (`To`) —
левая грань блока (`node.Pos.X`) по той же вертикали. `NodeWidth` вычисляется по ширине текста
заголовка + место под кнопки-иконки (минимум 90 единиц), `NodeHeight` — публичное поле (по
умолчанию 30).

**Связь** — кубическая кривая Безье с 4 точками. Контрольные точки выступают от портов строго по
горизонтали на расстояние `dx = |Δx| × 0.5`, клампированное в диапазон `[24, 120]` единиц (то
есть «усы» кривой никогда не короче 24 и не длиннее 120 единиц, даже при очень близких или очень
далёких блоках) — не по нормали к линии соединения, а именно по оси X. Число сегментов ленты
адаптивно к длине хорды (`расстояние / 2.5`, клампится в `[24, 128]`).

**Стрелка** на связи рисуется в параметрической середине кривой (`t = 0.5`, а не в середине по
фактической длине дуги), направление — приближённо, по конечной разности точек кривой при
`t = 0.45` и `t = 0.55`. Рисуется только при доступном GL-рендеринге и `ArrowLength > 0`.

## Стили

Обычный `Style` элемента-контейнера (фон/бордюр самого поля). Блоки и связи рисуются собственной
процедурной отрисовкой, не через слоты темы.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `AddNode(CanvasNode node)` | `CanvasNode` | Добавить блок. Пустой `Id` — сгенерируется; занятый — блок НЕ добавляется, возвращается `null`. |
| `AddNode(string title, Vec2 pos, string type = null, string group = null)` | `CanvasNode` | То же по частям, с генерацией `Id`. |
| `RemoveNode(string id)` / `RemoveNode(CanvasNode node)` | `bool` | Удалить блок ВМЕСТЕ с его связями. `false` = такого блока нет. |
| `AddLink(string type, string from, string to, bool respectRules = true)` | `CanvasLink` | Создать связь. `null`, если блока нет, концы совпадают, связь уже есть либо (при `respectRules`) нарушены лимиты типов. |
| `RemoveLink(string type, string from, string to)` / `RemoveLink(CanvasLink link)` | `bool` | Удалить связь. |
| `Clear()` | — | Очистить поле целиком. |
| `Serialize()` | `string` | Сериализовать `Nodes`/`Links` в плоский JSON (с версией формата). |
| `Deserialize(string json, out string error)` | `bool` | Загрузить `Nodes`/`Links` из JSON; `false` при ошибке. |
| `SetStyle(string path, string value)` | — | Установить поле стиля строкой (`"text.color"`, `"background"`, `"radius"` и т.д.). Именованных частей стиля (`StylePart`) у `NodeCanvas` нет — путь применяется напрямую к стилю поля целиком; блоки и связи рисуются собственной процедурной отрисовкой и `SetStyle` не затрагиваются. Неизвестный путь/значение — тихо игнорируется. |

### Списки `Nodes` и `Links`: прямой доступ нежелателен

Списки остаются публичными но работать с ними напрямую
**не рекомендуется**: они не поддерживают целостность поля. `Nodes.Remove(node)` оставляет висячие
связи, которые ссылаются на несуществующий блок, рисуются в никуда и попадают в сериализацию.

```csharp
canvas.Nodes.Remove(node);        // НЕ НАДО: связи узла останутся
canvas.RemoveNode(node);          // надо: блок и его связи уходят вместе
```

`AddLink` дополнительно проверяет лимиты типов связей (`CanvasBlockType.Allow`), чего ручное
`Links.Add` не делает. Отключить проверку можно параметром `respectRules = false` — это нужно при
восстановлении поля из внешних данных, где правила уже применены.

## Версия формата

Сериализация начинается с номера версии:

```json
{ "version": 1, "blocks": [ ... ], "links": [ ... ] }
```

Как читается: поля нет — данные считаются версией 1 (сохранения, сделанные до появления поля,
читаются как обычно); версия новее поддерживаемой — `Deserialize` возвращает `false` с понятным
сообщением, а не разбирает файл «как получится». Полупрочитанное поле хуже честного отказа.

Версия растёт при смене **смысла** полей, но не при добавлении нового необязательного: такое поле
старый читатель просто игнорирует, и ломать ему совместимость незачем.

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Changed` | `Action` | — | Любое изменение поля: перетаскивание завершено, связь создана/удалена, блок создан/удалён. |
| `OnConfigure` | `Action<CanvasNode>` | узел | Клик по «шестерёнке» настроек блока; `null` = кнопка скрыта. |
| `ConfirmDelete` | `Action<CanvasNode, Action>` | узел, колбэк-подтверждение | Перед удалением блока — вызывающий код сам показывает диалог и зовёт переданный `accept`, если пользователь согласился; `null` = блок удаляется сразу без подтверждения. |

Плюс универсальные события `UiElement.Events` (есть у любого элемента, включая `NodeCanvas`):

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы поля целиком (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над полем — КАЖДЫЙ КАДР (~60 раз/сек). Обработчик должен быть лёгким: без аллокаций, поиска, IO — тяжёлая логика здесь просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по полю в целом (клики по конкретным блокам/связям/портам обрабатываются отдельной логикой «Управление мышью» ниже, не через эти события). |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над полем. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

## Управление мышью

- Клик по телу блока и перетаскивание — свободное перемещение (без привязки к сетке).
- Кнопка-стрелка на блоке открывает меню доступных исходящих типов связи (с учётом лимитов
  `CanvasLinkRule.MaxOut`); после выбора типа — режим линковки: подсвечиваются совместимые
  кандидаты, клик по кандидату создаёт связь, ПКМ отменяет.
- ПКМ по связи — меню «удалить связь».
- ПКМ по пустому месту (если `AllowCreate`) — меню «добавить блок»; новый блок получает
  автоинкрементный `Id`; если задан `OnConfigure`, конфигурация открывается сразу.
- Крестик на блоке — удаление (через `ConfirmDelete`, если задан, иначе сразу).

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Частично | Только фон/бордюр поля целиком; блоки/связи — процедурная отрисовка. |
| Спрайт-рамка | Да (только поле) | Через `Style.Sprite` самого `NodeCanvas`. |
| Анимация | Да | Общий механизм `Style.Animation` у поля целиком. |
| Сохранение/загрузка | Да | `Serialize()`/`Deserialize()` — плоский JSON. |
| Disabled | Нет | Своего понятия «отключён» нет. |
| Hover-fade | Нет | Подсветка кандидатов при линковке — мгновенная, без плавного перехода. |


---

[Оглавление](../index_ru.md) - NodeCanvas | Пред.: [HeatmapChart](../14-charts/07_heatmapchart_ru.md) | След.: [Архитектура: как фреймворк устроен внутри](../16-internals/01_architecture_ru.md) | [English](01_nodecanvas_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
