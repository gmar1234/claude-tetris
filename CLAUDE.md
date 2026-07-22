# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Qué es

Tetris clásico en JavaScript vanilla + HTML5 Canvas + CSS. Sin dependencias, sin `package.json`, sin bundler ni transpilador. Solo 3 archivos: `index.html`, `style.css`, `game.js`.

## Ejecutar / probar cambios

No hay build ni tests. Para probar cambios, abrir `index.html` directamente en el navegador o servir el directorio:

```bash
python3 -m http.server 8000   # o: npx serve .
```

No existen linters ni suites de test configurados en el repo.

## Arquitectura

Toda la lógica vive en `game.js` (~300 líneas, sin módulos/imports). Puntos clave:

- **Estado global**: variables sueltas (`board`, `current`, `next`, `score`, `lines`, `level`, `paused`, `gameOver`, `dropInterval`, etc.) declaradas arriba del archivo y reinicializadas en `init()`. No hay clases ni un objeto de estado encapsulado.
- **Tablero**: matriz `ROWS × COLS` (20×10); cada celda es `0` (vacía) o un índice 1–7 que indexa en `COLORS`/`PIECES` para identificar de qué pieza vino.
- **Piezas**: matrices cuadradas en `PIECES`. La rotación (`rotateCW`) es una transposición + inversión de filas, no rotación por lookup table. `tryRotate` aplica *wall kicks* simples probando desplazamientos `[0, -1, 1, -2, 2]` antes de descartar el giro.
- **Colisión** (`collide`): única función que verifica límites del tablero y solapamiento con bloques fijados; es el punto central que reutilizan movimiento, rotación, soft/hard drop y el ghost piece.
- **Game loop** (`loop`, vía `requestAnimationFrame`): acumula delta time en `dropAccum`; al superar `dropInterval` baja la pieza o dispara `lockPiece()`. `dropInterval` se recalcula en cada subida de nivel: `max(100, 1000 - (level-1)*90)`.
- **Ciclo de vida de una pieza**: `spawn()` (mueve `next` a `current`, genera nuevo `next`) → cae vía `loop`/`softDrop` → `lockPiece()` (`merge()` al tablero + `clearLines()` + `spawn()` de la siguiente). Si la pieza recién generada colisiona en `spawn()`, se dispara `endGame()`.
- **Renderizado**: `draw()` limpia y redibuja todo el canvas cada frame (grid, tablero fijado, ghost piece con `globalAlpha=0.2`, pieza actual). `drawNext()` dibuja en un canvas aparte (`next-canvas`) la vista previa de la siguiente pieza.
- **Puntuación**: tabla `LINE_SCORES = [0,100,300,500,800]` multiplicada por `level`; hard drop suma 2 pts/celda, soft drop 1 pt/fila. El nivel sube cada 10 líneas (`clearLines`).
- **Input**: un único listener `keydown` global que ignora movimiento si `paused || gameOver`, pero siempre permite `KeyP` (pausa).

## Personalización común

Constantes en `game.js` para ajustar el juego: `COLS`, `ROWS`, `BLOCK` (tamaño de celda en px), `COLORS`, `LINE_SCORES`, `dropInterval` inicial. Si se cambian `COLS`/`ROWS`/`BLOCK`, hay que actualizar también `width`/`height` del `<canvas id="board">` en `index.html` para que coincidan (`COLS×BLOCK` y `ROWS×BLOCK`).
