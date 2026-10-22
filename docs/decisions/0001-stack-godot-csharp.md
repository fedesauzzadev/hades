# ADR-0001: Stack Godot 4.7 (.NET) + C#

| Campo | Valor |
|---|---|
| Estado | Aceptada |
| Fecha | 2026-10-22 |
| Contexto | SPEC.md (arranque del proyecto) |

## Decisión

Motor **Godot 4.7.2 edición .NET**, lenguaje **C# (net8.0)** para todo el gameplay.

## Opciones consideradas

| Opción | Veredicto | Motivo |
|---|---|---|
| C# | ✔ Elegido | Soporte oficial de Godot 4; tipado estático; rendimiento para cientos de proyectiles; SDK .NET 8 ya instalado; escalable. |
| GDScript | Descartado | Más fácil y rápido de iterar, pero menos tipado y menos performance para un action-roguelike denso. Reconsiderar si la fricción con C# frena el avance. |
| Go | Descartado | Godot no lo soporta oficialmente; bindings comunitarios inmaduros; fricción de build/toolchain. |
| Unity/Gamemaker | Descartado | El usuario eligió Godot (liviano, gratis, open source). |

## Consecuencias

- Requiere la edición .NET de Godot (verificado en spec 0001 / CA3).
- NuGet `Godot.NET.Sdk` fija la versión del engine en el `.csproj`.
- Data-driven se implementará con recursos de Godot (`Resource` + `[Export]`), no JSON externo,
  salvo que el volumen de datos lo justifique (revisar en M6).
