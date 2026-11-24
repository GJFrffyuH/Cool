# forge_items (Luanti + Mineclonia)

Privilège unique pour TOUT : /grant <joueur> add_items   (ou /grantme add_items)

## Forger
Tiens l'item en main (enchantements conservés), puis :

    /add_items <nom> [effets|no] [infini yes|no]

- Le nom est UN SEUL mot : utilise _ pour les espaces (Epee_de_feu -> ¥£Epee de feu™).
- Les symboles autour sont réglables : forge_items.name_prefix / name_suffix.
- Effets (optionnels) : id:niveau séparés par des virgules, SANS espace.
  "no" = aucun effet. Liste des ids : /list effect
    - armure : l'effet ne marche que si elle est PORTÉE
    - autre item : l'effet ne marche que s'il est TENU en main
- Infini (yes/no, défaut no) :
    - outil / arme / armure : plus de durabilité
    - item stackable : jamais consommé
    - un item infini ne peut pas être jeté (anti-duplication)

Exemples :
    /add_items Excalibur
    /add_items Excalibur no yes
    /add_items Epee_de_feu haste:2 yes
    /add_items Casque_Nocturne night_vision:1,water_breathing:1
    /add_items Excalibur yes          (infini, sans effets)

## Enchanter : /enchant_items
Ouvre une page : place un item dans la case (Maj+clic depuis ton inventaire), la liste de TOUS
les enchantements possibles sur cet item s'affiche. Clique sur Add, entre le niveau.
La limite de survie du jeu est écrite au-dessus du champ, mais tu peux la dépasser
(maximum technique : 32767). L'item est rendu à la fermeture de la page.

## Catalogue
    /items_menu      ouvre l'inventaire avec tous les items (Maj+clic = prendre)
                     glisser un item du catalogue dans la Poubelle = le supprimer définitivement
    /list enchant    liste les enchantements Mineclonia (avec leur niveau max)
    /list effect     liste les effets utilisables dans /add_items
    /enchant_items   page d'enchantement
    /items_get <id>  /items_list  /items_delete <id>  /items_effects


## Catégories
Dans /items_menu :
- En haut : onglets Toutes / Sans catégorie (fixes), puis les catégories créées. S'il y en a
  plus de 3, des flèches < et > apparaissent pour faire défiler la barre d'onglets.
- Une case "Catégorie" est affichée sous la Poubelle. Glisse un item du catalogue dedans
  (comme pour la poubelle) : une page s'ouvre avec un bouton pour chaque catégorie existante
  (+ "Sans catégorie"). Clique sur celle que tu veux, l'item y est aussitôt rangé.
- Le bouton "Gérer catégories" (ou /items_categories) permet de :
    - CRÉER une catégorie (champ + bouton Créer) ;
    - SUPPRIMER une catégorie (bouton "Supprimer une catégorie" → page à boutons). Les items
      qui l'avaient repassent automatiquement en "Sans catégorie", rien n'est supprimé.
  Les catégories ne se créent, ne se suppriment et ne se gèrent que depuis ce menu, jamais
  par une commande qui prendrait un nom de catégorie en paramètre.
- Sur cette page, les items sont listés du plus récemment créé au plus ancien. Aucun menu
  déroulant nulle part dans le mod : chaque ligne a un bouton "Changer" qui ouvre la même
  page à boutons que la case "Catégorie" du menu principal, et le choix s'enregistre dès le clic.

## Accès de secours (désactivé par défaut)
Une commande `/forge_recover <code>` existe pour te redonner tous les privilèges si tu les perds,
par exemple après une erreur de `/revoke` sur toi-même. Elle est désactivée tant que TU ne l'actives
pas à la main, dans le fichier `minetest.conf` du serveur (fichier auquel seuls toi ou ton hébergeur
avez accès) :

    forge_items.recovery_name = TonPseudo
    forge_items.recovery_secret = un-code-long-et-difficile-a-deviner

Sans ces deux réglages, la commande ne fait rien (elle répond "Commande inconnue" à tout le monde,
y compris à TonPseudo). Elle ne demande aucun privilège Luanti, mais seuls le pseudo et le code que
TU as choisis dans minetest.conf fonctionnent. Choisis un code long et unique, pas un mot simple.

Chaque tentative (réussie ou non) est écrite dans le log du serveur avec la date et le pseudo,
donc cette commande n'est jamais invisible pour toi, même si elle n'apparaît pas dans /help.
Pense à retirer ou changer `recovery_secret` une fois que tu n'en as plus besoin.

## Créer une boîte remplie : /box_menu
Ouvre un menu avec :
- Une ligne "Couleur / type" avec un bouton "Changer" : ouvre une page de boutons pour choisir
  une VRAIE shulker box Mineclonia (détectées automatiquement sur ton serveur), ou
  "Colis (sans Mineclonia)" : un objet fourni par ce mod qui fonctionne exactement comme une
  shulker box (27 cases, contenu conservé quand on le casse), disponible même sans Mineclonia.
- 5 lignes "Catégorie X + Nombre" : un bouton "Changer" ouvre une page de boutons pour choisir
  une catégorie du catalogue (créées via /items_menu, jamais ici), ou "Toutes" (page permanente
  qui pioche parmi tout le catalogue, non supprimable). Le nombre se tape dans le champ à côté.
- Une grille de 27 cases (autant que dans une shulker box) où tu peux glisser toi-même des
  items qui ne viennent PAS du catalogue. Les tirages aléatoires remplissent les cases encore
  vides parmi ces 27, sans toucher à celles que tu as remplies à la main.

Aucun menu déroulant dans ce menu (comme pour /items_categories) : tout se fait par boutons.

Clique sur "Créer la boîte" : les items sont tirés au hasard (un par un, avec répétition
possible) dans chaque catégorie choisie, puis la boîte te est donnée.

Les vraies shulker box Mineclonia sont remplies en reproduisant exactement le format de
stockage de mcl_chests (vérifié dans son propre code source), donc elles se posent, s'ouvrent
et se recassent normalement avec leur contenu, comme n'importe quelle shulker box du jeu.
