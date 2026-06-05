a un solo documento:
```sh
#!/usr/bin/env bash
set -euo pipefail

OUTPUT="corpus_unificado.md"

echo "# Corpus Unificado" > "$OUTPUT"
echo "" >> "$OUTPUT"
echo "Generado: $(date)" >> "$OUTPUT"
echo "" >> "$OUTPUT"

find . -type f \( \
    -iname "*.pdf" -o \
    -iname "*.docx" \
\) -print0 |
sort -z |
while IFS= read -r -d '' file; do

    echo "Procesando: $file"

    {
        echo ""
        echo "---"
        echo ""
        echo "# Documento: $file"
        echo ""
        echo "**Ruta:** \`$file\`"
        echo ""

        markitdown "$file"

        echo ""
    } >> "$OUTPUT" 2>/dev/null || {
        echo "ERROR: $file" >&2
    }

done

echo "Finalizado: $OUTPUT"
```