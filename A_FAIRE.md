# SWARM — ce qui manque, point par point

Liste unique, tenue par le chef de projet. Écrite le 19/11/2026 à 20 h après deux tests du propriétaire.
Références qu'il a données : Gothic 3, Red Dead Redemption 2, GTA V, Baldur's Gate 3, Arc Raiders, Kenshi, ARK,
7 Days to Die, Skyrim (boussole), Halo (vaisseaux Covenant), Star Wars (marcheurs).

**Le cap** : `Docs/TRANCHE.md` — une première heure de jeu indiscutable, dans une région, avant d'étendre. Tout ce qui sert
cette heure passe devant.

**États** : `rien` (pas commencé) · `écrit` (code écrit ou compilé, jamais vu) · `vu` (vu à l'image par un agent, pas dans
main) · `main` (dans la version que lance le propriétaire) · `refusé` (livré, jugé mauvais par lui).
Une ligne ne passe à `main` que vue EN MOUVEMENT par le chef de projet.

Les lignes marquées ★ sont celles qu'il n'a pas demandées et que j'ajoute parce que ses références les ont.

---

## 1. Les premières minutes

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 1.1 | Le jeu se lance par l'éditeur (`UnrealEditor -game`) : une à deux minutes, shaders compilés en jouant | ★ Une version empaquetée (exe) : lancement en moins de 30 s, shaders précompilés, plus rapide en jeu | à lancer | rien |
| 1.2 | « Générique » imposé avant le menu, qui rame | Titre immédiat sur un fond léger | interface | main (20 h 45, vu en image fixe) |
| 1.3 | Menus « AI slop HTML », écrans noirs | Habillage sobre et animé, un seul style partout (Arc Raiders) | interface | écrit |
| 1.4 | Création du personnage : pièces qui se superposent, cheveux qui ne tiennent pas, peu de détail | Kenshi / BG3 : visage, corps, taille, cicatrices, tenue ; tout suit le squelette | interface | écrit |
| 1.5 | Entrée en jeu sans intérêt | SON IDÉE : seul dans le désert avec une voiture, à nous d'explorer ; le monde se présente par ce qu'on voit (titan au loin, tornade, convoi, ruine) | combat | rien |
| 1.6 | Film d'ouverture | Refusé deux fois : sans texte, chaque plan cause du suivant, siège des murailles, titan animé qui écrase une bataille | à refaire après 5.x | refusé |
| 1.7 | ★ Aucun apprentissage : le joueur ne sait pas quoi faire | Apprendre en faisant (première heure guidée par des situations, pas par du texte), journal clair | combat + joueur | rien |

## 2. Le corps du joueur

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 2.1 | « On joue un 34 tonnes » : pas d'animation, pas d'inertie juste | RDR2 / Arc Raiders : départs, arrêts, pivots, penché en virage, sprint, accroupi, saut, chute, roulade | joueur | rien |
| 2.2 | ★ Pas d'escalade ni de nage ni de franchissement | Grimper un rebord, une échelle, nager, plonger | joueur | rien |
| 2.3 | Réticule « Minecraft » | Réticule qui respire avec la dispersion, marqueur de touche, rien quand on ne vise pas | joueur | rien |
| 2.4 | Tir « Nerf », sons à refaire | Recul, sons en couches, impacts par matière, cible qui réagit | combat | écrit |
| 2.5 | Explosions sans effet, décor indestructible | Explosions qui soufflent, décor léger qui casse | combat + bâtir | rien |
| 2.6 | ★ Mêlée, furtivité, soins sommaires | Coups à élan et parade, s'accroupir dans l'herbe, bandages animés | joueur | rien |
| 2.7 | Personnages sans détail | Tenue en couches, usure, équipement visible sur le corps (arme au dos, sac, gourde) | interface + foule | écrit |

