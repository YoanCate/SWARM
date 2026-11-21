# Direction artistique et assets — SWARM

Réécrit à partir du cahier des charges (le document d'origine n'était pas sur la machine).

## Cible

Science-fiction usée et crédible. Lumière naturelle forte et douce, grands paysages ouverts, atmosphère épaisse.
Matières réelles : béton, tôle rouillée, toile, bois, plastique vieilli. Palette désaturée, accents de couleur francs.
Silhouettes lisibles, équipements bricolés. On s'inspire d'une ambiance ; on ne copie aucun design, modèle, nom ou
logo d'un jeu existant.

Écart assumé : ce niveau est celui d'un grand studio avec des artistes. Avec des assets gratuits et du procédural,
la lumière, l'atmosphère et les environnements peuvent s'en approcher ; les personnages, les créatures et
l'animation seront en dessous. L'effort va d'abord à la lumière, à l'atmosphère, au terrain et aux matériaux.

## Règle n°1 : chaque variation visuelle montre un état réel de la simulation

| Ce qu'on voit | État de la simulation qu'il montre |
|---|---|
| Mucus qui s'étale sur le sol | `Cell.creep` (0..1), qui suit la prise de la case par l'Essaim |
| Mucus qui pâlit, vire au gris | `Cell.biomass / Cell.biomassCap` : la case s'épuise |
| Taille et hérissement d'une ruche | `Hive.units` (garnison), `Hive.hp / maxHp` |
| Arche qui grandit | `Ark.progress` (quille, membrures, coque, flèche) |
| Lueur magenta de l'Essaim | `SwarmState.queenAlive` ; elle s'éteint quand l'Essaim devient féral |
| Cicatrices et excroissances d'un champion | `Nemesis.scars`, `Nemesis.traits` (carapace épaissie, vif, fouisseur) |
| Remparts d'une cité | `Settlement.fort` |
| Taille d'une cité, feux, tentes | `Settlement.population`, `Settlement.garrison` |
| Ruines | `Settlement.alive == false`, `razedBy` |
| Couleurs et bannières | `Settlement.faction` |

Jamais de décoration qui ne corresponde à rien.

## L'Essaim

Organique. Chitine sombre et humide. **Un seul accent : magenta bioluminescent, qui n'apparaît nulle part ailleurs
dans le monde** (ni interface des factions, ni fleurs, ni éclairage). Tout est généré par code : mucus, ruches,
Arche, créatures.

Castes : ouvrière (petite, basse, porteuse), essaimeur (rapide, nombreux), lame (haute, membres tranchants),
cuirassé (massif, lent, plaques), garde royale (grande, hérissée, proche de la Reine), champion (individu nommé,
dérivé d'une caste, marqué par son histoire), Reine (immobile, dans sa ruche).

## Les factions

| Faction | Couleur | Matières | Silhouette des cités |
|---|---|---|---|
| Concordat d'Aram | or terni | pierre claire, toile écrue, cuivre | dômes, silos à semences, cloîtres |
| Ligue des Fonderies | rouille orangée | tôle, brique noire, fonte | cheminées, hauts-fourneaux, portiques |
| Clans du Vent | turquoise délavé | toile, cuir, bois léger | tentes, mâts, chariots, rien de fixe |
| Protectorat de Hael | bleu acier | béton, acier peint, barbelés | bastions, miradors, casernes alignées |
| Les Affranchis | rouge franc | récupération, bois, plastique vieilli | barricades, ateliers, toits de fortune |

## Rendu

- Éclairage global dynamique : Lumen, ray tracing matériel si disponible. Ombres virtuelles.
- Ciel et nuages volumétriques, cycle jour/nuit, brume de hauteur, brouillard volumétrique.
- Géométrie dense : Nanite pour sols, roches, falaises ; végétation abondante.
- Matériaux PBR avec usure, saleté, humidité, décalcomanies.
- Étalonnage filmique, exposition automatique bornée, profondeur de champ et flou de mouvement légers.
- Vent sur la végétation et les tissus ; particules d'ambiance (poussière, pollen, spores près de l'Essaim).

## Assets externes

Uniquement du gratuit et du légal (Megascans, Megaplants, contenu gratuit d'Epic sur Fab, MetaHuman, kits gratuits).
Licence vérifiée avant usage, consignée dans `Docs/CREDITS.md`. Le jeu doit rester jouable sans ces packs :
tout a une forme provisoire générée par code.

La liste des packs à télécharger sera donnée en une fois, au jalon où elle devient utile.
