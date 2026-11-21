# Modèles générés avec Meshy

Généré par `python Tools/Assets/meshy_doc.py` (ne pas retoucher à la main : relancer). Chaîne complète et règles
en fin de page. Repère de tous les modèles : X devant, Y à droite, Z en haut, centimètres, échelle réelle.

## Compte des crédits

Dépensés : **2764 crédits** (plafond de la mission : 2 800, la clé étant limitée à 3 000 ; aperçu 20, affinage 10, image de référence 6, image -> 3D texturé 30 ; une tâche ratée chez Meshy est rendue).

- solde lu le 2026-11-19 17:10 : 4405 crédits (dépensés alors : 100)
- solde lu le 2026-11-19 17:19 : 3869 crédits (dépensés alors : 636)
- solde lu le 2026-11-19 17:26 : 3069 crédits (dépensés alors : 1436)
- solde lu le 2026-11-19 17:31 : 2763 crédits (dépensés alors : 1742)
- solde lu le 2026-11-19 17:47 : 1849 crédits (dépensés alors : 2656)
- solde lu le 2026-11-19 18:54 : 1741 crédits (dépensés alors : 2764)

| Modèle | Appels | Crédits |
|---|---|---|
| ArmoredTruck_Body | image, image3d | 36 |
| ArmoredTruck_Turret | image, image3d | 36 |
| Autogyro | image, image3d | 36 |
| Autogyro_Rotor | image, image3d | 36 |
| Balloon_Envelope | preview, refine | 30 |
| Balloon_Gondola | preview, refine | 30 |
| Barge_Body | preview, image, image3d | 56 |
| Buggy_Body | preview, image, image3d | 56 |
| CampTent | preview, refine | 30 |
| Campfire | preview, refine, image, image3d | 66 |
| Cangue | preview, refine | 30 |
| CaptiveStage | preview, refine | 30 |
| Carrier_Body | image, image3d | 36 |
| Chains | preview, refine | 30 |
| ChoppingBlock | preview, refine | 30 |
| Colossus_Body | preview, preview, image, image3d | 76 |
| Colossus_LegLower | preview, preview, refine | 50 |
| Colossus_LegUpper | preview, preview, refine | 50 |
| Crawler_Body | preview, refine | 30 |
| Drifter_Body | image, image3d | 36 |
| Drifter_Wing | preview, image, image3d | 56 |
| Flesh_BroodPouch | preview, refine | 30 |
| Flesh_CocoonPen | preview, refine | 30 |
| Flesh_HiveHeart | preview, refine | 30 |
| Flesh_Mound | preview, refine | 30 |
| Flesh_MucusPool | preview, refine | 30 |
| Flesh_RelayPylon | preview, refine | 30 |
| Flesh_RibArch | preview, preview, refine | 50 |
| Flesh_SphincterGate | preview, image, image3d | 56 |
| Flesh_StorageSac | preview, refine | 30 |
| Flesh_TendonPillar | preview, refine | 30 |
| Fountain | preview, refine, image, image3d | 66 |
| Gallows | preview, refine | 30 |
| Giant_BrokenStatue | image, image3d | 36 |
| Giant_BrokenViaduct | image, image3d | 36 |
| Giant_CrackedDam | image, image3d | 36 |
| Giant_DeadColossus | image, image3d | 36 |
| Giant_FactoryCathedral | image, image3d | 36 |
| Giant_RadioTelescope | image, image3d | 36 |
| Giant_Shipwreck | image, image3d | 36 |
| Glider_Kite | preview, refine | 30 |
| Handcart | preview, refine | 30 |
| HangingCage | preview, refine | 30 |
| HitchingPost | preview, refine | 30 |
| JailCage | preview, image, image3d | 56 |
| Manger | preview, image, image3d | 56 |
| MarketStall | preview, refine | 30 |
| Monument_Affranchis | image, image3d | 36 |
| Monument_Clans | image, image3d | 36 |
| Monument_Concordat | image, image3d | 36 |
| Monument_Fonderies | image, image3d | 36 |
| Monument_Protectorat | image, image3d | 36 |
| Pillory | preview, refine | 30 |
| Propeller | image, image3d | 36 |
| RaidBuggy_Body | image, image3d | 36 |
| RamTruck_Body | image, image3d | 36 |
| SailCart_Body | preview, image, image3d | 56 |
| SailCart_Sail | preview, refine | 30 |
| SailCart_SailFurled | preview | 20 |
| ScrapBike | image, image3d | 36 |
| SiegeTower_Body | preview, refine | 30 |
| Stalker_Body | image, image3d | 36 |
| Stalker_LegLower | preview | 20 |
| Stalker_LegUpper | preview | 20 |
| SupplyWagon_Body | preview, image, image3d | 56 |
| TavernSign | preview, refine, image | 36 |
| WarAirship_Gondola | image, image3d | 36 |
| WarWagon_Body | preview, image, image3d | 56 |
| WaterTrough | preview, refine | 30 |
| Well | preview, refine, image, image3d | 66 |
| Wheel_IronSpoke | preview, refine | 30 |
| Wheel_Tire | image, image3d | 36 |
| Wheel_WoodIron | preview, refine | 30 |

## Coût mesuré (test Swarm.Modeles)