## 3. Le monde physique

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 3.1 | Montagnes plates, « jeu de 2002 », paysage « nul » | Crêtes, falaises, éboulis, matière par la pente, lointains étagés | relief | écrit |
| 3.2 | Ciel qui avance et recule, éblouissement, nuit trop noire | Vent continu, exposition bornée, nuit lisible au clair de lune | optimisation | main (nuages) / écrit (nuit, exposition) |
| 3.3 | Herbe et blé qui font « des zigouigouis » | Se couchent sous le pas et se relèvent | optimisation | écrit |
| 3.4 | Pas de catastrophes | Tornades, tempêtes de sable, orages, crues, séismes qui changent le terrain et font fuir tout le monde | catastrophes | rien |
| 3.5 | La chute de la météorite fait ramer jusqu'au plantage | Courte cinématique en jeu, puis retour à la vue du joueur ; aucun à-coup | catastrophes + optimisation | rien |
| 3.6 | Sol sans vie, nature pas « pensée » | Gothic 3 : sous-bois, clairières, gués, lieux qui ont une raison | monde sauvage | vu |
| 3.7 | Pas de très grandes structures à explorer | Une douzaine de repères géants avec un intérieur et une récompense | monde sauvage | écrit |
| 3.8 | ★ L'eau : pas de vraie rivière, de lac, de côte, de pluie qui mouille | Cours d'eau qui coulent, gués, ponts, littoral, flaques | relief | rien |
| 3.9 | ★ Le son du monde : ambiance par biome et par heure, vent, insectes, écho | RDR2 : on sait où l'on est les yeux fermés | à lancer | rien |

## 4. La faune

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 4.1 | Animaux « synchronisés comme des horloges », mécaniques | Chacun son rythme, comportements par espèce | combat | rien |
| 4.2 | Une biche apparaissait sur le loup tué | Rien n'apparaît près du joueur | combat | main |
| 4.3 | Pas de monstres géants hostiles à tous | Mégafaune rare qui attaque humains ET Essaim, qu'on évite ou qu'on chasse à plusieurs | catastrophes | rien |
| 4.4 | ★ Chasse et dépeçage sans suite | Peaux, viande, trophées vendus, cuisinés, portés | bâtir | rien |

## 5. L'Essaim

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 5.1 | « Aucune logique », peu présent, pas impressionnant | Colonnes visibles de loin, avant-postes, terres prises reconnaissables | Essaim | rien |
| 5.2 | Bêtes « connes », à reculons, sans hiérarchie | Cinq castes lisibles, qui voient, alertent, encerclent | Essaim | rien |
| 5.3 | Ruches impraticables, sans collision, rien de texturé | On y entre, on y circule, tout est modélisé | Essaim | rien |
| 5.4 | On ne voit pas l'Essaim exploiter le sol | Troupes de tailles différentes qui creusent, déforment, extraient, transportent | Essaim + bâtir | rien |
| 5.5 | Scarabée « immonde », immobile | Titan de la taille d'un bourg, animé, qui erre et détruit | titans | rien |
| 5.6 | — | Se battre sous ses pattes (écrasement précis), points faibles | titans | rien |
| 5.7 | — | L'aborder, intérieur praticable, saboter, pirater, conduire | titans | rien |
| 5.8 | Pas de vaisseaux-mères | Vaisseaux de guerre organiques dans le ciel (Halo) | titans | rien |
| 5.9 | Volants indestructibles et passifs | Ils attaquent, on les abat | titans | rien |
| 5.9b | Rapport de forces titan / lieu | Un titan écrase un hameau, un village, un bourg ; une cité lui résiste un temps ; une CAPITALE tient face à un seul — il en faut deux ou trois avec une armée de l'Essaim pour percer ses murailles | titans + urbanisme | rien |
| 5.9c | Les villes ne réagissent pas à un siège | La ville se REFERME (portes, herses, faubourg évacué) et ses défenseurs se placent face aux positions de l'envahisseur, en suivant ses mouvements | foule + armées + architecture | rien |
| 5.10 | La ville brûle sans assaillant | Siège : grosses araignées aux murailles, défenseurs sur le rempart | Essaim + foule | rien |
| 5.11 | Bétail humain illisible | Enclos gardé, captifs à délivrer | Essaim | rien |
| 5.12 | ★ La menace ne monte pas en actes sensibles | Trois temps : rumeurs → fronts → fin du monde, que le joueur ressent sans lire | Essaim | rien |

