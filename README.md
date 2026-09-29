# Agents de sourcing Kalent

Six agents IA qui couvrent le sourcing de bout en bout, de la fiche de poste aux relances, dans Claude, ChatGPT ou Grok. Ils s'appuient sur le MCP Kalent (`https://app.kalent.ai/api/mcp`). Vous validez, ils exécutent : rien ne part sans votre accord.

| Agent | Skill | Ce qu'il fait |
|---|---|---|
| Brief | `kalent-brief` | Lit la fiche de poste, en sort les critères, demande ce qui manque (quota, taille des comptes...) |
| Sourcing | `kalent-sourcing` | Cherche dans tout le marché, y compris les profils que LinkedIn ne vous montre pas |
| Qualification | `kalent-qualification` | Vérifie chaque critère sur chaque profil et dit pourquoi il le garde |
| Enrichissement | `kalent-enrichissement` | Trouve le mobile et l'email perso |
| Outreach | `kalent-outreach` | Écrit un premier message personnalisé à partir du parcours |
| Relance | `kalent-relance` | Relance par WhatsApp ou email quand LinkedIn ne répond pas |
| Flow complet | `kalent-flow` | Enchaîne les six agents avec une validation à chaque étape |

## Prérequis

Un compte Kalent (création sur app.kalent.ai). La recherche qualifiée, les sourcings et les séquences demandent un abonnement Kalent éligible. Pour les séquences LinkedIn, votre compte LinkedIn doit être synchronisé dans Kalent (Recruiter ou Sales Navigator pour l'InMail).

## Claude Code

```bash
claude mcp add --transport http kalent https://app.kalent.ai/api/mcp
```

Puis dans Claude Code :

```
/plugin marketplace add edouard-kalent/sourcing-agents
/plugin install kalent-sourcing@kalent
```

## Claude (web et desktop)

1. Paramètres > Connecteurs > Ajouter un connecteur personnalisé > URL `https://app.kalent.ai/api/mcp`, puis connexion à Kalent.
2. Paramètres > Capacités > Skills > Importer : un fichier .zip par dossier de `skills/` (commencez par `kalent-flow`).

## ChatGPT

1. Paramètres > Connecteurs > Avancé > activer le mode développeur.
2. Créer une app avec l'URL `https://app.kalent.ai/api/mcp`, puis connexion à Kalent.
3. Créer un projet « Sourcing Kalent » et coller `chatgpt/instructions-projet.md` dans ses instructions.

## Grok

Ouvrez le Grok Kalent (flow complet) : https://x.ai/bot/EmQ0HDJ_-lZ_98KbbNVa2

Ou collez `grok/instructions-grok.md` dans les instructions de votre propre projet Grok.

## Structure

```
.claude-plugin/     plugin et marketplace Claude Code
skills/<agent>/     un SKILL.md par agent
chatgpt/            instructions de projet ChatGPT (flow complet)
grok/               instructions Grok (flow complet)
```

Documentation du MCP : https://docs.kalent.ai/mcp/overview
