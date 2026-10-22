# Research: héroe (movimiento y dash)

> Fuente: Hades v1.x (Steam). Todos los valores con archivo y línea exactos.
> Unidades: unidades de mundo del engine de Hades. En Katabasis usamos 1 unidad ≈ 1 px de Godot (ajustable por zoom de cámara).

## Movimiento — `_PlayerUnit`

Fuente: `Content\Game\Units\PlayerUnits.sjson:15-92`

| Propiedad | Valor | Nota |
|---|---|---|
| Speed | **540 u/s** | velocidad base de caminar/correr |
| Acceleration | 2000 | aceleración hacia la velocidad objetivo |
| BackpedalSpeedModifier | **0.34** | retroceder respecto de la dirección mirada = 34% de la velocidad |
| FullControlVelocityThreshold | 400 | debajo de esta velocidad hay control total inmediato |
| HighSpeedThreshold | 700 | umbral animación correr vs caminar |
| TerminalVelocity | 5000 | tope de velocidad por fuerzas externas |
| Colisión (Points/Size) | octógono ~104×64 | hitbox más ancha que profunda (top-down) |
| Scale (sprite) | 0.7 | — |

## Dash — `RushWeapon` (el dash es un arma)

Fuente: `Content\Game\Weapons\PlayerWeapons.sjson:7-150`

| Propiedad | Valor | Nota |
|---|---|---|
| Type / Control | BLINK / Rush | desplazamiento "blink" deslizante, no velocidad constante |
| BlinkDuration | **0.21 s** | duración del dash |
| BlinkMaxRange | **400 u** | distancia máxima (WeaponRange 300) |
| BlinkEaseOut | 0.8 | el dash frena al final (curva eased) |
| ClipSize | **1** | 1 carga base (2 con Espejo, ver MetaUpgradeData) |
| ClipRegenInterval | **0.74 s** | recarga de la carga (cooldown real del dash) |
| Cooldown | 0.05 s | micro-cooldown entre usos |
| i-frames (RushWeaponInvulnerable) | **0.24 s** | INVULNERABLE, ligeramente más largo que el dash |
| ImmuneToForce | 0.24 s | inmune a knockback durante el dash |
| Post-dash SelfSlow | 0.65 × 15 frames (~0.25 s) | lentitud de recuperación al terminar |
| Post-dash DisableMove | 12 frames (~0.2 s) | cancelable (atacar cancela) |
| BlinkNumGhosts | 3 | los 3 "fantasmas" del rastro visual |
| CheckDanger / CheckMovementReactionDistance | true / 150 | sistema de "dash perfecto" (esquiva reactiva) |

> `DurationFrames` asume lógica a 60 fps (15 frames ≈ 0.25 s).

## Héroe general — `DefaultHero`

Fuente: `Content\Scripts\HeroData.lua:1-120`

| Propiedad | Valor | Nota |
|---|---|---|
| MaxHealth | **50** | vida base |
| DefaultWeapon | SwordWeapon | espada inicial |
| InvulnerableFrameDuration | **1.3 s** | i-frames tras recibir daño |
| InvulnerableFrameMinDamage | 10 | umbral para i-frames de bajo daño |
| PerfectDashHitDisableDuration | 0.3 s | ventana del dash perfecto |
| DashManeuverTimeThreshold | 0.4 s | ventana para encadenar input |

## Decisiones de diseño para Katabasis (spec 0002)

- Mover a 540 px/s con `CharacterBody2D` y aceleración alta (2000) para respuesta casi inmediata.
- Backpedal ×0.34: **sí** (define mucho del feel de combate de Hades).
- Dash: desplazamiento eased de 400 px en 0.21 s (implementar como tween de posición, no velocidad).
- i-frames de dash: 0.24 s desde el inicio; cooldown 0.74 s; 1 carga.
- Post-dash: multiplicador 0.65 durante 0.25 s (cancelable al atacar — se conecta con M3).
