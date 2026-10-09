# TP 0 — La carte des données de NidBuyer

**Durée : 45 min · en autonomie, le matin** · par groupe de 4, dans votre repo de groupe du M2 (`nidbuyer-groupe<N>`)

Avant de parler de RGPD, il faut savoir **où passent les données**. Vous allez suivre les données d'un acheteur
dans le code de NidBuyer, de son clavier jusqu'au dernier endroit où elles sont écrites.

## La situation

Une acheteuse, Mme Martin, utilise NidBuyer. Elle :
1. pose une question au chat, en remplissant son profil : budget, quartiers, et le champ libre
   « Parlez-nous de votre projet », où elle écrit : *« Je viens de divorcer, j'ai deux enfants, et mon fils est en fauteuil »* ;
2. s'inscrit aux alertes avec son e-mail ;
3. pendant ce temps, l'équipe relance le harnais d'évaluation du M2 sur des questions réelles copiées depuis le chat.

## Étape 1 — Suivre les données (25 min)

Ouvrez le code et, pour **chaque endroit** où une donnée de Mme Martin (ou d'un vendeur) passe ou est écrite, notez :

| Donnée | Entre par | Passe par | Écrite où ? | Combien de temps ? | Qui d'autre la voit ? |
|---|---|---|---|---|---|
| Texte libre du profil | `POST /chat` (`backend/main.py`) | … | … | … | … |

Fichiers à ouvrir, au minimum :
- `backend/main.py` : les modèles `ProfilAcheteur`, `AlerteProfil`, `Question`, et les routes ;
- `backend/llm.py` : où part le texte, ce que garde `JOURNAL` ;
- `backend/alert.py` : où et comment sont stockés les profils d'alerte ;
- `exercices/m2_eval.py` : ce qui est écrit dans `eval_resultats.json` ;
- `backend/sources/` et `backend/ingestion.py` : ce que contiennent les annonces collectées ;
- `backend/rag.py` : ce qui est mis dans la base vectorielle.

Cherchez au moins **cinq endroits**. Les TODO comptent : un TODO est une décision qui n'a pas encore été prise.

## Étape 2 — Trois questions (15 min)

Sous le tableau, répondez en deux ou trois lignes chacune :
1. Laquelle de ces données est la plus **sensible** ? Pourquoi ?
2. Laquelle est la plus **inutile** pour conseiller Mme Martin ?
3. Si Mme Martin écrit à NidDouillet « effacez tout ce que vous avez sur moi », dans combien d'endroits faut-il aller ?
   Y en a-t-il un où vous ne sauriez pas le faire ?

## Étape 3 — Pousser (5 min)

Créez `gouvernance/carte-donnees.md` dans votre repo de groupe, poussez-le.

```bash
mkdir -p gouvernance
# écrire gouvernance/carte-donnees.md
git add gouvernance/carte-donnees.md
git commit -m "M3 TP0 : carte des données"
git push
```

**Pas de vraie donnée personnelle dans le repo** : Mme Martin est fictive, gardez-la ainsi.

À 13h30, deux groupes présenteront leur carte en 2 minutes.
