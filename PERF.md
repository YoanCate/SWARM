# Cadence de SWARM : ce que coûte chaque système (chantier « optimisation et atmosphère », 19/11/2026)

## Comment mesurer

Tout se mesure dans le jeu, sans fenêtre, par les tests de `Source/Swarm/Tests/SwarmPerfSystemsTests.cpp` :

| Test | Ce qu'il fait |
|---|---|
| `Swarm.Perf.Systemes` | Sept lieux (réveil devant la cité de départ, rue de la capitale, nuit en cité, forêt, terre de l'Essaim, désert, dedans d'une ruche). Dans chacun : le recensement (par système, composants dessinés, instances, corps à squelette, Niagara actifs, lumières à ombre, composants qui tickent ; maillages instanciés les plus nombreux), « tel quel » (moyenne, 99e centile et pire image du fil de jeu, du rendu et du GPU), puis chaque système caché et arrêté 3,5 s : l'écart est ce qu'il coûte. `-SwarmPerfOnly=rue,foret` pour ne garder que certains lieux ; `-SwarmPerfQuick` sans l'attribution. |
| `Swarm.Perf.Marche` | Deux minutes de marche réelle, du réveil à travers la cité de départ puis en campagne, temps du monde à x15 (un jour passe). Chaque image de plus de 50 ms (fil de jeu, rendu ou intervalle de plus de 150 ms) est notée « ACOUP » avec les cinq postes de la sonde qui l'ont faite. |
| `Swarm.Perf.Accueil` | L'ouverture (trois plans) et l'écran-titre : coût image par image ; puis la luminance de l'image au titre, aux touches, au départ de la partie, à la pause, au menu de test : échoue si un écran est noir. |
| `Swarm.Perf.Matieres` | Lit le journal du moteur après chargement ; échoue s'il contient « Default Material will be used in game » ou « Failed to compile Material » (damier gris), en les nommant. |
| `Swarm.Ciel.Nuages` | Caméra posée, tournée vers le ciel, deux minutes : décalage de l'image du ciel toutes les cinq secondes ; échoue si les nuages repartent en arrière. `-SwarmOldClouds` rend l'ancien vent (avant / après). |

```powershell
Tools\Test\run_tests.ps1 -Filter "Swarm.Perf.Systemes" -Name perf -ResX 2560 -ResY 1440 -TimeoutSeconds 1800 -Extra @('-SwarmPerfOnly=rue,foret')
```

La sonde (`Source/Swarm/Game/SwarmPerfProbe.h`) : une ligne `SWARM_PROBE("nom");` en tête du `Tick` de chaque système
compte son temps au fil de jeu ; les tests l'écrivent (« SONDE », « ACOUP », « DEMARRAGE »). Interrupteur par système,
dès le lancement : `-SwarmOff=faune,feux,foule,...` (noms : air, grottes, cercles, foule, chute, chevaux, presages,
reine, societes, pietinement, engins, vehicules, lieux, feux, faune ; s'ajoutent `-SwarmNoWildlife`, `-SwarmNoWild`,
`-SwarmNoCaves`, `-SwarmNoCity`, `-SwarmAllBodies` qui existaient).

Pour le propriétaire : menu de test (T) → Réglages → « Afficher la cadence et les millisecondes » (`stat fps` et
`stat unit` : Game = fil de jeu, Draw = fil de rendu, GPU = carte).

Les mesures des agents sont BRIDÉES (une instance, priorité minimale, cadence plafonnée, machine partagée) : comparer
les millisecondes de travail de chaque fil, pas les images par seconde. La cadence réelle se mesure débridée, seul
sur la machine (`-MaxFps 0`), sur demande du propriétaire.

## Mesures

### Lancement (temps jusqu'à la première image jouable)

Lu dans les journaux (`Saved/Logs/*.log` : « Log file open », « Starting Game », « Took … LoadMap », et la ligne
`SWARM_DEMARRAGE` écrite par `ASwarmWorldDirector::BuildWorld`).

| Étape | Propriétaire, 19/11 17 h 30 (Swarm.log) | Agent perf, 18 h 10 (`perfCPU.log`) | Agent perf, 19 h 14, après correctif (`perfB.log`) |
|---|---|---|---|
| Moteur jusqu'à « Starting Game » | **104 s**, dont 93 s à attendre UnrealBuildTool (« UBT AutoSDK ») | 30 s, dont 23 s d'UBT | **10 s** |
| Chargement de la carte (LoadMap) | 37 s | 69 s (monde construit en 63 s) | 46 s (monde en 40 s) |
| Monde construit, depuis le lancement | ~140 s | 93,8 s | **50,7 s** |

