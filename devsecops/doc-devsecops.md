# Documentation devsecops

## Introduction
DevSecOps, également appelé Secure DevOps, s’appuie sur la pratique de DevOps en incorporant la sécurité à différentes étapes d’un cycle de vie DevOps traditionnel. Voici quelques-uns des avantages de l’établissement de la sécurité dans les pratiques DevOps :

- Rend vos applications et systèmes plus sûrs en offrant une visibilité sur les menaces de sécurité et en empêchant les vulnérabilités d’atteindre les environnements déployés
- Augmente la sensibilisation à la sécurité avec vos équipes de développement et d’exploitation
- Incorpore des processus de sécurité automatisés dans votre cycle de vie de développement de logiciels
- Réduit les coûts de correction en recherchant les problèmes de sécurité au début des phases de développement et de conception


## Flux de processus

1. Azure Active Directory (Azure AD) est configuré comme fournisseur d’identité pour GitHub. Configurez l’authentification multifacteur (MFA) pour fournir une sécurité d’authentification supplémentaire.
2. Les développeurs utilisent Visual Studio Code ou Visual Studio avec des extensions de sécurité activées pour analyser de manière proactive leur code à la recherche de failles de sécurité.
3. Les développeurs valident le code de l’application dans un référentiel GitHub Enterprise détenu et régi par l’entreprise.
4. GitHub Enterprise intègre une analyse automatique de la sécurité et des dépendances via GitHub Advanced Security.
5. Les demandes de tirage déclenchent des builds d’intégration continue (CI) et des tests automatisés
6. Le workflow de build CI via GitHub Actions génère une image conteneur Docker stockée dans Azure Container Registry.
7. Vous pouvez introduire des approbations manuelles pour les déploiements dans des environnements spécifiques, comme la production, dans le cadre du workflow de livraison continue (CD) dans GitHub Actions.
8. GitHub Actions active le déploiement continu sur AKS. Utilisez GitHub Advanced Security pour détecter les secrets, les informations d’identification et d’autres informations sensibles dans vos fichiers de configuration et de source d’application.
9. Microsoft Defender est utilisé pour analyser Azure Container Registry, le cluster AKS et Azure Key Vault à la recherche de failles de sécurité.
- Microsoft Defender pour les conteneurs analyse l’image conteneur à la recherche de failles de sécurité connues lors de son chargement dans Container Registry.
- Vous pouvez également utiliser Defender pour les conteneurs pour effectuer des analyses de votre environnement AKS et fournir une protection contre les menaces au moment de l’exécution de vos clusters AKS.
- Microsoft Defender pour Key Vault détecte les tentatives dangereuses et inhabituelles d’accès aux comptes Key Vault.
10. Azure Policy peut être appliqué à Container Registry et Azure Kubernetes Service (AKS) pour la conformité et l’application des stratégies. Les stratégies de sécurité courantes pour Container Registry et AKS sont intégrées pour une activation rapide.
11. Azure Key Vault est utilisé pour injecter en toute sécurité des secrets et des informations d’identification dans une application au moment de l’exécution, en tenant les informations sensibles hors de portée des développeurs.
12. Le moteur de stratégie réseau AKS est configuré pour vous aider à sécuriser le trafic entre les pods d’application à l’aide de stratégies réseau Kubernetes.
13. La surveillance continue du cluster AKS peut être configurée à l’aide d’Azure Monitor et de Container Insights pour ingérer les métriques de performances et analyser les journaux d’application et de sécurité.
- Container Insights récupère les métriques de performances et les journaux d’application et de cluster.
- Les journaux de diagnostic et d’application sont extraits dans un espace de travail Azure Log Analytics pour exécuter des requêtes de journal.
14. Microsoft Sentinel, qui est une solution SIEM (Gestion des informations et des événements de sécurité), peut être utilisé pour ingérer et analyser plus en détail les journaux de cluster AKS à la recherche de menaces de sécurité en fonction de modèles et de règles définis.
15. Des outils open source comme celui d’Open Web Application Security Project (OWASP ZAP) peuvent être utilisés pour effectuer des tests d’intrusion pour les applications et les services web.
16. Defender pour DevOps, un service disponible dans Defender pour le cloud, permet aux équipes de sécurité de gérer la sécurité DevOps dans les environnements à plusieurs pipelines, dont GitHub et Azure DevOps.


