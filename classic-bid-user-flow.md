flowchart TD
  A["🏠 Accueil / Landing Page<br/>Logo Classic BID + thème Jujutsu Kaisen"] --> B{"Choix du mode"}
  B -->|"Mode Libre"| D
  B -->|"Mode Enchère"| C
  C["🎯 Interstitiel : Choix de l'Objectif<br/>'Combien de personnages peux-tu citer ?'<br/>Saisie du nombre (ex: 15)"] --> C2{"Clique 'Démarrer'"}
  C2 --> D
  D["⚔️ Arène de Jeu — Core Loop<br/>Timer 60s démarre · Input auto-focus"]
  D --> E["⌨️ Saisie d'un nom → touche Entrée"]
  E --> F{"Validation"}
  F -->|"✅ Succès (alias reconnu, pas de doublon)"| G["Input vidé · Score +1<br/>Carte personnage ajoutée à la grille (fade-in)"]
  G --> H{"Timer atteint 00:00 ?"}
  F -->|"❌ Échec / Doublon"| I["Input tremble (shake)<br/>Texte rouge neon-error · correction possible"]
  I --> E
  H -->|"Non, temps restant"| E
  H -->|"Oui"| J["⏱️ Fin de partie<br/>Input désactivé · Modale de résultats"]
  J --> K{"Mode ?"}
  K -->|"Libre"| L["📊 Score final<br/>'Tu as trouvé X personnages !'"]
  K -->|"Enchère"| M{"Score ≥ Objectif ?"}
  M -->|"Oui"| N["🏆 'Contrat rempli' (Victoire)"]
  M -->|"Non"| O["💀 'Tu as été trop gourmand' (Défaite)"]
  N --> P["Boutons : Rejouer / Retour à l'accueil"]
  O --> P
  L --> P
  P -->|"Rejouer"| Q{"Mode courant ?"}
  Q -->|"Libre"| D
  Q -->|"Enchère"| C
  P -->|"Retour à l'accueil"| A