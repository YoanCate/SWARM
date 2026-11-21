# Journal de développement — SWARM

Mémoire du projet. À relire en reprenant une session.
Trois états à ne jamais confondre : **écrit** (pas compilé), **compilé** (pas testé), **testé** (vérifié, avec la méthode).

## Contexte de départ (14/11/2026)

- **Le dossier de départ était vide.** `Swarm.zip` (simulation existante, pont Unreal, outils, docs) n'était nulle part
  sur la machine. La consigne initiale disait de s'arrêter dans ce cas ; le propriétaire a ensuite demandé de démarrer
  sans l'attendre. Décision : **tout réécrire à partir du cahier des charges**, dans `C:\Dev\Swarm`.
  Conséquence : la simulation de ce dépôt n'est PAS celle qui avait été « testée sur 40 mondes ». Elle a la même
  interface (`Get*`, `Player*`) et les mêmes piliers, mais ses chiffres sont les siens.
  Si `Swarm.zip` réapparaît : comparer avant de fusionner, ne rien écraser.
- Machine : Windows 11, Ryzen 7 7800X3D, RTX 5080, 64 Go. Deux écrans : principal 2560×1440 (`DISPLAY1`),
  secondaire 1920×1080 à gauche (`DISPLAY2`, X=-1920).
- **Consigne permanente du propriétaire** : ne rien afficher sur l'écran principal, pas de son, pas de demande
  d'autorisation Windows. Donc : tout se lance hors écran (`-RenderOffscreen -nosound -nosplash -unattended`),
  jamais l'éditeur en fenêtre, jamais d'installeur qui demande une élévation.
- Unreal Engine **5.8.4** (`C:\Program Files\Epic Games\UE_5.8`), installé par le launcher le jour même.
- Visual Studio Community 2026 (18.11), MSVC **14.51.36231**, SDK Windows 10.0.26100. La charge « Développement de
  jeux en C++ » n'est pas installée et UE 5.8 préfère MSVC 14.50, mais 14.51 n'est pas dans sa liste de versions
  bannies et **la compilation passe** (vérifié). L'installer aurait demandé une élévation : non fait, voir la liste
  « à faire par le propriétaire » dans le README.
- Git + Git LFS (`Content/**`, `.uasset`, `.umap`, `.png`, `.mp4`).

## Commandes de référence

```powershell
# Compiler le module éditeur
& "C:\Program Files\Epic Games\UE_5.8\Engine\Build\BatchFiles\Build.bat" SwarmEditor Win64 Development -Project="C:\Dev\Swarm\Swarm.uproject" -WaitMutex -NoHotReload

# Compiler puis lancer l'outil headless (100 mondes)
cmd /c C:\Dev\Swarm\Tools\Headless\build.bat
C:\Dev\Swarm\Tools\Headless\bin\swarm_headless.exe --seeds 100

# Tests d'automatisation, hors écran, sans son
& "C:\Program Files\Epic Games\UE_5.8\Engine\Binaries\Win64\UnrealEditor-Cmd.exe" "C:\Dev\Swarm\Swarm.uproject" -game -RenderOffscreen -nosound -nosplash -unattended -nopause -ResX=1600 -ResY=900 -ForceRes -ExecCmds="Automation RunTests Swarm; Quit" -TestExit="Automation Test Queue Empty" -abslog=C:\Dev\Swarm\Saved\Logs\tests.log
```

Les captures des tests vont dans `Saved/Screenshots/Swarm/`.

## Décisions de conception (non tranchées par le cahier des charges)

