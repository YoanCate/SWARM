# Les véhicules de SWARM

Demande du propriétaire (19/11/2026) : un véhicule propre à chaque peuple et à l'Essaim, tous différents ; les humains
se volent entre eux, l'Essaim ne se vole pas mais se démonte ; puis « aérien, terrestre, maritime… ça ajoute de tout :
économie, acheter, voler, combat » ; puis « des Scarab de différentes envergures, mechas, créatures » (l'idée seulement :
des engins à plusieurs échelles ; pour l'Essaim, des marcheurs vivants). Ce document dit mes choix ; les chiffres sont
ceux de `kVehicleSpecs` (`Source/Swarm/Sim/SwarmSimVehicles.cpp`).

## Principe

Un véhicule est un **bien** de la simulation (`swarm::Vehicle`) : construit dans une cité (ou une ruche) avec du minerai
et des jours de travail — les vivres des charrons sont mangés ; pour l'Essaim, sa chair est prise sur sa biomasse —, il
appartient à quelqu'un, se vole, se capture au combat, s'achète, se vend, se loue, se répare, se démonte. **Rien ne se
crée** : le minerai passe du trésor au véhicule et un démontage n'en rend que la moitié ; ce qu'un convoi porte sort d'un
grenier et entre dans un autre. Le monde 3D n'y touche que par des fonctions `Player*()`.

États : en chantier → à quai (cité ou ruche) → en colonne / en convoi / au joueur → épave (immobilisé, abattu) → disparu
(démonté, rouillé après 120 jours, dépouille pourrie après 40).

## Tableau : échelle × peuple × milieu

| Genre | Peuple | Milieu | Échelle | Minerai / vivres (chair) / jours | Entailles | Ce qu'il change (mesuré) |
|---|---|---|---|---|---|---|
| Char à voile | Clans du Vent | terre | moyen, 3 places | 14 / 6 / 8 | 3 | +1,2 de marche par jour en plaine et désert, −0,3 en forêt et relief |
| Chenillard à vapeur | Ligue des Fonderies | terre | grand, 8 places | 45 / 10 / 16 | 8 | lent (−0,1) ; butin d'un coup de main ×2 et il emporte aussi du minerai ; endurance ×1,1 ; siège ×1,1 |
| Chariot de guerre | Protectorat de Hael | terre | moyen, 6 places | 30 / 8 / 12 | 7 | charge : force ×1,1 ; ceux qu'il porte tombent moins (×1,2) |
| Tour de siège | Protectorat (capitale, en guerre) | terre | très grand, 12 places | 70 / 14 / 24 | 12 | ne sort que pour un siège : force ×1,6 devant des murs ; très lente |
| Chariot des greniers | Concordat d'Aram | terre | moyen, 4 places | 16 / 8 / 10 | 4 | la colonne nourrie tient : repli à 67 % de pertes au lieu de 55 % ; commerce |
| Engin rafistolé | Affranchis | terre | petit, 2 places | 10 / 4 / 6 | 2 | +0,9 en terrain ouvert, +0,6 partout ailleurs ; casse au premier choc |
| Bac | toute cité à moins de 400 m de l'eau | eau | grand, 10 places | 12 / 6 / 10 | 5 | un groupe embarqué traverse l'eau (0,8 par case d'eau) au lieu de la contourner ; commerce par l'eau |
| Ballon à vapeur | Ligue (capitale) | air | grand, 2 places | 60 / 12 / 20 | 3 | voit loin : son peuple comprend la menace 1,6 fois plus vite ; pierres lâchées : la garnison ×1,1 ; l'orage l'arrache, les volants de l'Essaim le crèvent |
| Cerf-volant porteur | Clans (capitale) | air | moyen, 2 places | 22 / 6 / 12 | 2 | idem, moins cher, plus fragile |
| Rôdeur | Essaim | terre (pattes) | petit, taille d'un cheval | 1 chitine / 18 chair / 4 | 2 | la couvée qui l'a avance de +0,8 partout |
| Porteur | Essaim | terre (pattes) | moyen, haut comme une maison | 4 / 40 / 6 | 6 | porte la couvée (≥ 20 bêtes) : +0,5 partout ; charge de chair |
| Volant | Essaim | air | moyen | 6 / 55 / 8 | 3 | autour de sa ruche (5 cases), crève ce qui vole ; un à la Reine, un pour cinq ruches |
| Colosse | Essaim | terre (six pattes) | colossal, haut comme un rempart | 30 / 400 / 30 | 30 | un seul à la fois, 4 ruches au moins, une cité à portée ; se joint à l'assaut d'une cité : force ×1,25, ×1,8 devant des murs ; ses pattes cèdent à 30 entailles |

| Cheval | toute cité (cavalerie : Clans, Protectorat) | terre | monture, 1 place | 0 / 24 (vivres) / 30 (poulain) | 2 | ne change pas le pas d'une colonne (les fantassins marchent) ; deux cavaliers ou plus : force ×1,12 ; mange 0,1 de vivres par jour à l'écurie |

### Le cheval (19/11/2026, second chantier)

- **Écurie** : chaque cité vivante de 50 habitants au moins tient 2 chevaux (messagers, vente) ; les Clans et le
  Protectorat 3 de plus (4 à la capitale) pour leur cavalerie. Au premier jour, deux sont déjà là (payés sur le grenier) ;
  ensuite un poulain à la fois, un jour sur neuf, si le grenier garde dix fois son prix. Le poulain grandit 30 jours.
- **Il mange** : à l'écurie, 0,1 de vivres par jour sur le grenier de son peuple ; en campagne et chez le joueur, il
  broute. Une cité en famine abat un cheval par jour et en mange la moitié de ce qu'il a coûté (sa « viande », `bio`).
- **Cavalerie** : une colonne (pas une ronde) des Clans ou du Protectorat emmène jusqu'à trois chevaux. Combat : deux
  entailles et il meurt (« tué sous son cavalier ») ; capturé par des humains vainqueurs ; sa carcasse reste 40 jours.
- **Le joueur** : l'acheter (12 rations × le facteur d'estime 0,8 à 1,6, comme les engins : 10 à 24 rations), le louer (un douzième, 30 jours), le
  voler (crime), le rendre ; on ne le démonte pas, on ne le répare pas (il guérit seul : une entaille en 6 jours) ; mort,
  on le dépèce : la moitié de ce qu'il a mangé, en rations (6), jamais plus. `IsMount()` dans la simulation.
- **En 3D** (`Game/SwarmMount.*`) : corps du kit « Horse Starter Kit » (six robes, tapis de selle à la couleur du peuple),
  animé par le code : allures du kit mêlées selon la vitesse (pas 0,75 → galop 12 m/s), variantes de virage, saut, se
  cabrer ; le cavalier (le mannequin du joueur, que suit son corps MetaHuman) joue les séquences de cavalier du kit,
  racine sur racine avec le cheval. Montée et descente animées des deux côtés (le kit n'a que des descentes de cavalier :
  la montée les joue à rebours). Z : trot ; Maj : galop (il s'essouffle : 12 s de galop) ; Espace : sauter (95 cm, il
  passe ce qui est bas) ; E : pied à terre. Il s'effraie de l'Essaim (16 m) et d'un front de feu (30 m) : il se cabre,
  refuse d'avancer. Les coups qui touchent le cavalier le touchent aussi ; à zéro il s'abat sur le flanc et jette le
  cavalier (12 de dégâts). Écurie hors les murs (`ASwarmVehicles::StableOrigin`) : six stalles, auges, auvent, seaux,
  barres d'attache.
- Le kit a été copié sans son dossier `ThirdPersonBP` : ses animations de cavalier UE5 sont renvoyées au squelette du
  mannequin du jeu par `[CoreRedirects]` (`Config/DefaultEngine.ini`).

Ni l'aérien ni le maritime n'existent pour l'Essaim comme bien à prendre : **ses véhicules ne se volent pas, ne se
conduisent pas** ; abattus, ils restent sur place (« dépouille », « carcasse ») et se démontent.

## Règles de la simulation

- **Chantiers** : un jour sur neuf, une cité sédentaire (ou un camp à l'arrêt) de 50 habitants au moins, sans famine,
  bâtit dans l'ordre : son véhicule terrestre (deux à la capitale), un bac si elle touche l'eau, un aérien à la
  capitale (Ligue, Clans), une tour (Protectorat en guerre). Il faut que le trésor garde 60 de minerai pour ses
  soldats après paiement. Au premier jour, chaque cité a déjà le sien (un véhicule terrestre, payé sur le trésor de départ, sans garder de réserve ce jour-là).
- **Essaim** : une ruche de plus de 20 jours y pense un jour sur 27 ; au plus un rôdeur et un porteur pour deux
  ruches, un volant à la Reine et un pour cinq ruches, un colosse à la fois ; sa biomasse doit couvrir six fois le
  prix plus la réserve d'entretien.
- **Colonnes** : une colonne (pas une ronde) emmène le meilleur véhicule terrestre de sa cité (la tour seulement
  contre une cité) et le bac si l'eau raccourcit la route d'un quart. Une couvée emmène porteur ou rôdeur ; un colosse
  d'une ruche voisine rejoint l'assaut d'une cité (« Un colosse de l'Essaim se met en marche vers une cité »).
- **Combat** : chaque jour de combat, un véhicule prend une entaille par 40 points de dégâts subis par sa colonne ;
  à zéro, il est immobilisé (roues brisées, pattes qui cèdent) et reste en épave. Colonne anéantie par des humains :
  le véhicule intact est **capturé** et suit les vainqueurs ; par l'Essaim : épave. Une colonne humaine qui passe sur
  une épave humaine la remet en état si son trésor paie 30 % du minerai (qui entre dans le véhicule).
- **Vol entre peuples** : un coup de main réussi a 35 % de chances d'emmener le véhicule terrestre de la cité visée ; il
  garde son genre (un Clan peut rouler en chenillard). Une cité prise passe ses véhicules à quai au vainqueur ; une
  cité dévorée, une ruche détruite les laissent en épaves.
- **Commerce** : chaque jour, 8 % de chances qu'un chariot des greniers, un chenillard ou un bac à quai parte vers la
  cité la plus proche d'un peuple en paix (14 cases au plus) : des vivres contre du minerai (4 pour 1) ou l'inverse,
  payés à l'arrivée sur le grenier de l'acheteur, rapportés au retour. Chaque échange rapproche les deux peuples
  (+0,2). Un convoi qui croise l'ennemi est pillé (charge et véhicule aux pillards ; dévoré par l'Essaim, sa charge
  compte dans sa récolte).
