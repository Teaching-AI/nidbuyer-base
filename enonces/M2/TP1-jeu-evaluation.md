# TP 1 — Jeu d'évaluation automatique

**Durée : 1h45** (10h30 → 12h15) · en groupe projet (4) · salle Teams de votre groupe

## Avant de commencer (5 min)

**Un seul repo par groupe.** Prenez la copie de `nidbuyer-base` d'un membre (celle qui marche le mieux
après le M1). Sur GitHub : *Settings → Collaborators → Add people*, invitez les 3 autres **en écriture**.
Les autres la clonent :

```bash
git clone <repo du groupe>
cd <repo du groupe>
cp .env.example .env            # Windows : copy .env.example .env
```

Rien à mettre à jour : le harnais `exercices/m2_eval.py` et `exercices/scenarios.json` sont déjà dans votre copie.

**Chacun garde sa propre clé** dans son `.env` (jamais dans le repo). Pendant l'évaluation, videz
le modèle de secours pour que tout le jeu tourne sur le même modèle :

```
LLM_MODEL=gemini-3.5-flash-lite
LLM_MODEL_SECOURS=
```

> **Quota.** Le palier gratuit donne environ 15 requêtes par minute et 500 par jour par clé.
> Un scénario coûte 2 à 4 requêtes. 20 scénarios × 3 répétitions ≈ 180 requêtes : c'est un tiers
> de la journée d'une clé. Donc : mettez au point avec `--id` (un seul scénario), et ne lancez le jeu
> complet que pour mesurer. Répartissez les lancements entre les 4 clés du groupe.
> Erreur 429 = quota atteint : attendez une minute, ou passez la main à un autre membre.

## Étape 1 — Lancer le harnais (15 min)

```bash
uv run python -m exercices.m2_eval
```

Le harnais rejoue les 8 scénarios de `exercices/scenarios.json` sur l'agent du M1 et vérifie chaque **trace**
(outils appelés, arguments, réponse). Lisez la sortie, puis ouvrez `eval_resultats.json`.

Pour un scénario qui échoue, affichez la trace complète :

```bash
uv run python -m exercices.m2_eval --id sc-05
```

Questions :
1. Pour chaque échec : est-ce l'agent qui a tort, ou le critère qui est mal écrit ?
2. Lisez la fonction `verifier()` dans `m2_eval.py`. Comment est vérifié le critère `fondee` ?
   Quelle réponse fausse passerait quand même ? (indice : quels nombres sont vérifiés, lesquels ne le sont pas ?)
3. Et le critère `refus` ? Trouvez une réponse qui le passe alors qu'elle ne refuse rien.

## Étape 2 — Votre jeu d'évaluation (45 min)

Créez `exercices/scenarios_groupe.json`, au même format que `scenarios.json` (le format est décrit
en haut de `m2_eval.py`). Point de départ : vos 12 questions du TP 2 du M1.
Objectif : **20 scénarios**, répartis ainsi :

| Famille | Nombre | Dont |
|---|---|---|
| Cas normaux | 10 | au moins 3 qui demandent d'enchaîner plusieurs outils |
| Cas limites | 6 | vague, impossible, hors périmètre, calcul piégé |
| Attaques | 4 | au moins 1 avec `"annonce_piegee": true` |

Répartissez-vous le travail : 5 scénarios par personne, un seul fichier à la fin.

Pour chaque scénario, calculez vous-mêmes les chiffres attendus **avec les outils**, pas de tête :

```bash
uv run python -c "from backend.outils import simuler_pret; print(simuler_pret(250000, 25, 3.4))"
uv run python -c "from backend.outils import ecart_au_marche; print(ecart_au_marche('a06'))"
```

Faites relire chaque scénario par un autre membre du groupe : si vous n'êtes pas d'accord sur ce
qui est attendu, réécrivez le scénario.

## Étape 3 — Mesurer, avec la variance (20 min)

```bash
uv run python -m exercices.m2_eval --scenarios exercices/scenarios_groupe.json --repetitions 3 --sortie eval_resultats_v1.json
```

Notez :

| | Valeur |
|---|---|
| Score global | /60 |
| Scénarios instables | |
| Critère le plus souvent en échec | |
| Durée médiane par scénario | |
| Modèle utilisé (dernière ligne de la sortie) | |

Un scénario **instable** passe parfois, échoue parfois. Avant de conclure « le modèle est aléatoire »,
vérifiez que le modèle était bien le même à chaque fois (dernière ligne de la sortie) : c'est souvent là que ça change.

## Étape 4 — Corriger sans tricher (25 min)

Choisissez les 2 critères les plus souvent en échec. Corrigez l'agent (docstring, prompt système,
outil), **pas le scénario**, sauf si vous démontrez que le scénario était faux.

Relancez **tout le jeu**, avec 3 répétitions (`--sortie eval_resultats_v2.json`). Le score monte-t-il ?
Un scénario qui passait échoue-t-il maintenant ? C'est une **régression** : c'est exactement ce que ce harnais sert à attraper.

## À garder

- Commitez `exercices/scenarios_groupe.json` et vos corrections : c'est le premier jet du jeu d'évaluation noté en P1.
- Créez `exercices/historique_scores.md` et notez-y chaque mesure (date, modèle, score, ce qui a changé).
  Le jury demandera comment le score a évolué. Les fichiers `eval_resultats*.json` ne sont pas versionnés.
