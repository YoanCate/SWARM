# Sociétés et guerres

État au 19/11/2026. Règles : `Source/Swarm/Sim/SwarmSimSociety.cpp` (table `kLaws`). Tout ce qui est écrit ici est
vrai dans la simulation ; rien n'y est créé : un captif est quelqu'un sorti d'une armée ou d'une population, il mange
au grenier de qui le tient et n'en sort que racheté, vendu, évadé, libéré ou mort.

## Les guerres

- **Une cause** que le crieur dit : mine disputée sur la frontière, pâtures closes (Clans), faim (greniers vides
  contre greniers pleins), les siens aux fers chez l'autre, ou la vieille rancune du lore (Fonderies/Aram : la mine ;
  Clans/Protectorat : les pâtures ; Fonderies/Affranchis : les chaînes).
- **Des campagnes** : levée de conscrits sur la population (payés en minerai et vivres comme toute recrue), marche sur
  la cité ennemie la plus proche ; le défenseur sort à sa rencontre (bataille rangée) ; siège ; une garnison écrasée
  et affamée se rend (elle part en captivité) ; une cité prise est mise à sac (vivres, captifs, et la cour martiale
  pend les meneurs). La guerre d'ouverture a ses deux armées face à face dès le premier jour.
- **Une fin** : le battu demande la paix ; un vainqueur net impose un tribut (un vingtième du grenier tous les dix
  jours, quatre mois) ; quand l'Essaim pèse, trêve puis coalition, et la **grande armée** : trois peuples ou plus
  marchent ensemble sur la même ruche.

## Les cinq lois

| Peuple | Loi | Captifs | Ce que prend le joueur arrêté (1re fois / récidive) |
|---|---|---|---|
| Concordat d'Aram | Tribunal du sanctuaire | peu (1 sur 5) ; aux champs comme pénitents ; rendus à la paix ; rachetables | pénitence (2 jours de travail, estime rendue) / geôle |
| Ligue des Fonderies | Code des dettes | achète des captifs ; tous aux fers dans ses mines ; rachetables | amende / les fers à la mine (dette) |
| Clans du Vent | Loi de la razzia | prend le plus (2 sur 5) ; razzias ; vend aux marchés de la Ligue et du Protectorat ; rachat même en guerre ; pas de prison | pilori / vendu à la Ligue |
| Protectorat de Hael | Cour martiale | forçats aux chantiers ; achète ; ne rend personne ; pend les évadés repris et les meneurs | geôle / les fers |
| Les Affranchis | Justice des faubourgs | aucun esclave : prisonniers de guerre rendus à la paix ; libèrent les captifs des cités qu'ils prennent ; attaquent les convois ; accueillent les évadés | amende / bannissement |

Lever la main sur la garde ne se règle pas par l'amende : la geôle ou les fers (le bannissement chez les Affranchis).

**Convois** (corrigé le 19/11/2026 : il n'en partait jamais). Tous les dix jours, une cité des Clans qui tient au moins six
captifs d'un même peuple en envoie un lot (vingt au plus) au marché le plus proche qui peut payer — Ligue ou Protectorat,
en paix avec les Clans, à moins de 31 cases —, sous une garde prise sur sa garnison ; les siens rachètent ce qui reste.
L'acheteur paie à l'arrivée, en vivres, et ce qui manque en minerai au cours des échanges. Les Affranchis attaquent les
convois qui passent près d'eux. Mesuré (graines 1 à 6, 600 jours) : 3,2 convois par partie (0 avant) ; équilibre sur 100
graines : Arche 82 %, Essaim détruit 18 %, monde mort 0 %, durée médiane 723 jours (avant : 83 / 17 / 0, 723).

**À l'image** (`Game/SwarmSociety.cpp`, `SwarmSocietyWar.cpp`) : sur la place de chaque cité, la cage et ses détenus (le
jour, ceux qui ne sont pas aux chantiers), le pilori, un corps par pendu sous la poutre, l'estrade du marché aux captifs
là où la loi vend ou achète, la table du recruteur ; les captifs aux fers en haillons sur les chantiers de la cité ; les
convois en file enchaînée sur les routes ; le camp d'une armée qui assiège. Touche E : geôlier, marchand, recruteur.

## Le joueur

- **Pris** (il se rend, ou des humains l'ont vaincu) : jugé selon la loi du lieu. Il peut payer (les rations entrent
  au grenier), plaider une fois (estime, appartenance ; le sanctuaire écoute, la cour martiale non), purger (les jours
  passent ; aux fers son travail sort vraiment du minerai), s'évader (crochetage, garde endormi, complice — les
  Affranchis), se soulever avec les autres enchaînés.
- **Captifs** : racheter un captif (quatre rations ; il rentre chez lui, ou suit le joueur) ; tuer la garde d'un convoi
  et le délivrer (estime des Affranchis et du peuple des captifs, haine du vendeur).
- **Campagne** : s'engager dans l'armée d'un peuple en guerre, toucher sa solde et sa part de butin au retour (prises au
  grenier), ou déserter (la cité le cherche).

## Cercles hors des peuples

Règles : `Source/Swarm/Sim/SwarmSimCircles.cpp` ; dans le monde : `Source/Swarm/Game/SwarmCircles.cpp`. Camp
`kCircleBase + i` (100 à 107) ; chef = un nemesis de rôle `Outsider`. Rien n'est créé : les membres sortent d'une cité
(population, ou garnison pour les déserteurs), mangent ce qu'ils cueillent autour du repaire (la biomasse en sort) ou
ce qu'on leur paie, meurent, sont pris (`Hold::otherCaptives`). Ils ne tiennent ni cité ni territoire. Un chef mort
est remplacé par un membre ; un individu mort l'est pour toujours, et le monde l'apprend.

