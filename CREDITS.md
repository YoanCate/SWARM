# Crédits et licences

Tout ce qui n'est pas dans ce tableau est écrit ou généré par le projet (code, simulation, terrain, bâtisses,
créatures de l'Essaim, fusil, épave, interface, herbe, et tous les sons de `Content/Swarm/Audio`,
synthétisés par `Tools/Assets/make_sounds.py`).

| Élément | Auteur | Licence | Usage dans le jeu |
|---|---|---|---|
| Unreal Engine 5.8.4 | Epic Games | Unreal Engine EULA | Moteur |
| Police Roboto (livrée avec le moteur, `/Engine/EngineFonts/Roboto`) | Google / Epic Games | Apache 2.0 | Toute l'interface |
| Mannequins Manny / Quinn et leurs animations (copiés depuis `Templates/TemplateResources/High/Characters`) | Epic Games | Unreal Engine EULA (contenu livré avec le moteur, réservé aux projets Unreal) | Corps du joueur et des soldats |
| Formes de base et nuages volumétriques du moteur | Epic Games | Unreal Engine EULA | Pièces des bâtisses, ciel |
| Textures de sol : rocky_terrain_02, forest_leaves_02, sand_01, aerial_grass_rock, rock_face_03, snow_02, coast_sand_01 | Poly Haven (polyhaven.com) | CC0 (domaine public) | Sol du monde |
| Textures de murs et de toits : castle_wall_slates, clay_plaster, medieval_wood, castle_brick_02_white, concrete_wall_008, corrugated_iron, clay_roof_tiles_02, roof_slates_02, thatch_roof_angled | Poly Haven | CC0 | Bâtisses des cités |
| Arbres et buissons : fir_sapling_medium, island_tree_02, quiver_tree_02, shrub_02 | Poly Haven | CC0 | Forêts, plaines, collines, désert |
| Rochers : boulder_01, namaqualand_boulder_02, rock_moss_set_01 | Poly Haven | CC0 | Rochers du monde |
| Paragon : Kwang (maillage `Kwang_GDC`, animations), ajouté par le propriétaire depuis sa bibliothèque Fab | Epic Games | Licence Fab, contenu « UE-Only » (réservé aux produits Unreal Engine) | Corps des chefs de faction |
| Paragon : Grux (maillage, animations ; ses matériaux sont remplacés par la peau des bêtes du jeu, `M_BeastSkin`, qui garde ses cartes de normales `T_Grux_head_normals`, `T_Grux_Torso_normals`, `T_Grux_Legshands_normals`, `T_Grux_accessory_normals`), idem | Epic Games | Licence Fab, contenu « UE-Only » | Corps animé de la caste « lame » et de champions de l'Essaim |
| Paragon : cris d'effort, de douleur et de mort de Kwang et de Grux (18 `SoundWave`), idem | Epic Games | Licence Fab, contenu « UE-Only » | Voix des humains et des champions au combat |
| Paragon : Rampage, Sevarog, Khaimera (maillage et une quinzaine d'animations chacun : attente, course, coups, coups reçus, morts, cri ; sous la peau des bêtes du jeu), et leurs cartes de normales, ramenées à 2K (Rampage : `T_Rampage_Head_Legs_N`, `T_Rampage_Torso_N`, `T_Rampage_Arm_N`, `T_Rampage_Armor_N`, `T_Rampage_Teeth_N`, et l'occlusion de son armure `T_Rampage_Armor_MatMask` ; Sevarog : `T_Sevarog_Torso_N`, `T_Sevarog_ArmourR_N`, `T_Sevarog_Arms_BackCloth_N`, `T_Sevarog_Belt_Chainmall_Hammer_N` ; Khaimera : `T_Khaimera_Body_N`, `T_Khaimera_ArmsLegs_N`, `T_Khaimera_WeaponLoins_N`, `T_Khaimera_Beast_N_Demo`), idem | Epic Games | Licence Fab, contenu « UE-Only » | Corps animés des castes « cuirassé », « garde royale » et « ouvrière », et de champions de l'Essaim |
| Paragon : Iggy & Scorch (maillage et quatorze animations ; seul le lézard Scorch est montré, son cavalier est effacé ; cartes de normales ramenées à 2K `T_IggyScorch_Mount_Front_N`, `T_IggyScorch_Mount_Rear_N`, `T_IggyScorch_Items_N`), idem | Epic Games | Licence Fab, contenu « UE-Only » | Corps animé de la caste « essaimeur » |
| sA_Megapack : sA_ShootingVfxPack (flamme au canon, impacts), idem | éditeur du pack sur Fab (nom et libellé de licence à relire sur sa page) | Licence Fab | Effets des tirs |
| `ABP_TP_Rifle` (animations du gabarit de tir, copié de `Templates/TemplateResources/Standard/Variant_Shooter`) | Epic Games | Unreal Engine EULA (contenu livré avec le moteur) | Corps du joueur fusil en main |
| Medieval Village Megascans Sample : onze maisons, forge, moulins, puits, objets de rue (fondus en un maillage par assemblage), et 66 sons (pas, vent, oiseaux, grillons, feu, musique) | Epic Games / Quixel | Licence Fab, contenu Epic (libellé exact à relire sur la page du pack) | Maisons des cités, objets, tombes ; pas, ambiances et musique |
| Medieval Village Megascans Sample, nature : épicéas (`ThirdParty/PN_interactiveSpruceForest`, livré dans l'exemple), souches, troncs, rochers moussus, fougères, blé, fleurs, herbes sèches, tronçon de route | Epic Games / Quixel | idem | Forêts, sol de forêt, plantes, champs, rues, chantiers |
| Dark Ruins Megascans Sample : tour en ruine, arche, falaises, chaos de blocs, coffre, gisant | Epic Games / Quixel | idem | Lieux cachés, porte des cités, paysage |
| Goddess Temple (projet « RomanCave ») : colonnes, tête de pierre, lanternes, brûle-parfums, falaises, palmiers | Epic Games / Quixel | idem | Sanctuaire du Concordat, paysage |
| Bazaar (Scene_Bazaar_Vol1) : étals de marché, toiles tendues | éditeur du pack sur Fab (nom à relire sur sa page) | Licence Fab | Marchés des cités, tentes des Clans |
| Game Animation Sample : MetaHuman « Kellan » (corps, visage, sweat, pantalon, chaussures, mèches) et son reciblage (`ABP_GenericRetarget`, `RTG_UEFN_to_Metahuman_nrw`) | Epic Games | Licence Fab, contenu Epic ; MetaHuman réservé aux projets Unreal | Corps du joueur |
| Megascans Trees : European Black Alder (variante « SimpleWind » : arbres de plein champ, de forêt, jeunes pousses) et `MSPresets`, idem | Quixel / Epic Games | Licence Fab (libellé exact à relire sur la page du pack ; gratuit dans la bibliothèque du propriétaire) | Feuillus des plaines et des forêts |
| Plugin NVIDIA DLSS 4.5 pour UE 5.8 (v8.9.0), fourni par le propriétaire du projet | NVIDIA | NVIDIA RTX SDKs License | Mise à l'échelle (touche U). **Non redistribuable** : le dossier `Plugins/` n'est pas dans le dépôt, à réinstaller depuis l'archive NVIDIA. |
| Paragon : Narbash (maillage et animations ; chef des Clans), ajouté par le propriétaire depuis sa bibliothèque Fab | Epic Games | Unreal Engine EULA (contenu Paragon : gratuit pour les projets Unreal Engine) | Chef d'un peuple, dans sa cité |
| Game Animation Sample : TwinBlast « ActionHero » (maillage ; il emprunte les animations du mannequin) | Epic Games | Unreal Engine EULA | Chef des Affranchis |
| ASC Teuthisan (créature à tentacules : maillage, squelette, textures ; animée par code, sans ses animations), ajouté par le propriétaire depuis sa bibliothèque Fab | éditeur du pack sur Fab (nom et libellé de licence à relire sur la fiche) | licence Fab | Corps de la Reine dans l'Arche ; le jeu s'en passe si le pack est absent |
| MC_Sample (animations : blessé, ivre, démarche, attente ; squelette `SKM_MCUE5v2`), idem | éditeur du pack sur Fab (nom et libellé de licence à relire sur la fiche) | licence Fab | Attitudes des habitants |
| FreeAnimsMixPack (animations de mannequin), idem | éditeur du pack sur Fab (nom et libellé de licence à relire sur la fiche) | licence Fab | Gestes des habitants et des combattants |
| A_Surface_Footstep (huit effets Niagara de pas : terre, herbe, gravier, feuilles, boue, sable, étincelles, eau), idem | éditeur du pack sur Fab (nom et libellé de licence à relire sur la fiche) | licence Fab | Poussière et éclaboussures sous les pas |
| sA_Megapack : sA_Rayvfx (`NS_Energy_1`, `NS_Hit_1`, `NS_Ray_1`) et sA_SkillSet_1 (`NS_Cast`, `NS_Thorn_2`, `NS_Thorns_1`), idem | éditeur du pack sur Fab (nom et libellé de licence à relire sur la fiche) | licence Fab | Effets de l'Essaim et des impacts |
| Medieval Village Megascans Sample, sons (`Content/Audio`) : musiques par couches, oiseaux, insectes, vent dans les arbres, feu, pas (pavés, terre, herbe, sous-bois, bois), tonnerre | Epic Games | Unreal Engine EULA | Musique, ambiances, pas, orage |
| Valley of the Ancient, sons (`Content/AncientContent/Audio`) : trois ambiances (monde sombre, vent froid, cigales) | Epic Games | Unreal Engine EULA | Ambiances de l'Arche, des hauteurs et du désert |
| Medieval Village Megascans Sample, sons repris dans le film d'ouverture : `sfx_Ambient_Village_Dusk_01`, `sfx_Ambient_DistCrows`, `sfx_Ambient_VeryCUFire`, `sfx_Ambience_Wind_Trees_Soft_l_01` | Epic Games | Unreal Engine EULA | Bruitages du film (village à l'aube, corbeaux, cités en feu, vent du ralliement) |
| Partition du film d'ouverture (`/Game/Swarm/Audio/Film/SW_Film_Score`, `SW_Film_Short`) : synthétisée par le projet (`Tools/Audio/make_film_score.py`, Python pur, aucun échantillon repris) | projet SWARM | propre au projet | Bande-son du film d'ouverture et de sa version courte |
| Bande-son complète du film de 34 s (`/Game/Swarm/Audio/Film/SW_Film_Mix` : musique, bruitages, mixage) : synthétisée par le projet (`Tools/Assets/film_sound.py`, numpy, aucun échantillon repris) | projet SWARM | propre au projet | Film d'ouverture |
| Modèles générés avec Meshy (abonnement du propriétaire, usage commercial selon ses conditions) : liste dans `Docs/MODELES_MESHY.md` | Meshy (génération par IA, descriptions écrites pour le jeu) | Conditions de l'abonnement Meshy du propriétaire | Créatures de l'Essaim, architecture de chair, engins, justice, repères (`/Game/Swarm/Meshy/`) |