| Sujet | Décision | Raison |
|---|---|---|
| Biomasse « finie » | Hors mucus, la biomasse repousse (logistique) tant qu'il en reste. Sous le mucus rien ne repousse, et une case dévorée jusqu'à zéro reste morte. | Sans repousse les factions s'affament seules ; avec repousse sous mucus le monde ne pourrait jamais mourir. |
| Minerai | Strictement fini partout. | Pilier « ressources finies ». |
| Proportions des biomes | Seuils par centiles : chaque monde a 9 % d'eau, 9 % de montagne, 14 % de collines, etc. Seule la disposition change. | Équilibrage stable d'une graine à l'autre. |
| Terre | Une seule terre d'un seul tenant ; les îlots deviennent de l'eau. | Tout doit être atteignable à pied. |
| Trésor des factions | Nourriture et minerai mis en commun par faction. | Simplicité ; une cité prise emporte sa population, pas un stock séparé. |
| Cités indépendantes (schisme, sécession) | Figées : elles se défendent mais n'ont plus d'économie ni d'armée. | Abstraction assumée ; à enrichir si le jeu en a besoin. |
| Arche | Le site de l'Arche est une ruche spéciale (garnison, points de vie). Le couvain royal y est porté dès la pose de la quille. | Réutilise combat et matérialisation ; donne un sens à « Reine morte seule ». |
| Reine tuée avant toute Arche | L'Essaim est détruit (pas de couvain royal). | Seul cas où tuer la Reine suffit ; fenêtre courte et dangereuse. |
| Monde mort | Plus aucune cité vivante ET (il ne reste pas de quoi finir l'Arche, OU elle n'avance plus depuis 150 jours), ou l'Essaim s'éteint sur un monde vide. | Troisième issue demandée ; garantit que toute partie se termine. |
| Mucus hors de portée | Une case à plus de `claimRadius + 1` de toute ruche se dessèche (5 %/jour). | Stimulus « éloignement des ruches ». |
| Armées humaines | Une armée qui marche sur du mucus sans ruche ni couvée le brûle. | Donne aux factions un moyen juste de contenir l'Essaim. |
| Ouvrières | Transférées entre ruches sous forme de couvées qui marchent réellement. | Pas de téléportation ; visible en 3D plus tard. |
| Joueur | Némésis n°0 (« L'Anonyme »), avec niveau et mémoire comme les autres. | Pilier Némésis. |

## J1 — Le projet tourne — TERMINÉ (14/11/2026)

**Fait**
- `Source/Swarm/Sim/SwarmSim.h/.cpp` : simulation complète réécrite, C++17 standard, zéro include Unreal.
  Monde 48×48, 5 factions (économie, recrutement, diplomatie, guerres, crises, successions selon le régime,
  camps nomades), Essaim (récolte finie, entretien, stimuli qui se partagent le budget, castes, couvées, ruches,
  Arche, Reine, état féral), combat par case, Némésis (mémoire, cicatrices, adaptations, rivaux), chronique, trois issues.
- `Source/Swarm/Game/` : `USwarmWorldSubsystem` (fait tourner la simulation, vitesse, pause, actions `Player*`),
  `ASwarmDebugHUD` (carte, état, chronique), `ASwarmGameMode`, `USwarmSettings` (graine, secondes par jour).
- `Tools/Headless/` : exécutable d'équilibrage (`--seeds`, `--chronicle`, `--trace`, `--dump`).
- Tests : `Swarm.Sim.Determinism`, `Swarm.Sim.PlayerInterface`, `Swarm.Bridge.DayAdvances`.

**Vérifié**
- Compilation `SwarmEditor Win64 Development` : succès, aucun avertissement dans le module. (testé)
- Headless : compile en `/W4` sans avertissement, 40 mondes en moins de 2 s. (testé)
- Les 3 tests passent en `-game -RenderOffscreen`. (testé)
- Capture `Saved/Screenshots/Swarm/J1_debug.png` regardée : carte des biomes, territoires, cités, ruche, mucus,
  état de l'Essaim, tableau des factions et chronique lisibles ; le jour avance (2 jours en 0,6 s à 0,5 s/jour). (testé)

**Pas vérifié**
- L'éditeur n'a pas été ouvert en fenêtre ni « Play » cliqué (consigne : rien sur l'écran principal). Le chemin
  `-game` exécute le même GameMode, le même HUD et le même sous-système que Play. À confirmer par le propriétaire
  en ouvrant `Swarm.uproject` puis Play.

**Fragile**
- Équilibrage : à ce stade l'Essaim perd presque tous les mondes. C'est l'objet de J2.
- Les couleurs de la carte debug sont délavées (couleurs linéaires passées telles quelles au canevas). Cosmétique.

## J2 — Simulation équilibrée — TERMINÉ (14/11/2026)

**Point de départ** : la simulation réécrite donnait l'Essaim perdant dans 100 % des mondes, en 150 jours de médiane.

**Résultat** (réglages par défaut, monde sans joueur) : Arche partie 80 % sur les graines 1-100 et 80 % sur les
graines 101-400 ; 81 % des parties durent entre 600 et 1000 jours ; médiane 731 jours ; aucune partie sans fin ;
tension croissante de 6 % à 63 % de la carte. Détail dans le README.

**Ce qui a été changé, dans l'ordre où les problèmes sont apparus** (aucun ne donne de ressource) :
1. *Achat des castes* : la part de budget d'une ruche se cumule en crédit, sinon lames et cuirassés n'étaient jamais
   achetés et le minerai dormait.
2. *Logistique* : rayon de récolte 4, ruches espacées de 7, mucus jusqu'à 7 cases ; le mucus prend d'abord les
   cases qu'une ruche peut récolter.
3. *Stimuli* fondés sur la valeur restante des cases (frontière, éloignement) et non sur des comptages.
4. *Ralliement* : les ruches sans proie envoient leurs soldats à la ruche du front la plus proche ; les ruches
   voisines secourent une ruche menacée ; une couvée ne sort contre une armée que si elle est plus forte.
5. *Ruches naissantes* : éviter de fonder sous les murs d'une cité ; priorité pour recevoir des soldats ; ruches
   plus solides (1500 PV, les structures tombent moins vite).
6. *Famine* : la colonie affamée entame sa réserve pour élever des ouvrières, résorbe ses soldats au repos (50 % de
   perte), affame ses soldats avant ses ouvrières ; les ouvrières oisives comptent avant d'en élever d'autres.
7. *Minerai* : il ne compte dans la valeur d'une case que si la colonie en manque (sinon elle fondait en boucle des
   ruches sur des zones sans biomasse).
8. *Arche* : appel proportionnel au territoire ; vitesse de chantier plafonnée ; la colonie épargne pour l'Arche
   et se sacrifie avant de toucher à cette épargne ; épargne perdue si l'Arche est détruite.
9. *Factions* : part de la population sous les armes ×0,9 ; levée face à l'Essaim +110 % au lieu de +120 % ;
   une ruche n'est frappée que si elle est « sur leurs terres » (6 cases) tant que la menace n'est pas admise.
10. *Monde mort* : troisième issue (voir décisions).

**Défauts trouvés et corrigés en route**
- Lecture hors limites d'un tableau par ruche quand une ruche était fondée le jour même : certains mondes n'étaient
  pas reproductibles d'un lancement à l'autre. La récolte disponible est maintenant un champ de `Hive`.
- Arche bloquée à 99,99 % par l'arrondi des flottants : 5 mondes sur 100 ne se terminaient pas comme ils devaient.

**Vérifié** (testé)
- Headless : 100 graines, puis 300 autres, résultats ci-dessus. Deux lancements identiques donnent des sorties identiques.
- 6 tests d'automatisation passent hors écran : `Determinism`, `PlayerInterface`, `NoFreeResources`,
  `WorldDeadOutcome`, `FullGame`, `Bridge.DayAdvances`.
- Aperçu animé du monde 5 regardé : expansion, mucus qui pâlit à l'ouest une fois dévoré, cités encerclées.

**Fragile**
- Les victoires de l'Essaim se terminent presque toutes entre 715 et 770 jours : c'est le plafond du chantier qui
  fixe la durée. Peu de variété d'un monde à l'autre sur ce point.
- Les factions gagnent surtout tôt (mauvais point de chute de l'Essaim). Une coalition tue rarement un Essaim mûr :
  c'est voulu (c'est le rôle du joueur), mais cela rend les fins de partie humaines peu variées.
- « Monde mort » ne se produit pas sans joueur avec les réglages par défaut.
- Le minerai est abondant pour l'Essaim : c'est la biomasse qui le limite. Le défaut « plus de minerai pour finir
  l'Arche » de l'ancienne version n'existe pas ici.
- Cités indépendantes figées (ni économie ni armée).
- Outil : `Tools/Headless/sweep.ps1` + `--set clé=valeur` pour tout nouveau réglage ; `--hives graine jour` pour
  diagnostiquer la logistique.

## J3 — Le monde en 3D — TERMINÉ (14/11/2026)

**Fait**
- Une case = 200 m (justification dans `SwarmWorldLayout.h`). Monde de 9,6 km de côté, centré sur l'origine.
- `ASwarmTerrain` : relief généré au lancement depuis la carte (même graine = même relief) : interpolation bicubique
  des hauteurs par biome + bruit de détail (crêtes en montagne, dunes au désert). 144 morceaux de maillage
  procédural avec collision, 923 000 échantillons de hauteur. Mer : un plan.
- Mucus : texture 48×48 mise à jour chaque jour (couverture, richesse), fondue entre hier et aujourd'hui par le
  matériau du sol, bords organiques, veines magenta qui s'éteignent si l'Essaim est féral, pâleur quand la case s'épuise.
- `ASwarmSky` : soleil, lune, atmosphère, nuages volumétriques, brume de hauteur volumétrique, étalonnage, cycle
  jour/nuit calé sur le jour de simulation (jour 70 %, nuit 30 %). Lumen avec ray tracing matériel.
- `ASwarmStructures` : cités (bâtisses selon la population, remparts selon la fortification, édifice propre à
  chaque faction), ruines, ruches (taille selon la population, épines selon les soldats, sacs selon les ouvrières),
  ruche de la Reine, Arche (hauteur et membrures selon l'avancement), arbres (disparaissent avec la biomasse), rochers.
- `Tools/Editor/setup_assets.py` : crée les 4 matériaux (nœuds Custom HLSL), 4 maillages Nanite, le niveau. Sans fenêtre.

**Vérifié** (testé, hors écran)
- `Swarm.World.Terrain` : eau sous la mer, terre au-dessus, montagnes dominantes, 1536 lignes tirées du ciel
  touchent toutes le sol à la hauteur annoncée.
- `Swarm.World.Structures`, `Swarm.World.Screenshots`.
- Captures regardées (`Docs/Captures/J3_*.png`) : vue d'ensemble, cité du Concordat, ruche de la Reine, Arche,
  front du mucus. On reconnaît chaque élément.

**Décisions**
- Terrain en maillage procédural au runtime plutôt qu'en Landscape : c'est la seule façon d'avoir n'importe quelle
  graine sans étape d'éditeur. Conséquence : le sol n'a pas de cache de surface Lumen ; on compense par
  `r.Lumen.HardwareRayTracing.LightingMode=1` (éclairage calculé au point d'impact). Coût GPU plus élevé, acceptable
  sur une RTX 5080 (voir J4). Si un jour on fige la graine, un Landscape cuit serait plus beau et moins cher.
- Matériaux écrits en HLSL dans des nœuds Custom, bruit procédural : aucune texture externe.
- Serveur MCP : UE 5.8 fournit un plugin expérimental `ModelContextProtocol` (« Unreal MCP »). Non utilisé : il vit
  dans l'éditeur ouvert en fenêtre, ce qui est interdit ce soir. Le commandlet Python suffit pour l'instant.
- DLSS : NVIDIA publie un plugin officiel DLSS 4.5 pour UE 5.8 (compte développeur requis, ghosting signalé sur
  certains préréglages). Non installé : téléchargement à faire par le propriétaire. TSR en attendant.

**Écart avec la cible graphique (honnête)**
- Acquis : éclairage dynamique, ciel, nuages, brume, ombres douces, étalonnage. C'est la base juste.
- Très loin : tout le reste. Sol sans texture scannée (couleurs et bruit), aucune végétation réelle (des cônes),
  cités et ruches en formes géométriques, eau opaque. C'est un blockout lisible, pas un rendu final. J10.

**Fragile**
- De près et en vue rasante, le relief fin du sol fait des traînées.
- Le sol a 10 m entre deux points : pas de détail géométrique sous cette échelle.
- Les camps nomades qui migrent sont redessinés chaque jour de marche (reconstruction complète des instances).
- Le mannequin n'apparaît pas dans les captures de test (caméra libre).

## J4 — Le joueur — TERMINÉ (14/11/2026)

**Fait**
- `ASwarmPlayerCharacter` : marche, course (Maj), saut, caméra épaule avec inertie, ZQSD + souris, manette.
  Entrées Enhanced Input créées en C++ (aucun asset). Corps : mannequin d'Epic copié des modèles du moteur.
- Santé, faim (25 par jour de monde, 40 par ration), inventaire, cueillette : `PlayerForage` retire la biomasse
  de la case dans la simulation (rien sur le mucus ni sur l'eau).
- Temps du monde : pause (P), vitesse ×0,25 à ×32 (Page haut / bas). Un jour = 240 s à vitesse 1.
- Apparition devant la porte de la plus petite cité vivante. Affichage : santé, faim, sac, jour, heure, vitesse.

**Vérifié** (testé, hors écran)
- `Swarm.Player.Spawn`, `Needs` (biomasse cueillie retirée au gramme près), `TimeControl`.
- `Swarm.Player.Traverse` en 2560×1440 : d'Aram (42,3) à la ruche n°18 (4,47), 48 cases, **12,0 km en 270 s,
  147 images/s en moyenne, 102 images/s pour le 1 % le plus lent, pire image 28 ms, jamais sous le sol.**
  Le personnage est piloté par le test à 45 m/s le long d'un chemin de cases sans eau ni montagne.

**Pas vérifié**
- Aucune main humaine sur le clavier, la souris ou la manette : les liaisons de touches sont écrites et compilées,
  pas essayées. À confirmer par le propriétaire.
- L'animation du mannequin n'a pas été regardée.
- Motion Matching : le plugin PoseSearch est présent, mais il faut une base d'animations (pack gratuit « Game
  Animation Sample » sur Fab). Non fait ; locomotion du modèle Third Person en attendant.

**Fragile**
- Mort : message à l'écran, pas de réapparition ni d'écran de fin.
- L'avance rapide des tests (`AdvanceDays`) affame le personnage : normal, mais à connaître.
- Les montagnes sont raides ; l'angle de marche est à 55°. Pas de nage : on marche au fond de l'eau.
- La mesure de cadence est faite hors écran, sans foule ni combat : elle ne vaut que pour le monde nu.

## J5 — Matérialisation — TERMINÉ (14/11/2026)

**Fait**
- `ASwarmCrowd` : à moins de 2 cases du joueur (400 m), groupes en marche, garnisons de cités et population des
  ruches deviennent des unités visibles ; au-delà de 3 cases elles redeviennent des nombres. Les unités sont des
  instances (une composante par type), pas des acteurs : soldats aux couleurs de leur faction, cinq castes de l'Essaim.
- La simulation reste la référence : les effectifs affichés sont recalés sur elle à chaque nouveau jour et deux
  fois par seconde ; une unité tuée en 3D est retirée de la simulation au moment même par `PlayerDamageGroup`,
  `PlayerDamageHiveUnits` ou `PlayerDamageGarrison` (les deux dernières ajoutées à la simulation pour ce jalon).

**Vérifié** (testé, hors écran) — `Swarm.Crowd.Conservation`
- Ruche : chaque caste matérialisée = effectif de la simulation ; 3 essaimeurs tués en 3D = 3 de moins dans la
  simulation ; impossible d'en tuer plus qu'il n'y en a ; dématérialisée puis rematérialisée = même effectif.
- Garnison de cité et groupe en marche : mêmes vérifications.
- Après 10 jours de simulation, la 3D est recalée sur la simulation, caste par caste.
- 14 tests passent au total. Headless : 80 % / 81 %, inchangé.

**Pas vérifié**
- Aucune capture de la foule n'a été regardée. Les formes (cylindres, cônes, sphères) n'ont été vues par personne.
- Coût en images/s avec une grosse foule (plusieurs milliers d'unités près d'une grande ruche) : non mesuré.

**Fragile**
- Les unités tournent en rond autour de leur place : c'est une présence, pas un comportement. Elles ne se battent
  pas, ne marchent pas avec leur groupe, n'ont pas de collision. C'est l'objet du J6.
- Quand la simulation retire des unités (combat simulé), ce sont les dernières créées qui disparaissent, sans mise en scène.
- Mise à jour de toutes les instances à chaque image : à revoir si la foule dépasse quelques milliers.

## Reprise — où en est-on, quoi faire ensuite

État au 14/11/2026 au soir : J1 à J5 terminés et validés dans git. J6 à J10 non commencés.
Prochain jalon : **J6 — Combat**. Ce qu'il faut construire sur l'existant :
- armes du joueur (tir par ligne, corps à corps) qui appellent `ASwarmCrowd::KillUnits` ; dégâts localisés ;
- comportement des unités matérialisées (elles n'en ont aucun) : marche avec le groupe, combat entre camps hostiles,
  couverture et repli pour les humains, quatre comportements de castes ; les pertes passent par les fonctions
  `Player*` ou par une nouvelle fonction de résolution locale dans la simulation ;
- pendant qu'un combat est matérialisé, décider qui résout : la 3D (et la simulation suspend son `StepCombat`
  pour cette case) ou la simulation (et la 3D met en scène). Non tranché. Le critère du J6 (« un siège se joue
  en 3D du début à la fin et son issue modifie la simulation ») impose la première option pour la case jouée.
Avant d'écrire le J6, le propriétaire devrait avoir essayé le jeu une fois (commandes, caméra).

## J6 — Combat — TERMINÉ (14/11/2026)

**Décision de fond : qui résout le combat ?**
Dans les cases matérialisées autour du joueur, c'est la 3D. La simulation n'y échange plus de coups
(`PlayerSetLiveCells`) ; chaque mort en 3D lui est rapportée au moment même, avec son auteur
(`PlayerDamageGroup` / `PlayerDamageGarrison` / `PlayerDamageHiveUnits`, paramètre `killerOwner` ; `kOwnerPlayer`
pour le joueur lui-même). La simulation en tire les mêmes conséquences que pour ses propres combats : pertes face
à l'Essaim, usure de guerre, repli des armées, fuite des champions, mémoire des némésis, chronique. Une cité dont
le dernier défenseur tombe avec l'ennemi dans ses murs chute à l'instant (`ResolveUndefendedSettlement`).
Partout ailleurs la simulation continue de résoudre seule, comme avant : équilibrage sur 100 graines strictement
identique (80 % / 81 %).

**Fait**
- `ASwarmCrowd` réécrit : les unités matérialisées perçoivent (grille spatiale), choisissent une cible, se
  déplacent, attaquent. Toujours des instances, pas des acteurs.
  - Soldats humains : tir à 45 m, précision qui chute avec la distance ; cherchent un couvert (rocher, bâtisse)
    face à qui tire ; reculent en tirant devant le corps à corps ; repli individuel quand ils sont blessés ; les
    garnisons tiennent leurs murs sans poursuivre.
  - Essaimeur : charge en zigzag. Lame : bond sur les derniers mètres. Cuirassé : lent, ses plaques arrêtent les
    balles de face. Garde royale : ne quitte pas sa ruche. Ouvrière : fuit.
  - Les rapports de force suivent la simulation (testé) : dégâts par seconde et points de vie proportionnels à la
    force et à la robustesse des castes ; remparts et ruches protègent les leurs du même facteur.
- Joueur : barre de fer (corps à corps en arc), fusil (ligne de tir arrêtée par le décor), visée, changement
  d'arme, recul, viseur. Dégâts localisés : tête / torse / jambes pour un soldat, dos du cuirassé, tête de la
  garde royale et de la lame. Tirer dans une ruche entame sa structure. Un soldat tué laisse parfois fusil et
  munitions. Les unités de l'Essaim attaquent le joueur ; une faction dont il tue un soldat lui devient hostile.
- Mort du joueur : il reprend connaissance au point de départ après 5 s, affaibli (provisoire).
- Corps au sol, traits de tir, butin visible.

**Vérifié** (testé, hors écran)
- `Swarm.Combat.D_Siege` (critère) : l'Essaim assiège Haut-Four ; le combat se joue en 3D ; à chaque image la 3D
  et la simulation ont le même effectif ; au dernier défenseur la cité est dévorée dans la simulation et la
  chronique le raconte. Rejoué 4 fois avec des tirages différents : 4 fins nettes.
- `Swarm.Combat.C_FactionBattle` : une armée assiège une cité ennemie ; la cité est prise.
- `Swarm.Combat.B_PlayerRifle` : 2 balles pour un essaimeur protégé par sa ruche, 1 de moins dans la simulation ;
  corps à corps sur une ouvrière. `Swarm.Combat.A_HitZones`. `Swarm.Sim.LiveCells` (dont : le joueur anéantit un
  groupe mené par un némésis, qui s'en souvient, le prend pour rival et s'adapte au fusil).
- 20 tests passent au total. Traversée en 1440p avec la foule et les combats actifs : 156 images/s en moyenne,
  129 pour le 1 % le plus lent.
- Captures regardées : `Docs/Captures/J6_siege_1_pendant.png` (soldats, traits de tir, corps, cuirassé dans les murs).

**Défaut grave trouvé en route**
- Le projet n'avait pas de `Config/DefaultInput.ini` : le moteur créait un composant d'entrée ancien modèle et
  **aucune touche n'aurait répondu en jeu**. Trouvé parce que l'éditeur ouvert par le propriétaire a corrigé ça
  de lui-même dans sa copie. Corrigé, et `Swarm.Player.Spawn` vérifie maintenant que les actions sont liées.
  Leçon : « compilé et testé hors écran » ne prouvait rien sur les touches ; le journal du J4 le disait, c'était vrai.

**Pas vérifié**
- Toujours aucune main humaine sur les commandes. Le tir, la visée et le corps à corps ne sont testés que par
  appel direct des fonctions.
- Le combat vu de près, à hauteur de joueur : aucune capture regardée à cette échelle. De loin les unités font
  1,8 m dans une cité de 150 m : on voit des points.
- Charge de plusieurs milliers d'unités : la traversée mesurée n'en a croisé que 102 à la fois.
- Un enlisement (5 défenseurs contre 12 assaillants qui ne se tuaient plus) a été vu une fois, avant les
  corrections, et pas reproduit ensuite en 5 essais. Cause non identifiée. Le test échoue après 90 s si ça revient.

**Fragile**
- Un combat en 3D se règle en une minute de temps réel, soit quelques millièmes de jour : là où le joueur
  regarde, la guerre va bien plus vite que dans la simulation (10 % de pertes par jour). Assumé, à surveiller.
- Les tireurs ont l'avantage sur les corps à corps pendant l'approche : en 3D une couvée a besoin de plus de
  monde qu'en simulation pour prendre une cité (78 contre 32 a laissé 1 survivant). Les sièges vus par le joueur
  sont donc plus durs pour l'Essaim que les autres.
- Pas de collision entre unités et décor : elles traversent murs et bâtisses (« elles grimpent »). Pas de
  recherche de chemin. Pas de ligne de vue pour les tirs des soldats.
- Tous les soldats humains ont les mêmes valeurs en 3D ; la qualité propre à chaque faction n'y est pas appliquée.
- Les unités sont des formes géométriques sans animation. Aucun son.
- L'hostilité des factions envers le joueur est un simple drapeau dans la foule ; la vraie réputation est au J7.
- La compilation pendant que l'éditeur est ouvert demande `-NoHotReloadFromIDE` (Live Coding verrouille la cible).

## Reprise — où en est-on, quoi faire ensuite

État : J1 à J6 terminés et validés dans git. J7 à J10 non commencés.
Prochain jalon : **J7 — Factions vivantes** : réputation par faction (remplacer le drapeau `PlayerEnemies` de
`ASwarmCrowd` par un état dans la simulation), dialogues courts, travail, commerce (c'est là que le joueur doit
pouvoir acheter fusil, munitions, rations), compagnons, rejoindre ou trahir ; chefs et officiers comme personnages
qu'on peut rencontrer et tuer, leur mort déclenchant `DoSuccession` (critère). Il faudra une fonction
`PlayerKillNemesis` dans la simulation et des unités nommées dans la foule.

## J7 — Factions vivantes — TERMINÉ (14/11/2026)

**Fait**
- Simulation : réputation du joueur par faction (-100..100) avec quatre seuils (tir à vue, commerce et travail,
  recrue, adhésion). Elle baisse quand il tue des soldats (-8), un officier (-25), un chef (-50), quand il trahit
  (-60) ; elle monte par le travail (+3), le commerce (+1), et quand il tue l'Essaim auprès des factions qui ont
  compris la menace. Le temps apaise lentement les rancunes.
- Nouvelles actions du joueur dans la simulation, toutes payées sur des comptes réels :
  `PlayerWork` (le salaire sort du grenier), `PlayerBuy` (munitions et fusil contre du minerai de l'arsenal, une
  recrue contre un soldat de la garnison ; le prix en rations entre au grenier), `PlayerReturnSoldier`,
  `PlayerJoinFaction`, `PlayerLeaveFaction`, `PlayerKillNemesis` (si c'est un chef : succession immédiate selon la
  règle du régime).
- Chefs et officiers sont présents en personne dans leur cité quand elle est matérialisée (plus grands, couleur
  vive ; le chef devant l'édifice central). On leur parle, on travaille pour eux, on peut les tuer.
- Conversation (E près de quelqu'un) : ce que dit l'interlocuteur vient de l'état de sa faction — régime,
  réputation du joueur, famine, crise, guerre en cours et contre qui, conscience de l'Essaim ; un chef dit comment
  on lui succédera. Options selon la réputation : travailler une journée, acheter munitions ou fusil, recruter un
  compagnon, le rendre, rejoindre, trahir.
- Compagnons : un soldat recruté quitte la garnison dans la simulation, suit le joueur et se bat pour lui.
- Rejoindre une faction : ses ennemies deviennent hostiles au joueur.
- Annonces : les événements de la chronique s'affichent en haut à gauche au moment où ils arrivent.

**Vérifié** (testé, hors écran)
- `Swarm.Factions.KillLeaderInGame` (critère) : le chef de Hael est présent dans sa capitale ; on lui parle (il se
  nomme, il dit « je commande parce que je suis le plus fort ») ; on travaille une journée pour lui (2 rations, le
  jour passe) ; on lui achète 10 munitions ; on le tue de 2 balles dans la tête ; le plus fort des officiers lui
  succède aussitôt ; l'annonce s'affiche ; le nouveau chef est en personne dans la capitale ; Hael devient hostile.
- `Swarm.Factions.SuccessionRules` : pour les cinq régimes, le successeur est celui que désigne la règle (le plus
  ancien, le plus riche, le plus fort ; duel et vote vérifiés par la chronique).
- `Swarm.Factions.Economy` : salaire, minerai, prix et soldat de la recrue sortent et entrent au gramme près.
- 23 tests passent au total (traversée comprise). Équilibrage sur 100 graines inchangé (80 % / 81 %).

**Pas vérifié**
- La conversation n'a été vue par personne : ni la boîte de dialogue, ni les annonces. Aucune capture regardée.
- Navigation du dialogue au clavier (Z / S / E) : écrite, pas essayée.
- Les compagnons au combat : leur code est celui des soldats ; aucun test ne les fait se battre.

**Fragile**
- Un compagnon tué disparaît sans autre conséquence ; un compagnon laissé loin est perdu de vue (il n'existe
  plus que dans la foule matérialisée, plus dans la simulation).
- Les notables ne se défendent pas et les autres unités les ignorent : seul le joueur peut les tuer en 3D.
- Travailler fait passer un jour entier d'un coup, sans transition.
- Pas de monnaie : tout se paie en rations. Simple, mais le commerce est pauvre (trois biens).
- Les répliques sont des phrases fixes choisies selon l'état, pas des phrases composées ; peu de variété.
- Cités indépendantes : on ne peut rien leur demander.

## J8 — Némésis — TERMINÉ (14/11/2026)

**Fait**
- Les némésis existent en personne : un champion entre deux couvées se tient devant sa ruche ; celui qui mène une
  couvée ou une armée marche à sa tête. Ils ne s'occupent que du joueur (« c'est entre eux deux »).
- Vaincu par le joueur, un champion ou un meneur ne meurt pas : il s'arrache au combat et disparaît
  (`PlayerNemesisEncounter`). Il quitte son groupe, se remet 25 à 60 jours hors de vue, puis reparaît. À la
  troisième défaite il n'a plus la force de fuir et meurt. Un résident de cité, lui, meurt (J7).
- Il revient changé : une cicatrice de plus, le joueur pour rival, et une adaptation à l'arme qui l'a battu —
  carapace épaissie contre le fusil, vivacité contre la barre de fer. L'adaptation se sent : cette arme lui fait
  40 % de dégâts en moins. S'il tue le joueur, il monte d'un niveau.
- Son apparence dit son histoire : taille selon rang et niveau, carrure selon les adaptations, lueur des
  cicatrices pour un champion.
- Reconnaissance : à moins de 30 m, un bandeau donne son nom, son rang, son niveau, ses traits, et sa réplique.
  La réplique est composée par la simulation à partir de sa mémoire (`NemesisLine`) : jour, arme, issue de la
  dernière rencontre, ce qu'il en a gardé, nombre de cicatrices.

**Vérifié** (testé, hors écran)
- `Swarm.Nemesis.ChampionReturns` (critère) : au jour 138, le champion Xaael Fend-Acier (niveau 1, aucun trait)
  ne connaît pas le joueur. 14 balles le mettent en fuite : vivant, une cicatrice, le joueur pour rival, invisible
  tant qu'il se remet. Au jour 180 il est de retour devant sa ruche, « balafré, carapace épaissie », plus large,
  plus lumineux, et dit : « Jour 138. Ton fusil. La chair se souvient. Elle a repoussé plus épaisse. Tes balles ne
  mordront plus. » Une balle au même endroit lui fait moins mal qu'avant.
- `Swarm.Nemesis.Lines` : un officier battu à la barre de fer cite le jour et l'arme et devient « vif » ;
  vainqueur du joueur, il le rappelle et monte en grade.
- 25 tests passent au total. Équilibrage sur 100 graines inchangé.

**Pas vérifié**
- Le bandeau et l'allure des némésis n'ont été vus par personne.
- Un champion qui tue réellement le joueur en jeu : le branchement est écrit, aucun test ne le joue.
- Un officier humain menant une armée, battu puis revenu : même code que le champion, pas de test dédié.

**Fragile**
- Les répliques sont composées de morceaux fixes : une dizaine de tournures. Justes, mais vite répétitives.
- La « promotion » n'existe que pour celui qui bat le joueur (ou gagne une bataille simulée).
- Un némésis ne se bat que contre le joueur et les autres unités ne peuvent pas le toucher.
- Les champions utilisent la forme de la garde royale, en plus grand : rien ne les distingue d'elle que la taille
  et la lueur.

## Reprise — où en est-on, quoi faire ensuite

État : J1 à J8 terminés et validés dans git. J9 et J10 non commencés.
**J9 — La fin** : ruche de la Reine et Arche comme lieux jouables (percer la garde royale pour atteindre la Reine,
entrer dans l'Arche pour la saboter), trois écrans de fin. Déjà en place dans la simulation : `PlayerDamageQueen`
(sans effet tant qu'il reste des gardes), `PlayerSabotageArk`, les trois issues. Manque : la Reine et l'intérieur
de l'Arche en 3D, l'action de sabotage, les écrans de fin, et un test qui atteint les trois fins dans une vraie partie.
**J10 — Rendu final** : demande des téléchargements que seul le propriétaire peut faire (packs Fab, MetaHuman,
plugin DLSS). La liste est à lui donner au début du jalon.

## J9 — La fin — TERMINÉ (14/11/2026)

**Fait**
- La Reine existe en personne, dans une alcôve d'épines au pied de sa ruche. Sa garde royale fait cercle autour
  d'elle. Tant qu'un garde vit, une balle sur la Reine ne lui fait rien (c'est la simulation qui tranche, pas
  la 3D). La garde percée, la Reine a 600 points de vie.
- L'Arche est creuse : une paroi en anneau, une brèche côté +X, le couvain royal au centre. Sa garde tient la
  brèche. Le sabotage (touche E) n'est possible que depuis la salle du couvain ; de l'extérieur il ne fait rien.
- Trois écrans de fin (Essaim détruit, Arche partie, monde mort) avec le bilan de la partie. Entrée relance un
  autre monde avec la graine suivante.

**Vérifié** (testé, hors écran)
- `Swarm.Ending.PlayTheVictory` (en jeu) : partie laissée à elle-même jusqu'au jour 313 (Arche à 25 %) ; la Reine
  est là, sa garde matérialisée à l'unité près et autour d'elle ; une balle sur la Reine : zéro dégât ; la garde
  tuée, 18 balles tuent la Reine ; l'Essaim devient féral mais la partie continue ; dehors, sabotage impossible ;
  dans la salle du couvain, 10 gestes détruisent l'Arche ; issue « Essaim détruit » ; écran de fin regardé
  (`Docs/Captures/J9_ecran_de_fin.png`).
- `Swarm.Ending.Rules` : Reine morte seule = féral, partie continue ; Arche détruite seule = la Reine en rebâtit
  une ; personne n'intervient = l'Arche part (graine 2) ; **le joueur sabote l'Arche à 80 % sans tuer la Reine =
  monde mort au jour 749 (graine 3)**. Les trois fins sont donc atteignables, chacune par des actions du joueur
  ou par son absence.
- 26 tests passent au total. Équilibrage sur 100 graines inchangé.

**Limites honnêtes du critère « atteignables dans une vraie partie »**
- La victoire est jouée dans le vrai jeu, mais par un test : le joueur est téléporté devant la Reine puis dans
  l'Arche, la garde est tuée d'un appel, il est invulnérable. Personne n'a traversé la carte, percé la garde au
  fusil et survécu. Rien ne dit que c'est faisable, ni que c'est bien dosé, pour un humain.
- « Arche partie » et « monde mort » sont atteints dans la simulation seule, pas dans le jeu en 3D (les écrans
  correspondants n'ont pas été capturés ; seul celui de la victoire l'a été).

**Fragile**
- La garde royale près de la Reine : 9 gardes au jour 313, à 280 points de vie chacun. Un joueur seul avec un
  fusil à un coup n'a sans doute aucune chance ; il lui faut des compagnons ou une faction derrière lui. Pas testé.
- L'intérieur de l'Arche est une salle ronde vide de 10 m de rayon avec des sphères lumineuses. Pas de décor.
- Pas de mise en scène : la Reine disparaît quand elle meurt, l'Arche aussi. Le décollage de l'Arche n'est pas montré.
- Textes de l'interface sans accents (titres, messages) : à reprendre avec l'interface du J10.
- La relance d'un autre monde (touche Entrée) est écrite, pas essayée.

## J10 — Rendu final — BLOQUÉ, à peine entamé (14/11/2026)

**Fait**
- Préréglages de qualité (bas, moyen, élevé, épique, cinématique), touche O, enregistrés. Testé :
  `Swarm.Player.QualityPresets`. Deux réglages forcés dans `DefaultEngine.ini` qui empêchaient les préréglages
  bas de couper le brouillard volumétrique ont été retirés.
- `Docs/A_TELECHARGER.md` : la liste unique des packs. Je n'ai pas accès à Fab : pour Megascans et Megaplants ce
  sont des termes de recherche, pas des noms de fiches vérifiés. Sources : annonces d'Epic et presse de novembre 2026.

**Bloqué par** : les téléchargements (Fab, NVIDIA), la création des MetaHuman (travail à la main dans l'éditeur),
et surtout un regard humain. Aucune image de ce jeu n'a encore été vue en mouvement par quelqu'un.

**Pas commencé** : remplacement des formes provisoires, effets Niagara, son, interface, menu, sauvegarde,
passe d'éclairage finale, mesure à 60 images/s avec le contenu final, bilan d'écart avec la cible.
La sauvegarde demande de sérialiser toute la simulation : à prévoir comme un chantier à part.

**Écart avec la cible graphique, aujourd'hui**
Lumière, ciel, nuages, brume, ombres, étalonnage : en place et dynamiques. Tout ce qui est posé dans le monde est
une forme géométrique unie. On est au niveau d'un prototype de conception, très loin d'Arc Raiders.

## J10 — Rendu, suite (soir du 14/11/2026) — consigné après coup

Le propriétaire a joué et jugé : graphismes, carte, rythme et jouabilité « pas à la hauteur ». Fait ce soir-là, en
réponse (commit `c35d2b9`) : vue stratégique sur le vrai monde (touche M : territoires peints sur le sol, cités
cliquables, fiches, calques, niveau de détail), vitesses jusqu'à 32 jours par seconde en vue stratégique, textures
photo du sol et des bâtisses (Poly Haven, CC0), menu de test (touche T), DLSS (touche U ; mesuré : 187 images/s en
mode Qualité contre 149 sans, à 1440p ; le mode DLAA tombait à 4 images/s).

## J11 — Unités, accueil, sauvegarde, nature (15/11/2026)

Le propriétaire a rejoué : « quelques soucis mais globalement ça va, avance ». Il demande aussi de ménager le disque.

**Fait**
- **Soldats** : chaque humain matérialisé a un squelette (mannequin d'Epic) pris dans une réserve : attente arme à
  l'épaule, marche, course, mort (le corps reste à terre, 40 au plus). Torse à la couleur du camp. Un vrai fusil
  (maillage généré) dans la main ; le joueur a le même.
- **Essaim** : les cinq castes et la Reine sont des maillages générés par `Tools/Assets/make_meshes.py` (Python pur,
  aperçu PNG sans moteur) : tique gorgée pour l'ouvrière, coureur à faux pour l'essaimeur, mante pour la lame, char à
  plaques pour le cuirassé, échassier à lances pour la garde royale. Pas de squelette : l'animation est dans le
  matériau `M_ChitinUnit` (les UV disent quel sommet bouge, de combien et à quel moment du pas). Les pas suivent la
  distance réellement parcourue. Les dépouilles restent sur le flanc, éteintes. Épines et cosses des ruches
  remplacent cônes et sphères.
- **Accueil** (`ASwarmFrontEnd`) : cinématique de 15 s tournée dans le monde qui vient d'être généré (trois plans,
  trois heures du jour, une phrase chacun), écran-titre (Jouer, Reprendre, Touches, Quitter), pause sur Échap,
  réglage des touches (clic ou Entrée, puis la touche ; deux actions échangent leurs touches plutôt que d'en partager
  une ; enregistré dans GameUserSettings, section `Swarm.Keys`). Souris, clavier et manette.
- **Sauvegarde** : `Simulation::Save/Load` en C++ standard (un seul code pour lire et écrire), fichier
  `Saved/SaveGames/Swarm.sav` écrit quand on quitte depuis la pause ; « Reprendre » recharge le monde, l'Anonyme, son
  sac et ses compagnons.
- **Interface à pied** refaite (vie et faim en traits fins, arme et munitions, heure, boussole) ; plus aucun rappel de
  touche permanent (H ouvre le réglage des touches). Police Roboto nette à toute résolution.
- **Monde** : le sol est nivelé sous chaque cité (elles étaient bâties à flanc de montagne, maisons de travers) ;
  vrais arbres, buissons et rochers (Poly Haven, CC0) à la place des cônes et des cubes ; graine libre par défaut
  (`Seed=0` : un monde différent à chaque lancement ; les tests gardent le monde n° 1).
- **Lanceur Epic** : il ne listait pas le projet (le moteur est enregistré par un GUID, le lanceur cherchait donc les
  projets récents dans `Engine/Saved/Config/WindowsEditor`, qui n'existait pas). Corrigé en y écrivant
  `EditorSettings.ini`. Le propriétaire peut maintenant faire « Ajouter au projet » depuis sa bibliothèque Fab.

**Deux défauts trouvés par les tests, corrigés**
- Une unité adossée à un mur était intouchable : la balle s'arrêtait sur le mur, quelques centimètres avant son axe
  (défaut introduit par les bâtisses détaillées du 14 au soir, que personne n'avait relancé les tests pour voir).
  `TraceUnits` touche maintenant l'unité là où la ligne entre dans son volume ; et les unités ne restent plus dans
  les maisons ni les gros rochers (`ASwarmStructures::PushOut`).
- La collision des nouvelles épines était un prisme invisible autour d'elles : remplacée.

**Vérifié** (tests hors écran, captures regardées)
Suite complète lancée le 15/11 après tout ce qui précède (33 tests, hors écran, 1440p pour les mesures) :
**29 passent, 4 échouent.**

- Passent : toute la simulation (dont le nouveau `Swarm.Sim.SaveLoad` : un monde sauvé au jour 260, rechargé dans un
  autre monde, puis continué 300 jours, reste identique au monde d'origine, chronique mot pour mot), pont, monde,
  foule, joueur (sauf la traversée), combat (4), factions (3), némésis (2), fin (2), bestiaire, mise à l'échelle.
- `Swarm.Guide.FrontEnd` (cinématique → titre → touches → jouer → pause → enregistrer → reprendre) **passe quand il
  est lancé seul** (quatre fois de suite). Dans la suite complète il est compté en échec à cause d'une alerte du
  moteur, sans rapport avec ce qu'il vérifie (lancer de rayons : « geometry 'Chunk_..' is evicted » sur un morceau
  de terrain après le rechargement du niveau). Non résolu.
- `Swarm.Player.Traverse` échoue : le marcheur automatique, qui va en ligne droite sans rien contourner, reste bloqué
  contre un rocher au bout de 7,6 km. Les rochers ont maintenant la collision de leur vraie forme. C'est le test
  qui est trop simple ; un joueur contourne. Mesure utile qu'il donne quand même : **204 images/s en moyenne, 159
  pour le 1 % le plus lent**, à 1440p, avec les vrais arbres et jusqu'à 100 unités animées (147 puis 156 avant).
- `Swarm.Guide.TestMenu` échoue dans la suite seulement : il attend 60 munitions et en trouve 97, parce que le test
  de l'accueil, qui passe avant lui, laisse un fusil chargé. Défaut du test, pas du jeu.
- `Swarm.Guide.Strategic` échoue sur un point connu depuis le 14 : à huit jours par seconde, il passe un peu moins
  de 60 jours en 12 secondes (des jours sont perdus pendant les reconstructions des cités).
- DLSS à 1440p : sans 167 images/s, DLAA 166, Qualité 193, Équilibré 182, Performance 180. Le mode DLAA ne
  s'effondre plus.
- Simulateur sans moteur recompilé après l'ajout de la sauvegarde, relancé sur 20 graines seulement (pas 100) : rien
  n'a bougé, et pour cause, les règles n'ont pas été touchées.
- Captures regardées : `Docs/Captures/J11_*.jpg` (soldats, lame, garde royale), `J12_*.jpg` (titre, cinématique,
  touches, pause, interface en jeu).

**Pas vérifié / limites honnêtes**
- Personne n'a encore vu ces animations en mouvement : les captures sont des images fixes. Le cycle de marche des
  créatures, en particulier, est écrit et compilé mais jugé sur des poses.
- Le joueur tient son fusil avec les animations « sans arme » du mannequin : l'arme pend au bout du bras.
- Les soldats passent d'une animation à l'autre sans fondu.
- La cinématique montre un monde encore pauvre : bâtisses en blocs, pas d'herbe, pas de son. L'étoile qui tombe au
  troisième plan est une mise en scène (c'est ce que la simulation fera arriver quelques jours plus tard, ailleurs).
- Les murs d'enceinte, tours et halles n'ont pas de « rayon » connu des unités : elles peuvent encore les traverser.
- Dialogues, annonces et bandeau des némésis sont restés dans l'ancienne petite police.
- Le pack « ASC Teuthisan » (créature de qualité cinématique, `C:\Dev\ASCTeuthisan`, 1,9 Go) n'est pas repris : ses
  animations sont des séquences à convertir. Candidat pour la Reine.

**Disque**
Projet : 7,7 Go dont 3,4 Go d'`Intermediate` (recréable). Supprimé : 600 Mo de modèles téléchargés non retenus.
Hors dépôt git : `Plugins/` (DLSS), `SourceAssets/`, `Content/Swarm/Nature/` (recréables par les scripts).
À proposer au propriétaire : supprimer `C:\Dev\Swarm 5.8` (3,1 Go, vieille copie) et, une fois la créature reprise,
`C:\Dev\ASCTeuthisan` et son double dans le cache du lanceur (1,9 Go chacun).

## J12 — Un monde qui vit dès la première minute (15/11/2026)

Demande du propriétaire après essai : « il faut que ça aille vite », un monde vivant, des peuples qui agissent les
uns sur les autres, des personnages qu'on reconnaît. Ce qui a été fait, et où :

- **L'Essaim tombe entre le jour 1 et le jour 3** (`fallDayMin/Max`), et **le monde commence en guerre** : les deux
  peuples qui se détestent le plus sont en guerre au jour 0 (`openingWar`). Le joueur commence dans une cité
  frontalière d'un peuple en guerre (`ASwarmGameMode::FindStartSettlement`).
- **Coups de main et rondes** dans la simulation : une cité en guerre pille les terres ennemies (`TargetKind::Raid` ;
  les vivres changent de grenier, rien n'est créé), l'ennemi sort contre les pillards, chaque cité assez gardée tient
  une patrouille sur ses terres (`TargetKind::Patrol`). On croise donc des troupes en marche partout.
- **Habitants** : un par quatorze habitants de la simulation (28 au plus) se promène dans sa cité
  (`ESwarmUnitSource::Civilian`). On peut leur parler ; les tuer retire des habitants à la simulation.
- **Personnages reconnaissables** : les chefs portent le corps de Kwang (Paragon), les champions de l'Essaim celui de
  Grux sous une peau de chitine (`LeaderKinds`, `ChampionKinds` dans `SwarmCrowd.cpp`).
- **Arbres** : aulnes noirs de Megascans (plein champ, forêt, jeunes pousses) à la place des feuillus provisoires.
- Sol nivelé sous les cités ; les unités sont repoussées hors des maisons et des rochers.

Équilibre après ces changements, graines 1 à 100 : **Arche 83 %, Essaim détruit 17 %, 86 % des parties entre 600
et 1000 jours**, médiane 731 jours.

## J13 — Dialogues et ciel (15/11/2026)

- **Conversations en caméra d'épaule** (`UpdateDialogueCamera`, `DrawDialogue`) : la caméra passe par-dessus l'épaule
  du joueur, mise au point sur le visage de l'autre, bandes noires, sujets. Quatre sujets dont les réponses sont
  composées à partir de l'état du monde : les nouvelles (chronique des vingt derniers jours qui touche ce peuple),
  les autres peuples (diplomatie et relations réelles), le chef et son héritier (la règle de succession du régime),
  où il se passe quelque chose (combat, ruche, cité ennemie les plus proches, portés sur la boussole dix minutes).
- **La chute** (`ASwarmOmens::StartFall`) : le jour où la simulation fait tomber l'Essaim, un corps traverse le ciel
  et s'écrase à l'endroit exact ; éclair, onde au sol, secousse de caméra. La ruche n'apparaît qu'à l'impact.
- **Colonnes** : une lueur au-dessus de la Reine tant qu'elle vit, une autre au-dessus de l'Arche qui monte avec le
  chantier, une fumée au-dessus de chaque combat et de chaque cité tombée ces derniers jours.
- Tests `Swarm.Guide.Dialogue` et `Swarm.Guide.Fall`.

## J14 — Lieux cachés (15/11/2026)

Demande : « des zones cachées, comme dans Gothic 3 ». Dix-neuf lieux par monde, absents de toute carte tant que le
joueur n'y est pas allé.

**Simulation** (`swarm::Site`, `GenerateSites`) : un tombeau par peuple (un ancien, dans les hauteurs à quelques
heures de sa capitale), quatre épaves du monde d'avant, quatre sources, six caches. Ils sont tirés avec un générateur
à part : **l'histoire du monde ne change pas d'un jour** (mêmes 83 % / 17 % / 86 % sur les graines 1 à 100, et le
test `Swarm.Sim.Sites` compare un monde pillé à son jumeau intact sur 250 jours). Leur contenu est un stock fini,
compté à la création du monde comme le minerai du sol : rations (caches), cartouches et fusil (épaves, tombeaux).
Ce qui en sort n'y revient pas. Quatre actions du joueur : `PlayerDiscoverSite`, `PlayerLootSite` (une unité à la
fois, au même prix en métal qu'un achat en cité), `PlayerDrinkAtSpring` (une fois par jour, jamais si l'Essaim tient
la case), `PlayerReportSite` (dire aux siens où repose leur ancien : +12 de réputation, une fois). Sauvegarde en
version 3 : **une partie enregistrée avant ce jalon n'est plus lisible** (« Reprendre » ne la proposera pas).

**Monde** : le sol est aplani sous chaque lieu et aucun arbre n'y pousse (une clairière se voit de loin). L'épave
est un vrai maillage (`wreck()` dans `make_meshes.py`, matériau `M_Wreck` : peinture mangée par la rouille) : une
carlingue brisée en deux, une aile plantée dans le sol, l'autre arrachée ; sa collision est celle du maillage, on
entre dedans par la cassure. Le tombeau est un cercle de pierres levées avec sa table (vrais rochers étirés), sa
stèle et la bannière du peuple. La source a son miroir d'eau, sa margelle et sa couronne de jeunes arbres ; si
l'Essaim prend la case, ses cosses poussent dans l'eau. La cache est un abri de planches, un foyer froid, un piquet
au chiffon. Les caisses disparaissent et la dalle du tombeau est repoussée quand le lieu a été vidé.

**Jeu** : on découvre un lieu en arrivant à 45 m (son nom s'écrit en grand, la découverte entre dans la chronique) ;
E pour fouiller, boire, ouvrir le tombeau ou lire sa stèle. Les habitants et les soldats en parlent (« Où se
passe-t-il quelque chose ? ») : la rumeur du lieu inconnu le plus proche vient sur la boussole ; un soldat parle
d'abord du tombeau de son ancien. Un tombeau retrouvé donne un nouveau sujet avec les siens. Les lieux trouvés
figurent sur la carte du ciel (losange doré). Menu de test : « Aller près du lieu caché inconnu le plus proche ».

Au passage : annonces et bandeau des némésis dans la police de l'interface.

**Vérifié** : `Swarm.Sim.Sites` (12 mondes : lieux en pleine terre, loin des cités, mêmes lieux pour la même
graine ; ce qu'on emporte est exactement ce qui était là ; source une fois par jour ; tombeau rapporté une fois, aux
siens seulement ; sauvegarde) et `Swarm.Guide.Sites` (les quatre sortes visitées en jeu, captures
`Docs/Captures/J15_lieu_*.jpg`).

**Limites honnêtes** : la source se voit peu (eau sombre, pas de reflet marqué) ; la cache est un abri très sage ;
les lieux n'ont ni gardien ni piège ; l'Essaim ne les détruit pas en passant (seule la source est souillée) ;
personne n'a encore marché dans la carlingue de l'épave (la collision exacte est réglée, pas essayée à la main).

## J15 — Le son, et un blocage de fin de partie corrigé (15/11/2026)

**Le son.** Le jeu était muet. Trente et un sons sont fabriqués par `Tools/Assets/make_sounds.py` (synthèse en Python
pur : bruit filtré, sinus, enveloppes ; rien de téléchargé, donc libres de droits par construction) et importés par
l'étape `sons` de `setup_assets.py` dans `/Game/Swarm/Audio` (3 Mo). `ASwarmAudio` (`SwarmAudio.h/.cpp`) les joue :
- quatre ambiances en boucle dont le volume suit le monde : le vent (plus fort en altitude, en montagne, au désert),
  les grillons la nuit, les oiseaux le jour selon le biome et ce qu'il reste de végétation, et le bourdon de l'Essaim
  sur ses terres et à moins de 400 m d'une ruche. Sur les terres de l'Essaim, oiseaux et grillons se taisent ;
- tirs (joueur et soldats, portée 600 m, étouffés par la distance), barre de fer, chocs sur la chair et sur la
  chitine, pas du joueur, cliquetis des bêtes qui frappent, cris de mort ;
- la chute : le souffle du corps qui tombe, puis le choc, qui arrive **après** l'éclair (le son met son temps) ;
- découverte d'un lieu (deux notes de cloche), ramassage, source, interface (menus, dialogues).
Les cris humains (douleur, mort, effort) et ceux des champions viennent de Paragon (Kwang, Grux : 18 petits fichiers
copiés, 6 Mo) ; sans ces packs, ces familles restent muettes et tout le reste s'entend. Pas plus de quelques sons
d'une même famille à la fois. Volume général dans le menu (titre et pause), par crans de 20 %, enregistré.

**Vérifié** : `Swarm.Guide.Audio` (toutes les familles du projet chargées ; un coup reçu, un choix validé demandent
leur son ; les ambiances suivent l'heure). Lancé aussi avec la carte son (`run_tests.ps1 -Sound`, volume à 2 %) : le
mélangeur s'initialise sur les haut-parleurs, les sons se compressent et se jouent, aucune erreur audio au journal.
**Pas vérifié : personne n'a encore écouté ces sons.** Ils sont synthétiques ; les statistiques (niveau, absence de
coupure aux boucles) sont bonnes, mais c'est à l'oreille qu'on saura si le tir claque et si les oiseaux sont
supportables. À juger et à retoucher (les réglages sont en tête de chaque fonction de `make_sounds.py`).

**Un blocage de fin de partie.** La suite complète a fait apparaître trois échecs, dont deux avaient la même cause :
`Swarm.Sim.WorldDeadOutcome` et `Swarm.Ending.Rules` (« saboter l'Arche sans tuer la Reine mène au monde mort »).
Un camp nomade surpris en route par l'Essaim devenait intouchable : il ne pouvait ni lever le camp (assiégé) ni
tomber (`ResolveUndefendedSettlement` ignorait les camps en marche). Les couvées envoyées contre lui restaient
plantées devant pour toujours. Dans le monde de la graine 2, 530 bêtes (presque toute l'armée de l'Essaim)
attendaient ainsi devant un camp de 84 habitants sans soldat, pendant que la colonie mourait de faim et que trois
cités survivantes finissaient par l'achever. L'issue « monde mort » n'était presque plus atteignable. Corrigé : un
camp en route sans défenseur tombe comme une cité. Outil de diagnostic qui a montré le blocage : un petit programme
qui liste cités, ruches et groupes à un jour donné (non conservé ; trois lignes à réécrire avec `SwarmSim.h`).

Équilibre après correction, graines 1 à 100, monde laissé seul : **Arche 84 %, Essaim détruit 16 %, monde mort 0 %,
86 % des parties entre 600 et 1000 jours**, médiane 726 jours. Avec une Arche hors de prix (`--set arkBio=60000`,
24 graines) : monde mort 38 %, Essaim détruit 62 % (avant la correction : 0 % et 100 %).

Le troisième échec, `Swarm.Combat.C_FactionBattle` (quatre assaillants encore dans la case à la fin du délai), ne
s'est pas reproduit sur deux relances ; il reste à surveiller.

**Suite complète du 15/11** (avant ces corrections) : 30 réussites sur 33. Après corrections, les trois tests en
échec et les groupes Simulation, Fin, Combat et Son repassent tous ; **la suite entière n'a pas été relancée d'un
bloc depuis**.

## J16 — Herbe, fusil épaulé, effets de tir, champions (15/11/2026)

- **Herbe** (`ASwarmGrass`, `SwarmGrass.h/.cpp`) : des dalles de seize mètres se couvrent de touffes devant la caméra
  (48 m autour d'un point pris 24 m devant elle) et se vident derrière. La densité suit le biome et ce qu'il reste de
  biomasse dans la case ; plus rien sur les terres de l'Essaim ni dans les cités ; l'herbe jaunit quand la case est
  broutée. Une dalle se sème toujours pareil. Les touffes sont trois cartes croisées (6 triangles) qui portent une
  texture de brins dessinée par `Tools/Assets/make_grass_texture.py` ; matériau `M_Grass` (découpe, vent, teinte par
  touffe, éclairé pareil des deux côtés). Première version en brins de géométrie abandonnée : on aurait dit un champ
  de yuccas. Environ 39 000 touffes en pleine plaine, 2 400 au désert (test `Swarm.Guide.Grass`).
- **Le fusil se tient à deux mains** : quand il est en main, le corps du joueur passe sur les animations du gabarit de
  tir d'Epic (`ABP_TP_Rifle`, copié des gabarits du moteur dans `Content/Variant_Shooter/Anims`) : arme épaulée, buste
  qui suit la visée. Mains libres ou barre de fer : animations d'avant. L'arme est tournée d'un quart de tour dans la
  main (la poignée du mannequin attend un canon sur son axe Y).
- **Effets de tir** (`ASwarmEffects`) : flamme au canon et éclat à l'impact pour le joueur et les soldats, tirés du
  pack Niagara « sA_ShootingVfxPack » du propriétaire (50 Mo copiés, hors dépôt). Sans le pack, rien ne s'affiche.
- **Champions** : Rampage, Sevarog et Khaimera rejoignent Grux. Comme ils portent la chitine du jeu, seuls leur
  maillage et quatre animations sont copiés : 12 Mo chacun au lieu de 1 Go. Un champion garde son corps d'une
  apparition à l'autre (choisi par son numéro de némésis).

**Vérifié** : `Swarm.Guide.Grass`, `Swarm.Guide.Shot` (une cartouche part, une flamme et un éclat sont demandés, le
coup s'entend), `Swarm.Guide.Bestiary`. Captures `Docs/Captures/J16_*.jpg` : le pré, le désert, le joueur de profil
fusil épaulé.

**Pas vérifié / limites honnêtes**
- **La flamme et l'éclat ne se voient sur aucune capture** : hors écran, les effets Niagara n'étaient pas encore prêts
  à s'afficher au moment du tir (« Preparing Shaders »). Le code les crée, les tests le comptent, mais personne ne les
  a vus. À regarder en jeu ; s'ils sont trop gros ou trop discrets, l'échelle se règle dans `SwarmEffects.cpp`.
- Les trois nouveaux champions n'ont pas été regardés un par un (le bestiaire n'en montre qu'un).
- L'herbe n'a pas d'ombre et ne plie pas sous les pas ; au-delà de 50 m il n'y en a plus (le sol reprend sa texture).
- Les soldats, eux, gardent leurs animations sans fondu.

**Suite complète sur l'état J16** : 40 réussites sur 41. Cadence sur la traversée de 11,6 km à 1440p, herbe comprise :
**202 images/s en moyenne, 153 pour le 1 % le plus lent** (204 / 159 avant l'herbe, les habitants et les lieux).

Le seul échec, `Swarm.Combat.C_FactionBattle` (une fois sur deux depuis le matin), était un vrai défaut de jeu, que le
journal du test a fini par montrer : dans une cité attaquée, les assaillants arrivés au centre et les derniers
défenseurs restés sur leur ronde, à soixante mètres, ne se voyaient plus ; chacun restait planté et la bataille ne
finissait jamais. Corrigé dans `ASwarmCrowd::Think` : une troupe en campagne qui ne voit plus personne va chercher
l'ennemi le plus proche, jusqu'à 160 m. Trois relances du groupe Combat : 4 sur 4 à chaque fois, la bataille se
termine en 33 à 50 secondes au lieu de s'enliser. La suite entière n'a pas été relancée après cette dernière
correction.

## J17 — Un but, des textes, de vrais sons, des remparts qui arrêtent (15/11/2026)

Le propriétaire a demandé un bilan sans complaisance (réponse : c'est un prototype technique, pas un jeu), puis un
plan : `Docs/PLAN.md`, 119 points et 595 embranchements, lore compris. Il a ensuite téléchargé une trentaine de packs
et demandé de finir le jeu avec, en pensant au poids sur le disque. Cette entrée couvre ce qui a suivi.

- **Journal d'objectifs** (`FSwarmJournal`, `SwarmJournal.h/.cpp`) : dix objectifs en quatre actes (survivre, choisir,
  comprendre, trancher). Un objectif s'accomplit par ce que le joueur fait réellement (il a travaillé, il a trouvé un
  tombeau, une ruche est tombée près de lui), jamais par une case cochée. Rejoindre un peuple reste un choix. Quand
  l'Arche atteint 75 %, l'acte IV s'ouvre quoi que le joueur ait fait. Touche J : objectifs, puis recueil, puis fermé.
- **La boussole ne montre plus tout** : l'objectif suivi (losange doré), la cité la plus proche, une ruche à moins de
  2,5 km, un combat à moins de 1,5 km. La Reine n'apparaît que si le joueur l'a vue ou si son peuple, averti de la
  menace, le lui a dit ; l'Arche, quand elle se voit de loin (premier quart) ou qu'il l'a vue.
- **Compte à rebours** : sous l'heure, la jauge de l'Arche à quatre crans et les jours restants, calculés sur son
  rythme réel des vingt derniers jours.
- **Textes du monde** (`SwarmLore.h/.cpp`, référence `Docs/LORE.md`, options 1 du plan prises par défaut) : la
  question « Racontez-moi la chute » (cinq récits, un par peuple) ; sur chaque tombeau l'épitaphe de l'ancien puis la
  leçon qu'il a laissée ; dans chaque épave un carnet de trois pages (pilote, médecin, sergent, enfant) ; dans chaque
  cache un mot. La touche d'action tourne les pages. Tout ce qui est lu ou entendu entre au **recueil**. Règle tenue :
  ce qu'une stèle dit de la façon de battre l'Essaim est vrai dans la simulation (garde royale, quille de l'Arche,
  adaptation des champions, Essaim féral) ; chaque leçon porte en commentaire la règle qu'elle énonce.
- **Sons du pack « Medieval Village »** (66 fichiers, 209 Mo copiés dans `Content/Audio`, hors dépôt) : pas selon le
  sol (herbe, terre, pavé en cité, sous-bois), vent, oiseaux, grillons, et six nappes de **musique** : campagne, nuit,
  cité, Essaim proche, combat, écran-titre. La musique suit la situation (le combat monte quand on entend tirer et
  frapper) et se tait de longs moments exprès. Réglage « Musique » dans les menus. Les sons de synthèse restent pour
  ce qu'aucun pack ne fournit (tirs, chitine, Essaim) et en secours si le pack manque.
- **Remparts et tours arrêtent les unités** (`ASwarmStructures::Route`, `WallCover`) : on entre par la porte,
  assaillants compris. Les combats de test se terminent toujours.
- **Outils de reprise de packs** (`Tools/Editor`) : `list_assets.py` (inventaire d'un projet), `dump_level.py`
  (où est posée chaque pièce d'un niveau), `kit_merge.py` (fond un assemblage de cent pièces en un maillage Nanite,
  allège ce qui dépasse un nombre de triangles), `list_deps.py` + `Tools/Assets/copy_closure.py` (copie un asset et
  tout ce dont il dépend, rien de plus), `setup_kit.py` (collision exacte, textures ramenées à 2K).

**Vérifié** : `Swarm.Guide.Journal`, `Swarm.Guide.Lore`, `Swarm.Guide.Audio` (6 nappes sur 6, 13 pas sur l'herbe, la
musique de combat monte après dix tirs), `Swarm.Guide.FrontEnd` (six choix au titre), `Swarm.Combat.*` 4 sur 4.

**Pas vérifié / limites honnêtes**
- **Personne n'a écouté** : ni les nouveaux pas, ni les ambiances, ni la musique. Le choix des fichiers s'est fait sur
  leur nom. Une nappe peut ne pas convenir à sa situation.
- Les objectifs « être là quand une ruche tombe » et « en finir avec l'Essaim » n'ont pas été joués jusqu'au bout.
- Le lore suit les options par défaut ; le propriétaire ne les a pas encore validées.
- Les chefs n'ont toujours ni caractère écrit ni corps distinct ; le joueur est toujours le mannequin du moteur.

## J18 — Les packs du propriétaire entrent dans le monde (15/11/2026)

Méthode, valable pour tout pack à venir (`Tools/Editor`, détail en tête de chaque script) :
1. `list_assets.py` : inventaire du projet du pack, sans rien copier.
2. `dump_level.py` : où est posée chaque pièce de son niveau de démonstration.
3. `kit_merge.py` : chaque assemblage (une maison de cent à six cents pièces, un étal, une tour) devient **un**
   maillage Nanite, posé à l'origine, d'aplomb. Une pièce de plusieurs millions de triangles part du maillage de repli
   du moteur, pas de la source : sinon chaque pièce coûte des minutes.
4. `list_deps.py` puis `Tools/Assets/copy_closure.py` : on ne copie que ces maillages fondus et ce dont ils dépendent
   (matières, textures), pas les pièces.
5. `setup_kit.py` : textures ramenées à 2K (1K pour les petits objets) par les fonctions du moteur.

Ce que ça a donné :
- **Cités** : onze maisons et une forge du pack « Medieval Village », rangées en anneaux autour de la place, une rue
  de la place à la porte. Leur nombre suit toujours la population, dans la limite de la place (`GetHouseCapacity`).
  Autour : tonneaux, caisses, brouettes, bancs (rien dehors en temps de famine), un puits, une enseigne, des étals de
  marché du pack « Bazaar » selon la population.
- **Édifices centraux** : le Concordat a son sanctuaire (cercle de colonnes, tête de pierre de la Gardienne, lanternes,
  brûle-parfums, pack « Goddess Temple ») ; la Ligue sa grande forge et ses fours ; les Affranchis leur moulin commun ;
  les Clans leur grande toile. Le Protectorat garde son donjon de blocs.
- **Lieux cachés** : vraies tombes, coffre, épée, gisant du gardien ; coffres et caisses des caches et des épaves ;
  une cache sur deux au pied d'une tour de guet en ruine (pack « Dark Ruins »).
- **Paysage** : falaises en montagne, chaos de blocs en colline, cheminées de fée et palmiers bas au désert.
- **Le joueur est un humain** : le MetaHuman « Kellan » de « Game Animation Sample » (sweat à capuche, pantalon cargo).
  Le mannequin reste dessous, invisible, et porte les animations ; le corps humain les recopie par le reciblage du
  pack (`ASwarmPlayerCharacter::SetupHumanBody`). 180 Mo.

Poids : chaque kit est mesuré avant et après. Village 8,3 Go copiés, 3,2 Go après réduction des textures ; temple
1,8 Go puis 0,6 Go. Les projets d'origine ne sont pas modifiés, sauf un dossier `Content/SwarmKit` qu'on y crée (le
pack du temple est lu dans le cache du lanceur à travers `C:\Dev\_VuesPacks\RomanCave`, dont les dossiers sont des
jonctions : ne jamais le supprimer récursivement).

**Vérifié** : `Swarm.Guide.Houses` (autant de maisons que demandé, un rayon tiré du ciel s'arrête sur un toit, on ne
se tient pas dans un rempart, on vise la porte pour entrer), `Swarm.Guide.Human`, `Swarm.Guide.Sites`. Captures
`Docs/Captures/J17_cite_*.jpg`, `J18_centre_*.jpg`, `J19_humain_*.jpg`.

**Pas vérifié / limites honnêtes**
- Kellan n'a pas de cheveux à l'écran : les mèches sont chargées mais n'apparaissent pas dans les captures.
- La tête de la Gardienne sort très sombre ; son matériau d'origine attend peut-être un éclairage de grotte.
- Les habitants et les soldats sont toujours des mannequins teintés, les chefs ont toujours le corps de Kwang.
- Remparts, tours et donjon du Protectorat sont encore des blocs texturés.
- Les cités mortes sont encore des murs noircis en blocs.
- Personne n'a marché dans ces cités manette en main : collisions des cours et des étals non éprouvées.

## J19 — Souffle, esquive, compétences, ordres, ouverture ; et la cadence (15/11/2026)

- **Souffle** : courir, esquiver et frapper le vident ; il revient après un instant de répit. À bout de souffle on ne
  repart pas en courant, et un coup porte moitié moins.
- **Esquive** (Alt gauche) : un pas vif dans la direction où l'on va ; un tiers de seconde sans dégât.
- **Compétences par l'usage** : tir (une balle qui touche), mêlée (un coup qui porte), endurance (soixante mètres
  courus). Chaque niveau demande plus que le précédent (12, 48, 108… d'expérience). Effets réels : dégâts et cadence
  de tir, dégâts et cadence des coups, souffle maximal et faim plus lente. Visibles au bas du journal.
- **Ordres à l'escouade** (G) : « tenez ici » (ils se postent et ne poursuivent pas), « chargez là-bas » (là où l'on
  regarde), « avec moi ». Le point d'attache des compagnons passe du joueur à l'endroit désigné.
- **Ouverture** : quatre phrases s'écrivent pendant les premiers pas d'une nouvelle partie.
- **Menu de test** (T) : onze raccourcis de plus (capitale du peuple suivant, tombeau, épave, cache, montagne, désert,
  escouade d'essai, compétences, son suivant, musique suivante, ouverture). Tournée conseillée : `Docs/A_TESTER.md`.

**La cadence, mesurée et en partie réparée.** La suite complète a échoué sur un seul test, la traversée : 113 images/s
en moyenne (202 avant les packs), 43 pour le 1 % le plus lent (seuil : 45). Le test `Swarm.Perf.Kits` cache une
famille de décor à la fois devant la plus grande capitale :

| Devant la capitale | images/s |
|---|---|
| Tout affiché, avant correction | 29 |
| Sans les maisons | 146 |
| Sans l'éclairage indirect Lumen | 62 |
| Sans le chaume découpé, Nanite moins fin, sans reflets : aucun effet | 28 à 30 |
| **Après correction** (maisons et objets hors de la scène Lumen et du lancer de rayons, sans vent, ombres en cache) | **61** |

Ce qui reste (61 contre 150 sans maisons) n'est ni l'éclairage indirect, ni les ombres, ni la finesse de Nanite.
Hypothèse non vérifiée : le nombre de matières (soixante à soixante-dix par maison, environ deux cents à l'écran).
Piste : réduire les matières d'une maison à une poignée en cuisant ses textures dans un atlas.

**Vérifié** : `Swarm.Guide.Skills` (esquive, souffle, niveaux, ordres), suite complète 43 sur 44 avant ces ajouts.

**Pas vérifié / limites honnêtes**
- Personne n'a esquivé ni donné un ordre manette ou clavier en main : seules les fonctions ont été appelées.
- L'ordre « chargez » n'a pas été regardé dans un vrai combat.
- La parole (quatrième compétence du plan) n'existe pas : aucune règle ne lui donnerait d'effet réel pour l'instant.

## J20 — Plans de ville, forêts, plantes, champs (15/11/2026, soir)

Retour du propriétaire : cités « sans structure, des lignes droites », monde « très vide », arbres qui « twerkent »,
jeu qui rame sur une RTX 5080.

- **Cadence, cause trouvée** : les maisons fondues portaient 60 à 72 matières ; Nanite en refuse plus de 64 et
  l'échec est silencieux : elles passaient par le rendu classique. `kit_merge.py` coupe maintenant un assemblage en
  parts (`max_materials`). Devant la plus grande capitale : 29 → 167 images/s.
- **Plans de ville** (`ETownPlan`, `PlanOf`) : Aram rayonnant (rues en étoile et couronne), Fonderies en damier,
  Protectorat en camp (rangées et voie centrale), Affranchis en ruelles tordues et venelles, Clans en cercles de sept
  tentes autour d'un feu. Les rues sont pavées là où elles sont bâties (`Road` du pack), plus l'avenue de la porte.
- **Blocs remplacés** : tours de rempart, donjon du Protectorat (grande tour, quatre tours d'angle, courtine),
  cheminées des Fonderies : pièces du pack (`AddKitFit`). Les pans de rempart restent des blocs texturés.
- **Forêts** : 900 épicéas Nanite par case de forêt (44 arbres avant), sapinières en colline, sol jonché (souches,
  troncs, rochers moussus). Vent des arbres coupé : il déplaçait l'arbre entier. Les arbres sont donc immobiles.
- **Plantes** (`ASwarmGrass`, 52 sortes) : fougères, fleurs, herbes sèches selon le biome ; **champs de blé** en
  couronne autour des cités sédentaires (`IsField`), en rangs.
- **Mesure** (`Swarm.Player.Traverse`, 2560×1440) : 11,5 km à 158 images/s en moyenne, 129 pour le 1 % le plus lent.

## J21 — Chantiers : le monde est exploité à des endroits précis (15/11/2026, soir)

Demande : « un monde destructible et exploitable par les civilisations, des mines qui se créent, des ouvriers qu'on
voit, pas des ressources par blocs ».

- **Simulation** (`Worksite`, `StepWorksites`, `OpenWorksite`, `EndWorksite`) : un peuple sédentaire ne tire plus rien
  d'une case où il n'a pas envoyé d'équipe. Mine sur la roche qui porte un filon, camp forestier en forêt, ferme en
  plaine ou en colline. Les équipes viennent de la population de la cité (`labourShare`, `worksiteCrew`) ; ouvrir un
  chantier consomme des vivres. L'Essaim engloutit le chantier dont il prend la case ; un filon vide ferme sa mine ;
  une cité tombée passe ses chantiers à la voisine ou les abandonne. Les Clans, nomades, font paître sans chantier.
  Sauvegarde version 4 (jeu : 5) : **les anciennes parties ne se chargent plus**.
- **Équilibre** : avec les chantiers seuls, Arche 95 % / Essaim détruit 5 % (100 graines). Recalé par
  `humanMineRate` 0,0010 → 0,0013 et `humanHarvestRate` 0,012 → 0,013 : **Arche 88 %, Essaim détruit 12 %, monde mort
  0 %** (avant les chantiers : 84 / 16 / 0). L'écart restant est dans le bruit d'un tirage de 100 graines ; non
  re-vérifié sur 300.
- **Le sol est creusé** (`ASwarmTerrain::Dig`, `TickDig`) : chaque mine est une vraie fosse dans le terrain, de 5 m,
  plus 1 m par tranche de minerai sorti (15 m au plus), rampe du côté de la cité, roche à nu. Une mine fermée reste
  un trou. Les morceaux de terrain se rebâtissent un par image, collision en tâche de fond (un essai en un seul bloc
  figeait le jeu 341 ms). Un fond plat taillé dans le versant enfermait le joueur : abandonné, la fosse suit la pente.
- **Les arbres tombent autour du camp** : dans chaque case, on abat d'abord les plus proches du chantier ; en forêt
  ils laissent leur souche (1 635 souches au premier jour dans le monde de test). L'Essaim, lui, dévore tout.
- **Ce qu'on voit** (`BuildWorksite`) : carrière (échafaudage, blocs sortis et tas de stériles qui grandissent avec
  la production réelle, abri), camp forestier (abri, piles de troncs, souches), ferme (moulin, champ clos de blé).
  412 chantiers bâtis au premier jour du monde de test.
- **Ouvriers** (`ESwarmUnitSource::Worker`) : autant de personnes visibles que l'équipe en compte dans la simulation ;
  en tuer un retire un habitant à sa cité.
- **Menu T** : « VOIR - le chantier suivant ».
- **Le joueur y travaille** (`UseWorksite`, touche d'action près des ouvriers) : une journée passe, la cité qui
  tient le chantier paie deux rations sur son grenier (même règle que le travail en cité, `PlayerWork`). L'objectif
  « Trouver de quoi manger » le dit. Un peuple qui ne traite pas avec le joueur refuse.
- **Carte stratégique** : chaque chantier a son repère (triangle : carrière ; losange : ferme ; pentagone : camp).
- **Ciel : les nuées** (`ASwarmOmens`, `GetNumFlyers`) : des bêtes tournent au-dessus de chaque ruche vivante (90
  au-dessus de celle de la Reine, 26 ailleurs) et de chaque couvée en marche (une pour six bêtes au sol, de 8 à 40).
  Corps de chitine, visibles en plein jour à cinq cents mètres (capture `J21_ciel_1_nuee_de_loin`). Elles
  apparaissent une fois la chute finie et changent de place avec la couvée, au jour. Test `Swarm.Guide.Flocks`.
  Ce sont des ellipsoïdes, pas des créatures modelées ; elles n'attaquent pas.
- **Tests** : `Swarm.Sim.Worksites` (rien ne sort d'une case sans équipe, équipes bornées par la population, chantiers
  engloutis, sauvegarde), `Swarm.Guide.Worksites` (captures `J21_chantier_*`).
- **Corrigé au passage** : `PushOut` repousse en travers d'un rempart et non le long du mur.

**Suite complète du 15/11 à 21 h 44** (`Tools/Test/run_all.ps1`) : 44 réussites sur 45. L'échec,
`Swarm.Guide.Strategic` (le clic sur une capitale désignait l'armée stationnée dessus), tenait au test : il essaie
maintenant chaque capitale. Relancé avec les tests touchés par les derniers ajouts (carte, chantiers, menu, journal,
dialogue) : 5 sur 5. **La suite entière n'a pas été relancée d'un bloc après ces derniers ajouts.** Traversée dans la
suite : voir `Saved/Logs/all_main.log`.

**Derniers ajouts (22 h 05)** : le conseil de l'objectif en cours reste affiché sous son titre, en haut à gauche
(il n'était lisible que dans le journal). Vérification finale, 2560×1440, sur l'exécutable rendu : `Traverse`,
`World.*`, `Guide.Fall`, `Flocks`, `FrontEnd`, `Strategic`, `TestMenu`, `Worksites`, `Journal`, `Human`,
`Sim.Worksites`, `Bridge.DayAdvances` : tous réussis (`DayAdvances` a d'abord échoué parce que je l'avais lancé sans
son réglage `-SwarmSecondsPerDay=0.5` ; relancé avec, il passe). Traversée : 12,9 km, 151 images/s en moyenne,
98 pour le 1 % le plus lent, pire image 38 ms. Combat, Factions, Nemesis et Ending relancés à 22 h 09 : 11 sur 11. **Non relancés après les nuées et le conseil d'objectif** : Audio, Bestiary, Dialogue, Grass, Houses, Lore, Shot, Sites, Skills, Upscalers (ils passaient à 21 h 44).

**Non fait ce soir, malgré la demande** : le reste du ciel (lune, oiseaux, Arche visible de loin), des habitants et soldats humains (toujours des
mannequins teintés), le vent dans les feuillages, les pans de rempart, les cités mortes, une prise en main guidée du
jeu. Les ouvriers marchent sur le chantier ; ils n'ont pas de geste de travail.

## J22 — Le temps qu'il fait, les incendies (15/11/2026, 22 h 15 – 23 h)

Demande : « pense bien aux ambiances atmosphériques, catastrophes naturelles », et libérer l'espace de ce qui ne
sert plus.

- **Le temps dans la simulation** (`Weather`, `WeatherOn`) : ciel dégagé, couvert, pluie, orage, brouillard,
  sécheresse, par périodes de trois jours, tirés de la graine et du jour (aucun hasard de partie). Il agit : la pluie
  fait repousser une fois et demie plus vite et éteint les feux, la sécheresse freine la repousse (×0,4).
- **Incendies** (`StepWeather`, `Cell::burning`) : par sécheresse ou par orage (la foudre), une forêt peut prendre
  feu ; la case perd un cinquième de sa capacité en biomasse par jour pendant quatre jours au plus, le feu peut gagner
  les cases voisines, il emporte les fermes et les camps forestiers. Rien n'est créé : ce qui brûle est perdu pour
  tous, l'Essaim compris. Monde témoin (graine 9, 400 jours) : 14 incendies, 64 jours avec du feu.
  `PlayerIgnite` : le joueur peut mettre le feu (réputation −10 chez le peuple à qui est la terre) ; pour l'instant
  seulement par le menu T.
- **Séismes** (`QuakeOn`, `StepQuake`) : un jour sur 140 en moyenne, tiré de la graine, la terre tremble quelque
  part, force 1 à 3. Les cités bâties voisines de l'épicentre perdent des habitants (1 % par degré de force à
  l'épicentre, moins avec la distance) ; une secousse forte fissure le rempart le plus proche (un niveau de
  fortification en moins). Les camps nomades ne perdent personne. Le joueur à moins de douze cases sent la secousse
  (vue secouée quatre secondes, grondement). La chronique l'annonce.
- **Son** : un incendie s'entend à quatre cents mètres (enregistrement du pack Medieval Village) ; le vent force par
  temps couvert, pluie et orage. Le tonnerre est le son d'impact du jeu, faute d'enregistrement ; pas de bruit de pluie.
- **Équilibre recalé** : chantiers, temps, incendies et séismes avaient fait glisser les issues (Arche 90 %, Essaim
  détruit 10 % ; et aucune victoire des factions dans les douze mondes de `Swarm.Sim.FullGame`, qui échouait). Les
  réglages d'économie essayés ne bougeaient rien au-delà du bruit. Ce qui rétablit : `humanSoldierStrength` 2,0 → 2,2
  et `recruitRate` 0,020 → 0,025. **Résultat, 100 graines : Arche 84 %, Essaim détruit 16 %, monde mort 0 %**, 87 % des
  parties entre 600 et 1000 jours, médiane 719 jours : les chiffres d'avant les chantiers. Non re-vérifié sur 300
  graines. `FullGame` joue maintenant trente mondes (les douze premiers ne contiennent plus de victoire des factions).
- Mesure intermédiaire (avant recalage) : avec séismes, Arche 90 %, Essaim détruit 10 %, monde mort 0 %. Avant les séismes : 91 / 9. Sans incendies : identique. L'écart
  avec J21 (88 / 12) vient de la repousse selon le temps et reste dans le bruit du tirage ; à revoir sur 300 graines.
- **Le ciel suit** (`ASwarmSky::SetWeather`) : soleil voilé, brume épaisse, image terne et froide sous la pluie,
  chaude par sécheresse ; éclairs et tonnerre pendant l'orage ; brouillard où une cité à 150 m n'est plus qu'une
  silhouette. La **pluie** tombe autour de l'œil (1 400 traits au plus fort, `ASwarmOmens`). Transition en quelques
  secondes. La carte stratégique reste par temps clair.
- **Incendie visible** : quatre-vingts flammes qui dansent dans chaque case en feu (`GetNumFlames`), une lueur de
  brasier et une colonne de fumée de 1,5 km ; les arbres de la case disparaissent à mesure qu'elle brûle. Les flammes
  sont des cônes lumineux, pas une simulation de feu ; le sol ne noircit pas.
- **Nuées** : ce sont maintenant de vraies bêtes (le maillage de la bête de base de l'Essaim), plus des ellipsoïdes.
- **Écran** : le temps qu'il fait et le nombre d'incendies, sous l'horloge.
- **Menu T** : « VOIR - le temps suivant », « ESSAYER - mettre le feu à la forêt la plus proche », « VOIR - la secousse d'un séisme » (l'effet à l'écran ; dit le jour du prochain vrai séisme).
- **Tests** : `Swarm.Sim.Weather`, `Swarm.Guide.Weather` (captures `J22_temps_*`, `J22_incendie_1`).
- Sauvegarde version 5 (jeu : 6) : les parties d'avant ne se chargent plus.
- **Espace libéré** (48 Go, de 287 à 335 Go libres) : ma copie décompressée du plugin DLSS (4 Go), les caches
  dérivés des projets d'exemple (42 Go, régénérés si on les rouvre), mes assemblages intermédiaires dans ces projets
  (`Content/SwarmKit`, 0,9 Go, déjà copiés dans le jeu), les captures PNG (0,5 Go, les JPG restent dans
  `Docs/Captures`).
  **Gardés** : `ElectricDreamsEnv` (45 Go) et `ValleyoftheAncient` (71 Go), jamais utilisés mais qui contiennent des
  effets, des sons et un personnage humain qui peuvent servir ; les autres projets d'exemple (sources des pièces
  reprises) ; le cache du lanceur Epic (283 Go), que je n'ai pas trié.

**Tests du soir** : suite complète à 22 h 48, 48 réussites sur 50 ; les deux échecs (`Sim.FullGame`, `Sim.Worksites`)
venaient du temps, des incendies et des séismes (voir « Équilibre recalé » ; le test des chantiers ignorait qu'un feu
détruit de la biomasse). Corrigés, puis relancés sur l'exécutable final : `Swarm.Sim.*` (10 sur 10) et
`Guide.Weather` à 22 h 50.

**Vérification finale (22 h 58), incomplète** : sur l'exécutable final, Combat (4 sur 4), Ending (2 sur 2),
`Factions.Economy`, `SuccessionRules`, `Nemesis.Lines`, `Guide.Audio`, `Guide.Flocks` et `Traverse` passent
(traversée : 12,0 km, 147 images/s en moyenne, 94 pour le 1 % le plus lent, pire image 25 ms, en 2560×1440).
`Factions.KillLeaderInGame`, `Nemesis.ChampionReturns`, `Guide.FrontEnd`, `Guide.TestMenu` et `Guide.Worksites` ont
échoué dans ce lancement parce que je les avais mis dans le même monde que le test de fin de partie, qui termine la
partie (le jour restait bloqué à 516). Leur relance en lancements séparés a été interrompue à la demande du
propriétaire, pour ouvrir le jeu. **Ces cinq tests ne sont donc pas reconfirmés sur l'exécutable final** ; ils
passaient à 22 h 48 sur l'exécutable précédent (avant les flammes et le recalage des soldats). À relancer en premier :
`Tools/Test/run_all.ps1`.

**Non fait** : inondations, tempêtes qui abîment les cités, bâtiments visiblement effondrés après un séisme ; le temps n'agit pas encore sur
les combats ni sur les déplacements ; les nuages eux-mêmes ne changent pas (c'est la brume et la lumière qui font le
ciel couvert).

## J23 — La loi des cités (15/11/2026, 23 h) — consigné après coup

Retour du propriétaire : « le joueur peut flinguer les PNJ, ils s'en foutent ; il peut aller parler directement au
chef sans que personne l'en empêche ».

- **Sommation puis alerte** (`ASwarmCrowd::OnPlayerShot`, `RaiseAlarm`) : un premier coup de feu dans des murs qui ne
  sont pas hostiles vaut une sommation, les habitants s'écartent ; le second, ou le sang d'un habitant ou d'un garde,
  met la cité en alerte : les habitants fuient, la garnison prend le joueur pour cible, la simulation s'en souvient
  jusqu'au lendemain (`Settlement::alarmDay`, `PlayerOffend`) et le peuple perd de l'estime pour lui.
- **Audience** (`PlayerHasAudience`, `kRepAudience`) : on n'approche pas un chef sans s'être fait connaître ; deux
  gardes viennent se mettre en travers (`GuardLeaders`).
- **À l'écran** : un coin rouge au-dessus de chacun de ceux qui visent le joueur, leur nombre, la cité en alerte et le
  temps qu'il lui reste.
- **Vérifié** : `Swarm.Law.Alarm` (captures `J23_loi_*`), repassé le 17/11 après chaque étape.

## J24 — La vie : les gens se parlent, réagissent, viennent au joueur (16 et 17/11/2026)

Retours du propriétaire (« 4/20 ») : « les IA ne semblent pas interagir les unes avec les autres », « il manque des
événements venant au joueur directement », « ça manque de trash », « on ne comprend pas le système d'aggro ».
Tout est dans `Source/Swarm/Game/SwarmLife.cpp` (membres de `ASwarmCrowd`).

- **Paroles au-dessus des têtes** (`Say`, `GetBarks`, dessinées par `ASwarmDebugHUD::DrawBarks`) : ce que disent les
  gens se lit à portée de voix ; ce qui s'adresse au joueur est écrit plus fort.
- **Conversations** (`LiveCivil`, `MoodLine`) : à chaque arrêt de sa promenade un habitant cherche quelqu'un à qui
  parler ; ce qu'il dit vient de l'état de son peuple dans la simulation (famine, guerre contre tel peuple, trêve,
  ruche à tant de lieues dans telle direction, chef contesté, temps qu'il fait) et de la chronique (« Tu as su ? »
  suivi du dernier événement qui le concerne). L'autre répond. Les gardes se parlent aussi sur leur poste.
- **Réactions au joueur** : l'habitant ou le garde près duquel il passe le regarde et, selon ce que son peuple pense
  de lui, le salue, l'ignore, l'insulte ; il se terre si le joueur vise, crie si la cité est en alerte.
- **Deuil** : un mort dans la cité attire les siens, qui restent devant lui.
- **Messagers** (`SendMessenger`) : quand la chronique rapporte quelque chose qui regarde la cité où se trouve le
  joueur, l'un des habitants PRÉSENTS (pas un de plus) court le lui dire. À l'arrivée devant une cité, quelqu'un vient
  l'accueillir et lui dit où il met les pieds (guerre en cours, famine, ouvrage payé, direction de la ruche). Quand
  rien n'arrive : une supplique en temps de famine (E : donner une ration, `PlayerGiveFood`), une convocation du chef
  quand l'audience est acquise, une offre de travail, ou une rumeur VRAIE sur un lieu caché pas encore trouvé.
- **Scènes de rue** (`StartScene`, `StepScene`) : une harangue (l'orateur parle de ce qui arrive réellement à son
  peuple, on s'attroupe, la foule répond) ou une rixe (insultes, coups, un garde vient séparer, le vaincu saigne).
- **Traque** : l'Essaim sent celui qui marche sur sa terre ; au bout de vingt-cinq secondes la ruche voisine lâche
  une couvée prise sur ses propres bêtes (`Simulation::PlayerScented`), menée par son champion s'il y réside. Une cité
  en alerte dont le joueur a fui les murs lâche des chasseurs pris sur sa garnison (`PlayerPursued`).
- **Sang** : gouttes qui giclent à l'opposé du coup et retombent, gerbe au sol dans la direction du coup, flaque qui
  s'étale sous un mort (décalques, texture dessinée par `Tools/Assets/make_blood_texture.py`, matériau par
  `Tools/Editor/setup_gore.py`) ; rouge sombre pour les hommes, violet luisant pour l'Essaim. Un blessé grave laisse
  une traînée. La première version (disques rouges) faisait « prototype » sur capture : refaite.
- **Combat lisible** : celui qui prend le joueur pour cible le crie (ou crisse), celui qui le perd de vue le dit ;
  jauge de santé au-dessus de ceux qu'on a blessés ou qui en veulent au joueur.
- **Écran** : les annonces passent sous le conseil de l'objectif, l'alerte au-dessus du bandeau du némésis (ils se
  chevauchaient) ; traits de tir affinés (ils barraient l'écran de près).
- **Menu T** : scène de rue, habitant qui court vous trouver, être senti par l'Essaim.
- **Vérifié** (17/11, 20 h 29, exécutable du commit `08b0041`) : `Swarm.Sim.PlayerEvents` (la couvée et les chasseurs
  sortent de la ruche et de la garnison à l'unité près, la ration entre au grenier), `Swarm.Life.City`,
  `Swarm.Life.Scenes`, `Swarm.Life.Blood`, `Swarm.Law.Alarm`. Captures `J24_*`.
- **Pas vérifié / limites** : les gestes (`ESwarmEmote`) sont décidés ici mais joués par le chantier « humains » ; tant
  que sa branche n'est pas fusionnée les habitants restent debout. La traque et les chasseurs sont vérifiés dans la
  simulation, pas encore vus arriver sur le joueur en 3D. Personne n'a écouté les cris.

## J25 — Tâches et choix ; la nuit des habitants (17/11/2026)

Demande : « profondeur, choix, immersivité comme Gothic 3 ».

- **Tâches** (`SwarmQuests.h/.cpp`, une seule à la fois) : « Avez-vous une tâche pour moi ? » à un soldat, un officier
  ou un chef. Ce qu'il propose vient de l'état de son peuple : nourrir (famine), tuer un officier de l'ennemi, brûler
  ses champs, chasser l'Essaim, porter une offre de trêve (le chef seulement, quand la guerre lui pèse).
- **Choix** : chaque tâche peut être remplie, abandonnée (un peu d'estime perdue) ou TRAHIE auprès de ceux qu'elle
  vise (`PlayerBetray` : les uns s'en souviennent en bien, les autres en très mal, et les deux peuples se haïssent
  davantage). L'émissaire peut porter l'offre (`PlayerBrokerTruce` : la trêve se signe vraiment si l'autre y a
  intérêt) ou dire le contraire (`PlayerPoisonTalks`). Les rations attendues par des affamés se donnent ou se vendent
  contre des cartouches (`PlayerSellFood`).
- **Rien de gratuit** : le paiement sort du grenier du peuple (`PlayerReward`, au plus un vingtième de ce qu'il
  contient, rien en famine), les cartouches sont fondues dans son minerai.
- **Suivi** : la tâche tient en une ligne (`GetContractSummary`), dit où aller (`GetContractTarget`), prévient quand
  elle est remplie ; sur la terre à brûler, E met le feu. Sauvegarde version 8 (les parties d'avant ne se chargent plus).
- **La nuit** : au crépuscule sept habitants sur dix rentrent chez eux et quittent la rue, les autres veillent près de
  la place (boire, chanter) ; au matin tout le monde ressort. On peut demander où dormir (une ration, gratuit chez les
  siens) : un jour passe, on se relève entier.
- **Compagnons** : ceux qui suivent le joueur parlent (beau coup, blessure, terre de l'Essaim, nuit).
- **Équilibre** : inchangé, 100 graines le 17/11 à 20 h 40 : Arche 84 %, Essaim détruit 16 %, monde mort 0 %,
  87 % des parties entre 600 et 1000 jours (aucune de ces actions ne se produit sans joueur).
- **Vérifié** : `Swarm.Sim.Contracts` (paiement à la ration près, vente, mensonge, trahison, trêve signée des deux
  côtés), `Swarm.Quests.Dialogue` (demander, accepter, renoncer, trahir, porter la trêve, se faire payer ; captures
  `J25_tache_*`).

## J26 — Cinq chantiers d'agents fusionnés ; rendu demandé à 22 h 10 (17/11/2026)

Le propriétaire a demandé le rendu à 22 h 10, avant l'heure prévue : les agents ont été arrêtés en plein travail.
main (`82fe008`) contient leurs derniers commits STABLE ; ce qu'ils faisaient ensuite est resté dans leurs copies
(`C:\Dev\SwarmWT\<nom>`, branches `humains`, `essaim`, `monde`, `prise`, `atmo`), non commité pour partie.

- **Humains** (`493c466`) : habitants, ouvriers et soldats sont de vrais corps humains (le corps du MetaHuman du
  joueur, sans visage, sweat, pantalon et chaussures, teintés par peuple), avec couvre-chefs, masques et pièces
  portées par-dessus ; chefs distincts (Kwang, Narbash, TwinBlast) ; gestes, sursaut, coup. **Encore mauvais de
  près** : « tambours » rigides sur le torse, têtes en cylindre, toile à gros grain partout ; un seul gabarit
  d'homme (pas de femmes, pas de visages). L'agent corrigeait cela quand il a été arrêté.
- **Essaim** (`9d367a2`) : ruches et Arche en chair sombre et humide, os lisses, métal greffé (plaques, pompes,
  tuyaux, cages), carcasse de côtes de l'Arche, cadavres englués sur la terre tenue. **Pas fait** : la Reine (le
  Teuthisan s'affiche figé, débranché), les bêtes.
- **Monde** (`703a68f`) : rues qui serpentent et quartiers par peuple, trous et cratères propres (le sol lisait mal
  sa position), arbres abattables, on ne traverse plus les troncs, clôtures qui cèdent. **Pas réglé** : le vent dans
  les feuillages (rien ne bouge sur capture, cause non établie) ; variété du sol pas commencée.
- **Prise en main** (`58b1533`) : invites contextuelles, réticule qui nomme et dit ami / neutre / hostile, marqueur
  d'objectif et de tâche dans le monde, retours de combat, garde et parade, coups enchaînés, aide (H), écran de mort.
  **Pas vérifié** : secousse, recul, aide à la visée (invisibles sur capture) ; la barre n'est pas visible en main.
- **Atmosphère** (`4155a1f`) : nuages volumétriques selon le temps, étalonnage, aube et crépuscule à rayons, nuit
  lisible, orage (pluie penchée, éclairs, tonnerre décalé), ciel malade sur l'Essaim, particules portées par le vent.
- **Cadence : régression.** Une mesure de l'agent « atmosphère » sur l'ensemble : 79 images/s en 1440p (147 avant ce
  soir), machine chargée, mémoire vidéo dépassée. Non remesuré sur machine calme. À traiter en premier.
- **Tests sur main** : vagues 1 et 2 vérifiées par moi (18 sur 18, puis 13 sur 14 : `Swarm.Prise.A_FirstMinute`
  échouait après d'autres tests du même lancement). Les deux dernières fusions (corps humains réels, `58b1533`)
  compilent mais n'ont PAS été testées par moi sur main : la file à une seule instance du jeu ne m'a pas rendu la
  main avant le rendu. `Swarm.Guide.TestMenu` est signalé en échec (« et c'est un chef ») par l'agent « essaim » ;
  cause non établie, un diagnostic est écrit dans le test.
- **Machine** : le propriétaire joue pendant le travail. `Tools/Test/build.ps1`, `run_tests.ps1`, `governor.ps1` :
  une compilation et une instance du jeu à la fois, priorité minimale, cœurs logiques 8 à 15. Règles des agents :
  `Docs/AGENTS.md`.

## J27 — La machine d'abord ; profondeur : colonne d'assaut, projets des peuples, défense, reddition (18/11/2026)

**La machine.** Le propriétaire s'est plaint quatre fois en deux soirs que son jeu (LoL) saccadait. Mesure du 18/11 à
19 h 55 : ce n'était plus le processeur (déjà cantonné) mais la mémoire — trois Unreal à 10-15 Go chacun, 0,9 Go libre
sur 64 — et la carte graphique, à 100 % avec 15,6 Go de mémoire vidéo pris sur 16. Règles depuis, tenues par les
scripts (`Tools/Test/machine.ps1`, `run_tests.ps1`, `run_python.ps1`, `build.ps1`, `governor.ps1`) :
- un seul processus Unreal à la fois sur toute la machine (jeu ou script d'éditeur), une seule compilation, priorité
  minimale, cœurs logiques 8 à 15 ; le garde-fou tue tout Unreal surnuméraire ;
- rien ne dessine pendant qu'il est en partie : un test avec image attend la fin de sa partie (ou est interrompu puis
  relancé) ; `-NoRender` fait passer les tests sans image, et les étapes de capture ne bloquent plus (`SwarmGuideTestKit.h`) ;
- aucune mesure de cadence débridée. Vérifié après coup : 42 Go libres, carte à 23 %.
- Conséquence assumée : tant qu'il joue, on code et on vérifie les règles ; l'image attend. `run_all.ps1 -NoRender`
  passe toute la batterie sans image.

**Profondeur et choix** (tout dans la simulation par des fonctions `Player*`, rien de créé) :
- **Colonne d'assaut** (`PlayerCallAssault`, `AssaultTargetFor`) : un membre estimé de son peuple (55 d'estime) obtient
  d'une cité une colonne contre la ruche voisine ; les soldats sortent de la garnison (elle garde sa réserve),
  marchent et se battent comme toute armée ; la faveur coûte 12 d'estime. Dialogue : « Il faut frapper la ruche… ».
- **« Que prépare votre peuple ? »** (`AnswerPlans`, chef ou officier) : colonnes en campagne et leur but, guerres et
  ce qu'elles coûtent, chantiers, ce qu'il a compris de l'Essaim — lu dans la simulation.
- **Défendre une cité** (`SwarmLife.cpp`) : quand la cité où se trouve le joueur est attaquée, on l'appelle aux murs ;
  l'assaut passé, ce qu'il a tué se paie sur le grenier (une ration pour deux, six au plus) et en estime.
- **Se rendre** (`PlayerSurrender`, E près d'un garde d'une cité en alerte) : trois rations d'amende qui entrent au
  grenier, ou un jour de cachot dont on sort roué de coups ; l'alerte tombe, l'estime perdue ne revient pas.
- **Rangs** chez son peuple (tirés de la réputation), contre-offre de ceux qu'une tâche vise, troupes en campagne qui
  interpellent, ouvriers qui embauchent, la rue qui réagit aux séismes et aux incendies (écrits le 17/11 au soir).
- **Vérifié le 18/11 (sans image)** : `Swarm.Sim.*` 13 sur 13 dont `Assault` (la colonne sort de la garnison à l'homme
  près, refusée à un étranger) et `Contracts` (dont la reddition) ; `Swarm.Law.Alarm`, `Swarm.Law.Surrender`,
  `Swarm.Quests.Dialogue`, `Swarm.Quests.Plans`, `Swarm.Guide.TestMenu` (son échec d'hier venait du test : depuis
  qu'on parle à celui que vise le réticule, « le plus proche à qui parler » pouvait être un garde ; il vérifie
  maintenant que le chef est là, en chair, à portée de voix).
- **Pas vérifié** : la défense d'une cité n'a pas de test (il faut un assaut réel sous les yeux du joueur) ; rien de
  ce paragraphe n'a été vu à l'image ce soir.

## J28 — Machine prêtée une soirée ; plantage au rendu ; la défense d'une cité est testée (18/11/2026, 20 h 35 – 21 h 16)

À 20 h 35 le propriétaire part (« tu peux utiliser mon PC à fond »). La soirée s'arrête à 21 h 16 (limite d'usage).

**Ce que la fenêtre a donné, et pourquoi si peu.** Les lanceurs ont reçu trois régimes (en partie / présent / absent).
Le régime « absent » ne s'est enclenché qu'à 21 h 04, puis par à-coups, pour trois raisons trouvées l'une après l'autre :
1. un sémaphore nommé restait pris quand son lanceur était tué → remplacé par des fichiers ouverts en exclusif
   (`%TEMP%\SwarmLocks`), que Windows rend à la mort du processus ;
2. l'absence était lue au compteur de saisies de Windows (`GetLastInputInfo`) ; or **chaque commande de l'outil Bash
   d'un agent y compte pour une saisie** (mesuré : saisie à 21:11:50,1 pour une commande lancée à 21:11:49,6) → la
   présence se lit au curseur de la souris ;
3. un lanceur prenait la place 0 pendant qu'un test lancé « absent » tenait la 1 → le garde-fou tuait le plus récent
   (six lancements d'agents perdus) → une place ne se prend que si le total occupé le permet.
Le garde-fou écrit désormais le régime (`regime.txt`) et chaque arrêt (`governor.log`). **Depuis le 19/11 le régime
« absent » est sur demande seulement** (`governor.ps1 -OwnerSaidAway`) : un curseur immobile ne prouve rien.
Bilan honnête : la moitié de la fenêtre a servi à faire marcher la fenêtre.

**Corrigé sur main.**
- **Plantage au rendu** (`ASwarmCrowd::AcquireBody`) : trois références tenues dans la table `OutfitMaterials`
  au-delà d'un autre `FindOrAdd`, qui peut la réallouer (lecture de 0x0 dès qu'un habitant à corps humain se
  matérialise à l'image ; invisible sans image). Ce sont maintenant des pointeurs. Trouvé par l'agent « prise ».
- Collision de noms en compilation groupée (`Utf8` dans `SwarmIdentity.cpp`).
- **Caméra collée au dos** : dos au mur, le chapeau et le sac du joueur remplissaient l'image (vu sur les captures de
  l'alcôve de la Reine) ; son corps s'efface sous 115 cm et revient à 150 (`SetRenderInMainPass`, il garde son ombre).

**Testé.**
- `Swarm.Rempart.Defense` (nouveau, son propre lancement car il fait avancer le monde) : au jour 25 La Halle est
  assiégée, le joueur est appelé aux murs, abat 29 assaillants par la voie de ses propres coups ; la cité tient, paie
  6 rations, son grenier baisse de 12,00 (2 par ration). Passe sans image.
- Équilibrage sur 100 mondes avec la simulation du jour : Arche 84 %, Essaim détruit 16 %, monde mort 0 % — la référence.

**Fusionné.** Essaim (`22a9783` : dépouille de la Reine, couvée en marche) ; humains (`8327113` : habitants à visage
MetaHuman, pièces de tenue taillées sur le corps).

**Vu sur les captures des agents (1920x1080), sans fard.** Habitants : ce ne sont plus des mannequins (masques,
lunettes, capuches, gestes lisibles) mais tous ont le même visage. Intérieur de l'Arche : bon, de la chair. Reine :
illisible (caméra collée, forêt de pattes noires, aplat violet). Le joueur : devenu le plus laid (étoffes à facettes,
sac en cylindre). Porte des cités : rempart en boîtes olive unies, pendu en cubes, bannières plates. Arrivée en
ville : trois textes superposés. Sol : plaine et désert se distinguent. Tout cela a été renvoyé aux agents.

**Crédits** complétés (chefs, Reine, animations, effets de pas, sons) ; l'auteur et la licence exacts de plusieurs packs
Fab restent à relire sur leur fiche par le propriétaire.

## J29 — Les bêtes ont un corps ; les habitants de loin ne coûtent plus rien ; la machine se partage (19/11/2026)

**Ce que le propriétaire a dit ce matin** (après avoir testé) : « les graphismes étaient mauvais, pas de diversité de mob,
les swarm avaient même pas d'animation du tout, bref la cata » ; « les villes rétrécissent et grossissent, aucune
gueule, on peut rentrer dans rien, tout sur le même niveau, 0 level design » ; « inspire-toi des villes dans Gothic 3,
des bâtiments cachés, des endroits » ; et pour la machine : « c'est juste que tu me laisses suffisamment de ressources
pour jouer à LoL, tu répartis mieux la puissance ».

**La machine, partagée au lieu de coupée.** Jusqu'ici rien ne dessinait pendant ses parties : chaque partie coûtait
30 à 40 minutes d'attente (un test a attendu 35 minutes ce matin). Depuis 10 h 45 : pendant une partie, UNE instance
qui dessine en 1280x720 à 12 images/s (à 10, les tests automatiques ne démarrent pas : le moteur attend plus de
10 images/s), priorité minimale, cœurs 8 à 15. Mesuré pendant une partie : carte graphique à 22 %, 10,3 Go de mémoire
vidéo sur 16,3, 30 Go de mémoire vive libres. Retour arrière en une ligne : `$script:InMatchRender = $false` dans
`Tools/Test/machine.ps1`. Le régime « absent » (trois instances) n'existe plus que sur sa demande
(`governor.ps1 -OwnerSaidAway`). Les cinq branches d'agents du 18/11 ont été fusionnées dans main à la reprise.

**Les bêtes du rang de l'Essaim** (`SwarmCrowd.cpp` : `FBeast`, `CasteKinds`, `UpdateNear`, `BakePose`). Elles étaient
des maillages rigides animés par leur matière. Désormais, pour les 48 plus proches à moins de 60 m, un corps à
squelette par caste, pris dans les packs Paragon du propriétaire, avec ses animations (attente, marche, course, trois
ou quatre coups tirés au hasard, coups reçus de quatre côtés, une ou deux morts, un cri quand elle prend le joueur
pour proie) :
| Caste | Corps | Ce qui la distingue |
|---|---|---|
| ouvrière | Khaimera, 1,25 m | petit bipède voûté, fuit |
| essaimeur | Scorch (le lézard d'Iggy & Scorch, cavalier effacé) | quadrupède rapide |
| lame | Grux, 2,35 m | démon cornu, quatre coups |
| cuirassé | Rampage, 3 m | galope sur ses poings |
| garde royale | Sevarog, 3,6 m | spectre à marteau |
Au loin, la même silhouette : au démarrage, un maillage statique est taillé dans ce corps posé (dernier niveau de
détail, 3 à 5 mille triangles) en trois poses — attente et les deux temps de la course, alternés au rythme des pas —
plus la pose de mort pour les dépouilles. Les champions reprennent tous les gestes de leur corps.
La peau (`M_BeastSkin`, `Tools/Editor/setup_beasts.py`) est calculée, sans texture : plaques sombres, chair dans les
joints, veines fines à la couleur de la caste, relief. Trois passes pour y arriver (la première faisait un pelage
rose, la deuxième un scintillement partout). Correcte, pas belle de très près : pas de texture peinte.
Les tailles de touche suivent : l'ouvrière fait 1,20 m (elle faisait 45 cm).

**Les habitants de loin** (même mécanisme : `BakePeopleShapes`, `HumanShapes`, `M_FarPeople`). Mesure de 11 h 20, rue
d'une capitale : fil de jeu 22 ms par image, dont 2 pour la foule elle-même — le reste, c'est le moteur qui anime
plus de 1 100 composants à squelette (222 habitants, chacun avec trois vêtements et un visage). Et à l'arrivée en
ville, la création de ces corps coûtait une vingtaine de millisecondes par habitant, d'un coup. Désormais : corps
complet pour les 48 plus proches à moins de 35 m (60 m pour qui se bat, parle ou fait un geste ; toujours pour les
chefs, notables, compagnons et acteurs d'une scène) ; les autres sont une silhouette instanciée taillée dans le corps
habillé (attente, deux temps de la marche), teinte à la couleur de leur peuple. Les corps sont créés trois par image,
et 48 sont préparés au chargement. Tant que son corps n'est pas là, une unité garde sa silhouette : personne
n'est invisible. `-SwarmAllBodies` sur la ligne de commande revient à l'ancien comportement ; `-SwarmCrowdCost` écrit
le coût de chaque poste de la foule au journal.
| Lieu (1280x720, bridé, machine partagée avec une partie de LoL) | fil de jeu avant | après |
|---|---|---|
| rue de la capitale | 22,3 ms | 9,3 ms |
| devant la Reine | 10,3 ms | 5,4 ms |
| en forêt | 12,2 ms | 6,2 ms |
Ces chiffres sont pris sous bride et à priorité minimale : ils disent le sens et l'ordre de grandeur, pas la cadence
réelle. Aucune mesure débridée en 2560x1440 n'a encore été faite depuis le 17/11.

**Testé à l'image** : `Swarm.Flesh.Queen` (cinq castes : un corps, une animation qui joue, une silhouette de loin
taillée), `Swarm.People.*` (5), `Swarm.Perf.Budget`. **Pas vérifié** : les bêtes en combat réel contre le joueur
(portée des coups avec les nouvelles tailles), les silhouettes d'habitants vues par un œil humain en marchant, les
dépouilles de loin, la batterie complète après ces changements (lancée à 11 h 57).

**Cités, phase 1** (sous-agent Opus, commit `98b4ac7`, fusionné à 12 h 20 ; `SwarmCityLevels.cpp`, tests
`Swarm.Cites.Emprise` et `Swarm.Cites.Niveaux`). L'emprise d'une cité est fixée à sa fondation (`popCapacity`) : rempart,
portes, rues et maisons ne bougent plus ; la population se lit dans les maisons condamnées et les tentes hors les
murs. Chaque capitale a une butte de 4,8 m avec un escalier jusqu'à la halle du chef (on y entre, le chef et ses
officiers y sont), une taverne et une forge où l'on entre, un chemin de ronde praticable, deux lieux cachés (poterne
derrière des caisses, cour murée par une brèche). Vu à l'image par moi : c'est un vrai pas (on entre, on monte), mais
les salles sont des boîtes nues, la cité reste un disque plat semé de maisons, et aucune n'a encore de caractère.
**Phase 2 en cours** (autre sous-agent) : relief franc, rues, un repère par peuple, intérieurs habillés et habités,
lieux cachés qui récompensent, mesure de cadence (la phase 1 a ajouté environ 7 000 cubes instanciés, non mesurés).

**Cadence réelle, enfin mesurée** (12 h 06, propriétaire d'accord pour la pleine puissance, 2560x1440 natif, débridé,
seul sur la machine, `Swarm.Perf.Budget`) :
| Lieu | fil de jeu | rendu | GPU | cadence possible |
|---|---|---|---|---|
| rue de la capitale | 5,1 ms | 9,8 ms | 9,1 ms | 102 images/s |
| devant la Reine | 3,4 | 10,6 | 10,0 | 94 |
| en forêt | 3,5 | 10,3 | 9,7 | 97 |
| sous l'orage | 3,8 | 9,2 | 8,6 | 108 |
| terre de l'Essaim | 3,1 | 11,2 | 10,6 | 89 |
| nuit en cité | 5,9 | 9,2 | 8,9 | 109 |
À 67 % de rendu : 6,4 à 7,2 ms (130 à 150 images/s). D'où DLSS Qualité par défaut tant que le joueur n'a pas choisi
(touche U). Le fil de jeu n'est plus le goulot ; c'est le rendu. Mesure faite AVANT la fusion des cités phase 1.

### J29 (suite) — après-midi du 19/11 : batterie complète, relief des bêtes, coups annoncés

**Batterie complète à l'image** (12 h 17 – 12 h 46, deux flux en parallèle pendant la demi-heure de pleine puissance
accordée par le propriétaire) : 70 tests passent ; trois échecs et un plantage, tous expliqués et corrigés —
- `Swarm.People.Moves` plantait (`AddShowcaseMove` cherchait le modèle de corps d'un figurant par `BodyKindOf`, qui
  rend -1 à qui n'a que sa silhouette : `FullBodyKindOf`) ;
- `Swarm.World.Structures` comptait encore les anciennes formes de ruche (elles sont en pièces de chair depuis J24) ;
- `Swarm.Life.Scenes` échouait quand la rue avait lancé d'elle-même la harangue avant lui ;
- `Swarm.Combat.D_Siege` comptait les assaillants après qu'un jour a passé (une autre troupe était arrivée).
Relancés sur le build suivant (voir plus bas). `Swarm.Monde.*` passe en entier, dont la mesure du vent : entre deux
images, 62 % des pixels d'une cime changent par temps clair (0 à 23 % sans vent), 32 % de l'herbe sous l'orage (0 %
sans vent), 62 % sur une cime à 42 m. Le vent est donc prouvé sur main.

**Relief sculpté des bêtes** (sous-agent Opus, commit `3833c63`, fusionné). Chaque emplacement de matière des cinq corps
reçoit la carte de normales d'origine de son modèle (17 cartes copiées, ramenées à 2K : 91,6 Mo hors dépôt ; 26
instances `Content/Swarm/Materials/Beasts/MI_Beast_*`, `Tools/Editor/setup_beast_detail.py`), sous la peau de
l'Essaim ; les creux reçoivent la chair et les veines. Vu par moi : le cuirassé de près est réussi (écailles, muscles,
veines violettes dans les sillons). L'ouvrière reste la plus pauvre. Occlusion réelle seulement sur l'armure du
cuirassé. Coût en mémoire vidéo non mesuré à part.

**Coups annoncés.** Contre le joueur, une bête frappait à l'instant où son geste commençait : impossible de lire le coup.
Désormais le geste part (animation, son), et le coup ne porte qu'après un délai — essaimeur 0,30 s, ouvrière 0,35,
lame 0,40, garde 0,55, cuirassé 0,65, un dixième de plus pour un champion — et seulement si le joueur est encore à
portée. Pendant qu'elle arme, la bête ne poursuit pas. Entre unités de la foule, rien ne change (la simulation et ses
tests restent ce qu'ils étaient).

**Divers** : DLSS Qualité par défaut ; cri audible d'une bête qui prend le joueur pour proie ; un habitant garde son
corps quatre secondes après avoir parlé ou frappé (il ne clignote plus entre corps et silhouette) ; lumière d'appoint
dans le dos du joueur (il était une découpe noire en intérieur) ; un long sous-titre passe à la ligne et reste à
l'écran plus longtemps ; les tests automatiques démarrent dès 5 images/s.

**La machine, encore.** À 12 h 04 le propriétaire a donné la pleine puissance (« j'ai fini de jouer à LoL… tu peux
utiliser les perfs au max » : `governor.ps1 -OwnerSaidFull`, trois instances), à 12 h 46 il l'a reprise (« me voilà de
retour sur LoL, calme comme avant »). Retour immédiat au régime partagé ; un test lancé hors partie est désormais
arrêté et relancé en petit (1280x720, 12 images/s) si une partie commence.

**Cités, phase 2** (sous-agent Opus, commit `85bced7` « en cours », fusionné dans main à 13 h 20 et pris en charge
par moi pour la validation). Butte de 7 à 9,5 m couronnée d'une terrasse murée, grand escalier à portail, cité bombée
de 3 m et quartier bas, rue annulaire bordée de maisons (Concordat, Affranchis), un repère par peuple (rotonde à
coupole dorée, haut-fourneau et cheminées qui fument, grand feu des Clans, beffroi, carcasse d'immeuble habitée),
intérieurs habillés, tavernier et forgeron à leur poste, lieux cachés qui révèlent sur la boussole un lieu oublié de
la simulation (`DiscoverCityNooks`). Vu sur ses captures : Aram a enfin une forme (terrasse, rotonde, halle, escalier)
et le beffroi du Protectorat dépasse du rempart. Effet de bord trouvé par la batterie et corrigé : le menu de test
« aller voir un chef » visait encore la place alors que le chef siège dans sa halle (`GetHallSpot`).
Deux pièges de tests liés aux cités et aux bêtes, corrigés : `Swarm.Cites.Emprise` fait passer 25 jours et le joueur
mourait de faim, réapparaissant à sa cité de départ au milieu du test suivant ; même chose avant la mort de la Reine
(joueur laissé mortel au milieu des bêtes par le test précédent). `RespawnNow()` pour les tests.

**Combat lisible** : un cercle clair se referme à l'écran sur la bête qui arme son coup (`SwarmPriseHUD.cpp`), l'aide du
premier combat l'explique, et `Swarm.Betes.Annonce` vérifie en mêlée réelle qu'aucun coup ne porte sans annonce.

**Trois chantiers d'agents lancés à 13 h 25** (propriétaire : « et bah on s'arrête pas ») : cités phase 3 (branche
`monde`), variété des habitants — femmes, visages, teints, coiffes dans les silhouettes de loin (branche `humains`),
lieux sauvages mis en scène — tombeaux, épaves, sources, caches, indices dans le paysage (branche `atmo`).

## J30 — La grande liste du propriétaire ; ménage du disque ; armées en formation (19/11/2026, après-midi)

**Retour du propriétaire après avoir joué** (ses mots sont dans la mémoire « direction de jeu ») : temps trop rapide,
feux de forêt « qui ressemblent à rien », humains rigides et tous pareils, personnage non personnalisable, dialogues et
arrivée sans allure, villes laides et identiques, pas de grottes ni de mines, l'Essaim ne détruit pas le monde, ruines
en « rectangles noirs », aucune guerre entre humains ni formation, bases de l'Essaim qu'on ne peut ni visiter ni
saboter, pas d'Essaim « logique » (mineurs, camps, relais), ni gore, ni esclavage, ni prison, pas de bétail humain,
chute de l'Essaim trop courte, cinématique d'ouverture à refaire (il en a donné le scénario), inventaire « immonde »,
carte bavarde, pas d'animaux, herbe et blé inertes. Il a raison sur chaque point.

**Ce qui a été fait dans l'heure**
- Un jour dure vingt minutes réelles (`Config/DefaultGame.ini`, `SecondsPerDay=1200` ; quatre avant). Le joueur garde
  ses vitesses de temps, le sommeil et « laisser passer trente jours ».
- Le « joueur tout noir » des intérieurs : le corps effacé quand la caméra est serrée restait dessiné dans la passe de
  profondeur (une découpe noire). `SetRenderInDepthPass(false)` en plus : vu à l'image dans la taverne.
- Chaque caste se bat à sa manière contre le joueur : le cuirassé prend son élan, charge droit et s'essouffle s'il
  manque ; l'essaimeur frappe et se dérobe en arc. Compilé, `Swarm.Betes.Annonce` et `Swarm.Combat` passent ; pas
  encore joué par un humain.
- Fusions : habitants variés (femmes sur le corps « Echo », six teints, carrures, silhouettes de loin par peuple),
  cités phase 3 (repère des Clans, éboulis d'Aram praticable, greniers de taverne, tours de guet des bourgs, saillants
  du rempart ; `Swarm.Cites` et `Swarm.Monde.Villes` passent chez l'agent). Jugement à l'image : mieux, mais le
  rempart reste un disque, le sol des cités est une paille uniforme et il n'y a pas de vraies rues.
- **Armées humaines en formation** (`SwarmCrowd.cpp` : `FMarch`, `UpdateMarches`, `FormationSpot`) : le centre montré
  d'une armée marche au pas (1,7 m/s) vers la case où la simulation la place au lieu d'y sauter ; colonne de marche
  de trois ou quatre de front ; à l'arrêt, ligne de bataille face à l'objectif ; les rangs se resserrent quand un
  soldat tombe. Test `Swarm.Guerre.Formation` (captures `J30_armee_ligne.png`, `J30_armee_colonne.png`).
- **Disque** (le propriétaire : « tous les assets que tu n'utilises pas, supprime-les ») : 396 Go libérés. Cache du
  lanceur Epic (266 Go de doublons de packs déjà installés ; gardés : `RomanCave_4.25` dont dépend la vue à jonctions,
  et un pack sans autre copie), puis Electric Dreams, Valley of the Ancient et City Sample Buildings, jamais utilisés
  (aucune référence dans le code, les scripts ni les copies des agents ; retéléchargeables). 685 Go libres.
- **Nouveaux packs** : le propriétaire autorise le téléchargement de packs gratuits. Liste à liens :
  `Docs/PACKS_A_PRENDRE.html` (animaux, feu, effets Niagara d'Epic, créatures et héros de Paragon). Je n'ai aucun outil
  pour cliquer dans Fab ni dans le lanceur, et Fab refuse la lecture automatique : prix et compatibilité de chaque
  fiche restent à vérifier par lui au moment d'ajouter.

**Chantiers d'agents en cours à 14 h 45** : cités phase 3 (suite), lieux sauvages, véhicules (règles et 3D « en
cours », rien de stable encore), spectacle (feux de forêt, herbe couchée, chute de l'Essaim, cinématique : branche
`essaim`), Essaim dans le monde (ruines, destruction, ruches visitables, récolte visible, bétail humain, gore : branche
`ruche`), joueur et interface (création du personnage, inventaire, carte, dialogues : branche `interface`). À lancer
quand les règles des véhicules sont fusionnées : sociétés (esclavage, prisons) et guerres entre peuples ; grottes et
mines profondes (packs `Scene_OldMine`, `Scene_QuarrySlate`) ; animaux dès que le pack arrive.

**Suite de l'après-midi (14 h 30 – 15 h)**
- Le propriétaire a mis en téléchargement une cinquantaine de packs de sa bibliothèque Fab (« traite au fur et à mesure,
  ce que tu prends pas, dégage »). Un concierge tourne en fond (script de travail `pack_janitor.ps1`, hors dépôt) : il
  vide le cache du lanceur dès que la copie installée d'un pack est complète, décrit chaque pack arrivé et supprime
  ceux que je marque. Pris à cette heure : animaux (loup, renard, cerf, biche, cochon, corbeau), feux Niagara M5,
  effets d'Epic (explosions, fumées, impacts), chevaux avec selle et cavalier, épées, couteau, visages et corpulences
  de City Sample Crowds, surfaces de sol et de métal. Supprimés : asphaltes, bétons, sons « cyber ». Détail dans
  `Docs/CREDITS.md`.
- **Erreur de ma part, réparée** : mes commits « tout ajouter » ont fait suivre ces packs par git pendant une
  demi-heure (a7b4d9b à 9aebfea ; 3,8 Go d'objets LFS restent dans `.git`, l'historique n'a pas été réécrit). Retirés
  du suivi et ignorés (a1737b1). Danger écarté : une copie de travail qui les suivrait encore les effacerait à travers
  sa jonction en fusionnant le retrait — d'où `Tools/Test/merge_main.ps1`, seule façon de fusionner main désormais.
  Règle : tout dossier à la racine de `Content/` est un pack hors dépôt ; ce que le projet crée va sous `Content/Swarm/`.
- Fusionnés : spectacle (841b79b : feux de forêt, herbe et blé couchés avec traînée, chute de l'Essaim en vingt
  secondes avec onde de choc, début de cinématique non branchée), Essaim dans le monde (7b36075 : ruines, dedans des
  ruches et sabotages `PlayerBurstBrood` / `PlayerTakeHiveStore` / `PlayerBurnHiveStore` / `PlayerFreeHerd`, bétail
  humain — 25 % des habitants d'une cité prise, mangés à 4 % par jour —, terre ravagée, sang plus franc ; équilibre
  annoncé 83/17/0), habitants (silhouettes de loin allégées). Jugement à l'image : feux de jour bons, troncs brûlés
  zébrés et flammes roses la nuit ; ruines encore en découpes noires (le reproche même du propriétaire) ; dedans des
  ruches rose bonbon et ouvert sur le ciel. Renvoyé aux agents.
- Armées : bannière au peuple portée en tête ; `Swarm.Guerre.Formation` vert (ligne 90 %, colonne 100 %, 1,70 m/s).
  Leçon de test : sous bride le temps du monde avance bien moins vite que l'horloge (1,4 s en 12 s) — attendre en
  temps du monde (`FWorldDelayCommand`).
- Machine : carte graphique mesurée à 9 % et 8 Go de mémoire vidéo avec deux instances bridées ; le régime « lecture »
  de main passe à trois instances (les copies des agents restent à deux : la troisième sert de voie à l'orchestrateur).
- Agents lancés : faune (branche `faune`), cités phase 4 — vraies rues, rempart non circulaire (branche `monde`),
  sociétés et guerres (branche `societes`), spectacle 2 — cinématique complète et branchée (branche `essaim`).
  Toujours en cours : véhicules (+ chevaux), habitants (+ visages), lieux sauvages, Essaim, interface.

**Suite (15 h – 16 h) : fusions en rafale, agents de seconde vague**
- Fusionnés dans main, chacun après lecture de ses captures :
  - véhicules (e8187fb, 94c2a1c) : 13 genres dans la simulation (`SwarmSimVehicles.cpp`) et en 3D (`SwarmVehicles.*`),
    acheter / louer / voler / rendre / réparer / démonter, conduite ; engins humains encore « jouets », chair rose ;
  - Essaim dans le monde (c422278, b89d904) : ruines en pierre lisible, salle de ruche composée (cœur, couvain,
    réserve), bave qui goutte ;
  - habitants (3e8175e, c7a886d) : douze visages et corps de femme de City Sample Crowds, coiffures, tête nue,
    tenues de peuple — quinze habitants côte à côte, quinze personnes ;
  - cités phase 4 (edcff86, non STABLE) : `SwarmCityPlan.*` — enceinte polygonale par peuple, rues, parcelles façade
    sur rue —, `SwarmCityStreets.cpp` ; les habitants qui flânent suivent `Route` (`Stroll`) ; îlots encore vides ;
  - sociétés et guerres (deeec13) : `SwarmSimSociety.cpp` — guerres avec cause et campagnes, captifs comptés, justice
    du joueur (geôle, amende, évasion), places réservées pour les cercles hors des peuples ; `Docs/SOCIETES.md` ;
    12 batailles rangées par partie au lieu de 3,4, première bataille au jour 3 au lieu du jour 67 ;
  - faune (283b808) : `SwarmWildlife.*` — hardes de cerfs, renards, meutes de loups, sangliers, cochons, corbeaux,
    fuite devant le feu et les coups de feu, chasse et dépeçage. Vue à l'image : réussie.
- Sauvegarde : simulation 9, jeu 11. Équilibre des règles réunies (véhicules + Essaim + sociétés), 100 graines :
  Arche 83 %, Essaim détruit 17 %, monde mort 0 %, 80 % des parties entre 600 et 1000 jours (référence d'avant : 84 /
  16 / 0, 87 %). La durée s'étale un peu ; à surveiller.
- PAS fusionné : le film d'ouverture (branche `essaim`, fffffee : 14 plans, 90 s, branché sur « Jouer » par
  `ASwarmCinematic::PlayThen`). Cités en flammes réussies ; plan des bêtes cassé (caméra dans la chair), rayon du
  colosse invisible. Renvoyé à l'agent. Pas fusionnée non plus : l'interface (inventaire refait — vu, propre —,
  création du personnage à six passés, dialogues cadrés, carte allégée) : deux agents se sont marché dessus dans la
  même copie de travail ; le second remet la compilation d'aplomb.
- Foule (`SwarmCrowd.*`) : `SetUnitSaddle` / `SetUnitRiderPose` (un soldat en selle sur le cheval d'un autre acteur :
  place et cap lus sur le composant, corps posé racine sur racine, pose de cavalier donnée chaque image).
- Machine : trois instances en régime « lecture » saturaient la mémoire vidéo (14 Go sur 16) et faisaient tuer des
  lancements (« moins de 6 Go libres ») : retour à deux. La batterie complète n'a jamais pu finir : main change toutes
  les dix minutes et une fusion qui modifie un `.uasset` échoue tant qu'une instance de main tourne. Méthode retenue :
  tests ciblés à chaque fusion, batterie complète seulement quand les agents sont au repos.
- Packs : toute la collection Paragon est arrivée (pour les chefs, champions et chefs de cercles) ; pris aussi : feux
  et tentacules Vefects, explosions, épées, récif, surfaces de pierre, de sable et de rouille. 556 Go libres.
- Agents lancés : véhicules 2 (chevaux, colosse escaladable, engins moins « jouets »), cités 5 (densité, niveaux,
  faubourg), monde sauvage 2 (reprise des tombeaux, grottes, mines profondes, essences d'arbres), interface 2,
  cercles hors des peuples (branche `societes`). Agents arrêtés sans rapport et relayés : lieux sauvages, interface.

**Suite (16 h – 18 h) : le propriétaire joue vingt minutes sur le jeu réuni — verdict sévère**
- Remis à 17 h 07 (main 0b19c5e : tout ce qui précède, plus cercles hors des peuples, interface, menu de test à douze
  catégories, figurants et héros de la foule, film passe 2). Il a joué jusqu'à 17 h 36.
- Ce qui a gâché le test, trouvé dans SES journaux (`Saved/Logs`) :
  - plantage dans une ruche (`ASwarmStructures::UpdateHiveDrips`) : effets Niagara gardés dans des variables statiques
    de fonction, détruits par le ramasse-miettes. Corrigé (mis à la racine), ainsi que la fumée des ruines. Troisième
    plantage de cette famille aujourd'hui (coiffures de la foule, corps de la faune) ;
  - TOUTE L'HERBE en cartes grises à damier : `M_Grass` ne compilait plus (« CollectionParameter has invalid parameter
    None »). Cause : `setup_trample.py` recréait les paramètres de `MPC_Trample` à chaque passage (nouveaux
    identifiants) sans re-sauver les matières déjà branchées. Script corrigé (`collection()` garde l'existant,
    `rewire()` rebranche par le nom) ;
  - 26 matières retombaient sur la matière par défaut faute d'usage coché (Nanite, instances) : falaises, chapelle,
    colonnes romaines, chair de l'Essaim, eau, ferraille. `Tools/Editor/fix_material_usage.py` + `missing_usage.txt` ;
  - textures manquantes : `M_Kwang_Weapon`, `M_Grux_Torso`, `Debris_Mat`, `Saddle_Wood`, `Reins_Metal` ;
  - il avait ouvert TROIS jeux (chargement d'une à deux minutes sans rien à l'écran) : mémoire vidéo à 15,5 Go sur 16.
    Son raccourci refuse désormais un second lancement et garde l'écran de démarrage ;
  - mes agents dessinaient et compilaient derrière : `machine.ps1` (`Test-OwnerPlaysSwarm`) interdit maintenant toute
    instance qui dessine tant qu'un `UnrealEditor.exe` (son jeu) tourne.
- Son retour (mot pour mot dans la mémoire du projet) : film de qualité basse, « asset flip », plans qui traversent
  le décor, scarabée sans allure ; ancien fond du titre à retirer, menus « AI slop HTML », écrans noirs — il veut en
  fond deux armées qui se battent pendant que l'Essaim arrive, bien cadrées ; création du personnage (modèles
  superposés, pièces qui ne suivent pas, tête dans le chapeau) ; nuages au mouvement incohérent ; flou abusif des
  zones sèches ; Redoute sans allure, villes vides, habitants sans vie ; sol sans vie (référence : Gothic 3) ; arme à
  feu « Nerf » ; le jeu rame ; une biche apparaît sur le loup tué ; bûcherons qui coupent le vide ; blé qui
  tourbillonne ; char de guerre sans chevaux ; pas de très grandes structures ; pas de voitures de combat ni
  d'aéronefs. Ouverture voulue : arme en main, meute de loups devant la cité, les habitants viennent aider, quelqu'un
  vient parler.
- Leçon de méthode : avant de lui dire « teste », compiler main, lancer le jeu réuni une fois hors écran et lire le
  journal (matières par défaut, compilations de matière ratées, plantages) ; je ne l'avais pas fait.
- Fusionnés après son test : interface (3d6a3d5), lieux sauvages et grottes (b70a0fe), véhicules conduits (ece9c5d).
- Agents lancés à 17 h 45 : optimisation et atmosphère (branche `perf` : mesure système par système, nuages, flou,
  blé, démarrage, pointeurs nus), interface 3 (création, titre avec bataille en fond, habillage des menus), combat et
  ouverture (branche `faune` : sensations de tir, meute devant la cité, biche sur le loup), véhicules 3 (attelage
  logique, voitures de combat, autogire, dirigeable). Retour transmis aux agents en route : film, cités, foule,
  modèles Meshy (colosse, véhicules de combat, monuments géants ; 2 236 crédits sur 2 800), monde sauvage (sol
  vivant, très grandes structures), menu de test.

**Suite (17 h 50 – 19 h 10) : une heure sur le film d'ouverture, à la demande du propriétaire**
- Sa demande : « focalise-toi sur cette animation avec tous les agents pendant 30 min – 1 heure… que ce soit fait et
  qu'on n'y retouche pas » ; puis « animation veut dire film animé : il faut que ça bouge… s'il faut, réduis à 20 ou
  30 s mais très bien faite » ; puis « tu me mettras le fichier mp4 sur le bureau ».
- Organisation : réalisateur (branche `essaim`), agent son (bande entière synthétisée, `Tools/Assets/film_sound.py`,
  `/Game/Swarm/Audio/Film/SW_Film_Mix`, 34,0 s, −14,1 LUFS, 16 événements calés à ±20 ms), critique indépendant qui
  ne voit que les captures (verdict plan par plan à chaque tour), agent modèles (colosse Meshy importé d'abord),
  agent foule (bannières à emblème). Places de dessin réservées au film : `priorite.txt` + barrière
  `unreal.seul.lock` dans `machine.ps1`, et un garde (`scratchpad\priority_guard.ps1`) qui arrête toute autre
  instance qui dessine — nécessaire : des lanceurs partis AVANT la règle la contournaient (ancien `machine.ps1` en
  mémoire) et le garde-fou a tué trois fois la capture du film (mémoire : chaque Unreal prend 12 Go).
- Résultat : film de 34 s, 13 plans, montage figé dans `Saved/film_montage.txt` (copie `essaim`), dans main
  (974ac57) ; MP4 2560×1440, 30 images/s, 160 Mo, copié sur le Bureau du propriétaire à 19 h 07
  (`SWARM_film.mp4`). Export : test `Swarm.Film.Export` (pas fixe 1/30 s, une capture par image) puis
  `Tools/Film/assemble_film.ps1 -Audio Saved\FilmSound\SW_Film_Mix.wav` (ffmpeg installé par winget) ; ~7 min.
- Jugé réussi (critique, réalisateur, moi) : Protectorat dans les blés, terre prise, cité en feu, ligne en enfilade,
  révélation du colosse, charge, rayon (le meilleur), titre. Moyen : marche aux bannières, présage (point violet
  dans un ciel vide), impact (pas de colonne de feu, relief gris), éclat sans le soldat au masque vert.
- Retouches possibles, dans l'ordre du réalisateur : impact avec colonne de feu ; présage avec traînée et gens dans
  le cadre ; lampe au sol à droite du colosse ; soldat au masque vert ; bannières de la foule (`SwarmBanner`) à la
  place de `ASwarmCinematic::AddBanner` ; l'Essaim qui franchit la porte de la cité en feu.
- NON VÉRIFIÉ : le parcours « Jouer → création → film → réveil » ; la vraie pression sur Échap ou Espace ; le film
  n'a été ni vu en mouvement ni écouté par moi (planche de douze images et mesures seulement).
- Fusionné dans main pendant cette heure : menu de test (460e329), biche sur le loup (886ff78), attelage logique et
  cinq moteurs (e714fc5, simulation 12 / jeu 14), famine et démobilisation qui épargnent la case jouée (7f69528),
  convois de captifs réparés (1b807ff : 0 → 3,2 convois par partie, équilibre 82/18/0), bannières (8799fb3),
  68 modèles Meshy (2a8bfd1 ; 2 764 crédits sur 2 800). Démarrage : `UE_SKIP_UBT_SDK_SETUP=1` dans le raccourci du
  propriétaire et les lanceurs (jusqu'à 90 s gagnées). Piège de compilation groupée : `FindGameWorld` ambigu dans
  les tests dès qu'un voisin fait `using namespace SwarmGuideTests;` — appels qualifiés partout.

**Suite (19 h 10 – 20 h) : le film refusé, second test, le chantier réorganisé**
- Le propriétaire a vu le MP4 : « encore bien bien pire qu'avant » (texte non voulu, plans sans lien, montagnes plates,
  bêtes qui marchent à reculons, cité qui brûle sans assaillant, « une seule ligne » jugée bateau, scarabée Meshy
  immobile « immonde »). Ma faute de méthode : validé sur des images FIXES. Règle désormais : rien qui bouge ne se
  juge sur une image fixe. Le film ne sera retourné qu'une fois le siège et le titan animé dans le jeu.
- Il a rejoué (19 h 18 – 19 h 29, main d'avant les correctifs du soir) et dicté une seconde liste, puis une idée
  d'ouverture (« spawn dans le désert avec seulement une voiture »), des titans de la taille d'un bourg sous lesquels on
  se bat et dans lesquels on entre, des tornades, de la mégafaune ; puis : « sois autonome dans la profondeur… fais
  point par point ce qui manque… prends les devants » et « on sent à peine la différence entre Bas et Épique ».
  Tout est mot pour mot dans la mémoire du projet.
- `Docs/A_FAIRE.md` : LA liste de ce qui manque (71 points, douze chapitres : constat, cible, qui, état), avec ce que
  j'ajoute de moi-même (version empaquetée, rangs et maîtres à la Gothic, compagnons, contrats, eau, son du monde,
  partie automatique du soir). `Docs/REFERENCE_VILLES.md` : Gothic 3 lieu par lieu, principes, grille de rangs.
  `Docs/AGENTS.md` : nouvelle section « Règles du 19/11/2026 au soir », que tout brief d'agent cite.
- Machine (63 Go, 12 Go par Unreal) : `machine.ps1` fait attendre un lanceur tant qu'il ne reste pas 17 Go libres et
  que 90 s n'ont pas passé depuis le départ précédent ; garde-fou en `-OwnerSaidFull` (« puissance max »). La file
  prioritaire (`priorite.txt` + barrière) existe toujours mais bloque tout le monde : à n'utiliser que brièvement.
- Fusionné : ciel à vent constant, brume du désert, plus aucune matière par défaut (perf e2d7ee1) ; place de justice
  et marché aux captifs en mobilier Meshy (sociétés 1048a78).
- Agents terminés : cités phase 5 (aucun STABLE, relevé par l'urbanisme), foule 2 (bannières seules en STABLE ; le
  reste compilé non vu, repris par foule 3), sociétés (convois, repaires, justice), film, son, critique, référence.
- Agents en route à 20 h (copie) : urbanisme (`villes`), architecture (`archi`), vie quotidienne (`humains`), foule 3
  (`foule`), relief (`relief`), rendu (`rendu`), optimisation (`perf`), Essaim (`ruche`), titans (`titans`),
  catastrophes (`dangers`), armées (`armees`), véhicules 3 (`prise`), combat et ouverture (`faune`), joueur
  (`joueur`), bâtir (`batir`), interface 3 (`interface`), profondeur (`societes`), monde sauvage 2 (`atmo`).
- En attente de sa réponse : ré-sauvegarde de Rampage, Terra et 21 instances de matière (refusée par le contrôle des
  permissions : fichiers de packs) ; retirer ou non le film de 34 s du lancement d'une partie ; le titre « queens
  domain » que je n'ai pas compris.

**Suite (20 h – 21 h 15) : le cap, les causes trouvées, la revue en images**
- Délégation du propriétaire (« fais ce qui semble le mieux pour le projet en ayant une vision globale et future »,
  « continue sans t'arrêter ») : LE CAP est `Docs/TRANCHE.md` — une première heure de jeu indiscutable dans une
  région (Protectorat de Hael : réveil dans le désert case 45,20 avec une voiture, hameau de La Herse, capitale Fort
  Hael, cité de Redoute, bourg de Bastion, Reine de l'Essaim en 36,20), puis on étend. Une règle de simulation
  (branche `villes`, pas encore dans main) reproduit cette disposition dans chaque monde.
- Puis : « le film… surtout et avant tout le moment du scarab… si tu dois d'abord faire les scarabs qui font la taille
  des montagnes alors fais-le… en avoir qui roam dans la map peut être incroyable ». Le titan (corps de 250 à 400 m,
  animé par code, errant) devient la priorité n° 1 (branche `titans`) ; le film est refait à partir de la séquence du
  titan (branche `essaim`, découpage validé : `Docs/FILM.md` de cette branche, 14 plans, sans texte, bataille de
  démonstration hors simulation, événements du jeu écrits pour recaler le son).
- CAUSES TROUVÉES : (1) « le jeu lag » = image figée 11,8 s à chaque changement de jour (`ASwarmStructures::Refresh`
  refait toutes les cités d'un bloc) + pics de 0,4 à 1,1 s à la chute et aux apparitions de véhicules ; mesures
  complètes dans `Docs/PERF.md` (GPU 5,7 à 9,5 ms et 6,4 à 7,3 Go de mémoire vidéo partout ; foule 3,2 à 3,8 ms en
  cité). (2) « Bas = Épique » = le film d'ouverture verrouillait les réglages graphiques (tout changement ensuite
  ignoré), textures plafonnées à 2K pour tous, Épique du moteur abaissé. (3) Compilations en file unique : verrou
  global d'UnrealBuildTool — `build.ps1` passe `-NoMutex`, trois places, le tronc principal passe devant
  (`build.main.txt`). (4) Chaque instance du jeu prend ~14 Go de mémoire vive : deux instances au plus sur la machine.
- Piège revu trois fois : un lanceur PowerShell déjà en attente garde l'ancien `machine.ps1` en mémoire ; toute règle
  nouvelle ne vaut que pour les lancements suivants.
- Fusionné : titre sans séquence imposée et création du personnage (13c6e43), tourelle et attelage (ad52756),
  architecture a / b / c (784d40e, b6b1242, 7d84001 : cinq styles, rue du Protectorat réussie de jour et de nuit),
  registre de la vie quotidienne (61e9c0d), optimisation (7ea1212), place de justice (1048a78). Vérification à
  l'image de main à 21 h 02 : 10 tests sur 10, 0 erreur fatale, une matière par défaut (`M_Wear`, usage Clothing).
- Vu et renvoyé (pas fusionné) : monde sauvage (rochers qui flottent), relief (bosses lisses ; le désert du réveil est
  une plaine plate), rendu (rue et nuit identiques aux quatre niveaux), tir (flamme démesurée), ruche (noire et
  rose, illisible), patrouilles (clones plantés), catastrophes (aucune capture ne montre son sujet), villes (bonne
  échelle par rang mais octogones réguliers en grille, dalles brunes), soldats (casque qui flotte, armure en blocs).
- Agents remplacés au bout de leur temps : véhicules 4, optimisation 2, combat 2, foule 3 ; nouveaux : empaquetage et
  partie automatique (`paquet`), profondeur (`societes`), film 5 (`essaim`).

## Reprise — où en est-on, quoi faire ensuite

État au 17/11/2026, 22 h 10 : main = `82fe008`, compilé (DLL de 22 h 03). Lire J24 à J26.

Ensuite, dans l'ordre :
1. **Cadence** : mesurer sur machine calme (`run_tests.ps1 -Filter "Swarm.Player.Traverse" -MaxFps 0 -ResX 2560
   -ResY 1440`), puis couper (220 corps à squelette par capitale, nuages et brouillard volumétriques, ombres).
2. **Suite complète des tests sur main** (`Tools/Test/run_all.ps1`), dont `Swarm.Guide.TestMenu`.
3. **Récupérer le travail non fusionné** des cinq copies (`git -C C:\Dev\SwarmWT\<nom> status`), puis les
   supprimer proprement : `cmd /c rmdir` sur chaque jonction de `Content` AVANT `git worktree remove`.
4. **Personnages de près** : retirer les tambours, têtes à la forme d'un crâne, étoffes fines ; demander au
   propriétaire d'ajouter des personnages humains depuis Fab (femmes, visages).
5. **Reine, bêtes, vent des feuillages, variété du sol, barre en main, inventaire.**
6. Crédits à compléter : MC_Sample et FreeAnimsMixPack (gestes), Paragon Narbash et TwinBlast (chefs), sons de
   tonnerre (Medieval Village) et d'ambiance (Valley of the Ancient), effets Niagara des packs.


## PAUSE du 19/11/2026 à 22 h 35 — le propriétaire : « mets tout en pause pour ce soir, on reprend demain »

Tout est arrêté : les 22 agents, le garde-fou, les lanceurs en file, les compilations, la cuisson. Rien ne tourne pour
le projet (seule l'instance du jeu que le propriétaire a ouverte lui-même à 22 h 32 est restée : ne pas y toucher).

**État de main (4ed04ff), à connaître avant tout :**
- Sources : foule ba79f0d, sauvegardes 2cd0c14 (simulation 14 / jeu 15), relief 6d7689f, architecture (d) a556108.
- BINAIRE EN RETARD : la DLL date de 22 h 18 (foule + ruche + sauvegardes). La compilation avec le relief et
  l'architecture (d) a été interrompue par la pause. Le contenu (`M_Terrain` du relief) est donc plus récent que le code
  compilé : état NON vérifié.
- Dernière vérification complète : 47 tests de règles verts à 22 h 25 (avant relief et architecture d).

**Reprise, dans l'ordre :**
1. `build.ps1` sur main, tests de règles, puis `smoke.ps1` ; lire le verdict, regarder planche et vidéo (le relief a
   bougé partout : chercher ce qui flotte ou s'enfonce ; terres de l'Essaim sombres et non roses).
2. Relancer le garde-fou (`governor.ps1`), remettre `priorite.txt` / `build.priorite.txt`, réarmer `watch_all.sh`.
3. Relancer les agents un par un sur leur copie (`C:\Dev\SwarmWT\<nom>` : le travail non commité y est resté sur
   disque ; leur faire lire `git status` et `git log` d'abord). Priorités : titans (refonte compilée — chitine sombre,
   pattes épaisses, élytres, ombre — jamais vue à l'image ; captures de 22 h 05 effacées), film (bataille compilée sur
   `essaim` 1bfcd98, rien de tourné), optimisation (gel du changement de jour : correctif cc99a13 non vérifié ; puis
   mémoire par instance), villes (STABLE simulation : fusion de main faite, compilé, tests non passés), paquet (d163d6f
   « en cours » : octets de bourrage mis à zéro, test `Swarm.Sim.Sauvegarde.Continue` ; cuisson à 881 / 10 078),
   foule (tenues), interface (film retiré ee9452d, cheveux blancs), architecture 2 (taverne chaude, vrais objets, arène,
   hameau de La Herse en pisé ocre), vie quotidienne lot (b), furtivité / bâtir / combat 2 / véhicules 4 / joueur /
   catastrophes / armées / rendu / ruche / profondeur : compilés, en attente de captures.
4. Questions restées sans réponse : agrandir le fichier d'échange de Windows (la machine ne tient que trois instances
   d'Unreal à 13-14 Go ; 70,6 Go engagés sur 73,5) ; le jeu appelé « queens domain ».
5. Outils modifiés ce soir : `run_python.ps1` prend une place « calcul » ; départs à 20 s quand plus de 32 Go sont libres.


## REPRISE du 20/11/2026 à 20 h 10 — retours du propriétaire après l'aperçu du titan

Ses retours (sept points, mot pour mot dans la mémoire `swarm-design-direction` ; règles tirées dans `Docs/AGENTS.md`,
« Règles du 20/11/2026 » ; modules dans `Docs/A_FAIRE.md`, chapitre 13). L'essentiel : le joueur choisit entre deux
factions et apparaît dans leur bataille (IA autonome, 100 contre 100, véhicules, dirigeables) ; véhicules en pièces
façon Crossout ; décor destructible ; villes par style de peuple (dont l'ultra-moderne), en relief, posées sur le
terrain ; le titan géant est GARDÉ ; petits titans, intérieur et piratage à refaire ; volants à refaire ; plus de Meshy.

Relancé : garde-fou (plein régime), compilation de main (relief + foule + architecture d) puis règles et partie
automatique. Neuf agents repris avec leur contexte, un par module : titans, infanterie (ex-foule), conflit (ex-armées),
véhicules (copie `prise`), destruction (ex-bâtir), urbanisme, architecture 2, rendu, optimisation. En pause, travail
compilé non vu dans leurs copies : furtivité, profondeur, vie quotidienne, interface, joueur, combat 2, catastrophes,
monde sauvage, relief, ruche, paquet. Les volants auront leur agent quand le titan sera dans main.

À savoir : `Config/DefaultEngine.ini` de main porte une ligne non commitée `r.VirtualTextures=True`, ajoutée le 19/11 à
22 h 35 pendant que le propriétaire jouait (sans doute une consigne d'asset des falaises Megascans) ; question posée à
l'agent rendu. L'outil m'a refusé de l'écarter : ne pas contourner.


## 20/11/2026, 21 h 40 — quota de la session presque épuisé : état pour reprendre

Le propriétaire a annoncé qu'il lui restait 10 % d'utilisation (une demi-heure). Quatorze agents arrêtés pour garder de
quoi livrer une version ; restent actifs : titans, interface (cheveux, film), joueur (clavier, course), véhicules (copie
`prise`), destruction (copie `batir`), optimisation (`perf`).

**Main** (dernier commit : voir `git log`) contient : relief, foule ba79f0d, sauvegardes (ZeroPadding), architecture d,
fin du gel du changement de jour (perf 0ae54f5 + 0ad5632). Partie automatique de 20 h 56 : aucun plantage, changement de
jour 309 ms au pire. À fusionner dès qu'ils sont posés et vus : `STABLE` du titan géant (dehors), cheveux + film retiré
(interface), clavier et course (joueur), pick-up démontable (prise), mur destructible (batir).

**Chantiers arrêtés en plein travail** (tout est sur disque dans `C:\Dev\SwarmWT\<copie>`, commits « en cours ») —
règles du propriétaire : `Docs/AGENTS.md` « Règles du 20/11/2026 », modules : `Docs/A_FAIRE.md` chapitre 13 :
- `foule` infanterie autonome (bataille 74 contre 39 ; régression : les camps ne se rencontrent plus ; soldats noirs) ;
- `armees` conflit d'ouverture (règles vertes, équilibre 86 / 14 / 0 ; choix du camp et arrivée compilés, non vus) ;
- `villes` urbanisme (STABLE simulation 0674eba NON fusionné : 3 tests rouges, équilibre 75 / 25 ; épouser le terrain) ;
- `archi` styles par peuple (tours des Fonderies, base militaire, cathédrale : compilés, non vus) ;
- `rendu` (Épique aveuglant, reflet des cheveux, guides d'assets coupés) ; `faune` chaîne de tir `ASwarmGunfire`, arsenal ;
- `atmo` herbe en brins (`M_GrassBlade`) ; `menu` écran du personnage ; `monde` (branche `personnage`) corps, tenues,
  cheveux, visages ; `relief` terrain en volume ; `modeles` volants (sauterelle écrite) ; `ruche` ; `societes`
  profondeur ; `paquet` cuisson (1 929 / 10 004) ; en pause depuis plus tôt : `dangers`, `furtif`, `humains`.
- Demandes du propriétaire pas encore commencées : chantiers de titans (1 géant et 6 titans de ville par 60 jours,
  sabotables) ; sept grands lieux à rebâtir sans Meshy.

**Machine** : la file avait un interblocage (lanceurs d'avant et d'après le changement de règle de 21 h 21) — réglé en
arrêtant les anciens lanceurs ; tout lanceur lancé après 21 h 21 suit la même règle (priorité = dix minutes d'avance ;
12 Go libres pour un lancement sans image, 17 avec ; trois compilations à la fois). Garde-fou actif (PID dans
`scratchpad\governor.pid`) : à arrêter avant de rendre la main.
