# Référence : villes, camps et vie — pour refaire les lieux de SWARM

État au 19/11/2026. Document de référence pour les trois équipes qui refont les villes. Il ne décrit pas le code ; il
dit ce qu'il faut bâtir et pourquoi. Ordre de lecture pour aller vite : §0 (diagnostic), §5 (application), §2
(principes) ; §1, §3, §4 servent de preuves et d'idées.

Mention **(non vérifié)** : fait tiré de mémoire ou d'une seule source peu sûre, que je n'ai pas pu recouper.

---

## 0. Diagnostic : ce qui existe dans SWARM (lu dans le code et le journal)

- **20 cités au départ** (`kFactionDefs[].settlements`) : Concordat 3, Ligue 4, Clans 4, Protectorat 4, Affranchis 5.
  Elles sont toutes du même rang, sauf la capitale.
- **La capitale n'est que deux fois une cité** : rayon 3 cases contre 2 (`addSettlement`), capacité = biomasse des
  cases couvertes (`SettlementCapacity`) → 7×7 = 49 cases contre 5×5 = 25. C'est la cause chiffrée du « toutes
  les villes ont la même taille ».
- Un promeneur montré pour 14 habitants simulés, 28 au plus (J20) ; 220 corps à squelette par capitale coûtent déjà
  cher (Reprise, point 1).
- Plan (`SwarmCityPlan`, phase 4) : enceinte polygonale par peuple, grand-rue porte → place, parcelles façade sur rue ;
  trois variantes de bourg (village-rue, village-tas, hameau fortifié). Butte de 4,8 à 9,5 m avec la halle du chef
  dans chaque capitale, taverne et forge où l'on entre, deux lieux cachés.
- Ce qui manque, d'après le propriétaire et le journal : une hiérarchie de tailles, un site propre à chaque lieu
  (toutes les cités sont sur sol nivelé), des bâtiments et des remparts habités, des routines, des satellites
  (fermes, moulins, camps) et des lieux uniques.

---

## 1. Gothic 3, lieu par lieu

Clé : **(a)** site et arrivée, **(b)** taille, **(c)** repère, **(d)** organisation, **(e)** ce qui le rend unique et
ses satellites. Sources principales : fiches de lieux de gothicz.net (recoupées avec les guides GamePressure,
PC Games et NamuWiki). Les wikis Fandom et l'Almanach de World of Gothic ont refusé la lecture automatique : **aucune
taille chiffrée n'a pu être vérifiée** ; les tailles ci-dessous sont des ordres de grandeur estimés (marqués *est.*).

### Myrtana (tempérée : colombage, pierre, palissades, forêts)

- **Ardea** — (a) Falaise de la côte est, juste au sud de Vengard ; premier village atteint après le débarquement.
  (b) Village rond, une quinzaine de maisons, ~80 m (*est.*). (c) Au nord, les tours de Vengard sous leur barrière ;
  à l'est, un phare. (d) Palissade ronde à deux brèches ; l'orc a tenu le village, il ne reste que des esclaves sous
  le chef Hamlar ; forge vide, alchimiste parti, taverne sans aubergiste, grenier vide ; seul l'autel d'Innos
  fonctionne. (e) Le village des absences ; cabanes de pêcheurs sur la plage en bas, phare tenu par un ancien marin,
  bandits, camp rebelle dans les bois voisins.
- **Cap Dun** — (a) Côte sud-est, à l'ombre d'un énorme rocher, enfilé sur une seule longue route. (b) Bourg-rue,
  20–30 bâtiments (*est.*), sans muraille signalée. (c) Le phare, visible de très loin, devenu prison. (d) Esclaves
  à l'entrée (bûcheronnage, champs) ; le commandant orc Uruk dans l'ancienne maison du chef, qu'il ne quitte guère ;
  tente du marchand d'esclaves, alchimiste, entrepôt gardé à entrée latérale, cabane des mercenaires ; sur la hauteur,
  arène de fortune en cercle de torches, cimetière, phare. (e) Grottes sous la ville jusqu'à la plage au pied du
  phare ; autel de Beliar, ferme à cochons, cabane de chasseur.
- **Reddock** — (a) Côte est entre Ardea et Cap Dun, invisible : une petite fosse sous des buissons épais. (b) En
  surface deux huttes et un camp de bûcherons ; dessous, un large puits. (c) Aucun : c'est le principe. (d) Corniche
  qui fait le tour du puits, grottes latérales (vivres, étude, couchage) ; marchand et alchimiste en haut, chef des
  combattants et arène clôturée de 5 × 5 m au fond ; forge vide (forgeron mort). (e) Le plus grand camp rebelle de
  Myrtana ; galerie gardée vers une seconde sortie ; une ferme tenue par une patrouille orc à côté.
- **Montera** — (a) Intérieur, au croisement des routes du royaume ; château-forteresse au pied d'un massif rocheux.
  (b) Ville moyenne, 40–60 bâtiments (*est.*). (c) Le château contre la roche. (d) Maisons en demi-cercle autour du
  château, murs de pierre dont le mur sud est abattu ; orcs au château, humains à leur service, mercenaires qui
  tiennent l'ordre ; forge, taverne, arène ; le temple d'Innos ruiné sert de camp d'esclaves qui fouillent des
  artefacts. (e) Maisons brûlées, tours de guet en ruine habitées par des bêtes ; trois fermes : domaine au bord de
  la rivière, ferme à bêtes devant la porte (loups), grande ferme à blé avec moulin et grenier.
- **Gotha** — (a) Entre Montera et Faring ; château des paladins en hauteur (relief exact non vérifié). (b) Ville
  fortifiée moyenne (*est.*). (c) Le château. (d) Une malédiction des chamanes a changé les habitants en morts ;
  démon dans la grande cour ; les orcs campent en dessous et n'y entrent pas ; hutte du commandant orc ; grotte sous
  le château. (e) **La ville des morts** que l'occupant n'ose pas franchir ; une fois libérée, des réfugiés peuvent
  s'y installer.
- **Okara** — (a) Ancienne mine de fer isolée, loin des villes. (b) Camp, peu d'habitants. (c) Une statue d'Innos au
  milieu du camp. (d) Rebelles du paladin Roland ; on y vend armes, armures, arcs et potions. (e) Les orcs de Montera
  ne l'ont jamais trouvé ; avant-postes rebelles autour de Montera. *Mon souvenir d'une « ville » d'Okara n'est pas
  recoupé : les deux sources lues en font un camp.*
