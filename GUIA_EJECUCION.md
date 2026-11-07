# Guía de ejecución del repositorio — TPI Metodología de Sistemas II

**Orden obligatorio:** 1) Yohanna → 2) Gino → 3) Natalia.
Cada integrante empieza recién cuando el anterior terminó su `git push`.

Cada paso funciona igual: se copian los archivos de la carpeta `pasos/NN_...` dentro del
repositorio, se ejecuta la verificación, se hace el commit y se toma la captura indicada con 📸.
Las capturas tienen el mismo número que las figuras del informe.

---

## 0. Preparación (los tres)

1. Instalar **Git** (en Windows incluye *Git Bash*) y **Python 3.10 o superior**.
2. Tener cuenta de GitHub.
3. Descomprimir `kit_tpi_ms2.zip` en el **Escritorio**, de modo que quede la carpeta `Escritorio/kit_tpi_ms2`.
4. Todos los comandos se escriben en **Git Bash** (Windows: clic derecho en el Escritorio → *Open Git Bash here*) o en la Terminal (Mac/Linux).

> En Windows, la carpeta del Escritorio puede llamarse `Desktop` o `Escritorio`, y si usan OneDrive puede ser
> `~/OneDrive/Escritorio`. Ajustar la variable `KIT` si hace falta (`ls ~/Desktop` para comprobarlo).

Para Mac/Linux, reemplazar `source .venv/Scripts/activate` por `source .venv/bin/activate`
y `python` por `python3` si hace falta.

---

## 1. YOHANNA — crea el repositorio, versión inicial y diagnóstico

### 1.1 Crear el repositorio y sumar colaboradores (en la web de GitHub)

1. github.com → **New repository** → nombre: `tpi_ms2_grupo` → **Public** → **sin** README, sin .gitignore, sin licencia → *Create repository*.
   > Ya existe el repo anterior `tpi_ms2_gestion_solicitudes`. Conviene archivarlo o pasarlo a privado
   > (Settings → *Danger Zone*) para que la cátedra corrija solo el nuevo.
2. **Settings → Collaborators → Add people** → agregar el usuario de GitHub de Gino y el de Natalia.
3. Avisarles que acepten la invitación (llega por mail o en github.com/notifications).

📸 **Captura 1** — pantalla *Collaborators* con Gino y Natalia agregados.

### 1.2 Clonar y preparar el entorno

```bash
cd ~/Desktop
git clone https://github.com/YOHVASS/tpi_ms2_grupo.git
cd tpi_ms2_grupo
git config user.name "Yohanna Díaz Monroy"
git config user.email "yovassphoto@gmail.com"
KIT=~/Desktop/kit_tpi_ms2/pasos
python -m venv .venv
source .venv/Scripts/activate
```

(El aviso *"You appear to have cloned an empty repository"* es normal.)

### Paso 1 — Versión inicial del sistema

```bash
cp -r $KIT/01_yohana_estado_inicial/. .
git add -A
git commit -m "feat: prototipo inicial de gestión de solicitudes académicas"
git branch -M main
git tag estado-inicial
git push -u origin main --tags
```

Demostración de que el sistema base **funciona** (calidad externa):

```bash
python app.py init
python app.py registrar 1001 certificado alta "Constancia de examen" 2026-10-01
python app.py asignar 4 R1
python app.py estado 4 en_proceso
python app.py buscar estado=pendiente
python app.py reporte
```

📸 **Captura 2** — la terminal con los 6 comandos y sus respuestas.

Demostración de los **defectos** en los caminos de error:

```bash
python app.py estado abc resuelta
python app.py estado 4
python app.py buscar estado
```

📸 **Captura 3** — los tres errores (`ValueError`, `IndexError`, `ValueError`).

### Paso 2 — Red de seguridad (golden master)

```bash
cp -r $KIT/02_yohana_golden_master/. .
pip install -r requirements-dev.txt
python -m pytest tests/test_caracterizacion.py -v
```

Debe mostrar **2 passed**.

📸 **Captura 5** — `2 passed` sobre la versión inicial.

```bash
git add -A
git commit -m "test: pruebas de caracterización (golden master) sobre la versión inicial"
```

### Paso 3 — Diagnóstico y métricas iniciales

```bash
cp -r $KIT/03_yohana_diagnostico/. .
python -m ruff check app.py --statistics
radon cc -s -a app.py
```

Debe mostrar **Found 70 errors** y **Average complexity: C (12.8)**, con `buscar` en **D (27)**.

📸 **Captura 4** — salida de ruff (estadísticas) y de radon.

```bash
git add -A
git commit -m "docs: diagnóstico de calidad interna/externa, code smells y métricas base"
git tag etapa-diagnostico
git push origin main --tags
```

✅ **Avisar a Gino que ya puede empezar.**

---

## 2. GINO — refactorings R-01 a R-04

Antes: aceptar la invitación de Yohanna al repositorio.

```bash
cd ~/Desktop
git clone https://github.com/YOHVASS/tpi_ms2_grupo.git
cd tpi_ms2_grupo
git config user.name "Gino Canevaro"
git config user.email "EMAIL-DE-TU-CUENTA-GITHUB"
KIT=~/Desktop/kit_tpi_ms2/pasos
python -m venv .venv
source .venv/Scripts/activate
pip install -r requirements-dev.txt
```

En **cada** refactoring se corre el golden master **antes del commit**: si no da `2 passed`, no se commitea.

### Paso 4 — R-01 Extract Method + Rename Variable

```bash
cp -r $KIT/04_gino_R01_extract_method/. .
python -m pytest tests/test_caracterizacion.py
git add -A
git commit -m "refactor(R-01): Extract Method y Rename Variable (CS-02, CS-04, CS-07)"
```

