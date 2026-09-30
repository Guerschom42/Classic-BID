# AGENTS.md — Classic BID

## Contexte à lire
Tu es un coding agent assigné à la création de "Classic BID", un MVP de jeu de rapidité mémorielle centré sur l'univers de Jujutsu Kaisen. Ce projet est développé dans le cadre d'une validation académique de niveau BAC+2 en développement web, ce qui exige une rigueur absolue sur l'architecture, la propreté du code et le respect du périmètre défini. Ton objectif est de livrer un produit fonctionnel, sécurisé et déployable, en t'appuyant strictement sur les documents de cadrage fournis (PROJECT.md et DESIGN.md). Toute déviation du socle de base (notamment le CRUD administrateur, l'authentification et le moteur de tolérance orthographique par alias) est interdite.

## Stack
- Next.js (App Router)
- TypeScript (Typage strict)
- Supabase (PostgreSQL, Auth, RLS)
- Vercel (Déploiement continu)
- Tailwind CSS (Stylisation utilitaire)
- Zod (Validation des schémas de données)

## Commandes
- npm run dev (Lancement de l'environnement de développement local)
- npm run build (Vérification de la compilation avant tout déploiement)
- npm run lint (Contrôle strict des règles de linter)
- npx supabase start (Initialisation de la base de données locale)
- npx supabase gen types typescript --local > types/supabase.ts (Génération des types de la base de données)

## Conventions
- Typage explicite obligatoire pour chaque composant, fonction et retour d'API via TypeScript.
- Architecture modulaire respectant la séparation des préoccupations (Server Components par défaut, Client Components uniquement lorsque l'interactivité l'exige).
- Nommage des variables, fonctions et fichiers en anglais (camelCase pour les variables, PascalCase pour les composants).
- Nommage des données affichées à l'utilisateur (textes, boutons, interface) exclusivement en français.
- Approche "Mobile-First" systématique pour garantir une ergonomie parfaite sur smartphone.

## Interdits
- Ne jamais désactiver les alertes TypeScript ou utiliser le type any sous aucun prétexte.
- Ne jamais intégrer d'API externes complexes ou d'outils d'Intelligence Artificielle pour la logique de jeu.
- Ne jamais stocker de variables d'environnement sensibles ou de clés API en clair dans le code source.
- Ne jamais modifier le périmètre fonctionnel défini dans le MoSCoW sans une validation explicite du Product Owner.
- Ne jamais contourner les règles de sécurité Row Level Security (RLS) de Supabase pour gagner du temps.

## Workflow
- Analyser l'intégralité de PROJECT.md et DESIGN.md avant d'écrire la moindre ligne.
- Initialiser le projet Next.js avec la configuration Tailwind et le typage TypeScript.
- Configurer le projet Supabase (Auth, création des tables Theme et Character, configuration des Policies RLS).
- Développer le socle d'authentification et restreindre l'accès à la route sécurisée /admin.
- Construire le Dashboard administrateur avec un CRUD complet pour la gestion du catalogue de personnages.
- Implémenter le moteur de jeu (composants du chronomètre, champ de saisie, logique de validation par alias).
- Intégrer les vues spécifiques des modes "Libre" et "Enchère" avec mise à jour du score et apparition des images en temps réel.
- Vérifier l'absence totale d'erreurs de build (npm run build) et déployer la version finale sur Vercel.
