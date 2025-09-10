<!-- markdownlint-disable-next-line MD041 -->
[![build-pdfs](https://github.com/bbddetsisi/material-docente/actions/workflows/build-pdfs.yml/badge.svg?branch=main)](https://github.com/bbddetsisi/material-docente/actions/workflows/build-pdfs.yml)

# Bases de datos

Repositorio con el material para la asignatura _**Bases de Datos**_ que se imparte en la _Escuela Técnica Superior de Ingeniería de Sistemas Informáticos_ de la Universidad Politécnica de Madrid.

El repositorio se organiza de la siguiente forma:

- El directorio `transparencias` contiene las transparencias del temario de la asignatura en formato Markdown.
- El directorio `ejercicios` contiene los ejercicios en formato LaTeX.
- El directorio `examenes` contiene exámenes de otros años para las asignaturas de Bases de Datos de los diferentes grados de la escuela.

## Compilación local

Para generar el material sin la acción de GitHub es necesario tener instaladas las siguientes dependencias:

- `marp-cli` para procesar las transparencias.
- `latexmk` para compilar los ejercicios en LaTeX.
- Herramientas básicas de shell (`cp`, `find`, `bash`).

### Transparencias

```bash
mkdir -p build/transparencias
cp -R transparencias/img build/transparencias/img
marp -I transparencias/ -o build/transparencias/ --html --allow-local-files
marp -I transparencias/ -o build/transparencias/ --pdf --allow-local-files --theme-set transparencias/styles/bbdd.css
```

### Ejercicios

```bash
cd ejercicios
latexmk -pdf *.tex
cd ..
mkdir -p build/ejercicios
cp ejercicios/*.pdf build/ejercicios/
```

### Índice HTML

```bash
cat <<'EOF_HTML' > build/index.html
<!DOCTYPE html>
<html lang="es">
<head><meta charset="UTF-8"><title>Apuntes BBDD</title></head>
<body><h1>📚 Apuntes de Bases de Datos</h1><ul>
EOF_HTML

find build -type f \( -name "*.pdf" -o -name "*.html" \) -not -name index.html | sort | while read file; do
  name=$(basename "$file")
  path="${file#build/}"
  echo "<li><a href=\"$path\">$name</a></li>" >> build/index.html
done

cat <<'EOF_HTML' >> build/index.html
</ul></body></html>
EOF_HTML
```

Abra `build/index.html` en su navegador para consultar el resultado.
CUIDADO! QUE ESTE ES EL FORK!