- **Aériens** : chaque jour d'orage, 25 % de chances de perdre une entaille ; un volant de l'Essaim à 5 cases, 30 %.

## Équilibre (outil sans moteur, 100 graines)

Avant les véhicules : Arche 84 %, Essaim détruit 16 %, monde mort 0 %, 87 % des parties entre 600 et 1000 jours.
Après : Arche 81 %, Essaim détruit 19 %, monde mort 0 %, 83 % entre 600 et 1000 jours (après fusion avec la réserve et
le bétail des ruches de main, 19/11 15 h : Arche 83 %, Essaim détruit 17 %, monde mort 0 %, 83 %). Après fusion des sociétés
et guerres (main du 19/11 16 h), sans chevaux : Arche 81 %, Essaim détruit 19 % ; avec les chevaux (foin 0,25, charge ×1,05) :
85 / 15 ; réglés (foin 0,1, charge ×1,12) : **Arche 83 %, Essaim détruit 17 %, monde mort 0 %** ; environ 90 chevaux
élevés par partie. Par partie, en moyenne :
une trentaine de véhicules humains bâtis, une quarantaine de porteurs et cinquante rôdeurs (ils meurent avec leurs
couvées), un colosse, une trentaine d'échanges par convoi, deux à trois véhicules qui changent de mains.