- **Trelis** — (a) Sud de Myrtana, vers le passage de Varant (position exacte non vérifiée). (b) Forteresse.
  (c) Murs massifs. (d) Grosse garnison orc ; surveillants hashishins dont le chef orc se méfie ; un humain venu de
  l'ancienne colonie commande des orcs ; un temple voisin en fouilles, avec un chamane. (e) L'occupant surveillé par
  son propre allié ; ferme ruinée tenue par des orcs à l'ouest, ferme pillée au sud, fugitif caché dans une ferme au
  nord.
- **Nemora** — (a) Montagnes près de Trelis : ancienne mine qui tient le seul passage vers Varant. (b) Camp à
  plusieurs niveaux (un « niveau le plus bas » est cité). (c) Non décrit. (d) Rebelles qui embusquent les caravanes
  hashishins et pillent en secret les fermes ; un ancien mage du feu y fait des potions. (e) Un second groupe dans
  une grotte près de Trelis surveille les squelettes du temple.
- **Geldern** — (a) Plaque tournante du commerce, entourée de mines (relief non décrit). (b) Ville moyenne (*est.*).
  (c) Non décrit. (d) Chamanes orcs dans le bâtiment principal, gardé par un guerrier d'élite ; les artefacts
  fouillés y convergent ; marché noir près de l'arène, où l'on gagne aussi des artefacts au combat ; marchand
  d'esclaves, entrepôt, un mage qui travaille contraint. (e) La ville-comptoir des mines ; druides et rôdeurs autour.
- **Silden** — (a) Rive du lac de Silden. (b) Le plus grand *village* de Myrtana, presque sans fortifications, d'où
  sa chute rapide. (c) Le moulin à eau sur le lac, seul de ce genre dans le jeu. (d) Trois parties : en haut au nord,
  les riches (maison du gouverneur orc, forge) derrière leur propre palissade ; au bord de l'eau près du moulin, un
  camp-prison orc pour esclaves et condamnés ; plus loin, l'arène et les maisons modestes. (e) Cercle de pierres
  dressées juste hors du village.
- **Faring** — (a) Extrême nord, sur la rivière Faring, sous les premiers pics glacés de Nordmar ; verrou de la
  route marchande. (b) Forteresse plutôt que ville. (c) Le château le plus fort du jeu : double enceinte massive,
  hautes tours qui commandent le terrain. (d) Chef orc Kan, qui commande les mercenaires du continent ; chamane
  marchand ; deux arènes ; chasses au troll ; autrefois paladins, temple d'Innos, auberge pleine de marchands.
  (e) La porte de Nordmar ; grotte voisine.
- **Vengard** — (a) Côte nord-est sur la mer de Myrtana, ville coupée par la rivière Faring. (b) La plus grande
  ville du jeu (taille non vérifiée). (c) L'immense château des rois. (d) Palais royal → basse-cour entre palais et
  ville (paladins, petite noblesse, domestiques) → ville : quartier riche de la rive droite, artisans et marchands
  ailleurs. (e) Barrière magique contre les orcs : on n'y entre que par téléportation ; le commerce en meurt. Camps de
  siège orcs autour (non vérifié).
- **Fermes et tours isolées** — Trois fermes de Montera ; ferme à cochons de Cap Dun ; fermes autour de Trelis
  (l'une ruinée et occupée, l'une pillée, l'une refuge) ; phare d'Ardea habité par un marchand ; la ferme est le
  premier lieu que la guerre touche.
- **Cercles de pierres** — Cercle dressé à la sortie de Silden, avec une pierre de téléportation ; druides cités
  autour de Geldern. Inventaire complet des cercles non vérifié.
- **Ruines** — Temple d'Innos en fouilles à Montera, temple en fouilles près de Trelis, tours de guet ruinées ;
  les orcs font creuser les ruines par les esclaves : la ruine est un chantier, pas un décor.
- **Mines** — Geldern (plusieurs), Okara et Nemora (anciennes mines devenues camps rebelles). La mine abandonnée
  est le refuge naturel du rebelle.
- **Chasseurs et bûcherons** — Cabane de chasseur près de Cap Dun ; camp de bûcherons à la surface de Reddock (gêné
  par des sangliers) ; esclaves bûcherons à l'entrée de Cap Dun.
- **Campements orcs** — Sous Gotha ; patrouille de ferme près de Reddock ; ferme près de Trelis ; camp autour du
  temple des Anciens au sud de Mora Sul (Varant).

### Nordmar (neige, bois massif, clans en hauteur)

- **Clan du Loup** — (a) Probablement le premier établissement en entrant dans Nordmar. (b) Village de clan, 10–20
  bâtiments (*est.*). (c) La tour de Xardas au nord-est. (d) Chef Grim ; fondeur-forgeron, intendant, marchand de
  trophées contre flèches ; golems de glace donnés par Xardas. (e) Les chasseurs mènent des loups apprivoisés contre
  les gobelins ; camp de bûcherons, chasseurs dans une vallée infestée de gobelins, ermite et son loup près de la
  tour.
- **Clan du Marteau** — (a) Au pied d'une des plus hautes montagnes de Nordmar. (b) Village de clan (*est.*).
  (c) Non décrit ; son fourneau passe pour aussi vieux que la création. (d) Chef Tjalf ; forge d'armes et d'armures,
  fonderie, mine dans une grotte voisine. (e) Libérer **la fonderie de montagne**, prise par les orcs, est la plus
  grande quête du clan ; l'ancien maître-forgeron vit à l'écart, au-dessus d'un tombeau.
- **Clan du Feu** — (a) On y entre par un pont, gardé. (b) Village de clan (*est.*). (c) Non décrit. (d) Chef
  Kerth ; fondeur, forge, autel d'Innos ; chasseurs. (e) Le plus ancien clan ; les tombeaux des ancêtres sont
  semés dans Nordmar, profanés par les gobelins ; route du monastère ; camp de gobelins voisin.
- **Monastère** — (a) Refuge des mages du feu, en montagne (relief non vérifié). (b) Petit. (d) Gardien à
  l'entrée, sanctuaire d'Innos qu'on ne franchit pas sans titre, bibliothèque (chronique du fondateur du clan du
  Feu), petite chapelle, alchimiste. (e) Les calices de feu ; le savoir caché au sommet.
- **Fonderie de montagne** — Tenue par les orcs, cible des guerriers libres du Marteau (description physique non
  trouvée).
- **Tombeaux** — Tombeaux des héros dispersés, gardés ou profanés ; un ancien forgeron vit au-dessus de l'un d'eux.

### Varant (désert : terre crue, coupoles, toiles, oasis)

- **Braga** — (a) Petite ville-oasis, première étape depuis Myrtana. (b) « Plutôt une oasis qu'une ville ».
  (c) Non décrit. (d) Hashishins sous Nafalem ; marchand d'esclaves qui paie la tête des chefs nomades ; armurier,
  marchand d'armes. (e) Ruines tout autour où se cachent les nomades ; camp nomade sur une colline à l'ouest.
