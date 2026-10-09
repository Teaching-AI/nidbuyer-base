# Module 3 — Gouvernance de l'IA

**16 octobre 2026** · 9h30 → 17h30, à distance (Teams) · **la matinée commence en autonomie**

Au M1, vous avez construit un agent. Au M2, vous l'avez mesuré. Il reste une question que tout client,
tout juriste et tout jury vous posera : **quand il se trompe, qui en répond, et comment prouvez-vous
que vous avez fait ce qu'il fallait ?**

Ce module ne fait pas de vous des juristes. Il vous apprend à poser les bonnes questions, à savoir quand
appeler le juriste ou le DPO, et surtout à relier chaque règle à une **preuve** : un test, une mesure, un journal.

> La phrase à savoir dire : *« NidBuyer est un système à risque limité. Voici nos 8 risques,
> la mesure prise pour chacun, et le test qui le prouve. »* Pas « on est conformes ».

## Objectifs

À la fin du module, l'étudiant sait :
- dire qui est responsable de quoi autour d'un agent : fournisseur, déployeur, sous-traitant ;
- classer un système selon l'AI Act, avec le calendrier d'après l'Omnibus de 2026 ;
- appliquer le RGPD à un agent : ce qui passe par le prompt, les traces, le fournisseur du modèle ;
- dire quand une AIPD s'impose, et tenir un registre des risques ;
- relier chaque mesure à une preuve tirée du jeu d'évaluation ;
- fixer les règles d'un agent qui agit : outils, bac à sable, budget, validation humaine.

## Déroulé

| Horaire | Contenu |
|---|---|
| **9h30** | **En autonomie** (consignes ci-dessous) : Projet 1, puis [TP 0 — Carte des données](TP0-carte-donnees.md) |
| 11h00 | Retour sur le M2, point sur le Projet 1 |
| 11h15 | Cours : qui est responsable ? Cas réels, rôles, AI Act après l'Omnibus, exercice de classification |
| 12h15 | Pause déjeuner |
| 13h30 | Cours : les données d'un agent, RGPD, fournisseur, collecte, AIPD, registre des risques, biais |
| 14h15 | **TP 1** — [Audit de gouvernance de NidBuyer](TP1-audit-gouvernance.md) (1h15) |
| 15h30 | Pause |
| 15h45 | Cours : gouverner un agent qui agit |
| 16h05 | **TP 2** — [La charte de votre agent Kaggle](TP2-charte-agent.md) (55 min) |
| 17h00 | Restitution, QCM noté (10 min), clôture |

## 9h30 → 11h00 : la matinée en autonomie

Connectez-vous à Teams à 9h30, en salle de groupe. Les consignes sont ici ; les questions vont dans le canal.

