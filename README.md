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
1. Toca **primero la acción**, luego la **jugadora** en la rotación:
   - **S** Saque · **A** Ataque · **B** Bloqueo → cada botón está partido en dos mitades:
     **mitad derecha ＋** = jugada positiva (punto tuyo); **mitad izquierda －** = jugada negativa
     (error → punto del rival).
   - **D** Defensa negativa (saque) · **R** Recepción negativa → **punto para el rival**.
   - **E** Error del rival → **punto tuyo** (no necesita jugadora).
2. El **Saque (S)** asigna automáticamente a la jugadora en **posición 1** (la que saca).

### Orientación apaisada (tablet/teléfono horizontal)
La pantalla se divide en dos: a la **izquierda** marcador, línea de jugada, rotación y botones de
acción; a la **derecha** la tabla de jugadas (a 2 columnas, con scroll) y debajo los botones
auxiliares (deshacer, cambio, líbero, etc.).

La **rotación es automática**: tu equipo rota cuando **recupera el saque** (side-out).
El indicador **SAQUE** se puede tocar para corregir quién saca.

**Sets:** eliges **3 o 5 sets** en la configuración. El set se **cierra solo** a 25 (15 en el set
decisivo) con 2 de diferencia, y el partido termina al ganar 2 (de 3) o 3 (de 5) sets.

**Líbero (🔄):** al pulsarlo eliges **por qué jugadora de zaga entra**. Si la rotación lo lleva a
**zona delantera (P4)**, sale un **aviso** y la app lo saca solo, devolviendo a la jugadora original.
Estando en cancha, también se puede cambiar con **⇄ Cambio**.

**Otros botones:** ↶ Deshacer · ⇄ Cambio · 📊 Estadísticas · ⬇ Exportar (JSON / CSV / Compartir).

## Datos
- El partido se guarda solo en el dispositivo (`localStorage`) y se puede **reanudar**.
- **Exportar JSON:** registro completo (con los números repetidos aunque no cambie el marcador).
- **Exportar CSV:** la tabla tal como se ve (columna en blanco cuando el marcador no cambia).

## Pendiente / ideas para pulir
- Filtro de estadísticas por set.
- Íconos PNG con mejor diseño.
- Reentrada automática del líbero cuando la jugadora central vuelve a zaga.
