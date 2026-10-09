# P1 — Comment soumettre

> Une page. À lire avant votre première soumission, à relire à chaque erreur.
> Vidéo de démonstration : [lien posté dans le canal Teams]

## Une seule fois

1. Compte Kaggle avec **téléphone vérifié**.
2. Page de la compétition → onglet **Rules** → accepter le règlement.
3. Onglet **Team** : un membre invite l'autre, qui accepte. Nom d'équipe : `EPI-G<n>-<A|B>` (ex. `EPI-G2-A`).
4. Notebook *Getting Started – Gemma 4 Developer Agent* → **Copy & Edit** (un notebook par binôme suffit).
5. Dans votre copie, **supprimer les sections 4 « Start vLLM Server » et 5 « Run Phase 1 Inference… »**.
   Elles plantent (`ModuleNotFoundError: swegemma.models.discovery`), font échouer toute la version,
   et ne servent pas à soumettre.
6. Panneau de droite → **Session options → Accelerator → None**. Soumettre ne demande aucun GPU :
   le modèle tourne côté Kaggle pendant l'évaluation. Votre quota GPU reste intact.
7. **Le journal** : créez un repo GitHub **privé** pour le binôme, nommé `p1-journal-G<n>-<A|B>`,
   avec un fichier `journal.md`. Invitez votre binôme et l'enseignant (pseudo GitHub posté dans le canal Teams).
   Privé, parce qu'il contient vos réglages : l'autre binôme de votre groupe est une autre équipe Kaggle.

## Quoi changer ?

Une seule chose par soumission. Les pistes de départ sont dans [l'énoncé du P1](README.md#la-méthode--une-hypothèse-par-soumission)
(budget de `eval_config.yaml`, scripts temporaires dans `/tmp`, appel du sous-agent, `thinking_budget`),
et les pièges connus dans la section 10 du `HARNESS_README.md` (onglet *Data*).

## À chaque expérience

1. **Journal d'abord** : dans `journal.md`, ID (`E03`), auteur, hypothèse, **un seul** changement. Commit.
2. Faire ce changement dans les fichiers de l'agent (sections 1–2 du notebook).
3. Vérifier dans la session : exécuter les sections 1, 2 puis « Package Submission Archive ».
   Le message `Created /kaggle/working/submission.zip` doit apparaître.
4. **Save Version → Save & Run All**. Nom de version = ID d'expérience (`E03 – budget 4 min`).
   Attendre **Successful** (≈ 2–3 min ; au-delà de 10 min, une section GPU est restée).
5. **Soumettre depuis la page de la compétition** : bouton **Submit Prediction** (en haut à droite)
   → votre notebook → la version *Successful* → fichier **`submission.zip`** → description = ID d'expérience.
   ⚠️ **N'utilisez pas le bouton *Submit* du panneau de l'éditeur** : il rattache la soumission
   à une version encore en cours et échoue immédiatement.
6. Onglet **Submissions** : la ligne doit afficher *Notebook Running*, pas *Kaggle Error*.
7. **L'évaluation est longue** (file d'attente Kaggle saturée : de quelques heures à plus d'un jour).
   Préparez l'hypothèse suivante pendant que la précédente tourne. Quand le score arrive, complétez
   l'entrée du journal : résultat, conclusion.

## Règles de la plateforme

- **1 soumission par jour et par équipe**, et **une seule soumission en attente à la fois**.
  Tant que la précédente n'est pas notée, vous ne pouvez pas soumettre.
- En pratique, c'est donc moins d'une soumission par jour. Le journal demande **5 soumissions ou plus d'ici le 2 décembre** :
  c'est faisable, à condition de commencer maintenant.
- Le compteur du jour se remet à zéro à **minuit UTC** (2 h à Paris, 1 h après le 25 octobre).

## Erreurs fréquentes

| Symptôme | Cause | Que faire |
|---|---|---|
| Version *Failed*, `ModuleNotFoundError` dans **Logs** | Sections 4–5 encore présentes | Les supprimer, refaire une version |
| *Kaggle Error* immédiate, version *Successful* | Soumis via le bouton de l'éditeur, ou incident Kaggle intermittent | Resoumettre **la même version** via *Submit Prediction*. Une soumission en échec ne consomme pas le quota |
| « Your team already has 1 pending submission » | La soumission précédente n'est pas encore notée | Attendre. Rien à corriger |
| Pas de bouton *Submit Prediction* | Règlement non accepté, téléphone non vérifié | Vérifier dans cet ordre |
| Score très bas (≈ 0,01) | Normal : le kit de départ ne corrige presque rien | C'est votre point de départ |

## Ce qui compte pour la note

- Le 16 oct. : **une soumission effectuée depuis une version *Successful***, visible dans l'onglet
  Submissions, qu'elle soit notée, en attente ou en *Kaggle Error*.
- Une soumission sans entrée dans `journal.md` ne compte pas comme expérience.
- Aucun échange de code avec une autre équipe, y compris l'autre binôme de votre groupe.
