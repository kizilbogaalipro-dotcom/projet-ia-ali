# Réponse aux avis clients — Le Fournil des Monts

> Projet IA — BTS SIO 2 SLAM — Ali [NOM] — octobre 2026
> **URL publique** : https://[…].trycloudflare.com — code d'accès envoyé à l'enseignant par e-mail

## 1. Concevoir

**Sujet choisi** : n° 4 — Réponse aux avis clients — Le Fournil des Monts

**L'organisation (fictive) et son besoin**, en trois phrases : Le Fournil des Monts est une boulangerie fictive avec plusieurs boutiques, qui reçoit des avis clients que l'équipe n'a pas le temps de traiter un par un. Aujourd'hui, les réponses sont rédigées à la main, avec un ton variable, ou ne sont pas faites. L'application propose un brouillon de réponse poli, cohérent avec la marque, et signé automatiquement.

**Trois cas d'usage** :

1. En tant que gérant d'une boutique, je veux coller un avis positif et obtenir une réponse de remerciement, afin de répondre vite et avec le bon ton.
2. En tant que gérant, je veux coller un avis négatif (remboursement, boutique fermée) et obtenir une réponse qui reconnaît le problème sans promettre ce que je ne peux pas tenir, afin de limiter les conflits.
3. En tant que gérant, je veux que les questions sensibles (gluten, allergènes) soient traitées prudemment, afin de ne donner aucune information risquée.

**Ce que l'application ne fait pas** :

- Elle ne publie rien : elle propose un brouillon, un humain relit et envoie.
- Elle ne répond pas aux demandes hors sujet ni aux tentatives de détournement des consignes.
- Elle ne garantit aucune information sur les allergènes.

## 2. Le modèle et la machine

| | |
|---|---|
| Carte graphique et mémoire vidéo (VRAM) | aucune, processeur seul (Intel Core i5-1235U, 12e génération) |
| Mémoire vive | 16 Go |
| Modèle retenu | `qwen2.5:7b` |
| Pourquoi celui-là | Sans carte graphique, tout tourne sur le processeur. Le 7b est plus lent mais réussit nettement plus de cas (voir §4) ; la lenteur est acceptable pour un brouillon relu par un humain. |
| Modèle comparé | `qwen2.5:3b` (premier modèle utilisé, environ 8,5 tokens/s sur le processeur) |

## 3. Piloter — le journal

| Séance | Ce qui est fait | Ce qui a bloqué, et comment c'est réglé |
|---|---|---|
| Lundi 05/10 | … | … |
| Mardi 06/10 | … | … |
| … | … | … |

## 4. Mesurer

Jeu de **14 cas** dans `cas.json` (dont des cas hors sujet et une tentative de détournement des consignes), chaque cas rejoué **3 fois** avec `evaluer.py`.

| | Modèle retenu (`qwen2.5:7b`) | Modèle comparé (`qwen2.5:3b`) |
|---|---|---|
| Réussite (sur 14 cas × 3 essais) | 90 % | 74 % |
| Temps de réponse médian | 14,1 s | 8,1 s |

**Ce que les échecs montrent** : [deux exemples commentés tirés de `resultats-….csv`, et ce que j'ai changé dans le prompt ou le code]

**Historique des modifications** :

| Version | Modification | Effet |
|---|---|---|
| Départ | prompt initial | [… %] |
| n°1 | [… ] | [… %] |
| n°2 | quatre exemples ajoutés dans `prompt.txt` (remboursement, gluten, pirate, boutique fermée) | [… %] |
| n°3 | [… ] | [… %] |
| n°4 | [… ] | [… %] |
| Code | signature ajoutée par `main.py`, pas par le modèle | signature toujours présente |

**Choix du modèle** : [justification avec mes mots : 90 % contre 74 %, 14,1 s contre 8,1 s, pourquoi la lenteur est acceptable ici].

## 5. Sécuriser

| Risque | Ce qui pourrait arriver | Mesure prise dans le projet |
|---|---|---|
| L'URL est publique | n'importe qui utilise mon PC et mon électricité | code d'accès vérifié côté serveur, limite de requêtes [à confirmer] |
| Ollama exposé | quelqu'un appelle directement le modèle sans passer par l'application | Ollama n'est joignable que depuis la machine (ou le réseau Docker) ; seule l'API passe par le tunnel |
| Détournement des consignes | un texte d'avis demande au modèle d'ignorer ses règles | consignes dans le prompt, cas de test dédié dans `cas.json` |
| Données personnelles | un avis contient un nom ou un numéro de téléphone | [aucune donnée stockée / à préciser] |
| Secrets dans le dépôt | le code d'accès ou la configuration fuite sur GitHub | `.env` ignoré par Git, `.env.example` sans valeurs réelles |

## 6. Mettre en production — comment refaire

```bash
cp .env.example .env      # puis remplir
docker compose up -d --build
docker compose ps
docker compose logs tunnel
```

## 7. Usage de l'IA pendant le projet

J'ai utilisé Claude comme assistant pour [ce que j'ai demandé : rédaction du prompt, script d'évaluation, analyse des échecs, README].

- **Gardé** : [ … ]
- **Modifié** : [ … ]
- **Refusé ou corrigé** : [ … ]

Limites de la mesure : 14 cas seulement, 3 essais par cas, un seul évaluateur ; les pourcentages donnent un ordre de grandeur, pas une précision.

## 8. Local ou en ligne ? (séance 24)

**Séance 24 — mon nouveau sujet** (différent du premier) : n° … — … · **premier sujet** : n° 4 · pourquoi ce choix : …

**Les fournisseurs essayés** (conditions relevées le …/10/2026, avec le lien) :

| | Fournisseur 1 : … | Fournisseur 2 : … |
|---|---|---|
| Ce qu'il a fallu pour l'utiliser (compte, téléphone, clé…) | … | … |
| Où partent les données (pays, société) | … | … |
| Les requêtes peuvent-elles servir à entraîner ? (citer la phrase des conditions) | … | … |
| Limite de l'offre gratuite rencontrée (429 ?) | … | … |

**La mesure** — même `cas.json`, même `prompt.txt`, même température, trois essais :

| | Local (Ollama, `…`) | En ligne (…, `…`) |
|---|---|---|
| Réussite | … % | … % |
| Temps médian | … s | … s |
| Cas qui changent de verdict | … | … |

**La décision** : pour l'organisation de mon sujet, avec ses vraies données, je choisirais … parce que … (données personnelles ? données internes ? coût ? disponibilité ?). Règle : « les textes qui contiennent … ne partent jamais vers … ».
