# 🏐 Estadísticas Voleibol (PWA)

App para llevar **marcador, rotación, entrada/salida del líbero y estadísticas de jugada final**
de un solo equipo (el que analizas). Funciona en **Android e iPhone/iPad**, se instala como app
y corre **sin conexión**. Todo es un único archivo (`index.html`), sin servidores ni Node.

## Archivos
- `index.html` — la aplicación completa.
- `manifest.json` — datos para instalarla como app.
- `sw.js` — service worker (uso sin conexión).
- `icon-192.png`, `icon-512.png` — íconos.

## Cómo usarla

### En la computadora (para probar)
1. Abre una terminal en esta carpeta.
2. Ejecuta: `python -m http.server 5599`
3. Abre en el navegador: `http://localhost:5599/index.html`

> Hace falta un servidor (aunque sea local) para que funcionen el modo sin conexión y la
> instalación. Abrir el archivo con doble clic funciona para probar, pero sin esas dos cosas.

### En el teléfono o tablet (lo ideal)
La forma recomendada es subirla a **GitHub Pages** (igual que el Marcador de Tenis de Mesa) y
abrir esa dirección en el móvil. Luego:
- **Android (Chrome):** menú ⋮ → *Instalar app* / *Añadir a pantalla de inicio*.
- **iPhone/iPad (Safari):** botón Compartir → *Añadir a pantalla de inicio*.

## Flujo de inicio y equipos guardados
1. **Al abrir** la app pregunta *¿Qué equipo vas a analizar?* y lista los equipos guardados
   (1, 2, 3…). Si **no hay ninguno**, salta directo a crear el primero.
2. Los equipos se guardan en el dispositivo en un **JSON** (clave `voleibol_equipos_v1`) con solo el
   **nombre del equipo y sus jugadoras (número + nombre)**. Las posiciones NO se guardan: se arman
   en cada partido.
3. Al elegir un equipo entra a **Configura tu equipo**: ves la plantilla con un **✓** por jugadora.
   La app **recuerda la última alineación** con la que jugaste (posiciones, líbero, banca, quién
   saca), así que normalmente solo ajustas lo que cambió.
   - Marca quién juega y ponle **posición** (P1–P6 titular · Banca suplente · Líbero).
   - **Añadir jugadora** o **editar número/nombre** → se guarda solo en el JSON.
   - **Eliminar una jugadora** del equipo: botón **🗑** en su fila (pide confirmación).
4. **⇄ Cambio** durante el partido sustituye a la jugadora seleccionada por una de la banca
   (queda registrado y la sustituta acumula sus propias estadísticas).

> Los avisos y confirmaciones son **dentro de la app** (no usan las ventanitas del navegador), así
> funcionan igual en el móvil y en paneles que las bloquean.

## Estadísticas por jugadora
Botón **📊 Estadísticas** → pantalla aparte con, por jugadora: Saque (S), Ataque (A), Bloqueo (B),
Puntos a favor (S+A+B), Defensa neg. (D), Recepción neg. (R) y **% de eficacia**
= positivas/(positivas+negativas). Incluye fila de totales y se puede exportar a CSV.

## Cómo se registra una jugada
1. Toca a la jugadora en el cuadro de **Rotación** (se resalta en amarillo).
2. Toca el botón de acción:
   - **S** Saque positivo · **A** Ataque positivo · **B** Bloqueo positivo · **E** Error del rival → **punto para tu equipo**.
   - **D** Defensa negativa (saque) · **R** Recepción negativa → **punto para el rival**.
   - **E** no necesita número (es error del rival).
   - **＋ / －** suman un punto manual a tu equipo o al rival (para jugadas que no encajan).

La **rotación es automática**: tu equipo rota cuando **recupera el saque** (side-out).
El indicador **SAQUE** se puede tocar para corregir quién saca.

**Líbero:** selecciona una posición de zaga (P1, P5 o P6) y pulsa **🔄 Líbero** para que entre o salga.

**Otros botones:** ↶ Deshacer (corrige el último registro), ＋ Nuevo set, ⬇ Exportar (JSON / CSV / Compartir).

## Datos
- El partido se guarda solo en el dispositivo (`localStorage`) y se puede **reanudar**.
- **Exportar JSON:** registro completo (con los números repetidos aunque no cambie el marcador).
- **Exportar CSV:** la tabla tal como se ve (columna en blanco cuando el marcador no cambia).

## Pendiente / ideas para pulir
- Pantalla de estadísticas agregadas por jugadora (totales de S/A/B/D/R, % de eficacia).
- Líbero automático (entrada/salida según rotación).
- Fin de set automático (25 / 15 con ventaja de 2).
- Íconos PNG con mejor diseño.
