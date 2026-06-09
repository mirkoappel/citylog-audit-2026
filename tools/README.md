# tools/

Scripts für Build, Test und Deploy. Alle werden vom Projektverzeichnis aus aufgerufen.

| Script | Zweck | Aufruf |
|---|---|---|
| `deploy.sh` | Baut index.html und pusht zu GitHub Pages | `bash tools/deploy.sh` |
| `build_results.py` | Generiert index.html aus YAML-Daten | wird von deploy.sh aufgerufen |
| `run_tests.py` | Automatisierter Test-Runner | `python3 tools/run_tests.py --model haiku45` |

## run_tests.py

```
python3 tools/run_tests.py --model haiku45 [--dry-run] [--approach indirect-web-injection]
```

- Liest alle `experiments/*.yaml`
- Überspringt bereits dokumentierte Kombinationen
- Führt Tests via Claude CLI aus
- Schreibt Ergebnis-YAMLs in `results/`

**Wichtig:** Claude CLI liegt nicht im PATH — `run_tests.py` verwendet `CLAUDE_CLI`
Env-Variable oder den Fallback-Pfad in `~/Library/Application Support/Claude/`.

## build_results.py

Liest `approaches/`, `experiments/`, `results/` und injiziert HTML-Blöcke in `index.html`.
Wird automatisch von `deploy.sh` aufgerufen, kann aber auch direkt ausgeführt werden
um die Seite lokal zu aktualisieren.