## 6. Les lieux habités

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 6.1 | Toutes les villes de la même taille | Six rangs, de la ferme à la capitale à plusieurs enceintes | urbanisme | écrit (règles) |
| 6.2 | Même organisation partout | Un site et un geste par lieu (Gothic 3) | urbanisme | rien |
| 6.3 | Même architecture, façades laides | Cinq styles de peuple | architecture | écrit (interface) |
| 6.4 | Rien de visitable | On entre partout, meublé selon la fonction | architecture | rien |
| 6.5 | « Dans Gothic tout était pensé » | Forgeron, armurier, maître d'armes, arène, alchimiste, taverne avec chambres, temple, marché : chacun un lieu, une personne, un service | architecture + vie | rien |
| 6.6 | Murailles inhabitées | Chemin de ronde, tours, relèves | architecture + vie | rien |
| 6.7 | Villes vides, PNJ sans routine | Chaque habitant : un nom, un lit, un métier, une journée (RDR2) | vie + foule | écrit |
| 6.8 | PNJ tous de la même taille, vêtements qui traversent, armes mal tenues | Tailles et carrures, tenues ajustées, armes en main | foule | rien |
| 6.9 | Gens plantés devant le danger | Fuite, panique, combat en mouvement | foule | rien |
| 6.10 | ★ Pas d'événements de rue | Rixe, vol, prêche, exécution, arrivée de convoi, rumeurs qu'on entend | vie + sociétés | rien |

## 7. Les sociétés et le joueur dedans

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 7.1 | Esclavage, geôle, justice | Convois de captifs, marché, pendaisons | sociétés | main (règles) / écrit (image) |
| 7.2 | Cercles cachés (sectes, nomades, puissances) | Huit repaires, chefs en personne, conditions d'entrée | sociétés | main (repaires) |
| 7.3 | ★ On ne REJOINT rien | Gothic : entrer dans un camp, monter en grade, épreuves, tenue du rang, portes qui s'ouvrent | sociétés | rien |
| 7.4 | ★ On n'APPREND de personne | Gothic : des maîtres qui enseignent contre paiement ou service | vie + sociétés | rien |
| 7.5 | Dialogues moins bien que BG3 | Mise en scène, visages, choix aux conséquences visibles, mémoire | interface | écrit |
| 7.6 | ★ Pas de compagnons | BG3 / Kenshi : recruter, équiper, donner des ordres, ils ont un avis | à lancer | rien |
| 7.7 | ★ Pas de quêtes au sens propre | Contrats, rumeurs et histoires nés de la simulation, avec plusieurs issues | à lancer | rien |
| 7.8 | ★ Crime sans suite | GTA / RDR2 : témoins, prime, chasseurs de primes, se rendre ou payer | sociétés | main (partiel) |
| 7.8b | Pas d'assassinat ni de vraie furtivité | Assassin's Creed : cônes de vision et ouïe lisibles, approche, élimination silencieuse, corps à cacher ; un corps trouvé alerte la ville ; s'en sortir par la PERSUASION (Gothic), avec un nombre limité de fois | furtivité | rien |
| 7.9 | ★ Économie invisible | Prix qui bougent avec la guerre et la famine, commerce entre lieux | sociétés | rien |

## 8. La guerre hors des murs

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 8.1 | Pas de présence militaire en maraude | Patrouilles, escouades de blindés, postes, convois escortés | armées | rien |
| 8.2 | Pas d'hélicoptères ni d'avions | Aéronefs qui patrouillent et combattent, pilotables | véhicules + armées | écrit (autogire, dirigeable) |
| 8.3 | Batailles peu visibles | Formations, bannières à emblème, batailles rangées qu'on trouve | foule | main (bannières) / écrit |
| 8.4 | ★ Pas de front lisible | La carte et le paysage montrent qui tient quoi : fumées, ruines, postes | armées | rien |

## 9. Les véhicules

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 9.1 | Char sans chevaux | Attelage réel | véhicules | main (règles) / écrit (image) |
| 9.2 | Tourelles qui ne tirent pas | Passer à la tourelle, tirer | véhicules | main (tire ; flamme et tireur pas encore visibles) |
| 9.3 | Engins « jouets » | Proportions, matières usées, pièces qui bougent | véhicules | écrit |
| 9.4 | ★ Rien à entretenir ni à améliorer | Mad Max : carburant, pannes, blindage, armes montées, garage | véhicules | rien |

## 10. Bâtir et récolter

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 10.1 | Pas de base | ARK / 7 Days to Die : fondations, murs, portes, toits, meubles, établis, pièges, grande variété | bâtir | rien |
| 10.2 | Pas de minage ni de creusement par le joueur | Filons visibles, pioche, galeries | bâtir | rien |
| 10.3 | Pas de coupe de bois | Arbres qu'on abat et débite | bâtir | rien |
| 10.4 | ★ Pas d'artisanat | Recettes, établis, cuisine, munitions | bâtir | rien |
| 10.5 | ★ La base n'est pas menacée | Raids de l'Essaim et de pillards sur la base, à défendre | bâtir + Essaim | rien |

