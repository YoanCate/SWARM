# Règles de travail pour un agent qui reprend un chantier de SWARM

À lire en entier avant de toucher à quoi que ce soit. Tu travailles pour le propriétaire du jeu, sous la conduite du
chantier principal (l'orchestrateur), qui fusionne ton travail. Tu es autonome : ne pose pas de question, tranche,
avance, et dis franchement dans ton rapport ce qui n'est pas fait.

## Le jeu en trois lignes

SWARM : jeu solo Unreal Engine 5.8.4, vue à la troisième personne, monde ouvert simulé. Cinq peuples vivent, se
gouvernent et se font la guerre ; l'Essaim tombe du ciel et dévore la carte ; on gagne en tuant la Reine ET en
sabotant l'Arche. État et histoire du projet : `README.md`, `Docs/JOURNAL.md` (sections « Reprise » et les dernières
« J.. »), monde et textes : `Docs/LORE.md`.

## Ce que le propriétaire a dit (il juge en joueur, à l'œil, sur les dix premières minutes)

Il a noté le jeu 4/20. Ses reproches, dans ses mots :
- « tous les personnages sont des mannequins UE ; pas de malice ni de gentillesse » ;
- « trop peu d'animations en combat ; on ne comprend pas le système d'aggro ni de combat » ;
- « les IA ne semblent pas interagir les unes avec les autres » ;
- « les villes sont organisées dégueulassement », « on dirait des lignes droites » ;
- « il manque des événements venant au joueur directement, divers et variés » ;
- « ça manque de trash » (du cru, du sale, du sanglant) ;
- « le monde destructible, un peu comme un monde voxel, est inexistant » ;
- « l'architecture de l'Arche et des cités de l'Essaim est inexistante : je voulais un truc un peu chair, cybernétique » ;
- « un monde sans aucune saveur, organisé de manière aléatoire, où tout est traité de la même manière » ;
- monde « très vide », arbres dont le feuillage ne bouge pas, ciel pas exploité, gameplay « incompréhensible ».
Sa demande du 17/11/2026 : « qualité AAA, Arc Raiders, profondeur, choix, immersivité comme Gothic 3, compréhension de
prise en main, facilité d'exécution, environnement dynamique (herbe, météo…), tous les points évoqués, voire aller
plus loin ». Les références désignent un ressenti, pas un contenu à copier.

Avant de dire qu'une chose est faite, regarde ta capture d'écran (outil Read sur le PNG) et demande-toi ce qu'il en
dirait. Si ça fait « prototype », « blocs teintés », « mannequin », recommence autrement. Un point vérifié par capture
vaut mieux que cinq écrits et jamais vus.

## Règles du cahier des charges (non négociables)

- La simulation (`Source/Swarm/Sim`) est du C++ standard pur, sans Unreal. Le jeu la lit par `Get*()` et n'y écrit que
  par des fonctions `Player*()`.
- Rien de gratuit, rien d'inventé : aucune ressource, aucune unité ne se crée hors des règles de la simulation. Ce
  qu'on montre doit venir de ce que la simulation fait vraiment (une couvée sort d'une vraie ruche, un messager est un
  des habitants présents).
