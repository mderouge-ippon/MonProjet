# MonProjet : consommation et production électriques françaises (eco2mix)

Projet Power BI au format PBIP (modèle sémantique en TMDL, rapport en PBIR) construit sur le jeu de données **eco2mix national, données consolidées et définitives** publié par RTE sur l'Open Data Réseaux Énergies (ODRÉ).

## Contenu

| Dossier | Rôle |
|---|---|
| `monPremierProjet.SemanticModel/` | Modèle sémantique : table source `eco2mix-national-cons-def`, table de dates `Calendrier`, table `Mesures` |
| `monPremierProjet.Report/` | Rapport : une page avec l'histogramme de la consommation annuelle |
| `data/` | Fichier CSV source, **non versionné** (86 Mo) |

Le modèle suit quelques conventions : les colonnes brutes sont masquées et seules les mesures sont exposées, chaque filière de production a une mesure de puissance moyenne (MW) et une mesure d'énergie (MWh), et chaque objet porte une description en français.

## Récupérer les données

Le CSV n'est pas dans le dépôt. Après un clone :

1. Télécharger l'export avec libellés de colonnes et séparateur point-virgule :

   ```
   https://odre.opendatasoft.com/api/explore/v2.1/catalog/datasets/eco2mix-national-cons-def/exports/csv?delimiter=%3B&use_labels=true&lang=fr
   ```

   Page du jeu de données : https://odre.opendatasoft.com/explore/dataset/eco2mix-national-cons-def/

2. L'enregistrer sous `data/eco2mix-national-cons-def.csv`.

3. Si le projet n'est pas dans `C:\PBI\MonProjet`, modifier le paramètre Power Query `CheminDonnees` (Transformer les données > Gérer les paramètres, ou directement dans `monPremierProjet.SemanticModel/definition/expressions.tmdl`) pour qu'il pointe vers le dossier `data`.

4. Ouvrir `monPremierProjet.pbip` dans Power BI Desktop et actualiser.

## Particularités de la source

- Relevés au pas de 15 minutes, mais seules les lignes à :00 et :30 sont renseignées. La conversion MW -> MWh utilise donc 0,5 h par relevé (mesure `Durée d'un relevé (h)`).
- Certaines colonnes numériques contiennent la valeur `ND` (non disponible), surtout en 2012. La requête la remplace par null avant le typage ; sans cela, l'actualisation échoue sur environ 17 600 lignes.
- Les données définitives couvrent 2012 à 2025, les données consolidées l'année en cours.

## Outillage

Le modèle a été édité avec le serveur MCP Power BI Modeling et le rapport avec `powerbi-report-author` (CLI), via Claude Code.
