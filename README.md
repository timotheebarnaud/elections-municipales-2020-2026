# Municipales 2020 vs 2026 : comment les blocs politiques ont bougé, commune par commune

*Comparaison des résultats du premier tour des élections municipales de 2020 et 2026 par bloc politique, dans les 13 régions de France métropolitaine*

## Visualisations

Chaque story Flourish contient deux cartes (2020 et 2026) avec des tooltips détaillés : bloc dominant, scores par bloc et par nuance, et évolution entre les deux scrutins.

- [Auvergne-Rhône-Alpes](https://public.flourish.studio/story/3618906/)
- [Bourgogne-Franche-Comté](https://public.flourish.studio/story/3618909/)
- [Bretagne](https://public.flourish.studio/story/3618912/)
- [Centre-Val de Loire](https://public.flourish.studio/story/3620099/)
- [Corse](https://public.flourish.studio/story/3633873/)
- [Grand Est](https://public.flourish.studio/story/3620101/)
- [Hauts-de-France](https://public.flourish.studio/story/3618886/)
- [Île-de-France](https://public.flourish.studio/story/3620107/)
- [Normandie](https://public.flourish.studio/story/3620115/)
- [Nouvelle-Aquitaine](https://public.flourish.studio/story/3620128/)
- [Occitanie](https://public.flourish.studio/story/3620137/)
- [Pays de la Loire](https://public.flourish.studio/story/3620146/)
- [Provence-Alpes-Côte d'Azur](https://public.flourish.studio/story/3620143/)

## Méthodologie

### Sources

Les résultats du premier tour proviennent des **fichiers CSV du Ministère de l'Intérieur** (format wide, un bloc de colonnes par liste candidate). Les données de population et de rattachement régional proviennent de l'**INSEE**. Les deux sources sont croisées via le code INSEE de la commune, reconstruit à partir du code département (`zfill(2)`) et du code commune (`zfill(3)`).

### Passage du format wide au format long

Les fichiers du Ministère contiennent jusqu'à 16 listes candidates par commune (2020) et 13 (2026), chacune sur un bloc de colonnes contigu. Le passage en format long est réalisé par une boucle `iterrows` qui extrait la nuance et le pourcentage de voix de chaque liste candidate par position de colonnes, calculée à partir de la structure régulière du fichier.

### Classification en blocs politiques

Chaque nuance politique (attribuée par le Conseil d'État) est regroupée en **6 blocs** : Extrême gauche, Gauche, Centre, Droite, Extrême droite, Divers ou non classé. Ce mapping est un **choix éditorial** propre à cette analyse. Pour 2026, **La France insoumise a été placée dans le bloc Gauche** (et non Extrême gauche comme classée par le Conseil d'État) à des fins de comparaison avec 2020, où elle figurait déjà dans ce bloc. Les scores sont ensuite agrégés par bloc via `groupby` + `sum`, puis pivotés en format wide (`pivot_table`). Le bloc dominant de chaque commune est déterminé par `idxmax`.

### Tooltips Flourish

Les tooltips des cartes 2026 affichent pour chaque commune : le bloc dominant, le score de chaque bloc avec l'évolution par rapport à 2020 (en vert si hausse, en rouge si baisse), et le détail par nuance politique. Ce rendu a nécessité la construction de **colonnes dédiées** dans le DataFrame final : évolution absolue, signe (+/-), et code couleur hexadécimal pour chaque bloc, intégrés dans les templates Handlebars de Flourish.

**Limites** :

L'analyse porte sur le **premier tour uniquement**. La carte montre le bloc dominant (celui qui cumule le plus de voix via ses différentes listes), et non la liste arrivée en tête. Le scrutin municipal fait qu'une large part des communes (notamment les plus petites) ne dispose pas de nuances politiques attribuées par le Conseil d'État, et n'est donc pas concernée par cette analyse. Le regroupement en blocs simplifie des réalités locales complexes (alliances, listes transpartisanes).

## Organisation du dépôt

```
├── data/
│   ├── raw/            # CSV Ministère (2020, 2026) + population INSEE
│   └── clean/          # Résultats par commune, par région, par année
├── notebooks/
│   └── Comparaison_municipales_2020-2026.ipynb
└── README.md
```

## Outils

Python (pandas, numpy) · Données Ministère de l'Intérieur · INSEE · Jupyter Notebook · Flourish

---

Projet réalisé par **Timothée Barnaud**.
