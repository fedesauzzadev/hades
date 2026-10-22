# SPEC maestra — Katabasis

| Campo | Valor |
|---|---|
| Nombre interno | Katabasis (κατάβασις, "el descenso") |
| Género | Roguelike de acción top-down/isométrico |
| Inspiración | Hades (Supergiant Games, 2020) — mecánicas, no assets |
| Motor | Godot 4.7.2 (edición .NET) |
| Lenguaje | C# / .NET 8 |
| Plataforma objetivo | Windows (desktop) |

## Visión

Un clon jugable y completo del loop de Hades: un héroe desciende por biomas generados
por salas encadenadas, combate en tiempo real con arma melee + especial, obtiene
"boons" (mejoras modulares) de distintos dioses, muere, vuelve al hub, gasta recursos
en mejoras permanentes y reintenta con más poder. Todo con arte placeholder original.

## Objetivos (alcance completo)

1. Movimiento 8 direcciones + dash con invulnerabilidad (i-frames).
2. Combate: ataque, especial, knockback, hitstop, telegraphs enemigos.
3. Sistema de salas: encuentros por oleadas, lock/unlock, puertas con recompensas.
4. Enemigos variados por bioma + al menos 1 jefe.
5. Economía de carrera (monedas) y tienda.
6. Boons: mejoras modulares data-driven con sinergias simples.
7. Meta-progresión: hub, árbol de mejoras permanentes, guardado local.
8. Múltiples armas jugables (al menos 3).

## No-objetivos

- NO usar ni redistribuir assets de Supergiant (arte, audio, texto, música, nombres con marca).
- NO replicar 1:1 los números del juego: los datos extraídos se usan como referencia
  de diseño y se ajustan por jugabilidad.
- NO multiplayer. NO localización. NO ports a consola.

## Principios

1. **Spec-driven**: cada feature nace de una spec con criterios de aceptación verificables.
2. **Data-driven**: enemigos, armas, salas, boons y rewards se definen como recursos
   (archivos de datos), no hardcodeados.
3. **Placeholder original**: todo asset es generado (formas, colores) hasta fase de pulido.
4. **Referencia limpia**: los datos de ingeniería inversa se documentan en `docs/research/`
   con su fuente (archivo Lua del juego original); el código se escribe desde cero.
5. **Pasos chicos**: cada spec es implementable y verificable en una sesión.

## Definición de Done (por spec)

- Criterios de aceptación todos cumplidos y tildados en la spec.
- `dotnet build` sin errores ni warnings nuevos.
- Probado manualmente en el editor (o test automático si la spec lo pide).
- ROADMAP y estado de spec actualizados.

## Roadmap

Ver [docs/specs/ROADMAP.md](docs/specs/ROADMAP.md). Hitos M0–M10.
