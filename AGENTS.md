# AGENTS.md

Repositorio especial de GitHub: el `README.md` de este repo se renderiza como la portada del perfil [@jenarvaezg](https://github.com/jenarvaezg). No hay código, solo el README + `Profile.pdf`.

## Reglas duras

### Featured Projects y Advent of Code son generados

Las secciones marcadas con `<!-- PROJECTS:START --> ... <!-- PROJECTS:END -->` y `<!-- AOC:START --> ... <!-- AOC:END -->` en `README.md` **se generan automáticamente** desde el repositorio del portfolio:

```
../jenarvaezg.github.io/src/data/projects.json
../jenarvaezg.github.io/src/data/aoc.json
```

#### Flujo normal (automático)

1. Editas `src/data/projects.json` o `src/data/aoc.json` en `jenarvaezg.github.io`.
2. Push a `main`.
3. El workflow `.github/workflows/sync-profile.yml` del portfolio:
   - Checkout de este repo.
   - Regenera el README con el script de sync.
   - Abre (o actualiza) un PR rolling en la rama `sync/profile-readme` y habilita auto-merge.
4. El PR se mergea solo en cuanto pasen los checks (este repo no tiene checks, así que es inmediato).

**No hay que hacer nada manual.** Tu único trabajo es editar los JSONs en el portfolio.

#### Setup inicial (una sola vez)

- Crear PAT fine-grained en GitHub limitado a `jenarvaezg/jenarvaezg` con permisos `Contents: write` + `Pull requests: write`.
- Pegarlo como secret `PROFILE_SYNC_TOKEN` en `jenarvaezg.github.io` (Settings → Secrets and variables → Actions).
- En este repo: Settings → General → "Allow auto-merge" activado.
- Recordatorio en calendario para renovar el PAT antes de su expiración (default 1 año).

#### Flujo manual (fallback)

Si el Action está caído o quieres previsualizar el cambio:

```bash
cd ../jenarvaezg.github.io
npm run sync:profile                                       # dry run, imprime a stdout
npm run sync:profile -- --write ../jenarvaezg/README.md    # escribe local y commiteas a mano
```

**Nunca** editar a mano el contenido entre los marcadores `PROJECTS:START/END` o `AOC:START/END`. Cualquier cambio manual se perderá en el siguiente sync automático.

### Resto del README

El resto del contenido (intro, tech stack, conexión) sí se edita a mano en `README.md`. Es contenido curado para la portada del perfil, no para audiencia técnica como el portfolio.

### Idioma

El README del perfil es **solo en inglés**. La versión bilingüe es responsabilidad del portfolio (`jenarvaezg.github.io`).

## Convenciones del repo

- **Commits**: conventional commits — `docs: update featured projects`, `chore: refresh AoC table`.
- **No añadir build steps**: este repo debe seguir siendo trivial. Cualquier lógica vive en `jenarvaezg.github.io`.
- **`Profile.pdf`**: CV en PDF, trackeado en el repo y enlazado desde la sección Connect del README. **El mismo archivo se sirve también desde `jenarvaezg.github.io/public/cv.pdf`** (visible publicamente en `https://jenarvaezg.github.io/cv.pdf` y descargable desde el botón del hero). Cuando actualices el CV, **reemplaza ambos archivos en el mismo PR** (o secuencia de commits) para evitar drift.

## Estructura

```
.
├── README.md       # Portada del perfil de GitHub (con marcadores HTML para sync)
├── Profile.pdf     # CV descargable
└── AGENTS.md       # Este archivo
```
