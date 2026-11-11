# Respirer en Métropole : qualité de l'air dans le Grand Paris

Une géographie de la pollution atmosphérique dans la métropole du Grand Paris, de janvier 2018 à mars 2026 : cartes mensuelles du dioxyde d'azote (NO2) et des particules fines (PM2.5), et profil temporel de chaque commune. Projet de géomatique et de datavisualisation réalisé dans le cadre du Master Observation de la Terre et Géomatique (Université de Strasbourg).

**Site en ligne : [https://louishmboc.github.io/qualite-air-grand-paris/](https://louishmboc.github.io/qualite-air-grand-paris/index.html)**

**Carte interactive : [https://louishmboc.github.io/qualite-air-grand-paris/carte.html](https://louishmboc.github.io/qualite-air-grand-paris/carte.html)**

**Carte interactive de la qualité de l'air**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/4501ea2f-20f6-47f6-9a77-9b0bae51d2ba" />



## Objectif

La cartographie de l'air met en évidence de fortes inégalités environnementales entre les territoires. Le site vise à :

- expliquer la différence entre deux polluants aux logiques spatiales opposées : le NO2, pollution de proximité liée au trafic routier, et les PM2.5, pollution de fond aux sources plus variées (chauffage au bois, usure des freins et des pneus) ;
- situer les niveaux observés par rapport aux recommandations 2021 de l'Organisation mondiale de la santé (OMS) ;
- permettre d'explorer l'évolution mois par mois, commune par commune, dans le contexte de la mise en place des Zones à Faibles Émissions (ZFE) depuis 2021.

## Fonctionnalités de la carte interactive

- Bascule entre deux polluants : particules fines (PM2.5) et dioxyde d'azote (NO2)
- Curseur temporel mensuel, de janvier 2018 à mars 2026
- Cartes mensuelles de concentration superposées à un fond sombre
- Contours des communes cliquables
- Panneau latéral repliable affichant, pour la commune choisie, le graphique de l'évolution de ses moyennes mensuelles
- Légende dynamique avec rappel du seuil OMS (5 µg/m³ pour les PM2.5, 10 µg/m³ pour le NO2)

## Repères OMS (lignes directrices 2021, moyenne annuelle)

| Polluant | Recommandation |
| --- | --- |
| Particules très fines (PM2.5) | 5 µg/m³ |
| Dioxyde d'azote (NO2) | 10 µg/m³ |

## Données

| Élément | Contenu |
| --- | --- |
| `dioxyde_azote_png_MGP/` | 100 images mensuelles de concentration de NO2, de janvier 2018 à avril 2026 (`NO2_AAAA-MM.png`) |
| `particules_fines_png_MGP/` | 100 images mensuelles de concentration de PM2.5, de janvier 2018 à avril 2026 (`PM25_AAAA-MM.png`) |
| `communes_NO2.geojson` | 150 communes, avec la moyenne mensuelle de NO2 de chaque commune |
| `communes_PM25.geojson` | 150 communes, avec la moyenne mensuelle de PM2.5 de chaque commune |

Chaque entité des fichiers GeoJSON porte les identifiants de la commune (code, nom, département, EPCI) et une valeur par mois (`2018-01`, `2018-02`, etc.).

**Sources :** Airparif (qualité de l'air), IGN (limites communales), OMS (lignes directrices 2021).

La méthodologie s'est articulée autour d'une chaîne de traitement géomatique et web complète : les relevés ponctuels d'Airparif pour le NO2 et les PM2.5 ont d'abord été agrégés en moyennes mensuelles de janvier 2018 à mars 2026, puis spatialisés sous forme de rasters continus par interpolation déterministe IDW (Pondération Inverse à la Distance) sous Python. Ces rasters ont ensuite été reprojetés en WGS 84, masqués selon les contours stricts de la Métropole du Grand Paris pour générer des visuels PNG transparents aux dimensions exactes, tandis qu'une analyse de statistiques zonales a permis d'extraire la moyenne mensuelle de chaque polluant par commune afin de compiler un fichier vectoriel GeoJSON. Enfin, ces couches ont été intégrées dans une interface web interactive sous Leaflet.js adoptant une charte graphique sombre, combinant un curseur temporel pour animer l'évolution spatiale des panaches de pollution et un panneau latéral dynamique exploitant Chart.js pour restituer la courbe chronologique locale au clic sur chaque commune, avant un déploiement public automatisé sur GitHub Pages.

## Technologies

- HTML5, CSS3, JavaScript
- [Leaflet](https://leafletjs.com/) 1.9.4 : carte, superposition d'images (`L.imageOverlay`) et couche vectorielle (`L.geoJSON`)
- [Chart.js](https://www.chartjs.org/) : graphique temporel par commune
- Fond de carte ESRI
- Hébergement : GitHub Pages

## Structure du dépôt

```
.
├── index.html                  Présentation, polluants, enjeux
├── carte.html                  Carte interactive
├── style.css                   Feuille de style
├── communes_NO2.geojson        Moyennes mensuelles par commune (NO2)
├── communes_PM25.geojson       Moyennes mensuelles par commune (PM2.5)
├── dioxyde_azote_png_MGP/      Images mensuelles NO2
├── particules_fines_png_MGP/   Images mensuelles PM2.5
└── captures/                   Captures d'écran du README
```

## Lancer le site en local

La carte charge ses données avec `fetch`, ce qui exige un petit serveur local (ouvrir `carte.html` directement ne suffit pas) :

```
git clone https://github.com/louishmboc/qualite-air-grand-paris
cd qualite-air-grand-paris
python -m http.server 8000
```

Puis ouvrir `http://localhost:8000`.

## Limites

- Les cartes mensuelles sont des images PNG superposées à la carte sur une emprise rectangulaire : elles ne permettent pas de lire une valeur en un point. Les valeurs chiffrées proviennent des moyennes par commune.
- Une moyenne communale lisse les écarts internes à la commune, en particulier pour le NO2, qui varie fortement à proximité des axes routiers.
- Les niveaux affichés décrivent la pollution de l'air, pas l'exposition individuelle des habitants.

## Pistes d'amélioration

- Ajouter avril 2026 au curseur, dont les images et les données sont déjà dans le dépôt
- Afficher la valeur de chaque commune au survol et la comparer au seuil OMS
- Adapter la carte aux petits écrans (le panneau latéral fait 380 px)
- Ajouter une comparaison avant et après la mise en place des ZFE

## Auteur

Louis Bocquet, Master Observation de la Terre et Géomatique, Université de Strasbourg, 2026.
Le projet a été réalisé seul pour un enseignement en Dataviz.

### Données et attributions
Ce projet exploite plusieurs jeux de données tiers réutilisés selon leurs conditions d'ouverture respectives :
* **Données de pollution atmosphérique (NO₂ et PM2.5) :** © [Airparif](https://www.airparif.asso.fr/) — Relevés mensuels consolidés (2018-2026), réutilisés conformément aux conditions de mise à disposition des données ouvertes.
* **Contours administratifs :** Métropole du Grand Paris / Communes — [IGN (Admin Express)](https://geoservices.ign.fr/) via la Licence Ouverte 2.0 (Etalab).
* **Fonds cartographiques :** © OpenStreetMap contributors (ODbL) / Esri.
