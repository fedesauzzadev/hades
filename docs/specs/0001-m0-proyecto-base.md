# Spec 0001 — M0: proyecto base Godot + C#

| Campo | Valor |
|---|---|
| ID | 0001 |
| Hito | M0 |
| Estado | Done |
| Dependencias | — |

## Objetivo

El repo tiene un proyecto Godot 4.7.2 con C# que compila, corre y muestra la escena
base, más toda la estructura spec-driven operativa.

## Requisitos funcionales

- RF1: `game/project.godot` abre en Godot 4.7.2 con soporte C# habilitado.
- RF2: La escena principal `res://scenes/Main.tscn` carga e imprime en consola al arrancar.
- RF3: Al cerrar el juego no hay errores en la consola de Godot.

## Requisitos no funcionales

- RNF1: `dotnet build` en `game/` compila sin errores ni warnings.
- RNF2: Estructura de carpetas lista para M1: `game/scripts/`, `game/scenes/`, `game/data/`.
- RNF3: Documentación del repo completa (SPEC, ROADMAP, TEMPLATE, ADR-0001, research/README).

## Parámetros de diseño

| Parámetro | Valor | Fuente |
|---|---|---|
| Nombre del proyecto | Katabasis | SPEC.md |
| TargetFramework | net8.0 | ADR-0001 |

## Criterios de aceptación

- [x] CA1: Existe `game/project.godot` con features `4.7` + `C#`.
- [x] CA2: `dotnet build` en `game/` termina en 0 errores.
- [x] CA3: `godot --headless --path game --quit` corre sin errores (verifica edición .NET).
  Verificado 2026-10-22: edición .NET instalada, imprime `Katabasis M0 — proyecto base OK`.
- [x] CA4: La escena Main carga e imprime al arrancar (verificado headless; confirmación
      visual en editor F5 pendiente por parte del usuario).

## Notas de implementación

- Escena `Main.tscn`: Node2D raíz con script `Main.cs`.
- Si CA3 falla con "build without .NET support", el usuario debe descargar la edición
  .NET de Godot 4.7.2 (godotengine.org/download) y reemplazar `C:\Users\hp\tools\godot\`.
