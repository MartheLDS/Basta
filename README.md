# Basta - Cartes interactives & Analyse de représentativité

Outils d'analyse et de cartographie interactive pour le **Baromètre Basta**, une enquête participative menée principalement à Marseille (avec une extension sur Nice). Ce projet produit des cartes Folium et des analyses statistiques de représentativité à partir des réponses au baromètre et des données démographiques INSEE.

## Contexte

Le **Baromètre Basta** est une enquête participative citoyenne permettant de recueillir les avis et expériences des habitants sur leur cadre de vie à Marseille. Ce dépôt regroupe :

1. **La cartographie** (`carto/`) : visualisation géographique des points de collecte (PAV — Points d'Apport Volontaire), des densités de réponses par quartier, et des distances moyennes domicile–PAV.
2. **L'analyse de représentativité** (`analysis/`) : comparaison statistique des répondants au baromètre avec la population réelle de Marseille (données de recensement INSEE 2022), afin de vérifier si l'échantillon est représentatif et de calculer des poids de pondération.

## Structure du projet

```
Basta/
├── carto/                    # 🗺️ Cartographie interactive
│   ├── scripts/              # Notebooks Jupyter de génération des cartes
│   │   ├── densite_pav_pop_quartiers.ipynb    # Densité PAV / population par quartier
│   │   ├── densite_pav_pop_Nice.ipynb         # Densité PAV / population – Nice
│   │   ├── carte_volume_pop_arr.ipynb         # Volume de population par arrondissement
│   │   ├── carto_bacs_quartiers.ipynb         # Couverture des bacs par quartier
│   │   ├── distance_moyenne_habitation_pav.ipynb  # Distance moyenne habitation–PAV
│   │   ├── distance_moyenne_nice.ipynb        # Distance moyenne – Nice
│   │   ├── surface_couverte.ipynb             # Surface couverte par les PAV
│   │   └── explore_pav.ipynb                  # Exploration des données PAV
│   ├── cartes/               # Cartes HTML générées (sorties Folium)
│   ├── data/
│   │   ├── raw/              # Données brutes (GeoJSON quartiers, CSV population, CSV PAV)
│   │   └── cache/            # Distances précalculées (shapefiles)
│   └── main.py               # Point d'entrée Python (placeholder)
│
├── analysis/                 # 📊 Analyse de représentativité
│   ├── notebooks/
│   │   └── clean.ipynb       # Notebook principal d'analyse statistique
│   └── data/
│       ├── raw/              # Données brutes (CSV INSEE par arrondissement)
│       └── processed/        # Données nettoyées et combinées
│
└── docs/                     # 🌐 Publication GitHub Pages
    ├── index.html            # Page d'accueil avec liens vers les cartes
    └── *.html                # Cartes interactives publiées
```

**Séparation claire :**
- `carto/` → tout ce qui concerne les cartes interactives Folium
- `analysis/` → tout ce qui concerne l'analyse statistique du baromètre

## Technologies utilisées

| Outil | Rôle |
|---|---|
| **Python 3.13** | Langage principal |
| **Folium** | Génération de cartes interactives (basé sur Leaflet.js) |
| **GeoPandas** | Traitement des données géospatiales (GeoJSON, shapefiles) |
| **Pandas** | Manipulation des données tabulaires |
| **Matplotlib** | Visualisation statique |
| **Contextily** | Fonds de carte (tiles OpenStreetMap) |
| **Mapclassify** | Classification choroplèthe (quantiles, seuils naturels…) |
| **Jupyter** | Environnement de notebooks interactifs |
| **UV** | Gestionnaire de paquets Python moderne (alternative rapide à pip/Poetry) |

## Installation

```bash
# Prérequis : Python 3.13 et UV installés
# Installer UV : https://docs.astral.sh/uv/

# Installer les dépendances
uv sync
```

## Utilisation

### Générer les cartes

Ouvrir un notebook depuis `carto/scripts/` dans Jupyter :

```bash
cd carto/scripts
jupyter notebook densite_pav_pop_quartiers.ipynb
```

Les cartes HTML générées sont sauvegardées dans `carto/cartes/`.

### Lancer l'analyse de représentativité

```bash
cd analysis/notebooks
jupyter notebook clean.ipynb
```

**Analyses réalisées :**
- Comparaison répondants vs population par arrondissement et secteur
- Tests du Chi² d'ajustement
- Calcul des intervalles de représentativité
- Calcul des poids de pondération

Les données sources sont dans `analysis/data/raw/`.

### Tester les cartes en local

```bash
python3 -m http.server 8000
# Ouvrir http://localhost:8000 dans le navigateur
```

## Déploiement GitHub Pages

Les cartes sont publiées automatiquement sur : **https://marthelds.github.io/Basta/**

### Configuration initiale

1. Aller dans Settings > Pages du repo
2. Source : Deploy from a branch
3. Branch : `main`
4. Folder : `/docs`
5. Save

### Mettre à jour les cartes publiées

Après avoir regénéré les cartes dans `carto/cartes/` :

```bash
# Copier les nouvelles cartes dans docs/
cp carto/cartes/*.html docs/

# Commit et push
git add docs/
git commit -m "Update maps"
git push
```

GitHub Pages se met à jour automatiquement (1–2 minutes).

## Sources de données

- **PAV Marseille** : fichier CSV des points de collecte et réponses au baromètre (`carto/data/raw/aix_marseille_pav.csv`)
- **Quartiers Marseille** : GeoJSON des limites des quartiers (`carto/data/raw/marseille_quartiers.geojson`)
- **Population INSEE 2022** : données de recensement par arrondissement (`carto/data/raw/pop_marseille_2022.csv`, `analysis/data/raw/`)
- **Population nationale** : fichier Parquet de la population française (`carto/data/raw/population_france.parquet`)

## Notes techniques

- Les cartes utilisent Leaflet via CDN → pas besoin de servir des assets locaux
- Les GeoJSON sont embarqués directement dans les fichiers HTML générés
- Les distances domicile–PAV sont précalculées et mises en cache dans `carto/data/cache/`