- **Ben Erai** — (a) À l'ombre de montagnes percées de galeries d'or. (b) Petite ville (*est.*). (c) Les galeries
  dans la roche. (d) Un gouverneur ventru qui tient aussi Lago, un rival qui feint la loyauté ; garde à l'entrée de
  la mine qui envoie les nouveaux au travail ; alchimiste. (e) Réservoir d'eau hors de la ville (un chasseur nomade
  y campe) ; grotte aux vers dans une vallée.
- **Ben Sala** — (a) À l'est, au pied ouest d'une montagne qui la sépare de Bakaresh. (b) Petite ville. (c) Au
  nord, un temple à plan carré visible de loin, envahi de morts-vivants. (d) Ancienne forge de Varant (enclume dans
  ses armes), forge sans matière ; hashishins et orcs ensemble contre les morts ; nomades qui pillent les
  marchands. (e) Deux mines : l'une dans la roche derrière la ville, l'autre derrière une colline.
- **Bakaresh** — (a) Côte nord-est, sur la mer de Myrtana. (b) Deuxième ville de Varant, mais de la taille d'un
  village ; **sans murailles** : on craint la colère de Beliar. (c) Temple de Beliar à trois tours reliées par des
  ponts, la plus haute couronnée d'un trône. (d) Les mages noirs gouvernent depuis le temple ; hashishins en
  patrouille ; village à l'ombre du temple ; arène, chaire de prédication, marché. (e) Ville de pèlerinage.
- **Ishtar** — (a) Pointe sud de Varant, contre une falaise. (b) Capitale modeste et bien gardée. (c) Le palais.
  (d) Ville en demi-cercle régulier, **une seule porte** ; palais au centre, muré à part, avec un jardin rare, en
  partie taillé dans la falaise sud ; Zuben, ses mages et son harem ; un intendant tient la ville et le travail des
  esclaves. (e) La capitale qui tient dans une main : on y est toujours vu ; deux autels de Beliar.
- **Lago** — (a) Petite oasis du nord-ouest entre de hautes collines ; un passage étroit mène à la mer, vue sur la
  côte sud de Myrtana. (b) Quelques maisons. (c) Non décrit. (d) Hashishins en titre, pillés par les nomades ;
  culture d'herbe des marais par un esclave. (e) Petite mine d'or à l'arrêt ; fosse de gladiateurs envahie de cinq
  mètres ; une cellule avec un prisonnier célèbre.
- **Mora Sul** — (a) Sud de Varant. (b) Grande ville marchande. (c) Tours monumentales visibles de loin. (d) Le
  chef, un riche hashishin, prélève sur chaque affaire, même des voleurs ; palais ouvert sur cadeaux ; marchands
  d'artefacts et d'esclaves ; arène. (e) Au sud, temple des Anciens entouré de ruines et d'un camp orc ; à l'ouest,
  une oasis-marché aux esclaves.
- **Al Shedim** — (a) Sud de Varant : cité-temple en ruine des Anciens. (b) Grande ruine. (c) Un temple géant qui a
  survécu à un déluge, scellé par cinq clés. (d) Cinq quartiers en ruine (bibliothèque, guérisseurs, école des
  guerriers, gardiens, prêtres) ; nomades et mages de l'eau au nord du temple ; gobelins ; monstres. (e) La ruine
  comme ville entière, découpée en quartiers ; deux autres groupes nomades autour.
- **Camps nomades, oasis, temples** — Nomades dans les ruines de Braga, autour d'Al Shedim, pillards à Lago ; camp
  des mages de l'eau ; oasis de Lago, de Braga, oasis-marché à l'ouest de Mora Sul ; temples de Ben Sala, de
  Bakaresh, des Anciens (Mora Sul, Al Shedim).

---

## 2. Les principes à en tirer

1. **Mêmes pièces, partis différents.** Gothic 3 bâtit Myrtana avec un seul jeu de maisons à colombage, de
   palissades et de tours ; ce qui sépare les lieux, c'est le site et un geste. Ardea (palissade ronde sur une
   falaise) et Cap Dun (une seule rue sous un énorme rocher, phare-prison) ont les mêmes maisons et ne se confondent
   jamais. → SWARM peut garder un kit par peuple, à condition que le **site** change d'un lieu à l'autre.
2. **Un seul grand geste par lieu**, qu'on résume en une phrase : le phare-prison de Cap Dun, le château collé au
   massif rocheux de Montera, le puits en spirale de Reddock, Vengard sous sa barrière magique, Gotha ville des
   morts. Règle : si l'équipe ne peut pas dire le lieu en dix mots, il n'existe pas encore.
3. **Petit mais dense ; rien n'est vide sans raison.** Ardea fait une quinzaine de maisons (non vérifié pour le
   compte exact), mais chaque bâtiment a un occupant ou une absence qui raconte : forge vide, alchimiste parti,
   taverne sans aubergiste, grenier pillé. Une maison vide doit être *condamnée, pillée ou en deuil*, jamais
   simplement non meublée.
4. **Verticalité = pouvoir.** En haut l'occupant, en bas le travail : Montera (château orc au pied du massif,
   esclaves qui fouillent le temple ruiné), Cap Dun (arène, cimetière et phare sur la hauteur ; grottes au pied de
   la falaise), Reddock (marchands sur la corniche, chef et arène au fond du puits).
5. **Le lieu se lit depuis la route.** On voit le repère avant les murs (phare de Cap Dun « visible de très loin » ;
   tours de Vengard visibles depuis Ardea), puis les satellites, puis le seuil. Composer chaque arrivée en quatre
   temps : **repère → satellites → seuil → place**.
6. **Une ville déborde de ses murs.** Montera a trois fermes : un domaine au bord de la rivière, une ferme à bêtes
   devant la porte (assaillie par les loups), une grande ferme à blé avec moulin et grenier. Cap Dun a sa ferme à
   cochons et sa cabane de chasseur. La ville se lit sur un kilomètre autour d'elle.
7. **Hiérarchie des tailles, proportions tenues.** Gothic compresse (village ↔ capitale dans un rapport de 1 à 5
   environ, non vérifié en chiffres) ; il compense par la densité. SWARM a un monde simulé et une carte plus vaste :
   il doit étirer l'échelle (§5a) — la capitale vaut des dizaines de villages.
8. **Le contre-pouvoir vit dans le relief, pas en ville.** Le plus grand camp rebelle de Myrtana (Reddock) est un
   puits caché sous des buissons ; le camp d'Okara n'a jamais été trouvé par les orcs ; Nemora tient une ancienne
   mine sur le seul passage vers Varant. Chaque puissance doit avoir son ombre à une heure de marche.
