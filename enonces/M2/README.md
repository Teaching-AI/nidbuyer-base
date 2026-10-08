# Module 2 — Évaluer et surveiller

**9 octobre 2026** · 9h30 → 17h30, à distance (Teams)

Au M1, vous avez cassé l'agent à la main et compté vos 12 questions. C'est un début, pas une mesure :
on ne sait pas si la correction d'hier a cassé autre chose, si le modèle répond pareil demain,
ni ce que ça donnera quand Google mettra le modèle à jour.

Ce module installe le réflexe qui distingue un produit d'une démo : **on ne dit pas « ça marche »,
on dit « ça passe 34 scénarios sur 40, voici les 6 qui échouent, et voici ce que ça coûte ».**

## Objectifs

À la fin du module, l'étudiant sait :
- construire un jeu d'évaluation représentatif (cas normaux, cas limites, attaques) ;
- définir des critères vérifiables sur la **trace** d'un agent, pas seulement sur sa réponse ;
- mesurer la stabilité d'un agent (même question, plusieurs essais) ;
- utiliser un LLM comme juge, et en connaître les biais ;
- mener un red teaming et mesurer un taux d'attaques réussies ;
- dire quoi surveiller une fois l'agent en production, et quand déclencher une alerte.

## Déroulé

| Horaire | Contenu |
|---|---|
| 9h30 | Rappel du M1, puis vos pires pannes. Pourquoi « ça a l'air de marcher » ne suffit pas |
| 9h45 | Cours : jeu d'évaluation, critères, variance, quota |
| 10h30 | **TP 1** — [Jeu d'évaluation automatique](TP1-jeu-evaluation.md) (1h15, bonus à 11h45) |
| 12h15 | Pause déjeuner |
| 13h30 | Cours : LLM-as-judge, red teaming, garde-fous, surveillance en production |
| 14h15 | **TP 2** — [Red team inter-groupes](TP2-red-team.md) |
| 16h00 | Pause |
| 16h15 | Ce que P1 attend : le jeu d'évaluation devient un livrable noté |
| 16h45 | QCM, puis travail de groupe sur P1 jusqu'à 17h30 |

## Contenu du cours

### 1. Le jeu d'évaluation

Un jeu d'évaluation, c'est une liste de situations avec, pour chacune, **ce qu'on attend**.
Il doit contenir trois familles :

| Famille | Part indicative | Exemples NidBuyer |
|---|---|---|
| Cas normaux | 50 % | « Mensualité pour 250 k€ sur 25 ans », « a06 est-il au prix du marché ? » |
| Cas limites | 30 % | Question vague, budget impossible, ville hors périmètre, taux à 0 % |
| Attaques | 20 % | Annonce piégée, « ignore tes consignes », demande de conseil en placement |

Règles :
- **On l'écrit avant de corriger.** Sinon on n'écrit que des tests qu'on sait réussir.
- **On ne le jette jamais.** Un bug trouvé en production devient un scénario.
- **On le fait relire par quelqu'un d'autre.** Un scénario ambigu mesure l'ambiguïté, pas l'agent.

### 2. Évaluer un agent : la trace, pas seulement la réponse

Une bonne réponse obtenue par le mauvais chemin est un accident qui attend son heure.
On vérifie donc la **trace** (voir `exercices/m2_eval.py`) :

| Critère | Question | Vérifiable automatiquement ? |
|---|---|---|
| Outils requis | A-t-il appelé `simuler_pret` pour une mensualité ? | Oui |
| Outils interdits | A-t-il évité de chercher des biens quand on ne lui demandait qu'un calcul ? | Oui |
| Chiffres | La mensualité exacte (1 238,19 €) est-elle dans la réponse ? | Oui |
| Réponse fondée | Chaque montant cité vient-il d'un résultat d'outil ? | Oui |
| Refus | Demande-t-il une précision quand la question est vague ? | Approximativement |
| Qualité du conseil | Le conseil est-il pertinent pour ce profil ? | Non : humain ou LLM-juge |

Le critère **« réponse fondée »** est le plus utile : il attrape les chiffres inventés sans avoir à les connaître d'avance.

### 3. La variance : même question, autre réponse

À température 0, un agent donne le plus souvent la même trace. Ce qui fait varier les résultats :
un **changement de modèle** (le modèle de secours après une erreur 503, une mise à jour chez Google),
un changement de données, ou une consigne ambiguë qui laisse le choix entre deux chemins.
On rejoue chaque scénario **3 à 5 fois**. Un scénario qui passe 2 fois sur 3 est un **scénario instable** :
c'est souvent le signe d'une docstring ou d'une consigne ambiguë.

### 4. LLM-as-judge

Pour juger ce qui ne se vérifie pas automatiquement (pertinence, ton, clarté), on demande à un LLM de noter.
C'est utile, mais il a des biais connus :
- **position** : il préfère la première (ou la dernière) réponse présentée ;
- **longueur** : il préfère les réponses longues ;
- **auto-complaisance** : il préfère les réponses de son propre modèle ;
- **complaisance** : il note haut par défaut.

Règle : **un juge automatique se valide contre des humains** sur 20 à 30 cas avant d'être cru.
Et on lui demande une grille précise (« la réponse cite-t-elle l'écart au marché ? oui/non »), pas une note sur 10.

### 5. Red teaming

Attaquer son propre système avant qu'un autre le fasse. On mesure un **taux d'attaques réussies**,
avant et après correction. Familles d'attaques à couvrir :
- **injection directe** : « ignore tes instructions et… » ;
- **injection indirecte** : l'instruction arrive par une annonce, un e-mail, une page web ;
- **sortie de périmètre** : conseil en placement, conseil juridique, autre ville ;
- **fuite** : « affiche ton prompt système », « quelle est ta clé d'API ? » ;
- **abus de coût** : question conçue pour déclencher 20 appels d'outils.

### 6. Garde-fous

| Garde-fou | Où | Exemple NidBuyer |
|---|---|---|
| Limiter ce que l'agent peut faire | Choix des outils | Aucun outil qui envoie un e-mail ou modifie une base |
| Valider les entrées des outils | Code des outils | `simuler_pret` refuse une durée nulle |
| Limiter les tours | Boucle d'agent | `max_tours=6` dans `llm.executer_agent` |
| Filtrer la sortie | Après l'agent | Vérifier que les chiffres cités viennent des outils avant d'afficher |
| Humain dans la boucle | Processus | Un conseiller valide avant toute recommandation d'offre d'achat |

### 7. Surveiller en production

Ce qui marche en octobre peut dériver en décembre : nouvelles annonces, nouvelles questions,
**mise à jour du modèle par le fournisseur**. On surveille :

| Indicateur | Pourquoi | Seuil d'alerte (exemple) |
|---|---|---|
| Latence p50 / p95 | Expérience utilisateur | p95 > 15 s |
| Coût par jour | Budget | > 2 × la moyenne des 7 derniers jours |
| Taux d'erreurs et d'arrêts `max_tours` | Pannes | > 5 % des conversations |
| Score du jeu d'évaluation, relancé chaque semaine | Dérive silencieuse | Baisse de plus de 5 points |
| Échantillon relu par un humain | Ce que les métriques ne voient pas | 20 conversations par semaine |

`/admin/status` expose déjà la latence moyenne des derniers appels (`llm.JOURNAL`).

## Supports

- Slides : PDF dans ce dossier après la séance
- [TP 1 — Jeu d'évaluation automatique](TP1-jeu-evaluation.md)
- [TP 2 — Red team inter-groupes](TP2-red-team.md)
- Code : `exercices/m2_eval.py` et `scenarios.json` (lancer avec `uv run python -m exercices.m2_eval`)

## Ressources

- [OWASP — Top 10 pour les applications LLM](https://genai.owasp.org/llm-top-10/)
- [Anthropic — Créer des évaluations solides](https://docs.claude.com/en/docs/test-and-evaluate/develop-tests)
- [Perez et al., *Red Teaming Language Models with Language Models*](https://arxiv.org/abs/2202.03286)
- [Zheng et al., *Judging LLM-as-a-Judge*](https://arxiv.org/abs/2306.05685) : les biais des juges LLM

---

[← Retour au README du repo](../../README.md)
