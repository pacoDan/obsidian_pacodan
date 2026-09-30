para este disco
```md
/run/media/daniel/auxext4 -------------------------------------------- 22:32:00
> df -h .
S.ficheros     Tamaño Usados  Disp Uso% Montado en
/dev/sdb1        458G    26G  409G   6% /run/media/daniel/auxext4

```

teniendo conda instalado (example miniconda)
```sh
cd /run/media/daniel/auxext4
mkdir -p .minicondaSELF/{envs,pkgs,pip-cache,projects}

conda config --add envs_dirs /run/media/daniel/auxext4/.minicondaSELF/envs
conda config --add pkgs_dirs /run/media/daniel/auxext4/.minicondaSELF/pkgs

pip config set global.cache-dir /run/media/daniel/auxext4/.minicondaSELF/pip-cache
```
al comprobar los resultados:
```md
> conda config --show envs_dirs
conda config --show pkgs_dirs

envs_dirs:
  - /run/media/daniel/auxext4/.minicondaSELF/envs
  - /home/daniel/miniconda3/envs
  - /home/daniel/.conda/envs
pkgs_dirs:
  - /run/media/daniel/auxext4/.minicondaSELF/pkgs
```

para que env_dirs use solo el disco externo:
```sh
conda config --remove-key envs_dirs
conda config --add envs_dirs /run/media/daniel/auxext4/.minicondaSELF/envs
pip cache dir
```