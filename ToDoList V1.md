# ACW Hackathon PoC - Plan de Travail

**Objectif :** Démo fonctionnelle (MVD) du flux ACW sur 1 cas d'usage (ex: Prime Toiture Fernelmont) montrant RAG fiable et escalade intelligente.

**Tech Stack :** Python, FastAPI/Flask, Streamlit, Sentence-Transformers, ChromaDB, API LLM (OpenAI/Gemini/Claude).

---

## Priorité #1 : Démarrage (Tous)

-   [ ] **Valider Cas d'Usage :** Confirmer "Prime Toiture 2025 Fernelmont".
-   [ ] **Trouver & Préparer Données :** (Cybersec 1 & 2) Trouver 2-5 docs officiels (PDF/Web). Extraire le texte brut -> `/data/*.txt`. **CRUCIAL !** *Si bloqué, créer des données fictives réalistes.*
-   [ ] **Setup Projet :** (Lead) Repo Git, `venv`, `requirements.txt`.
-   [ ] **Accès API LLM :** (Lead/Tous) Confirmer clé API fonctionnelle & tester appel simple + Function Calling basique.

---

## Répartition des Tâches Principales

### Lead Dev (Junior Freelance) - Backend & Orchestration

-   [ ] **API Backend :** Mettre en place FastAPI/Flask. Créer endpoint principal `/chat` (POST).
-   [ ] **Appel LLM :** Intégrer appel API LLM (avec prompt système MCP).
-   [ ] **Intégration RAG :** Recevoir contexte du module RAG et l'injecter dans le prompt LLM.
-   [ ] **Orchestration Function Calling :**
    -   Envoyer définitions des outils au LLM.
    -   Interpréter la réponse du LLM (texte ou appel d'outil).
    -   Appeler les fonctions Python des outils si demandé par LLM.
    -   (Optionnel) Renvoyer résultat outil au LLM pour mise en forme.
-   [ ] **Gestion Git :** Gérer les branches (`develop`, `main`) et les merges. Débloquer les autres.

### Étudiant CS - Logique des Outils

-   [ ] **Implémenter `generer_checklist(procedure)` :** Fonction Python. Retourne liste étapes (peut être hardcodée pour la démo si parsing complexe).
-   [ ] **Implémenter `simplifier_texte(texte_original)` :** Fonction Python. Appelle LLM avec prompt de simplification.
-   [ ] **Implémenter `preparer_escalade_humaine(query, historique)` :** Fonction Python.
    -   Phase 1: Retourne questions de clarification prédéfinies.
    -   Phase 2: Formate un ticket final (texte/JSON) avec les réponses.
-   [ ] **Assister Backend :** Aider à l'intégration des fonctions outils dans l'API.

### Étudiant Cybersec 1 - Pipeline RAG

-   [ ] **Chargement Données :** Charger les fichiers `.txt` préparés.
-   [ ] **Chunking :** Découper le texte en morceaux (chunks).
-   [ ] **Embedding :** Utiliser `sentence-transformers` pour vectoriser les chunks.
-   [ ] **Indexation :** Stocker les vecteurs dans `ChromaDB`.
-   [ ] **Retrieval :** Créer fonction `retrieve(query)` qui cherche les chunks pertinents dans ChromaDB et les retourne. *Commencer simple, viser fonctionnel.*

### Étudiant Cybersec 2 - Frontend & Data Initiale

-   [ ] **Extraction Initiale Données :** Aider à trouver et nettoyer les docs -> `.txt`.
-   [ ] **Interface Streamlit :**
    -   Créer interface chat basique (input user, zone affichage conversation).
    -   Bouton "Envoyer" qui appelle l'endpoint `/chat` du backend (POST).
    -   Afficher la réponse texte du backend.
    -   Ajouter boutons/logique pour "Simplifier" et "Pas utile / Aide humaine".
    -   Gérer l'affichage des questions pour l'escalade et l'envoi des réponses.

---

## Coordination

-   **Git :** Commits fréquents. Branches par feature/personne. PR vers `develop`.
-   **Communication :** Canal dédié (Discord/Slack...). Points rapides réguliers. Demander de l'aide tôt !
-   **Focus MVD :** Simple et fonctionnel > Complexe et buggé. Hardcoder si besoin pour la démo.