## Vehicules 3 (19/11/2026, soir) : ce qui fait avancer chaque genre

Demande du propriétaire : « certains véhicules qui nécessitent normalement des chevaux, comme le char de guerre,
fonctionnent sans rien ; soit tu le supprimes, soit tu es logique et tu fais tout bien ». Revue des genres, avec la
question « qu'est-ce qui le fait avancer, et est-ce visible et compté ? » :

| Genre | Ce qui le fait avancer | Compté dans la simulation | Visible en 3D |
|---|---|---|---|
| Chariot de guerre, chariot des greniers | **deux chevaux attelés** (`TeamOf` = 2) | chevaux de l'écurie (`hitchedTo`), élevés sur les vivres ; l'écurie en élève deux de plus par chariot ; une colonne ou un convoi ne part qu'attelé de tout l'attelage ; un cheval tué libère sa place, le chariot sans cheval reste en épave (« son attelage est tombé ») ; une colonne ne le relève qu'avec un cheval libre | deux chevaux devant le timon (perche et palonnier), au pas, au trot, au galop selon la vitesse du chariot ; sans cheval, il ne part pas et le dit ; un seul cheval : 60 % de la vitesse |
| Tour de siège | **douze hommes qui la poussent** (`CrewOf`) | une colonne de moins de douze soldats ne la prend pas ; ni vendue ni volable | le joueur seul ne la bouge pas (message) |
| Chenillard, ballon | **vapeur** : naphte (`fuel`, `FuelPerDay` 0,6 / 0,4) | le plein se fait au départ sur le trésor de son peuple (minerai distillé) ; brûlé chaque jour de route ; à sec, il s'arrête (épave « à court de naphte ») | fumée ; le joueur brûle son réservoir en roulant |
| Buggy de raid (ancien « engin rafistolé »), pick-up à tourelle, moto, camion-bélier, autogire, dirigeable | **moteur à naphte** (0,3 / 0,35 / 0,1 / 0,5 / 0,3 / 1,0 par jour) | idem ; l'autogire et le dirigeable à quai font la ronde au-dessus de leur cité (la moitié d'un jour de route brûlée par jour, refaite sur le trésor) | bruit de moteur calculé (`SwarmEngineSound`), régime, poussière, trace dans l'herbe ; plein au marché (« Faire le plein ») |
| Char à voile, cerf-volant | le vent | rien à brûler | voile qui se creuse |
| Bac | les perches de son équipage (et sa voile) | rien à brûler | conduit sur l'eau seulement |
| Porteur, rôdeur, volant, colosse ; cheval | ce sont des bêtes | leur chair | leurs pattes, leurs allures |

