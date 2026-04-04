# Machine Learning en Drug Discovery

Projet pédagogique explorant l'application du machine learning à la conception
de médicaments, basé sur Dara et al. (2022), *Artificial Intelligence Review*.

Le projet se compose de deux expérimentations pratiques :

- **AlphaFold** — prédiction de structure protéique et analyse de confiance
- **Graph Neural Networks** — prédiction de propriétés moléculaires

---

## Contenu du dépôt
```
ml_drug_discovery/
├── AlphaFold_Analysis.ipynb   # Expérimentation 1 : prédiction de structure
├── GNN_Analysis.ipynb         # Expérimentation 2 : propriétés moléculaires
├── README.md
└── fig/                       # Figures générées
```

---

## Expérimentation 1 — AlphaFold (`AlphaFold_Analysis.ipynb`)

Analyse pas à pas du pipeline AlphaFold sur deux protéines contrastées :
**p53** (suppresseur de tumeur, partiellement désordonnée) et le **lysozyme**
(protéine globulaire compacte).

| Section | Description |
|---|---|
| 1 | Le problème : de la séquence à la structure 3D (paradoxe de Levinthal) |
| 2 | Encodage de la séquence avec ESM2 (embeddings contextuels, analogie NLP) |
| 3 | Cartes d'attention et contacts inter-acides aminés |
| 3b | Architecture Evoformer : couplage des représentations par acide aminé et par paire |
| 4 | Structure 3D et score de confiance pLDDT via l'API AlphaFold DB |
| 5 | Implications pour le drug discovery : poches de liaison et régions désordonnées |

## Expérimentation 2 — Graph Neural Networks (`GNN_Analysis.ipynb`)

*À venir.*

---

## Prérequis système

- **Python 3.10** (recommandé, testé avec 3.10)
- **CUDA** optionnel mais recommandé — ESM2 (650M paramètres) est lent sur CPU

---

## Installation

### 1. Créer et activer un environnement virtuel
```bash
python3.10 -m venv .venv
source .venv/bin/activate  # Linux / macOS
# .venv\Scripts\activate   # Windows
```

### 2. Installer les dépendances
```bash
pip install torch --index-url https://download.pytorch.org/whl/cu118  # GPU CUDA 11.8
# ou
pip install torch  # CPU uniquement
```
```bash
pip install -r requirements.txt
```

### 3. Enregistrer le kernel Jupyter
```bash
python -m ipykernel install --user --name ml_drug --display-name "ML Drug Discovery"
```

### 4. Lancer un notebook
```bash
jupyter notebook AlphaFold_Analysis.ipynb
```

---

## Données externes

Le notebook AlphaFold appelle deux APIs au démarrage (connexion internet requise) :

- **UniProt REST API** — séquences FASTA de p53 (`P04637`) et du lysozyme (`P61626`)
- **AlphaFold DB API** — structures PDB prédites avec scores pLDDT

Le modèle ESM2 (`esm2_t33_650M_UR50D`, ~2,5 Go) est téléchargé automatiquement
par `fair-esm` lors du premier appel et mis en cache dans `~/.cache/torch/hub/`.