- 68 modeles, 404.5 Mo de memoire video en tout (maillages et textures, tous niveaux)
- Swarm.Modeles.Budget : Success
- Niveaux de détail : Nanite sur tous les modèles (aucun ne se déforme, aucun n'est translucide). Les quatre niveaux classiques ne se fabriquent pas dans l'éditeur sans fenêtre (le sous-système des maillages y est absent : set_lods rend -1) ; un modèle au-dessus du plafond de sa classe est rogné par Nanite (KeepPercentTriangles).
- Temps GPU de la galerie entière : pas encore mesuré (deux lancements tués par le garde-fou de mémoire le 19/11 à 18 h 46).

## Marcheurs assemblés en jeu (ASwarmModelGallery::BuildWalker)

![MOD_marcheur_Carrier](Captures/Meshy/MOD_marcheur_Carrier.jpg)

![MOD_marcheur_Colossus](Captures/Meshy/MOD_marcheur_Colossus.jpg)


## Modèles livrés

### Crawler_Body

![Crawler_Body](Captures/Meshy/Crawler_Body.jpg)

- asset : `/Game/Swarm/Meshy/Crawler_Body/SM_Crawler_Body` (matière `MI_Crawler_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 800 x 294 x 381 cm (X x Y x Z) ; boîte X -400..400, Y -147..147, Z 0..381
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 35023 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Chenillard a vapeur de la Ligue des Fonderies (genre 1, Crawler).
- à brancher dans : Source/Swarm/Game/SwarmVehicleParts.cpp, GCrawlerParts (Body) ; Tools/Assets/make_vehicles.py

### Flesh_RibArch

![Flesh_RibArch](Captures/Meshy/Flesh_RibArch.jpg)

- asset : `/Game/Swarm/Meshy/Flesh_RibArch/SM_Flesh_RibArch` (matière `MI_Flesh_RibArch`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 561 x 846 x 800 cm (X x Y x Z) ; boîte X -281..281, Y -423..423, Z 0..800
- pivot : au sol, au centre ; avant : +X
- classe `chair` : 9290 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 50
- sert a : Arche de cotes : entree et galeries d'une ruche de l'Essaim.
- à brancher dans : Source/Swarm/Game/SwarmHiveInside.cpp et SwarmFlesh.cpp (pieces de chair)

### Colossus_Body

![Colossus_Body](Captures/Meshy/Colossus_Body.jpg)

- asset : `/Game/Swarm/Meshy/Colossus_Body/SM_Colossus_Body` (matière `MI_Colossus_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 1900 x 736 x 834 cm (X x Y x Z) ; boîte X -950..950, Y -368..368, Z 0..834
- pivot : au sol, au centre ; avant : +X
- classe `colosse` : 87300 triangles (plafond 90000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 76
- sert a : Colosse de l'Essaim, corps (genre 12, Colossus). Six pattes : Colossus_LegUpper + Colossus_LegLower.
- à brancher dans : Source/Swarm/Game/SwarmVehicleParts.cpp, GColossusParts (Body) ; film d'ouverture SwarmCinematic.cpp

### Colossus_LegUpper

![Colossus_LegUpper](Captures/Meshy/Colossus_LegUpper.jpg)

- asset : `/Game/Swarm/Meshy/Colossus_LegUpper/SM_Colossus_LegUpper` (matière `MI_Colossus_LegUpper`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 145 x 137 x 760 cm (X x Y x Z) ; boîte X -72..72, Y -68..68, Z -760..0
- pivot : en haut (articulation), pend vers -Z ; avant : +X
- classe `patte` : 10480 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 50
- sert a : Cuisse du colosse : pivot a la hanche, pend vers -Z, 700 cm utiles (longueur de la table) + 60 de recouvrement.
- à brancher dans : SwarmVehicleParts.cpp, GColossusParts (LegUpper, Radius 700)

### Colossus_LegLower

![Colossus_LegLower](Captures/Meshy/Colossus_LegLower.jpg)

- asset : `/Game/Swarm/Meshy/Colossus_LegLower/SM_Colossus_LegLower` (matière `MI_Colossus_LegLower`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 404 x 399 x 940 cm (X x Y x Z) ; boîte X -202..202, Y -200..200, Z -940..0
- pivot : en haut (articulation), pend vers -Z ; avant : +X
- classe `patte` : 10323 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 50
- sert a : Tibia du colosse a griffe : pivot au genou, pend vers -Z, 900 cm utiles + 40 de recouvrement.
- à brancher dans : SwarmVehicleParts.cpp, GColossusParts (LegLower, Radius 900)

### Carrier_Body

![Carrier_Body](Captures/Meshy/Carrier_Body.jpg)

- asset : `/Game/Swarm/Meshy/Carrier_Body/SM_Carrier_Body` (matière `MI_Carrier_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 1100 x 498 x 612 cm (X x Y x Z) ; boîte X -550..550, Y -249..249, Z 0..612
- pivot : au sol, au centre ; avant : +X
- classe `porteur` : 36130 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Porteur de l'Essaim, corps (genre 8, Carrier). Pattes : Colossus_LegUpper / LegLower a l'echelle 0,39 (voir doc).
- à brancher dans : SwarmVehicleParts.cpp, GCarrierParts (Body)

### Stalker_Body

![Stalker_Body](Captures/Meshy/Stalker_Body.jpg)

- asset : `/Game/Swarm/Meshy/Stalker_Body/SM_Stalker_Body` (matière `MI_Stalker_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 340 x 131 x 110 cm (X x Y x Z) ; boîte X -170..170, Y -65..65, Z 0..110
- pivot : au sol, au centre ; avant : +X
- classe `rodeur` : 9418 triangles (plafond 15000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Rodeur de l'Essaim, corps (genre 11, Stalker). Pattes : Colossus_LegUpper / LegLower a leur echelle (95 et 120 cm utiles).
- à brancher dans : SwarmVehicleParts.cpp, GStalkerParts (Body)

### Drifter_Body

![Drifter_Body](Captures/Meshy/Drifter_Body.jpg)

- asset : `/Game/Swarm/Meshy/Drifter_Body/SM_Drifter_Body` (matière `MI_Drifter_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 520 x 125 x 210 cm (X x Y x Z) ; boîte X -260..260, Y -63..63, Z 0..210
- pivot : au sol, au centre ; avant : +X
- classe `rodeur` : 12551 triangles (plafond 15000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Volant de l'Essaim, corps (genre 9, Drifter) ; ailes : Drifter_Wing.
- à brancher dans : GDrifterParts (Body)

### Drifter_Wing

![Drifter_Wing](Captures/Meshy/Drifter_Wing.jpg)

- asset : `/Game/Swarm/Meshy/Drifter_Wing/SM_Drifter_Wing` (matière `MI_Drifter_Wing`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 297 x 560 x 77 cm (X x Y x Z) ; boîte X -149..149, Y 0..560, Z -39..39
- pivot : à l'emplanture (bord -Y), s'étend vers +Y ; avant : +X
- classe `patte` : 6222 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 56
- sert a : Aile droite du volant : pivot a l'emplanture, s'etend vers +Y (echelle Y -1 pour la gauche).
- à brancher dans : GDrifterParts (Wing)

### Flesh_SphincterGate

![Flesh_SphincterGate](Captures/Meshy/Flesh_SphincterGate.jpg)

- asset : `/Game/Swarm/Meshy/Flesh_SphincterGate/SM_Flesh_SphincterGate` (matière `MI_Flesh_SphincterGate`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 105 x 499 x 500 cm (X x Y x Z) ; boîte X -53..53, Y -249..249, Z 0..500
- pivot : au sol, au centre ; avant : +X
- classe `chair` : 9231 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 56
- sert a : Porte-sphincter : entrees des ruches et salles.
- à brancher dans : SwarmHiveInside.cpp / SwarmFlesh.cpp

### Flesh_TendonPillar

![Flesh_TendonPillar](Captures/Meshy/Flesh_TendonPillar.jpg)

- asset : `/Game/Swarm/Meshy/Flesh_TendonPillar/SM_Flesh_TendonPillar` (matière `MI_Flesh_TendonPillar`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 385 x 397 x 900 cm (X x Y x Z) ; boîte X -193..193, Y -198..198, Z 0..900
- pivot : au sol, au centre ; avant : +X
- classe `chair` : 8276 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Pilier de tendons : soutient les voutes des ruches.
- à brancher dans : SwarmHiveInside.cpp / SwarmFlesh.cpp

### Flesh_BroodPouch

![Flesh_BroodPouch](Captures/Meshy/Flesh_BroodPouch.jpg)

- asset : `/Game/Swarm/Meshy/Flesh_BroodPouch/SM_Flesh_BroodPouch` (matière `MI_Flesh_BroodPouch`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 268 x 358 x 250 cm (X x Y x Z) ; boîte X -134..134, Y -179..179, Z 0..250
- pivot : au sol, au centre ; avant : +X
- classe `chair_translucide` : 7243 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Poches de couvain (lueur interne) : salles de couvee.
- à brancher dans : SwarmHiveInside.cpp / SwarmFlesh.cpp

### Flesh_StorageSac

![Flesh_StorageSac](Captures/Meshy/Flesh_StorageSac.jpg)

- asset : `/Game/Swarm/Meshy/Flesh_StorageSac/SM_Flesh_StorageSac` (matière `MI_Flesh_StorageSac`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 309 x 440 x 320 cm (X x Y x Z) ; boîte X -155..155, Y -220..220, Z 0..320
- pivot : au sol, au centre ; avant : +X
- classe `chair` : 8345 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Sac de reserve : reserves de biomasse de la ruche.
- à brancher dans : SwarmFlesh.cpp

### Flesh_HiveHeart

![Flesh_HiveHeart](Captures/Meshy/Flesh_HiveHeart.jpg)

- asset : `/Game/Swarm/Meshy/Flesh_HiveHeart/SM_Flesh_HiveHeart` (matière `MI_Flesh_HiveHeart`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 434 x 456 x 650 cm (X x Y x Z) ; boîte X -217..217, Y -228..228, Z 0..650
- pivot : au sol, au centre ; avant : +X
- classe `chair` : 11640 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Coeur de la ruche : la salle du fond, point a detruire.
- à brancher dans : SwarmHiveInside.cpp

### Flesh_CocoonPen

![Flesh_CocoonPen](Captures/Meshy/Flesh_CocoonPen.jpg)

- asset : `/Game/Swarm/Meshy/Flesh_CocoonPen/SM_Flesh_CocoonPen` (matière `MI_Flesh_CocoonPen`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 600 x 617 x 403 cm (X x Y x Z) ; boîte X -300..300, Y -308..308, Z 0..403
- pivot : au sol, au centre ; avant : +X
- classe `chair` : 10139 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Enclos a cocons : betail et prisonniers de l'Essaim.
- à brancher dans : SwarmFlesh.cpp (betail des ruches)

### Flesh_RelayPylon

![Flesh_RelayPylon](Captures/Meshy/Flesh_RelayPylon.jpg)

- asset : `/Game/Swarm/Meshy/Flesh_RelayPylon/SM_Flesh_RelayPylon` (matière `MI_Flesh_RelayPylon`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 577 x 652 x 1200 cm (X x Y x Z) ; boîte X -289..289, Y -326..326, Z 0..1200
- pivot : au sol, au centre ; avant : +X
- classe `chair` : 7058 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Pylone de relais : reseau entre ruches.
- à brancher dans : SwarmFlesh.cpp

### Flesh_Mound

![Flesh_Mound](Captures/Meshy/Flesh_Mound.jpg)

- asset : `/Game/Swarm/Meshy/Flesh_Mound/SM_Flesh_Mound` (matière `MI_Flesh_Mound`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 600 x 645 x 385 cm (X x Y x Z) ; boîte X -300..300, Y -322..322, Z 0..385
- pivot : au sol, au centre ; avant : +X
- classe `chair` : 6270 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Monticule de chair : couvre le sol autour des ruches.
- à brancher dans : SwarmFlesh.cpp

### Flesh_MucusPool

![Flesh_MucusPool](Captures/Meshy/Flesh_MucusPool.jpg)

- asset : `/Game/Swarm/Meshy/Flesh_MucusPool/SM_Flesh_MucusPool` (matière `MI_Flesh_MucusPool`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 500 x 500 x 25 cm (X x Y x Z) ; boîte X -250..250, Y -250..250, Z 0..25
- pivot : au sol, au centre ; avant : +X
- classe `chair` : 3954 triangles (plafond 12000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Nappe de mucus a bords : sol des galeries.
- à brancher dans : SwarmFlesh.cpp / SwarmHiveInside.cpp

### WarWagon_Body

![WarWagon_Body](Captures/Meshy/WarWagon_Body.jpg)

- asset : `/Game/Swarm/Meshy/WarWagon_Body/SM_WarWagon_Body` (matière `MI_WarWagon_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 448 x 420 x 253 cm (X x Y x Z) ; boîte X -224..224, Y -210..210, Z 0..253
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 25746 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 56
- sert a : Chariot de guerre du Protectorat (genre 2, WarWagon).
- à brancher dans : GWarWagonParts (Body)

### SiegeTower_Body

![SiegeTower_Body](Captures/Meshy/SiegeTower_Body.jpg)

- asset : `/Game/Swarm/Meshy/SiegeTower_Body/SM_SiegeTower_Body` (matière `MI_SiegeTower_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 625 x 651 x 1600 cm (X x Y x Z) ; boîte X -312..312, Y -325..325, Z 0..1600
- pivot : au sol, au centre ; avant : +X
- classe `engin_grand` : 40589 triangles (plafond 50000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Tour de siege (genre 10, SiegeTower) ; le pont-levis reste la piece SM_Veh_SiegeTower_Bridge.
- à brancher dans : GSiegeTowerParts (Body)

### SailCart_Body

![SailCart_Body](Captures/Meshy/SailCart_Body.jpg)

- asset : `/Game/Swarm/Meshy/SailCart_Body/SM_SailCart_Body` (matière `MI_SailCart_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 600 x 279 x 281 cm (X x Y x Z) ; boîte X -300..300, Y -140..140, Z 0..281
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 25013 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 56
- sert a : Char a voile des Clans (genre 0, SailCart) ; voile : SailCart_Sail.
- à brancher dans : GSailCartParts (Body)

### SailCart_Sail

![SailCart_Sail](Captures/Meshy/SailCart_Sail.jpg)

- asset : `/Game/Swarm/Meshy/SailCart_Sail/SM_SailCart_Sail` (matière `MI_SailCart_Sail`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 637 x 55 x 520 cm (X x Y x Z) ; boîte X -318..318, Y -28..28, Z 0..520
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 3013 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Voile deployee du char a voile : pivot au pied, sous le bord bas (a raccorder au mat).
- à brancher dans : GSailCartParts (Sail)

### SupplyWagon_Body

![SupplyWagon_Body](Captures/Meshy/SupplyWagon_Body.jpg)

- asset : `/Game/Swarm/Meshy/SupplyWagon_Body/SM_SupplyWagon_Body` (matière `MI_SupplyWagon_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 720 x 342 x 402 cm (X x Y x Z) ; boîte X -360..360, Y -171..171, Z 0..402
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 22935 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 56
- sert a : Chariot des greniers du Concordat (genre 3, SupplyWagon).
- à brancher dans : GSupplyWagonParts (Body)

### Buggy_Body

![Buggy_Body](Captures/Meshy/Buggy_Body.jpg)

- asset : `/Game/Swarm/Meshy/Buggy_Body/SM_Buggy_Body` (matière `MI_Buggy_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 420 x 295 x 322 cm (X x Y x Z) ; boîte X -210..210, Y -148..148, Z 0..322
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 19169 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 56
- sert a : Engin rafistole des Affranchis (genre 4, Buggy).
- à brancher dans : GBuggyParts (Body)

### Barge_Body

![Barge_Body](Captures/Meshy/Barge_Body.jpg)

- asset : `/Game/Swarm/Meshy/Barge_Body/SM_Barge_Body` (matière `MI_Barge_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 920 x 455 x 175 cm (X x Y x Z) ; boîte X -460..460, Y -227..227, Z 0..175
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 25405 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 56
- sert a : Bac (genre 5, Barge) : pivot au fond de la coque (la table le pose a la ligne de flottaison).
- à brancher dans : GBargeParts (Body)

### Balloon_Envelope

![Balloon_Envelope](Captures/Meshy/Balloon_Envelope.jpg)

- asset : `/Game/Swarm/Meshy/Balloon_Envelope/SM_Balloon_Envelope` (matière `MI_Balloon_Envelope`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 1440 x 402 x 734 cm (X x Y x Z) ; boîte X -720..720, Y -201..201, Z -367..367
- pivot : au centre (moyeu / centre) ; avant : +X
- classe `engin_grand` : 30664 triangles (plafond 50000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Ballon a vapeur, enveloppe (genre 6, Balloon) : pivot a son centre.
- à brancher dans : GBalloonParts (Envelope)

### Balloon_Gondola

![Balloon_Gondola](Captures/Meshy/Balloon_Gondola.jpg)

- asset : `/Game/Swarm/Meshy/Balloon_Gondola/SM_Balloon_Gondola` (matière `MI_Balloon_Gondola`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 481 x 182 x 320 cm (X x Y x Z) ; boîte X -240..240, Y -91..91, Z 0..320
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 19699 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Ballon a vapeur, nacelle (Body ; a poser sous l'enveloppe, Z -630).
- à brancher dans : GBalloonParts (Body)

### Glider_Kite

![Glider_Kite](Captures/Meshy/Glider_Kite.jpg)

- asset : `/Game/Swarm/Meshy/Glider_Kite/SM_Glider_Kite` (matière `MI_Glider_Kite`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 617 x 1240 x 721 cm (X x Y x Z) ; boîte X -309..309, Y -620..620, Z 0..721
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 20600 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Cerf-volant porteur des Clans (genre 7, Glider) : aile entiere (Wing).
- à brancher dans : GGliderParts (Wing)

### Wheel_WoodIron

![Wheel_WoodIron](Captures/Meshy/Wheel_WoodIron.jpg)

- asset : `/Game/Swarm/Meshy/Wheel_WoodIron/SM_Wheel_WoodIron` (matière `MI_Wheel_WoodIron`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 160 x 61 x 160 cm (X x Y x Z) ; boîte X -80..80, Y -31..31, Z -80..80
- pivot : au centre (moyeu / centre) ; avant : +X
- classe `roue` : 4111 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Roue de bois cerclee de fer (chariots, tour, char a voile) : pivot au moyeu, axe Y.
- à brancher dans : Wheel des genres 0, 2, 3, 10

### Wheel_IronSpoke

![Wheel_IronSpoke](Captures/Meshy/Wheel_IronSpoke.jpg)

- asset : `/Game/Swarm/Meshy/Wheel_IronSpoke/SM_Wheel_IronSpoke` (matière `MI_Wheel_IronSpoke`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 100 x 22 x 100 cm (X x Y x Z) ; boîte X -50..50, Y -11..11, Z -50..50
- pivot : au centre (moyeu / centre) ; avant : +X
- classe `roue` : 4082 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Roue de fer a rayons (chenillard, engin rafistole) : pivot au moyeu, axe Y.
- à brancher dans : Wheel des genres 1, 4

### Gallows

![Gallows](Captures/Meshy/Gallows.jpg)

- asset : `/Game/Swarm/Meshy/Gallows/SM_Gallows` (matière `MI_Gallows`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 151 x 236 x 450 cm (X x Y x Z) ; boîte X -76..76, Y -118..118, Z 0..450
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 6614 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Gibet a potence : place des cites, justice.
- à brancher dans : SwarmSociety.cpp

### Pillory

![Pillory](Captures/Meshy/Pillory.jpg)

- asset : `/Game/Swarm/Meshy/Pillory/SM_Pillory` (matière `MI_Pillory`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 98 x 105 x 210 cm (X x Y x Z) ; boîte X -49..49, Y -53..53, Z 0..210
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 3380 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Pilori.
- à brancher dans : SwarmSociety.cpp

### HangingCage

![HangingCage](Captures/Meshy/HangingCage.jpg)

- asset : `/Game/Swarm/Meshy/HangingCage/SM_HangingCage` (matière `MI_HangingCage`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 82 x 82 x 230 cm (X x Y x Z) ; boîte X -41..41, Y -41..41, Z -230..0
- pivot : en haut (articulation), pend vers -Z ; avant : +X
- classe `mobilier` : 4983 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Cage de fer suspendue : pivot en haut (accroche de la chaine).
- à brancher dans : SwarmSociety.cpp

### Cangue

![Cangue](Captures/Meshy/Cangue.jpg)

- asset : `/Game/Swarm/Meshy/Cangue/SM_Cangue` (matière `MI_Cangue`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 12 x 82 x 90 cm (X x Y x Z) ; boîte X -6..6, Y -41..41, Z 0..90
- pivot : au sol, au centre ; avant : +X
- classe `petit` : 1170 triangles (plafond 2000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 512 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Carcan (porte par un prisonnier).
- à brancher dans : SwarmSociety.cpp

### ChoppingBlock

![ChoppingBlock](Captures/Meshy/ChoppingBlock.jpg)

- asset : `/Game/Swarm/Meshy/ChoppingBlock/SM_ChoppingBlock` (matière `MI_ChoppingBlock`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 89 x 90 x 51 cm (X x Y x Z) ; boîte X -45..45, Y -45..45, Z 0..51
- pivot : au sol, au centre ; avant : +X
- classe `petit` : 1739 triangles (plafond 2000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 512 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Billot.
- à brancher dans : SwarmSociety.cpp

### Chains

![Chains](Captures/Meshy/Chains.jpg)

- asset : `/Game/Swarm/Meshy/Chains/SM_Chains` (matière `MI_Chains`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 21 x 150 x 105 cm (X x Y x Z) ; boîte X -11..11, Y -75..75, Z 0..105
- pivot : au sol, au centre ; avant : +X
- classe `petit` : 1542 triangles (plafond 2000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 512 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Chaines et fers (un troncon).
- à brancher dans : SwarmSociety.cpp

### CaptiveStage

![CaptiveStage](Captures/Meshy/CaptiveStage.jpg)

- asset : `/Game/Swarm/Meshy/CaptiveStage/SM_CaptiveStage` (matière `MI_CaptiveStage`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 500 x 532 x 362 cm (X x Y x Z) ; boîte X -250..250, Y -266..266, Z 0..362
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 7477 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Estrade du marche aux captifs.
- à brancher dans : SwarmSociety.cpp

### HitchingPost

![HitchingPost](Captures/Meshy/HitchingPost.jpg)

- asset : `/Game/Swarm/Meshy/HitchingPost/SM_HitchingPost` (matière `MI_HitchingPost`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 60 x 72 x 180 cm (X x Y x Z) ; boîte X -30..30, Y -36..36, Z 0..180
- pivot : au sol, au centre ; avant : +X
- classe `petit` : 1433 triangles (plafond 2000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 512 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Poteau d'attache.
- à brancher dans : SwarmSociety.cpp

### Monument_Concordat

![Monument_Concordat](Captures/Meshy/Monument_Concordat.jpg)

- asset : `/Game/Swarm/Meshy/Monument_Concordat/SM_Monument_Concordat` (matière `MI_Monument_Concordat`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 644 x 744 x 1600 cm (X x Y x Z) ; boîte X -322..322, Y -372..372, Z 0..1600
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 51034 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Repere du Concordat d'Aram (statue colossale).
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### Monument_Fonderies

![Monument_Fonderies](Captures/Meshy/Monument_Fonderies.jpg)

- asset : `/Game/Swarm/Meshy/Monument_Fonderies/SM_Monument_Fonderies` (matière `MI_Monument_Fonderies`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 979 x 1051 x 1800 cm (X x Y x Z) ; boîte X -489..489, Y -525..525, Z 0..1800
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 49811 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Repere de la Ligue des Fonderies (haut fourneau).
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### Monument_Protectorat

![Monument_Protectorat](Captures/Meshy/Monument_Protectorat.jpg)

- asset : `/Game/Swarm/Meshy/Monument_Protectorat/SM_Monument_Protectorat` (matière `MI_Monument_Protectorat`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 755 x 931 x 1600 cm (X x Y x Z) ; boîte X -377..377, Y -466..466, Z 0..1600
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 45358 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Repere du Protectorat de Hael (beffroi d'armes).
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### Monument_Clans

![Monument_Clans](Captures/Meshy/Monument_Clans.jpg)

- asset : `/Game/Swarm/Meshy/Monument_Clans/SM_Monument_Clans` (matière `MI_Monument_Clans`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 915 x 1005 x 1400 cm (X x Y x Z) ; boîte X -457..457, Y -502..502, Z 0..1400
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 41329 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Repere des Clans du Vent (mat-totem).
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### Monument_Affranchis

![Monument_Affranchis](Captures/Meshy/Monument_Affranchis.jpg)

- asset : `/Game/Swarm/Meshy/Monument_Affranchis/SM_Monument_Affranchis` (matière `MI_Monument_Affranchis`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 384 x 423 x 1500 cm (X x Y x Z) ; boîte X -192..192, Y -212..212, Z 0..1500
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 46256 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Repere des Affranchis (colonne de ferraille aux chaines brisees).
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### Well

![Well](Captures/Meshy/Well.jpg)

- asset : `/Game/Swarm/Meshy/Well/SM_Well` (matière `MI_Well`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 222 x 270 x 320 cm (X x Y x Z) ; boîte X -111..111, Y -135..135, Z 0..320
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 5875 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 66
- sert a : Puits de cite.
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### Fountain

![Fountain](Captures/Meshy/Fountain.jpg)

- asset : `/Game/Swarm/Meshy/Fountain/SM_Fountain` (matière `MI_Fountain`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 398 x 400 x 301 cm (X x Y x Z) ; boîte X -199..199, Y -200..200, Z 0..301
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 7760 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 66
- sert a : Fontaine de place.
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### MarketStall

![MarketStall](Captures/Meshy/MarketStall.jpg)

- asset : `/Game/Swarm/Meshy/MarketStall/SM_MarketStall` (matière `MI_MarketStall`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 215 x 400 x 249 cm (X x Y x Z) ; boîte X -108..108, Y -200..200, Z 0..249
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 7760 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Etal de marche.
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### Handcart

![Handcart](Captures/Meshy/Handcart.jpg)

- asset : `/Game/Swarm/Meshy/Handcart/SM_Handcart` (matière `MI_Handcart`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 280 x 106 x 114 cm (X x Y x Z) ; boîte X -140..140, Y -53..53, Z 0..114
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 4939 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Charrette a bras.
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### WaterTrough

![WaterTrough](Captures/Meshy/WaterTrough.jpg)

- asset : `/Game/Swarm/Meshy/WaterTrough/SM_WaterTrough` (matière `MI_WaterTrough`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 250 x 115 x 96 cm (X x Y x Z) ; boîte X -125..125, Y -58..58, Z 0..96
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 2693 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Abreuvoir.
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### Manger

![Manger](Captures/Meshy/Manger.jpg)

- asset : `/Game/Swarm/Meshy/Manger/SM_Manger` (matière `MI_Manger`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 220 x 110 x 210 cm (X x Y x Z) ; boîte X -110..110, Y -55..55, Z 0..210
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 1552 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 56
- sert a : Mangeoire d'ecurie.
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### CampTent

![CampTent](Captures/Meshy/CampTent.jpg)

- asset : `/Game/Swarm/Meshy/CampTent/SM_CampTent` (matière `MI_CampTent`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 450 x 376 x 263 cm (X x Y x Z) ; boîte X -225..225, Y -188..188, Z 0..263
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 6988 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 30
- sert a : Tente de camp (Clans, colonnes en marche).
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### Campfire

![Campfire](Captures/Meshy/Campfire.jpg)

- asset : `/Game/Swarm/Meshy/Campfire/SM_Campfire` (matière `MI_Campfire`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 182 x 188 x 150 cm (X x Y x Z) ; boîte X -91..91, Y -94..94, Z 0..150
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 4121 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 66
- sert a : Feu de camp avec trepied.
- à brancher dans : cites (SwarmCityPlan.cpp / SwarmCityLevels.cpp)

### RaidBuggy_Body

![RaidBuggy_Body](Captures/Meshy/RaidBuggy_Body.jpg)

- asset : `/Game/Swarm/Meshy/RaidBuggy_Body/SM_RaidBuggy_Body` (matière `MI_RaidBuggy_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 380 x 172 x 151 cm (X x Y x Z) ; boîte X -190..190, Y -86..86, Z 0..151
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 24022 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Buggy de raid a arceaux (Affranchis, pillards) ; roues : Wheel_IronSpoke.
- à brancher dans : SwarmVehicleParts.cpp / make_vehicles.py (agent vehicules) : nouveau genre a creer

### ArmoredTruck_Body

![ArmoredTruck_Body](Captures/Meshy/ArmoredTruck_Body.jpg)

- asset : `/Game/Swarm/Meshy/ArmoredTruck_Body/SM_ArmoredTruck_Body` (matière `MI_ArmoredTruck_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 540 x 229 x 189 cm (X x Y x Z) ; boîte X -270..270, Y -114..114, Z 0..189
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 35141 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Pick-up blinde a tourelle (Protectorat / pillards) ; tourelle : ArmoredTruck_Turret ; roues : Wheel_IronSpoke.
- à brancher dans : SwarmVehicleParts.cpp / make_vehicles.py (agent vehicules) : nouveau genre a creer

### ArmoredTruck_Turret

![ArmoredTruck_Turret](Captures/Meshy/ArmoredTruck_Turret.jpg)

- asset : `/Game/Swarm/Meshy/ArmoredTruck_Turret/SM_ArmoredTruck_Turret` (matière `MI_ArmoredTruck_Turret`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 144 x 273 x 180 cm (X x Y x Z) ; boîte X -72..72, Y -137..137, Z 0..180
- pivot : au sol, au centre ; avant : +X
- classe `mobilier` : 5890 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Tourelle du pick-up blinde : pivot au sol de la couronne, tourne autour de Z.
- à brancher dans : SwarmVehicleParts.cpp / make_vehicles.py (agent vehicules) : nouveau genre a creer

### ScrapBike

![ScrapBike](Captures/Meshy/ScrapBike.jpg)

- asset : `/Game/Swarm/Meshy/ScrapBike/SM_ScrapBike` (matière `MI_ScrapBike`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 220 x 92 x 110 cm (X x Y x Z) ; boîte X -110..110, Y -46..46, Z 0..110
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 15434 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Moto de loubard ; roues : Wheel_IronSpoke a l'echelle.
- à brancher dans : SwarmVehicleParts.cpp / make_vehicles.py (agent vehicules) : nouveau genre a creer

### RamTruck_Body

![RamTruck_Body](Captures/Meshy/RamTruck_Body.jpg)

- asset : `/Game/Swarm/Meshy/RamTruck_Body/SM_RamTruck_Body` (matière `MI_RamTruck_Body`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 800 x 218 x 265 cm (X x Y x Z) ; boîte X -400..400, Y -109..109, Z 0..265
- pivot : au sol, au centre ; avant : +X
- classe `engin_grand` : 44410 triangles (plafond 50000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Camion-belier a plaques soudees (sieges) ; roues : Wheel_IronSpoke a l'echelle.
- à brancher dans : SwarmVehicleParts.cpp / make_vehicles.py (agent vehicules) : nouveau genre a creer

### Autogyro

![Autogyro](Captures/Meshy/Autogyro.jpg)

- asset : `/Game/Swarm/Meshy/Autogyro/SM_Autogyro` (matière `MI_Autogyro`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 450 x 264 x 394 cm (X x Y x Z) ; boîte X -225..225, Y -132..132, Z 0..394
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 25076 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Autogire de fortune (aerien) ; rotor : Autogyro_Rotor (a creer) ; helice arriere comprise.
- à brancher dans : SwarmVehicleParts.cpp / make_vehicles.py (agent vehicules) : nouveau genre a creer

### WarAirship_Gondola

![WarAirship_Gondola](Captures/Meshy/WarAirship_Gondola.jpg)

- asset : `/Game/Swarm/Meshy/WarAirship_Gondola/SM_WarAirship_Gondola` (matière `MI_WarAirship_Gondola`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 1200 x 536 x 557 cm (X x Y x Z) ; boîte X -600..600, Y -268..268, Z 0..557
- pivot : au sol, au centre ; avant : +X
- classe `engin` : 30467 triangles (plafond 40000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Dirigeable de guerre, nacelle ; enveloppe : Balloon_Envelope a l'echelle (x1,5).
- à brancher dans : SwarmVehicleParts.cpp / make_vehicles.py (agent vehicules) : nouveau genre a creer

### Giant_DeadColossus

- asset : `/Game/Swarm/Meshy/Giant_DeadColossus/SM_Giant_DeadColossus` (matière `MI_Giant_DeadColossus`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 12000 x 8802 x 4656 cm (X x Y x Z) ; boîte X -6000..6000, Y -4401..4401, Z 0..4656
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 67900 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Carcasse d'un colosse mort, a demi enterree (lieu a explorer, 120 m).
- à brancher dans : monde sauvage (SwarmRuins.cpp / lieux caches)

### Giant_Shipwreck

- asset : `/Game/Swarm/Meshy/Giant_Shipwreck/SM_Giant_Shipwreck` (matière `MI_Giant_Shipwreck`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 15000 x 3015 x 4234 cm (X x Y x Z) ; boîte X -7500..7500, Y -1507..1507, Z 0..4234
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 67900 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Epave de grand paquebot echoue (lieu a explorer, 150 m).
- à brancher dans : monde sauvage (SwarmRuins.cpp)

### Giant_RadioTelescope

- asset : `/Game/Swarm/Meshy/Giant_RadioTelescope/SM_Giant_RadioTelescope` (matière `MI_Giant_RadioTelescope`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 6857 x 13136 x 6000 cm (X x Y x Z) ; boîte X -3429..3429, Y -6568..6568, Z 0..6000
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 61300 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Tour de radio-telescope effondree (lieu a explorer, 60 m).
- à brancher dans : monde sauvage (SwarmRuins.cpp)

### Giant_BrokenStatue

- asset : `/Game/Swarm/Meshy/Giant_BrokenStatue/SM_Giant_BrokenStatue` (matière `MI_Giant_BrokenStatue`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 4469 x 7477 x 6000 cm (X x Y x Z) ; boîte X -2234..2234, Y -3738..3738, Z 0..6000
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 61781 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Statue colossale brisee (lieu a explorer, 60 m).
- à brancher dans : monde sauvage (SwarmRuins.cpp)

### Giant_CrackedDam

- asset : `/Game/Swarm/Meshy/Giant_CrackedDam/SM_Giant_CrackedDam` (matière `MI_Giant_CrackedDam`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 20000 x 8422 x 8279 cm (X x Y x Z) ; boîte X -10000..10000, Y -4211..4211, Z 0..8279
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 67900 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Barrage fissure (lieu a explorer, 200 m).
- à brancher dans : monde sauvage (SwarmRuins.cpp)

### Giant_BrokenViaduct

- asset : `/Game/Swarm/Meshy/Giant_BrokenViaduct/SM_Giant_BrokenViaduct` (matière `MI_Giant_BrokenViaduct`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 18000 x 17642 x 5628 cm (X x Y x Z) ; boîte X -9000..9000, Y -8821..8821, Z 0..5628
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 58510 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Viaduc rompu (lieu a explorer, 180 m).
- à brancher dans : monde sauvage (SwarmRuins.cpp)

### Giant_FactoryCathedral

- asset : `/Game/Swarm/Meshy/Giant_FactoryCathedral/SM_Giant_FactoryCathedral` (matière `MI_Giant_FactoryCathedral`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 10000 x 5361 x 6828 cm (X x Y x Z) ; boîte X -5000..5000, Y -2681..2681, Z 0..6828
- pivot : au sol, au centre ; avant : +X
- classe `monument` : 67466 triangles (plafond 70000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 2048 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Usine-cathedrale en ruine (lieu a explorer, 100 m).
- à brancher dans : monde sauvage / Ligue des Fonderies

### Wheel_Tire

- asset : `/Game/Swarm/Meshy/Wheel_Tire/SM_Wheel_Tire` (matière `MI_Wheel_Tire`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 95 x 30 x 95 cm (X x Y x Z) ; boîte X -48..48, Y -15..15, Z -47..47
- pivot : au centre (moyeu / centre) ; avant : +X
- classe `roue` : 4137 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Roue a pneu a crampons (buggy, pick-up, camion-belier, moto) : pivot au moyeu, axe Y ; mettre a l'echelle (80 a 110 cm).
- à brancher dans : agent vehicules (copie prise) : nouveaux genres

### Autogyro_Rotor

- asset : `/Game/Swarm/Meshy/Autogyro_Rotor/SM_Autogyro_Rotor` (matière `MI_Autogyro_Rotor`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 177 x 900 x 109 cm (X x Y x Z) ; boîte X -89..89, Y -450..450, Z -55..55
- pivot : au centre (moyeu / centre) ; avant : +X
- classe `mobilier` : 2911 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Rotor de l'autogire : pivot au moyeu, tourne autour de Z (diametre 9 m).
- à brancher dans : agent vehicules (copie prise) : nouveaux genres

### Propeller

- asset : `/Game/Swarm/Meshy/Propeller/SM_Propeller` (matière `MI_Propeller`, mère `/Game/Swarm/Meshy/M_Meshy`)
- taille : 40 x 180 x 34 cm (X x Y x Z) ; boîte X -20..20, Y -90..90, Z -17..17
- pivot : au centre (moyeu / centre) ; avant : +X
- classe `mobilier` : 3105 triangles (plafond 8000) ; niveaux de détail : Nanite (tient lieu de niveaux) ; textures 1024 px (couleur, normale ; masques à la moitié) ; collision : boîte simple ; crédits : 36
- sert a : Helice (autogire, dirigeable) : pivot au moyeu, tourne autour de X ; mettre a l'echelle.
- à brancher dans : agent vehicules (copie prise) : nouveaux genres

## Ratés et jetés

- JailCage : barreaux remailles en voiles triangulaires entre les barres (image -> 3D, 36 credits) : jete
- TavernSign : texte peint en anglais sur l'enseigne (image de reference) : pas pousse en 3D
- SailCart_SailFurled : voile carguee : un drap pendu, pas une voile roulee (apercu, 20 credits) ; la voile deployee sert seule
- Stalker_LegUpper : cuisse du rodeur : apercu en forme de queue a griffes (20 credits) ; le rodeur prend les pattes du colosse a son echelle
- Stalker_LegLower : tibia du rodeur : apercu d'un insecte entier (20 credits) ; idem

## Refaire ou ajouter un modèle

1. Décrire le modèle dans `Tools/Assets/meshy_models.json` : nom, classe (budget de triangles et de texture),
   polygones demandés, description courte et concrète (matière + époque + usure + silhouette, finir par « Game asset,
   single object, no base, no ground, centered. »), description des textures, taille reelle, pivot.
2. `python Tools/Assets/meshy.py apercu NOM` (20 crédits), `python Tools/Assets/meshy.py suivre` ; regarder
   `C:\Dev\Swarm\SourceAssets\Meshy\NOM\apercu_N.png`. Raté : `apercu NOM --encore`. Bon : `affine NOM` (10), `suivre`.
3. `python Tools/Assets/meshy_prepare.py NOM` (échelle, pivot, axes, textures à la taille de la classe).
4. `$env:SWARM_MESHY_ONLY = "NOM"; Tools\Test\run_python.ps1 -Script Tools/Editor/import_meshy.py`.
5. `Tools\Test\run_tests.ps1 -Filter "Swarm.Modeles" -Name modeles -ResX 1920 -ResY 1080` : budget vérifié,
   capture `MOD_xx_NOM.png` ; puis `python Tools/Assets/meshy_doc.py`.

La clé d'API est lue dans `C:\Dev\Swarm\Saved\meshy_key.txt` et n'est écrite nulle part ailleurs. L'état des
tâches (`_etat.json` à côté des sources) empêche de payer deux fois la même ; ne pas le supprimer.
