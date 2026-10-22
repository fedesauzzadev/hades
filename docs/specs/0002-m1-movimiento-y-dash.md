# Spec 0002 — M1: movimiento y dash con i-frames

| Campo | Valor |
|---|---|
| ID | 0002 |
| Hito | M1 |
| Estado | Aprobada (lista para implementar) |
| Dependencias | 0001 |

## Objetivo

Un héroe placeholder (círculo/cápsula) se mueve en 8 direcciones con velocidad constante
y ejecuta un dash corto con invulnerabilidad y cooldown, con los valores reales extraídos
del juego original.

## Contexto

Primer sistema de gameplay. Valores reales ya extraídos en
[docs/research/hero.md](../research/hero.md) (Hades instalado en
`C:\Program Files (x86)\Steam\steamapps\common\Hades`).

## Requisitos funcionales

- RF1: Input WASD/flechas mueve al héroe en 8 direcciones; la diagonal se normaliza.
- RF2: Input de dash (Space/Shift) produce un desplazamiento "blink" rápido y frenado
  (eased) en la dirección actual de movimiento (o la última dirección mirada si está quieto).
- RF3: Durante el dash el héroe es invulnerable (flag `IsInvulnerable` consultable) y
  la invulnerabilidad se extiende levemente tras el fin del desplazamiento.
- RF4: El dash tiene 1 carga con recarga; inputs durante la recarga no hacen nada.
- RF5: El héroe tiene una dirección "mirada" que luego usará el combate.
- RF6: Moverse en dirección opuesta a la mirada aplica penalización de velocidad
  (backpedal), como en el original.

## Requisitos no funcionales

- RNF1: El héroe es una escena `Player.tscn` con script propio; nada de lógica en `Main`.
- RNF2: Parámetros de movimiento y dash expuestos como `[Export]` para tuning en editor.
- RNF3: `_PhysicsProcess` para movimiento (delta-correcto).
- RNF4: `Main.tscn` incluye una zona de daño de prueba (Area2D) para verificar i-frames.

## Parámetros de diseño (valores reales — research/hero.md)

| Parámetro | Valor | Fuente |
|---|---|---|
| Velocidad de movimiento | 540 px/s | PlayerUnits.sjson `Speed` |
| Aceleración | 2000 px/s² | PlayerUnits.sjson `Acceleration` |
| Modificador backpedal | 0.34 | PlayerUnits.sjson `BackpedalSpeedModifier` |
| Duración del dash | 0.21 s | PlayerWeapons.sjson `BlinkDuration` |
| Distancia del dash | 400 px máx. | PlayerWeapons.sjson `BlinkMaxRange` |
| Frenado del dash (ease out) | 0.8 | PlayerWeapons.sjson `BlinkEaseOut` |
| i-frames del dash | 0.24 s | `RushWeaponInvulnerable.Duration` |
| Recarga del dash | 0.74 s | PlayerWeapons.sjson `ClipRegenInterval` |
| Cargos de dash | 1 | PlayerWeapons.sjson `ClipSize` |
| Post-dash: lentitud | ×0.65 durante 0.25 s (cancelable al atacar) | `RushWeaponSelfSlow` (M3) |
| i-frames tras recibir daño | 1.3 s (para M3/M4) | HeroData.lua `InvulnerableFrameDuration` |

## Criterios de aceptación

- [ ] CA1: Mover en diagonal no es más rápido que en línea recta.
- [ ] CA2: El dash atraviesa sin recibir daño la zona de peligro de prueba; caminar
      dentro de ella sí registra un "daño" (log en consola).
- [ ] CA3: Durante la recarga (0.74 s), presionar dash no produce dash.
- [ ] CA4: Con el héroe quieto, el dash va hacia la última dirección mirada.
- [ ] CA5: Sin input, el héroe se frena rápido (sin deslizamiento largo).
- [ ] CA6: Retroceder respecto de la mirada es visiblemente más lento (×0.34).
- [ ] CA7: `dotnet build` sin errores ni warnings.

## Notas de implementación

- `CharacterBody2D` + `MoveAndSlide`; acelerar hacia la velocidad objetivo (accel 2000).
- Dash: mover la posición con curva eased durante 0.21 s (tween manual en
  `_PhysicsProcess`, no `Tween` de escena) manteniendo el cálculo en física.
- Máquina de estados mínima: `Normal | Dashing | PostDash` (enum en el Player).
- Zona de peligro de prueba: `Area2D` + `CollisionShape2D` que llama a
  `Player.RegisterHit()` si `!IsInvulnerable` (log por ahora; HP real en M3/M4).
- Debug visual opcional: parpadeo del sprite durante i-frames.
