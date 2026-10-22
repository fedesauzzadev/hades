# Katabasis — roguelike de acción inspirado en Hades (Godot 4 + C#)

Proyecto educativo: estudiamos las **mecánicas** de Hades (Supergiant Games) mediante
ingeniería inversa y las re-implementamos desde cero como juego original.

> ⚠️ Proyecto personal/educativo. **No** incluye ni redistribuye assets de Supergiant
> (arte, audio, texto, música). Todo el contenido visual y sonoro es placeholder u original.

## Stack

| Pieza | Versión |
|---|---|
| Godot (edición .NET) | 4.7.2 |
| C# / .NET SDK | 8.0 |
| `rea` (investigación del juego original) | npm `rea-agents` (requiere Node ≥ 24.11) |

## Estructura del repo

```
C:\repos\Hades\
├── SPEC.md                  # Especificación maestra (visión, alcance, principios)
├── docs/
│   ├── specs/               # Specs incrementales (TEMPLATE.md, ROADMAP.md)
│   ├── decisions/           # ADRs: decisiones de arquitectura
│   └── research/            # Datos extraídos del juego original (fórmulas, stats)
└── game/                    # Proyecto Godot
```

## Workflow (spec-driven incremental)

1. Elegir el próximo ítem del [ROADMAP](docs/specs/ROADMAP.md).
2. Escribir o actualizar la spec en `docs/specs/` usando `TEMPLATE.md`.
3. Implementar en `game/` cumpliendo los criterios de aceptación.
4. Verificar: `dotnet build` sin errores + prueba manual en el editor.
5. Marcar la spec como **Done**, actualizar ROADMAP y registrar ADRs si hubo decisiones.

Regla de oro: **nada de código sin spec previa; nada de spec sin criterio de aceptación verificable.**

## Cómo correr

```powershell
cd game
dotnet build
godot --path .        # o abrir game/project.godot desde el editor
```

## Investigación del juego original

Ver [docs/research/README.md](docs/research/README.md). Requiere tener Hades instalado
(aún pendiente de compra/instalación).
