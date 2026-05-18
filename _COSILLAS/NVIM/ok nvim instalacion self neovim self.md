git clone https://github.com/LazyVim/starter ~/.config/nvim

SELF PRIMER instalación self nvim:
```sh
# rm -rf ~/.local/share/nvim ~/.local/state/nvim ~/.cache/nvim
git clone https://github.com/pacodan/nvim ~/.config/nvim
nvim --headless "+Lazy! sync" +qa # Lazy.nvim 
# o nvim --headless "+PlugInstall" +qa # vim-plug
```


backup:
```sh
# required
mv ~/.config/nvim{,.bak}

# optional but recommended
mv ~/.local/share/nvim{,.bak}
mv ~/.local/state/nvim{,.bak}
mv ~/.cache/nvim{,.bak}
```
OK RESTAURACIÓN:
```sh
# Restaurar configuración (obligatoria)
mv ~/.config/nvim{.bak,}

# Restaurar opcionales (si los respaldaste)
mv ~/.local/share/nvim{.bak,}
mv ~/.local/state/nvim{.bak,}
mv ~/.cache/nvim{.bak,}
```
SELF re instalación self nvim:
```sh
mv ~/.config/nvim{,.bak}
mv ~/.local/share/nvim{,.bak}
mv ~/.local/state/nvim{,.bak}
mv ~/.cache/nvim{,.bak}
# 2. Deja las otras VACÍAS (se regeneran)
# rm -rf ~/.local/share/nvim ~/.local/state/nvim ~/.cache/nvim
git clone https://github.com/pacodan/nvim ~/.config/nvim
nvim --headless "+Lazy! sync" +qa # Lazy.nvim 
# o nvim --headless "+PlugInstall" +qa # vim-plug
```
borrado de cache:
```sh
# 2. Deja las otras VACÍAS (se regeneran)
rm -rf ~/.local/share/nvim ~/.local/state/nvim ~/.cache/nvim
```


ok reiniciar:
```sh
#Deja las otras VACÍAS (se regeneran) 
rm -rf ~/.local/share/nvim ~/.local/state/nvim ~/.cache/nvim
# 2. Regenera todo automáticamente 
nvim --headless "+Lazy! sync" +qa # Lazy.nvim 
# o nvim --headless "+PlugInstall" +qa # vim-plug
```



GIT volver y pushear a un commit anterior:
```sh
git reset --hard 88bbdb2d0e25286df675e9733488c1d585de9bf6 # hash del commit
git push --force-with-lease origin main
```