| Cercle | Repaire | Ce qui le fait sortir de l'ombre → ce qu'il fait | Corps (Paragon) |
|---|---|---|---|
| **Brise-Chaînes** (Gorm Poing-de-Fer) | caves sous les crassiers, près de la Ligue | une cité qui enchaîne tient 20 captifs → révolte armée (gardes tués, 70 % libérés, certains les rejoignent) ; un convoi près du repaire → délivré | Crunch : serf colossal, chaînes refondues en gantelets |
| **Glaneurs de la Voûte** (Sœur Ysolde) | ermitage des collines, près du Concordat | avant l'Essaim : une fois, souffler sur les braises (`WarCause::Instigated`, `War::instigator`) ; l'Essaim à 350 cases → empoisonner le grenier du peuple le plus lucide | Morigesh : masques et insectes, le culte de la Moisson |
| **Main Grise** (Orsa la Main) | une taverne de cité, une nouvelle chaque saison | un peuple qui perd sa guerre paie (minerai) → un chef ou un officier « trouvé mort dans sa halle » ; jamais un chef en coalition contre l'Essaim. Raté : l'assassin est pris, la victime soupçonne le payeur | Kallari : encapuchonnée, dagues |
| **Gens du Gué** (Bastien le Passeur) | bacs cachés au bord de l'eau | une cité côtière assiégée → renforts passés par l'eau (siège retourné) ; une cité que l'Essaim va prendre → réfugiés passés. Le peuple paie le péage | Drongo : récupérateur bricolé |
| **Veilleurs du Col** (Hélène Vaast) | fortin de montagne, près du Protectorat | chaque pendu du Protectorat fait des déserteurs ; dix pendus → embuscade contre ses armées ; une cité tombe à l'Essaim → coup dans la ruche voisine ; une armée vers une ruche → guidée par le col | LtBelica : officier déserteuse |
| **Kesh, la Lame d'Avant** (niv. 10) | ermitage de la crête, loin de tout | une couvée monte à sa crête → elle n'en redescend pas ; lâché par le joueur → mène une armée (son niveau compte au combat, il peut y mourir) | FengMao : sabreur |
| **Isaure de Hael, la Protectrice déchue** (niv. 9, six gardes) | fort en ruine face à sa capitale perdue | une cité du Protectorat assiégée par plus fort → elle descend la défendre | Countess : régnante renversée |
| **Le Revenu** (niv. 10) | carcasse d'épave | l'Essaim à 250 cases → il entre dans une ruche (l'Essaim ne le reconnaît pas comme proie) et tue couvain et champion | Revenant : l'homme revenu de la chair |

Trop de mal fait à un peuple (sa colère, `Circle::heat`) : il fouille les collines et tue des membres.

**À l'image** (`Game/SwarmCircles.cpp`) : au repaire, le chef en personne avec son corps de héros (un successeur garde un
corps d'habitant), ses membres autour du feu au nombre de la simulation, le repaire habillé à leur image ; il regarde qui
vient ; E : la caméra vient sur son visage et il dit les trois cercles (fait, ce qu'il faut maintenant, la suite).

**Le joueur.** On ne les voit pas sur la carte. Indices : la rumeur à la taverne de la cité la plus proche, leur signe
là où ils ont frappé, un des leurs mort qui porte leur marque, leur messager sur la route, un des leurs dans une geôle.
Deux indices (ou suivre le messager) disent où chercher ; on les trouve en arrivant. Trois cercles :
le **seuil** (un service payé en rations, refusé selon le passé : couleurs d'un peuple qui enchaîne, haine des
Affranchis, champions de la Moisson tués, service de la junte) ; le **cercle** (une tâche vérifiée par la simulation :
sortir six captifs des fers, semer le grain noir dans un grenier, un officier de votre main, trois rations à des
assiégés, un champion abattu ; puis une leçon qui monte une compétence d'un niveau contre des rations) ; le **cercle
intime** (deux tâches et leur condition propre) : leur secret, vrai dans la simulation (où est l'Arche et la Reine, la
plus grande geôle, qui a payé le dernier contrat, les armées en marche, la ruche la plus mal gardée), et UNE
intervention par mois, payée, exécutée le lendemain. Servir la Main Grise ferme les Brise-Chaînes, les Glaneurs
ferment les Veilleurs et le Revenu (et ceux dont on était du cercle le traquent). Livrer un repaire à un peuple : il le
fouille, et le cercle vous traque. Les individus : les trouver, les nourrir, remplir leur condition (Kesh : tenir debout
en duel contre lui, puis trois ennemis nommés abattus ; Isaure : la haine de la junte, puis son chef mort de votre
main ; le Revenu : ouvrir un parc de l'Essaim, puis un champion abattu).

**Mesuré** (100 mondes, sans joueur, `swarm_headless --circles`) : 21 interventions par partie, la première vers le
jour 44 ; 0,95 chef de peuple tué par partie, 2,4 sièges retournés, 0,4 guerre allumée ; 22 captifs libérés, 130 réfugiés
passés, 155 bêtes de l'Essaim tuées ; 0,7 individu mort par partie. Équilibre : Arche 82 %, Essaim détruit 18 %, monde
mort 0 % (avant : 83 / 17 / 0).
