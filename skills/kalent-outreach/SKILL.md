---
name: kalent-outreach
description: Agent outreach Kalent. Écrit un premier message personnalisé pour chaque candidat à partir de son parcours, le fait valider, puis prépare la séquence dans Kalent (LinkedIn en premier contact). Se déclenche sur "écris le message", "premier message", "approche", "outreach", "invitation LinkedIn", "InMail".
---

# Agent outreach

Vous écrivez le premier message de chaque candidat à partir de son vrai parcours, puis vous créez la séquence dans Kalent. Rien ne part sans l'accord du recruteur.

## Étapes

1. **Lire chaque parcours** avec `get_candidate` (`candidateId`) : poste actuel, ancienneté, entreprises, réalisations visibles, verdict de qualification.
2. **Choisir le canal du premier contact** avec le recruteur :
   - `LINKEDIN_INVITATION_WITH_MESSAGE` (300 caractères, par défaut),
   - `LINKEDIN_INMAIL` (nécessite Recruiter ou Sales Navigator synchronisé dans Kalent),
   - `LINKEDIN_MESSAGE` si déjà en relation.
3. **Écrire un message par candidat.** Structure : un détail précis de son parcours, le poste en une phrase, une question simple. Exemple :
   « Bonjour Julie, 6 ans à signer des grands comptes chez un éditeur SaaS, ça se remarque. Je recrute un Head of Sales pour une scale-up B2B à Paris, avec une équipe à monter. Ouverte à en parler 15 minutes cette semaine ? »
   Règles d'écriture : vouvoiement, 300 caractères max pour une invitation, pas de flatterie générique, pas de lien, pas de placeholder non résolu du type [entreprise].
4. **Faire valider** : présentez tous les messages numérotés. Le recruteur corrige, valide tout, ou valide au cas par cas.
5. **Créer la séquence** avec `create_sequence_blueprint` :
   - `sourcingId`, `name` : « Approche · <poste> ».
   - Étape 1 : `type: LINKEDIN`, `linkedInType` choisi, `temporalityType: ASAP`, `content` : le modèle validé.
   - Variables autorisées : `{{firstname}}`, `{{lastname}}`, `{{candidateJobTitle}}`, `{{candidateCompanyName}}`, `{{candidateLocation}}`, `{{sourcingJobTitle}}`, `{{sourcingLocation}}`, `{{recruiterFirstname}}`, `{{recruiterLastname}}`. Aucune autre.
   - Pour garder la personnalisation candidat par candidat, utilisez `contentSource: suggestedByKalent` avec `policy.autoValidateAIDraft: false` : Kalent prépare un brouillon par candidat que le recruteur valide dans l'app. Sinon, gardez un modèle commun avec variables.
6. **Ne pas démarrer la séquence ici.** L'agent relance ajoute les relances, puis démarre le tout après accord.

## Livrable

```
OUTREACH
Séquence : <nom> (blueprintId : ...)
Canal du premier contact : ...
Messages validés : n / n
```

Terminez par : « Premier message prêt. On ajoute les relances WhatsApp et email ? »
