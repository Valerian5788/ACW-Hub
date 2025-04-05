**1. Vision Globale (Synthèse Exécutive)**

ACW (Assistant Contextuel Wallon) est une plateforme web innovante conçue pour **simplifier et fiabiliser** les interactions entre les **citoyens wallons** et leur administration. Face à la complexité administrative, à la fragmentation de l'information et aux risques de désinformation (y compris par des IA non contrôlées), ACW se positionne comme un **orchestrateur IA intelligent et digne de confiance**.

Son innovation clé réside dans son **"Model Context Protocol" (MCP)** : un ensemble de règles et l'utilisation contrôlée d'outils spécifiques, garantissant que l'IA :

- Utilise **exclusivement des sources d'information officielles wallonnes** validées pour répondre aux questions.
- **Choisit l'action la plus pertinente** : répondre, assister dans une démarche, simplifier l'information, ou préparer une escalade efficace vers un agent humain.
- Apporte un **double bénéfice** : une aide précieuse et fiable pour le citoyen, et une optimisation du travail pour les agents communaux/administratifs.

**2. Le "Model Context Protocol" (MCP) d'ACW : Le Cerveau et ses Règles**

Dans ACW, le MCP n'est pas juste une contrainte, c'est le **mode opératoire intelligent de l'IA**. Il définit :

- **Les Règles du Jeu :** Des instructions claires pour l'IA (le LLM) sur comment se comporter. Ex: "Priorise toujours la recherche dans les documents officiels pour répondre", "N'invente jamais d'information", "Si une action spécifique est demandée, utilise l'outil correspondant", "Si tu ne sais pas ou si l'utilisateur est bloqué, prépare une escalade humaine", "Reste dans le périmètre administratif wallon".
- **La Boîte à Outils Contrôlée :** Une liste définie de "capacités" (implémentées comme des fonctions Python) que l'IA peut décider d'utiliser :
    - `chercher_info_fiable`: Interroger la base de connaissance RAG des documents officiels.
    - `generer_checklist`: Produire la liste des étapes/documents pour une procédure.
    - `verifier_eligibilite`: Exécuter une simulation basée sur des règles codées.
    - `simplifier_texte`: Reformuler un texte en langage simple.
    - `preparer_escalade_humaine`: Collecter les infos et formater un ticket pour un agent.
- **La Fondation : Fiabilité des Données :** Le MCP impose que l'outil `chercher_info_fiable` (et idéalement les autres outils qui dépendent d'infos spécifiques comme `verifier_eligibilite`) ne puise ses informations **que** dans le corpus de données officielles wallonnes que nous avons préparé et validé.

**3. Fonctionnalités Clés & Implémentation Technique (Focus PoC 24h)**

Nous allons implémenter un PoC démontrant le flux principal sur **UN cas d'usage précis** (ex: Prime Toiture 2025 à Fernelmont).

