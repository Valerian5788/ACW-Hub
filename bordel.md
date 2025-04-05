MCP = Model Context Protocol
Quelque questions pour t'aider avec le contexte du projet : 
**1. Quel est le nom du projet ?**

> "Notre projet s'appelle **ACW - Assistant Contextuel Wallon**." 

**2. Le projet répond à quelle problématique ?**

> "ACW s'attaque à un double problème :
> 
> 1. **Pour les citoyens :** La **complexité et la fragmentation** de l'information administrative wallonne rendent l'accès difficile et peuvent mener à des erreurs ou à la désinformation, surtout avec l'émergence d'IA génériques non contrôlées.
> 2. **Pour les communes/administrations :** Elles perdent un temps précieux à répondre à des questions récurrentes ou à gérer des demandes citoyennes incomplètes, ce qui génère des allers-retours inefficaces." 

**3. Quel est le service/la solution proposé(e) ?**

> "ACW est une **plateforme web interactive (pour le prototype)** qui utilise une **Intelligence Artificielle fiable et contrôlée**. Grâce à une technologie clé - les **Model Context Protocols (MCPs)** - ACW devient une **source de vérité** :
> 
> - Elle fournit aux citoyens des réponses **exactes et vérifiables**, basées **uniquement** sur des documents officiels wallons et communaux.
> - Elle **assiste activement** les citoyens dans leurs démarches (checklists, simulations...).
> - Elle **simplifie** le jargon administratif à la demande.
> - Et, crucialement, lorsque l'IA atteint ses limites, elle aide le citoyen à **structurer une demande parfaite pour l'agent communal**, réduisant la charge de travail administratif." 

**4. À qui s'adresse-t-il ? (Public cible)**

> "Notre solution s'adresse principalement **à tous les citoyens wallons** qui cherchent une information administrative fiable ou une aide dans leurs démarches. Mais elle bénéficie aussi directement **aux agents communaux et administratifs**, en leur fournissant des demandes citoyennes mieux qualifiées et en filtrant les questions simples." 

**5. Quel est le support ? (Appli mobile, plateforme web,...)**

> "Pour ce hackathon, nous développons un **prototype sur une plateforme web** pour démontrer le concept. À l'avenir, une application mobile pourrait évidemment être envisagée pour une accessibilité maximale." 
J'ai besoin de mettre en place un premier workflow :

  

Arbre décisionnel --> IA reçoit les réponses de l'arbre décisionnel + input utilisateur --> IA utilise model context protocol (MCP) pour prendre une décision : Réponse spécifique (Regarde en base de donnée, là où on aura nos documents etc), Lance un process de completion d'un ticket complet pour ensuite etre transfère vers un agent communal par exemple. Il récupère les questions grace aussi à la base de donnée et à ces connaissances internes. Il faut qu'on innove un peu ici..

Le projet final sera interface pour aider administrativement dans leurs recherches etc les citoyens.

Soit une question avec une réponse rapide et fiable (avec source etc, grace a mcp) ou alors une escalade finissant par un ticket ultra complet pour un agent communal. Hackaton innovant, il faut que ce soit innovant dans l'explication au jury etc...

  

Aide moi à mettre ça en place techniquement (quel ia, faire tourner le mcp), je peux tester en local avec claude desktop et un mcp, mais il faudra le mettre en ligne ensuite.