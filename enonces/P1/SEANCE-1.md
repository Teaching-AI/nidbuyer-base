# P1 — Séance 1 : mise en route (30 octobre)

| Heure | Quoi | Où |
|---|---|---|
| 10h00 | Kick-off : scores, rythme, barème, dates | Réunion principale |
| 10h20 | **Exercice 1** : deux outils en ligne de commande, chaque binôme de son côté | Salle de groupe |
| 10h45 | Annonce : ce qui change à partir de maintenant | Réunion principale |
| 10h50 | **Exercices 2 à 5** : fusion, mise en commun, plan, lancer | Salle de groupe |
| 11h45 | Tour des groupes : votre première hypothèse d'équipe, et qui la porte | Réunion principale |

> Votre token Kaggle (`KGAT_…`) ne va **jamais** dans un repo, un message ou une capture d'écran.

---

## Exercice 1 — Deux outils en ligne de commande (25 min)

Dans chaque binôme : **une personne fait 1a, l'autre 1b**, en même temps (15 min).
Puis 10 min pour mettre vos réponses en commun dans un fichier `outils.md`.
Chaque réponse se termine par **une hypothèse pour le plan** (« donc on pourrait essayer… »).

### 1a — Kaggle CLI : voir sans cliquer

La Kaggle CLI fait en une commande ce que le site fait en dix clics : voir vos soumissions,
télécharger les données, suivre un notebook.

**Étape 1 — installer**

```bash
mkdir -p ~/p1-outils && cd ~/p1-outils
uv init --no-workspace
uv add kaggle
uv run kaggle --version
```

Ce que vous devez voir : un numéro de version (`Kaggle API 1.x…`).

**Étape 2 — le token**

Sur kaggle.com : photo de profil → *Settings* → *API* → *Create New Token*.
Copiez le token, puis :

```bash
mkdir -p ~/.kaggle
nano ~/.kaggle/access_token      # collez le token, enregistrez (Ctrl+O, Entrée, Ctrl+X)
chmod 600 ~/.kaggle/access_token
```