- **3.1. Noyau Q&R Fiable (Outil `chercher_info_fiable`)**
    
    - **Fonctionnel :** ACW répond aux questions factuelles sur la Prime Toiture en citant ses sources (les documents officiels indexés). Garantit l'exactitude dans les limites du corpus.
    - **Technique (PoC - Priorité #1) :**
        - Backend Python (Flask/FastAPI).
        - **Corpus :** 2-5 fichiers `.txt` dans `/data` (textes extraits via scraping/PDF parsing des sources officielles sur la Prime Toiture).
        - **RAG Pipeline :**
            - _Indexation :_ Création d'un index vectoriel simple (Sentence-Transformers + ChromaDB/FAISS) à partir des fichiers `.txt`.
            - _Retrieval :_ Fonction qui cherche les chunks pertinents dans l'index vectoriel basé sur la question utilisateur.
            - _Generation :_ Appel API LLM (OpenAI/Google/Anthropic) avec :
                1. Prompt Système contenant les règles MCP ("Réponds uniquement basé sur CONTEXTE...", "N'invente rien...").
                2. Le CONTEXTE récupéré par le Retrieval.
                3. La question utilisateur.
        - **Tool Definition :** Déclarer `chercher_info_fiable` comme un outil potentiel pour le LLM (via Function Calling API).
- **3.2. Assistant d'Action (Ex: Outil `generer_checklist`)**
    
    - **Fonctionnel :** Si l'utilisateur demande "la checklist pour la prime toit", l'IA utilise cet outil pour la fournir.
    - **Technique (PoC - 1 Action) :**
        - Coder une fonction Python `generer_checklist(procedure)` qui, pour `procedure='prime_toiture'`, va chercher dans le fichier `.txt` correspondant et extrait la liste (via parsing simple : chercher `*`, `-`, `1.`) ou renvoie une liste pré-codée pour la démo.
        - **Tool Definition :** Déclarer `generer_checklist` (avec son paramètre `procedure`) pour le Function Calling de l'API LLM.
- **3.3. Simplification de Texte (Outil `simplifier_texte`)**
    
    - **Fonctionnel :** Un bouton "Simplifier" permet à l'utilisateur de demander une version plus simple de la _dernière réponse fournie par ACW_.
    - **Technique (PoC - Focus Interne) :**
        - Coder une fonction Python `simplifier_texte(texte_original)` qui renvoie ce texte à l'API LLM avec un prompt spécifique ("Reformule ce texte en langage simple...").
        - **Tool Definition :** Déclarer `simplifier_texte` (avec paramètre `texte_original`) pour le Function Calling. Le Frontend appellera le backend qui déclenchera cet outil via le LLM.
- **3.4. Escalade Humaine Intelligente (Outil `preparer_escalade_humaine`)**
    
    - **Fonctionnel :** Si l'IA ne trouve pas de réponse via RAG ou si l'utilisateur clique "Pas utile", l'IA (via cet outil) pose 2-3 questions ciblées pour enrichir la demande initiale et **affiche un ticket structuré**, prêt à être envoyé (simulation) à un agent communal. Met en avant le gain d'efficacité pour la commune.
    - **Technique (PoC - Simulation) :**
        - Coder une fonction Python `preparer_escalade_humaine(query_initiale, historique_chat)` qui :
            1. Identifie le sujet (fixé à "Prime Toiture" pour le PoC).
            2. Retourne des questions prédéfinies à poser à l'utilisateur via l'interface.
            3. Une fois les réponses obtenues, formate une chaîne de caractères/JSON simulant le ticket final.
        - **Tool Definition :** Déclarer `preparer_escalade_humaine` pour le Function Calling. L'IA décidera de l'appeler dans les cas définis (pas de réponse RAG, clic bouton user).
- **3.5. Interface & Orchestration (Le Chef d'Orchestre)**
    
    - **Fonctionnel :** Interface web simple, type chat, avec les boutons nécessaires. Intuitive.
    - **Technique (PoC) :**
        - **Frontend :** Streamlit (recommandé) ou HTML/CSS/JS simple. Envoie la requête et affiche les réponses/résultats. Gère l'affichage des questions de clarification pour l'escalade.
        - **Backend (Flask/FastAPI) :** Reçoit la requête du Frontend. C'est lui qui **orchestre l'appel à l'API LLM avec la description des outils (Function Calling)**. Il interprète la réponse du LLM : si c'est du texte, il le renvoie au Frontend ; si c'est une demande d'appel d'outil, il exécute la fonction Python correspondante, récupère le résultat, le renvoie potentiellement au LLM pour mise en forme, puis envoie la réponse finale au Frontend.

**4. Technologies Envisagées (PoC)**

- **Langage :** Python 3.x
- **Backend :** Flask ou FastAPI
- **Frontend PoC :** Streamlit (fortement recommandé pour la rapidité)
- **Data Prep :** requests, BeautifulSoup4, PyMuPDF
- **LLM API :** OpenAI, Google Gemini, ou Anthropic Claude 3 (choisir une API supportant bien le Function Calling / Tool Use et avoir une clé !)
- **RAG Index/Search :** sentence-transformers + ChromaDB/FAISS (ou juste recherche simple si manque de temps/compétence)
- **Orchestration :** Code Python custom ou potentiellement Langchain/LlamaIndex si le temps permet _après_ avoir maîtrisé les bases.

**5. Objectif Hackathon & Prochaines Étapes**

- **Focus MVD :** Démontrer le flux complet (Question -> Réponse Fiable OU Action OU Escalade Intelligente) sur **UN cas d'usage**. La fiabilité (MCP via RAG) et l'escalade intelligente (bénéfice commune) sont les points clés à montrer.
- **Prochaines Étapes Immédiates :**
    1. Valider définitivement le cas d'usage ET trouver les sources officielles.
    2. Setup du repo et de l'environnement Python.
    3. Test de l'appel API LLM de base (avec clé).
    4. Commencer l'extraction des données pour le corpus (Tâche pour les Cybersec).