- **Cause n° 1 : la détection des kits de plateformes par UBT** que l'éditeur lancé en `-game` fait à chaque démarrage
  (`TargetPlatformManagerModule.cpp`) ; elle attend derrière les compilations des agents (18 à 93 s). Corrigé par le
  coordinateur : `set UE_SKIP_UBT_SDK_SETUP=1` dans « SWARM - Jouer.bat » et dans `run_tests.ps1` / `run_python.ps1`.
- **Cause n° 2 : Rampage et Terra (Paragon) reconstruits à chaque lancement** (« Building Skeletal Mesh … Reducing
  skeletal mesh for LOD 2 », 3,4 à 9,2 s chacun, 16 lancements sur 16) et 21 instances de matière « recompilées à chaque
  lancement jusqu'à ce qu'elles soient re-sauvées ». Script prêt : `Tools/Editor/resave_rebuilt.py` ; NON lancé (la
  session de l'agent n'a pas la permission de modifier les fichiers de packs : au propriétaire de l'accepter).
- Notre code au démarrage (sondes `demarrage.*`) : foule 45,6 s dont l'essentiel est le chargement des corps Paragon
  (les deux reconstructions ci-dessus et les attentes « Waiting for skinned assets ») — les silhouettes taillées ne
  coûtent que 15 ms chacune ; structures 12,4 s ; terrain 2,5 s ; feux 1,1 s ; faune 0,8 s ; le reste < 0,4 s.
- Écran de chargement (`SwarmLoadingScreen.cpp`) : pendant le chargement de la carte, un écran noir « S W A R M » avec
  l'étape en cours (« Les cités se bâtissent… ») et une barre ; il ne couvre pas les 10 s du moteur d'avant.

### Fil de jeu : ce que coûte le Tick de chaque système (sonde, sans image, 18 h 10)

| Lieu | foule | monde (directeur) | faune | air | feux | autres |
|---|---|---|---|---|---|---|
| réveil devant la cité | 0,40 ms | 0,08 | 0,07 | 0,06 | 0,02 | < 0,02 chacun |
| rue de la capitale | 1,19 | 0,08 | 0,04 | 0,05 | 0,02 | < 0,02 |
| nuit en cité | 1,39 | 0,08 | 0,05 | 0,04 | 0,02 | < 0,02 |
| en forêt | 0,17 | 0,08 | 0,05 | 0,07 | 0,02 | < 0,02 |
| désert | 0,02 | 0,08 | 0,03 | 0,06 | 0,03 | < 0,02 |
| dedans d'une ruche | 0,11 | 0,42 (dont creuser 0,34) | 0,03 | 0,07 | 0,02 | engins 0,13, chevaux 0,10 |

Conclusion : le code par image des onze chantiers du jour coûte peu (2 ms au plus, ensemble). Ce qui pèse est ailleurs :
le moteur (composants à squelette animés, composants qui tickent), le rendu et le GPU — voir le recensement.

### Recensement (ce que chaque système met devant le moteur)

| Lieu | corps à squelette affichés (ombres) | composants de la foule qui tickent | composants d'herbe dessinés (instances) |
|---|---|---|---|
| réveil | 26 (5) | 237 | 1 026 (35 019) |
| rue de la capitale | 174 (66) | 237 | 1 046 (2 289) |
| nuit en cité | 197 (75) | 265 | 1 055 (3 194) |
| forêt | 47 (17) | 265 | 1 954 (24 848) |
| désert | 13 (5) | 265 | 2 242 (11 949) |

Partout : structures 246 composants, **818 949 instances** (épicéas ~92 000 par modèle, souches ~35 000, `SM_Cube`
38 695) ; grottes 32 567 pièces et 275 lumières (éteintes loin) ; 261 à 345 composants à squelette dans le monde.
- Les ~260 composants de la foule qui tickaient même là où aucun corps n'est affiché (désert : 0 corps de la foule à
  l'écran) étaient les corps rangés dans la réserve et leurs vêtements : corrigé (`ASwarmCrowd::SetBodyTicking`), mesure
  avant / après dans `Swarm.Perf.Systemes` (« AVANT : corps rangés qui tiquent »).
- L'herbe : une dalle de 16 m porte un composant par sorte de plante (jusqu'à 2 242 composants pour 12 000 touffes au
  désert) : coût au fil de rendu à mesurer avec image ; non modifié.

