# AGENTS.md — Katabasis

Clon educativo de mecánicas de Hades. Godot 4.7.2 (.NET) + C# 8.0. Assets placeholder originales
(NUNCA usar assets del juego original). Sin comentarios en el código.

## Comandos

| Acción | Comando |
|---|---|
| Compilar | `cd game; dotnet build` (TreatWarningsAsErrors=true) |
| Correr headless | `godot --headless --path game --quit` |
| Editor | `godot --path game` |
| Editor exe | `C:\Users\hp\tools\godot\godot.exe` (edición .NET, en PATH) |

## Estructura

- `BACKLOG.md` — cola de ejecución (fuente de verdad de qué toca).
- `.opencode/plan/<slug>.md` — planes ejecutables (`/spec` → `/clear` → `/ejecutar <slug>`).
- `docs/specs/` — specs de diseño por hito (ROADMAP.md M0–M10) · `docs/research/` — datos extraídos del juego original con fuente exacta · `docs/decisions/` — ADRs.
- `game/` — proyecto Godot: `scripts/` (C#, namespace `Katabasis`, clases `partial`), `scenes/`, `data/`.
- Parámetros de gameplay: valores REALES del juego original (ver `docs/research/`), nunca inventados.

## Juego original (solo lectura — research)

`C:\Program Files (x86)\Steam\steamapps\common\Hades`
- Lógica gameplay: `Content\Scripts\*.lua` (HeroData, WeaponData, EnemyData, TraitData, RoomData*).
- Stats de engine: `Content\Game\Units\PlayerUnits.sjson`, `Content\Game\Weapons\PlayerWeapons.sjson`,
  `Content\Game\Units\Enemies.sjson`.
- NO copiar assets (arte/audio/texto) ni pegar código Lua: solo extraer números/estructura a `docs/research/`.

## Reglas

- Git: commit SOLO cuando el usuario lo pida explícitamente.
- Cada sesión `/ejecutar` cierra actualizando `BACKLOG.md` y `docs/specs/ROADMAP.md`.
- Unidad: 1 unidad de Hades ≈ 1 px de Godot.
