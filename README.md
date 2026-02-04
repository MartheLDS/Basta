# Basta - Cartes interactives

Cartes interactives Folium pour visualiser les données du baromètre Basta à Marseille.

## Structure du projet

```
Basta/
├── cartes/           # Cartes sources (générées par les scripts)
├── docs/             # Cartes pour GitHub Pages
├── analysis/         # Analyse de représentativité
│   ├── notebooks/    # Notebooks Jupyter
│   ├── data/
│   │   ├── raw/      # Données brutes (CSVs INSEE, réponses)
│   │   └── processed/# Données combinées/traitées
│   └── scripts/      # Scripts Python
└── README.md
```

## Déploiement GitHub Pages

Les cartes HTML sont accessibles via GitHub Pages à partir du dossier `docs/`.

### Configuration

1. Aller dans Settings > Pages du repo
2. Source : Deploy from a branch
3. Branch : `main` (ou `deploy/docs-pages`)
4. Folder : `/docs`
5. Save

Les cartes seront dispo sur : `https://martheleds.github.io/Basta/`

### Mettre à jour les cartes

Après avoir regénéré les cartes dans `cartes/` :

```bash
# Copier les nouvelles cartes dans docs/
cp cartes/*.html docs/

# Commit et push
git add docs/
git commit -m "Update maps"
git push
```

GitHub Pages se met à jour automatiquement (ça prend 1-2 minutes).

## Analyse de représentativité

Le notebook `analysis/notebooks/clean.ipynb` contient l'analyse de la représentativité du baromètre par rapport à la population marseillaise (données INSEE).

**Analyses réalisées :**
- Comparaison répondants vs population par arrondissement et secteur
- Tests du Chi² d'ajustement
- Calcul des intervalles de représentativité
- Calcul des poids de pondération

### Lancer l'analyse

```bash
cd analysis/notebooks
jupyter notebook clean.ipynb
```

Les données sources sont dans `analysis/data/raw/`.

## Notes

- Les cartes utilisent Leaflet via CDN, donc pas besoin de servir des assets locaux
- Les GeoJSON sont embarqués directement dans les HTML
- Pour tester en local : `python3 -m http.server 8000` depuis le dossier `docs/`
