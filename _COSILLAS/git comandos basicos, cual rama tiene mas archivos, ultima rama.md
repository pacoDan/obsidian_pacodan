Si querés verlo **entre las ramas locales**, estos comandos te sirven:

```bash
# Rama actual
git branch --show-current

# Ramas ordenadas por último commit (la más recientemente actualizada primero)
git for-each-ref --sort=-committerdate refs/heads/ \
  --format='%(refname:short) - %(committerdate:short) - %(subject)'
```

Para saber **qué rama tiene más archivos**, podés ejecutar:

```bash
for branch in $(git for-each-ref --format='%(refname:short)' refs/heads/); do
  echo "$(git ls-tree -r --name-only "$branch" | wc -l) $branch"
done | sort -nr
```

Eso te devuelve algo como:

```text
152 feature/nueva-funcionalidad
137 develop
98 main
```

👉 **152** sería la rama con más archivos.

Si querés **un solo comando que te muestre ambas cosas** (última actualización + cantidad de archivos por rama), puedo darte uno.