1. **Projet 1 (jusqu'à 10h15 environ)** — c'est l'échéance du jour : **binôme inscrit sur Kaggle et première
   soumission valide** (2 + 2 points). Suivez la procédure de soumission postée dans le canal Teams.
   Écrivez l'entrée correspondante dans votre `journal.md` (hypothèse, changement, résultat, conclusion).
   Une seule soumission par jour et par équipe : ne la gaspillez pas.
2. **TP 0 — Carte des données (45 min)** — [énoncé](TP0-carte-donnees.md). Il sert de point de départ
   au cours de 13h30 : deux groupes présenteront leur carte.

À 11h00, tout le monde revient en réunion principale.

## Contenu du cours

### 1. « C'est l'IA » n'a jamais été une défense

| Cas | Ce qui s'est passé | Qui a répondu |
|---|---|---|
| Air Canada, 2024 | Le chatbot invente une politique de remboursement | La compagnie, condamnée à rembourser |
| Fisc néerlandais, 2013–2019 | Un modèle de risque cible des familles pour fraude aux allocations, la nationalité parmi les critères | Le gouvernement démissionne (2021), amende de 2,75 M€ |
| OpenAI, Italie, 2024 | Données d'entraînement sans base légale, information insuffisante, pas de contrôle d'âge | 15 M€ d'amende au titre du RGPD |
| Replit, 2025 | Un agent de code efface la base de production pendant un gel du code | L'éditeur s'excuse, rembourse, sépare dév. et prod. |

Dans les quatre cas, une organisation humaine paie.

### 2. Les rôles autour de NidBuyer

| Acteur | AI Act | RGPD |
|---|---|---|
| Google (Gemini) | Fournisseur d'un modèle d'IA à usage général | Sous-traitant des appels API |
| NidDouillet | **Fournisseur** du système NidBuyer (il le développe et le met en service sous son nom) **et déployeur** | **Responsable de traitement** |
| Le conseiller | Utilisateur interne, à former (art. 4) | — |
| L'acheteur | Personne qui doit savoir qu'elle parle à une IA (art. 50) | Personne concernée |

Vous construisez pour NidDouillet : vos choix techniques engagent NidDouillet.

### 3. Trois textes à la fois

- **AI Act** — règlement (UE) 2024/1689 : le produit, selon son niveau de risque.
- **RGPD** — règlement (UE) 2016/679 : les données personnelles, dès qu'il y en a, quel que soit le risque.
- **Le reste du droit** : responsabilité civile, droit de la consommation (Air Canada a été condamnée sans loi sur l'IA),
  droit des bases de données, non-discrimination, et la nouvelle directive sur les produits défectueux (UE) 2024/2853,
  qui couvre les logiciels (transposition au plus tard le 9 décembre 2026).

### 4. L'AI Act : on régule l'usage, pas la technologie

| Niveau | Exemples | Ce qu'il faut faire |
|---|---|---|
| **Interdit** (art. 5) | Notation sociale, manipulation, reconnaissance des émotions au travail et à l'école, collecte non ciblée de visages | Ne pas le faire |
| **Haut risque** (art. 6, annexe III) | Crédit et solvabilité des personnes, recrutement, éducation, accès aux services essentiels, justice | Gestion des risques, qualité des données, documentation, journalisation, supervision humaine, robustesse, enregistrement, marquage CE |
| **Transparence** (art. 50) | Chatbots, contenus générés, hypertrucages | Dire que c'est une IA, marquer les contenus |
| **Minimal** | Anti-spam, recommandations, outils internes | Rien de spécifique |

À part : les **modèles d'usage général** (Gemini, Gemma) ont leurs propres obligations, côté fournisseur du modèle.

Le même modèle peut être minimal dans un filtre anti-spam et à haut risque dans un outil de crédit.
**Une fonctionnalité peut changer le régime** : NidBuyer qui conseille relève de la transparence ;
NidBuyer qui calcule un score « êtes-vous finançable ? » transmis à une banque devient un système à haut risque.

### 5. Le calendrier, après l'Omnibus

| Date | Ce qui s'applique |
|---|---|
| 1 août 2024 | Entrée en vigueur de l'AI Act |
| 2 févr. 2025 | Pratiques interdites ; maîtrise de l'IA (art. 4) |
| 2 août 2025 | Modèles d'usage général ; sanctions |
| 27 juil. 2026 | Omnibus IA en vigueur : règlement (UE) 2026/1744 |
| **2 août 2026** | **Transparence (art. 50) : NidBuyer est concerné** |
| 2 déc. 2026 | Marquage des contenus générés pour les systèmes déjà sur le marché ; nouvelle interdiction (contenus intimes non consentis) |
| 2 déc. 2027 | Haut risque, annexe III (initialement 2 août 2026) |
| 2 août 2028 | Haut risque intégré à des produits, annexe I |

Sanctions : jusqu'à 35 M€ ou 7 % du chiffre d'affaires mondial pour les pratiques interdites, 15 M€ ou 3 % pour les autres obligations.
L'Omnibus a aussi assoupli la maîtrise de l'IA (art. 4) : il faut toujours former ses équipes, sans garantir un niveau individuel.
Le volet RGPD de l'Omnibus n'est **pas** adopté (septembre 2026) : le RGPD actuel s'applique.

**Dans ce domaine, une date se vérifie toujours le jour où on la cite.**

### 6. Le RGPD en six questions

| Question | Pour NidBuyer |
|---|---|
| Pourquoi ? (finalité) | Conseiller un acheteur sur un bien à Toulon. Pas : revendre des profils |
| À quel titre ? (base légale) | Le service demandé par l'acheteur ; pour les alertes par e-mail, son consentement |
| Quoi, au minimum ? | Budget, quartier, surface. Pas : revenus détaillés, situation de famille, santé |
| Combien de temps ? | Une durée écrite pour les conversations, les traces, les alertes |
| Quels droits ? | Accès, effacement, opposition ; savoir qu'une IA répond |
| Qui d'autre ? | Google, l'hébergeur : un contrat chacun, transferts hors UE encadrés |

### 7. Dans un agent, les données vont où on ne regarde pas

| Où | Dans le code | Le problème |
|---|---|---|
| Le prompt | `description_libre`, `backend/main.py` | L'acheteur écrit ce qu'il veut, y compris des données de santé |
| Le fournisseur | `backend/llm.py` | Chaque question part chez Google |
| Les traces | `eval_resultats.json`, `exercices/m2_eval.py` | Questions et réponses en clair, et dans git si on les pousse |
| Les alertes | `data/alertes.json`, `backend/alert.py` | E-mail et profil en clair, sans durée ni désabonnement |
| Les annonces | `backend/sources/` | Nom et téléphone des vendeurs particuliers |

**Le fournisseur.** Les conditions de l'API Gemini (mise à jour du 28 avril 2026) prévoient pour l'offre gratuite que
« des relecteurs humains peuvent lire, annoter et traiter » les entrées et sorties, et demandent de ne pas y envoyer
d'informations personnelles. Mais pour les utilisateurs situés dans l'EEE, en Suisse ou au Royaume-Uni, ce sont
les conditions de l'offre payante qui s'appliquent à tous les services. On ne devine pas : on lit les conditions, avec leur date.
En production : offre payante, contrat de sous-traitance, région européenne (M4).

### 8. Collecter des annonces

Notre propre template (`backend/sources/leboncoin.py`) suggère d'appeler l'API interne de Leboncoin.
À ne pas faire sans accord :
- **Bases de données** : Leboncoin c. Entreparticuliers (CA Paris, 2 février 2021) — reprendre chaque jour les annonces
  d'un site concurrent a coûté 50 000 € de préjudice économique et 20 000 € de préjudice d'image.
- **RGPD** : la CNIL admet le moissonnage sous conditions — exclure les sites qui s'y opposent (CGU, `robots.txt`),
  filtrer dès la collecte, supprimer ce qui n'est pas nécessaire (le téléphone du vendeur).
- **DVF**, pourtant ouvert : interdit de réidentifier les personnes, ou de faire indexer les ventes par un moteur de recherche.

Pour le P2 : un instantané figé et daté, avec sa source et ses conditions d'utilisation écrites ; mieux, une API officielle.

### 9. L'AIPD et le registre des risques

**AIPD** (RGPD, art. 35) : obligatoire en principe dès que **deux critères sur neuf** sont réunis
(évaluation ou profilage, décision automatique, surveillance systématique, données sensibles ou très personnelles,
grande échelle, croisement de données, personnes vulnérables, usage innovant, blocage d'un droit).
NidBuyer coche au moins « usage innovant » et « données très personnelles ». Une AIPD regarde les risques **pour les personnes** ;
la CNIL fournit un outil gratuit, PIA.

**Registre des risques** : tous les risques, pour les personnes et pour l'entreprise. Une ligne par risque :
risque, qui est touché, gravité × probabilité (1 à 3), mesure, **preuve**, responsable (un nom).
Modèle : [`registre-risques.md`](registre-risques.md).

> **Une mesure sans preuve n'est qu'une intention.** Une preuve, c'est un scénario du jeu d'évaluation, un test,
> une mesure. Pas « on a mis une consigne dans le prompt ».

### 10. Biais et discrimination

Orienter ou refuser selon l'origine, la situation de famille ou le handicap est un délit (Code pénal, art. 225-1 et 225-2),
que la décision vienne d'un humain ou d'un agent. Aux États-Unis, Meta a dû refondre en 2022 l'algorithme qui diffusait
les annonces de logement. La preuve se fait par **test par paires** : deux questions identiques, un seul mot change
(un prénom, une situation) ; les réponses doivent être les mêmes. Attention au bruit (M2) : on répète avant de conclure.

### 11. Un agent qui agit

**Replit, juillet 2025** : un agent de code efface la base de production pendant un gel du code, alors que la consigne
« ne rien changer sans accord » était dans la conversation. Ce qui manquait n'était pas une meilleure consigne,
mais de l'architecture : séparer dév. et prod., valider dans le système, interdire les commandes destructrices, des sauvegardes testées.

**L'échelle d'autonomie** : 0 — l'agent suggère ; 1 — il agit, un humain valide (pull request) ;
2 — il agit seul dans un bac à sable (votre agent Kaggle) ; 3 — il agit seul en production.
**On n'automatise que ce qu'on sait défaire.**

**Ce que la compétition Kaggle a déjà verrouillé** : pas d'internet ; un conteneur neuf par bug ; un second conteneur
qui remet les tests d'origine (l'agent ne peut pas « réussir » en modifiant les tests : Goodhart) ; 12 h et des appels plafonnés ;
des outils déclarés ; un jeu caché. En entreprise, personne ne le fait pour vous.

**Six règles pour un agent de code** : moindre privilège · bac à sable · budget · humain avant l'irréversible
(fusion, déploiement, suppression, envoi) · journal complet · interrupteur. Et le commit porte aussi le nom
de la personne qui l'a validé.

## Supports

- Slides : [`M3-gouvernance-ia.pdf`](M3-gouvernance-ia.pdf)
- [TP 0 — Carte des données](TP0-carte-donnees.md) (autonomie, matin)
- [TP 1 — Audit de gouvernance de NidBuyer](TP1-audit-gouvernance.md)
- [TP 2 — La charte de votre agent Kaggle](TP2-charte-agent.md)
- [Modèle de registre des risques](registre-risques.md)

## Ressources

**Textes**
- [AI Act, règlement (UE) 2024/1689](https://eur-lex.europa.eu/eli/reg/2024/1689/oj?locale=fr)
- [Omnibus IA, règlement (UE) 2026/1744](https://eur-lex.europa.eu/legal-content/FR/TXT/?uri=OJ:L_202601744) (JOUE du 24 juillet 2026)
- [RGPD, règlement (UE) 2016/679](https://eur-lex.europa.eu/eli/reg/2016/679/oj?locale=fr)
- [Directive (UE) 2024/2853 sur la responsabilité du fait des produits défectueux](https://eur-lex.europa.eu/eli/dir/2024/2853/oj?locale=fr)

**Guides**
- [CNIL — Les fiches pratiques IA](https://www.cnil.fr/fr/les-fiches-pratiques-ia)
- [CNIL — Intérêt légitime et collecte par moissonnage](https://www.cnil.fr/fr/focus-interet-legitime-collecte-par-moissonnage)
- [CNIL — L'analyse d'impact (AIPD) et l'outil PIA](https://www.cnil.fr/fr/aipd)
- [Conditions de l'API Gemini](https://ai.google.dev/terms) (lire la date de mise à jour)

**Cas**
- Moffatt c. Air Canada, 2024 BCCRT 149
- Garante per la protezione dei dati personali, décision contre OpenAI, décembre 2024 (15 M€)
- Autoriteit Persoonsgegevens, amende au fisc néerlandais, décembre 2021 (2,75 M€)
- Cour d'appel de Paris, 2 février 2021, LBC France c. Entreparticuliers, n° 17/17688
- [Incident Replit, juillet 2025](https://www.eweek.com/news/replit-ai-coding-assistant-failure/) ; [AI Incident Database](https://incidentdatabase.ai/)
