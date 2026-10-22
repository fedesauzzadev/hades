# Investigación del juego original (Hades)

Guía para extraer datos de diseño del juego original y volcarlos acá como referencia.
**Estado: bloqueado hasta instalar Hades** (pendiente de compra/instalación).

## Fuentes de datos (en orden de utilidad)

1. **Scripts Lua del juego** — Hades implementa casi toda la lógica de gameplay en Lua
   legible, en `<instalación>/Content/Scripts/` (ej: `WeaponData.lua`, `EnemyData.lua`,
   `HeroData.lua`, `RoomData.lua`, `TraitData.lua` para boons, ` encounterData`...).
   No hace falta descompilar: son archivos de texto.
2. **`rea` (github.com/morluto/rea)** — servidor MCP para que el agente analice binarios.
   Útil para el ensamblado C#/.NET del motor cuando algo NO esté en los Lua
   (colisiones exactas, timing de engine, rendering). Instalación:
   ```powershell
   npm install --global rea-agents
   npx rea-agents setup     # registrar en tu agente (soporta varios; ver README de rea)
   ```
   Requisito: Node ≥ 24.11 (tenemos 24.19 ✓). Análisis local, MIT, sin costo propio.
3. **Wiki de modding de Hades** (community) — documentación de IDs, eventos y fórmulas.

## Reglas del área research/

- Cada volcado va en su propio `.md` con tabla + **fuente exacta** (archivo.lua y línea/sección).
- Los números son **referencia de diseño**: el juego los ajusta por dificultad/escalado;
  nosotros los retuneamos por jugabilidad (ver SPEC.md, principio 4).
- Nada de assets: solo números, fórmulas y estructura de datos.

## Volcados planeados

| Archivo | Contenido | Alimenta a |
|---|---|---|
| `hero.md` | velocidad, dash (duración, cargas, cooldown, i-frames) | spec 0002 |
| `weapons.md` | stats de armas (daño, velocidad de ataque, alcance, especial) | M3, M6 |
| `enemies-biome1.md` | HP, daño, velocidad, patrones de enemigos de Tartarus | M4 |
| `encounters.md` | estructura de oleadas y spawns | M5 |
| `rooms.md` | layout/count de salas, puertas, recompensas | M5, M7 |
| `boons.md` | estructura de TraitData: rarity, efectos, sinergias | M8 |
| `meta.md` | economía (obolos), espejo/darkness, hub | M9 |

## Checklist cuando el juego esté instalado

1. Ubicar la instalación (Steam: `...steamapps\common\Hades`) y confirmar `Content\Scripts`.
2. Copiar SOLO los `.lua` de datos necesarios a una carpeta de trabajo temporal (fuera del repo).
3. Volcar `hero.md` → desbloquear valores finales de la spec 0002.
4. Instalar `rea` y validar un primer análisis del ensamblado (opcional).
