# Policy Sentinel  
Agent d'analyse Web3 pour Warden — rendre l'écosystème Web3 plus lisible, plus sûr et plus accessible.

## Présentation  
Policy Sentinel est un agent conçu pour analyser un sujet Web3 (token, protocole, projet), synthétiser les informations importantes, évaluer les risques et envoyer des alertes en cas de menace identifiée.

Il fonctionne au sein de Warden et peut être piloté en langage naturel par un LLM.  
L’objectif est de permettre à n’importe qui, professionnel ou non, de comprendre rapidement ce qui se passe dans l’écosystème Web3, sans passer des heures à chercher, vérifier ou analyser des informations techniques.

## Pourquoi utiliser Policy Sentinel  
Policy Sentinel apporte une valeur immédiate dans un environnement complexe, rapide et souvent opaque.

| Bénéfice | Description |
|----------|-------------|
| Vision claire | Vous posez une question, l’agent analyse et renvoie une synthèse compréhensible |
| Aide à la décision | Évaluation structurée basée sur la réputation, les signaux faibles et les anomalies |
| Surveillance automatisée | Alertes automatiques dès qu’un risque apparaît |
| Intégration naturelle | Piloté en langage naturel via Warden |
| Expérience simple | Aucune installation complexe, aucune compétence technique requise |

## Exemples d'utilisation  
- Analyse d’un token et détection de signaux de risque  
- État des lieux d’un protocole DeFi  
- Détection d’alertes ou d’anomalies autour d’un projet Web3  
- Surveillance continue avec alerte en cas de dépassement d’un seuil de risque  

## Flux fonctionnel  
Utilisateur  
→ Warden  
→ LLM  
→ Policy Sentinel (API)  
→ Collecte Web3 (scraping / requêtes HTTP)  
→ Analyse et calcul du niveau d’alerte  
→ Réponse structurée vers le LLM  
→ Warden  
→ Utilisateur  

En parallèle :  
Policy Sentinel → Alerte Telegram (si risque élevé)

## Architecture  
+---------------------------+  
|       Policy Sentinel     |  
|        (FastAPI App)      |  
+-------------+-------------+  
              |  
   +----------+----------+  
   |                     |  
+--v--------+       +----v------+  
|  Routers  |       | Templates |  
| API Layer |       | Structure |  
+--+--------+       +----+------+  
   |                     |  
+--v--------+       +----v------+  
| Sentinelle|       |  Config   |  
| Core Logic|       | Paramètres|  
+--+--------+       +-----------+  
   |  
+--v--------+  
|  Scraping |  
| Web3/DeFi |  
+-----------+

## Architecture Warden
                          +-----------------------+
                          |      Warden / LLM     |
                          +-----------+-----------+
                                      |
                                      v
                          +-----------------------+
                          |    Policy Sentinel    |
                          |      (FastAPI App)    |
                          +-----------+-----------+
                                      |
        +-----------------------------+-----------------------------+
        |                             |                             |
        v                             v                             v
+---------------+           +------------------+          +------------------+
|   Module      |           |    Module        |          |    Module        |
|   Security    |           |     Crypto       |          |     Analyse      |
| (adresses,    |           | (scraping,       |          | (scores,         |
|  etherscan)   |           |  web3, defi)     |          |  signaux faibles)|
+-------+-------+           +---------+--------+          +---------+--------+
        |                             |                             |
        +-----------------------------+-----------------------------+
                                      |
                                      v
                          +-----------------------+
                          |    Module Alertes     |
                          | (détection, Telegram) |
                          +-----------+-----------+
                                      |
                                      v
                          +-----------------------+
                          |   Réponse structurée  |
                          |   (JSON pour Warden)  |
                          +-----------------------+



## Arborescence du projet  
policy-sentinel/  
├── sentinelle/        (Cœur logique : analyse, scoring, orchestration)  
├── routers/           (Routes API : analyse, alertes)  
├── templates/         (Structures internes)  
├── config/            (Configuration : seuils, clés, paramètres)  
├── main.py            (Point d'entrée API)  
└── requirements.txt   (Dépendances Python)

## Fonctionnalités principales  
- Analyse ciblée d’un sujet Web3 (token, protocole, projet)  
- Collecte d’informations via scraping et requêtes HTTP  
- Agrégation et filtrage des données  
- Analyse de risque (signaux faibles, réputation, anomalies)  
- Calcul d’un niveau d’alerte  
- Retour structuré pour un LLM (texte + données)  
- Alerte Telegram pour l’administrateur  

## Monétisation — Mode Freemium & Mode OPEN  

### Mode Freemium (gratuit)  
| Fonctionnalité | Limite |
|----------------|--------|
| Analyses par mois | 3 |
| Sources | 3 maximum |
| Unités par source | 5 maximum |
| Données brutes | Non |
| Scoring | Non |
| Alerte Telegram | Non |

Exemple de réponse Freemium :  
{
  "mode": "freemium",
  "usage": {
    "remaining_free_requests": 2,
    "limit": 3,
    "reset_date": "01/03/2026"
  },
  "analysis": {
    "summary": [],
    "sources_used": [],
    "risk_level": "modéré",
    "raw_data": null,
    "scoring": null,
    "telegram_alert": false
  }
}

### Mode OPEN (payant — 0.10 USDC / requête)  
| Fonctionnalité | Disponibilité |
|----------------|--------------|
| Sources | Illimitées |
| Volume | Illimité |
| Données brutes | Oui |
| Scoring détaillé | Oui |
| Alerte Telegram | Oui |

Exemple de réponse OPEN :  
{
  "mode": "open",
  "analysis": {
    "summary": [],
    "sources_used": [],
    "risk_level": "faible",
    "raw_data": {},
    "scoring": {},
    "telegram_alert": true
  },
  "billing": {
    "price": "0.10 USDC",
    "charged": true
  }
}

## Intégration avec Warden et un LLM  
Policy Sentinel fonctionne exclusivement via Warden.  
L’utilisateur final n’a rien à installer : il interagit simplement en langage naturel.

Le LLM peut :  
- Appeler la route d’analyse  
- Transmettre un sujet, un contexte, des paramètres  
- Recevoir une réponse structurée (texte + données)  
- Présenter cette réponse à l’utilisateur final  

## Alertes Telegram (admin uniquement)  
Policy Sentinel peut envoyer des alertes Telegram lorsqu’un risque important est détecté.  
Cette fonctionnalité est interne et destinée uniquement à l’administrateur.

### Configuration  
Définir les variables d’environnement suivantes :  
TELEGRAM_BOT_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxxxxxx  
TELEGRAM_CHAT_ID=yyyyyyyyyyyy  

TELEGRAM_BOT_TOKEN : token généré via BotFather  
TELEGRAM_CHAT_ID : identifiant du destinataire (compte ou groupe)  

Ces informations ne transitent jamais par Warden et ne sont jamais exposées à l’utilisateur final.

### Contenu d’une alerte  
Une alerte Telegram contient :  
- Le sujet analysé  
- Le niveau d’alerte  
- Un résumé court  
- Éventuellement un lien ou une donnée contextuelle  

## Auteur  
Claude Vaillant  
Consultant IA — Automatisation — Agents Warden  
France