Les cinq moteurs nouveaux (`GunTruck`, `Bike`, `RamTruck`, `Autogyro`, `Airship`, genres 14 à 18) : chaque cité des
Affranchis arme un pick-up à tourelle (après son buggy), puis sa moto et son autogire ; la capitale des Fonderies
bâtit un camion-bélier (siège ×1,4) puis, riche (minerai > 200), un dirigeable de guerre. Les coups de main des
Affranchis partent avec leurs engins : ce sont les pillards motorisés du monde (pas un cercle à part : faute de temps).
Collision : un engin lancé renverse ce qu'il percute, dégâts ∝ (vitesse/30 km/h)² × masse.
Équilibre (100 graines) : **Arche 84 %, Essaim détruit 16 %, monde mort 0 %, 86 % des parties entre 600 et 1000 jours**
(référence avant : 83 / 17 / 0 / 86 %) ; environ 105 chevaux élevés par partie (90 avant : les attelages), 0,9 pick-up,
1,5 moto, 2,2 camions-béliers, 0,6 autogire, 0,7 dirigeable par partie. Sans les nouveaux moteurs, attelages et naphte
seuls donnaient 83 / 17 ; les moteurs coûtent du minerai aux peuples, d'où leur force (pick-up ×1,22, camion ×1,18, siège
×1,4) réglée pour revenir à l'équilibre.

Modèles (19/11/2026 au soir) : les pièces « jouet » générées sont remplacées, quand le modèle Meshy est importé, par
`SwarmMeshyOverride` (`Game/SwarmMotorParts.cpp`) : chenillard (locomotive à chenilles de 8 m, sans l'agrandissement
×1,45), chariot de guerre et chariot des greniers (caisse levée, roues de bois cerclées de fer qui tournent), buggy de
raid (roues à crampons), ballon (enveloppe et nacelle), cerf-volant, tour de siège ; les cinq moteurs ont leurs propres
pièces. Les échelles sont mesurées sur le maillage (`Scale` nul = « à la taille voulue »). Le char à voile garde ses
pièces générées (le modèle Meshy est un squelette frêle). Sans les modèles, tout retombe sur les pièces générées.

### Tourelle, conduite, et pour les autres chantiers

- **Tourelle** (pick-up) : touche « changer d'arme » (F) au volant : on passe à la tourelle (le véhicule roule sur son
  élan), elle suit la visée à 180°/s, bute à ±150° ; clic gauche : rafale à 9 coups/s, une cartouche du sac par coup,
  flamme, douille, recul de la caisse ; les coups touchent comme ceux du fusil (`FireAlong`, du bout du canon). Les PNJ
  ne s'en servent PAS encore.
- **Conduite des moteurs** (`MotorStep`) : couple qui s'essouffle près de la vitesse de pointe, frottement, frein ;
  glissade latérale quand le virage dépasse l'adhérence du sol (sable 0,55, herbe 0,75, roche 0,9 ; frein + volant :
  l'arrière décroche) ; tangage et roulis de suspension (la moto se couche jusqu'à 35°) ; régime à trois rapports ;
  bruit calculé (`USwarmEngineSound`) ; poussière aux roues, herbe couchée. Percuter : dégâts ∝ (vitesse / 30 km/h)² × masse.