## Vérification des licences

- **Poly Haven** : tous les assets du site sont publiés en CC0 (https://polyhaven.com/license). Aucune attribution
  n'est exigée ; ils sont cités ici par honnêteté. Téléchargés sans compte par `Tools/Assets/fetch_polyhaven.ps1`
  et `Tools/Assets/fetch_models.py`.
- **Contenu Epic** (mannequins, animations) : utilisable dans tout projet Unreal Engine, pas ailleurs.
- **DLSS** : le plugin se télécharge sur https://developer.nvidia.com/rtx/dlss après acceptation de la licence NVIDIA.

## Packs Fab du propriétaire

Il les ajoute depuis le lanceur Epic à un autre projet (`C:\Dev\ASCTeuthisan`), puis seule la partie utile est copiée
dans `Content/` (dossiers hors dépôt git : ils se retéléchargent depuis Fab). Paragon, Megascans et MetaHuman sont
gratuits pour les projets Unreal Engine. Arrivés et pas encore utilisés dans le jeu :
Stone Pine Forest ; Bark Pack ; Advanced Universal Spawner ; le reste de sA_Megapack ; City Sample Buildings (15 Go,
non retenu). Chacun sera inscrit dans le tableau avec sa licence le jour où il entre dans le jeu.

**Packs ajoutés par le propriétaire le 19/11/2026** (depuis sa bibliothèque Fab ; copiés dans `Content/` sous leur nom
de dossier d'origine, hors dépôt git ; auteur et libellé de licence exacts à relire sur chaque fiche) :
- `AnimalVarietyPack` — Animal Variety Pack, PROTOFACTOR Inc (ancienne collection gratuite permanente d'Unreal) : loup,
  renard, cerf, biche, cochon, corbeau, animés. Faune du monde.
- `M5VFXVOL2` — M5 VFX Vol2. Fire and Flames (ancienne collection gratuite permanente d'Unreal) : feux Niagara. Incendies.
- `NiagaraExamples` — Niagara Examples Pack, Epic Games : explosions, fumées, étincelles, impacts. Spectacle.
- `HorseStarterKit` — Horse Starter Kit : cheval, selle, rênes, allures, monter / descendre, cavalier. Montures.
- `Atris_swords` — épées (neuf modèles). Armes blanches.
- `CitySampleCrowd` — City Sample Crowds, Epic Games (gratuit pour les projets Unreal Engine) : utilisé dans le jeu
  depuis le 19/11/2026, part utile seulement (560 paquets, 1,35 Go, textures ramenées à 2K, premier niveau de détail
  des visages retiré) : douze visages MetaHuman (six d'homme portés par le corps de Kellan, six de femme), le corps de
  femme « normal », trois hauts, trois bas, des chaussures, cinq coiffures en cartes. Habitants.
- `Characters/Echo` — Echo, du Game Animation Sample (Epic Games, gratuit pour les projets Unreal Engine), copié le 15/11
  puis allégé le 19/11 (319 Mo, textures 2K) : une habitante sur cinq.
- Surfaces : `Granite_001`, `Ground_011`, `Construction_Ground`, `Construction_Ground_01`, `Construction_Ground_02`,
  `Marble_001`, `Metal_Rust_5`, `Metal_Rust_Base`, `Metal_Plate_005`. Sols des rues, sanctuaire, engins.
- `LootDropVFX` — effets de butin. Caches.
- Effets de feu, de fumée et de braises repris de Medieval Village Megascans Sample (Epic Games / Quixel) sous
  `Content/Effects` : incendies (`SwarmWildfire`).
- `Knife_light` — un couteau ; `Metal_Stained_2` — tôle tachée.
- Véhicules des peuples et de l'Essaim (maillages, matériau `M_Vehicle`) : fabriqués par script
  (`Tools/Assets/make_vehicles.py`, `Tools/Editor/setup_vehicles.py`) ; aucun asset tiers.
- `Vefects` — Free Fire VFX et Tentacles VFX (Vefects) : feux Niagara, bave et tentacules. Incendies, ruches.
- `Fire_EXP_Vol01_Free` — explosions de feu Niagara. Spectacle.
- `MedievalSword` — une épée médiévale ; `ReefKit` — récif (coraux, rochers, épave de barque, poissons). Côtes.
- Surfaces (suite) : `Stonework_004`, `Stones_001`, `Rock_001`, `Prehistoric_Cave`, `Sand_02`, `Rusty_Metal`.
- Héros Paragon (Epic Games, gratuits pour projet Unreal) arrivés dans `C:\Dev\ASCTeuthisan\Content` : toute la
  collection ; seuls ceux qui entrent dans le jeu seront copiés et inscrits ici, le reste sera supprimé.
Supprimés sans être utilisés (surfaces et sons modernes) : Asphalt_Rough_01/02, Concrete_027/047/048/052, Line_Concrete,
Cyber_Metal_Step.

**À relire par le propriétaire avant toute diffusion** (18/11/2026) : les lignes marquées « nom et libellé de licence
à relire sur la fiche » — je n'ai pas accès à sa bibliothèque Fab ; l'auteur et la licence exacts de ces packs sont
sur leur fiche (Fab > Ma bibliothèque).
