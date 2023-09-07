# REST API vs GraphQL vs gRPC


## Introduction

Depuis de nombreuses années, REST est un style architectural standard de l’industrie pour la conception d’API Web. Cependant, GraphQL et gRPC sont récemment apparus afin de remédier à certaines des limitations de REST. Chacune de ces approches API présente des avantages substantiels et certains compromis.

Dans ce document , nous examinerons d’abord chaque approche de conception d’API. Nous les comparerons ensuite en examinant plusieurs critères à prendre en compte avant d’en choisir un.

Enfin, comme il n’existe pas d’approche unique, nous verrons comment différentes approches peuvent être combinées sur différentes couches applicatives.

## REST API

Le transfert d'état représentatif (REST) ​​est le style architectural d'API le plus couramment utilisé dans le monde. Il a été défini en 2000 par Roy Fielding.

- Style architectural
REST n'est pas un framework ou une bibliothèque mais un style architectural décrivant une interface basée sur la structure URL et le protocole HTTP. Il décrit une architecture sans état, pouvant être mise en cache et basée sur des conventions pour l'interaction client-serveur. Il utilise des URL pour adresser les ressources appropriées et des méthodes HTTP pour exprimer l'action à entreprendre :

GET sert à récupérer une ressource existante ou plusieurs ressources
POST est utilisé pour créer une nouvelle ressource
PUT permet de mettre à jour une ressource ou de la créer si elle n'existe pas
DELETE est utilisé pour supprimer une ressource
PATCH est utilisé pour mettre à jour partiellement une ressource existante
REST peut être implémenté dans différents langages de programmation et prend en charge plusieurs formats de données tels que JSON et XML.

- Avantages et inconvénients
Le plus grand avantage de REST est qu’il s’agit du style architectural d’API le plus mature du monde de la technologie. En raison de sa popularité, de nombreux développeurs connaissent déjà REST et trouvent qu'il est facile de travailler avec. Cependant, en raison de sa flexibilité, REST peut être interprété différemment selon les développeurs.

Étant donné que chaque ressource est généralement située derrière une URL unique, il est facile de surveiller et de limiter le débit des API. REST simplifie également la mise en cache en tirant parti de HTTP. En mettant en cache les réponses HTTP, notre client et notre serveur n'ont pas besoin d'interagir constamment les uns avec les autres.

REST est sujet à la fois à une récupération insuffisante et excessive. Par exemple, pour récupérer des entités imbriquées, nous devrons peut-être effectuer plusieurs requêtes. D’un autre côté, dans les API REST, il n’est généralement pas possible de récupérer uniquement une donnée d’entité spécifique. Les clients reçoivent toujours toutes les données que le point de terminaison demandé est configuré pour renvoyer.

## GraphQL

GraphQL est un langage de requête open source pour les API développé par Facebook.

- Style architectural
GraphQL fournit un langage de requête pour développer des API avec un cadre pour répondre à ces requêtes. Il ne s’appuie pas sur les méthodes HTTP pour manipuler les données et utilise principalement uniquement le POST. En revanche, GraphQL utilise des requêtes, des mutations et des abonnements :

Les requêtes sont utilisées pour demander des données au serveur
Les mutations sont utilisées pour modifier les données sur le serveur
Les abonnements sont utilisés pour obtenir des mises à jour en direct lorsque les données changent
GraphQL est axé sur le client, car il permet à ses clients de définir exactement les données dont ils ont besoin pour un cas d'utilisation particulier. Les données demandées sont ensuite récupérées du serveur en un seul aller-retour.

- Avantages et inconvénients
GraphQL est très flexible envers ses clients, car il permet de récupérer et de fournir uniquement les données demandées. Comme aucune donnée inutile n'est envoyée sur le réseau, GraphQL peut conduire à de meilleures performances.

Il utilise une spécification plus stricte par rapport à l'ambiguïté de REST. De plus, GraphQL fournit des descriptions détaillées des erreurs à des fins de débogage et génère automatiquement une documentation sur les modifications de l'API.

Étant donné que chaque requête peut être différente, GraphQL interrompt la mise en cache intermédiaire du proxy, ce qui rend les implémentations de mise en cache plus difficiles. De plus, étant donné qu'une requête GraphQL peut potentiellement exécuter une opération côté serveur volumineuse et complexe, les requêtes sont souvent limitées par la complexité pour éviter de surcharger le serveur.

## gRPC

RPC signifie appel procédural à distance et gRPC est un framework RPC open source hautes performances créé par Google.