### Paso 5 — R-02 Introduce Parameter Object + enums

```bash
cp -r $KIT/05_gino_R02_parameter_object/. .
python -m pytest tests/test_caracterizacion.py
git add -A
git commit -m "refactor(R-02): Introduce Parameter Object y enums del dominio (CS-03, CS-05, CS-06)"
```

### Paso 6 — R-03 Move Method + Extract Class

```bash
cp -r $KIT/06_gino_R03_move_method/. .
python -m pytest tests/test_caracterizacion.py
git add -A
git commit -m "refactor(R-03): Move Method a Solicitud y Extract Class, elimina la God Class (CS-01, CS-08, CS-09, CS-11)"
```

### Paso 7 — R-04 Strategy para reportes + tabla de comandos

```bash
cp -r $KIT/07_gino_R04_strategy/. .
python -m pytest tests/test_caracterizacion.py -v
radon cc -s -a solicitudes
```

📸 **Captura 7** — golden master `2 passed` después de los 4 refactorings + radon con **Average complexity: A (1.9)**.

```bash
git add -A
git commit -m "refactor(R-04): Strategy para reportes (OCP) y CLI con tabla de comandos (CS-06, CS-10)"
git tag etapa-refactoring
git push origin main --tags
```

En GitHub: pestaña **Commits** → abrir el commit `refactor(R-03)` (vista *Split*).

📸 **Captura 6** — diff del commit R-03 (antes en rojo / después en verde).

✅ **Avisar a Natalia que ya puede empezar.**

---

## 3. NATALIA — pruebas, CI, code review y cierre

Antes: aceptar la invitación de Yohanna al repositorio.

```bash
cd ~/Desktop
git clone https://github.com/YOHVASS/tpi_ms2_grupo.git
cd tpi_ms2_grupo
git config user.name "Natalia Gutierrez"
git config user.email "EMAIL-DE-TU-CUENTA-GITHUB"
KIT=~/Desktop/kit_tpi_ms2/pasos
python -m venv .venv
source .venv/Scripts/activate
pip install -r requirements-dev.txt
```

### Paso 8 — Pruebas unitarias, de contrato y de regresión

```bash
cp -r $KIT/08_natalia_pruebas_unitarias/. .
python -m pytest
git add -A
git commit -m "test: pruebas unitarias, de contrato y de regresión (73 casos)"
```

Debe mostrar **73 passed**.

### Paso 9 — Integración continua y evidencias

```bash
cp -r $KIT/09_natalia_ci_evidencias/. .
python -m pytest --cov=solicitudes --cov-branch
git add -A
git commit -m "ci: GitHub Actions, pre-commit y evidencias de verificación antes/después"
git tag etapa-pruebas
git push origin main --tags
```

### Paso 10 — Correcciones del code review (OBS-04 y OBS-08)

```bash
cp -r $KIT/10_natalia_fix_code_review/. .
python -m pytest --cov=solicitudes --cov-branch
```

Debe mostrar **75 passed** y **TOTAL ... 99%**.

📸 **Captura 8** — 75 pruebas en verde y la tabla de cobertura.

```bash
git add -A
git commit -m "fix(review): escritura atómica de datos.json (OBS-04) y ClassVar en FiltroSolicitudes (OBS-08)"
```

### Paso 11 — Documentación final y cierre

```bash
cp -r $KIT/11_natalia_docs_finales/. .
python -m ruff check .
python -m ruff format --check .
radon cc -s -a solicitudes
```

Debe mostrar **All checks passed!**, **19 files already formatted** y **A (1.9)**.

📸 **Captura 9** — análisis estático final.

```bash
git add -A
git commit -m "docs: code review grupal, refactorings, pruebas y README final"
git tag estado-refactorizado
git tag estadorefactorizado
git push origin main --tags
git log --format="%h  %<(20)%an %s" --decorate=short
git tag
```

📸 **Captura 11** — historial con los commits de los tres integrantes y la lista de tags.

En GitHub: pestaña **Actions** → esperar el tilde verde del workflow **CI** (1-2 minutos).

📸 **Captura 10** — workflow CI en verde.

---

## Comparación antes/después (para el video, cualquiera puede hacerla)

```bash
git checkout etapa-diagnostico        # versión inicial + golden master
python -m pytest tests/test_caracterizacion.py
git checkout main                     # versión refactorizada
python -m pytest tests/test_caracterizacion.py
git diff --stat estado-inicial estado-refactorizado
```

La **misma** prueba pasa en las dos versiones: el comportamiento se conservó.

## Declaración de uso de IA (al final)

Completar `docs/uso-ia.md` y la sección 10 del informe. Luego, quien lo complete:

```bash
git pull
git add docs/uso-ia.md
git commit -m "docs: declaración de uso de inteligencia artificial"
git push
```

## Si algo falla

| Problema | Solución |
|---|---|
| `Permission denied` / `403` al hacer push | No se aceptó la invitación de colaborador, o GitHub pide iniciar sesión: aceptar y reintentar. |
| `rejected ... fetch first` | Alguien subió antes: `git pull` y volver a `git push origin main --tags`. |
| `python: command not found` | Probar `py` (Windows) o `python3` (Mac/Linux). |
| `radon: command not found` | Activar el entorno: `source .venv/Scripts/activate`. |
| `cp: cannot stat` | La variable `KIT` no apunta a la carpeta correcta: `ls ~/Desktop/kit_tpi_ms2/pasos`. |
| El golden master falla | No commitear. Repetir el `cp` del paso; si sigue fallando, avisar al grupo. |
