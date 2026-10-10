# EffectCraft Custom

Объединённая пользовательская сборка [EffectCraft](https://github.com/storytold/effectcraft):
**Infinite Drag + Type-in Font Search + TextBox Plugin**.

Полные исходники находятся в этом репозитории. Это независимая модификация EffectCraft
команды ArtCraft, а не официальный релиз проекта.

## Скачать для Windows

[Готовая portable-сборка Windows x64](https://github.com/kikikikier/effectcraft-custom/releases/latest).
Распакуйте ZIP и запустите `effectcraft.exe`. TextBox уже встроен; устанавливать его отдельно не нужно.

## Что добавлено

| Отдельный патч | Возможность |
|---|---|
| [Infinite Drag](https://github.com/kikikikier/effectcraft-infinite-drag) | Бесконечное перетаскивание числовых значений |
| [Type-in Font Search](https://github.com/kikikikier/effectcraft-type-in-font-search) | Поиск шрифта прямо в поле выбора |
| [TextBox Plugin](https://github.com/kikikikier/effectcraft-textbox-plugin) | Плашка под текст с независимым Matte |

- Числовые значения можно тянуть через край монитора сколько угодно раз, сохраняя одно действие Undo.
- Шрифт ищется прямо в строке его выбора, в Character и Properties.
- TextBox поддерживает отрицательные отступы и Matte, который не зависит от прозрачности заливки.

## Сборка из исходников

База: upstream commit [`608d1b077b65bb7591f26be587a2a5a47f4278bc`](https://github.com/storytold/effectcraft/commit/608d1b077b65bb7591f26be587a2a5a47f4278bc).
Нужны Git, Rust 1.95.0 и инструменты сборки C++ для Windows (MSVC).

```powershell
git clone https://github.com/kikikikier/effectcraft-custom.git
Set-Location effectcraft-custom
cargo +1.95.0 build --locked --release -p effectcraft -p effectcraft-cli
```

Для portable-сборки со статической CRT см. [CUSTOM-BUILD.md](CUSTOM-BUILD.md).
Папка [patches](patches) содержит те же самостоятельные патчи для чистого upstream.
К исходникам этого репозитория применять их повторно не нужно — изменения уже включены.

## Проверки и ограничения

Сборка Windows и 122 UI-теста прошли при подготовке portable-версии, включая 8 тестов
непрерывного перетаскивания. TextBox также проверялся через host/compositor и CPU/GPU-рендер.
Проверка физической мышью через Windows Computer Use не завершилась из-за тайм-аутов захвата окна.
Полный upstream CI не заявлен зелёным: в базовом коде известны предупреждения Clippy.
При публикации повторно прошли 105 модульных UI-тестов и 17 тестов углов, drag/drop,
поиска шрифтов и редактирования текста. Расширенный прогон остановился на `ui_gpu`:
в тестовом окне не инициализировался GPU-композитор; причина пока не установлена.
Подробности — [CUSTOM-BUILD.md](CUSTOM-BUILD.md).

## Лицензия и авторство

**MIT OR Apache-2.0**, см. [LICENSE-MIT](LICENSE-MIT), [LICENSE-APACHE](LICENSE-APACHE)
и [NOTICE](NOTICE). Авторство исходного EffectCraft сохранено.
Оригинальный плагин TextBox предоставлен kikikikier; здесь включена доработанная версия.
Фирменные знаки ArtCraft заменены простыми инициалами EC в интерфейсе и исключены из
публикуемой документации. Открытые ассеты перечислены в [ATTRIBUTION.md](ATTRIBUTION.md).