## Phases du cycle de vie DevSecOps
Les contrôles de sécurité sont implémentés dans chaque phase du cycle de vie du développement logiciel . Cette implémentation est un élément clé d’une stratégie DevSecOps et de l’approche shift-left.


### Phase de planification
La phase de planification a généralement le moins d’automatisation, mais elle a des implications de sécurité importantes qui ont un impact significatif sur les phases de cycle de vie DevOps ultérieures. Cette phase implique la collaboration entre les équipes de sécurité, de développement et d’exploitation. L’inclusion des parties prenantes de la sécurité dans cette phase de conception et de planification garantit que les exigences de sécurité et les problèmes de sécurité sont correctement pris en compte ou atténués.

-Meilleure pratique : Concevoir une plateforme d’application plus sécurisée
La création d’une plateforme hébergée par AKS plus sécurisée est une étape importante pour garantir que la sécurité est intégrée au système à chaque couche, en commençant par la plateforme elle-même

-Meilleure pratique : Intégrer la modélisation des menaces dans votre processus
La modélisation des menaces est généralement une activité manuelle qui implique des équipes de sécurité et de développement. Elle est utilisée pour modéliser et rechercher des menaces au sein d’un système afin que les vulnérabilités puissent être traitées avant tout développement de code ou modification d’un système.
Nous vous recommandons d’utiliser le modèle de menace STRIDE
- Meilleure pratique : Appliquer Azure Well Architect Framework (WAF)

### Phase de développement
Le « Shift Left » est un principe clé de l’esprit DevSecOps. Ce processus commence avant même que le code soit validé dans un référentiel et déployé via un pipeline.

Meilleure pratique : Appliquer des normes de codage sécurisé
En utilisant les meilleures pratiques et listes de contrôle de codage sécurisées établies, vous pouvez protéger votre code contre les vulnérabilités courantes, comme l’injection et la conception non sécurisée. La fondation OWASP publie des recommandations de codage sécurisé standard que vous devriez adopter lors de l’écriture de code

Vous devez également examiner les pratiques de codage sécurisées pour vos runtimes de langage 

Vous pouvez appliquer des normes de journalisation pour protéger les informations sensibles contre les fuites dans les journaux d’application

Meilleure pratique : Utiliser des outils et plug-ins d’IDE pour automatiser les vérifications de sécurité

SonarLint est un plug-in d’IDE qui analyse automatiquement votre code à la recherche d’erreurs de programmation courantes et de problèmes de sécurité potentiels.
Le plug-in Synk est axé sur des éléments spécifiques à la sécurité, comme les 10 principales vulnérabilités courantes d’OWASP ,analyse la source de votre application et les dépendances tierces, et vous avertit si des vulnérabilités sont détectées.
Le plug-in SARIF (Static Analysis Results Interchange Format) vous permet d’afficher facilement les vulnérabilités des outils de test de sécurité des applications statiques (SAST)

Meilleure pratique : Établir des contrôles sur vos référentiels de code source
Établissez une méthodologie de branchement pour une utilisation cohérente des branches au sein de l’entreprise

Meilleure pratique : Sécuriser vos images conteneur
Utilisez des images légères avec un encombrement minimal du système d’exploitation pour réduire la surface d’attaque globale. Considérez des images minimales comme Alpine ou même des images distroless qui contiennent uniquement votre application et son runtime associé

Empêchez l’accès/le contexte utilisateur racine pour une image. Par défaut, les conteneurs s’exécutent en tant que racine.

Trivy est un exemple d’outil open source que vous pouvez utiliser pour analyser les vulnérabilités de sécurité au sein de vos images conteneur.