9. **Une culture par région, lisible sans carte.** Myrtana : colombage, pierre, palissades, forêts tempérées.
   Nordmar : bois massif, neige, villages-clans perchés derrière leurs palissades. Varant : terre crue, coupoles,
   toiles, palmiers, oasis. On sait où l'on est à la silhouette d'un toit.
10. **L'occupation se voit à ce qui manque et à qui travaille.** L'occupant prend la maison du chef (Uruk à Cap
    Dun), les occupés travaillent à l'entrée (bûcheronnage, champs), les services s'éteignent. SWARM a des guerres,
    des captifs et des sièges réels : une cité prise doit **changer d'aspect** (bannières, gibet, maisons
    condamnées, esclaves aux champs).
11. **L'histoire laisse des cicatrices persistantes.** Le mur sud abattu de Montera, ses tours de guet en ruine
    habitées par des bêtes ; Gotha, ville maudite dont l'occupant n'ose pas franchir les portes. Dans SWARM chaque
    siège, incendie et passage de l'Essaim doit laisser une trace qui dure des saisons.
12. **Chaque lieu a une raison d'y aller.** Arène (Cap Dun, Montera, Silden, Faring, Mora Sul), mines (Geldern,
    Ben Erai, Ben Sala), temple et fouilles (Trelis, Montera), pèlerinage (Bakaresh), marché (Mora Sul, Geldern). Un
    lieu sans fonction pour le joueur est un décor.
13. **Le plus grand n'est pas le plus fort, ni l'inverse.** Silden, le plus grand village, tombe faute de murs ;
    Faring, petite, est la plus forte ; Bakaresh, deuxième ville de Varant, n'a pas de murailles car on craint son
    dieu ; Ishtar, capitale, est modeste mais n'a qu'une porte. La forme dit ce que le peuple craint.
14. **Le quartier est séparé par une limite physique.** Silden : palissade intérieure autour des riches. Vengard :
    palais, basse-cour, ville, et la rivière qui coupe riches et artisans. Ishtar : palais muré dans la ville. Une
    enceinte intérieure, une rivière, un pont ou un escalier suffisent à faire un quartier.
15. **L'échelle d'une vraie capitale existe ailleurs** : Kuttenberg (Kingdom Come 2) fait environ dix fois Rattay,
    la plus grande ville du premier jeu, et s'organise en marchés (charbon, fruits) et rues de métier (bouchers,
    tailleurs). C'est le bon rapport cité → capitale, et la bonne façon de remplir une capitale.
16. **Les camps sont des lieux à part entière**, pas des tas de tentes : Reddock a des grottes de stockage,
    d'étude et de couchage, une arène de cinq mètres sur cinq clôturée, une forge sans forgeron, une sortie
    secrète gardée. Densité d'usage, pas de surface.

---

## 3. La vie

### Ce que font Gothic et Red Dead Redemption 2

- **Gothic** : chaque PNJ a une **routine quotidienne** écrite en plages horaires (de telle heure à telle heure :
  telle activité, à tel endroit) ; un combat ou un dialogue l'interrompt, puis elle reprend (documentation « Inside
  Gothic »). Activités typiques des deux premiers jeux, de mémoire et non recoupées une à une : dormir, manger au
  feu, rôtir la viande à la broche, forger à l'enclume, balayer, fumer, boire à la taverne, s'entraîner à l'épée,
  prier, monter la garde, s'asseoir sur un banc, jouer du luth. La force n'est pas la variété mais la **fidélité** :
  le forgeron est à l'enclume chaque matin, on le retrouve.
- **RDR2** : routines entièrement écrites — se lever, aller au travail vers 8 h, passer au saloon après le travail ;
  un habitant suivi à Valentine s'endort au saloon vers 1 h du matin. Des personnages ne sont là qu'à certaines
  heures (à Saint Denis, de 8 h à 18 h pour l'un, de 9 h à 21 h pour un autre) ; le trappeur de Saint Denis est
  tenu à toute heure à partir d'un certain chapitre. Le reste, de mémoire (non vérifié en détail) : Valentine ville
  de bétail (boue, enclos, vente aux enchères, saloon et rixes) ; Saint Denis grande ville (tramway, crieurs de
  journaux, marchands des rues, quartiers pavés et misère) ; Rhodes petite ville du Sud tenue par deux familles ;
  Strawberry ville de montagne **en chantier** (ouvriers qui bâtissent) ; le camp de la bande (corvées : foin, bois,
  ragoût ; chants autour du feu le soir ; chacun réagit au retour du joueur). La force : **chaque lieu a une
  activité économique visible**, et la ville répond au joueur (salut, insulte, fuite, témoin).

### Règles pour SWARM

**1. Trois ancres par habitant.** Une maison, un poste de travail, un lieu social (feu, taverne, temple, place). La
routine est une suite de plages entre ces trois ancres ; jamais une promenade sans but. Les flâneurs existent, mais
ils sont 15 à 25 % au plus.

**2. La journée type** (heures du jeu ; un jour = 20 min réelles) :

| Heures | Ce que fait la majorité | Exceptions visibles |
|---|---|---|
| 4–6 | sommeil ; boulanger et palefrenier levés, feux qu'on rallume | relève de la garde de nuit, premier convoi qui part |
| 6–8 | lever, puits, toilette, bêtes sorties, premier repas | cloche ou corne du matin (une par peuple), ouvriers vers les champs et la mine |
| 8–12 | travail : ateliers ouverts, étals montés, champs | exercice de la garnison dans la cour, enfants qui courent |
| 12–14 | repas au feu, à l'ombre ; marchés au plus plein | sieste chez les Clans (été) ; prière de midi chez Aram |
| 14–18 | travail ; arrivées de charrettes et caravanes | harangue, rixe, crieur public (scènes existantes de J24) |
| 18–21 | retour, taverne pleine, feux de camp, prière du soir | jeux, musique, disputes, recrutement |
| 21–4 | 75–85 % chez eux, volets clos | garde, veilleurs, ivrognes, amants, voleurs, malades, Main Grise |

**3. Proportions de jour** dans une rue : 40–50 % qui **travaillent** de façon lisible (outil en main, charge sur le
dos), 25–35 % qui **passent** avec un but (porter, livrer, rentrer), 15–25 % qui **s'arrêtent** (parler, s'asseoir,
regarder), 5–10 % de **gardes** — 15–20 % au Protectorat, presque aucun chez les Affranchis où la milice est en
civil, avec un brassard.

**4. La nuit** : fenêtres éclairées à 60 % à 21 h, 10 % à 2 h ; tavernes et forges de la Ligue (qui ne s'éteignent
jamais) font les seules îles de bruit ; braseros sur le rempart tous les 40–60 m ; boutiques fermées (porte close,
volet tiré, on frappe : le marchand ouvre en grognant, prix majoré) ; portes de la ville fermées de 22 h à 5 h sauf
chez les Clans et les Affranchis.