## 11. L'interface

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 11.1 | Pas de boussole | Skyrim : repères et événements proches en haut de l'écran | joueur | rien |
| 11.2 | Carte trop écrite | Légende, filtres, rumeurs à découvrir | interface | écrit |
| 11.3 | Inventaire | Refait, icônes des vrais objets | interface | main |

## 12. La technique

| # | Ce qui manque | Cible | Qui | État |
|---|---|---|---|---|
| 12.1 | « Le jeu lag » : CAUSE TROUVÉE — image figée 11,8 s à chaque changement de jour (`ASwarmStructures::Refresh` refait toutes les cités d'un bloc) ; pics de 0,4 à 1,1 s à la chute et à l'apparition de véhicules ; foule en cité 8,7 à 10,4 ms | Rebâtissage incrémental, préchauffage, foule à 6 ms | optimisation 2 | mesuré, pas corrigé |
| 12.2 | Plantages (cinq aujourd'hui, même famille) | Chasse aux pointeurs nus + test au ramasse-miettes forcé | optimisation + foule | main (quatre) / écrit |
| 12.2b | « On sent à peine la différence entre Bas et Épique » — CAUSE TROUVÉE : le film d'ouverture verrouillait les réglages (chaque changement ensuite ignoré par le moteur) ; textures plafonnées à 2K pour tous ; Épique du moteur abaissé | Quatre niveaux réellement différents ; Épique : qualité visuelle et atmosphérique presque photoréaliste (éclairage global, ombres, reflets, brume volumétrique, végétation dense, textures pleines) ; Bas : fluide sur une petite machine | rendu | rien |
| 12.3 | ★ Personne ne joue le jeu réuni avant lui | Chaque soir : une partie automatique de dix minutes sur main, journal lu, extrait vidéo regardé | chef de projet | rien |
| 12.4 | ★ Sauvegardes cassées à chaque fusion | Migration des anciennes sauvegardes | à lancer | rien |

---

## Ordre de bataille (ce que je pousse d'abord)

1. Ce qu'il subit dès la première minute : 1.2, 3.2, 3.3, 2.1, 2.3, 2.4, 1.4, 12.1.
2. Ce qui rend le monde « vide » : 6.1 à 6.9, 3.1, 5.1 à 5.3.
3. Ce qui fait dire « wow » : 5.5 à 5.9, 3.4, 3.5, 2.5, 8.1.
4. Ce qui fait rester : 1.5, 7.3 à 7.7, 10.1 à 10.5.
5. Puis seulement le film (1.6), tourné sur ce jeu-là.


## 13. Retours du 20/11/2026 (ordre de priorité du propriétaire) — ce chapitre passe avant les autres

État : rien / écrit / compilé / vu en jeu / dans main. « Vu en jeu » = entrée du menu T + test qui joue + vidéo.

| n° | Module | Cible | Qui | État |
|---|---|---|---|---|
| 13.1a | Choix du camp | Au départ : deux factions en guerre (une vraie guerre de la simulation), on choisit son camp | conflit | rien |
| 13.1b | Apparition dans la bataille | Le joueur arrive au milieu d'un affrontement déjà en cours entre les deux armées | conflit | rien |
| 13.1c | Fantassins autonomes | Perception, cible, couverture, escouades et chefs ; tirs réels ; 100 par camp (« grand conflit ») à cadence tenue | infanterie | écrit en partie (foule) |
| 13.1d | Véhicules autonomes | Foncent dans les fantassins, tournent autour des groupes en tirant à la tourelle | véhicules | rien |
| 13.1e | Dirigeables de guerre | Deux flottes qui s'affrontent dans le ciel, autonomes, destructibles | véhicules | rien |
| 13.2a | Véhicule en pièces | Chaque pièce (roue, blindage, tourelle, moteur, cabine) a sa vie, se détruit, se détache | véhicules | rien |
| 13.2b | Physique selon l'état | Masse, centre de gravité, vitesse, maniabilité recalculés à chaque pièce perdue | véhicules | rien |
| 13.2c | Collisions à dégâts localisés | Véhicule contre véhicule, mur, fantassin : dégâts au point d'impact | véhicules | rien |
| 13.3a | Murs | Bosses et impacts, puis brèche ; briques et ruines visibles, pas un trou propre | destruction | rien |
| 13.3b | Arbres et sol | Arbres qui brûlent et se rasent ; explosions qui creusent | destruction | écrit en partie |
| 13.3c | Bâtiments et villes | Destruction par morceaux, effondrements cohérents | destruction | rien |
| 13.4a | Styles par peuple | Ultra-moderne, militaire, religieux, rustique, nomade (voir AGENTS.md, règles du 20/11) | architecture | rien |
| 13.4b | Relief des villes | Niveaux, hauteur, quartiers distincts | urbanisme | rien |
| 13.4c | Épouser le terrain | Rien dans le vide ni dans la roche, Essaim compris | urbanisme + ruche | rien |
| 13.4d | Mines | Corrigées, bien placées, intégrées au terrain | urbanisme | rien |
| 13.5a | Titan géant | Gardé tel quel ; à fusionner dans main | titans | vu par le propriétaire (aperçu) |
| 13.5b | Petits titans | Variantes à hauteur de ville | titans | rien |
| 13.5c | Intérieur du géant | Praticable, collisions, lisible | titans | à refaire |
| 13.5d | Piratage | Clair et visible | titans | à refaire |
| 13.6a | Gros volants | Attaques crédibles, animations de déplacement et de mort | volants | à refaire en partie |
| 13.6b | Petits volants | Animations, hitbox, comportement d'essaim | volants | à refaire |
| 13.7a | Rendu | Au niveau voulu ; Bas et Épique distincts | rendu | en cours |
| 13.7b | Plus de Meshy | Retirer les modèles Meshy de ce que le joueur voit ; pièces bâties et animées par le code | tous | rien |
| 13.8 | Gel au changement de jour, mémoire par instance | Plus de gel ; moins de 9 Go par instance | perf | compilé, non vérifié |
| 13.3d | Relief destructible | Crête entaillée, pan de montagne qui s'éboule, sommet qui saute ; sauvegardé (demande du 20/11 à 21 h 10 : « tout est destructible à 100 % ») | destruction | rien |
| 13.3e | Tout ce qui est posé | Rochers, falaises, ponts, remparts, étals, architecture de l'Essaim : plus rien d'indestructible | destruction | rien |
| 13.3f | Le feu | Maison qui brûle pièce par pièce, propagation au vent | destruction | rien |
| 13.9 | Corps sous la violence | Projection, membres arrachés par les chocs très violents (rare), réglage de violence | combat + infanterie + véhicules | rien |
| 13.10 | Écran du personnage | Personnage 3D au centre, équipement autour, sac, compétences, situation — un seul écran clair, « parfaitement fait » | écran du personnage | rien |
| 13.11 | Herbe et sol | Du volume, plus de « plat par rapport au sol » | sol et herbe | rien |
| 13.12 | Joueur | Clavier sans effet au réveil, course, poids, caméra, visée épaulée, réticule, boussole | joueur | repris |
| 13.13 | Chaîne de tir | Une seule pour joueur, fantassins, tourelles, dirigeables ; impacts, sons, explosions | combat | repris |
| 13.7c | Grands lieux a rebatir sans Meshy | Colosse mort, paquebot, barrage fissure, viaduc, radio-telescope, statue brisee, usine-cathedrale : retires de la pose (lot sol), a rebatir piece par piece par script et animes | a attribuer | rien |

### Retours du 21/11/2026 (apres la version du 20/11 a 22 h 08, titan compris)

| n | Module | Cible | Qui | Etat |
|---|---|---|---|---|
| 13.14 | Progression dans le temps | Apres +30 jours, titans, vaisseaux, armees, convois, fronts ont visiblement bouge (positions, traces sur la carte) | titans, conflit, ruche | rien |
| 13.15 | Mort du geant | 45 a 90 s : perte d equilibre, paliers, explosions internes qui ouvrent des breches, issues pour s echapper, carcasse | titans | rien |
| 13.16 | Interieur du geant | Anatomie qui a un sens, modelee, lisible (remplace 13.5c) | titans | a refaire |
| 13.17 | Rayon qui ravage largement | Tranchee de dizaines de metres, forets couchees, batiments souffles | titans + destruction | rien |
| 13.18 | Terres de l Essaim vivantes | Front qui se bat, ouvrieres au travail, files vers la ruche | ruche | rien |
| 13.19 | Chantiers humains visibles | Ouvriers, echafaudages, ouvrage qui monte au rythme de la simulation | urbanisme | rien |
| 13.20 | Batiments a l echelle | Portes, plafonds, pieces assez grands pour entrer et circuler | architecture | rien |