- **Pour le chantier « armées »** : `ASwarmVehicleActor::FollowRoute(Points, Vitesse, Décalage, bBoucle)` fait rouler
  (ou voler, à son altitude de croisière) un engin conduit par un PNJ le long d'un trajet, décalé de sa place dans une
  formation (X en avant, Y à droite) ; il ralentit dans les virages, s'arrête contre un obstacle ; `IsOnRoute()`,
  `GetRouteIndex()`. Un trajet vide rend la main à la simulation. `AimTurret(Point)` pointe une tourelle (pas de tir PNJ
  encore : il faudrait une porte de dégâts « au nom d'un peuple » dans `ASwarmCrowd`).

## Le joueur (fonctions `Player*()`)

| Action | Ce que ça coûte / rapporte |
|---|---|
| Acheter (`PlayerBuyVehicle`) | prix en rations = minerai + travail, ×(1,4 − estime/125) borné 0,8–1,6, ×0,8 pour un membre, ×1,25 si c'est le dernier de son milieu dans ce peuple. Le prix entre au grenier. |
| Louer (`PlayerHireVehicle`) | un douzième du prix, pour 30 jours ; non rendu à temps, c'est un vol (−9 d'estime). |
| Vendre (`PlayerSellVehicle`) | 60 % de sa valeur (selon son état), au plus un dixième du grenier ; un peuple ne rachète pas ce qu'on lui a volé. |
| Voler (`PlayerStealVehicle`) | dans les murs : alerte (sommation, garnison, chasseurs : la loi des cités) et −15 d'estime ; sur la route, −9. |
| Rendre (`PlayerReturnVehicle`) | rend la moitié de l'estime perdue au volé ; donner un véhicule à un autre peuple est un don. |
| Faire réparer (`PlayerRepairVehicle`) | rations aux forgerons ; leur minerai entre dans le véhicule ; une épave près de leurs murs devient au joueur. |
| Démonter (`PlayerDismantleVehicle`) | la moitié du minerai qu'il contient (en cartouches), le reste est perdu. |
| Démonter une dépouille (`PlayerDismantleRemains`) | la moitié de sa chair en rations, la moitié de sa chitine : jamais plus que son coût. |
| Piller un convoi (`PlayerPlunderVehicle`) | sa charge de vivres en rations ; crime envers son peuple. |
| Abattre (`PlayerDamageVehicle`) | une entaille par coup porté ; un porteur tombe en six. |
| Atteler / dételer (`PlayerHitchHorse`, `PlayerUnhitchHorse`) | un de ses chevaux à un de ses chariots (deux places) ; un chariot s'achète, se loue, se vole avec son attelage. |
| Faire le plein (`PlayerBuyFuel`) | le minerai du trésor de la cité passe au réservoir ; rations au grenier (×1,2 chez les Fonderies, ×1,6 ailleurs). La route le brûle (`PlayerBurnFuel`). |

## En 3D (ordre de réalisation)

1. Présence : véhicules à quai à côté de la porte de leur cité, dans les colonnes en marche, porteur et rôdeur près des
   ruches et dans les couvées, colosse en marche, épaves et dépouilles là où elles sont tombées.
2. Monter, conduire (ZQSD, frein), descendre (E) un véhicule terrestre humain ; caméra reculée ; compagnons qui suivent.
3. Vol, achat, démontage, invites (« Monter », « Voler ce char — crime », « Démonter »), aide (H), journal.
4. Bac pour traverser ; aérien (monter, voir loin) ; colosse (points faibles, monter dessus) : après.

L'état réel de chaque point est dans le rapport du chantier (ce qui est jouable, ce qui n'existe que dans la
simulation, ce qui n'est que décor).

### Second chantier (19/11/2026, après-midi) — ce qui a changé en 3D

- Le conducteur d'un engin est **assis** (pose de repos du cavalier du kit de chevaux), bassin sur le siège : il ne se
  tient plus debout sur le banc, la tête dans le toit.
- Coup de barre : un cheval perd 25 de vie ; un engin prend une entaille tous les trois coups ; un marcheur de l'Essaim
  se brise **patte par patte** (deux coups par entaille de sa part ; la patte cède et rend sa part d'un coup ; le corps
  s'affaisse et penche de ce côté ; la carapace du colosse ne prend rien, son cœur trois). Les balles suivent la même règle.
- Ballon et cerf-volant : amarrés à quai, nacelle au sol ; menés, ils montent à 45 m (au-dessus des murs), se posent
  deux secondes après qu'on les lâche ; on ne descend qu'au sol ; l'orage en vol en arrache une entaille toutes les 20 s.
- Le volant de l'Essaim tourne en orbe au-dessus de sa ruche (32 m de rayon, 23 à 41 m de haut).
- Le chenillard est agrandi de moitié (échelle 1,45 : 9,6 m de long, 4,2 m de haut).
- Cavalerie : chaque cheval d'une colonne en marche porte un soldat de la foule (`ASwarmCrowd::SetUnitSaddle`, pose
  poussée chaque image), sur les flancs de la formation.
