#### **Front-end (Étudiants)**

- **Tâche** : Créer une page HTML/CSS vanilla.
    - Champ texte pour la question utilisateur.
    - Bouton "Envoyer".
    - Zone pour afficher la réponse IA.
- **Objectif** : Rendre l’interface lisible (style minimal).
- **Livrable** : Page statique connectable au back-end.

#### **Back-end (Freelance Junior)**

- **Tâche** : Configurer un serveur Python avec Flask.
    - Route POST pour recevoir la question.
    - Route pour renvoyer une réponse.
- **Objectif** : Ajouter une fonction placeholder pour fetch des données (ex. texte statique).
- **Livrable** : Serveur qui prend une question et renvoie "Réponse en cours".

#### **Script MCP (Val)**

- **Tâche** : Développer un script Python pour le MCP.
    - Définir des règles pour choisir les outils IA selon le contexte.
    - Préparer l’utilisation des données TXT/CSV.
- **Livrable** : Script simulant le choix d’outil (ex. "permis" → outil spécifique).

#### **Arbre Décisionnel (Val)**

- **Tâche** : Concevoir un arbre simple.
    - Ex. "horaire" → réponse directe ; sinon → IA.
- **Objectif** : Intégrer au back-end.
- **Livrable** : Module Python pour trier les questions.

#### **Données (Étudiants)**

- **Tâche** : Extraire/scraper des données de PDF wallons officiels (ex. démarches).
- **Objectif** : Convertir en TXT ou CSV.
- **Livrable** : Fichier structuré (ex. "Démarche : Permis\nDétails : ...").