- Pas de code factice, pas de test qui ne teste rien. Compte rendu honnête : écrit / compilé / testé.
- Aucun asset payant ou de licence douteuse ; uniquement ce qui est sur le disque (packs Fab du propriétaire, déjà
  copiés ou dans `C:\Dev\ASCTeuthisan\Content` et `Documents\Unreal Projects\`) ou ce que tu fabriques par script.
  Note pour `Docs/CREDITS.md` ce que tu reprends (pack, auteur, licence).
  État des réserves au 19/11/2026 (le propriétaire a demandé de libérer la place) : supprimés car jamais utilisés,
  Electric Dreams, Valley of the Ancient, City Sample Buildings, et le cache du lanceur. Restent : Medieval Village,
  Dark Ruins, Goddess Temple, Game Animation Sample (`Documents\Unreal Projects\`) ; dans `C:\Dev\ASCTeuthisan\Content` :
  Paragon (sept héros), `Scene_OldMine` et `Scene_QuarrySlate` (galeries, étais, wagonnets, carrière : pour les mines et
  les grottes), `Scene_Banquet` (intérieurs), `Scene_Bazaar_Vol1`, arbres (`BlackAlder`, `CommonHazel`,
  `StonePineForest`, `BarkPack`, épicéas), `sA_Megapack_v1` (effets), `FreeAnimsMixPack`, `MC_Sample`. Le
  propriétaire ajoute de nouveaux packs gratuits (liste : `Docs/PACKS_A_PRENDRE.html`) : ils arrivent dans
  `C:\Dev\ASCTeuthisan\Content` ; un agent ne télécharge rien lui-même.
- Textes du jeu en français, avec accents dans les chaînes affichées ; commentaires de code en français sans accents,
  dans le ton des fichiers existants (ils disent le pourquoi).

## Ton espace de travail

- Tu as UNE copie de travail git (un « worktree ») sous `C:\Dev\SwarmWT\<nom>`, sur la branche `<nom>`. Tu ne
  compiles, ne lances l'éditeur et les tests QUE sur son `Swarm.uproject`.
- INTERDIT : modifier quoi que ce soit dans `C:\Dev\Swarm` (l'arbre principal), lancer quoi que ce soit sur
  `C:\Dev\Swarm\Swarm.uproject`, pousser, fusionner vers `main`, toucher à `Docs/JOURNAL.md`, `README.md`,
  `Docs/A_TESTER.md`, `Docs/CREDITS.md`, `Docs/AGENTS.md`, `Tools/Test/*.ps1`. Tu PEUX faire `git merge main` dans ta
  copie pour récupérer le travail des autres quand on te le demande.
- Dans ta copie, les dossiers de packs sous `Content\` (tout sauf `Content\Swarm`, `Content\Maps`, `Content\Characters\Mannequins`)
  sont des JONCTIONS vers l'arbre principal : n'y supprime et n'y modifie rien d'existant, ne supprime jamais une
  jonction récursivement (`Remove-Item -Recurse` sur une jonction détruirait le contenu du propriétaire). Tes assets
  propres au jeu vont sous `Content\Swarm\...` (suivis par git LFS : ils arrivent par la fusion). Un nouveau pack
  copié (dossier ignoré par git) se crée pour de vrai dans `C:\Dev\Swarm\Content\<dossier>` puis se lie dans ta copie
  par `cmd /c mklink /J`, et sa ligne s'ajoute à `.gitignore`.
- Disque : textures 2K au plus (1K pour le petit), seulement la fermeture utile.

## Compiler, tester, capturer

```powershell
# Compiler (lis la ligne « Result: ») : une seule compilation a la fois sur toute la machine.
C:\Dev\SwarmWT\<nom>\Tools\Test\build.ps1
# Tests dans le jeu, sans fenetre ni son (captures dans Saved\Screenshots\Swarm\ de ta copie)
C:\Dev\SwarmWT\<nom>\Tools\Test\run_tests.ps1 -Filter "Swarm.<Groupe>+Swarm.<Autre>" -Name <nom> -TimeoutSeconds 1500
# Tests sans image (regles, simulation) : passent meme pendant une partie du proprietaire
C:\Dev\SwarmWT\<nom>\Tools\Test\run_tests.ps1 -Filter "Swarm.Sim" -Name sim -NoRender
# Script Python d'editeur, sans fenetre
C:\Dev\SwarmWT\<nom>\Tools\Test\run_python.ps1 -Script Tools/Editor/<script>.py
```

- JAMAIS de fenêtre ni de son sur la machine du propriétaire : toujours `-RenderOffscreen -nosound -unattended
  -nosplash` (le script de test le fait) ; `-nullrhi` pour les scripts Python.
- **La machine est celle du propriétaire.** Il navigue, il joue, et il s'est plaint quatre fois qu'on la fasse ramer
  (mesure du 18/11/2026 : 0,9 Go de mémoire libre sur 64 et carte graphique à 100 %, à cause de trois Unreal à
  10-15 Go chacun). Il a dit aussi, en partant : « tu peux utiliser mon PC à fond ». Les scripts choisissent seuls
  entre trois régimes (`Tools/Test/machine.ps1`) :
  - EN PARTIE (un de ses jeux tourne) : un seul Unreal, aucun qui dessine — `run_tests.ps1` attend la fin de sa
    partie, ou interrompt puis relance le test si elle commence ; les tests sans image passent avec `-NoRender` ;
  - PRÉSENT (clavier ou souris touchés depuis moins de trois minutes) : un seul Unreal à la fois (jeu ou script
    d'éditeur), une compilation, priorité minimale, cœurs 8 à 15, cadence bridée ;
  - ABSENT : trois Unreal, deux compilations, tous les cœurs ; mesure de cadence débridée permise (`-MaxFps 0`,
    elle tourne alors seule). Ce régime n'existe QUE si le propriétaire a dit qu'il laissait sa machine (l'orchestrateur
    lance alors le garde-fou avec `-OwnerSaidAway`) : un curseur immobile ne prouve rien, il regarde peut-être une
    série. Sans cela, tout est PRÉSENT.
  Le garde-fou écrit le régime dans `%TEMP%\SwarmLocks\regime.txt` et chaque arrêt qu'il fait dans `governor.log`.
  Pas de boucle de relance maison : un lancement = un appel au lanceur, qui attend son tour.
  Ne lance JAMAIS `UnrealEditor-Cmd.exe` ni `Build.bat` toi-même : le jeu par `run_tests.ps1`, un script Python
  d'éditeur par `run_python.ps1`, la compilation par `build.ps1`. Un garde-fou tue tout Unreal surnuméraire quand il
  est là. Lance tes tests en arrière-plan, ne relance jamais un lancement qui attend, avance sur le code pendant ce
  temps. Quand il est absent, fais passer en priorité tout ce qui a besoin de l'image.
- Les tests d'un même lancement partagent le même monde : un test qui met une cité en alerte, avance le temps ou
  termine la partie fausse les suivants. Un test doit se remettre lui-même dans l'état dont il a besoin.
- Tests : dans un fichier à toi `Source/Swarm/Tests/Swarm<Chantier>Tests.cpp`, avec `#include "Tests/SwarmGuideTestKit.h"`
  (trouver le monde et le joueur, attendre, capturer : voir `SwarmLifeTests.cpp` comme modèle récent). Un test de
  jeu = une situation mise en place, des vérifications réelles, une ou deux captures.
- Pièges connus : Git Bash transforme `/Game/...` en chemin Windows (lance les scripts depuis PowerShell) ; un heredoc
  bash contenant des apostrophes échoue (écris le script dans un fichier) ; un maillage fondu à plus de 64 matières
  n'est pas Nanite (cadence divisée par cinq, sans erreur) ; rebâtir un morceau de terrain avec sa collision d'un bloc
  fige le jeu (passer par la file `TickDig`) ; un test qui termine la partie fausse ceux qui le suivent dans le même
  lancement ; l'outil d'édition refuse d'écrire un fichier qu'un formateur vient de modifier : relis-le d'abord.

## Fichiers partagés : rester dans son couloir

Plusieurs agents modifient le même dépôt. Sur un fichier existant, n'utilise que des remplacements ciblés (outil
Edit), jamais une réécriture entière, et ne reformate rien. Reste dans les fichiers et les fonctions que ta mission
t'attribue ; si tu as besoin de beaucoup de code neuf, mets-le dans de nouveaux fichiers. Si tu dois toucher hors de
ton couloir, fais-le petit, groupé, et signale-le dans ton rapport.

## Commits et rapport

- Commits fréquents sur ta branche. Un commit dont l'état compile, passe tes tests et peut être fusionné tel quel
  commence par `STABLE : ` ; l'orchestrateur fusionne le dernier commit STABLE de ta branche quand il veut, sans te
  prévenir. Termine chaque message de commit par la ligne :
  `Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>`
- Ton rapport final est ta réponse. Court et honnête : dernier commit STABLE ; ce qui est fait et VÉRIFIÉ (méthode,
  noms des captures à regarder) ; ce qui est écrit mais pas vérifié ; ce qui n'est pas fait ; cadence mesurée s'il y
  a lieu ; Go ajoutés ; licences à inscrire ; pièges rencontrés ; ce qu'il faut savoir pour fusionner.
- Respecte l'heure limite de ta mission : à l'heure dite, tu rends ce que tu as, commité et STABLE.

## Règles du 19/11/2026 au soir (à lire en premier : elles priment sur ce qui précède)

**Ta copie de travail.** Si elle n'existe pas : `git -C C:\Dev\Swarm worktree add C:\Dev\SwarmWT\<nom> -b <nom> main`, puis, pour CHAQUE
dossier de `C:\Dev\Swarm\Content` et de `C:\Dev\Swarm\Content\Characters` absent de ta copie, une jonction
`cmd /c mklink /J <copie>\Content\<dossier> C:\Dev\Swarm\Content\<dossier>` (les packs sont hors dépôt), et une copie réelle de
`C:\Dev\Swarm\Plugins`. Ne supprime JAMAIS une jonction récursivement ; n'écrase ni ne supprime rien dans les packs ; jamais de
`git add` d'un dossier de pack ni de `git add -A` ; tout ce que tu crées va sous `Content/Swarm/`. Pour prendre main :
`C:\Dev\Swarm\Tools\Test\merge_main.ps1` lancé depuis ta copie (jamais `git merge main` à la main). `Tools/Test/*.ps1` de ta copie
sont posés par le chef de projet : ne les commite pas, ne les restaure pas.

**Lancer.** JAMAIS `UnrealEditor-Cmd.exe` ni `Build.bat` directement : `Tools\Test\build.ps1`,
`Tools\Test\run_tests.ps1 -Filter "Swarm.X" -Name <nom> -ResX 2560 -ResY 1440` (`-NoRender` pour les tests de règles : file séparée),
`Tools\Test\run_python.ps1 -Script …`. Aucune fenêtre ni son sur les écrans du propriétaire.

**La machine** (63 Go de mémoire, une vingtaine d'agents). Le propriétaire a dit « puissance max » : trois instances qui dessinent
au plus, 12 Go chacune ; un lanceur attend seul qu'il reste 17 Go libres et que 90 s aient passé depuis le départ précédent. Donc :
UN SEUL lancement à toi à la fois, groupé (plusieurs tests dans le même `-Filter`) ; lance en arrière-plan et écris pendant
l'attente ; un lancement tué par le garde-fou se relance UNE fois ; ne rends pas la main tant qu'un lancement que tu attends n'a
pas répondu. Ne fusionne ni ne compile pendant qu'un de tes lancements attend ou tourne. Si le propriétaire joue, tout s'arrête
de dessiner tout seul : attends.

**Qualité, ses mots du jour.** « On dirait de l'asset flip », « un jeu de 2002 », « Palworld tellement c'est vide », « aucun sens »,
« tout se ressemble », « trop mécanique ». Avant de dire qu'une chose est faite, REGARDE tes captures (outil Read sur le PNG) et
juge-les contre ces mots. Rien qui bouge ne se juge sur une image fixe : planche de 6 à 8 images consécutives (toutes les 0,1 à
1 s). Un modèle généré (Meshy) n'entre que s'il bouge quand il doit bouger, s'il est rhabillé aux matières du jeu et s'il tient à
l'image dans le monde. Aucun texte dans une cinématique.

**Robustesse.** Tout objet chargé (maillage, matière, animation, son, effet, texture) est tenu par un membre `UPROPERTY` ou mis à
la racine : cinq plantages aujourd'hui venaient de pointeurs nus ou de variables statiques détruits par le ramasse-miettes.
Matière sur un maillage Nanite : usage Nanite coché ; matière translucide : `bDisallowNanite`. Le journal de chaque lancement ne
doit contenir AUCUNE ligne « Default Material will be used in game » ni « Failed to compile Material ». Sous charge le temps du
monde avance moins vite que l'horloge : les tests attendent en temps de MONDE (`FWorldDelayCommand`, `SwarmGuerreTests.cpp`).
Compilation groupée : pas deux noms locaux identiques dans deux fichiers (`Cloth`, `Accent`, `Time`) ; dans les tests, appelle
`FindGameWorld` avec son espace de noms.

**Simulation** (`Source/Swarm/Sim`) : C++ standard pur, déterministe, sauvegardée ; le jeu n'y écrit que par les fonctions
`Player*()` ; rien de gratuit ni d'inventé. Si tu changes règles ou sauvegarde : monte les versions (simulation 12, jeu 14 le
19/11), relance `Tools\Headless` (`cmd /c ".\build.bat"` depuis ce dossier, puis `bin\swarm_headless.exe --seeds 100 --quiet` ;
référence : Arche 82 à 84 %, Essaim détruit 16 à 18 %, monde mort 0 %, médiane 723 jours) et les tests de règles `-NoRender`
(`Swarm.Sim+Swarm.Societes+Swarm.Cercles+Swarm.Faune.Regles+Swarm.Vehicules.Sim` : 40 verts). Plusieurs agents y touchent :
changements localisés, fusionne main souvent.

**Sauvegardes (depuis le 19/11 à 22 h : simulation 14, jeu 15).** Chaque structure de la sauvegarde porte sa taille. Un champ ajouté
à la FIN d'une structure, ou un bloc ajouté en fin de `Serialize` sous `if (s.More())`, NE MONTE PLUS la version : une sauvegarde
plus ancienne se relit avec la valeur par défaut. On ne monte la version que pour un champ qui change de sens, de type ou de
place. Tests `Swarm.Sim.Sauvegarde.Ancienne` et `.Temoins` (témoins `Tools/Saves/sim_v14_*.bin` : ne pas les supprimer).

**File d'attente de la machine.** Compilations et lancements passent dans l'ordre d'arrivée (tickets), les copies prioritaires en
tête (`priorite.txt`, `build.priorite.txt`, tenus par le chef de projet). Un lanceur lit ces règles UNE fois à son départ : si
le chef de projet annonce un changement, arrête ton lanceur en attente et relance-le. `build.ps1` appelle UnrealBuildTool
directement : plusieurs copies compilent en même temps.

**Performance.** Il joue en 2560×1440 sur RTX 5080 et dit que « le jeu lag ». Cible : fil de jeu ≤ 6 ms, rendu ≤ 8 ms, GPU ≤ 10 ms,
aucun à-coup de plus de 50 ms, mémoire vidéo sous 10 Go, monde construit en moins de 60 s. Mesure ce que tu ajoutes
(`Swarm.Perf.Budget`, `Docs/PERF.md`). Rien de lourd d'un bloc (terrain, collision, apparition de foule) : étale.

**Livrer.** Commite souvent (« en cours : … », avec la liste de tes fichiers). Un commit `STABLE : …` par lot PETIT, compilé, testé
et VU à l'image, que le chef de projet fusionne aussitôt — le propriétaire juge sur main, pas sur ta branche. Chaque nouveauté
visible a son entrée dans son menu de test (`ASwarmTestMenu::Register`, catégorie « Nouveau aujourd'hui »). Rapport final court
et honnête : écrit / compilé / testé / vu à l'image, ce qui reste laid ou faux, fichiers touchés hors de ton couloir, chemins des
captures. Pour parler à un autre agent : `SendMessage` à son identifiant ; au chef de projet : « main ».


## Règles du 20/11/2026 — retours du propriétaire après avoir joué (elles priment sur ce qui précède)

**Sa méthode, mot pour mot :** « Applique-les par ordre de priorité, puis teste chaque point toi-même avant de me
répondre. Ne me dis pas que c'est fait sans l'avoir vérifié en jeu. » ; « Travaille module par module et teste en jeu
après chaque module. » ; « À la fin, donne-moi la liste de ce qui marche, de ce qui est partiel et de ce que tu n'as pas
pu corriger. »

1. **Un `STABLE` = un module VU EN JEU.** Avant de l'écrire : une entrée du menu de test (touche T) qui y mène en un
   clic, un test automatique qui JOUE la situation (pas seulement une capture posée), une planche d'images consécutives
   et, pour tout ce qui bouge, un extrait vidéo (`smoke.ps1` montre comment en produire un). Le message de commit dit :
   ce qui marche, ce qui est partiel, ce qui n'est pas fait. Rien d'autre ne compte comme « fait ».
2. **Plus de Meshy.** « Ne pas utiliser d'assets 3D générés par Meshy. Je préfère les assets que tu crées toi-même, car
   ils sont animés et pleinement fonctionnels. » Aucune génération ; ne pose plus aucun modèle `/Game/Swarm/Meshy` ;
   ceux qui sont visibles sont à remplacer par des pièces bâties par script (`Tools/Assets/make_*.py`) et ANIMÉES par le
   code, sur le modèle du titan (`SwarmTitan.*` : pièces rigides articulées, pattes posées sur le relief). « Tous les
   assets doivent être réellement animés et fonctionnels, pas statiques. »
3. **Rien de scripté.** « Tout doit être autonome : l'IA décide seule, sans script. Pas de mécanique de jeu
   artificielle. » Une bataille, une charge de véhicule, un duel de dirigeables sortent des décisions de chaque unité
   (perception, choix de cible, couverture, ordres de son chef) et de l'état réel de la simulation — jamais d'une
   séquence écrite d'avance ni d'unités apparues pour le décor.
4. **Le titan géant ne change plus d'allure** : « Le scarabée géant actuel est sympa. Garde-le. »
5. **Cohérence du monde** : fusils, véhicules, dirigeables → plus de villes toutes médiévales. Style par peuple
   (décision du chef de projet) : Fonderies = ultra-moderne (tours de verre et d'acier, passerelles, néons, façon
   Shanghai / Shenzhen) ; Protectorat = militaire moderne (béton, bunkers, hangars à dirigeables, casernes) ; Concordat =
   religieux (cathédrales, monastères, coupoles, cloîtres) ; Affranchis = rustique et médiéval ; Clans = nomade
   (caravanes, véhicules-maisons, yourtes). Tout bâtiment épouse le terrain : terrasses, soutènements, pilotis — rien à
   moitié dans le vide ni dans la roche, y compris l'architecture de l'Essaim.
6. **Machine** : trois instances d'Unreal au plus (13-14 Go chacune). Peu d'agents à la fois, chacun groupe ses
   vérifications en UN lancement ; `run_python.ps1` et les tests `-NoRender` passent par la file « calcul ».


### Ajout du 20/11/2026 à 21 h — « pense plus loin que tout ce que tu dis et de tout ce qu'on a déjà fait »

Ses mots : « l'herbe c'est juste plat par rapport au sol » ; « faut que les combats paraissent vraiment plus dynamiques
et moins des PNJ qui courent comme des poulets sans tête » ; « pour les dirigeables, comme pour les véhicules, des
animations de réalisme : tire sur le ballon dirigeable, on voit l'air qui s'en sort » ; « la destruction, c'est pas juste
le truc qui disparaît : faut qu'il y ait des déchets, des débris, une chute réaliste comme dans War Thunder quand
t'exploses un avion » ; « pense plus loin que tout ce que tu dis et de tout ce qu'on a déjà fait ».

7. **La règle de la CAUSE VISIBLE.** Rien ne change d'état sans qu'on voie pourquoi et comment : un dégât a un point
   d'impact, une conséquence physique qui dure (fuite, feu, fumée, gîte, perte de puissance), une fin (chute, épave) et
   des restes (débris, épave fouillable, sol marqué). Jamais un objet qui disparaît, jamais une barre de vie seule.
   Exemple exigé : une balle dans l'enveloppe d'un dirigeable y fait un TROU ; le gaz s'en échappe en jet visible,
   l'enveloppe se dégonfle et se plisse de ce côté, l'appareil perd de la portance et penche, l'équipage largue du lest,
   le feu prend si le gaz s'enflamme, la toile brûle par plaques en laissant la carcasse, la nacelle tombe en tournant
   avec ses débris et sa traînée, s'écrase, reste une épave qui fume.
8. **Chaque unité a une INTENTION lisible.** À tout instant on doit pouvoir dire ce que fait un soldat et pourquoi
   (« il couvre son voisin », « il contourne par le talus », « il traîne un blessé », « il recharge à l'abri ») — sinon
   c'est un poulet sans tête. Pas de course sans but, pas d'aller-retour, pas d'hommes debout à découvert ; des gestes
   (signes du chef, épauler, s'accroupir, se jeter à terre), des cris, des rôles (tireur, servant, porteur, chef, médecin).
9. **Pense à l'étape d'après.** Avant chaque `STABLE`, écris dans le message de commit les TROIS choses qu'un joueur
   exigeant remarquera ensuite sur ton module — et fais la première sans attendre qu'il la demande.
