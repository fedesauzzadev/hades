# BACKLOG — cola de ejecución paso a paso

Fuente de verdad para la ejecución. Cada ítem = una sesión `/ejecutar` (atómico y verificable).
Estado: `[x]` hecho · `[~]` en curso · `[ ]` pendiente.

Flujo por ítem:
1. Si no tiene plan: `/spec <slug> <descripcion>` → genera `.opencode/plan/<slug>.md`.
2. `/clear` → `/ejecutar <slug>`.
3. Verificado → marcar `[x]` acá y en `docs/specs/ROADMAP.md`.

| Estado | Slug | Hito | Descripción |
|---|---|---|---|
| [~] | `m1-movimiento-dash` | M1 | Movimiento 8-dir + dash blink con i-frames (spec 0002). Plan listo. |
| [ ] | `m2-arena-camara` | M2 | Arena de prueba con paredes, cámara que sigue al jugador, colisiones. |
| [ ] | `research-armas` | M3 | Volcar `docs/research/weapons.md` desde PlayerWeapons.sjson + WeaponData.lua. |
| [ ] | `m3-combate-melee` | M3 | Ataque y especial de espada: hitbox, daño, knockback, hitstop. |
| [ ] | `research-enemigos-tartarus` | M4 | Volcar `docs/research/enemies-biome1.md` desde EnemyData.lua + Enemies.sjson. |
| [ ] | `m4-enemigos` | M4 | Enemigo melee básico: spawn, persecución, telegraph, HP, muerte. |
| [ ] | `m5-salas-encuentros` | M5 | Encuentros por oleadas, lock/unlock de sala, puertas. |
| [ ] | `m6-data-driven` | M6 | Armas/enemigos/salas como recursos Godot (no hardcode). |
| [ ] | `research-boons` | M8 | Volcar `docs/research/boons.md` desde TraitData.lua (estructura + rarity). |
| [ ] | `m7-carrera-biomas` | M7 | Encadenar salas, economía, tienda, 2° bioma, jefe. |
| [ ] | `m8-boons` | M8 | Sistema de boons data-driven con sinergias simples. |
| [ ] | `m9-meta-progresion` | M9 | Hub, mejoras permanentes, guardado local. |
| [ ] | `m10-pulido` | M10 | Game feel, UI, balance con datos reales. |

Notas:
- Los hitos se pueden subdividir en más slugs si un plan queda grande (regla: 1 sesión = 1 ítem).
- Pendiente aparte: **commit inicial** del repo — solo cuando el usuario lo ordene.
- Vista estratégica: `docs/specs/ROADMAP.md`. Diseño y research: `docs/`.
