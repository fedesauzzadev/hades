# M1 — Movimiento y dash con i-frames

Spec de diseño: `docs/specs/0002-m1-movimiento-y-dash.md`. Research: `docs/research/hero.md`.

## Objetivo

Héroe placeholder (CharacterBody2D) que se mueve en 8 direcciones con valores reales de
Hades y ejecuta un dash tipo blink con i-frames y recarga, verificable en una escena de
prueba con zona de peligro.

## Criterios de aceptación

- [ ] CA1: La diagonal no es más rápida que la línea recta (input normalizado).
- [ ] CA2: Caminar dentro de la zona roja registra daño en log; hacer dash a través de ella no.
- [ ] CA3: Durante los 0.74 s de recarga, presionar dash no hace nada.
- [ ] CA4: Quieto, el dash va hacia la última dirección mirada.
- [ ] CA5: Sin input se frena rápido (sin deslizamiento largo).
- [ ] CA6: Retroceder respecto de la mirada es ~3× más lento (×0.34).
- [ ] CA7: `dotnet build` sin errores NI warnings; `godot --headless --path . --quit` sin errores.

## Parámetros (extraídos del juego original — NO inventar)

| Parámetro | Valor |
|---|---|
| MoveSpeed | 540 |
| Acceleration | 2000 |
| BackpedalModifier | 0.34 |
| DashDuration | 0.21 s |
| DashDistance | 400 px |
| DashIFrames | 0.24 s |
| DashCooldown (recarga) | 0.74 s |
| PostDashSlowMult / dur | 0.65 / 0.25 s |
| PostHitIFrames (RegisterHit) | 1.3 s |

## Archivos involucrados

- `game/scripts/Player.cs` (nuevo)
- `game/scenes/Player.tscn` (nueva)
- `game/scripts/HazardZone.cs` (nuevo)
- `game/scenes/Main.tscn` (modificar: instanciar Player y HazardZone)
- `game/project.godot` (modificar: agregar [input])
- `game/scripts/Main.cs` (sin cambios de lógica)

## Plan de pasos

1. **Input map**: agregar a `game/project.godot` la sección `[input]` con acciones
   `move_up`, `move_down`, `move_left`, `move_right` (WASD + flechas) y `dash`
   (Space + Shift). Serialización exacta de eventos (physical_keycode):
   W=87, A=65, S=83, D=68, UP=4194320, DOWN=4194322, LEFT=4194319, RIGHT=4194321,
   SPACE=32, SHIFT=4194325. Formato Godot 4:
   `Object(InputEventKey,"resource_local_to_scene":false,"resource_name":"","device":-1,"window_id":0,"alt_pressed":false,"shift_pressed":false,"ctrl_pressed":false,"meta_pressed":false,"pressed":false,"keycode":0,"physical_keycode":87,"key_label":0,"unicode":0,"echo":false,"script":null)`
2. **Player.tscn**: raíz `CharacterBody2D` "Player" con script `res://scripts/Player.cs`,
   hijo `CollisionShape2D` (CircleShape2D radius 24) e hijo `Polygon2D` "Visual"
   (hexágono naranja ~32px, color `Color(0.9, 0.48, 0.1, 1)`).
3. **Player.cs** (namespace `Katabasis`, clase `partial`, SIN comentarios):
   - `[Export]` de todos los parámetros de la tabla (nombres en PascalCase).
   - Enum estado: `Normal`, `Dashing`, `PostDash`.
   - `Facing` (Vector2, última dirección de movimiento no-nula; inicial (0,1)).
   - En `_PhysicsProcess` estado `Normal`: input → `Input.GetVector("move_left","move_right","move_up","move_down")`;
     velocidad objetivo = dir.Normalized() * MoveSpeed; si `dir.Dot(Facing) < -0.5` multiplicar por BackpedalModifier;
     acelerar `Velocity = Velocity.MoveToward(target, Acceleration * delta)`; `MoveAndSlide()`;
     actualizar `Facing` si dir != zero y estado Normal.
   - Dash (inicio por `Input.IsActionJustPressed("dash")` solo en `Normal` y con carga disponible):
     guardar `dashStart`, `dashDir` (dir actual o `Facing` si input cero), estado `Dashing`.
     Durante `Dashing` (t = delta acumulado / DashDuration): posición eased
     `pos = start + dashDir * DashDistance * EaseOut(u)` con `EaseOut(u) = 1f - MathF.Pow(1f - u, 3f)`,
     aplicada vía `Velocity = (posNueva - GlobalPosition) / delta` + `MoveAndSlide()`.
     Al terminar: estado `PostDash` (timer 0.25 s, multiplicador 0.65) → `Normal`.
   - Invulnerabilidad: `IsInvulnerable` = true desde inicio del dash por DashIFrames (0.24 s);
     `RegisterHit()` → si `IsInvulnerable` return; sino `GD.Print("[HIT] daño registrado")`
     + invulnerable 1.3 s (PostHitIFrames) para evitar spam del log.
   - Recarga: timer DashCooldown independiente del estado; dash solo si timer <= 0.
   - En `_Ready`: `AddToGroup("player")`.
   - Debug: `QueueRedraw()` + `Draw()` dibuja línea corta hacia `Facing`.
4. **HazardZone.cs** (Area2D, en `game/scenes/Main.tscn` como nodo "HazardZone" en posición
   (350, 0)): hijo CollisionShape2D (RectangleShape2D 220×260) + hijo Polygon2D rojo
   semitransparente `Color(1, 0.15, 0.15, 0.35)` cubriendo la misma área.
   En `_PhysicsProcess`: si `HasOverlappingBodies()` →
   `GetTree().GetFirstNodeInGroup("player") as Player` → `RegisterHit()`.
   Collision mask default (layer 1) detecta al Player.
5. **Main.tscn**: agregar instancia de `res://scenes/Player.tscn` como hijo de Main
   (ext_resource + node con script igual al patrón existente) y el HazardZone descripto.

## Comandos de verificación

```powershell
cd C:\repos\Hades\game
dotnet build        # CA7: 0 errores, 0 warnings (TreatWarningsAsErrors=true)
godot --headless --path . --quit   # sin errores de parseo/carga
```

CA1–CA6: prueba manual en editor (`godot --path .` → F5), tildar en la spec 0002.

Al cerrar: marcar `m1-movimiento-dash` como `[x]` en `BACKLOG.md`, M1 como Hecho en
`docs/specs/ROADMAP.md`, y estado Done en `docs/specs/0002-m1-movimiento-y-dash.md`.
