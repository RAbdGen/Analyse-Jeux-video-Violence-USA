# Jeux vidéo et violence scolaire aux États-Unis

> La violence des jeunes américains est-elle corrélée à une industrie du jeu vidéo orientée vers la violence ?

Projet Big Data (2026) de **Renaud ABDELLOU--GENESTIER** et **Baptiste LAFARGE**.

## Le sujet

Après chaque fusillade dans une école américaine, les jeux vidéo violents reviennent dans le débat public. Ce projet confronte cette idée aux chiffres. Il croise deux jeux de données, de 1999 à 2024 :

- **les tueries en milieu scolaire aux États-Unis** : date, année, établissement, district, etc. (`school-shootings-data.csv`) ;
- **l'industrie du jeu vidéo** : titre, genre, éditeur, plateforme, date de sortie, ventes, notes (base VGChartz 2024, `vgchartz-2024.csv`).

On mesure d'un côté « l'offre de violence » (poids des genres Action et Shooter, records de ventes, éditeurs dominants), de l'autre l'évolution réelle du nombre d'incidents, puis on compare les deux année par année.

## Contenu du dépôt

| Fichier | Rôle |
|---|---|
| `projet.ipynb` | Notebook principal : nettoyage, analyse, graphiques et conclusions |
| `index.qmd` | Présentation (Quarto / reveal.js) qui résume le projet |
| `school-shootings-data.csv` | Données brutes sur les tueries scolaires |
| `vgchartz-2024.csv` | Données brutes sur les jeux vidéo |
| `data.db` | Base SQLite générée par le notebook (tables `school_shootings` et `video_games`) |

## Méthode

1. **Nettoyage** : suppression des colonnes inutiles (identifiants NCES, codes FIPS, `lunch`, `ulocale` pour les tueries ; `img`, `jp_sales`, `pal_sales`, `other_sales`, `last_update` pour les jeux).
2. **Export SQLite** : les deux tables sont écrites dans `data.db` pour être interrogées en SQL.
3. **Analyse exploratoire** : requêtes SQL + graphiques matplotlib / seaborn.
4. **Croisement** des deux séries par année.

## Conclusions du notebook

1. **Le genre Action domine sur 25 ans.** C'est le genre le plus sorti sur la période. À partir de 2019, le role-playing et l'action-aventure passent parfois devant.
2. **Les gros succès commerciaux sont souvent violents.** Le jeu le plus vendu de l'année est très souvent un titre de la série GTA ou Call of Duty. Mais ce n'est pas systématique : en 2005, le meilleur vendeur est un jeu de cuisine sur Nintendo DS.
3. **Le Shooter n'est jamais le genre n°1 d'une année, mais il est souvent dans le top 3** (2005, 2010, 2011, 2012, 2013 notamment). Le sport domine de 1999 à 2003, l'action de 2005 à 2018.
4. **Les éditeurs leaders produisent surtout de l'Action et du Shooter.** Activision et Ubisoft figurent en tête, ce qui fait de ces genres un standard de l'industrie.
5. **Les tueries explosent tard.** Le nombre d'incidents augmente fortement à partir de 2018, alors que l'offre de jeux violents est massive depuis le début des années 2000.
6. **Pas de lien visible entre ventes et tueries.** En 2015, un Call of Duty se vend à plusieurs millions d'exemplaires pour très peu de tueries, alors qu'en 2005 il y a davantage d'incidents sans jeu violent en tête des ventes.

**Conclusion générale** : l'analyse ne montre **aucune corrélation directe et immédiate** entre la consommation de jeux vidéo violents et les tueries en milieu scolaire. Le décalage temporel et l'absence de co-variation suggèrent que le phénomène relève plutôt de facteurs sociétaux, législatifs (accès aux armes) et psychologiques.

### Limites

- **Qualité des données VGChartz après 2018** : les ventes et les notes sont très souvent manquantes pour les sorties récentes, ce qui fragilise la comparaison sur la dernière période.
- **Volume limité** du jeu de données sur les tueries.
- **Définition de « jeu violent »** réduite aux genres (Action, Shooter, Action-Aventure), sans analyse du contenu réel des jeux.
- **Pas de test statistique dans le notebook** : la conclusion repose sur la lecture des graphiques. La présentation `index.qmd` évoque une corrélation et une régression linéaire, mais elles ne sont pas calculées dans `projet.ipynb`.
- Corrélation et causalité sont deux choses distinctes : ce travail exploratoire ne prouve rien au niveau individuel.

## Installation rapide pour lire le notebook

Prérequis : Python 3.10 ou plus récent.

```bash
git clone <url-du-depot>
cd Analyse-Jeux-video-Violence-USA

python -m venv .venv
source .venv/bin/activate        # Windows : .venv\Scripts\activate

pip install jupyterlab pandas matplotlib seaborn numpy
jupyter lab projet.ipynb
```

Les graphiques sont déjà enregistrés dans le notebook : on peut le lire sans rien exécuter, y compris directement sur GitHub.

### Réexécuter le notebook

Les deux CSV et `data.db` sont dans le dépôt, et les chemins sont relatifs : lancez Jupyter **depuis la racine du projet**. La cellule de création de la base (`data.db`) régénère les tables depuis les CSV. La cellule d'aperçu des jeux vidéo utilise un chemin absolu propre à une machine (`/home/r/Work/Licence/...`) et affiche une erreur ailleurs : elle n'empêche pas le reste de fonctionner, puisque la cellule suivante relit les CSV avec des chemins relatifs.

### Générer la présentation (optionnel)

Installez [Quarto](https://quarto.org/docs/get-started/), puis :

```bash
pip install jupyter
quarto render index.qmd
```

Cela produit `index.html`, une présentation reveal.js autonome.

## Sources des données

- Base VGChartz 2024 (ventes de jeux vidéo)
- Jeu de données sur les fusillades en milieu scolaire aux États-Unis (1999 et après)
