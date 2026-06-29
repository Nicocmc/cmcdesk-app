# Identidad visual de CmcDesk

Tomada de **cmctech.ar** (variables CSS reales) para aplicar en el build propio (Fase 2B).

## Paleta de colores

| Rol | Variable | Hex |
|-----|----------|-----|
| Primario (teal) | `--teal` | `#00babe` |
| Primario suave | `--teal-soft` | `#2fd6da` |
| Acento (coral) | `--coral` | `#ff6162` |
| Acento suave | `--coral-soft` | `#ff8b8c` |
| Texto principal | `--ink` | `#eaf0fb` |
| Texto atenuado | `--ink-mut` | `#97a4bd` |
| Texto tenue | `--ink-dim` | `#5e6a83` |
| Fondo base | — | `#06121b` |
| Fondo profundo | — | `#0a0e1c` / `#05070f` |

Estilo: **dark theme** con glassmorphism (superficies translúcidas
`rgba(255,255,255,0.045)`), bordes sutiles y acentos teal + coral.

## Tipografías

- **Títulos / display:** `Space Grotesk`, fallback `Montserrat`
- **Cuerpo:** `Poppins`
- **Monoespaciada:** `JetBrains Mono`

## Logos

| Archivo | Dimensiones | Uso sugerido |
|---------|-------------|--------------|
| `logos/cmc-logo.png` | 1080×861 (RGBA) | Logo horizontal (splash, cabecera) |
| `logos/logocmc.png` | 1024×1024 (RGBA) | Ícono de la app (cuadrado) |

## Dónde se aplica en el build de RustDesk (Fase 2B)

- **Nombre del producto / APP_NAME** → `CmcDesk`
- **Ícono** → `logocmc.png` (se generan los `.ico`/`.png` por tamaño)
- **Color de acento del tema** → `#00babe` (teal)
- **Logo del about/splash** → `cmc-logo.png`
- **Tema por defecto** → oscuro
