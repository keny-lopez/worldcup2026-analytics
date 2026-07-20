# Mundial 2026 — Análisis de Datos de Jugadores

Dataset y análisis de estadísticas de jugadores y resultados del Mundial de la FIFA 2026 (Estados Unidos, México y Canadá), actualizado tras la final del torneo (19 de julio de 2026).

## Origen del proyecto

Este proyecto nace de la iniciativa de un grupo de compañeros de la **Digital Week de la USAP**, quienes decidieron reenfocar el análisis hacia datos del Mundial 2026 recién finalizado, en lugar de continuar con datos deportivos de meses atrás. La búsqueda de una fuente actualizada y confiable, y la construcción de este dataset, es trabajo propio del grupo.

## Fuente de los datos

Todos los datos provienen de **[FBref](https://fbref.com/en/comps/1/World-Cup-Stats)** (Sports Reference).

> Como pide FBref explícitamente en sus tablas: *"please cite us and provide a link and/or a mention"*. Este repositorio da ese crédito aquí y en cada sección donde se usan los datos.

## Contenido

| Archivo | Descripción |
|---|---|
| `data/player_standard_stats.csv` | Estadísticas estándar por jugador: goles, asistencias, tarjetas, penales, y sus versiones por 90 minutos |
| `data/player_shooting_stats.csv` | Disparos, disparos al arco, eficiencia de conversión |
| `data/player_playing_time_stats.csv` | Minutos, titularidades, suplencias, +/- del equipo con y sin el jugador en cancha |
| `data/player_misc_stats.csv` | Tarjetas dobles, faltas, offsides, cruces, intercepciones, penales, autogoles |
| `data/player_goalkeeping_stats.csv` | Estadísticas de porteros: goles recibidos, atajadas, % de atajadas, arcos en cero |
| `data/scores_fixtures.csv` | Calendario y resultados de los 104 partidos del torneo, con la fase de cada uno |

## Notas metodológicas

- Las tablas de jugadores (excepto Playing Time) solo incluyen a quienes acumularon minutos jugados; Playing Time incluye a todos los convocados, jugaran o no.
- Todos los partidos del torneo se disputaron en sede neutral (Estados Unidos, México, Canadá); las columnas `Home`/`Away` de `scores_fixtures.csv` no reflejan ventaja de localía real.
- En los partidos definidos por tanda de penales, el marcador de la tanda se excluye del conteo de goles de juego.
- La última columna de cada tabla de jugadores (identificada como `Matches`/`-9999` en el CSV crudo) es en realidad un **identificador único por jugador** generado por FBref — se usa como llave de unión entre tablas en vez del nombre, para evitar problemas de acentos o variantes de escritura.

## Glosario de columnas (bilingüe)

Glosario completo de las 6 tablas fuente, con nombre original en inglés, traducción al español, y descripción del contenido. Las columnas calculadas incluyen su fórmula, pensada para reutilizarse como notación LaTeX en el notebook.

### Columnas comunes a varias tablas

| Columna (EN) | Traducción (ES) | Descripción |
|---|---|---|
| `Rk` | Rango / N.° de fila | Conteo de filas de arriba hacia abajo; se recalcula si se ordena por otra columna. No es un dato del jugador. |
| `Pos` | Posición | Posición más jugada. `GK` Portero, `DF` Defensa, `MF` Mediocampista, `FW` Delantero, `FB` Lateral, `LB` Lateral izquierdo, `RB` Lateral derecho, `CB` Defensa central, `DM` Mediocampista defensivo, `CM` Mediocampista central, `LM` Mediocampista izquierdo, `RM` Mediocampista derecho, `WM` Mediocampista de banda, `LW` Extremo izquierdo, `RW` Extremo derecho, `AM` Mediocampista ofensivo |
| `Squad` / `Club` | Selección / Club | `Squad` es la selección nacional; `Club` es el club de origen del jugador (con prefijo de nivel de liga y país, ej. `1.eng Leeds United`) |
| `Age` | Edad | Edad al inicio del torneo |
| `Born` | Año de nacimiento | Año de nacimiento |
| `90s` | Partidos de 90' | $90s = \dfrac{Min}{90}$ — minutos jugados convertidos a equivalente de partidos completos |
| `Matches` (última columna, hash) | Identificador único de jugador | Llave interna de FBref para vincular filas del mismo jugador entre tablas; no es una estadística |

### Tabla 1 — Player Standard Stats → *Estadísticas Estándar de Jugador*

| Columna (EN) | Traducción (ES) | Descripción |
|---|---|---|
| `MP` | Partidos jugados | Partidos jugados por el jugador |
| `Starts` | Titularidades | Partidos en los que fue titular |
| `Min` | Minutos | Minutos jugados |
| `Gls` | Goles | Goles anotados |
| `Ast` | Asistencias | Asistencias |
| `G+A` | Goles + Asistencias | $G{+}A = Gls + Ast$ |
| `G-PK` | Goles sin penal | $G{-}PK = Gls - PK$ |
| `PK` | Penales anotados | Penales convertidos |
| `PKatt` | Penales intentados | Penales pateados (anotados o no) |
| `CrdY` | Tarjetas amarillas | Tarjetas amarillas |
| `CrdR` | Tarjetas rojas | Tarjetas rojas |
| `Gls` *(por 90)* | Goles por 90' | $Gls/90 = \dfrac{Gls}{90s}$ |
| `Ast` *(por 90)* | Asistencias por 90' | $Ast/90 = \dfrac{Ast}{90s}$ |
| `G+A` *(por 90)* | Goles+Asist. por 90' | $(G{+}A)/90 = \dfrac{Gls+Ast}{90s}$ |
| `G-PK` *(por 90)* | Goles sin penal por 90' | $(G{-}PK)/90 = \dfrac{Gls-PK}{90s}$ |
| `G+A-PK` *(por 90)* | Goles+Asist. sin penal por 90' | $(G{+}A{-}PK)/90 = \dfrac{Gls+Ast-PK}{90s}$ |

### Tabla 2 — Player Shooting → *Disparos de Jugador*

| Columna (EN) | Traducción (ES) | Descripción |
|---|---|---|
| `Sh` | Disparos totales | Disparos totales |
| `SoT` | Disparos al arco | Disparos al arco |
| `SoT%` | % Disparos al arco | $SoT\% = \dfrac{SoT}{Sh}\times 100$ |
| `Sh/90` | Disparos por 90' | $Sh/90 = \dfrac{Sh}{90s}$ |
| `SoT/90` | Disparos al arco por 90' | $SoT/90 = \dfrac{SoT}{90s}$ |
| `G/Sh` | Goles por disparo | $G/Sh = \dfrac{Gls}{Sh}$ |
| `G/SoT` | Goles por disparo al arco | $G/SoT = \dfrac{Gls}{SoT}$ |
| `PK` / `PKatt` | Penales anotados / intentados | Igual que en Standard Stats |

### Tabla 3 — Player Playing Time → *Tiempo de Juego de Jugador*

| Columna (EN) | Traducción (ES) | Descripción |
|---|---|---|
| `Mn/MP` | Minutos por partido jugado | $Mn/MP = \dfrac{Min}{MP}$ |
| `Min%` | % de minutos del equipo | $Min\% = \dfrac{\text{minutos del jugador}}{\text{minutos totales del equipo}}\times 100$ |
| `Mn/Start` | Minutos por titularidad | $Mn/Start = \dfrac{Min}{Starts}$ |
| `Compl` | Partidos completos | Partidos jugados de inicio a fin sin ser sustituido |
| `Subs` | Apariciones de cambio | Partidos entrando desde el banco |
| `Mn/Sub` | Minutos por sustitución | $Mn/Sub = \dfrac{\text{minutos como suplente}}{Subs}$ |
| `unSub` | Suplente no utilizado | Partidos en el banco sin entrar |
| `PPM` | Puntos por partido | Promedio de puntos del equipo en los partidos donde jugó el jugador |
| `onG` | Goles a favor (en cancha) | Goles del equipo mientras el jugador estaba en cancha |
| `onGA` | Goles en contra (en cancha) | Goles recibidos por el equipo mientras el jugador estaba en cancha |
| `+/-` | Diferencial | $+/{-} = onG - onGA$ |
| `+/-90` | Diferencial por 90' | $(+/{-})/90 = \dfrac{onG-onGA}{90s}$ |
| `On-Off` | Diferencial neto con/sin el jugador | Diferencial de goles por 90' del equipo con el jugador en cancha, menos el diferencial por 90' con el jugador fuera |

### Tabla 4 — Player Miscellaneous Stats → *Estadísticas Misceláneas de Jugador*

| Columna (EN) | Traducción (ES) | Descripción |
|---|---|---|
| `CrdY` / `CrdR` | Amarillas / Rojas | Igual que en Standard Stats |
| `2CrdY` | Doble amarilla | Expulsión por segunda tarjeta amarilla |
| `Fls` | Faltas cometidas | Faltas cometidas |
| `Fld` | Faltas recibidas | Faltas recibidas |
| `Off` | Fuera de lugar | Posiciones de offside |
| `Crs` | Centros | Centros/cruces al área |
| `Int` | Intercepciones | Intercepciones |
| `TklW` | Entradas ganadas | Entradas (tackles) en las que el equipo recuperó el balón |
| `PKwon` | Penales ganados | Penales provocados a favor |
| `PKcon` | Penales concedidos | Penales cometidos en contra |
| `OG` | Autogoles | Autogoles |

### Tabla 5 — Player Goalkeeping → *Estadísticas de Portero*

| Columna (EN) | Traducción (ES) | Descripción |
|---|---|---|
| `GA` | Goles recibidos | Goles encajados |
| `GA90` | Goles recibidos por 90' | $GA/90 = \dfrac{GA}{90s}$ |
| `SoTA` | Disparos al arco recibidos | Disparos al arco enfrentados |
| `Save%` | % de atajadas | $Save\% = \dfrac{SoTA-GA}{SoTA}\times 100$ — no todos los disparos al arco los detiene el portero, algunos los despejan defensas |
| `W` / `D` / `L` | Victorias / Empates / Derrotas | Resultados de los partidos jugados por el portero |
| `CS` | Arco en cero | Partidos completos sin recibir gol |
| `CS%` | % Arco en cero | $CS\% = \dfrac{CS}{MP}\times 100$ |
| `PKatt` | Penales enfrentados | Penales pateados en contra |
| `PKA` | Penales recibidos | Penales convertidos en contra |
| `PKsv` | Penales atajados | Penales atajados |
| `PKm` | Penales fallados (rival) | Penales que el rival falló (no al arco) |
| `Save%` *(penales)* | % de atajadas en penales | $Save\%_{PK} = \dfrac{PKA}{PKatt}$ — penales fuera del arco no cuentan |

### Tabla 6 — Scores & Fixtures → *Resultados y Calendario*

| Columna (EN) | Traducción (ES) | Descripción |
|---|---|---|
| `Round` | Fase/Ronda | Fase del torneo (Fase de grupos, Ronda de 32, Octavos, Cuartos, Semifinal, Tercer puesto, Final) |
| `Wk` | Jornada | Número de jornada (solo aplica en fase de grupos) |
| `Day` | Día | Día de la semana |
| `Date` | Fecha | Fecha local del partido |
| `Time` | Hora | Hora local de la sede, formato 24h |
| `Score` | Marcador | Los números entre paréntesis corresponden a la tanda de penales, no a goles de juego |
| `Attendance` | Asistencia | Espectadores en el estadio |
| `Venue` | Sede | Estadio |
| `Referee` | Árbitro | Árbitro principal |
