# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Regole

- Rispondi sempre in italiano.
- Lavora sempre su un ramo nuovo, mai su `main`. Le modifiche arrivano su `main` solo tramite PR su GitHub (`federicodb1978/Classification`).
- Per modifiche ampie mostra prima il piano e aspetta conferma.
- Non creare file che non sono stati chiesti esplicitamente.
- Non salvare output nei notebook: committa solo il `source` delle celle.
- Non leggere e non modificare il file `.env`.

## Il progetto

Progetto di classificazione sul dataset Iris con scikit-learn. Il lavoro è tutto in `Classification_Decision_Tree_example_IRIS_Dataset.ipynb`:

- EDA sul dataset `iris` di seaborn (150 campioni, 4 feature, 3 specie): scatter plot, matrice di correlazione, distribuzioni per specie.
- Split train/test 75/25 (`random_state=2`) e due `DecisionTreeClassifier` (Gini, `random_state=12`) che differiscono per `min_samples_leaf` (10 e 20).
- Valutazione con accuracy, classification report e matrice di confusione, più il commento ai risultati in celle markdown.
- Grafici dei due alberi salvati in `my_tree.png` e `my_tree_2.png` (file generati, ignorati da git).

Non ci sono build, linter o test: il notebook è l'unico codice.

## Ambiente e comandi

```bash
python3 -m venv .venv && source .venv/bin/activate   # il venv locale usa Python 3.14
pip install -r requirements.txt                      # output di pip freeze con versioni fissate
jupyter lab

# Eseguire il notebook senza interfaccia, per verificare che giri dall'inizio alla fine.
# Senza --inplace l'output va in un file nuovo: non sovrascrivere il notebook con gli output.
jupyter nbconvert --to notebook --execute Classification_Decision_Tree_example_IRIS_Dataset.ipynb --output /tmp/iris_check.ipynb
```

Per aggiungere una dipendenza, aggiungila con versione fissata a `requirements.txt` invece di rigenerare il file.

## Notebook e git (nbstripout)

In questo clone è configurato un filtro git `nbstripout` (`.git/info/attributes` + `filter.nbstripout` in `.git/config`, che punta a `.venv/bin/python3.14`). Il filtro rimuove output e contatori di esecuzione dai `*.ipynb` quando li metti in staging. Di conseguenza:

- `git status` può mostrare un notebook come modificato anche se `git diff` è vuoto, perché sono cambiati solo gli output.
- Il filtro è solo locale, non versionato con `.gitattributes`: è comunque necessario non salvare output nei notebook.
- Se il `.venv` viene ricreato con un'altra versione di Python, il percorso del filtro in `.git/config` smette di funzionare. In quel caso esegui `nbstripout --install --attributes .git/info/attributes`.

Per modificare un notebook, cambia il `source` delle celle (ad esempio con NotebookEdit), mai gli output.
