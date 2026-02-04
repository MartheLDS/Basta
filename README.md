# Basta - Cartes interactives

Cartes interactives Folium pour visualiser les données du baromètre Basta à Marseille.

## Structure du projet

```
Basta/
├── carto/               # 🗺️ Cartographie
│   ├── cartes/          # Cartes générées
│   ├── docs/            # Cartes publiées sur GitHub Pages
│   ├── scripts/         # Scripts de génération des cartes
│   ├── data/            # Données géographiques (GeoJSON, cache)
│   └── main.py          # Script principal
│
└── analysis/            # 📊 Analyse de représentativité
    ├── notebooks/       # Notebooks Jupyter d'analyse
    ├── data/
    │   ├── raw/         # Données brutes (CSVs INSEE, réponses questionnaire)
    │   └── processed/   # Données combinées/traitées
    └── scripts/         # Scripts Python d'analyse
```

**Séparation claire :** 
- `carto/` → tout ce qui concerne les cartes interactives Folium
- `analysis/` → tout ce qui concerne l'analyse statistique du baromètre

## Déploiement GitHub Pages

Les cartes HTML sont accessibles via GitHub Pages à partir du dossier `carto/docs/`.

### Configuration

1. Aller dans Settings > Pages du repo
2. Source : Deploy from a branch
3. Branch : `main` (ou `deploy/docs-pages`)
4. Folder : `/carto/docs`
5. Save

Les cartes seront dispo sur : `https://martheleds.github.io/Basta/`

### Mettre à jour les cartes

Après avoir regénéré les cartes dans `carto/cartes/` :

```bash
# Copier les nouvelles cartes dans docs/
cp carto/cartes/*.html carto/docs/

# Commit et push
git add carto/docs/
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
- Pour tester en local : `python3 -m http.server 8000` depuis le dossier `carto/docs/`