- Style architectural
Le framework gRPC est basé sur un modèle client-serveur d'appels de procédures distantes. Une application client peut appeler directement des méthodes sur une application serveur comme s'il s'agissait d'un objet local. Il s'agit d'une approche stricte basée sur un contrat dans laquelle le client et le serveur doivent accéder à la même définition de schéma.

Dans gRPC, un DSL appelé langage tampon de protocole définit les requêtes et les types de réponses. Le compilateur de tampon de protocole génère ensuite les artefacts de code serveur et client. Nous pouvons étendre le code serveur généré avec une logique métier personnalisée et fournir les données de réponse.

Le framework prend en charge plusieurs types d'interactions client-serveur :

Interactions demande-réponse traditionnelles
Streaming sur serveur, où une requête du client peut donner lieu à plusieurs réponses
Streaming client, où plusieurs demandes du client aboutissent à une réponse unique
Les clients et les serveurs communiquent via HTTP/2 en utilisant un format binaire compact qui rend l'encodage et le décodage des messages gRPC très efficaces.


- Avantages et inconvénients
Le plus grand avantage de gRPC réside dans les performances, rendues possibles par un format de données compact, un codage et un décodage rapides des messages et l'utilisation de HTTP/2. En outre, sa fonction de génération de code prend en charge plusieurs langages de programmation et nous permet de gagner du temps en écrivant du code passe-partout.

En exigeant HTTP 2 et TLS/SSL, gRPC offre de meilleurs paramètres de sécurité par défaut et une prise en charge intégrée du streaming. La définition indépendante du langage du contrat d'interface permet la communication entre des services écrits dans différents langages de programmation.

Cependant, pour le moment, gRPC est beaucoup moins populaire que REST dans la communauté des développeurs. Son format de données est illisible pour les humains, des outils supplémentaires sont donc nécessaires pour analyser les charges utiles et effectuer le débogage. De plus, HTTP/2 n'est pris en charge via TLS que dans les versions récentes des navigateurs modernes.

## Quelle API choisir?

Maintenant que nous connaissons les trois approches de conception d’API, examinons plusieurs critères à prendre en compte avant d’en choisir une.

- Format des données
REST est l'approche la plus flexible en termes de formats de données de requête et de réponse. Nous pouvons implémenter des services REST pour prendre en charge un ou plusieurs formats de données tels que JSON et XML.

D'un autre côté, GraphQL définit son propre langage de requête qui doit être utilisé lors de la demande de données. Les services GraphQL répondent au format JSON. Bien qu’il soit possible de convertir la réponse dans un autre format, cela est rare et peut affecter les performances.

Le framework gRPC utilise des tampons de protocole, un format binaire personnalisé. Il est illisible pour les humains, mais c'est aussi l'une des principales raisons qui rendent gRPC si performant. Bien que pris en charge dans plusieurs langages de programmation, le format ne peut pas être personnalisé.

- Récupération de données
GraphQL est l'approche API la plus efficace pour récupérer des données depuis le serveur. Comme il permet aux clients de sélectionner les données à récupérer, aucune donnée supplémentaire n'est généralement envoyée sur le réseau.

REST et gRPC ne prennent pas en charge ces requêtes client avancées. Ainsi, des données supplémentaires peuvent être renvoyées par le serveur à moins que de nouveaux points de terminaison ou filtres ne soient développés et déployés sur le serveur.

- Prise en charge du navigateur
Les API REST et GraphQL sont prises en charge dans tous les navigateurs modernes. En règle générale, le code client JavaScript est utilisé pour envoyer des requêtes HTTP du navigateur aux API du serveur.

La prise en charge du navigateur n'est pas prête à l'emploi pour les API gRPC. Cependant, une extension de gRPC pour le web est disponible. Il est basé sur HTTP 1.1., mais ne fournit pas toutes les fonctionnalités de gRPC. Semblable à un client Java, gRPC pour le Web nécessite que le code client du navigateur génère un client gRPC à partir d'un schéma de tampon de protocole.

- Génération de code
GraphQL nécessite l'ajout de bibliothèques supplémentaires à un framework principal comme Spring. Ces bibliothèques ajoutent la prise en charge du traitement des schémas GraphQL, de la programmation basée sur les annotations et de la gestion par le serveur des requêtes GraphQL. La génération de code à partir d'un schéma GraphQL est possible mais pas obligatoire. Tout POJO personnalisé correspondant à un type GraphQL défini dans le schéma peut être utilisé.

