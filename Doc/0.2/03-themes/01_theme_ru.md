![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Темы | Пред.: [SpriteFrame — спрайт-рамка и тайлинг](../02-styles/02_spriteframe_ru.md) | След.: [Animations — реестр модулей](../04-animations/01_animations_ru.md) | [English](01_theme_en.md)

---

# Theme — темы и файл темы

Тема — набор дефолтных значений стиля для всех компонентов сразу («слоты») плюс палитра цветов и
метрики. Компонент не обязан ничего задавать в своём `Style` явно — то, что не задано, добирается
из слота темы на каждом кадре. Дефолтная тёмная тема (тёплая, «ванильная» гамма RimWorld) зашита
в код и работает даже без файла темы. Файл темы — JSON, парсится собственным парсером фреймворка
(не `UnityEngine.JsonUtility` — тот не умеет вложенные структуры); разбор строгий: любая ошибка
собирает список, и тема с ошибками не применяется целиком.

`RimUI.Styling.Theme` (данные) + `RimUI.Adapter.ThemeManager`/`ThemeJson` (загрузка).

## Пример

```csharp
string err;
if (!RimUI.Adapter.ThemeManager.LoadTheme("MyTheme", myModRootDir, out err))
    Log.Message("Тема не загружена: " + err);
```

```json
{
  "name": "MyTheme",
  "palette": {
    "surface": "#1A1713", "surfaceAlt": "#2E2A23", "accent": "#BE9E63",
    "borderColor": "#675F50", "textColor": "#E8E3D8", "textMuted": "#A39C8B"
  },
  "metrics": {
    "radiusSmall": 4, "radiusMiddle": 8, "radiusLarge": 16,
    "borderSmall": 1, "borderMiddle": 2, "borderLarge": 3,
    "maxWindowHeightFraction": 0.85, "hoverFadeDuration": 0.12
  },
  "checkbox": {
    "box": { "background": "#232018", "radius": "small", "borderWidth": "small" },
    "box_on": { "background": "linear:vertical:#BE9E63:#8E7A50" }
  }
}
```

Экспорт полного шаблона темы (все слоты и ключи с текущими значениями) доступен из настроек мода
RimUI Framework — кнопка «Экспортировать шаблон темы в буфер обмена».

## Параметры (палитра и метрики `Theme`)

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Name` | `string` | `"default"` | Имя темы. |
| `Surface` | `ColorRGBA` | тёплый тёмный | Фон окон. |
| `SurfaceAlt` | `ColorRGBA` | тёплый тёмный+ | Фон панели-секции. |
| `Accent` | `ColorRGBA` | тёпло-золотой | Акцентный цвет (активные элементы). |
| `BorderColor` | `ColorRGBA` | тёплый серо-коричневый | Цвет рамок по умолчанию. |
| `TextColor` | `ColorRGBA` | тёплый белый | Основной цвет текста. |
| `TextMuted` | `ColorRGBA` | приглушённый | Второстепенный цвет текста. |
| `SpacingUnit` | `float` | `8` | Базовая единица отступов. |
| `RadiusSmall`/`Middle`/`Large` | `float` | `4`/`8`/`16` | Пиксели за вариантами `BorderRadius`. |
| `BorderSmall`/`Middle`/`Large` | `float` | `1`/`2`/`3` | Пиксели за вариантами `BorderWidth`. |
| `MaxWindowHeightFraction` | `float` | `0.85` | Доля высоты экрана, которую максимум занимает окно. |
| `HoverFadeDuration` | `float` | `0.12` | Длительность плавного перехода hover, сек; `0` = мгновенно. |

## Структура JSON-файла темы

| Ключ верхнего уровня | Содержит |
|---|---|
| `palette` | `surface`, `surfaceAlt`, `accent`, `borderColor`, `textColor`, `textMuted`. |
| `metrics` | `radiusSmall/Middle/Large`, `borderSmall/Middle/Large`, `maxWindowHeightFraction`, `hoverFadeDuration`. |
| `text`, `field`, `button`, `divider`, `popover`, `menu`, `tooltip`, `modal` | Именованные слоты верхнего уровня (у `modal` дополнительно `sectionGap`). |
| `buttonPresets` | `defaultBg/dangerBg/warningBg/successBg/text/textDisabled/hoverShift/pressShift`. |
| `menuItems` | `textColor/textSize/hover/disabledText/height/iconSlot/minWidth`. |
| `input` | `bg/border/borderFocus/placeholder`. |
| `control` | `bg/border/accent/handle/disabled` (чекбокс/радио/свитч/слайдер). |
| Прочие секции (`"checkbox"`, `"slider"`, `"table"`...) | Слоты конкретных компонентов (реестр `ThemeSlots`) — пути перечислены на страницах самих компонентов. Корень секции — слот `"компонент"`, вложенный объект — слот `"компонент/часть"`. |

### Форматы значений внутри полей

| Тип значения | Формат |
|---|---|
| Цвет | `"#RRGGBB"` или `"#RRGGBBAA"`. |
| Заливка | Цвет, `"linear:vertical\|horizontal\|diagonal:#цвет1:#цвет2"`, или `"image:Ключ[:fit[:ax,ay[:#подложка[:#тинт]]]]"`. |
| Размер (radius/borderWidth) | `"small"` \| `"middle"` \| `"large"`. |
| Отступы | `"все"` \| `"гориз верт"` \| `"лево верх право низ"`. |
| Тень | `"offsetX offsetY blur spread [#цвет]"`. |
| Спрайт | `"КлючАтласа [N [N \| лево верх право низ]]"` или объект `{ atlas, corner, cornerLeft/Top/Right/Bottom }`; `N` — размер угла/грани в единицах (1 ед = 4px текстуры). Точный формат самой картинки-атласа (3×3 плитки, 1px разрывы, формула размера листа) — см. страницу «SpriteFrame — спрайт-рамка и тайлинг». |
| Текст | `textSize` (`tiny\|small\|medium`), `textAlign` (`left\|center\|right`), `textValign` (`top\|middle\|bottom`), `textWrap` (`true\|false`), `textWeight` (`normal\|bold`). |

### Специальные значения

| Значение | Смысл |
|---|---|
| `"none"` | «Не задано» — ищется дальше по цепочке приоритетов (код → файл темы → дефолты компонента). |
| `"off"` | «Явно выключено» — обрывает цепочку (фон/тень/спрайт не рисуются, отступы = 0). |
| Ключ с префиксом `_` | Игнорируется (удобно для комментариев в JSON, например `"_comment"`). |

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `ThemeManager.LoadTheme(string name, string modRootDir, out string error)` | `bool` | Загрузить и применить тему; `false` при ошибке разбора (текст — в `error`). |
| `Theme.Slot(string path)` | `Style` | Слот по пути (например `"checkbox/box"`); `null` = слот не объявлен. |
| `Theme.RadiusPx(BorderRadius r)` | `float` | Вариант скругления → пиксели темы. |
| `Theme.BorderPx(BorderWidth w)` | `float` | Вариант толщины рамки → пиксели темы. |
| `static Theme.BuildDefault()` | `Theme` | Дефолтная тема, зашитая в код. |

## События

Не применимо — загрузка темы синхронная (`LoadTheme` сразу возвращает результат), без колбэков.
Смена активной темы подхватывается ВСЕМИ уже открытыми окнами без перезапуска — резолв стиля идёт
заново каждый кадр.

### Точный порядок пересборки слотов и «горячая» смена темы

`RebuildSlots()` вызывается только ОДИН РАЗ за загрузку файла темы (после разбора палитры/метрик,
но до применения именованных секций слотов из JSON) — не каждый кадр. Порядок внутри неё: сброс
реестра слотов → пересборка всех registry-слотов от текущей палитры → пересборка именованных
слотов верхнего уровня (`TextSlot`, `Field`, `Button`, панели попапов/меню/тултипов) → сброс
панелей окон (`WindowPanel`/`ModalPanel`/…) к «нативная подложка», пока файл темы явно не задаст
свою.

Смена активной темы «на лету» (`ThemeManager.Apply`) — это просто замена ссылки на объект темы и
инвалидация кэша запечённых текстур; сама пересборка слотов НЕ повторяется (она уже отработала
при парсинге нового файла). Каждое окно читает актуальную тему заново КАЖДЫЙ кадр перед `Measure`,
поэтому в общем случае смена темы отражается уже в том же кадре. Единственный кадр задержки
теоретически возможен, только если смена темы происходит как побочный эффект обработки клика уже
ПОСЛЕ того, как это же окно в этом кадре уже прошло `Measure` — тогда отрисовка (`Emit`) этого
кадра пройдёт по старому стилю, а полный эффект будет виден со следующего кадра. Это не
формальная гарантия фреймворка, а следствие обычного порядка `Measure → Arrange → Emit`.

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Слоты компонентов | Да | Основной механизм темизации — см. страницы конкретных компонентов. |
| Спрайты | Да | Строкой или объектом в JSON. |
| Анимация строкой | Да | Ключ `"animation"` в любом слоте JSON. |
| Горячая перезагрузка | Да | Смена темы на лету без переоткрытия окон. |


---

[Оглавление](../index_ru.md) - Темы | Пред.: [SpriteFrame — спрайт-рамка и тайлинг](../02-styles/02_spriteframe_ru.md) | След.: [Animations — реестр модулей](../04-animations/01_animations_ru.md) | [English](01_theme_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
