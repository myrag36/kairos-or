# Consignes de mise à jour de l'analyse (pour Claude)

Ce fichier décrit comment mettre à jour `data/analysis.json`, lu par le site toutes les 5 minutes.

## Étapes
1. Chercher l'actualité de l'or (XAU/USD) des dernières heures : prix spot actuel, Fed et probabilités de hausse/baisse, chiffres US publiés, dollar, rendement 10 ans, pétrole et géopolitique, flux ETF et banques centrales, et les niveaux techniques publiés par plusieurs analystes (supports, résistances, RSI, moyennes mobiles). Recouper au moins deux sources récentes.
2. Relire `data/analysis.json` actuel. Pour chaque plan, vérifier avec le prix actuel s'il est toujours valable :
   - garder un plan valable tel quel (mêmes chiffres), en ajustant au plus le texte ;
   - remplacer un plan invalidé (niveau `invalidAbove`/`invalidBelow` franchi, stop touché, objectif atteint, ou contexte changé) par un nouveau plan cohérent avec le prix actuel ;
   - toujours 2 à 4 plans, dont au moins un dans le sens du biais.
3. Mettre à jour : `updatedAt` (ISO UTC, heure réelle), `price`, `change` (% du jour), `priceTime` (ex. « 30 oct. · 15h00 Paris »), `bias` (sell|buy|neutral), `biasLabel`, `summary` (4–5 phrases simples en français), `setups`, `levels`, `factors` (impact de -2 à +2), `noTrade`, `calendar` (événements à venir, heure UTC dans `at`, `result` pour ceux publiés), `sources` (titre + URL des articles utilisés).
4. Chaque plan : `side`, `name`, `status` (active|watch), `zone` [bas, haut], `trigger` (signal de confirmation à attendre), `stop`, `tps` [obj1, obj2], `invalidAbove` ou `invalidBelow` (niveau qui annule le plan avant l'entrée), `note`. Ratio gain/risque d'au moins 1,5 sur l'objectif 2. Stops au-delà d'un niveau technique, jamais au milieu de nulle part.
5. Vérifier que le JSON est valide (`python3 -m json.tool data/analysis.json`), puis commit sur `main` avec un message court (« Analyse or 30/10 15h ») et push.

## Règles
- Tout en français, phrases courtes, sans jargon inutile.
- Ne jamais inventer un chiffre ou une nouvelle : si une info n'est pas trouvée, ne pas l'écrire.
- Si une annonce majeure (NFP, CPI, Fed) tombe dans l'heure, le dire dans `summary` et mettre les plans en `watch`.
- Ce n'est pas un conseil en investissement : rester prudent, toujours un stop.
