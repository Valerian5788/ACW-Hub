**Confirmation : OUI, l'approche MCP (implémentée via RAG) reste absolument au cœur du projet.** C'est votre différentiateur clé et la base de la fiabilité que vous voulez offrir. Tout le travail technique initial doit converger vers la mise en place de ce système.

**Premier Axe de Travail Technique (ASAP - les ~3-4 prochaines heures) :**

**Objectif :** Mettre en place l'environnement de base et **créer le mini-corpus de connaissances fiable** pour UN cas d'usage précis. Sans ces données, le RAG/MCP ne peut pas fonctionner.

**Titre de l'Axe :** **"Setup Projet & Constitution du Corpus Initial (Ex: Prime Toiture)"**

**Pourquoi c'est la priorité N°1 :**
1.  **Dépendance Critique :** Le système RAG/MCP a besoin de données *officielles* pour fonctionner. C'est le carburant.
2.  **Implication Immédiate de Tous :** Permet aux débutants de contribuer dès le départ sur une tâche essentielle (recherche, extraction simple) avec l'aide de l'IA pour le code.
3.  **Base pour la Suite :** Fournit des données concrètes pour que toi (Tech Lead) et l'étudiant CS puissiez ensuite construire et tester le pipeline RAG.

**Tâches Détaillées et Répartition Suggérée :**

1.  **Validation Finale du Cas d'Usage (30 min) - Toute l'équipe :**
    * Confirmez **définitivement** le cas d'usage précis pour la démo. Ex: "Prime Isolation Toiture Wallonie 2025 pour un particulier propriétaire".
    * **ACTION IMMÉDIATE :** Tout le monde cherche rapidement les **sources officielles primaires** (pages sur `energie.wallonie.be` ou `wallonie.be`, PDFs des règlements/brochures...). Il faut **valider** qu'on trouve au moins 2-3 sources claires et exploitables. Si c'est trop flou ou introuvable, **changez de sujet MAINTENANT** pour un autre plus simple (ex: Règles de tri BEP pour Fernelmont).

2.  **Setup de l'Environnement et du Repo (1h) - Tech Lead + Étudiant CS :**
    * **Tech Lead :** Crée le repo GitHub public, ajoute le README simple. Définit la structure de base des dossiers (`/data`, `/src`, `/scripts`...).
    * **Étudiant CS :** Met en place l'environnement virtuel Python (`venv`), crée le `requirements.txt` initial (avec `requests`, `beautifulsoup4`, `PyMuPDF`, `python-dotenv`, `Flask`/`FastAPI`, `openai`/`google-generativeai`...).
    * **Tech Lead OU Étudiant CS :** Crée un script `test_llm_api.py` très simple pour vérifier la connexion à l'API LLM choisie (OpenAI, Google, Anthropic). **Utiliser `python-dotenv` pour gérer la clé API de manière sécurisée** (ne pas la commiter sur GitHub !). Confirmer que l'appel de base fonctionne.

3.  **Acquisition et Préparation des Données (2-3h) - Étudiants Cybersec (+ AI Assist & Support Tech Lead) :**
    * **Cybersec 1 (Focus Web) :**
        * Utilise `requests` et `BeautifulSoup4` pour **scraper le contenu textuel principal** des 2-3 pages web officielles identifiées à l'étape 1.
        * **Nettoyage minimal :** Essayer d'enlever les menus, footers, pubs pour ne garder que le contenu utile.
        * Sauvegarde le texte dans des fichiers `.txt` clairs dans le dossier `/data` (ex: `data/prime_toiture_conditions_web.txt`).
        * *Peut utiliser Copilot/ChatGPT pour générer les snippets de code Python pour le scraping d'une URL donnée.*
    * **Cybersec 2 (Focus PDF) :**
        * Identifie et télécharge le(s) 1-2 PDF(s) officiel(s) les plus pertinents (règlement, brochure).
        * Utilise `PyMuPDF` (module `fitz`) pour **extraire le texte brut** de ces PDFs.
        * Sauvegarde le texte dans des fichiers `.txt` clairs dans le dossier `/data` (ex: `data/prime_toiture_reglement_details.txt`).
        * *Peut utiliser Copilot/ChatGPT pour générer les snippets de code Python pour l'extraction de texte d'un PDF.*
    * **Tech Lead :** Fournit un support technique ponctuel, aide à débloquer si l'extraction est complexe, valide la qualité/pertinence des textes extraits.

**Livrable de cette Première Phase (Objectif pour ~15h aujourd'hui) :**

* Le repo Git est fonctionnel et partagé.
* L'environnement de développement de base est prêt (`requirements.txt`).
* L'appel à l'API LLM est confirmé comme fonctionnel.
* Le dossier `/data` contient **2 à 5 fichiers `.txt`** représentant le **contenu officiel essentiel** pour répondre aux questions de base sur le cas d'usage choisi (ex: Prime Toiture).

**Suite Logique (Après cette phase) :**

Une fois ces données disponibles, toi (Tech Lead) et l'étudiant CS pourrez commencer à implémenter le **cœur du RAG** :
* Mettre en place le **Retrieval** (simple ou vectoriel) pour chercher dans ces fichiers `.txt`.
* Construire la logique qui prend la question utilisateur, récupère le contexte pertinent, et appelle l'API LLM avec le **prompt MCP contraignant**.

Cette première étape est cruciale et permet à tout le monde de contribuer dès le début sur des tâches concrètes et indispensables au projet. C'est la fondation de votre WCA fiable !