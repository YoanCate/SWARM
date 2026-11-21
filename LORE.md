# Le monde de SWARM

État au 15/11/2026. Ce document est la référence : tout texte du jeu doit s'y tenir. Il suit les options « 1 » de
`Docs/PLAN.md` (L1.1, L2.1, L5.1), prises par défaut ; le propriétaire peut en changer, et ce qui en dépend est
regroupé dans `Source/Swarm/Game/SwarmLore.cpp`.

**Règle d'écriture.** Les personnages peuvent se tromper ou mentir sur le passé. Aucun texte ne ment sur les règles
du jeu : ce qu'une stèle dit de la façon de battre l'Essaim est vrai dans la simulation.

## L'Essaim

Une moisson. Il passe de monde en monde, mange ce qui pousse, bâtit avec ce qu'il a mangé une Arche vivante, y porte
son couvain royal, et repart. Il n'a ni haine ni projet : on ne négocie pas avec lui.

Il est déjà venu. C'est lui qui a fait tomber le monde d'avant, il y a trois siècles. Le monde a mis trois siècles à
repousser. Il revient parce que le monde a repoussé.

Chaque peuple lui donne un nom : **la Moisson** (Concordat), **la Vermine** (Fonderies), **la Nuée** (Clans),
**l'Ennemi** (Protectorat), **les Mangeurs** (Affranchis).

## Le monde d'avant

Une civilisation technique : des engins qui volaient, des armes « qui brûlaient l'horizon ». Elle a tenu une saison.
Elle a perdu parce que chaque ville s'est défendue seule. Il en reste :

- **les épaves** : ses engins, abattus ou posés à bout de carburant, avec les carnets de ceux qui étaient dedans ;
- **les Archives-Semences** : une voûte de graines sous une colline, d'où tout a été replanté ;
- **les cinq peuples**, descendants de cinq groupes de survivants.

## Les cinq peuples

| Peuple | D'où il vient | Ce qu'il retient de la chute | Sa rancune |
|---|---|---|---|
| **Concordat d'Aram** (théocratie) | Les archivistes de la voûte | « Tout a repoussé de nos graines. » | Les Fonderies creusent sous ses terres. |
| **Ligue des Fonderies** (oligarchie) | Les ouvriers réfugiés au fond des mines | « Tout ce qui tient debout sort de notre four. » | Aram s'attribue leur œuvre. |
| **Clans du Vent** (nomades) | Ceux qui ont marché | « Elle a mangé ce qui ne bougeait pas. » | Le Protectorat a clôturé leurs pâtures. |
| **Protectorat de Hael** (junte) | Le dernier régiment, qui a tenu les cols | « Il fallait une ligne. Maintenant il y en a une. » | Les Affranchis, serfs révoltés. |
| **Les Affranchis** (commune) | Les serfs du Protectorat, soulevés il y a une génération | « La chute, c'est tous les jours d'après. » | Le Protectorat, et les Fonderies qui vendaient les chaînes. |

Ces rancunes sont celles de la simulation (inimitiés de départ) : elles ne sont pas un décor.

## Le joueur

**L'Anonyme.** Un survivant que personne ne connaît. Son nom, ce sont les peuples qui le lui donneront, selon ce
qu'il fait. (Pas encore dans le jeu : la cité d'origine, les surnoms.)

## Les anciens

Chaque peuple a perdu la tombe d'un de ses fondateurs. Le nom est tiré par la simulation ; l'épitaphe dit ce qu'il a
fait ; et chacun a laissé une leçon, qui est une règle vraie du jeu :

| Tombeau | Ce que l'ancien a fait | Ce qu'il a laissé (vrai dans la simulation) |
|---|---|---|
| Concordat | A porté les semences sous la terre | La Reine se tue avant que l'Arche ait sa quille : alors tout finit. Après, il faut aussi briser l'Arche. |
| Fonderies | A rallumé le premier four | Un champion qui s'échappe revient adapté à l'arme qui l'a blessé. |
| Clans | A marché devant | Pas d'Arche sans assez de ruches ; brisée, elle n'est pas rebâtie tout de suite. |
| Protectorat | A tenu le col | Éventrer la ruche, puis abattre toute la garde royale, avant de pouvoir toucher la Reine. |
| Affranchis | Né dans les fers, mort debout | Si l'Arche est en chantier quand la Reine tombe, l'Essaim continue, sans tête. |

## Ce qu'on lit et entend dans le jeu

- **« Racontez-moi la chute »** : nouvelle question à tout habitant, soldat ou chef. Cinq récits, un par peuple.
- **Tombeaux** : l'épitaphe, puis la leçon de l'ancien (deux pages).
- **Épaves** : quatre carnets de trois pages — un pilote, un médecin, un sergent, une enfant. Mis bout à bout, ils
  racontent le dernier jour d'avant, et chacun rejoint le récit d'un peuple (les mines, les graines, la marche).
- **Caches** : un mot laissé par celui qui n'est pas revenu (six).
- **Recueil** : second écran du journal (deux appuis sur J). Tout ce qui a été lu ou entendu s'y range, vingt textes
  au plus par monde.

## Ce qui n'est pas encore écrit

- Les chefs : un caractère, une peur, une demande personnelle pour chacun (PLAN 5.2).
- Les secrets des peuples (PLAN L3.5), les coutumes visibles en cité (L3.4).
- Le sort de l'Arche comme dilemme entre peuples (L4.2), l'intérieur de l'Arche (L4.3).
- La cité d'origine du joueur et ses surnoms (L5.1).
- Les reliques des tombeaux et la Voûte des Archives-Semences (L6.4, PLAN 9.5).
- Dictons, chants, stèles de cité (L7.2, L7.5).