### Phase de build
Pendant la phase de build, les développeurs travaillent avec les ingénieurs de fiabilité du site et les équipes de sécurité pour intégrer des analyses automatisées de leur source d’application dans leurs pipelines de build CI.

Meilleure pratique : Effectuer une analyse du code statique (SAST) pour détecter les vulnérabilités potentielles dans le code source de votre application

Meilleure pratique : effectuer une analyse des secrets pour empêcher l’utilisation frauduleuse de secrets qui ont été accidentellement validés dans un référentiel:
Pour Azure DevOps, Defender for Cloud utilise l’analyse des secrets pour détecter des informations d’identification, des secrets, des certificats et d’autres contenus sensibles dans votre code source et votre sortie de build.

Meilleure pratique : Utiliser des outils d’analyse de la composition logicielle (SCA) pour suivre les composants open source dans le codebase et détecter les vulnérabilités dans les dépendances

Dependabot effectue une analyse pour détecter des dépendances vulnérables, et envoie des alertes

Meilleure pratique : Activer les analyses de sécurité des modèles d’infrastructure en tant que code (IaC) pour réduire les erreurs de configuration du cloud qui atteignent les environnements de production
Surveillez de façon proactive les configurations des ressources cloud tout au long du cycle de vie du développement.
Microsoft Defender pour DevOps prend en charge les référentiels GitHub et Azure DevOps.

Meilleure pratique : Analyser vos images dans les registres de conteneurs pour identifier les vulnérabilités connues

Meilleure pratique - Générer automatiquement les nouvelles images sur la mise à jour de l’image de base
Azure Container Registry Tasks découvre dynamiquement les dépendances des images de base lorsqu’il génère une image conteneur. Par conséquent, il peut détecter quand l’image de base d’une image d’application est mise à jour. Avec une tâche de build préconfigurée, Container Registry est capable de reconstruire automatiquement chacune des images d’application faisant référence à l’image de base.

Meilleure pratique : Utiliser Container Registry, Azure Key Vault et la notation pour signer numériquement vos images conteneur et configurer un cluster AKS pour autoriser uniquement les images validées
Azure Key Vault est utilisé pour stocker une clé de signature qui peut être utilisée par notation avec le plug-in de notation Key Vault (azure-kv) pour signer et vérifier les images conteneur et d’autres artefacts. Container Registry vous permet d’attacher ces signatures à l’aide des commandes Azure CLI.
Les conteneurs signés permettent aux utilisateurs d’assurer que les déploiements sont générés à partir d’une entité approuvée et que l’artefact n’a pas été falsifié depuis sa création

**Ratify** permet aux clusters Kubernetes de vérifier les métadonnées de sécurité des artefacts avant le déploiement et d’admettre pour le déploiement uniquement les éléments conformes à une stratégie d’admission que vous créez

### Phase de déploiement
Pendant la phase de déploiement, les développeurs, opérateurs d’application et équipes d’opérateurs de cluster travaillent ensemble à l’établissement des contrôles de sécurité appropriés pour les pipelines de déploiement continu (CD) afin de déployer du code dans un environnement de production de manière plus sécurisée et automatisée.

Meilleure pratique : Contrôler l’accès et le workflow du pipeline de déploiement
Vous pouvez protéger les branches importantes en définissant des règles de protection de branche.
En utilisant des environnements pour le déploiement, vous pouvez configurer des environnements avec des règles de protection et des secrets.
Vous pouvez tirer parti de la fonctionnalité Approbations et Portes pour contrôler le workflow du pipeline de déploiement. 

Meilleure pratique : Sécuriser les informations d’identification de déploiement

En utilisant des environnements pour le déploiement, vous pouvez configurer des environnements avec des règles de protection et des secrets.
Une approche du CI/CD basée sur le tirage avec GitOps vous permet de déplacer les informations d’identification de sécurité vers votre cluster Kubernetes. En évitant de stocker les informations d’identification dans vos outils de CI externes

Meilleure pratique : Exécuter des tests de sécurité des applications dynamiques (DAST) pour rechercher des vulnérabilités dans votre application en cours d’exécution