**5. La pluie** : rues à 40 % de leur monde ; les gens sous les auvents et les porches ; étals bâchés ou
démontés ; tavernes +50 % ; travaux des champs arrêtés, forges et mines continuent ; chiens sous les charrettes ;
boue qui garde les traces (sol de Valentine).

**6. Le jour de marché** (un jour sur sept dans les bourgs et cités, permanent dans les capitales) : +60 à +100 % de
monde ; dès l'aube, paysans des hameaux sur la route avec charrettes et bêtes ; étals sur la place et la grand-rue ;
crieurs, bétail, voleurs, garde doublée ; tout repart avant la nuit. C'est **le** moment où un bourg paraît une
ville.

**7. Densité visible par rang** : voir la colonne « Visibles au cœur » du tableau 5a. Plancher : jamais moins de
3 personnes dans le champ dans une rue de village de jour, 15 dans une rue de capitale. Mieux vaut moins de corps
pleins et plus de silhouettes au fond qu'une rue vide.

**8. Des bâtiments et des remparts habités** (reproche direct du propriétaire) :
- chaque maison montre un état : **habitée** (fumée, lumière le soir, linge, porte entrouverte, pot de fleurs,
  chien), **condamnée** (planches en croix, après famine ou mort), **pillée** (porte arrachée, objets dans la rue),
  **en deuil** (tissu noir, voisins devant) ; ces états suivent la simulation (population, pertes, prise) ;
- le rempart a ses gens : sentinelles sur le chemin de ronde (une tous les 40–60 m de jour, deux fois moins la
  nuit), relève visible à heure fixe, braseros, linge accroché aux créneaux chez les pauvres ; tours avec un poste
  (feu, table, jeu de dés) ;
- la garnison a une routine : exercice le matin, corvées, repas, garde ; elle n'attend pas plantée.

**9. Les métiers se voient et s'entendent** : forgeron (marteau, étincelles), meunier (roue qui tourne, sacs),
boucher (carcasse, mouches), lavandières au lavoir ou à la rivière, porteurs d'eau, charrons, tanneurs à l'écart et
à l'odeur (en aval), boulanger à l'aube. Un métier par lieu doit être **l'activité-signature** du lieu (le haut
fourneau de Haut-Four, les semis bénis du Semoir).

**10. Bêtes et circulation** : chiens errants (2–5 par village), poules et cochons dans les cours, chevaux attachés
devant la taverne, chats sur les toits ; entre deux lieux voisins, une charrette ou un voyageur toutes les 2–4
minutes réelles de jour, convois et caravanes visibles de loin (poussière) ; aux portes, file du matin et du soir.

**11. Les événements de rue** (en plus de la harangue et de la rixe de J24) : pendaison ou bastonnade publique,
enterrement, mariage, arrivée d'une caravane, incendie qu'on éteint en chaîne de seaux, recrutement sur la place,
voleur poursuivi, bête échappée, annonce du crieur (tirée de la chronique de la simulation). Un toutes les 3–5
minutes dans une cité, une par jour dans un village.

**12. L'état du monde se lit dans la rue** : famine → file devant le grenier, étals vides, mendiants ; guerre →
recrues à l'exercice, forges qui battent, affiches de levée ; siège → réfugiés dans les rues, rationnement ;
Essaim proche → charrettes chargées qui partent, prières, portes barricadées ; victoire → fête, bannières.

**13. Réactions au joueur** (base J24 existante) : à ajouter — enfants qui le suivent, marchands qui l'appellent,
volets qu'on ferme sur son passage s'il est craint, salut du garde s'il est estimé, témoin qui court prévenir.

---

## 4. Les autres références

- **Kenshi — la ville tient au pouvoir qui la tient.** Chaque faction a ses villes-forteresses : Heng (Cités-Unies),
  ville de commerce fortifiée sur un plateau rocheux ; Stack (Saint Empire) aux portes à projecteurs ; Admag,
  capitale du royaume Shek ; Squin, ville-frontière avec cuivre et fer à portée. The Hub, ancienne ville du Saint
  Empire, est en ruine et tenue par de petites bandes. Quand le joueur tue ou enlève un chef, la ville **change de
  visage** au retour (« Town Override » : version affaiblie, version enragée). Leçon : une cité prise, un chef
  tombé, l'Essaim passé doivent produire un autre lieu, pas le même avec un drapeau différent. Avant-postes et
  ruines disséminés font le tissu entre les villes.
- **Kingdom Come — le village crédible.** Bâti à l'échelle réelle, chaque maison et chaque remise d'après
  l'architecture médiévale ; Rattay d'après la vraie Rataje nad Sázavou (château et église). Les villages (Uzhitz,
  plus petit que Rattay ou Sasau) ont ce que SWARM doit donner au rang « village » : champs en couronne, étang,
  moulin, église, auberge sur la route, scierie, cimetière (de mémoire pour la liste). Kingdom Come 2 donne la
  proportion : **Kuttenberg ≈ 10 × Rattay**, quartiers nés autour des marchés et des rues de métier, la Cour
  italienne (monnaie) comme centre du pouvoir, et les galeries de mines d'argent sous la ville.
