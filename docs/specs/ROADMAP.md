# Roadmap de hitos

> Ejecución paso a paso: ver `BACKLOG.md` (cola de slugs para `/ejecutar`).

Estado: `Pendiente` · `En curso` · `Hecho` · `Bloqueado`

| Hito | Nombre | Spec | Estado | Depende de |
|---|---|---|---|---|
| M0 | Proyecto base Godot + C# compila y corre | [0001](0001-m0-proyecto-base.md) | Hecho | — |
| M1 | Movimiento y dash con i-frames | [0002](0002-m1-movimiento-y-dash.md) | En curso | M0 |
| M2 | Arena, cámara y colisiones | (a escribir) | Pendiente | M1 |
| M3 | Combate melee: ataque, especial, hitstop, knockback | (a escribir) | Pendiente | M2 |
| M4 | Enemigos: spawn, IA básica, telegraph, muerte | (a escribir) | Pendiente | M3 |
| M5 | Encuentros por oleadas, lock/unlock de sala, puertas | (a escribir) | Pendiente | M4 |
| M6 | Data-driven: armas/enemigos/salas como recursos | (a escribir) | Pendiente | M5 |
| M7 | Carrera: biomas múltiples, jefe, economía, tienda | (a escribir) | Pendiente | M6 |
| M8 | Boons de dioses (mejoras modulares + sinergias) | (a escribir) | Pendiente | M7 |
| M9 | Meta-progresión: hub, mejoras permanentes, guardado | (a escribir) | Pendiente | M8 |
| M10 | Pulido: game feel, UI, balance con datos reales | (a escribir) | Pendiente | M9 |

## Investigación (paralelo a los hitos)

| Tarea | Estado |
|---|---|
| Instalar Hades y ubicar `Content/Scripts` | Hecho (2026-10-22) |
| Volcar datos de movimiento/dash del héroe | Hecho → [research/hero.md](../research/hero.md) |
| Volcar datos de armas y enemigos (bioma 1) | Pendiente (M3/M4) |
| Setup de `rea` para análisis del ensamblado C# | Opcional (los `.sjson` + Lua cubren lo necesario) |

Ver [docs/research/README.md](../research/README.md).