### Mesure débridée, jeu réuni (19/11, 19 h 35, 2560×1440, réglage par défaut DLSS Qualité, `perfA.log`)

Seule instance qui dessinait sur la machine (fenêtre de mesure du coordinateur) ; tournaient en même temps des
compilations et des tests sans image d'autres agents. Moyenne, et entre parenthèses le pire sur 5 s. Le fil de rendu
n'est pas lu par ce test (`GRenderThreadTime` reste à 0 dans ce mode) : seuls le fil de jeu et le GPU sont fiables.
Bruit de la mesure (« tel quel » refait en fin de lieu) : ±1 ms au fil de jeu, ±0,5 ms au GPU (plus la nuit).

| Lieu | fil de jeu | GPU | corps affichés (ombres) | mémoire vidéo |
|---|---|---|---|---|
| réveil devant la cité | 5,2 ms (11,7) | 6,8 ms (7,8) | 68 (61) | 7,2 Go |
| rue de la capitale | **8,7** (15,7) | 5,7 (6,3) | 218 (114) | 7,3 Go |
| nuit en cité | **10,4** (34,9) | 7,1 (8,1) | 242 (125) | 7,3 Go |
| en forêt | 5,6 (13,6) | 7,1 (7,8) | 81 (65) | 6,6 Go |
| désert (sécheresse) | 5,4 (20,3) | 6,5 (15,6) | 13 (5) | 6,4 Go |
| dedans d'une ruche | 5,7 (14,4) | 9,5 (10,3) | — | 6,5 Go |
| écran-titre | 4,7 (10,7) | 7,4 (8,4) | — | 7,1 Go |
| ouverture (3 plans, ancienne) | 5,6 (30,0) | 6,7 (10,3) | — | — |

Objectifs (jeu ≤ 6, GPU ≤ 10, mémoire vidéo < 10 Go) : GPU et mémoire vidéo tenus partout ; fil de jeu tenu sauf en
cité (rue 8,7, nuit 10,4) à cause de la foule.

**Ce que coûte chaque système** (écart « sans le système » − « tel quel », au-delà du bruit seulement) :
- **foule** : rue −3,8 ms de fil de jeu (8,7 → 4,9), nuit −3,2 ; son propre Tick n'en fait que 1 à 2 (sonde) : le reste
  est l'animation et la mise à jour des 218 à 242 corps à squelette affichés (et 114 à 125 ombres) par le moteur.
- **ciel** (atmosphère, nuages volumétriques, brouillard) : −2,0 ms de GPU partout (6,8 → 4,9 au réveil).
- **structures** (cités, ruines, ruches) : −0,5 à −1 ms de GPU.
- tous les autres (faune, feux, herbe, herbe couchée, lieux sauvages, grottes, cercles, sociétés, véhicules et chevaux,
  reine, présages et chute, air, effets, accueil) : sous le bruit de la mesure, en moyenne.
- réserve de corps qui ne tique plus (`SetBodyTicking`) : écart sous le bruit (−0 à −1 ms) ; gardé, sans gain prouvé.

