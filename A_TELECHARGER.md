# A telecharger pour le jalon 10 (rendu final)

Liste unique. Rien ici n'est indispensable : le jeu est jouable sans, avec ses formes provisoires.
Tout se recupere avec ton compte Epic sur https://www.fab.com (bouton "Add to my library", puis dans l'editeur :
fenetre Fab -> My Library -> Add to project), sauf le plugin NVIDIA.

Ce que j'ai pu verifier, et ce que je n'ai pas pu : je n'ai pas acces a Fab. Les noms ci-dessous viennent des
annonces d'Epic et de la presse (novembre 2026). Pour Megascans et Megaplants je donne des termes de recherche
plutot que des noms de fiches que je ne peux pas confirmer. Avant d'ajouter un pack, regarde la ligne "License"
de sa fiche et dis-moi ce que tu as pris : je le consignerai dans Docs/CREDITS.md.

## 1. Animation du joueur

| Pack | Editeur | Sert a | Licence annoncee |
|---|---|---|---|
| **Game Animation Sample** (version UE 5.8) | Epic Games | Motion Matching : plus de 500 animations de locomotion, pour remplacer la marche du mannequin | Gratuit ; utilisable seulement avec Unreal Engine |

## 2. Sols, roches, falaises (Megascans)

Epic a annonce en novembre 2026 que la bibliotheque Megascans redevenait gratuite sur Fab. A verifier sur
chaque fiche (prix affiche "Free").

Sur Fab : filtre Publisher = Quixel, Type = 3D ou Surfaces. Il me faut, par biome :

| Biome | A chercher | Quantite |
|---|---|---|
| Plaine | surfaces "grass", "dry grass", "soil" | 3 surfaces |
| Foret | surfaces "forest floor", "moss", "roots" ; modeles "fallen trunk", "stump" | 3 surfaces, 4 modeles |
| Colline | surfaces "rocky ground", "gravel" ; modeles "rock", "boulder" (Nanite) | 2 surfaces, 6 modeles |
| Desert | surfaces "sand", "cracked mud" ; modeles "desert rock" | 2 surfaces, 4 modeles |
| Montagne | surfaces "cliff", "rock face", "snow" ; modeles "cliff" (Nanite) | 3 surfaces, 6 modeles |
| Rivage | surfaces "wet sand", "pebbles" | 2 surfaces |
| Cites | surfaces "concrete", "rusted metal", "corrugated metal", "old wood planks", "brick" | 6 surfaces |

Prends la qualite "High" (2K ou 4K) ; pas besoin de "Raw".

## 3. Vegetation (Megaplants)

Meme annonce : Megaplants gratuit sur Fab. Sur Fab : Publisher = Quixel, recherche "Megaplants".

| Besoin | A chercher | Quantite |
|---|---|---|
| Arbres de foret | un conifere et un feuillu, avec leurs variantes de taille | 2 especes |
| Arbustes et buissons | "shrub", "bush" | 3 especes |
| Herbes | "grass clump", "fern", "dry grass" | 4 especes |
| Desert | "dry bush", "dead tree" | 2 especes |

## 4. Humains

| Outil | Editeur | Sert a | Remarque |
|---|---|---|---|
| **MetaHuman Creator** (integre a UE 5.8, plugin MetaHuman) | Epic Games | Chefs, officiers, joueur | Rien a telecharger sur Fab ; il faut activer le plugin et creer les personnages dans l'editeur. C'est un travail a la main : dis-moi si tu veux le faire, sinon on garde le mannequin. |

## 5. NVIDIA DLSS

| Plugin | Ou | Sert a |
|---|---|---|
| **DLSS 4.5 Plugin for Unreal Engine**, version pour UE 5.8 | https://developer.nvidia.com/rtx/dlss (compte NVIDIA Developer gratuit) | Super Resolution, Frame Generation, Ray Reconstruction sur ta RTX 5080 |

Decompresse l'archive et copie son dossier `Plugins` dans `C:\Dev\Swarm\Plugins`. Des utilisateurs signalent du
ghosting avec certains prereglages sur UE 5.8 : on comparera avec TSR avant de le garder.

## Ce qu'aucun pack ne couvrira

L'Essaim (mucus, ruches, Arche, creatures) reste genere par code. Le son : aucune source gratuite n'est encore
choisie ; je te proposerai une liste a part quand on y sera.

## Etat au 15/11/2026 : ce qui est arrive, ce qui servirait encore

Arrive dans ta bibliotheque (via `C:\Dev\ASCTeuthisan`) et **utilise** : European Black Alder (arbres), Paragon Kwang
(chefs), Grux, Rampage, Sevarog, Khaimera (champions de l'Essaim), sA_Megapack (flammes et impacts des tirs).
Arrive et **pas encore utilise** : Stone Pine Forest, Bark Pack, Narbash, Iggy & Scorch, A_Surface_Footstep,
Advanced Universal Spawner, ASC Teuthisan (candidat pour la Reine), City Sample Buildings (trop lourd, non retenu).

Ce qui servirait le plus maintenant, par ordre d'effet a l'ecran :

| Besoin | A chercher sur Fab | Pourquoi |
|---|---|---|
| Des humains pour les chefs et les soldats | Paragon : **Lt. Belica, Murdock, Twinblast, Wraith, Sparrow, Greystone, Kallari** (gratuits, Epic) | Aujourd'hui tous les chefs ont le corps de Kwang et les soldats sont des mannequins |
| Des batisses | un pack de village ou de ruines medievales/post-apo gratuit (regarde « Free » + « modular ») | Les cites sont encore faites de blocs textures |
| Des sons | un pack d'ambiances et d'armes gratuit | Les sons actuels sont synthetises par le projet : corrects pour une maquette, pas plus |
| Animations du joueur | **Game Animation Sample** (Epic) | Marche, course, sauts fluides |

Rien de tout cela n'est indispensable ; dis-moi seulement ce que tu ajoutes.
