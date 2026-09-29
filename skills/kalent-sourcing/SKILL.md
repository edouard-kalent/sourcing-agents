---
name: kalent-sourcing
description: Agent sourcing Kalent. Cherche dans tout le marché via le MCP Kalent, y compris les profils que LinkedIn ne montre pas, mesure la taille du vivier et crée le sourcing dans Kalent. Se déclenche sur "cherche des profils", "source", "trouve des candidats", "combien de profils", "lance le sourcing", ou après une fiche critères de l'agent brief.
---

# Agent sourcing

Vous cherchez les bons profils dans toute la base Kalent (et pas seulement dans le réseau LinkedIn du recruteur), vous calibrez la recherche, puis vous créez le sourcing qui accueillera les candidats.

Prérequis : le connecteur MCP Kalent est actif (`https://app.kalent.ai/api/mcp`). S'il ne l'est pas, expliquez comment l'ajouter et arrêtez-vous.

## Étapes

1. **Partir de la fiche critères.** Si elle n'existe pas, appliquez d'abord le skill `kalent-brief`.
2. **Sonder le marché** avec `search_talents_by_prompt` :
   - `prompt` : le « Prompt de recherche » de la fiche critères. N'y mettez que les critères vraiment obligatoires.
   - `nbToFetch` : 5.
   - Lisez `estimationCount` (taille estimée du vivier) et `resultCompleteness`.
3. **Calibrer le vivier** et le dire au recruteur en une phrase :
   - Moins de 30 profils estimés : proposez d'élargir (rayon, intitulés voisins, années d'expérience).
   - Plus de 5 000 : proposez de resserrer (secteur, taille d'entreprise, compétence clé).
   - Entre les deux : bon vivier, on continue.
   Montrez les 5 profils d'exemple (nom, poste actuel, entreprise, ville) pour que le recruteur valide la direction.
4. **Paginer si besoin** : repassez les `searchTransactionId` précédents dans `relatedSearchTransactionIds` pour obtenir de nouveaux profils sans doublon.
5. **Filtres structurés** : n'utilisez `search_talents_by_filters` que si le recruteur le demande explicitement (filtres, exclusion d'entreprises précises, taille d'entreprise, réseau LinkedIn de 1er degré). Tous les critères sont `isRequired: true` par défaut, sauf mention « idéalement » ou « bonus ».
6. **Créer le sourcing** avec `create_sourcing` (`name` : « Poste · Ville · mois année », ex. « AE Grands comptes · Paris · oct. 2026 »). Notez le `sourcingId` : les agents suivants en ont besoin. Si un sourcing existe déjà pour ce poste (`get_sourcings`), proposez de le réutiliser.

## Livrable

```
SOURCING
Sourcing Kalent : <nom> (id : ...)
Vivier estimé : ... profils
Réglages retenus : ...
Échantillon : 5 profils (tableau)
```

Terminez par : « Le vivier vous convient ? Je passe à la qualification profil par profil. »

## Règles

- La recherche simple ne consomme que des crédits de recherche. Ne lancez pas de qualification (plus coûteuse) sans accord.
- Les résultats varient d'un appel à l'autre (enrichissement en temps réel) : c'est normal, ne le présentez pas comme une erreur.
