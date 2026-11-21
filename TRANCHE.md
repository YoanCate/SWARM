# SWARM — le cap : une première heure indiscutable

Décision du chef de projet, 19/11/2026 à 21 h, sur la délégation du propriétaire (« fais ce qui semble le mieux pour le
projet en ayant une vision globale et future »).

## Pourquoi

Vingt chantiers avancent en même temps et le propriétaire juge le jeu en y jouant vingt minutes. Aujourd'hui chaque
système existe « un peu partout » et rien n'est fini nulle part : c'est ce qu'il voit (« vide », « aucun sens »,
« asset flip »). On ne finira pas un monde entier d'un coup. On finit d'abord UNE HEURE DE JEU, dans UNE RÉGION, au
niveau de ses références — puis on étend ce niveau au reste du monde, système par système, avec la même exigence.

Tout agent place en tête de sa liste ce dont cette heure a besoin. Ce qui n'y sert pas attend.

## La région de départ

Un désert et la vallée habitée qui le borde, sur le territoire d'un seul peuple, au contact d'un front de l'Essaim.
L'agent URBANISME choisit la région réelle dans le monde généré (graine par défaut) et écrit ici, sous ce titre, les
lieux retenus : point de réveil, hameau, bourg, capitale, camp hors des murs, front de l'Essaim, repère géant.

**Peuple : le Protectorat de Hael (faction 3).** Monde de la graine 1 (celle des tests ; en partie réelle la graine est
tirée au hasard : la RÈGLE plus bas reproduit la même disposition dans chaque monde). Repère : une case = 200 m ; centre
monde d'une case (x, y) = ((x + 0,5) × 200 − 4 800 m, (y + 0,5) × 200 − 4 800 m) ; +X = est, +Y = sud ; la porte
principale d'une cité regarde +X. Ids = `Settlement::id` / `Satellite::id` de la simulation.

La disposition : au nord-est, un désert (cases 41–47 × 15–27) ; à son pied, une vallée habitée d'est en ouest
(lignes 27–31) entre le désert, les montagnes (30–38 × 20–25) et un lac au sud (33–44 × 31–35) ; plus au sud, les
forêts des bourgs. L'Essaim tombe au jour 1 sur la bordure ouest du désert, au pied des montagnes.

| Lieu | Nom (id) | Case | Monde (m) | Distance |
|---|---|---|---|---|
| Point de réveil (+ voiture) | désert, sur une croupe | 45, 20 | 4 300, −700 | — |
| Épave à fouiller | Épave du Tenace (lieu 6) | 42, 16 | 3 700, −1 500 | 1,0 km du réveil |
| Repère géant n° 1 | la flèche de la Reine (ruche 0) | 36, 20 | 2 500, −700 | 1,8 km à l'ouest du réveil |
| Repère géant n° 2 | la Tour-Signal de Fort Hael | 44, 29 | 4 100, 1 100 | 1,8 km au sud du réveil |
| Hameau | La Herse-lès-Redoute (satellite 103, champs) | 44, 28 | ~4 100, ~800 (au nord du rempart) | 1,5 km du réveil, à la lisière du désert |
| Second hameau | Grange-Haute-lès-Redoute (satellite 105, champs) | 42, 29 | 3 700, 1 100 | 0,4 km à l'ouest de la capitale |
| Capitale à plusieurs enceintes | Fort Hael (3), ~2 750 habitants, 613 bâtiments, 372 m | 44, 29 | 4 100, 1 100 | 1,8 km du réveil |
| Cité (étoile de bastions, fossé) | Redoute (15) | 38, 29 | 2 900, 1 100 | 1,2 km à l'ouest de la capitale |
| Bourg | Bastion (13), ~120 habitants | 44, 36 | 4 100, 2 500 | 1,4 km au sud de la capitale, 3,2 km du réveil |
| Second bourg | Citadelle (14) | 40, 37 | 3 300, 2 700 | 1,8 km de la capitale |
| Village | La Dîme-lès-Redoute (satellite 102, bûcherons) | 34, 30 | 2 100, 1 300 | 0,8 km à l'ouest de Redoute |
| Camp hors des murs (désert) | repaire des Glaneurs de la Voûte (cercle 1) | 45, 11 | 4 300, −2 500 | 1,8 km au nord du réveil |
| Camp hors des murs (lac) | repaire des Gens du Gué, passeurs (cercle 3) | 37, 35 | 2 700, 2 300 | rive du lac, 1,8 km de la capitale |
| Front de l'Essaim | chute de la Reine au jour 1 : cratère 3 × 3 cases, ruche-reine, premières cases de mucus et récolte | 36, 20 | 2 500, −700 | 1,8 km du réveil, 2,4 km de Fort Hael, 1,8 km de Redoute |
| Tombeau (lieu caché) | Tombeau de Cassian de Garde (lieu 3) | 37, 21 | 2 700, −500 | au bord du cratère |