**À-coups en marchant** (`Swarm.Perf.Marche`, 120 s, temps du monde ×15 pour qu'un jour passe ; images > 50 ms) :
| Instant | Durée | Poste (sonde) |
|---|---|---|
| changement de jour | **11 781 ms** (image figée 11,9 s) | `monde.rebatir` 11 362 ms : `Terrain->Dig` + `Structures->Refresh` |
| 46,9 s et 91,2 s | 704 et 400 ms | `chute` (Tick de `ASwarmFallShow`) |
| 71,3 s | 295 ms (intervalle 712) | `vehicules` 691 ms (Tick de `ASwarmVehicles`) |
| départ | 428 ms | (premières images après le saut) |
Moyenne de la marche hors à-coups : jeu 10,7 ms, GPU 7,8 ms.

Le rebâtissage d'un bloc est LE défaut le plus grave : `ASwarmStructures::Refresh` vide puis refait toutes les
instances de toutes les cités, ruines et ruches dès que l'état visuel du monde change (`VisualStateHash`), c'est-à-dire
à chaque jour (un jour = 20 min) et à la chute de l'Essaim. Correctif à faire par le propriétaire du système : rebâtir
cité par cité (seules celles dont l'état a changé), étalé sur plusieurs images. Sondes `monde.rebatir.creuser` et
`monde.rebatir.structures` ajoutées pour départager `Dig` et `Refresh`.

**Écrans noirs des menus** (`Swarm.Perf.Accueil`, luminance des captures) : titre 31, jouer 35, création validée 29,
pause 29, touches depuis la pause 29, menu de test 36 (sur 255) ; **« Touches » ouvert depuis l'écran-titre : 2,8**,
ni texte ni monde (capture `PERF_accueil_touches_depuis_le_titre.png`) — signalé à l'agent interface qui refait le menu.

### Ciel, blé, chute (20 h 15, 2560×1440 débridé, `perfC.log`)

- **Exposition** (`Swarm.Ciel.Exposition`, devant la cité de départ) : midi 0,1 % de l'image brûlée, aube 0 %, crépuscule
  luminance 34 (3,8 % bouché). La capture « nuit » n'était pas la nuit (heure imposée pas encore basculée au moment de la
  capture) : **nuit non vérifiée**. Réglages changés sans preuve à l'image : lune ×2 (bleutée), lumière du ciel la nuit
  ×1,75, exposition minimale -2,5 au lieu de -1,5, adaptation en sortie d'intérieur 3 au lieu de 2.
- **Nuages** (`Swarm.Ciel.Nuages`, caméra posée, beau temps) : les images changent (écart moyen 5/255 sur 8 s, 13/255 sur
  40 s), aucun recul détecté, mais aucune translation globale non plus : le test ne PROUVE pas encore le défilement
  continu (le recalage cherche ±8 vignettes de 13 px ; le mouvement est plus lent ou plus déformant que cela). Planche :
  `NUAGES_planche_00..09.png` (1 s d'écart), `NUAGES_avant_*` (ancien vent), `NUAGES_apres_*` (nouveau).
- **Blé** (`Swarm.Perf.Ble`, planche `BLE_planche_0..7.png`, une image par seconde en marchant) : plus de « bouillie »,
  les épis restent nets autour du joueur ; le sillon couché ne se lit pas nettement sur les images : **couchage non
  prouvé**.
- **Chute de l'Essaim** (`Swarm.Perf.Chute`, 45 s) : jeu 7,7 ms (p99 16,0), GPU 7,2 ms (pire 11) ; à-coups : 1 138 ms au
  déclenchement, **601 ms à l'impact (Tick de `ASwarmFallShow`, premières apparitions des effets)**. Deuxième chute avec
  ramasse-miettes forcé chaque seconde : aucun plantage. Repris par l'agent « catastrophes » (préchauffage des effets,
  rebâtissage du jour de la chute différé).
- Le garde-fou Nanite balayait tous les composants toutes les 2 s (10 à 11 ms par balayage, vu dans cette mesure) :
  découpé en tranches de 8 000 objets par image (compilé, **non remesuré**).

### Correctifs du 19/11 au soir (reprise du chantier)

**1. Le gel du changement de jour** (11 781 ms d'image figée à chaque jour et à la chute de l'Essaim).
- Cause trouvée en lisant le moteur : les composantes d'instances des cités pouvaient « compter » pour la navigation ;
  chaque `AddInstance` invalidait les bornes de navigation de toute sa composante et les recalculait sur TOUTES ses
  instances (`UInstancedStaticMeshComponent::PartialNavigationUpdate` → `GetNavigationBounds`) : le rebâtissage
  croissait comme le carré du nombre de pièces. Personne ne se sert d'un maillage de navigation dans SWARM : toutes les
  composantes d'instances de `ASwarmStructures` sont maintenant hors navigation.
- Et le rebâtissage est devenu INCRÉMENTAL (`Game/SwarmStructuresRebuild.cpp`) : une empreinte par lieu (cité ou ruine,
  ruche ou Arche, lieu caché, chantier, « l'Essaim au travail ») ; seul un lieu changé est retiré (ses instances, rang
  par rang, par échange avec la dernière : rien ne glisse ; ses salles, rondes, caches, bâtiments, enceintes, dedans de
  ruche, fumerolles, lumières, couverts, compteurs) puis rebâti ; le jeu le fait par tranches de 2 ms par image (un lieu
  au moins), les lieux proches de la caméra d'abord, rien pendant la chute. `-SwarmFullRebuild` rend l'ancien chemin.
- Effet de bord corrigé : `Hidden` (les caches des cités) n'était jamais vidé et se dupliquait à chaque jour.
- Quand on étale (le jeu), une pièce qui arrête le pas est posée à échelle nulle — le moteur ne lui crée pas de corps
  physique — puis prend sa vraie taille aux images suivantes, 2 ms par image ; l'ancien tracé reste visible et solide
  jusque-là, puis part. Banc (`Swarm.Perf.Rebatir`, 3 000 cubes) : 22,5 ms avec collision, 0,5 ms sans : le corps
  physique fait 97 % du coût d'une instance.
- Menu de test → Réglages → « Rebâtir toutes les cités, sans gel » (tout se rebâtit, un lieu par image).

**Rebâtissage, avant / après** (20/11, 2560×1440, bridé, machine partagée ; le second lancement sous `-llm` et sous
forte charge : le banc y est 3 fois plus lent)

| Mesure | Avant (19/11) | Lieu par lieu, d'un coup | Étalé (le jeu) |
|---|---|---|---|
| Tout rebâtir (453 lieux) | 11 362 ms d'un bloc | 1 173 ms (2 226 sous charge) ; pire lieu 78 ms | aucune image figée ; pire part du rebâtissage dans une image 32,9 ms (sous charge) |
| Changement de jour pendant `Swarm.Perf.Marche` | image figée 11 781 ms | 168 ms (une cité : poser 60, retirer 11) | **14 ms** au plus, 2 ms d'ordinaire |
| État du monde après tout rebâtir | — | identique (808 012 instances, 744 maisons, 26 700 formes, 24 salles, 81 rondes, 52 caches, 868 bâtiments, 20 enceintes, 412 chantiers) | identique |

Les autres images de plus de 50 ms de la marche ne viennent plus du rebâtissage : foule 35-73 ms (corps qui
apparaissent en cité), chute de l'Essaim 440-457 ms (premières apparitions de ses effets : préchauffage de l'agent
catastrophes, pas encore dans main), première image après le saut 519-791 ms. Véhicules : 1 102 ms → 36 ms au plus
(le premier cheval : son corps, ses robes et ses allures sont maintenant chargés à l'écran de chargement).

**2. Préchauffage** (`Game/SwarmPrewarm.cpp`, appelé pendant l'écran de chargement par `BuildWorld`) : les effets de la
chute, leurs matières et leurs éclats, toutes les pièces de tous les véhicules (et ce qu'ils chargent à l'assemblage)
sont chargés et TENUS ; chaque effet de la chute joue une fois sous terre. Les véhicules n'apparaissent plus que deux
par passage de 0,4 s (`ASwarmVehicles::Refresh`). L'agent catastrophes préchauffe aussi ses effets à la première image
(impact : 601 → ~21 ms, sa mesure).

**3. Foule** : ombre des corps à moins de 25 m (au lieu de 40) ; pose calculée moins souvent à mesure qu'un corps
rapetisse à l'écran, images sautées interpolées (`Game/SwarmCrowdPerf.cpp`, seuils 0,5 / 0,2 / 0,08 : chaque image,
une sur deux, une sur trois, une sur quatre ; hors champ une sur huit).

### Ce qui reste lent, et pourquoi

1. ~~`ASwarmStructures::Refresh` : 11,4 s d'un bloc à chaque changement de jour~~ : corrigé le 20/11 (ci-dessus).
2. La foule en cité : 3,2 à 3,8 ms de fil de jeu (animation de 218 à 242 corps, 114 à 125 ombres) : rue 8,7 ms, nuit
   10,4 ms pour un objectif de 6. Leviers : moins de corps complets et ombres de corps seulement de près (table des
   niveaux de l'agent rendu).
3. Pics de 400 à 700 ms dans les Tick de `ASwarmFallShow` et `ASwarmVehicles` (création d'acteurs, premières
   apparitions d'effets) : préchauffer au chargement.
4. Lancement : Rampage et Terra reconstruits à chaque fois (7 à 14 s) ; `Tools/Editor/resave_rebuilt.py` attend
   l'accord du propriétaire.