Sous Windows sans WSL, le dossier est `C:\Users\<vous>\.kaggle\`.

**Étape 3 — vérifier**

```bash
uv run kaggle competitions submissions -c gemma-4-developer-agent
```

Ce que vous devez voir : la liste de vos soumissions, avec leur statut et leur score.

| Erreur | Cause | Solution |
|---|---|---|
| `401 Unauthorized` | Token absent ou mal copié | Refaire l'étape 2 |
| `403 Forbidden` | Règles de la compétition non acceptées | *Join Competition* sur le site |
| `command not found: kaggle` | Lancé sans `uv run` | Préfixer par `uv run` |

**Commandes utiles**

```bash
uv run kaggle competitions leaderboard -c gemma-4-developer-agent --show   # le classement
uv run kaggle competitions files -c gemma-4-developer-agent                # les fichiers (200 par page)
uv run kaggle competitions download -c gemma-4-developer-agent -f <chemin>  # un seul fichier, pas tout (22 Go)
uv run kaggle kernels status <votre-pseudo>/<votre-notebook>               # où en est votre notebook
uv run kaggle kernels output <votre-pseudo>/<votre-notebook> -p runs/      # récupérer ses sorties
```

**Questions à rendre (dans `outils.md`)**

1. Combien de soumissions votre binôme a-t-il faites ? Combien ont échoué, et pourquoi (d'après le statut) ?
2. Combien de tâches publiques y a-t-il dans `tasks.jsonl` ? Lesquelles touchent le plus de fichiers ?
3. Quel est le statut de votre dernier notebook ? Combien de temps a-t-il tourné ?
4. → **Une hypothèse pour le plan.**

### 1b — Un assistant de code dans le terminal

Le chat du navigateur répond à vos questions. Un assistant en ligne de commande **lit vos fichiers et
lance des commandes** : il peut compter, comparer, écrire un script pour vous.

Choisissez celui que vous voulez :

| Outil | Installation | Compte |
|---|---|---|
| Gemini CLI | `npm install -g @google/gemini-cli` puis `gemini` | Google (gratuit) |
| Claude Code | `npm install -g @anthropic-ai/claude-code` puis `claude` | Anthropic |
| Codex CLI | `npm install -g @openai/codex` puis `codex` | OpenAI |
| Copilot | extension VS Code ou `gh copilot` | GitHub |

**Trois règles**

1. Lancez-le dans un dossier **sans secret** (pas de `.env`, pas de token).
2. Demandez un **script, un calcul ou une citation du fichier**, pas une opinion.
3. **Relisez avant d'accepter** une modification (règle 4 de la charte du M3).

**Exemple**

```bash
cd ~/p1-outils
uv run kaggle competitions download -c gemma-4-developer-agent -f tasks.jsonl
gemini      # ou claude, codex…
```

Puis, dans l'assistant :

> Écris un script Python `patch_stats.py` qui lit tasks.jsonl et affiche, pour les patchs de référence,
> le nombre de fichiers modifiés et de lignes changées : moyenne, médiane, maximum. Lance-le.

Relisez le script, puis notez le résultat. Si 78 % des patchs touchent un seul fichier
(chiffre inventé), une hypothèse possible : « dire dans le prompt de chercher d'abord *le* bon fichier ».

**Questions à rendre (dans `outils.md`)**

1. Taille typique d'un patch de référence (fichiers, lignes) ?
2. Le pire cas : combien de temps faudrait-il si chaque tâche prenait votre budget maximum ? Ça tient dans 12 h ?
3. Parmi les pièges de la section 10 du `HARNESS_README.md`, lesquels votre prompt actuel ne couvre pas ?
4. Quelle différence entre votre configuration et celle du meilleur notebook public ?
5. → **Une hypothèse pour le plan.**

Chaque binôme de son côté pendant l'exercice 1 : **aucun échange de code avec l'autre équipe**.

---

## Ce qui change à partir de maintenant

**Les deux binômes de votre groupe fusionnent en une seule équipe Kaggle.** Un seul journal, un seul classement.

Et chacun reste noté individuellement :

- chacun **signe au moins 2 lignes du plan** et **mène au moins 1 expérience** ;
- à la keynote du 27 novembre, chacun **présente son expérience** (1 min 30) et **répond à une question** individuelle,
  qui peut porter sur l'expérience d'un coéquipier ;
- le journal compte à partir d'aujourd'hui : 5 expériences menées par l'équipe, numérotées à partir de **E01**.
  Vos anciens essais restent comme point de départ (préfixez-les `A-` et `B-`).

**Le journal (6 points)**

| Points | Critère |
|---|---|
| 2 | Au moins 5 expériences d'équipe à partir du 30 octobre (E01, E02…) |
| 2 | Une hypothèse = un seul changement, et la prédiction est **commitée avant** la soumission (horodatage git) |
| 2 | Des conclusions qui s'enchaînent, dont au moins un échec ou une trace analysés |

**Tant que la fusion n'est pas faite sur Kaggle, pas d'échange de code** : vous êtes encore deux équipes.

---

## Exercice 2 — Fusion (10 min)

1. Sur Kaggle, onglet *Team* : une équipe envoie une demande de fusion (*Merge*), l'autre l'accepte.
   Nom de l'équipe : `EPI-G<n>`.
2. Le total des soumissions des deux équipes doit rester sous la limite Kaggle : si la fusion est refusée, appelez-moi.
3. Créez le repo GitHub **privé** `p1-journal-G<n>`, invitez les membres et l'enseignant.
4. Copiez-y les deux `journal.md` (renommés en `A-…` et `B-…`) et les deux `outils.md`.

## Exercice 3 — Mise en commun (15 min)

Chaque binôme montre ce qu'il a essayé et ses deux hypothèses de l'exercice 1.

Livrable : un tableau dans `journal.md`.

| Essai | Équipe | Changement | Score | Ce qu'on en retient |
|---|---|---|---|---|

Puis choisissez **la configuration de départ** de l'équipe (la meilleure, ou la plus propre) et écrivez pourquoi.

## Exercice 4 — Plan d'expériences (25 min)

Fichier `plan.md` : **au moins 8 lignes**, **au moins 2 signées par chacun**, classées.

| # | Hypothèse | Changement (un seul) | Résultat attendu | Responsable | Pourquoi à ce rang |
|---|---|---|---|---|---|

Une ligne = une hypothèse = un seul changement = un nom. L'ordre se justifie : gain attendu, coût, risque.

Rappel du rythme : une soumission en attente à la fois, 1 à 2 jours d'évaluation, donc une quinzaine
d'essais d'ici le 2 décembre. Pas de soumission « pour voir ».

## Exercice 5 — Lancer (5 min)

1. Écrivez l'entrée **E01** dans `journal.md` **et commitez-la avant de soumettre** :

```markdown
## E01 — 30 octobre — menée par : Prénom N.

**Hypothèse** :
**Changement** : (un seul)
**Résultat attendu** : (votre prédiction, chiffrée si possible)
**Résultat** : (à remplir quand le score arrive)
**Conclusion** :
```

2. Soumettez (procédure : [`SOUMETTRE.md`](SOUMETTRE.md)).
3. Si une soumission est déjà en attente : préparez la suivante, prête à partir dès que le score arrive.

---

## 11h45 — Tour des groupes

Chaque équipe annonce en une phrase sa première hypothèse et qui la porte.

## Prochains rendez-vous

| Date | Attendu |
|---|---|
| 13 nov. | Follow-up : journal à jour, `plan.md`, **3 traces lues** et commentées |
| 27 nov. | Keynote, 15 min par équipe |
| 2 déc. | Rendu final Kaggle |

### D'ici le 13 novembre : lire une trace

Le score dit **combien**, la trace dit **pourquoi**. Pour chaque tâche ratée que vous lisez, répondez à 4 questions :

1. L'agent a-t-il ouvert **le bon fichier** ?
2. A-t-il eu **assez de temps et d'appels** d'outils ?
3. A-t-il **vu le bug échouer**, puis corrigé, puis vérifié ?
4. **Qu'attendait le test** qui a échoué ?

Utilisez un notebook « lab » (GPU, quelques tâches publiques, jamais soumis) pour produire des traces,
et un notebook « submit » (sans accélérateur) pour soumettre.