Utilisez des outils open source tels que OWASP ZAP pour effectuer des tests d’intrusion pour détecter les vulnérabilités courantes des applications web

Meilleure pratique : Déployer des images conteneur à partir de registres approuvés uniquement
Utilisez Defender pour les conteneurs pour activer le module complémentaire Azure Policy pour Kubernetes.
Activez Azure Policy afin que les images conteneur ne puissent être déployées qu’à partir de registres approuvés.


### Phase d’exploitation
Au cours de cette phase, des tâches de surveillance des opérations et de surveillance de la sécurité sont effectuées pour surveiller, analyser et signaler de manière proactive les incidents de sécurité potentiels. Des outils d’observabilité de production comme Azure Monitor et Microsoft Sentinel sont utilisés pour surveiller et garantir la conformité aux normes de sécurité de l’entreprise.

Meilleure pratique : Utiliser Microsoft Defender pour le cloud afin d’activer l’analyse et la surveillance automatisées de vos configurations de production

Meilleure pratique : Maintenir vos clusters Kubernetes à jour

De nouvelles versions de Kubernetes sont fréquemment déployées. Il est important d’avoir une stratégie de gestion du cycle de vie en place pour vous assurer de ne pas prendre du retard et de ne pas perdre la prise en charge

Les nœuds Worker AKS doivent être mis à niveau plus fréquemment. Des mises à jour du système d’exploitation et du runtime sont fournis d'une façon hebdomadaire 


Meilleure pratique : Utiliser Azure Policy pour sécuriser et régir vos clusters AKS
Après l’installation du module complémentaire Azure Policy pour AKS, vous pouvez appliquer des définitions de stratégie individuelles ou des groupes de définitions de stratégie appelés initiatives (aussi appelées ensembles de stratégies) à votre cluster.
Utilisez des stratégies Azure intégrées pour des scénarios courants, comme l’interdiction de l’exécution de conteneurs privilégiés ou l’approbation d’adresses IP externes autorisées uniquement. Vous pouvez également créer des stratégies personnalisées pour des cas d’usage spécifiques.
Appliquez des définitions de stratégie à votre cluster et vérifiez que ces attributions sont appliquées.
Utilisez Gatekeeper pour configurer un contrôleur d’admission qui autorise ou refuse les déploiements en fonction des règles spécifiées. Azure Policy étend Gatekeeper.

Meilleure pratique : Utiliser Azure Monitor pour la surveillance et les alertes continues
Utilisez Azure Monitor pour collecter les journaux et métriques à partir d’AKS. Vous obtenez des insights sur la disponibilité et les performances de votre application et de votre infrastructure. Il vous donne également accès aux signaux permettant de surveiller l’intégrité de votre solution et d’épingler rapidement toute activité anormale.

Meilleure pratique : Utiliser Microsoft Defender pour le cloud pour la surveillance active des menaces
Microsoft Defender pour le cloud fournit un monitoring actif des menaces sur AKSe au niveau du nœud (menaces de machine virtuelle) et des éléments internes.
Defender pour Key Vault peut être utilisé pour détecter les tentatives inhabituelles et suspectes d’accès aux comptes de coffre de clés et peut alerter les administrateurs en fonction de la configuration.
Defender pour les conteneurs peut alerter en cas de vulnérabilités détectées dans vos images conteneur stockées sur Container Registry.

Meilleure pratique : Activer la surveillance centralisée des journaux et utiliser des produits SIEM pour surveiller les menaces de sécurité en temps réel
Connectez les journaux de diagnostic AKS à Microsoft Sentinel pour une surveillance centralisée de la sécurité basée sur les modèles et les règles. Sentinel permet cet accès en toute transparence via des connecteurs de données.

Meilleure pratique : Activer les diagnostics sur vos ressources Azure
En activant les diagnostics Azure sur toutes les ressources de votre charge de travail, vous avez accès aux journaux de plateforme qui fournissent des informations de diagnostic et d’audit détaillées pour vos ressources Azure. 