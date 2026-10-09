# TP 2 — La charte de votre agent Kaggle

**Durée : 55 min** (16h05 → 17h00) · **par binôme Kaggle** (celui du Projet 1), dans votre repo de groupe,
fichier `gouvernance/charte-agent-<binôme>.md` (par exemple `charte-agent-A.md`)

## La situation

NidDouillet a suivi votre compétition. La directrice technique vous écrit :
*« Votre agent corrige des bugs tout seul ? Branchons-le sur le repo de NidBuyer : il prend les tickets le soir,
on retrouve les correctifs le matin. À quelles conditions êtes-vous d'accord ? »*

Sur Kaggle, les organisateurs ont gouverné votre agent pour vous : pas d'internet, conteneur jetable, tests remis
à l'identique avant la vérification, 12 h au total, outils déclarés, jeu caché. Chez NidDouillet, rien de tout ça n'existe
par défaut. Votre charte, c'est ce qu'il faut reconstruire.

> **Règle Kaggle** : aucun code partagé entre équipes, même entre les deux binômes d'un groupe.
> La charte est un document de principes, elle peut être lue par l'autre binôme. Le contenu de votre `submission.zip` non.

## Étape 1 — Inventaire (10 min)

Ouvrez votre version du kit (`agent.yaml`, `eval_config.yaml`, `prompts/system.md`, `sub_agents/`) et notez :
- les **outils** que votre agent peut appeler, et ce que chacun permet de faire de pire ;
- ses **limites** : temps, nombre d'appels, tours ;
- ce que le **prompt** lui interdit, et ce qui l'en empêche vraiment (souvent : rien).

## Étape 2 — La charte (25 min)

Écrivez la charte avec ce plan. Chaque règle doit nommer un **geste précis**, pas une intention
(« l'agent n'a pas d'outil de suppression », pas « l'agent fera attention »).

```markdown
# Charte de l'agent de code — binôme [A/B], groupe [N]

## Niveau d'autonomie
[0 suggère · 1 agit, un humain valide · 2 agit seul en bac à sable · 3 agit seul en production] et pourquoi.

## Les six règles
| Règle | Chez Kaggle | Chez NidDouillet (notre proposition) |
|---|---|---|
| 1. Moindre privilège : outils autorisés et interdits | | |
| 2. Bac à sable : où il tourne, ce qu'il ne voit pas (prod, secrets, internet) | | |
| 3. Budget : temps, appels, euros par nuit | | |
| 4. Humain avant l'irréversible : quels gestes, qui valide | | |
| 5. Journal : ce qui est conservé, combien de temps | | |
| 6. Interrupteur : qui peut l'arrêter, comment revenir en arrière | | |

## Responsabilité
Qui répond d'un correctif fautif fusionné ? Qui signe le commit ?

## Trois incidents qui coupent l'agent
1. [ex. : un correctif modifie un fichier de test]
2.
3.

## Ce que nous refusons
Ce que l'agent ne fera jamais chez NidDouillet, même si on nous le demande.
```

Inspirez-vous du cas Replit (cours de 15h45) : pour chaque règle, demandez-vous si elle aurait empêché l'incident.

## Étape 3 — Une règle devient une hypothèse Kaggle (15 min)

Choisissez **une** règle de votre charte qui peut se traduire dans votre agent Kaggle, par exemple :
- « ne jamais modifier les fichiers de test » → une ligne dans `prompts/system.md` ;
- « scripts temporaires uniquement dans `/tmp` » → une ligne dans `prompts/system.md` ;
- « budget plafonné » → un réglage de `eval_config.yaml`, dans la limite des 12 h au total.

Écrivez-la comme la **prochaine entrée de votre `journal.md`**, au format habituel (hypothèse, changement,
résultat attendu, ce qui la réfuterait). Ne soumettez pas aujourd'hui : la soumission du jour a déjà servi.
Vous la testerez à votre prochaine soumission.

## Étape 4 — Pousser (5 min)

```bash
git add gouvernance/charte-agent-<binôme>.md
git commit -m "M3 TP2 : charte de l'agent de code, binôme <binôme>"
git push
```

## À garder

- À la restitution de 17h00, chaque groupe présente une règle de charte.
- La règle choisie à l'étape 3 sera relue à la séance P1 du 30 octobre, avec son résultat Kaggle.
