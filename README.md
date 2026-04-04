# AlphaFold Drug Discovery — Analyse de la prédiction de structure protéique

Notebook pédagogique qui explique pas à pas comment AlphaFold prédit le repliement d'une protéine, et ce que cela implique concrètement pour la conception de médicaments. Deux protéines sont analysées : **p53** (suppresseur de tumeur) et le **lysozyme**.

---

## Contenu du notebook (`AlphaFold_Analysis.ipynb`)

| Section | Description |
|---|---|
| 1 | Le problème : de la séquence à la structure 3D (paradoxe de Levinthal) |
| 2 | Encodage de la séquence avec **ESM2** (embeddings contextuels, analogie NLP) |
| 3 | Visualisation des cartes d'attention et de contacts inter-résidus |
| 3b | Architecture Evoformer : couplage des représentations par résidu et par paire |
| 4 | Structure 3D et score de confiance **pLDDT** récupérés depuis l'API AlphaFold DB |
| 5 | Implications pour le drug discovery (poches de liaison, régions désordonnées) |

---

## Prérequis système

- **Python 3.10** (recommandé, testé avec 3.10)
- **CUDA** optionnel mais recommandé — ESM2 (650M paramètres) est lent sur CPU

---

## Installation de l'environnement

### 1. Créer et activer un environnement virtuel

```bash
python3.10 -m venv .venv
source .venv/bin/activate  # Linux / macOS
# .venv\Scripts\activate   # Windows
```

### 2. Installer les dépendances

Installer PyTorch selon votre configuration :

```bash
pip install torch --index-url https://download.pytorch.org/whl/cu118  # GPU (CUDA 11.8)
# ou
pip install torch  # CPU uniquement
```

Puis installer le reste des dépendances via `requirements.txt` :

```bash
pip install -r requirements.txt
```

> Les versions exactes sont épinglées dans `requirements.txt`.

### 3. Enregistrer le kernel Jupyter

```bash
python -m ipykernel install --user --name alphafold --display-name "AlphaFold (Python 3.10)"
```

### 4. Lancer le notebook

```bash
jupyter notebook AlphaFold_Analysis.ipynb
# ou avec VS Code : ouvrir le fichier et sélectionner le kernel "AlphaFold (Python 3.10)"
```

---

## Données externes (récupérées automatiquement)

Le notebook appelle deux APIs au démarrage, une connexion internet est donc requise :

- **UniProt REST API** — récupère les séquences FASTA de p53 (`P04637`) et du lysozyme (`P61626`)
- **AlphaFold DB API** — récupère les structures PDB prédites avec les scores pLDDT

---

## Modèle ESM2

Le modèle `esm2_t33_650M_UR50D` (~2,5 Go) est téléchargé automatiquement par `fair-esm` lors du premier appel à `esm.pretrained.esm2_t33_650M_UR50D()`. Il est mis en cache dans `~/.cache/torch/hub/`.

> Sur CPU, le chargement et l'inférence peuvent prendre plusieurs minutes. Une carte GPU (>6 Go VRAM) est recommandée pour un temps de réponse raisonnable.

---

## Structure du dépôt

```
alphafold_drug_discovery/
├── AlphaFold_Analysis.ipynb   # Notebook sur l'analyse  de Alpha Fold 
├── README.md
└── fig/                       # Figures annexes
```
