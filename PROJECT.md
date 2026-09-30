# PROJECT.md — Classic BID

## Proposition de valeur
Pour les fans d'anime compétitifs

Qui cherchent à tester leurs connaissances de manière rapide et ludique

Le MVP est un jeu de speed-quizzing web

Qui fournit une expérience fluide avec validation automatique, tolérance orthographique (alias) et récompense visuelle immédiate

Contrairement à des plateformes comme Sporcle dont les champs textuels sont stricts, punitifs sur l'orthographe et sans feedback visuel immersif

Notre avantage différenciateur réside dans l'absence totale de friction lors de la saisie (vidage automatique du champ) et l'apparition instantanée des personnages, gardant le joueur dans un état de "flow".

## Énoncé du problème
Cible : Les fans de mangas/animes (spécifiquement de Jujutsu Kaisen pour ce MVP) et les amateurs de jeux de mémoire.

Difficulté : Les quiz classiques sur la pop-culture sont généralement des QCM trop faciles, ou des formulaires rigides qui pénalisent la moindre faute de frappe sur des noms japonais souvent complexes.

Cause : Manque d'outils de quiz mémoriels basés sur la saisie libre tolérante et l'urgence (chronomètre).

Conséquence : L'utilisateur ne ressent pas de véritable défi et abandonne l'expérience rapidement par frustration ou ennui.

## Persona principal
Alvin, 20 ans, fan d'anime et utilisateur mobile.
Il suit Jujutsu Kaisen de près, consomme beaucoup de contenu sur son smartphone, aime les petits jeux rapides (hypercasual) pour passer le temps dans les transports et adore tester ses connaissances face à ses amis.
(Persona secondaire : Satoru, l'Administrateur qui gère le catalogue de personnages pour s'assurer que l'application reste à jour avec le manga).

## Must have
- Authentification (réservée à l'accès Administrateur).
- Dashboard Administrateur (CRUD complet des personnages et des thèmes).
- Recherche et filtrage des personnages dans le dashboard (par thème ou par nom).
- Jeu - Mode Libre : Compte à rebours d'une minute, champ de saisie, compteur de score.
- Jeu - Mode Enchère : Choix d'un objectif chiffré avant le lancement, validation si l'objectif est atteint.
- Moteur de validation : Tolérance orthographique basique (via un système d'alias/mots-clés en base de données).
- Affichage dynamique : Apparition de l'image du personnage dès que son nom est deviné.
- Interface responsive et Design System appliqué.

## Should / Could have
- S : Écran de fin de partie avec un résumé des personnages oubliés.
- Authentification pour les joueurs (sauvegarde des scores personnels).
- Leaderboard global.
- C : Algorithme avancé de tolérance orthographique (ex: distance de Levenshtein) au lieu de simples alias.
- Animations de feedback (secousse si faux, confettis si victoire).
- Partage du score sur les réseaux sociaux.

## Hors périmètre
- Aucune API IA ou externe ne sera utilisée pour générer ou deviner les personnages. Tout est géré en interne via la base Supabase.
- Pas de monétisation ni de système de publicité.
- Pas de gestion d'amis ou de messagerie interne.

## Entités métier et relations
- Theme : Représente la franchise (ex: Jujutsu Kaisen). Bien qu'il n'y en ait qu'un pour l'instant, cela structure la base pour l'avenir.
- Attributs : id, name, description, created_at.
- Character : Représente le personnage à deviner.
- Attributs : id, theme_id (Clé étrangère vers Theme), primary_name, aliases (tableau de chaînes pour la tolérance orthographique, ex: "Gojo", "Satoru"), image_url, created_at.
- AdminUser : Géré nativement par l'Auth Supabase.
- Relation : Un Theme possède un ou plusieurs Character (One-to-Many).

## User stories Must
- US-01 — En tant qu'administrateur, je veux me connecter à un espace sécurisé afin de gérer les données du jeu.
- Étant donné que je suis sur la page de login, quand je saisis mes identifiants corrects, alors je suis redirigé vers le Dashboard.
- US-02 — En tant qu'administrateur, je veux ajouter, modifier ou supprimer un personnage (CRUD) afin de garder le catalogue à jour.
- Étant donné que je suis sur le Dashboard, quand je valide le formulaire d'édition d'un personnage, alors la base de données est mise à jour et la liste s'actualise.
- US-03 — En tant qu'administrateur, je veux filtrer et rechercher les personnages par nom afin de les trouver rapidement.
- Étant donné que je suis sur la liste des personnages, quand je tape "Itadori" dans la barre de recherche, alors seuls les résultats correspondants s'affichent instantanément.
- US-04 — En tant que joueur, je veux lancer une partie en mode "Libre" afin de tester combien de noms je peux citer en 1 minute.
- Étant donné que je suis sur l'écran d'accueil, quand je clique sur "Mode Libre", alors le chronomètre de 60 secondes démarre et le champ de texte s'active.
- US-05 — En tant que joueur, je veux lancer une partie en mode "Enchère" afin de me fixer un objectif précis (ex: 15 personnages).
- Étant donné que je choisis le mode Enchère, quand je saisis "15" et valide, alors le jeu démarre avec une jauge d'objectif fixée à 15.
- US-06 — En tant que joueur, je veux saisir le nom d'un personnage avec une tolérance sur l'orthographe (via ses alias) afin de ne pas être bloqué par une erreur mineure.
- Étant donné que je suis en partie, quand je tape "Gojo" (alias) ou "Satoru Gojo" (nom exact) et que j'appuie sur Entrée, alors le champ se vide, le score s'incrémente de 1, et l'image du personnage apparaît.

## Critères d'acceptation clés
- US-04 — Le Mode Libre (La boucle de temps)
- CA1 : Le compte à rebours de 60 secondes doit être clairement visible et se mettre à jour chaque seconde.
- CA2 : Le timer démarre automatiquement à la saisie du premier caractère dans le champ, ou via un bouton explicite "Démarrer".
- CA3 : À la seconde 00:00, le champ de texte se désactive (attribut disabled) pour empêcher toute nouvelle saisie.
- CA4 : À la fin du temps imparti, un récapitulatif affiche le score final (ex: "12 personnages trouvés").
- US-06 — Le Moteur de Validation (La tolérance orthographique)
- CA1 (Insensibilité) : La validation ignore la casse (majuscules/minuscules) et supprime les espaces superflus avant/après la saisie (trim).
- CA2 (Alias) : Si la saisie correspond au nom principal OU à l'un des alias définis en base (ex: "Satoru", "Gojo", "Satoru Gojo"), la réponse est validée.
- CA3 (Fluidité) : En cas de succès, le champ de saisie se vide automatiquement. L'utilisateur ne doit jamais avoir à effacer le nom précédent pour en taper un nouveau.
- CA4 (Anti-triche/Doublon) : Si le joueur tape le nom ou l'alias d'un personnage déjà trouvé, le score n'augmente pas et le champ se vide (ou tremble) pour indiquer que l'entrée est déjà comptabilisée.
- US-02 — Le Dashboard Administrateur (CRUD et sécurisation)
- CA1 : L'accès à la route /admin (ou équivalent) renvoie systématiquement vers la page de login si l'utilisateur n'a pas de session active.
- CA2 : Lors de la création ou de la modification d'un personnage, le champ "Alias" doit permettre d'enregistrer plusieurs valeurs séparées par des virgules (ou via un champ de tags).
- CA3 : Toute suppression de personnage déclenche une modale ou une alerte de confirmation (ex: "Êtes-vous sûr de vouloir supprimer Yuji Itadori ?") pour éviter les erreurs de manipulation.
