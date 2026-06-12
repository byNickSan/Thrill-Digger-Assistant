# ⛏💣 Thrill Digger Assistant

[![⭐ Stars](https://img.shields.io/github/stars/byNickSan/Thrill-Digger-Assistant?style=for-the-badge&logo=github&color=e0b14a&labelColor=14100c)](https://github.com/byNickSan/Thrill-Digger-Assistant/stargazers)
[![🍴 Forks](https://img.shields.io/github/forks/byNickSan/Thrill-Digger-Assistant?style=for-the-badge&logo=github&color=5dbe6a&labelColor=14100c)](https://github.com/byNickSan/Thrill-Digger-Assistant/network/members)
[![👁 Watchers](https://img.shields.io/github/watchers/byNickSan/Thrill-Digger-Assistant?style=for-the-badge&logo=github&color=4e95e4&labelColor=14100c)](https://github.com/byNickSan/Thrill-Digger-Assistant/watchers)
[![📜 License](https://img.shields.io/github/license/byNickSan/Thrill-Digger-Assistant?style=for-the-badge&color=e25151&labelColor=14100c)](LICENSE)

[![🌐 Live Demo](https://img.shields.io/badge/🎮_play_online-bynicksan.github.io-d4a02e?style=for-the-badge&labelColor=2a1f10)](https://bynicksan.github.io/Thrill-Digger-Assistant/)
[![🔧 GitHub Pages](https://img.shields.io/github/deployments/byNickSan/Thrill-Digger-Assistant/github-pages?style=for-the-badge&label=deploy&logo=github&labelColor=14100c)](https://github.com/byNickSan/Thrill-Digger-Assistant/deployments)
[![📅 Last commit](https://img.shields.io/github/last-commit/byNickSan/Thrill-Digger-Assistant?style=for-the-badge&logo=git&color=b8a890&labelColor=14100c)](https://github.com/byNickSan/Thrill-Digger-Assistant/commits/main)
[![📦 Size](https://img.shields.io/github/repo-size/byNickSan/Thrill-Digger-Assistant?style=for-the-badge&color=c0c0c0&labelColor=14100c)](https://github.com/byNickSan/Thrill-Digger-Assistant)

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![PWA](https://img.shields.io/badge/PWA-5A0FC8?style=for-the-badge&logo=pwa&logoColor=white)
![No deps](https://img.shields.io/badge/no_dependencies-success?style=for-the-badge&color=5dbe6a)

🇬🇧 English · [🇷🇺 Русский](#русский)

---

Helper for the Thrill Digger minigame in Zelda: Skyward Sword. Shows bomb and rupoor probability for every cell and highlights the safest moves.

[🎮 Play online](https://bynicksan.github.io/Thrill-Digger-Assistant/)

## How to use

Click a cell and pick what you dug up in-game. Gold outline marks the safest move (rank 1, 2, 3 — first is best). Red pulsing cell means a guaranteed hazard — don't dig.

The game runs until you hit a bomb. You can't quit early, so your score is locked at the moment of death.

## Strategy — what's tested

Each cell gets a "utility" score:

```
U = pHaz − 0.02 × number_of_unrevealed_neighbors
```

The cell with the lowest U gets rank 1. Lower pHaz is better, and more unrevealed neighbors is better (a safe reveal gives more constraint info for the next move).

Tested on 10000 paired Expert games (same boards, different strategies):

| Strategy | Avg score | Δ vs current |
|---|---|---|
| Frontier only (next to revealed cells) | 65.0 | — |
| Global min pHaz, no info-gain | 73.9 | +8.9 |
| **Current: min pHaz + info-gain w=0.02** | **88.7** | — |

The info-gain term gains ~+15 points per game (95% CI [+12, +18]). The weight is flat between 0.015 and 0.040 — anywhere in that range works.

## First move — what's tested

Forcing different first moves on the same 10000 boards:

| First move | Avg | Δ vs corner |
|---|---|---|
| Free (info-gain picks an 8-neighbor cell near corner) | 101.6 | +12.9 |
| Forced center (2,4) | 96.5 | +7.8 |
| Forced edge-mid (2,0) | 93.2 | +4.5 |
| **Forced corner (0,0)** | **88.7** | — |

Corner is the **worst** opening. 95% CI [+9.8, +16.0] vs free choice.

## Why corner is worse — my guess

The numbers say corner is the worst start. Why exactly, I'm guessing:

A corner has 3 neighbors. An interior cell has 8. When you dig a corner and find, say, a blue rupee, the solver gets a constraint about 3 cells. Dig an interior cell — and you get a constraint about 8. More cells touched per dig means more useful information for the next move.

In Minesweeper there's a separate trick that helps corners: a zero auto-opens all its neighbors, and corners hit a zero more often (fewer neighbors that need to be empty). Thrill Digger doesn't have this — you open every cell by hand, so the "corner = zero more often" advantage shrinks to a single free click instead of a chain reaction.

I haven't tried to separate those two effects. The result is the same either way: don't open a corner first.

The solver under the hood is just CSP backtracking with constraint propagation, idea from [Studholme's Minesweeper paper](https://www.cs.toronto.edu/~cvs/minesweeper/minesweeper.pdf). His "prefer corners when guessing" rule from there I don't use — for the reason above.

## License

MIT

---

## Русский

Помощник для мини-игры Thrill Digger из Zelda: Skyward Sword. Показывает шанс бомбы и rupoor для каждой клетки, подсвечивает самые безопасные ходы.

[🎮 Поиграть](https://bynicksan.github.io/Thrill-Digger-Assistant/)

### Как пользоваться

Кликай по клетке и выбирай что выкопал в игре. Золотая обводка — безопасные ходы (ранг 1, 2, 3 — первый лучший). Красная пульсирующая — гарантированная опасность, копать нельзя.

Игра идёт до первой бомбы. Выйти раньше нельзя, очки фиксируются в момент смерти.

### Стратегия — что измерено

Для каждой клетки считаем «утилитку»:

```
U = pHaz − 0.02 × число_неоткрытых_соседей
```

Клетка с минимальным U получает ранг 1. Ниже pHaz — лучше, больше неоткрытых соседей — лучше (безопасное вскрытие даст больше констрейнт-инфо для следующего хода).

Проверено на 10000 парных партий Expert (одинаковые доски, разные стратегии):

| Стратегия | Средний счёт | Δ к текущей |
|---|---|---|
| Только клетки рядом с открытыми | 65.0 | — |
| Глобальная min pHaz без info-gain | 73.9 | +8.9 |
| **Текущая: min pHaz + info-gain w=0.02** | **88.7** | — |

Info-gain даёт ~+15 очков за партию (95% CI [+12, +18]). Кривая плоская в диапазоне 0.015–0.040 — любое значение работает.

### Первый ход — что измерено

Если форсировать разные первые ходы на тех же 10000 досок:

| Первый ход | Средний | Δ к углу |
|---|---|---|
| Свободный (info-gain берёт клетку с 8 соседями возле угла) | 101.6 | +12.9 |
| Форсированный центр (2,4) | 96.5 | +7.8 |
| Форсированный edge-mid (2,0) | 93.2 | +4.5 |
| **Форсированный угол (0,0)** | **88.7** | — |

Угол — **худший** первый ход. 95% CI [+9.8, +16.0] vs свободный выбор.

### Почему угол хуже — моё объяснение

Цифры говорят: угол это худший первый ход. Почему — честно гадаю.

У угла 3 соседа, у клетки в середине доски — 8. Когда копаешь угол и попадаешь, скажем, на синюю рупию, солвер получает информацию про 3 клетки вокруг. Копаешь центр — получаешь ту же подсказку, но уже про 8 клеток. Больше затронутых клеток за ход — больше материала для следующего решения.

В Minesweeper угол выигрывает по другой причине: ноль автоматически открывает всех соседей, а угол ловит ноль чаще (меньше соседей, которые должны быть пустыми). В Thrill Digger такого нет — каждую клетку копаешь руками. Так что преимущество «угол = ноль чаще» сжимается до одного бесплатного клика, а не цепной реакции.

Какой из этих факторов сильнее — отдельно не проверял. Итог один и тот же: первым ходом в угол лучше не лезть.

Солвер внутри — обычный CSP с пропагацией и backtracking. Идея из [статьи Studholme про Minesweeper](https://www.cs.toronto.edu/~cvs/minesweeper/minesweeper.pdf). Правило «при гадании предпочитай угол» оттуда я не использую — по причине выше.

### Лицензия

MIT
