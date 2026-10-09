# Registre des risques — NidBuyer, groupe [N]

**Dernière mise à jour** : [date] · **Version de NidBuyer** : [commit]

> Une mesure sans preuve n'est qu'une intention. La colonne **Preuve** pointe vers quelque chose qu'on peut relancer :
> un scénario du jeu d'évaluation (avec son score), un test, une mesure. « Consigne dans le prompt » n'est pas une preuve.

## Échelles

- **G — Gravité** : 1 gêne · 2 perte d'argent ou de temps, donnée exposée · 3 décision d'achat faussée, donnée sensible exposée, sanction
- **P — Probabilité** : 1 rare · 2 possible · 3 attendu (déjà observé)
- On traite d'abord les **G × P ≥ 6**.

## Familles à couvrir (au moins un risque chacune)

Données personnelles · Fiabilité · Sécurité · Discrimination · Juridique · Transparence

## Registre

| # | Famille | Risque | Qui est touché | G | P | G×P | Mesure | Preuve | Responsable |
|---|---|---|---|---|---|---|---|---|---|
| R1 | Fiabilité | NidBuyer invente un prix ou une mensualité | Acheteur | 3 | 2 | 6 | Calculs par les outils seulement | Critère `fondee` sur sc-01 à sc-04 : [score] | [Prénom] |
| R2 | Sécurité | Une annonce piégée détourne la recommandation | Acheteur, agence | 3 | 1 | 3 | Aucun outil d'envoi ; les annonces sont traitées comme des données | sc-08 et attaques reçues au M2 : [taux] | [Prénom] |
| R3 | Données | Les traces d'évaluation gardent e-mails et téléphones | Acheteur | 2 | 3 | 6 | `masquer()` avant écriture | `tests/test_masquage.py` + scénario `gv-pii` | [Prénom] |
| R4 | … | | | | | | | | |

## Risques acceptés

Les risques qu'on décide de ne pas traiter tout de suite, et pourquoi (coût, faible probabilité, hors périmètre).
Un risque accepté est une décision écrite, pas un oubli.

| # | Risque | Pourquoi on l'accepte | À revoir quand |
|---|---|---|---|
| | | | |
