# 🗡️ Thrill Digger Assistant

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

Click a cell and pick what you dug up in-game. Gold outline marks the safest move. Red pulsing cell means a guaranteed hazard — don't dig.

The game runs until you hit a bomb. You can't quit early, so your score is locked at the moment of death.

## Strategy

The assistant picks the cell with the lowest hazard probability across the whole board. Tested on 10000 paired Expert games:

| Picking from | Avg score | ≥200 |
|---|---|---|
| Only cells next to revealed ones | 65.0 | 10.6% |
| **All cells** | **73.9** | **12.4%** |
| Corners first (Minesweeper-style) | 62.8 | 9.5% |

The "all cells" strategy beats "frontier only" by ~**+9 points per game** on average (95% CI [+7, +11]).

The solver is a standard CSP: constraint propagation plus backtracking with an iteration cap. Idea borrowed from [Studholme's Minesweeper paper](https://www.cs.toronto.edu/~cvs/minesweeper/minesweeper.pdf), but without the "corner > edge > interior" rule from his Step 7 — that rule wins in Minesweeper because zero-cells trigger cascading reveals. Thrill Digger has no cascade, so the rule actually hurts here.

## License

MIT

---

## Русский

Помощник для мини-игры Thrill Digger из Zelda: Skyward Sword. Показывает шанс бомбы и rupoor для каждой клетки, подсвечивает самые безопасные ходы.

[🎮 Поиграть](https://bynicksan.github.io/Thrill-Digger-Assistant/)

### Как пользоваться

Кликай по клетке и выбирай что выкопал в игре. Золотая обводка — самый безопасный ход. Красная пульсирующая — гарантированная опасность, копать нельзя.

Игра идёт до первой бомбы. Выйти раньше нельзя, очки фиксируются в момент смерти.

### Стратегия

Беру клетку с минимальным шансом опасности по всей доске. Проверил на 10000 парных партий Expert:

| Откуда выбираю | Средний счёт | ≥200 |
|---|---|---|
| Только клетки рядом с открытыми | 65.0 | 10.6% |
| **Все клетки** | **73.9** | **12.4%** |
| Углы в приоритете (как в Minesweeper) | 62.8 | 9.5% |

«Все клетки» обгоняют «только соседние» примерно на **+9 очков за партию**, доверительный интервал [+7, +11].

Под капотом обычный CSP-солвер: пропагация плюс backtracking с лимитом итераций. Идея из [статьи Studholme про Minesweeper](https://www.cs.toronto.edu/~cvs/minesweeper/minesweeper.pdf), но без правила «угол → край → центр» из Step 7. В Minesweeper оно выгодно, потому что нулевые клетки запускают каскад. В Thrill Digger каскада нет — каждую клетку копаешь руками — и это правило только мешает.

### Лицензия

MIT
