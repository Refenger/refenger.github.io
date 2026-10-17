# Доступные функции

## Базовые функции Lua

Работают как в обычном Lua 5.1 / LuaJIT ([справочник](https://www.lua.org/manual/5.1/manual.html#5.1)).

| Имя | Что делает |
|---|---|
| `print(...)` | Печатает в консоль игры. |
| `type(v)` | Тип значения: `"nil"`, `"number"`, `"string"`, `"table"`, `"function"`, `"boolean"`, `"userdata"`, `"thread"`. |
| `tostring(v)`, `tonumber(v [, base])` | Преобразуют в строку / число (`tonumber` возвращает `nil`, если не получилось). |
| `pairs(t)`, `ipairs(t)`, `next(t [, k])` | Обход таблиц. |
| `select(n, ...)` | Аргумент номер `n`; `select("#", ...)` — количество аргументов. |
| `unpack(t [, i [, j]])` | Элементы таблицы как отдельные значения. |
| `assert(v [, msg])` | Ошибка `msg`, если `v` ложно. |
| `error(msg [, level])` | Вызывает ошибку. |
| `pcall(f, ...)`, `xpcall(f, handler)` | Вызов с перехватом ошибки: `local ok, result = pcall(f)`. |
| `setmetatable(t, mt)` | Задаёт метатаблицу (для классов и `__index`). |
| `getmetatable(v)` | Метатаблица значения. Для строк возвращает `nil`: метатаблица строк принадлежит игре. |
| `rawget`, `rawset`, `rawequal` | Доступ к таблице в обход метатаблицы. |
| `_VERSION` | `"Lua 5.1"`. |
| `_G` | Окружение самого скрипта (не игры). |

## Библиотеки

У каждого скрипта свои копии этих таблиц: если скрипт что-то в них заменит, игра и другие песни этого не увидят.

| Имя | Что внутри |
|---|---|
| `math` | Вся стандартная: `math.sin`, `math.floor`, `math.random`, `math.pi`, `math.huge`… |
| `string` | Вся стандартная: `string.format`, `string.find`, `string.gsub`, `string.sub`… Методы строк тоже работают: `("abc"):upper()`. |
| `table` | Вся стандартная: `table.insert`, `table.remove`, `table.sort`, `table.concat`. |
| `coroutine` | Корутины: `create`, `resume`, `yield`, `status`, `wrap`, `running`. |
| `utf8` | Работа с UTF-8 (кириллица): `utf8.len`, `utf8.char`, `utf8.codepoint`, `utf8.offset`, `utf8.codes`. |
| `bit` | Битовые операции LuaJIT: `band`, `bor`, `bxor`, `bnot`, `lshift`, `rshift`… |
| `os` | Только время: `os.time()`, `os.clock()`, `os.date(...)`, `os.difftime(a, b)`. |

## love

Весь модуль LÖVE 11.5 без ограничений ([документация](https://love2d.org/wiki/Main_Page)). Чаще всего нужны:

| Модуль | Для чего |
|---|---|
| `love.timer` | `getTime()` — время в секундах (удобно для анимаций через `math.sin`), `getDelta()`, `getFPS()`. |
| `love.keyboard` | `isDown("a")`, `isScancodeDown("a")` — зажата ли клавиша. |
| `love.mouse` | `getPosition()`, `isDown(1)`. |
| `love.audio` | `newSource(path, "static")`, `play`, `stop` — звуки. |
| `love.graphics` | Картинки, шрифты, шейдеры (`newImage`, `newShader`…). |
| `love.filesystem` | Чтение файлов: `read`, `getInfo`, `getDirectoryItems`. |
| `love.math` | `random`, `noise`, `newRandomGenerator`. |

Важно:
- Пути в `love.*` указываются от корня игры, а не от папки скриптов. Например, файл песни: `"songs/<Песня>/..."`.
- Колбэка `draw` пока нет, поэтому то, что скрипт нарисует через `love.graphics` в `update`, на экране не появится.
- `love.graphics.getWidth()/getHeight()` возвращают размер настоящего окна. Игра же рисует в виртуальном поле 1920×1080, которое масштабируется под окно.
- `love.filesystem.write` пишет в папку сохранений, `love.event.quit()` закрывает игру: модуль открыт целиком, пользуйтесь аккуратно.

## Что недоступно

| Нет | Почему |
|---|---|
| Глобальные переменные игры (`PlayState`, `Options`, `Mods`…) | Пока не открыты. Будут открываться по одной и описываться здесь. |
| `io`, `os.execute`, `os.remove`, `os.exit`, `os.getenv`… | Доступ к системе и файлам компьютера. |
| `debug` | Позволяет обойти любые ограничения. |
| `load`, `loadstring`, `dofile`, `loadfile` | Запуск кода в обход песочницы. Вместо них — `require`. |
| `setfenv`, `getfenv` | Смена окружения функций. |
| `package`, `module`, `collectgarbage` | Модульная система и сборщик мусора игры. |
