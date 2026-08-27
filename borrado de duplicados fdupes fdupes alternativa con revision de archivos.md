`rmlint -T "df" -S dl .`
```md
╰─❯ rmlint -T "df" -S dl .
# Duplicate(s):
    ls '/mnt/c/Users/Daniel/Dropbox/COMU supuestos examenes de ALSINA/DEL DRIVE/final alsina 6 9 2022.pdf'
    rm '/mnt/c/Users/Daniel/Dropbox/COMU supuestos examenes de ALSINA/DEL DRIVE/final alsina 6 9 2022 (1).pdf'
    rm '/mnt/c/Users/Daniel/Dropbox/COMU supuestos examenes de ALSINA/DEL DRIVE/Alsina/2022-09-06.pdf'
    ls '/mnt/c/Users/Daniel/Dropbox/COMU supuestos examenes de ALSINA/Final 01-08-2018 Alsina/Ej 3 Practica.jpg'
    rm '/mnt/c/Users/Daniel/Dropbox/COMU supuestos examenes de ALSINA/DEL DRIVE/Final 01-08-2018 Alsina/Ej 3 Practica.jpg'
    ls '/mnt/c/Users/Daniel/Dropbox/COMU supuestos examenes de ALSINA/Final 01-08-2018 Alsina/Examen Final.jpg'
    rm '/mnt/c/Users/Daniel/Dropbox/COMU supuestos examenes de ALSINA/DEL DRIVE/Final 01-08-2018 Alsina/Examen Final.jpg'
a 1C 2021/Cursada 1C 2021/1er parcial/Presentacion Com Clase 6 2021.pdf'
    ls '/mnt/c/Users/Daniel/Dropbox/COMU supuestos examenes de ALSINA/Cursada 1C 2021_2/Cursada 1C 2021/Cursada 1C 2021/2do parcial/Presentación Com Clase 7 2021.pdf'
    rm '/mnt/c/Users/Daniel/Dropbox/COMU supuestos examenes de ALSINA/Cursada 1C 2021_2/Cursada 1C 2021/Cursada 1C 2021/1er parcial/Presentación Com Clase 7 2021.pdf'

==> Note: Please use the saved script below for removal, not the above output.
==> In total 174 files, whereof 10 are duplicates in 9 groups.
==> This equals 5.60 MB of duplicates which could be removed.
==> Scanning took in total 0.136s.

Wrote a json file to: /mnt/c/Users/Daniel/Dropbox/COMU supuestos examenes de ALSINA/rmlint.json
Wrote a sh file to: /mnt/c/Users/Daniel/Dropbox/COMU supuestos examenes de ALSINA/rmlint.sh
```