Pour le relief : la rivière de la vallée descend des montagnes (36, 24) vers le sud, longe Redoute par l'est (39, 28)
— c'est le confluent de son geste — et se jette dans le lac en (40, 31) ; la route Redoute–Fort Hael (ligne 29) la passe
à gué ou sur un pont en (40, 29). Le désert vu du réveil : mesas et canyons au nord et à l'est ; au sud-ouest, la
descente vers la vallée et la Tour-Signal.

Pour les titans : la proie « bourg » est Bastion (id 13) ; Redoute (id 15) est sur le chemin direct depuis la Reine.

**La règle (pour que chaque monde ait sa région de départ)** — écrite dans la simulation sur la branche `villes`
(`Simulation::StartPeople`, `Config::landNearStart`) : le peuple de départ est celui dont la capitale (rang capitale,
sédentaire) a le plus de désert à moins de 14 cases ; l'Essaim tombe à 9–13 cases de cette capitale, hors de ses terres,
hors du désert, de l'eau et des montagnes, là où il y a le plus à récolter. Le point de réveil (agent joueur) : la case
de désert à 8–10 cases de la capitale du peuple de départ, du côté du désert, vue dégagée sur la capitale et le front.
Équilibre mesuré (100 graines) : la chute près d'une capitale affaiblit l'Essaim (Arche 73 %) ; compensé par sa
réserve de départ (150 → 220 de biomasse) : Arche 75 %, Essaim détruit 25 %, monde mort 0 %, médiane 722 jours (au bord
bas de la fourchette acceptée, à surveiller).

## L'heure, minute par minute

| Temps | Ce que vit le joueur | Chantiers |
|---|---|---|
| 0 – 2 min | Lancement en moins de 30 s, titre immédiat, création du personnage sans défaut, réveil SEUL dans le désert près d'une voiture. Aucun texte ne dit quoi faire. | empaquetage, interface, combat, véhicules, joueur |
| 2 – 10 min | Il marche, court, monte en voiture, roule. Relief spectaculaire. À l'horizon : une fumée, une tour, un titan qui marche, une tempête qui monte. Une épave à fouiller, une bête géante à éviter, une patrouille motorisée qui le contrôle ou le poursuit. La boussole nomme ce qu'il voit. | relief, rendu, joueur, véhicules, titans, catastrophes, armées, monde sauvage |
| 10 – 20 min | Un hameau, puis un bourg : des gens qui ont une journée, un forgeron, une taverne où l'on entre, des enfants, un marché. On lui parle, il achète, il apprend qu'un cercle recrute, qu'un convoi de captifs passe ce soir. La nuit tombe : fenêtres allumées, veilleurs, on dort ou on rôde. | urbanisme, architecture, vie quotidienne, foule, profondeur, sociétés |
| 20 – 35 min | La capitale à plusieurs enceintes : faubourg, porte, ville basse, ville haute ; armurier, maître d'armes, arène, temple, place de justice. Il s'engage ou prend un contrat, apprend un geste auprès d'un maître, recrute un compagnon. | urbanisme, architecture, vie, profondeur, interface |
| 35 – 50 min | Le front : un chantier de l'Essaim qui éventre le sol, des castes de tailles différentes, une ruche où l'on entre pour saboter, du bétail humain à délivrer. Le tir a du poids, les explosions soufflent, les bêtes encerclent. | Essaim, combat, bâtir, foule |
| 50 – 60 min | Le titan arrive sur le bourg. Les murailles sont attaquées, les défenseurs tirent, les gens fuient, une tornade ou la chute de la météorite passe en cinématique. Il se bat sous les pattes, ou monte dessus. Il rentre bâtir un abri avec ce qu'il a miné. | titans, Essaim, foule, catastrophes, bâtir, armées |

## Ce qui rend l'heure « indiscutable »

- 60 images par seconde en 2560×1440 au niveau Épique, aucune image de plus de 50 ms, aucun plantage, aucune matière
  grise, aucun écran noir — prouvé par la partie automatique du soir (verdict, planche, extrait vidéo).
- Rien de ce qui bouge n'a été validé sur une image fixe.
- Chaque lieu traversé a un site, un geste, des gens nommés ; chaque bâtiment s'ouvre ; chaque bête a une caste lisible.
- Le chef de projet a regardé l'extrait vidéo de l'heure entière avant de dire « teste ».

## Ensuite

1. Étendre : les quatre autres peuples et leurs régions, au même niveau, un par un.
2. Approfondir : compagnons, quêtes nées du monde, économie, base menacée, fin de partie (l'Arche).
3. Le film d'ouverture, tourné sur ce jeu-là : la vie d'une cité, la chute, l'Essaim et ses castes, le siège, le titan
   qui écrase une bataille, le rayon. Sans texte.
4. La version empaquetée livrée au propriétaire à chaque fin de journée, avec la liste de ce qu'elle contient et de ce
   qu'elle ne contient pas encore.
