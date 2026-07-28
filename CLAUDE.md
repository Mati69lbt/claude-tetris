# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Tetris clásico en JavaScript vanilla (HTML5 Canvas + CSS), sin dependencias, sin build, sin `package.json`. Solo 3 archivos de código: `index.html`, `style.css`, `game.js` (~300 líneas).

## Running

No hay build ni tests. Para correr el juego:

```bash
start index.html        # abrir directo (Windows)
# o servir estático:
npx serve .
python3 -m http.server 8000
```

## Architecture (`game.js`)

- **Tablero**: matriz `ROWS × COLS` (20×10); cada celda es `0` (vacía) o índice de color 1–7 de una pieza fijada.
- **Piezas**: matrices cuadradas; rotación por transposición + reverso de filas (`rotateCW`).
- **Colisiones**: `collide()` valida límites del tablero y solapamiento con bloques fijos.
- **Wall kicks**: `tryRotate()` desplaza ±1/±2 columnas si la rotación choca antes de descartarla.
- **Loop**: `requestAnimationFrame` en `loop()`, acumula `dt` y baja la pieza al superar `dropInterval`.
- **Líneas**: `clearLines()` recorre de abajo hacia arriba, elimina filas completas e inserta vacías arriba.
- **Puntaje**: tabla `LINE_SCORES = [0,100,300,500,800]` × nivel; hard drop = 2 pts/celda, soft drop = 1 pt/fila.
- **Nivel/velocidad**: sube cada 10 líneas; `dropInterval = max(100, 1000 - (level-1)*90)` ms.
- **Ghost piece**: `ghostY` proyecta la caída final, se dibuja con `globalAlpha = 0.2`.
- Game over se dispara en `spawn()` si la pieza nueva ya colisiona al aparecer.

Si se cambian `COLS`, `ROWS` o `BLOCK` en `game.js`, hay que ajustar `width`/`height` del `<canvas id="board">` en `index.html` para que coincidan (`COLS × BLOCK`, `ROWS × BLOCK`).