Le framework gRPC nécessite également l'ajout de bibliothèques supplémentaires à un framework principal, ainsi qu'une étape obligatoire de génération de code. Le compilateur de tampon de protocole génère le code passe-partout du serveur et du client que nous pouvons ensuite étendre. Dans le cas où nous utilisons des POJO personnalisés, ils devront être mappés aux types de tampons de protocole générés automatiquement.

REST est un style architectural qui peut être implémenté à l'aide de n'importe quel langage de programmation et de diverses bibliothèques HTTP. Il n'utilise aucun schéma prédéfini et ne nécessite aucune génération de code. Cela dit, utiliser Swagger ou OpenAPI nous permet de définir un schéma et de générer du code si nous le souhaitons.

- Temps de réponse
Grâce à son format binaire optimisé, gRPC a des temps de réponse nettement plus rapides que REST et GraphQL. De plus, l'équilibrage de charge peut être utilisé dans les trois approches pour répartir uniformément les demandes des clients sur plusieurs serveurs.

Cependant, gRPC utilise également HTTP 2.0 par défaut, ce qui rend la latence dans gRPC inférieure à celle des API REST et GraphQL. Avec HTTP 2.0, plusieurs clients peuvent envoyer plusieurs requêtes simultanément sans établir de nouvelle connexion TCP. La plupart des tests de performances indiquent que gRPC est environ 5 à 10 fois plus rapide que REST.

- Mise en cache
La mise en cache des requêtes et des réponses avec REST est simple et mature car elle permet la mise en cache des données au niveau HTTP. Chaque requête GET expose les ressources de l'application, qui peuvent facilement être mises en cache par le navigateur, les serveurs proxy ou les CDN.

Étant donné que GraphQL utilise la méthode POST par défaut et que chaque requête peut être différente, cela rend la mise en cache plus difficile. Cela est particulièrement vrai lorsque le client et le serveur sont géographiquement éloignés l'un de l'autre. Une solution possible à ce problème consiste à effectuer des requêtes via GET et à utiliser des requêtes persistantes précalculées et stockées sur le serveur. Certains services middleware GraphQL proposent également la mise en cache.

Pour le moment, la mise en cache des requêtes et des réponses n'est pas prise en charge par défaut dans gRPC. Cependant, il est possible d'implémenter une couche middleware personnalisée qui mettra en cache les réponses.

- Utilisation prévue
REST, un bon choix pour les domaines, peut être facilement décrit comme un ensemble de ressources par opposition à des actions. L'utilisation de méthodes HTTP permet des opérations CRUD standard sur ces ressources. En s'appuyant sur la sémantique HTTP, il est intuitif pour ses appelants, ce qui en fait un bon choix pour les interfaces publiques. Une bonne prise en charge de la mise en cache pour REST le rend adapté aux API ayant des modèles d'utilisation stables et des utilisateurs géographiquement répartis.

GraphQL convient parfaitement aux API publiques où plusieurs clients nécessitent différents ensembles de données. Par conséquent, les clients GraphQL peuvent spécifier les données exactes qu'ils souhaitent via un langage de requête standardisé. C'est également un bon choix pour les API qui regroupent des données provenant de plusieurs sources et les transmettent ensuite à plusieurs clients.

Le framework gRPC convient parfaitement au développement d'API internes avec des interactions fréquentes entre les microservices. Il est couramment utilisé pour collecter des données auprès d'agents de bas niveau tels que différents appareils IoT. Cependant, sa prise en charge limitée par les navigateurs rend difficile son utilisation dans les applications Web destinées aux clients.

## Mélanger et assortir
Chacun des trois styles architecturaux API présente ses avantages. Cependant, il n’existe pas d’approche unique, et l’approche que nous choisirons dépendra de notre cas d’utilisation.

Nous n’avons pas à faire un seul choix à chaque fois. Nous pouvons également mélanger et assortir différents styles dans notre architecture de solution :

Exemple d'architecture utilisant REST, GraphQL et gRPC
Dans l'exemple de diagramme d'architecture ci-dessus, nous avons démontré comment différents styles d'API peuvent être appliqués dans différentes couches d'application.

## Conclusion
Dans ce document, nous avons exploré trois styles architecturaux populaires pour la conception d'API Web : REST, GraphQL et gRPC. Nous avons examiné chaque cas d'utilisation de style différent et décrit ses avantages et ses compromis.

 Nous les avons comparés en examinant plusieurs critères à prendre en compte avant de décider d’une approche. Enfin, comme il n’existe pas d’approche unique, nous avons vu comment nous pouvions mélanger et assortir différentes approches dans différentes couches d’application.