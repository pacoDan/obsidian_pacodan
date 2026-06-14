descargar subtitulos (por defecto en vtt):
```sh
 yt-dlp --write-auto-subs --sub-langs es --skip-download https://youtube.com/playlist\?list\=PLRs-mZsXKdGslGCNnopHMaX6qPkGesAUI\&si\=Z0-udVpf-zl4aALt  
```

ok_vtt_a_txt.py:
```py
#!/usr/bin/env python3

import re
from pathlib import Path

for vtt_file in Path(".").glob("*.vtt"):
    output = []
    previous = None

    with open(vtt_file, encoding="utf-8") as f:
        for line in f:
            line = line.strip()

            if (
                not line
                or line.startswith("WEBVTT")
                or line.startswith("Kind:")
                or line.startswith("Language:")
                or "-->" in line
            ):
                continue

            line = re.sub(r"<\d{2}:\d{2}:\d{2}\.\d{3}>", "", line)
            line = re.sub(r"<[^>]+>", "", line)
            line = re.sub(r"\s+", " ", line).strip()

            if not line:
                continue

            if line == previous:
                continue

            output.append(line)
            previous = line

    txt_file = vtt_file.with_suffix(".txt")

    with open(txt_file, "w", encoding="utf-8") as f:
        f.write("\n".join(output))

    print(f"✓ {txt_file}")
```