- **The Witcher 3 — Novigrad, la capitale par couches.** Quartiers nommés et distincts : Temple Isle (centre
  religieux, île reliée par le pont Saint-Grégoire, où la classe et l'architecture changent), Gildorf (riche),
  Silverton, Harborside (port), Farcorners, The Bits (misère surpeuplée à l'est), plus les faubourgs hors les murs.
  Modèle cité : Amsterdam médiévale avec des touches de Venise (fan-wiki). Leçon : ce sont les **ponts, l'eau et
  les murs** qui font les quartiers, et le joueur sent qu'il change de classe sociale en traversant un pont.
  Bûchers et prédicateurs sur les places (de mémoire, non vérifié).

---

## 5. Application à SWARM

**Les cinq décisions qui comptent le plus** (le détail suit) :
1. Un **rang** par lieu (ferme → capitale), et une capitale qui vaut 5 cités et 100 villages — pas 2 cités.
2. Un **parti de site** par lieu (colline, gorge, île, col, fosse, oasis…) au lieu du sol nivelé partout.
3. **Un geste et un métier-signature** par lieu, qu'on résume en dix mots.
4. Des **satellites** (fermes, moulins, camps) qui dépendent des cités et tombent les premiers.
5. Des **routines à trois ancres** (maison, travail, lieu social) et des bâtiments et remparts dont l'état suit la
   simulation (habité, condamné, pillé, en deuil).

### 5a. Grille de tailles

Une case de simulation fait 200 m. Habitants = population simulée. « Visibles » = personnes montrées ensemble dans
le champ du joueur, de jour, au cœur du lieu (le reste est hors champ, dans les maisons ou aux champs).

| Rang | Habitants | Bâtiments | Emprise (murs ou limite bâtie) | Portes | Visibles au cœur | Obligatoire |
|---|---|---|---|---|---|---|
| **Ferme isolée** | 5–15 | 3–6 | 80–150 m (champs en plus) | 0 : barrière, chien | 3–6 | maison, grange ou étable, puits, champ ou enclos, tas de bois, feu le soir |
| **Hameau** | 20–60 | 6–15 | 150–250 m | 0–1 : haie, palissade basse | 6–15 | source ou puits, four ou feu commun, un atelier (bûcheron, charron), autel, chemin vers le village |
| **Village** | 100–300 | 20–50 | 250–400 m | 1–2 : palissade ou rien | 15–35 | place et puits, auberge, sanctuaire, forge, moulin (hors du bâti), cimetière, couronne de champs, 1–3 fermes satellites |
| **Bourg** | 400–1 200 | 60–150 | 400–700 m | 2 (+ brèche ou poterne) : rempart de bois ou de pierre basse | 30–60 | marché hebdomadaire et halle, 2 tavernes, rues de métier, poste de garde et garnison, faubourg d'une rue hors les murs, tour de guet |
| **Cité** | 2 000–6 000 | 250–600 | 0,8–1,4 km | 3–4 + poterne : enceinte de pierre à tours | 60–120 | quartier du pouvoir sur un relief, 3–5 quartiers distincts, port ou mine, grand temple, arène ou place d'exécution, caserne, bas-fonds, faubourgs, cimetière dehors |
| **Capitale** | 10 000–30 000 | 1 000–2 500 | 2–3,5 km ; **2 ou 3 enceintes** | 6–10 + port ou porte d'eau | 120–250 (marché) | ville haute (palais du chef + monument du peuple), ville moyenne (quartiers, marchés spécialisés), ville basse et faubourgs pris dans la 2e enceinte, 3e enceinte ou camps hors les murs, arène, nécropole, routes convergentes avec auberges-relais |

**Rapports à tenir.** Chaque rang vaut environ 4 fois le précédent en habitants (ferme 10 → hameau 40 → village 200
→ bourg 800 → cité 4 000 → capitale 20 000). La capitale vaut **100 villages** en habitants, **40 à 60** en
bâtiments, **50 à 80** en surface ; elle vaut **5 cités**. Aujourd'hui elle en vaut deux.

**Combien par peuple** (au lieu de 3 à 5 cités identiques) : 1 capitale, 1–2 cités, 2–3 bourgs, 4–6 villages,
puis 10–20 hameaux et fermes satellites. Les Affranchis n'ont pas de vraie capitale (une cité-symbole et beaucoup de
villages) ; les Clans ont un Grand Camp saisonnier (voir 5b).

**Conséquences pour la simulation et le rendu** (à trancher par l'équipe, pas de code ici) :
- Donner un **rang** à chaque lieu, fixé à la fondation, et dériver la capacité du rang, pas du seul anneau de
  biomasse (sans cela la capitale restera à 2×).
- Hameaux et fermes = **dépendances** d'une cité : leur population compte dans celle de la cité, ils vivent en 3D,
  et ce sont eux qui tombent d'abord (pillards, Essaim). Spectacle gratuit : les fermes brûlent avant les murs.
  Les chantiers (`worksite`) existants sont déjà l'embryon des camps de bûcherons et de mineurs.
- Une capitale de 2 000 bâtiments ne se bâtit pas acteur par acteur : **quartiers matérialisés autour du joueur**,
  le reste en silhouettes fusionnées ; 10 à 15 % des bâtiments entrables au plus, mais tous les bâtiments
  **habités** au sens visible (lumière, linge, fumée, porte ouverte ou condamnée).
- Foule : plafond de corps pleins (~220 aujourd'hui), au-delà de 60 m des silhouettes. La densité visible croît
  moins vite que la population (voir tableau) ; ce qui compte est qu'**aucune rue ne soit vide de jour**.

### 5b. Les cinq peuples : architecture et trois partis de site

| | Matériaux | Toits | Rempart | Couleurs | Bâtiment-repère |
|---|---|---|---|---|---|
| **Concordat d'Aram** | pierre calcaire blonde, enduit chaulé, bois clair | terrasses plates plantées, coupoles basses de pierre sur les greniers | murs de terrasses étagées : chaque jardin est un mur ; portes en chicane | blanc, vert des jardins, or | la **Tour-Grenier** : silo-reliquaire haut et étroit, percé de fentes, une lampe au sommet |
| **Ligue des Fonderies** | brique sombre, fer riveté, pierre noire, tôle | dents de scie, tôle, forêts de cheminées | digue de scories sous un mur de brique à contreforts ; portes de fer à glissière | suie, rouille, orange des fours la nuit | le **haut fourneau** et le chevalement de mine ; la lueur se voit la nuit à des lieues |
| **Clans du Vent** | feutre, toile tendue, peaux, mâts, chariots | tentes rondes, toiles en voile, auvents | pas de mur : cercle de chariots, haies d'épineux, chiens | ocre, indigo, rouge ; manches à vent | le **Mât du Clan** : mât de 25–30 m, manches à vent, crânes, cerf-volant de guet |
| **Protectorat de Hael** | pierre grise taillée, bois noirci | ardoise, pentes raides, beffrois | bastions en étoile, fossé, glacis nu ; gibets à la porte | gris, noir, rouge | la **Tour-Signal** : beffroi à feu ou sémaphore, relié aux autres tours |
| **Affranchis** | torchis, bois brut, chaume, réemploi (pierres de château, tôle volée, grilles de geôle) | chaume, appentis, toits rapiécés | palissade de pieux, fossé, barricades de charrettes ; brèches réparées | brun, paille, rouge des foulards ; chaînes brisées pendues | la **Halle commune** ou le château pris, donjon décapité, drapeau rouge |

**Trois partis de site par peuple** (chaque lieu en prend un ; ne jamais en donner deux du même parti au même
peuple côte à côte) :
- **Concordat** : (1) colline à terrasses au-dessus d'une source, la ville monte en spirale vers le grenier sacré ;
  (2) île ou presqu'île dans un lac, une seule chaussée ; (3) ville adossée à une falaise percée d'une entrée de
  voûte, façades troglodytes, jardins suspendus.
- **Ligue** : (1) gorge étroite, ville sur les deux rives, roues à aubes et ponts habités ; (2) fosse de carrière à
  ciel ouvert, ville en gradins qui descend en spirale ; (3) plateau minier hérissé de chevalements, crassiers
  comme collines artificielles autour.
- **Clans** : (1) point d'eau dans une plaine, camp en anneau autour de l'eau ; (2) cuvette abritée du vent ou
  dessous d'une arche rocheuse, parois peintes ; (3) gué d'un grand fleuve, camp saisonnier sur les deux rives,
  enclos de bêtes. Le **Grand Camp** (capitale) a deux emplacements, été et hiver ; il déménage dans la simulation.
- **Protectorat** : (1) verrou de col : la route traverse la ville par deux portes face à face ; (2) éperon au
  confluent de deux rivières, citadelle à la pointe ; (3) plateau dominant une plaine à blé, fermes de serfs en
  contrebas, routes droites tirées au cordeau.
- **Affranchis** : (1) ancienne seigneurie prise : bourg autour d'un château éventré ; (2) village caché en marais
  ou en forêt, passerelles, accès par un seul sentier ; (3) ville-moulin sur une rivière, moulins communaux, ponts.

### 5c. Vingt lieux uniques à bâtir d'abord

Un geste fort chacun ; l'inspiration Gothic 3 est donnée entre crochets, jamais reprise telle quelle. Noms tirés
des racines existantes (`kTownRoots`) quand c'est possible.

| # | Lieu (peuple, rang) | Le geste | [Inspiré de] |
|---|---|---|---|
| 1 | **Aram** (Concordat, capitale) | trois enceintes en terrasses autour de la colline creuse des Archives-Semences ; la Tour-Grenier au sommet se voit de partout | [Vengard : capitale à enceinte ; Montera : forteresse au pied du massif] |
| 2 | **Siloé** (Concordat, cité) | île dans un lac, une chaussée qui se noie aux crues ; on attend la décrue à l'auberge du bord | [aucun lieu direct : contrepoint aux cités de colline] |
| 3 | **Nébo** (Concordat, monastère-grenier) | perché au sommet, 300 marches taillées, novices qui montent les sacs de grain à dos | [monastère des mages du feu, Nordmar] |
| 4 | **Le Semoir** (Concordat, hameau) | huit maisons autour d'un cercle de pierres où l'on bénit les semis | [cercles de pierres des druides] |
| 5 | **Haut-Four** (Ligue, capitale) | ville dans une gorge, quatre hauts fourneaux ; ville basse ouvrière au fil de l'eau, ville haute des maîtres sur la crête ; la nuit, la gorge rougeoie | [fonderie du clan du Marteau ; Geldern et sa mine] |
| 6 | **Le Puits** (Ligue, cité) | ville-cratère : rues en spirale dans une fosse de mine ; plus on descend, plus on est pauvre | [Reddock : corniche en spirale, inversée et publique] |
| 7 | **Crassier** (Ligue, bourg) | bâti sur un terril tiède ; enfants qui glanent le métal, fumerolles | [Ben Sala : ancienne forge de Varant qui manque de métal] |
| 8 | **Coulée** (Ligue, village de charbonniers) | forêt rasée en souches, meules fumantes, ciel jaune | [camp de bûcherons à la surface de Reddock] |
| 9 | **Grand Camp** (Clans, capitale saisonnière) | 600 tentes en anneaux concentriques autour d'un lac salé ; piste de course, marché aux bêtes ; il n'est pas au même endroit l'hiver | [Al Shedim, camps nomades de Varant (non vérifié pour le plan)] |
| 10 | **Camp du Ciel** (Clans, camp de clan) | sous une arche de pierre géante, cerfs-volants de guet attachés à son sommet | [oasis et camps cachés de Varant] |
| 11 | **Camp des Dunes** (Clans, oasis) | palmeraie, puits à balancier, tentes ombrées ; caravanes qui arrivent au crépuscule | [Lago et Braga : oasis entre collines, pillées par les nomades] |
| 12 | **Fort Hael** (Protectorat, capitale) | verrou d'un col : trois enceintes en escalier, la route traverse la ville ; la Tour-Signal communique avec les vigies | [Trelis et Nemora : forteresse et mine qui tiennent le passage vers Varant] |
| 13 | **La Redoute** (Protectorat, cité) | étoile de bastions à la pointe d'un confluent, ponts-levis sur les deux rivières | [Faring : double enceinte sur la rivière, verrou de Nordmar] |
| 14 | **Vigie** (Protectorat, bourg) | phare de falaise devenu prison ; village de pêcheurs serfs au pied, arène de fortune sur la hauteur | [Cap Dun : phare-prison, arène sur la colline] |
| 15 | **La Herse** (Protectorat, village de serfs) | plaine à blé, fermes fortifiées, tour de contremaître au centre, cloche du travail | [Montera : grande ferme à blé, esclaves aux champs] |
| 16 | **La Commune** (Affranchis, cité-symbole) | ancien château du Protectorat, donjon décapité, enceinte seigneuriale ouverte d'une brèche jamais réparée « pour se souvenir » | [Montera : mur sud abattu ; Gotha : forteresse prise] |
| 17 | **Les Barricades** (Affranchis, village caché) | sur pilotis dans un marais, passerelles qu'on retire la nuit | [Reddock, Okara : camps rebelles introuvables] |
| 18 | **Le Quai** (Affranchis, bourg) | ville-pont : maisons et moulins communaux sur le pont même | [aucun lieu direct] |
| 19 | **Le Fortin des Veilleurs** (cercle, camp de déserteurs) | ancienne mine tenant le seul sentier du col ; embuscades sur les convois | [Nemora : ancienne mine sur le passage vers Varant] |
| 20 | **Les Caves des Brise-Chaînes** (cercle) | puits caché sous les crassiers ; corniche, grottes, arène au fond, sortie secrète | [Reddock] |
| 21 | **La Ville du monde d'avant** (ruine) | cité tombée il y a trois siècles, tours d'acier rouillées, bêtes dedans, épave d'un engin sur la place | [Gotha : ville des morts que l'occupant n'ose pas franchir] |

### 5d. Les camps hors des villes

| Camp | Où | Ce qu'on y voit | Rythme, ce qui y arrive |
|---|---|---|---|
| **Chasseurs** | lisière, au-dessus d'un gibier (harde, gué) | 2–4 tentes ou une cabane, peaux tendues sur cadres, viande qui sèche, chiens, crânes cloués | partent à l'aube, rentrent avec une bête ; vendent peaux et viande ; savent où sont les loups |
| **Bûcherons** | forêt en coupe, près d'une rivière (flottage) | souches fraîches, piles de grumes, scie de long, charrette à bœufs, abri ouvert | hache du matin au soir, chute d'arbre, repas au feu ; un accident, une dispute de salaire |
| **Charbonniers** | clairière rasée (Ligue) | meules de terre fumantes, ciel voilé, huttes coniques, visages noirs | veille de nuit des meules (un garde réveillé), feux qui prennent |
| **Mineurs** | flanc de montagne, entrée de galerie | chevalement ou treuil, wagonnets, crassier, baraquement, contremaître ; chez le Protectorat, captifs enchaînés | équipes de jour et de nuit, coups sourds, éboulement, évasion |
| **Nomades** (Clans) | point d'eau, pâture | tentes rondes, chariots en cercle, troupeau, femmes au métier à tisser, enfants à cheval | traite, abreuvoir, départ en colonne à l'aube ; démontent en une heure |
| **Contrebandiers** (Gens du Gué) | crique, bac caché, cave | barques, ballots sous toile, lanterne voilée, guetteur | arrivées de nuit, mot de passe, prix doublé pour le joueur connu |
| **Déserteurs** (Veilleurs) | fortin de col, ruine | armes de régiment, uniformes décousus, tour de guet de fortune, pendus vengés sur un arbre | embuscades, recrutement, tirage au sort de la garde |
| **Pèlerins** (Concordat) | sur la route d'un tombeau ou de Nébo | tentes blanches, autel portatif, malades, mendiants, marchands de reliques | marche à l'aube, chants, prière de groupe ; proies des pillards |
| **Pillards motorisés** | ruines du monde d'avant, croisement de routes | engins rafistolés en cercle, carcasses, réservoirs, fumée noire, trophées | sortent au moteur vers les convois ; on entend arriver avant de voir |
| **Réfugiés** (devant l'Essaim) | sous les murs d'une cité, en faubourg | abris de toile, files à la distribution, bétail sauvé, malades | grossit à chaque cité tombée ; tension avec la garde ; jour de famine |
| **Siège** | autour d'une cité assiégée | tranchées, palissade de contrevallation, tentes de régiment, forges de campagne, bourreau | relève, sape, assaut à l'aube ; ce camp est le plus vivant du jeu |

---

## Sources

Gothic 3, fiches de lieux (gothicz.net, en tchèque, lues le 19/11/2026) :
- Ardea — https://gothicz.net/gothic-3/lokace/ardea/
- Cap Dun — https://gothicz.net/gothic-3/lokace/cape-dun/
- Reddock — https://gothicz.net/gothic-3/lokace/reddock/
- Montera — https://gothicz.net/gothic-3/lokace/montera/
- Gotha — https://gothicz.net/gothic-3/lokace/gotha/
- Okara — https://gothicz.net/gothic-3/lokace/okara/
- Trelis — https://gothicz.net/gothic-3/lokace/trelis/
- Nemora — https://gothicz.net/gothic-3/lokace/nemora/
- Geldern — https://gothicz.net/gothic-3/lokace/geldern/
- Silden — https://gothicz.net/gothic-3/lokace/silden/
- Faring — https://gothicz.net/gothic-3/lokace/faring/
- Vengard — https://gothicz.net/gothic-3/lokace/vengard/
- Clan du Marteau — https://gothicz.net/gothic-3/lokace/klan-kladiva/
- Clan du Feu — https://gothicz.net/gothic-3/lokace/klan-ohne/
- Clan du Loup — https://gothicz.net/gothic-3/lokace/vlci-klan/
- Monastère — https://gothicz.net/gothic-3/lokace/klaster/
- Bakaresh — https://gothicz.net/gothic-3/lokace/bakaresh/
- Ishtar — https://gothicz.net/gothic-3/lokace/ishtar/
- Mora Sul — https://gothicz.net/gothic-3/lokace/mora-sul/
- Braga — https://gothicz.net/gothic-3/lokace/braga/
- Ben Erai — https://gothicz.net/gothic-3/lokace/ben-erai/
- Ben Sala — https://gothicz.net/gothic-3/lokace/ben-sala/
- Lago — https://gothicz.net/gothic-3/lokace/lago/
- Al Shedim — https://gothicz.net/gothic-3/lokace/al-shedim/

Gothic 3, recoupements :
- Gothic 3: Myrtana (index des lieux, GamePressure) — https://www.gamepressure.com/gothic3/myrtana/zc5be
- Gothic 3: Nemora, Trelis (GamePressure) — https://www.gamepressure.com/gothic3/nemora/zf5c7 ,
  https://www.gamepressure.com/gothic3/trelis/za5cb
- Gothic 3: Gotha (GamePressure) — https://gamepressure.com/gothic3/gotha/zd5c5
- Gothic 3 — NamuWiki (Montera, Okara) — https://en.namu.wiki/w/Gothic%203
- Lösungsweg für Rebellen (PC Games, Komplettlösung) —
  https://www.pcgames.de/Gothic-3-Spiel-4368/Tipps/Gothic-3-Komplettloesung-So-ueberstehen-Sie-das-Abenteuer-als-Rebell-Ork-und-Assassine-699981/4/
- Teleportation Stones (cercle de pierres de Silden) — https://kosgames.com/gothic-3-forsaken-gods-enhanced-edition-teleportation-stones-locations-10625/
- Gothic 3 (Wikipédia anglaise : régions, factions) — https://en.wikipedia.org/wiki/Gothic_3
- Inside Gothic, Daily Routine (routines en plages horaires) — https://ataulien.github.io/Inside-Gothic/DailyRoutine

Autres jeux :
- RDR2, suivre un PNJ une journée — https://jamtest.dexerto.com/red-dead-redemption/what-happens-when-you-follow-a-red-dead-redemption-2-npc-for-an-entire-day-215958
- RDR2, personnages à horaires (Saint Denis) — https://www.rdr2.org/guides/special-characters-guide/
- RDR2, NPC following a full day (Business Insider) — https://businessinsider.nl/red-dead-redemption-2-following-npcs-for-full-day-video-2018-11
- Kenshi, villes — https://pindrop.gg/kenshi/towns ; Town Overrides — https://raiderking.com/kenshi-town-overrides/
- Kingdom Come, historical accuracy — https://www.pcgamesn.com/kingdom-come-deliverance/kingdom-come-deliverance-historical-accuracy
- Kuttenberg: A Medieval Metropolis (Deep Silver) — https://www.deepsilver.com/games/kingdom-come-deliverance-ii/news/kuttenberg-a-medieval-metropolis
- Novigrad, The Bits (Fextralife) — https://thewitcher3.wiki.fextralife.com/The+Bits ; Novigrad (GameBanshee) —
  https://dev.gamebanshee.com/witcher-3/guides/novigrad

Inaccessibles à la lecture automatique (donc rien n'en est tiré) : Gothic Fandom (refus payant), Almanach de World of
Gothic (échec), BreezeWiki (vérification anti-robot).
