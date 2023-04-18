# Devoxx France 2023 - 11ème édition
12/04/2023 - 14/04/2023

2 Place de la Porte Maillot, 75017 Paris, France

## Thèmes
- **Cloud, Containers & Infrastructure, DevOps**
- Java, JVM
- Big Data, Machine Learning, Analytics
- Languages
- **Architecture, Performance and Security**
- **People & Culture, Soft Skills, Future of work, Innovation, Side projects**
- Web, JS, HTML5 & UX

## Mercredi 12 avril 2023

### 08h00-09h30 : Registration and breakfast
### 09h30-12h30 (Amphi Bleu) : Une Architecture GitOps from scratch : Gitlab, Ansible, Terraform, Kubernetes et AWS
#### Abstract
##### Talk
On a compris : Kubernetes permet de gérer les ressources applicatives.

Mais du coup, comment bien utiliser l'Infrastructure-As-Code ? Comment provisionner mon socle technique ? Combien de clusters dois-je utiliser ? Qu'est-ce que j'utilise dans ma CI pour mon infra ? Quid de la zone grise (DNS, Storage, DBs etc…) qui touche à la fois à de l’infra et à de l’applicatif ? Où mettre mon observabilité ? Comment bien gérer une infra multi-cluster ?

Dans ce talk, on va vraiment aborder les aspects importants du day-2.

On vous propose de mettre ensemble en place une architecture GitOps avec Kubernetes, Terraform, Helm et ArgoCD (notamment) dans un SI pas trop compliqué (pas de multi-régions, pas de multi-tenancy, pas de cluster on-premises).

Le mieux dans tout ça ? On part de zéro, on expliquera en détail toutes les étapes et on va aussi faire pas mal de live-coding.

Les participants pourront repartir avec une grille d’analyse end-to-end (CI / Infra-as-code / Templating / Managed services), et un joli repo tout propre qu'ils pourront rapidement adapter et mettre en place dans leur projet !

##### Speakers
**Loïc Ortola**

Quand Loïc n’est pas en train de militer pour qu’on ne copie-colle pas les réponses de StackOverflow sans regarder la doc, il gère les DevRel en tant que CTO de Takima.

Passionné de transmission, il aime aller au fond et partager ses trouvailles sur des sujets trendy pour faire gagner du temps à ceux qui se lancent, parce que ça, on le trouve pas sur Stack Overflow.

Rudement impliqué dans l'architecture et le DevOps, il est formateur Kubernetes et GitOps. Après avoir lancé Jawg Maps, Frog Connexion, Notion Automations et quelques libs Opensource, il revient au Devoxx pour le début des beaux jours !

**Aurélien Moreau**

Aurélien n’est pas juste un adepte de la marinière. Ops passionné, ayant fait ses armes dans les équipes infra chez EDF, il a vu passer 10 ans d’innovations et a vécu les changements de paradigmes engendrés par l’avènement des outils DevOps. Formateur Kubernetes en France et à l’international, il mobilise son expertise sur des chantiers de migration de grands acteurs vers le Cloud Native. Il est aujourd’hui Head of Infrastructure chez Takima et saura vous donner une lecture de ce que le DevOps a changé dans sa manière de fonctionner au quotidien. Spoiler alert : maintenant, il code !

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | University |
| Track | Cloud, Containers & Infrastructure, DevOps |
| Presentation level | beginner/novice |
| Keywords | Kubernetes DevOps GitOps |

#### Notes

On va monter une infra GitOps complete ensemble en 3H

On part sur une archi 3 tiers (DB en PostgreSQL, API en Java SpringBoot, Front en Angular), qui soit scalable
On va parler multi-environnement, avec le stagging et un cluster tech pour observability et control des autres environnements
On va mettre tout ça dans Gitlab

En amont, on a créé un compte AWS et acheté un domaine chez Gandi (importé dans Route 53, le DNS AWS)

##### Crash Course - OPS 101
###### Infrastructure as Code
Il faut des serveurs pour pouvoir hoster ses applicatifs.
Avant on gérait son propre hardware dans les datacenter, maintenant on loue de l'infra directement chez des providers. On reste bas niveau

Provisioning d'infrastructure

###### Conf
On configure l'infra

Configuration Management

##### Orchestration de l'app
Deployer, gérer sa conf, cycle devie et publication


Infrastructure as code = faire ces 3 étapes sous forme de code

Provisioning d'infra => **Terraform**

Configuration Mgt => **Ansible**

Orchestration de l'app => **Helm**

##### Gitops
Git va être la source de vérité de tous mes chantiers, de toutes mes confs car c'est du code qui est versionné sur Git

Avant Docker => on devait dev, puis créer d'un war, et on le file aux OPS. Les OPS provisionnaient, compilaient / déployaient les artefacts sur les serveurs applicatifs qui allaient bien.
Les soucis entre devs et OPS ont fini par créer des organisations très silotés, avec de fortes frictions. D'où l'idée de mettre le runtime au milieu du problème et donc de mettre la conf avec les éléments de devs.

==L'artifact est donc maintenant l'image Docker => elle contient l'artefact ET le runtime==

Docker Compose c'est cool pour lancer des stacks completes, mais ça ne suffit pas à l'OPS pour aller en PROD. Il manque des répliques des API, de faire du load balancing et compagnie.
Il manque beaucoup de choses pour être "production ready".
Il faut donc **Kubernetes**

L'orchestrateur (Kubernetes) permet de gérer de manière simple les run / déploiement dans un environnement hyper-distribué

Les devs ont une responsabilité supplémentaire maintenant : ils sont responsables du code ET du runtime jusqu'à la PROD.

Kubernetes => Making it easier to manage applications

Kubernetes
Kube-api => porte d'entrée pour aller communiquer avec un Cluster Kubernetes (en API REST authentifiée JWT)

**Dans Kubernetes tout est ressource**. On va les décrire le plus souvent au format *YAML*, en décrivant l'état attendu. Le cluster va ensuite l'appliquer
Quand on fait de l'infra, on attend un état spéficique de machine. K8S va servir à cela

**Les ressources de base :**
- **POD**
- **ReplicaSet**
- **Deployment**

Elle est là pour expliquer les différences d'états (passage entre une V1 et V2 par exemple). Une super ressource pour gérer les cycles de l'appli, pouvoir faire des rollbacks, ...
Elle contient le POD et ReplicaSet (qu'on ne fait donc jamais à la main, juste des Deployment)

On va vouloir configurer et paramétrer les applicatifs que l'on fait tourner.
Les ConfigMaps et les Secrets permettent de faire ça, car ils sont consomés par les PODS
- **ConfigMap** = code en clair
- **Secret** = encodé en base64 (pas fou mais mieux que rien)
On peut surtout les séparer en terme de droits d'accès et autres éléments de sécurité

On veut publier : on va utiliser les **Services** (le load balancer de Kubernetes)

**Ingress** : pour publier sur internet. Il s'agit d'une implémentation d'un reverse proxy
Mais l'Ingress ne fonctionne pas tout seul, on a besoin d'autres outils pour gérer des éléments spécifique (tel le certificat TLS, que K8S ne sait pas faire en natif)

**Namespace** : un regroupement de ressources, pour organiser logiquement (ou non) nos ressources

##### Deployer une infra avec un Cluster K8S
EKS => Kubernetes As a Service managé par AWS

AZ => Availability Zone. Chez AWS c'est un datacenter

Terraform => solution de HashiCorp ([site](https://www.terraform.io/))

Seulement passé en V1 il y a 2 ans.
Il dispose d'un langage déclaratif propre (le *HCL*). Fonctionne par modules et par état (State)

Gitlab CI/CD pour l'automatisation

##### Terraform

On va décrire l'infra que l'on souhaite avoir => décrire les ressources que l'on veut avoir chez notre provider

On va déclarer le provider chez qui on va déployer notre infra

On va déclarer les ressources que l'on veut instancier. Format en snake case

On ne va pas tout hardcoder, on utilise du code ! On va passer par des variables, que l'on va fournir suivant les environnements ou un fichier `.tvars` ou encore définir avec une commande

On va également avoir des variables output, des variables de sortie de l'infra (comme l'IP publique du serveur par exemple)
On va avoir un fichier `.tfstate` qui contient toutes les informations de l'infra une fois déployée

On va utiliser des modules (soit communautaires soit faits maison) pour faire des tâches spéficiques (sécurité, worker nodes, ...). Avec des variables inputs et outputs.

On lance :
- **init**
- **plan**
- **apply**
- **destroy**

:warning: les AMI sont régionnalisés chez AWS :warning:

Pour un seul miniserveur, le `.tfstate` est ultra ultra verbeux. C'est pour ça qu'on va utiliser des variables input et output

Terraform va lire tous les fichiers `.tf` comme un seul bloc. Donc les noms de fichiers on s'en fiche, c'est pour nous

Terraform a des modules VMWare, donc on peut tout à fait faire du cloud privé ou on premise sans soucis.
L'Infra As Code n'est pas réservé aux clouds providers. On peut tout à fait en faire en interne

Les variables d'output permettent de faire passer des info d'un module à l'autre. Donc très utile pour chaîner des modules

Le `.tfstate` va être stocké sur un endroit sécure et répliqué, car on ne veut pas perdre ces infos

##### Configurer mon socle technique

C'est du config management => on va utiliser **Ansible** (plateforme pour configurer et manager des serveurs)

On a un control node sur lequelle tourne Ansible et ses modules. Il va déployer / configurer des trucs sur des nodes.
Pour ça, on utilise un **Inventory** (inventaire des noeuds), les groupes de noeuds, ...
Mais on est sur K8S, on ne va pas faire de SSH, on va taper des API K8S pour effectuer des actions

On va utiliser des **Playbooks** (dans Ansible) afin de réaliser les installations. Il s'agit d'une procédure d'installation technique

**Je dpéloie un playbook dans un inventory**

Chaque chapitre est un **role**. Il est composé de **tasks** qui sont des commandes
Dans une task on vise un état attendu, en utilisant des modules Ansible

##### Petite projection dans le futur
On se rend compte qu'une ressource est un peu limite en terme de résilience.

On va augmenter le ReplicaSet, via un commit.

Un applicatif va reprérer le nouveau commit, va le récupérer, voir les modifs et donc délencher la création de pods supplémentaires pour replication
Si je veux faire un rollback => git revert

Là on a recréé un cluster qui existait, mais on peut aussi faire des clusters tous neufs sur d'autres infras.

##### PAUSE

##### REPRISE

On va utiliser **HELM** pour faire les installations

Ansible commence là où Terraform s'arrête

Le `.tfstate` va me permettre de chopper mes variables kubeconfig
Le Ansible va donc chopper tout ça pour pour mettre en route ses playbook sur les infras générées par Terraform

*Artifact-hub* site qui permet de lister les artifacts HELM dont on peut se servir pour faire nos tâches
Je vais créer un module Ansible qui va permettre en mode déclaratif de faire les tâches que l'on veut via les artifacts

On peut créer des *group_vars* pour stocker les variables dans les inventories

:warning: **penser à créer un *playbook* pour détruire tout !** :warning:

On a mis à jour notre pipeline, pour rajouter des steps et mettre à jour le step de destroy (intégration du destroy Ansible)

Notre Cluster fonctionne et est prêt à l'emploi maintenant

On pourrait tout faire dans un seul outil (Ansible et/ou Terraform) car ils en sont techniquement capables MAIS
- Terraform est là pour gérer des ressources, pas les tâches et inventaires
- Ansible il fait des tâches, mais n'a pas la notion des `tfstates`

##### Deployer mes APPS

Dans le cadre du déploiement, on ne va pas utiliser les éléments K8S. On va utiliser les variables globales. On va même injecter des variables sur le pod lui même car ce sera utilisé par l'applicatif

Dans K8S on ne remplace pas des pods. On kill les anciens et on en créé des nouveaux.

La DB choisie est PostgreSQL, on veut qu'elle soit réppliquée et persistante.
On ne veut pas qu'elle reparte de 0 chaque fois qu'on modifie la ressource, qu'on la redéploie

Pour gérer la persistance, on va consommer des disques AWS qui seront montés dans les pods qui vont bien.

Dans K8S on a une ressource pour créer des ressources.
Un Pattern pour étendre des fonctionnalités de K8S => c'est un **Operator**
Il s'appuie sur deux elems :
- des CRDS
- ?

On va donc pouvoir créer un Operator PostgreSQL et le définir pour qu'il soit Production Ready.
On a donc un vrai DB-as-a-service dispo, gratuitement, et utilisable facilement

On a tout fait sur notre repo Git, en modifiant les fichiers. On a commité et mis à jour notre pipeline. :fire:

On va créé un nouveau repo qui va nous permettre de stocker les différentes apps

K8S rend effortless l'orchestration applicative
Les équipes de devs ont la main pour créer et deployer des ressources

**On veut être indépendants des vendors et donc via K8S on va chercher à utiliser des ressources génériques et non spécifiques aux vendors**

##### Plusieurs environnements

On va devoir faire évoluer notre infra as code pour refléter ces différents environnements
On va duppliquer les `.tfvars` pour faire PROD et STAGING par exemple
On aura des `.tfstage` pour chqaue env
De même pour les *inventory*

Mais les noms de domaines ?

On a besoin de templater ces ressources, via HELM (package manager + moteur de template)

**Chart HELM**

On va mettre les fichiers de ressources dans le dossier *templates*
On va avoir des variables d'envs par envs, avec à chaque fois un fichier

Du coup, maintenant ça va être très facile de créer de nouveaux envs (devs, patch, ...)

Côté Pipeline, même chose qu'avant, sauf qu'on gère les différents environnements par rapport à avant.
On a également un `.tfstate` par environnement maintenant

##### LOGS

On ne va pas mettre nos logs dans ces environnements qui sont là pour faire tourner les applis.
On va créer un nouvel environnement, un environnement technique pour centraliser toutes les applis et outils OPS (`tech.tfvars`)

On ne va pas installer les mêmes outils, Du coup on va créer un nouveau playbook Ansible pour mettre des rôles, des outils différents. Ou certains en commun avec les autres envs

**Le cluster tech va servir à manager les autres clusters**

Via **Rancher** je peux gérer les droits des users, les accès aux projets. Et mes users ont accès aux logs, à un shell, et autres. Ils sont totalement indépendants, tout en étant dans un environnement sécurisé, sur lesquels ont peut donner des accès ou non de manière très fine

Dans Rancher il y a également des dashboards *grafana* afin de voir les consommations mémoire, disk, ...
Pour les logs, on utilise un operator Elastic Search ou Kibana, afin de centraliser le tout

:warning: Attention, aaujourd'hui on est sur la magie de K8S. Il faut une maîtrise de la stack pour résoudre les problèmes quotidiens, tels les bugs logiciels. On veut se faciliter la vie, gagner du temps, mais on ne vit pas dans un monde parfait.

##### Déploiement des APPS

La partie applicative vit beaucoup plus souvent que l'infra, donc on va utiliser **Argo CD** pour s'assurer entre les specs de l'infra et l'état actuel de l'infra (car des actions manuelles sur l'infra peuvent la faire dévier de son état prévu)

On connecte nos clusters à Argo et notre repo Git qui contient la définition de l'infra. Tout ça via Ansible bien sûr
On lui a donné les droits pour également créer / Modifier des ressources.

ArgoCD fournit une ressource pour scripter tous les déploiements afin d'éviter le click o drome

Git est devenu notre source de vérité pour l'infra, et pour les applicatifs


**En Infra on ne met pas sa prod dans un outil qu'on ne maîtrise pas**

:rotating_light:**Il faut se faire former pour K8S, pas d'auto formation**:rotating_light:

On n'a ici vu que la partie magique, pas toutes les parties admins et tout


https://taki.li/gitops-e2e

site : formation.takima.fr

#### Alternatives
Rendons le DDD aux devs - Neuilly 252 AB
### 12h30-13h30 : Repas
### 13h30-16h30 (Amphi bleu) : Kubernetes, dépassionné et pour les ultra débutants
#### Abstract
##### Talk
Que l’on le veuille ou non, Kubernetes fait bien partie de notre paysage aujourd’hui, adulé par certains et décrié par beaucoup d’autres. La réalité est que Kubernetes est bien implanté et sera présent pour un moment, peut-être caché sous une couche d’abstraction mais la tendance est quand même qu’il devienne l’ossature de l'écosystème Cloud Native.

Dans cette université, je vous propose de tout reprendre à zéro et de découvrir ensemble les concepts fondamentaux de Kubernetes : Pod, Service, Health checks … Durant les 3 heures nous entrerons progressivement plus dans les entrailles de la bête pour finir sur ces concepts plus avancés tel que les Operators. Chaque concept sera bien entendu illustré avec des démonstrations concrètes. Étant moi-même développeur, l’angle d’attaque sera celui du déploiement et gestion des applications, toute la partie opérationnelles tel que la mise en place d’un cluster ne sera pas couverte.

###### Speakers
**Sébastien Blanc**

Sébastien est Staff Developer Advocate à Aiven. Il se considère comme un Passion-Driven-Developer avec comme objectif principal de partager au maximum sa passion en donnant des conférences . Avant d’être Developer Advocate, Sébastien a passé 15 ans dans l’ingénierie logicielle, essentiellement orienté autour de Java EE. Durant son temps libre, il aime apprendre le code aux enfants et pratique le Kobudo, un vieil Art Martial japonais.

**Horacio Gonzalez**

Malgré ce que son accent espagnol bien prononcé peut suggérer, Horacio est arrivé en France il y a plus d'une vingtaine d'années. Passionné d'informatique, dans laquelle il est tombé depuis tout petit,Horacio est Directeur de Developer Relations chez OVHcloud. Il est cofondateur du @FinistDevs, et des @RdvSpeakers.

Passionné par le développement web et tout ce qui gravite autour des composants web et des standards web, Horacio aime aussi discuter de Kubernetes, AI et le cloud en général. Il est Google Developer Expert (GDE) en Web Technologies and Flutter.

**Sun TAN**

Sun est Senior Software Engineer chez Red Hat. Il a le plus beau métier du monde : Il développe des outils pour les développeurs (et donc pour lui-même). Il a notamment contribué aux projets Eclipse Che, Eclipse JKube et le client Java pour Kubernetes Fabric8. Pendant son temps libre, il co-anime le JUG de Paris et brasse sa bière dans sa cuisine.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | University |
| Track | Cloud, Containers & Infrastructure, DevOps |
| Presentation level | beginner/novice |

#### Notes

#### Intro

Pain points sur les applications historiques :
- déploiements manuels. Builds foireuses, déploiements par des personnes qui n'y connaissent rien / s'en fichent. Surtout quand il y a besoin de mettre à jour la version Java, faire des modifs en DB, etc.
- le scaling des applicatifs. Les applications non pensées pour ça, il fallait passer par les équipes dédiées, faire des setups compliqués, ... Beaucoup d'actions manuelles, qui souvent faisaient que ça marchait à peu près, ou de travers.
- l'environnement de dev. Non iso prod, pas les mêmes capacités, ...

Et voilà **Kubernetes** !

:closed_book: Think big, start small, Scale fast - Jim Carroll

Pour commencer K8S, commençons par les **conteneurs**

C'est le merge entre la méthode traditionnelle, avec le déploiement à la main sur des machines, et des VM avec des OS émulés.
Là elles tournent sur une machine physique, mais sont comme des VM et ne se voient pas les unes les autres.

Reco Containers :
- **Docker**
- **Podman**

Pour jouer avec les conteneurs : lancer un conteneur existant dans les *Container Registry* qui sont des conteneurs existants créés par la communauté (DB, Serveurs web, ...)
- *docker.io*
- *quay.io*

On lance le conteneur `docker run -p 80880:80 httpd:alpine` .On y accède via `localhost:8080`

On peut executer des commandes dans le container en faisant un exec avec comme paramètre le `container-id`

Il y a également in *tty* à l'intérieur du container (terminal interactif)
Sur ma machine, en faisant un top, j'ai mes process mais également ceux de mes containeurs. Les process et conteneurs sont isolés, mais s'exécutent bien sur la machine physique

On peut créer ses propres conteneurs, via un fichier *dockerfile*.
On part d'une image existante que l'on récupère, on ajoute les fichiers / outils dont on a besoin. Puis on fait un `docker build`.
On peut mettre le registry en prefix pour anticiper

On peut ensuite pousser l'image du conteneur dans un registry via un `docker push`.

On peut ensuite run son image de conteneur `docker run`

Les conteneurs existent depuis bien avant Docker, ça existait déjà sous Linux.
Pourquoi Docker a fait tout exploser ?

Car il a rendu facile le fait de builder, partager et donc faire tourner des conteneurs partout pour peu que la machine cible ait un docker d'installé.
Comme Java en fait. C'est ce qui a fait le succès de Docker

Il n'y a rien qui permet d'interconnecter en réseau les conteneurs. D'où K8S

Grâce aux conteneurs, y'a plus de "ça marche sur ma machine". On a tous le même environnement ! Exactement le même ! C'est facile pour les devs
Pour un sys-admin c'est pas forcément facile, car il faudra quand même du load balancing, de la réplication, ...

Le conteneurs ont donc ouvert les portes des micro services, de découpage applicatifs plus fin. Mais pensez au sys-admin. C'est bien plus complexe à maintenir, surveiller, ...
Donc un gain énorme de productivité pour les devs, mais pour le sys-admin ça fait beaucoup de petites tâches sans valeur ajoutées à faire régulièrement.

Donc en tant que sysadmin je voudrais un installeur qui ferait tout ça pour moi ! Et qu'il mette des sondes, de monitoring et tout !

Parfait c'est K8S. C'est un orchestrateur, il gère :
- déploiement
- scaling
- monitoring
- reparation
- sécurité
- ...

C'est un berger pour vos conteneurs :sheep:

C'est pas le seul outil qui fait ça, peut-être même pas le meilleur sur le marché (Docker Compose, Docker Swarm, Nomad, Mesos).
Mais il est peu complexe en tant qu'utilisateur, il est très complet. Il est surtout bien marketé, bien développé et de l'argent pour le soutenir !

Derrière K8S, il y a la Linux Foundation, avec beaucoup beaucoup de grosses boites derrière qui financent et dev le projet.

##### K8S
K8S est composé d'un *control plane* et de *workers*
Il se base sur une API qui permet aux workers de faire leur boulot. Cette API est modifiable par n'importe qui pour l'adapter à nos besoins, en plus des fonctions natives
- *ETCD* : mémoire du cluster, de ce qu'il a fait
- *Controller Manager*
- *Scheduler* 

Dans les workers, il y a 
- kubelet
- kube-proxy
- pods

Les pods sont les endroits où nos conteneurs seront stockés.
On peut par exemple rajouter un POD qui va chiffrer les données sortant du POD A et les déchiffrer avant d'entrer dans le POD B. Du coup le traffic est bien chiffré, juste on ne touche pas aux pods existants pour ajouter une couche de sécurité.

On fait alors un fichier déclaratif de type YAML pour arriver à un état désiré du POD (Desired State Management). Des fichiers de haut niveau de description de l'archi cible
K8S va faire de son mieux pour réaliser ce que tu lui a dis, avec ce qu'il a sous la main
Il va générer alors générer 5 objets :
- **PODs** : artefact qui s'execute
- **Deployement** : indique ce que tu souhaites en terme de nombre de pods, de volumes de mémoire, espace disque, ... C'est l'usine à PODS
- **Service** : va associer un déploiement à un point d'entrée dans le cluster
- **Ingress** : reverse proxy qui permet d'accéder à un service dans le cluster. On regroupe plusieurs services derrière un point unique
- **Load Balancer** : va donner accès à un service via internet (en donnant une IP publique)

Si un POD plante, K8S va le détecter, kill le POD et le redéployer ailleurs
Autre cas : on pert carrément un node K8S. K8S va recréer les pods déployés dans d'autres noeuds ayant de la dispo. On pourra alors recréer des noeuds à chaud et K8S pourra rebasculer des PODS dans ce nouveau noeud

Pour commencer à bosser avec K8S, je recommande d'utiliser **KUBECTL** avant le YAML. C'est la tool/CLI de K8S

On va créer un espace logique, nommé **Namespace**. Exemple : Prod, Indus, ou du métier, ou autre. C'est un moyen de séparer logiquement des ressources

:bulb: Utiliser `kubens` pour pouvoir facilement gérer et exploiter les namespaces

Lorsqu'un pod est détruit, le scheduler va le voir et va immédiatement en reconstruire un, via le deployement.
Vos PODS vont mourrir, soyez-y préparés.

Via KubeCTL je peux scale un POD existant via le `kubectl scale (...) --replicas`

On va vouloir exposer notre pod pour qu'il soit accessible `kubectl expose`. Mon POD a maintenant une IP Cluster et une IP Publique
Lorsque j'ai 3 instance de mon PODS, je vais avoir 3 Endpoints, un pour chaque instance de PODS. Par défaut il fera du round robin entre les 3 PODS

Avec `kubeclt` on peut aussi modifier l'image du POD. On peut le faire à la volée (ON NE DOIT JAMAIS FAIRE CA). Il va créer les nouveaux pods et tuer les anciens

##### YAML

**On peut définir des types custom de ressources, via un fichier YAML**

Dans K8S il y a des controllers qui écoutent pour les Creation/MAJ/Suppression des ressources et faire en sorte que ça matche les user requirements

Par défaut dans K8S, on a plein de ressources et controllers pour gérer les clusters K8S

On se sert des ressource Deployement pour faire du scaling. Et on va mettre les infos du POD dans le template. On utilise le champ `replicas` pour savoir le nombre de pods à instancier

Les services servent à la communication entre pods dans le cluster ou en dehors. K8S va utiliser le selector pour choisir avec quels PODS communiquer

Les Ingress servent à faire des redirections et noms de domaines sur des services

##### Etre un bon citoyen du cloud natif

L'objectif est de limiter l'influence d'un POD en pleine charge / défaut sans impacter négativement les autres PODS du noeud

- *Request* : les ressources nécessaires pour tourner
- *Limites* : limites max de ressources au dela de laquelle il faudra tuer le POD

On agit sur la mémoire et le CPU

Si le POD essaye de dépasser les limites :
- CPU : compressible : si le POD essaye d'excéder, l'OS peut le bloquer en lui donnant pas la main. Il est bridé
- mémoire : incompressible : s'il dépasse, le POD sera détruit.

On peut définir les limites ressources sur les pods, les clusters ou les namespaces
Sur les namespace ça s'appelle un `Resource quota`

Il y a également les `Limit Range` par pod, cluster ou namespace
Ce sont les valeurs par defauts qui seront appliquées partout, sauf si un resource quota est définis

:wrench: **GitPod** pour creer des pods dans le cloud

:warning: la commande top de `kubectl` n'est pas instantannée et va faire un calcul sur un certain temps. C'est différent de Linux !

Sur la plupart des K8S de providers, il y a des auto scaler par défauts qui détectent quand l'ensemble des pods concernés commencent à être plein, il va pouvoir ajouter un autre pod au cluster.

K8S ne va pas nous limiter même si un pod exige plus de ressources CPU que ce qu'il y a. Il fera juste de son mieux pour faire tourner les pods. Il met en pending les PODS qu'il n'arrive pas à instancier pour faire les faire tourner. S'il y un auto scaling, le K8S va aller creer de nouveaux clusters pour instancier les pods en pending

:warning: PLACER DES LIMITES SUR LES CLUSTERS / NAMESPACES. PUIS EN METTRE SUR LES PODS AU FUR ET A MESURE :warning:

==C'est pas parce que notre conteneur a démarré que notre application fonctionne bien. Il faut que le conteneur donne son go au cluster pour recevoir des requêtes clients
On va ajouter dans notre YAML de définition de déploiement avec `readinessProbe`. Par exemple une requête HTTP GET qui fait une 200 sur une adresses spécifique

Comment dire qu'on est toujours vivant ? On peut aussi dire au cluster de taper sur un point spécifique pour checker l'état de santé du POD=> `livenessProbe`==

Permet notamment de faire des rolling updates sans risque


**Config Map**

On ne met pas les configs dans les pods, on les en sort pour les adpater aux besoins.

`kubctl set env` pour modifier une variable d'environnement

`kubctl create cm` pour creer une Config Map. Je peux alors modifier mon déploiement pour tenir compte de ma nouvelle config map créé en mettant `envFrom` dans la configuration du déploiement

Les config map c'est la vie :heart:

**Secret**

Les secrets dans K8S ne sont pas secrets, donc attention. Ils sont injectés comme volume ou variable d'env dans le pod. 
Le secret est juste encodé en base64
Il faut coupler le secret avec un Vault par exemple pour vraiment mettre de la sécurité. N'importe quelle infra externe dont c'est le boulot fera l'affaire

##### Persistent Volumes
Mauvaise idée de stocker dans le POD car K8S va les buter quand ils seront trop gros.
Pareil pour les nodes K8S.

**On ne doit donc pas stocker localement**, on doit éviter de stocker dans K8S. Cependant on peut le faire avec les Persistent Volumes. Qui est de l'espace physique réel, qui est monté comme un volume sur le pod / node en question.

Mais chaque Cloud Provider a des façons différentes de le faire, donc attention.

#### Alternatives
Des tuyaux sur les pipelines - Neuilly 252 AB
Sécurité de la chaîne logicielle avec Sigstore - Neuilly 253
SQL (le retour): Démystifions les idées préconçues et utilisons la DB efficacement - Paris 242 AB
### 16h30-17h00 : Pause
### 17h00-17h30 (Neuilly 253) : Comment être bien onboardée en tant que développeuse junior reconvertie ??
#### Abstract
##### Talk
En tant que développeuse juniore en reconversion professionnelle (quand celle-ci s'arrête-t-elle exactement ?), j'ai dû changer d'entreprise suite à une très mauvaise expérience express dans ma première entreprise. De la première à la seconde, ce fut le jour et la nuit.

C'est un sujet qui part sur deux axes principaux :

- L'entreprise, qui possède un rôle majeur dans l'onboarding des salarié-e-s, autant techniquement (process, mini formations, présentations des pôles si moyenne/grande entreprise) qu'humainement,
- La développeuse ou le développeur qui doit s'intégrer, avec force, autonomie, en gérant la pression de son nouveau métier + nouveau lieu de travail + nouvelles responsabilités etc..

##### Speaker
**Amélie ABDALLAH**

Membre de Motiv'Her, DuchessFr, le fait que je sois reconvertie et développeuse juniore ne m'empêche pas d'être active dans de nombreuses communautés, que ce soit pour donner de l'aide tant pour le moral ou technique lorsque je peux.

Je milite donc pour un environnement plus sain, code compris.

Animée par les principes du "clean Code", le devOps, je passe de tout à tout techniquement et aime apprendre chaque jour, mais surtout rendre accessible l'information.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Tools-in-Action |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |
| Keywords | retraining team onboarding |

#### Notes

Légitimité ? charge mentale ? Entourage, soutien ? Temps personnel aà allouer pour arriver à faire le taff ? Santé mentale ? Handicap ? Autonomie ? Age ? ...

Quel facteur de stress et comment le gérer ? Finances, temps disponible, contexte perso, crédits, contrat de travail, ...

##### Première XP

Sortie de burn out donc santé mentale instable.
Formation accélérée de 5 mois, continue la formation en alternance
Pourquoi ça ne marche pas ? Plein d'éléments mais un principal.

Le premier jour : j'ai codé : un gros soucis => Aucune formation métier alors que le sujet est hyper spécifique. Méconnaissance des collègues, de leurs métiers, compétences, responsabilités. Y'a pas de documentation non plus.

Il y avait une dissonnance entre ce qui était promis et la réalité. Donc départ de l'entreprise.

##### Deuxieme XP

Ensuite entrée chez The Tribe (agence web / ESN. Truc un peu mixte). Aider à l'entreprenariat, gros focus sur élèves sortis de Centrale Nantes dans le recrutement.
Mais avec les mêmes profils on obtient un moule. Tout le monde est pareil.
Ils ont donc voulu tester de recruter une personne reconvertie, ça a bien matché ils ont donc continué.

Autour d'elle des gens d'horizons très différents, des parcours variés, des responsabilités variées. Plus de motivation, vision plus globale, plus de recul de la part des reconvertis. On accompagne mieux nos clients de par nos expériences. Plus de variété dans le recrutement.

Chez The Tribe, peu importe le métier, il y avoir un onboarding ritualisé.
- on te donne toutes les infos chiantes (administratives, légales, ...)
- on te présente tous les métiers de la boite et rencontre les gens (designers, sales, product manager, recrutement, support)
- rencontrer tout le monde
- recontre avec un ou plusieurs fondateurs
- système de parrainage / marrainage
- pas besoin d'ancienneté pour dires les choses, en direct avec les cofondateurs si besoin. Pas besoin de passer par la hiérarchie
- rapport d'étonnement après 6 semaines

Dès que je me suis sentie assez légitime, j'ai marrainé. Ca créé un rapport plus fort que juste le rapport de collègues.

Quand junior reconverti :
- création d'un repo pour mise à niveau technique
- sandbox sur les langages utilisés dans la boite
- docs multiples sur les archi utilisées
- Mentor disponible (questions, pair programming, ...)
- temps priorisé pour le mentor. Bloqué dans son agenda

##### Différence entre les 2 expériences

Rien à voir, toujours un parrain et un mentor, à apprendre plein de choses
passé d'une expérience très solitaire, à quelque chose de très collectif

L'accompagnement, le rituel posé, qui évolue dans le temps suivant les retours.
Un tronc commun pour tous les nouveaux + une partie spéciale pour les reconvertis

Il faut donc pour les reconvertis :
- empathie sur le parcours, comprendre et aider
- être à l'écoute de ses attentes (tâches, besoins, engagements ...)
- valoriser l'ENSEMBLE du parcours professionnel
- être disponible pour l'accompagner
- avoir envie de recruter un(e) reconverti(e) pour ce qu'elle apporte, son bagage, malgré le coût et le temps que ça peu représenter

#### Alternatives
Des secrets dans les pixels ! À la découverte de la stéganographie - Paris 242 AB
Loki: Comme Prometheus Mais Pour Les Logs -  Maillot
Besoin d'env de développement ultra robuste? On se crée un env immutable et reproductible avec Nix en quelques minutes! - Paris 241
### 17h45-18h15 (Paris 241) : Pourquoi quitter la tech
#### Abstract
##### Talk
Le développement web, eldorado promis ? Après 7 ans à développer dans tout type de structures (startup, entreprise éthique, administration française,...), j’ai (parfois) envie de tirer ma révérence au monde de la tech. Sexisme, impasse de carrière, plafond de verre, défense du pré carré, le tableau n'a pas l'air très rose comme ça. Mais il permet de faire un état des lieux du monde du développement web aujourd'hui et de sa difficulté à faire de la place aux profils en minorité dans la tech. En partant des phrases en apparence anodines entendues au cours de ma carrière, nous allons démontrer qu’il s’agit d’un problème systémique qui concerne tous les profils discriminés. Nous essaierons aussi de voir ce qui se passe derrière ces micro-agressions, les effets qu’elles ont lorsqu’elles sont répétées. En dernier point, je tenterai de lister les bonnes pratiques, les actions que personnes discriminées, entreprises et personnes en situation de domination peuvent mettre en place pour faire de la tech un lieu plus accueillant pour toutes et tous, qu’elles que soient notre genre, nos origines, nos handicaps, nos anciennes carrières,...

##### Speaker
**Sonia Prévost**

De la gestion de projet au développement web en passant par la création d'événements littéraires, Sonia Prévost a fait pas mal de métiers différents avec toujours une soif d'apprendre et de comprendre. Après 6 ans en tant que développeuse puis responsable d'équipe, elle envisage (parfois) de quitter le monde de la tech.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Tools-in-Action |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |
| Keywords | diversity |

#### Notes

[@wondersonja](https://twitter.com/wondersonja)

[slides](https://slides.com/soniaprevost/quitterlatech)

Reconversion
Développeuse RubyOnRails

Minorité dans la tech : Femme et reconvertie
Mais qu'une minorité parmis d'autres. Certaines qui vont vivre ou ont vécu des situations pire que celle que Sonia a vécu

##### Petites phrases anodines

Argument d'autorité : "Ah bon mais tu ne connais pas ce concept de base ?"
Gate keeping, on t'empêche d'entrer dans notre club

"Toi tu sais parler aux gens" => gluework => lier entre elles les parties prenantes. Donc je stagne pendant que le "dev" progresse techniquement. Donc on créé un plafond de verre pour la personne.

"T'es pas assez technique pour ce poste" => critique bienveillante humiliante. Saper, rabaisser en restant sympa

"Mauvaise ambiance" sexisme décomplexée. 

Et encore plein d'autres, qui ont un impact. Et on a le droit de s'enflammer.
Syndrome de l'imposteur.

##### Problème systémique

Effet cumulatif => burn out, fatigue, bloquage de carrière

La moitié des femmes en poste quittent la tech à 35 ans

Sauf que ça coûte cher, aux personnes, aux entreprises. Turnover + pénurie de profils

Problème sociétal
Controle social informel

##### Faire bouger les lignes

Les entreprises ont un grand rôle à jouer

Révéler que c'était systémique fait un bien fou. Comprendre quand on est dominé que c'est systémique et non pas personnel.

:closed_book: La vonlonté de changer - Bell Hooks

#### Alternatives
OPA, mais que fait la policy ? 👮 - Neuilly 253
Playwright : l'outil qui va révolutionner les tests end-to-end - Neuilly 252 AB
### 18h30-19h00 (Paris 242 AB) : De la première ligne de code au succès : REX d’un projet open source
#### Abstract
##### Talk
On a tous eu des idées de projets pendant qu'on faisait nos études, mais que faire quand ça marche? Comment on gère l'administratif? Comment on gère l'infra? La communication? La traduction? Et tant d'autres choses auxquelles je n'avais pas pensé.

Dans ce talk, je vous décrit mon parcours semé d'embuches, depuis 5 ans, entre technique et administratif, tout ce que j'aurais aimé savoir quand j'ai lancé mon projet, qui a évolué pour devenir un produit complet, avec 500 000 utilisateurs actifs mensuels, une traduction en 10 langues, une communauté active et un seul développeur: moi.

##### Speaker
**Flavien Normand**

Consultant Angular, Développeur Open Source, Joueur de MMO, Pêcheur, enfin quand j'ai le temps.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Tools-in-Action |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |
| Keywords | scaling Management community |

#### Notes

Problème de gestion de dépendances, sous forme d'abre. Pour des milliers d'objets dans le jeu FF XIV

On fait un tableur excel, pour gérer les dépendances et répartir les tâches
Mais on est tous devs dans la guilde ! Du coup on le fait en code => TeamCraft

Beaucoup de retro ingénierie pour sortir les donées du jeu, car aucune doc, aucune API
Le besoin de base était simple, il s'est complexifié avec le temps petit à petit et s'est renforcé de plein de fonctions périphériques (notamment un traducteur de macros)
Certaines fonctionnalités ont été créés entièrement par des contributeurs.

:wrench: *ant-design* => le design d'alibaba sur Angular. Topissime pour un dev qui n'est pas designer

:wrench: **Pirsch analytics**. Système d'analytics sans cookie, qui creuse donc beaucoup moins loin dans les données persos. Parfait pour les petits analytics qu'on veut

:wrench: outil de traduction : **Crowdin**. Il fait automatiquement les PR, avec toutes les traductoins d'un coup, mais surtout c'est gratuit pour les projet open source.

Au début c'était en matérial design, c'était moche.
Il faut un backend qui tourne derrière, qui scale et qui est maintenable. Et pas gestion d'infra aussi. Du coup **Firebase**. Il a des quotas gratuits qui suffisaient bien lors des 2 premières années. Aujourd'hui ça coûte 200€ par mois

On voit ici que le pricing n'est pas linéaire avec l'utilisation de l'application. Donc l'optimisation est plutôt pas mal

Financement du projet :
- Patreon -> 4K€ par mois
- Tipee -> 18€ par mois
- playwire -> régie de pub. Pub ciblée par le site pas de vidéo, pas d'audio, pas d'animation. Une seule pub, toujours au même endroit.

Pour faire tout ça, il faut être auto entrepreneur, voir société et déclarer à l'ursaff

Et sa boite Zenika, lui donne du temps pour faire ce travail. Jour offert par la boite

##### Avantages
- Communauté internationale, projet et librairies qui fédèrent
- énorme bac à sable personnel
- projet vitrine sur le CV
- des rencontres, des échanges fous
- un revenu complémentaire

##### Inconvénients
- par de contacts avec Square Enix
- beaucoup de temps passé
- beaucoup de stress
- Confusion projet open source / Produit professionnel

#### Alternatives
Télétravail asynchrone - Amphi bleu
JKube remote-dev : coder avec tous vos micro-services du cloud ... en local ! -  Neuilly 252 AB

## Jeudi 13 avril 2023

### 09h00-09h20 (amphi bleu): Et si le digital permettait de mieux vivre et plus longtemps ?
#### Abstract
##### Talk
Maladies chroniques, cardiovasculaires ou néonatales… ces maux sont à l’origine de +50% des 55 millions de décès annuels. Certaines frappent soudainement et ne laissent que peu de place à l’espoir alors que d’autres nous accompagnent au quotidien. Dans les deux cas, des vies sont bouleversées à jamais. Et si le digital permettait de mieux vivre et plus longtemps ?

##### Speaker
**Céline Lazorthes**

Entrepreneure optimiste et passionnée, Céline Lazorthes est la co-fondatrice et co-CEO de Resilience. Sa mission ? Universaliser l'excellence médicale pour vivre mieux et plus longtemps. Elle a précédemment fondé le groupe Leetchi, vendu au Crédit Mutuel Arkea en Septembre 2015. Business angel active, elle a investi dans plus de 40 entreprises telles que : Jimmy Fairly, Talent.io, Frichti, Le Slip Français, Welcome to the jungle... Profondément engagée, elle a co-fondé France Digitale, France FinTech, SISTA et plus récemment #ProtègeTonSoignant, un collectif d'entrepreneurs et d'artistes au service du personnel médical. Elle intervient régulièrement sur les thèmes de l'économie du partage, de l'égalité des chances et du women empowerment.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Keynote |
| Track | Big Data, Machine Learning, Analytics |
| Presentation level | beginner/novice |
| Keywords | health Artificial Intelligence innovation |

#### Notes

3 causes principales de cancer :
- vieillissement de la population
- meilleure détection et prise en charge
- l'évolution de notre mode de vie

en 2040 il y aura 40 millions de nouveaux cas par ans dans le monde

En France on a 1200 oncologues pour 5 millions de malades.
Du coup il y a un petit soucis, notamment pour prendre des décisions de traitements.
En France, pour un patient, il y 2 minutes consacrées au fait de choisir un traitement.

Les patients souffrent, mais les professionnels de santé également

On est toutes et tous concernés

Avec des outils digitaux basiques (RTable, Leetchi et What's App), ils ont monté #ProtègeTonSoignant pour réuinir des fonds et livrer aux hopitaux/cliniques/professionnels de santé des outils et produits dont ils avaient besoin.

Résilience : permet de faire du suivi par télésurveillance des patients, qui permet aux équipes soignantes de suivre à distance ses patients et de recevoir des alertes quand un soucis arrive, voire de l'anticiper.

MIT a créé une IA qui permet de détecter 7 ans avant le début d'un cancer des poumons, d'identifier un potentiel risque de tumeurs. Examen peu invasif, et peu coûteux qui pourrait donc sauver des millions de vie.
### 09h25-09h45 (amphi bleu): La tech au secours de la planète ?
#### Abstract
##### Talk
Alors que les « limites planétaires » se rapprochent dangereusement (changement climatique, effondrement de la biodiversité, dégradation des sols, tensions sur l’énergie et les matières premières…), les promesses « techno-solutionnistes » sont plus prégnantes que jamais.

A en croire les prophètes de la Silicon Valley, métavers, intelligence artificielle, robots autonomes, puces neuronales et conquête de l’espace seraient notre destin inéluctable. Et en attendant, énergies renouvelables, voitures électriques et hydrogène « vert » devraient nous permettre de ne pas trop entamer notre niveau de vie.

Mais ces innovations consomment des métaux, souvent plus rares et difficilement recyclables. La contrainte sur les ressources matérielles nous imposera-t-elle des limites ? Et si nos efforts d’innovation devaient se concentrer plutôt sur les technologies sobres et plus résilientes ?

##### Speaker
**Philippe Bihouix**

Philippe Bihouix a travaillé comme ingénieur-conseil ou dirigeant dans différents secteurs industriels, en particulier les transports et la construction, avant de rejoindre le groupe AREP, agence d’architecture interdisciplinaire, comme directeur général. Il est l’auteur de plusieurs ouvrages sur la question des ressources non renouvelables et des enjeux technologiques associés, en particulier L’âge des low tech. Vers une civilisation techniquement soutenable (Seuil, 2014 ; Points 2021) et Le bonheur était pour demain. Les rêveries d’un ingénieur solitaire (Seuil, 2019 ; Points 2022).

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Keynote |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |
| Keywords | greenIT futures innovations |

#### Notes
On a un stock fini de matières qui nous permettent de créer des outils
Pour réaliser le futur qu'on nous promet (voitures électriques, hydrogènes, énergies renouvelables), il faudra toujours plus de ressources, une croissance constante

Mais les métaux on peut les recycler, non ? En fait notre monde ne marche pas comme ça
On ne recycle qu'une toute petite partie des éléments, généralement la majorité des ressources issues du recyclage deviennent des déchets recyclables

Si on continue notre croissance de 2% d'énergie par ans. Dans 1500 ans il faudra qu'on génère l'énergie équivalente à notre soleil. Donc c'est pas viable

Déjà on ne peut partir que sur des estimations des impacts environnementaux de l'industrie informatique.
Le digital aujourd'hui c'est à peu près 10% de la consommation mondiale d'énergie, c'est plus que le transport aérien et ça continue de croitre fortement.

Green IT, beaucoup de progrès qui sont très importants.
Sauf que tout ça est balayé par l'effet rebond : chaque tâche unitaire consomme moins, mais du coup c'est moins cher et du coup on fait toujours plus de tâches. Donc c'est une fuite en avant continue

###### Qu'est-ce qu'on fait ?
La sobriété : réduire à la source. Mais il faut qu'elle soit systémique et profonde. Réduire les tailles des voitures, réduire le nombre de réseaux, ...
On doit maintenir ! On doit faire durer et mieux réparer nos outils. C'est vrai également pour des bâtiments (centres commerciaux, bureaux, ...). Vraiment rentrer dans la réparation, la réutilisation.
Faire preuve de techno discernement. Il y a plein d'endroits où la technologie ne sert à rien : l'école numérique.
Jusqu'où on remplace des humains par des outils et technos qui sont consomatrices.

##### Comment on le met en place ?
Ce n'est que quelques multi nationales qui mettent tous ces produits dans nos vie et nous consommateurs on s'adapte à ce qu'on nous vend.
Le client n'est pas roi. On le sait depuis les années 60. Les enjeux financiers sont tels que vous ne pouvez vous permettre que votre produit ne se vende pas.
Il faut transformer par l'offre et non la demande.
C'est donc par l'action politique qu'on va avoir un levier pour faire ces transformations. On va devoir contraindre. Est-ce bien grave ?
Il faudra manier ce levier avec subtilité.
Un autre levier sera la fiscabilité. Rendre plus attratif les réparations. Il va falloir basculer d'une fiscalité sur le travail humain, à une fiscalité sur l'énergie et les ressources, afin de réduire le prix du travail humain et donc rendre attractif la réparation / réutilisation.

##### Note optimiste
On a tous les moyens aujourd'hui pour changer.
On en a sous le pied, tellement de marge dans tous les sens qu'on devrait y arriver si on s'y mettait. C'est hyper facile techniquement.

Le problème c'est que ça heurte les finances et nos habitudes.
### 09h50-10H10 (amphi bleu): Definition of Done : une notion évolutive en start-up santé
#### Abstract
##### Talk
Le développement logiciel est au coeur de la différenciation de Ganymed Robotics. Une des briques technologique majeures repose sur la technologie unique et brevetée de vision par ordinateur, véritable “GPS instantané” qui permet la mise en correspondance précise des informations pré et per-opératoires via des algorithmes testés et validés en conditions cliniques.

Ensuite vient évidemment le contrôle du mouvement du robot, le suivi du workflow chirurgical, la gestion de l’interface chirurgien, des données générées en cours d’opération, etc.

Bref, on code !

Mais comment et dans quel but ? Le passage d’algorithmes de recherche et de test à un véritable code industriel, robuste, adapté aux exigences de qualité du médical, très normé, représente un changement de méthodes de travail majeur au sein des équipes de R&D.

Sophie parlera de l’expérience de Ganymed Robotics et de ses équipes logiciel dans la gestion de ce virage entre développement d’une technologie et développement d’un produit, nécessaire et difficile!

##### Speaker
**Sophie Cahen**

Sophie a co-fondé Ganymed Robotics en 2018 à Paris. La société, qui a levé 36m€ en 2022, développe des logiciels de vision par ordinateur et des technologies robotiques visant à guider le geste du chirurgien pendant des opérations de chirurgie. Son robot d’assistance chirurgicale, destiné à la pose de prothèse de genoux, ambitionne d’améliorer la précision des interventions et les bénéfices patients, tout en démocratisant l’accès à des soins chirurgicaux de qualité.

Sophie a 10 ans d’expérience professionnelle internationale ; avant de co-fonder Ganymed Robotics, elle a travaillé chez Avencore, l'Agence française de développement (AFD) et Astorg. Sophie a co-inventé 3 brevets déposés, elle siège au comité de pilotage pour la robotique et l'électronique du programme gouvernemental "France 2030" et a été nommée Chevalière de l'Ordre National du Mérite en 2022.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Keynote |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |
| Keywords | Artificial Intelligence Robots health |

#### Notes
Robotique pour la chirurgie
70% des gens de la RD codent dans la boite. C'est le coeur du métier

Faire du quick & dirty quand on fait de la R&D c'est normal
Faire du beau code propre pour la prod et pour que ça marche bien dans les salle d'opérations, c'est important.

A un moment le quick & dirty c'est nécessaire à la survie de l'entreprise, idem pour le beau code quelques temps plus tard.
Ce sont deux visions complémentaires, pas anthagonistes
### 10h15-10h40 : Pause
### 10h45-11h30 (maillot): Rencontrer les autres: les outils au service de la prise de parole
#### Abstract
##### Talk
Le monde professionnel attend que l’on sache présenter des solutions, gérer des conflits et défendre ses idées au sein d'une équipe pour maximiser l'innovation. A la sortie d’une réunion, est-ce que ca vous est déjà arrivé que ce ne soit pas la meilleure idée mais celle qui a été le mieux présentée qui a été prise? Plus le télétravail et les moyens de communication numériques se développent, plus il est difficile de pratiquer la communication interpersonnelle. Entrer en relation avec l'autre implique de la confiance, de la responsabilité et de l'empathie. Par quoi commencer? Comment surmonter ses peurs et avoir de la confiance en soi pour s’affirmer sans s’imposer? Comment avoir une attitude face à l'imprévu et avoir de la répartie tout en favorisant les échanges constructifs?
Dans cette conférence, je vous propose des clefs et des exemples pour faire ces premiers pas. Elles sont issues de mon expérience personnelle et professionnelle depuis plus de 15 ans en tant que comédienne, formatrice et sophrologue. Vous repartirez avec des outils prêts à être mis en pratique dès le lendemain.

##### Speaker
**Alex Casanova**

Comédienne, improvisatrice, sophrologue, écrivaine, et surtout femme de coeur, sa soif de savoirs et de compétences est loin d’être étanchée ! C’est aujourd’hui au coaching et à l’enseignement qu’elle se forme. C'est en pratiquant divers accompagnements, essentiellement de relaxation, de préparation à la prise en parole en public et d’improvisation, qu'elle se rend compte qu'elle peut allier exercices de sophrologie et direction ou coaching d'acteur et d'avocat. Elle accompagne avec sincérité et bienveillance les personnes vers leur propre singularité dans l’expression d’eux-mêmes, au moment présent, et en toutes circonstances.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Conference |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |

#### Notes
Prise de parole se heurte à beaucoup de problématiques humaines, sur lesquelles il n'est pas facile de prendre la main
Le monde de l'entreprise n'est pas toujours l'endroit le plus simple pour passer outre ces obstacles

Accpeter l'échec fait partie du processus

Plus je me sens bien, plus ça va se propager à mes collègues.

Improvisation fait partie du truc, tant qu'on a un fil directeur, des idées à proposer et d'écouter les propositions des gens avec qui on fait la prise de parole.

C'est de notre responsabilité individuelle de créer une bonne ambiance

3 types de communication :
- communication interpersonnelle : s'adresse à une personne
- communication groupe : s'adresse à un groupe cible précis / connu
- communication de masse : s'adresse au plus grand nombre

Définir un objectif pour révéler le blocage

Point de départ : sortir de l'isolement (mental et physique). Le monstre : la peur de sortir de sa zone de confort.
Utilisation de technique de relaxation par la respiration par exemple, ou techniques de visualisation de moments positifs

Deuxieme épreuve : la prise de parole et confiance en soi. Le monstre : trac ou anxiété
Structurer sa pensée, coaching, théatre et impro peuvent également aider.
Posture : lever les bras en V de victoire, pour se mettre dans un état optimiste positif / ouvert

Troisième épreuve : la relation d'équipe et la répartie. Le monstre : les conflits ou la difficulté à collaborer avec les autres
Association d'idées, créer le mécanisme de productivité en nous. Philosophie de l'impro "oui et", écoute, répétition, reformulation

Quatrieme épreuve : créativité et innovation, renforcer sa confiance
Le monstre : la peur de l'echec ou de ne pas être à la hauteur. De sortir de sa zone de confort, de changer ses habitudes, de ne pas être original, d'être jugé
Enumérer 5 adjectifs positif / qualité en commanceant par un qui commence par la première lettre de mon prénom

Cercle vertueux :
- je vis donc j'existe donc j'ai ma place
- je reconnais mes qualités, mes expériences
- j'accueille donc l'erreur comme une opportunité d'apprendre
- moi au moins j'essaie
- j'atteins mes objectifs à mon rythme
### 11h45-12h30 (253): Vous voulez sauver la planète ? Utilisez DailyClean, une application open source réalisée avec Quarkus.
#### Abstract
##### Talk
Je ne sais pas vous, mais moi, quand j'étais gosse, mes parents m'ont toujours cassé les pieds pour que j'éteigne les lumières non utilisées. Je détestais ça ! Et puis, de fil en aiguille, c'est devenu un automatisme, un geste quotidien et normal.

Aujourd'hui, dans mon boulot, il y a un truc qui émerge : la Finops ! Ou encore, en bon français : comment pouvons-nous réduire les coûts de nos infrastructures informatiques ? Jusque-là, on se dit que c'est une histoire de sous (c'est vrai et c'est aussi important) et puis, en y regardant d'un peu plus près, on se rend compte que cette discipline s'inscrit pleinement dans une démarche GreenIT. Dans ma boîte, nous nous sommes rendus compte que nos services Cloud étaient en moyenne inutilisés 67% du temps ... Si mes parents savaient ça !

Lors de cette présentation, j'aimerais vous montrer un outil de Dailyclean open source qui nous permet de libérer les ressources non utilisées de notre Kubernetes.

Vous aussi vous voulez contribuer à sauver la planète ? Venez découvrir comment faire le premier pas !

https://github.com/AxaGuilDEv/dailyclean

##### Speaker
**Guillaume Thomas**

Lead développeur militant et passionné chez AXA France, j'essaie de mettre au maximum en pratique mon nouveau credo : la Finops au service du climat (et de mon portefeuille). Je crois donc fermement au potentiel des technologies dites natives telles que GraalVM, .Net core ou encore Rust. Javaiste confirmé, j'apprends actuellement le Python afin de faire ma mue en tant que ML Engineer. Ah ! Et j'aime bien la musique aussi : je travaille depuis 20 ans sur mon premier album. Promis, il sort bientôt (non) !

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Conference |
| Track | Cloud, Containers & Infrastructure, DevOps |
| Presentation level | beginner/novice |
| Keywords | greenIT Open Source code Quarkus Graal Cloud Machine Learning |

#### Notes
DailyClean un projet open source chez Axa France => sauvegarder un max de vos ressources K8S, éviter les ressources inutilisées
Chez Axa France le **DailyClean est obligatoire pour tout projet** qui utilise K8S

DailyClean
Page web qui permet
- d'allumer / éteindre un environnement
- une liste de toutes les ressources de l'environnement (précisant celles qui seront assujeties à DailyClean ou non)
- Estimations des coûts en utilisant ou non dailyClean
- un onglet configuration (heure d'allumage et d'extinction). On peut paramétrer les jours d'utilisations (tout le temps, le week-end, juste la semaine)
- possibilité de juste garder une heure d'extinction et pas d'heure de démarrage. On doit alors faire un démarrage manuel.

*Cas exemple*

On veut pouvoir scaler des solutions temps réels pour traiter des documents, suivant la complexité et le nombre de traitements à faire. On part sur des micro services conteneurisés à cause de leurs dépendances, de leurs versions Python, etc.

*Pourquoi DailyClean ?*

La facturation de la plateforme se fait sur la RAM réservée, payée en forfait. Egalement une RAM à la demande, qui va coûté plus cher mais qui va être payée uniquement lorsqu'elle est utilisée. Déjà on peut optimiser ça.
On s'est rendu compte qu'en mode classique, la facturation était très lourde, et le ROI était très mauvais. Il fallait donc une solution, donc optimisation et daily clean.
Résultat : division du coût par 10

Applications qui tournent 24/7 toute l'année. Sauf que l'application en question ne sert que 12h par jour, pas le week-end. Donc 60h/semaine ou 240h/mois
Du coup l'appli ne sert à rien 4,5 jours par semaine, et donc 7 mois par an. Donc 67% de son temps elle ne sert à rien.
Donc on trouve des solutions.

K8S quand on delete un POD, K8S va en redémarrer un de lui-même. Par contre le deploiement dans K8S peut nous permettre de le faire. Notamment en jouant sur les replicas
**DailyClean, si on resume ne fait que ça : du "scale to 0"**

Pour que les déploiements ne soient pas impactés par dailyclean, on lui met un label spécifique que DailyClean va ignorer ce déploiement
Il y a également des ressources cronjob qui permettent de trigger ces job de stop / start

CronJob -> Job -> ReplicaSets

Que se passe-t-il quand j'ai un env avec des replicas > 1 ? DailyClean ne le fait pas, il faut utiliser d'autres tools couplés à DailyClean comme Flux

DailyClean n'est installé que dans son NS pour ne pas taper les envs des autres, pour laisser la main à ses équipes, et pour gérer les droits finement grâce à K8S.
Mais du coup il est répliqué dans tous les NS !

Objectifs:
- facile à développer
- facile à maintenir
- l'empreinte mémoire la plus faible possible

**GraalVM** et **Quarkus**

GraalVM : JVM d'Oracle qui propose la compilation par anticipation
On n'a donc plus un `.jar` mais un `.exe`

Gain de performances, RAM, et pas de JVM dans le conteneur !
Et puis le temps de démarrage est beaucoup plus faible !

Quarkus : framework de dev. "C'est comme un SpringBoot" et il marche avec GraalVM ! Et puis dev et supporté par Red Hat qui est le partenaire infra de la boite.

##### Conclu
Pour ton portefeuille, pour la planète, c'est bon de couper les envs quand ils ne servent à rien !
Fonctionne avec tous vos projets K8S.

C'est gratos ! et Open Source !

##### Alternatives
Ctrl+Alt+Depression - 241
Bien choisir sa base de données - Amphi bleu
Les dessous des noms de domaines - Maillot
Alice au pays d'OpenTelemetry - 252 AB
#RetourAuxSources : Le cache HTTP - 242 AB
### 12h30-13h30 : Repas + Talk Avoir un journal de codeur (252 AB)
#### Abstract
##### Talk
Avez-vous déjà perdu du temps à rechercher les mêmes snippets de code ? L’impression d’avoir déjà résolu le même problème mais oublié comment ? Ne pas savoir où est passée votre journée de travail ? Du mal à gérer votre charge de travail ? L'impression de ne pas contrôler votre carrière et de naviguer dans le brouillard ? L’envie de devenir un(e) meilleur(e) développeur(se) ? La réponse c’est d’avoir un journal de codeur(se). Parlons-en !

##### Speaker
**Sandrine Banas**

Expert technique Senior Java avec plus de 20 ans d’expérience. Présentatrice au Snowcamp, Volcamp, MixIt, Agile Grenoble... Actuellement je travaille sur des projets Java (Microservices / Spring boot), Angular / Ionic et AWS.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Quickie |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |
| Keywords | Notes snippet workplace |

#### Notes
Exemples de journaux professionnel : Charles Darwin, Leonard Da Vinci, ou encore Carl Jung, ou Marie Curie

Ca montre que les grandes découvertes n'arrivent pas du jour au lendemain, qu'il faut des années de tatonnement et de travail

Un journal répond à un besoin, et que tant que le besoin est d'actualité. Le journal peut s'arrêter quand le besoin est rempli
Du coup : à quel besoin doit répondre le mien ?

Capitaliser mes connaissances, suivres ma carrière, trouver mes appétences pour expertises, ...

Journal = (Date + trace) * nb Jours

Traces informatives : informations du projet, avec qui quelle technologie, quelle responsabilité, qui inviter au pot de départ. Les tâches faites dans la journée. Les succès professionnels
Ne pas hésiter à mettre des indicateurs (SMART si possible) comme smiley (bien être emotionnel), checkbox suivi de tâche, pourcentage, échelle (1 à 10) comme par exemple les tâches confiées et l'intérêt technique associé.
Ca permet de garder une trace, car la mémoire arrondi les angles
Si un soucis bien pensé à prendre le temps de prendre du recul, pas juste noter le problème

Traces graphiques : sketchstorm, schéma heuristiques, visualiation

Traces techniques : pour / contre, résolution de bugs, DDR (Developer Decision Record) noter pourquoi on choisi de faire telle implémentation / archi
Les snippets n'ont pas forcément besoin d'être dans le journal. Plutôt mettre ça sur un Github, et veiller à gérer la confidentiallité

Quel support ? Papier vs Informatique
Sécurité ? Ne pas mettre à visu de tous surtout si mood, ne pas mettre d'infos confidentielle
Organisation : numéroter pages, pas forcément indexer tous les jours, juste les points importants

Tips :
- reprise du jour d'avant avant d'écrire le jour actuel
- On peut ensuite remonter à une semaine pour affiner les éléments
- un mois en arrière, pour voir sur quoi on capitalise et se remettre au question par la suite

**Bien voir le besoin et choisir en conséquence les traces qui nous intéressent**

##### Alternatives
Bienvenue dans ma zone d'inconfort - 241
Tous architectes ! - Maillot
### 13h30-14h15 (amphi bleu): Loi de Conway : lorsque les bonnes pratiques ne suffisent pas :warning: A REVOIR / COMPLEXE :warning:
#### Abstract
##### Talk
Avez-vous des APIs découpées d'une manière qui semble au final arbitraire et orthogonale au métier ? Que l'architecture décidée n'est jamais vraiment respectée ni réalisée ?

Vos utilisateurs ont toujours du mal à récupérer les informations dont ils ont besoin, alors que vous avez mis le paquet sur le métier et l'expérience utilisateur ?

N'avez-vous jamais remarqué, que bien vous suivez les bonnes pratiques, le logiciel qu'on construit s'écarte souvent de la vision produit, technique et parfois même des besoins de l'utilisateur ?

Et si on vous disait que tout cela est lié, et qu'il existe une force qui a une influence certaine sur votre produit, votre expérience utilisateur, votre architecture et même la qualité de votre logiciel ?

Venez découvrir la Loi de Conway ! Cette force méconnue qui a un pouvoir certain sur ce que vous construisez quel que soit votre métier. A travers des études scientifiques et des retours du terrain sur des exemples réels, nous verrons ses impacts sur les différents aspects du logiciel et nous apprendrons comment les apprivoiser.

##### Speaker
**Julien Topçu**

Tech Coach chez Shodo, j'accompagne le développement de logiciels à forte valeur métier en usant de techniques issues du Domain-Driven Design, le tout propulsé en Xtreme Programming dans la philosophie Kanban #NoEstimates. Membre de la fondation OWASP, j'évangélise sur les techniques de sécurité applicative afin d'éviter de se faire hacker bien comme il faut.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Conference |
| Track | Architecture, Performance and Security |
| Presentation level | Intermediate |
| Keywords | organization Design architecture user experience product |

#### Notes
Via le site du consultant, on en apprend plus sur la hiérarchie et la position de la boite, et d'une pietre UX. Plus que comment ils peuvent t'aider

*Jakob Nielsen*, expert UX à l'époque
Rejoins par *Nigel Bevan* qui explique pourquoi les sites sont si frustrants à utiliser

Alors pourquoi on en est toujours là alors que c'est documenté depuis les années 90 ?

Correlation entre plein de plantages en prod et un score de KPI ?
Ils vont rajouter un indicateur qui s'appelle **Organisational Structure**

Et à priori, dans le cas de Windows Vista, c'est l'organisation qui a poussé le plus de soucis.

Product Quality is strongly affected by organization structure

:closed_book: The mythical man-month - Frederick Brooks

Feature Team : pattern d'équipe pour implémenter à travers les couches du SI une feature
Donc avec des pays différents si tu mets une équipe différente, qui codaient toutes au meme endroit. Et comme owner ship n'est pas clair, personne ne prend sa part pour le refacto

Tout ça est impacté par votre organisation

Chaque équipe a son systeme. Quand 2 systèmes s'interlaccent, on formalise, on dev ensemble et on met un scrum master. Donc là on retrouve l'organisation dans la structure IT des projets
On parle ici d'**homomorphisme**

Une organisation peut même limiter la conception en n'envisageant même pas certains scénarios, certaines structures.
Que se passe-t-il quand il y a un system à cheval entre 2 équipes ? Ca va être compliqué voir impossible à faire.

Conway : *Choisir une orga c'est déjà contrainte la structure du systeme qu'elle produira [...]*

Le soucis est donc qu'on va créer une organisation avant même de comprendre le system à designer et donc contraindre et siloter le projet

La structure de communication va ressembler à la structure de hiérarchique de l'entreprise

On est contraint à toujours subir la **loi de Conway**.

**Inverse Conway Maneuveur** : casser les silo, flexibilité. Mais pas réaliste de changer constament de changer l'organisation

Normalement modèle BAPO, se concentrer sur le besoin du client
Mais la plupart des projet sont OPAB. Donc on va dans le mur

Pour traiter BA : Domain Driven Design.
- Event storming
- Context Map (voir l'orthogonalité entre les besoins, solutions, comms, ...)

Mais comment on s'organise ? => Team Topologies pour traiter le PO

Limiter la taille des domaines doit être dimensionné à la taille de l'équipe pour ne pas la surcharger

:closed_book: étude Accelerate de Nicole Forsgren

Inverse Conay Maneuver n'est pas une baguette magique. Si ton système est rigide à la modif, il ne pourra pas bouger. Il faudra l'assainnir avant.
Et une réorganisation ça coute cher, en temps, en argent, en humains ...
##### Alternatives
Savez-vous vraiment comment fonctionne git ? - Maillot
Poser les bonnes questions au bon moment pour accélérer son développement produit. -  242 AB
### 14h30-15h15 252 AB: Passer de Dallas à Happy Days: conseils pour hacker positivement sa vie
#### Abstract
##### Talk
Les développeurs aiment bien optimiser leur environnement de dev aux petits oignons. Et on s’entretient physiquement en faisant du sport – ou pas mais on se dit que ça serait pas mal 😜.
Et votre mental ? Vous optimisez votre mental ? En prenez-vous même soin ?

Dans cette présentation, Emmanuel (Red Hat, Quarkus, Les Cast Codeurs, Hibernate, etc) partage son expérience sur l’importance de, et les approches pour garder la tête sereine dans notre vie de travailleur de l’information et surtout d’humain passant par des coups durs. Via le prisme de son vécu, Emmanuel discutera comment s’organiser sans couler, comment prendre soin de soi mentalement et aussi d’un ensemble de conseils autour de la façon de voir son évolution professionnelle.

L’objectif est de vous avoir incité à réfléchir sur certains aspects de votre vie, d’avoir identifié quelques approches que vous voudrez mettre en place ou d’avoir déniché quelques outils à mettre dans votre arsenal.

##### Speaker
**Emmanuel Bernard**

Emmanuel est Java Champion, Distinguished Engineer et Chief Architect services cloud applicatifs chez Red Hat. Son travail est Open Source. Il est connu pour ses contributions et sa direction des projets Quarkus, Hibernate ainsi qu'à ses contributions aux standards Java.

Son aventure la plus récente est la construction d'un Kafka as a service managé par les équipes Red Hat

Il parle régulièrement dans des conférences et JUGs notamment JavaOne, Red Hat Summit et Devoxx. Il est l'hôte de plusieurs podcasts et notamment Les Cast Codeurs.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Conference |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |
| Keywords | organization health real life situations careers skills |

#### Notes
Témoignage, prendre que ce qui nous concerne / intéresse

Mesurer cohésion entre travail et bonheur. Ca n'a pas l'air de matcher trop.
Donc perception externes de la personne vs sa propre perception sont totallement différents

Super pour anticiper le futur, et très souvent de manière pessimiste
Choix purement inerne de se juger sois-même

Tout ça c'est des règles de notre firmware, on va essayer de les comprendre / les voir et éventuellement les changer

On a une liste de tâches qui en fait est infini. Elle va grandir plus vite que ce qu'on va produire.
Peut être imposée par notre supérieur, mais on a la main dessus,surtout de nos jours
Se sentir coupable de ne pas faire les petites taches insignifiantes ? celles qui ne seront jamais faites ?

Une méthode possible : **Getting Things Done (GTD)**
- Capture -> définir le done pour chaque tâche, regrouper, ...
- Process -> quelle forme, sous étapes, ...
- Organize -> le mettre dans le system pour pas le perdre
- Reflect -> faire une revenue régulière, ajouter les trucs capturées, revoir les priorités, sortir l'inutile, faire
- Do -> 

Autre méthode : **Personal Kanban**
- To Do : elle est ordonnée pour se simplifier le travail
- Doing : 
- Done : 

N'importe quelle autre méthode marche, tant que c'est bon pour vous
Il faut faire confiance 100% au système, que le cerveau ne gambade pas
Arrête les commitments artificials

Capture des besoins facile (genre sur portable), après tout sur PC. trello, papier / crayon, ...

Par contre la vie n'est pas une TODO ! On n'est pas obligés de toujours viser la prochaine tache.

Que faire pour être heureux, genre vraiment ?
Pour autant ce qui est à côté, ce n'est pas acquis ! Ne pas se reposer dessus.

Pyramide de Maslow, un exemple de fondement intéressant

Du coup, parler ! Voir un psy si besoin, mais parler. Amis / Famille compliqué car relation proche. Un professionnel (psy / coach / whatever)

Pourquoi rechercher l'efficacité ? T'es pas heureux déjà là ?

Meditation de pleine conscience pour reprendre le controle sur son attention. Travaille le muscle de la "focalisation"
Se rendre compte quand on dérive. Ca peut être tout à fait ok, mais au moins c'est en connaissance de cause

Méditation pour creuser en interne (comme un psy mais solo)

Je fais moins, pas toujours chasser le plus
C'est bien de ne rien faire

Médiatation via des bouquins, ou Petit Bambou, ...

Ce qui est important c'est pas le nombre choses faites, mais l'impact des choses faites !
C'est ok de ne pas être toujours à 100%
C'est ok de se donner du temps pour rien

Communication non violente
Centré sur soi, commencé par se comprendre avant de faire du lien avec une autre personne

Ne pas rager de ne pas arriver à changer les choses, c'est ok aussi. Tant qu'on prend du temps pour soit c'est ok.
##### Alternatives
Du bonheur dans le Craft - 241
### 15h30-16h15 (maillot): DevOps is Dead? Au fait, c’est quoi le DevOps? Et pourquoi faut-il le sécuriser?
#### Abstract
##### Talk
Après 14 ans de DevOps, il est temps pour nous de regarder ce qui font les forces et les faiblesses du mouvement DevOps. Il y cinq ans, nous avons écrit le livre “Liquid Software”. Nous étions sûrs qu’aujourd’hui la vision serait âgée et dépassée, mais elle est plus que jamais un facteur déterminant dans le succès d’une transformation digitale. Dans cette session, je parlerai du présent et du futur du DevOps dans notre industrie changeante, et comment votre entreprise peut bénéficier au maximum du mouvement DevOps.

##### Speaker
**Fred Simon**

Fred est le cofondateur de JFrog, et l’un des architectes les plus respectés de la communauté des développeurs, avec plus de 20 ans d’expérience Java et Open Source. Avant JFrog, Fred a fondé AlphaCSP, où il dirigeait 5 branches dans le monde en tant que CTO et visionnaire. Fred a traversé les évolutions de technologies dans son rôle de programmateur, architecte, consultant, et speaker. Fred est titulaire d’un Masters in Computer Science de l’école Centrale de Lille.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Conference |
| Track | Cloud, Containers & Infrastructure, DevOps |
| Presentation level | beginner/novice |
| Keywords | DevOps |

#### Notes
Ecriture de :closed_book: Liquid Software

Aider les développeurs a délivrer du software vers la production. On appelle ça du *software flow*
Les 3 secrets du DevOps : automation, automation, automation
Mais c'est pas si simple que ça

Besoin d'aide pour déployer sur des dizaines de machines, quand il y a une équipe qui travaille en parallèle

On veut pouvoir écrire son code, et qu'il arrive en prod sans aucune intervention humaine
Est-ce qu'on peut faire ça ? Certaines boites le font déjà
On identifie les tâches humaines, et on les automatise

Sauf que les humains ils aiment bien être dans le flow, faire des trucs et en être fier
Comment on fait pour l'enlever ?
On ne le fait pas, on lui donne juste les outils pour pouvoir créer les conditions du flow automatisé

##### what is devops ?
But : prendre ce qui est créé par le développeur et le mettre dans le runtime
Le DevOps est l'humain qui essaie de mettre ça en place
Attention aux étapes manuelles dans le flow devops. Du coup on n'y est pas

C'est la prochaine étape : que ce soit que des machines du développeur jusqu'au runtime
Il faut que les machines puissent review le code et la sécurité

Même si vous essayez de faire le meilleur DevOps, il y a des moments où vous allez faire un raccourci qui va bypasser le flow devops.

Prochaine étape ça va être de permettre au edge computing (téléphones, voitures, etc.) de faire du devops aussi
##### Alternatives
De l’oiseau bleu à l’éléphant : la transition vers Mastodon - 243
Gestion de la dette d'architecture dans un contexte d'hypercroissance - Amphi bleu
### 16h15-16h45 : Pause
### 16h45-17h30 (242 AB): Comment être condamné par la CNIL
#### Abstract
##### Talk
Spoiler alert: bien entendu, je ne vais pas vous apprendre à être condamné par la CNIL ! La CNIL a sorti un guide RGPD (Règlement Général sur la Protection des Données) pour l'équipe de développement, où elle donne des conseils aux développeuses et développeurs afin d'être en conformité avec le RGPD, notamment dans le développement. Dans ce talk, nous plongerons dans les bonnes pratiques recommandées par la CNIL et nous verrons aussi différents cas de sanctions données par la CNIL par des entreprises qui ont, elles, été condamnées.

##### Speaker
**Juliette Audema**

Je m'appelle Juliette, j'ai 31 ans, j'ai fait une reconversion en tant que développeuse à l'automne 2018 avec le bootcamp en Ruby On Rails "The Hacking Project". Je suis originellement diplômée de Sciences Po Grenoble (master Affaires Publiques) et j'ai été chargée de projets dans l'éducation musicale. Actuellement je suis Software Engineer chez Aircall, après avoir travaillé en start-up et dans une agence web. À côté de mon travail, je suis impliquée dans plusieurs communautés de code: les Women On Rails, pour lesquelles j'édite une newsletter (https://womenonrails.substack.com/) et Paris.rb. En-dehors du code j'aime tricoter, broder, coudre, courir et faire du yoga.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Conference |
| Track | Performance and Security |
| Presentation level | beginner/novice |
| Keywords | security best practices user experience |

#### Notes

##### Bases
CNIL autorité administrative indépendante, surveille la protection des données personnelles
18 membres de commissions, 250 agents

Mission qui nous intéresse : contrôler et sanctionner

**RGPD** : Règeement Général sur la Protection des Données (GDPR en anglais) entrée en application en mai 2018
Encadre le traitement des données personnelles sur le continent européen

Qui concerné ? toute organisation qui traite des données pour son compte ou non dès l'instant que les données personnelles viennent d'européens

##### Donnnées personnelles
Toute info se rapportant à une personne physique identifiée ou identifiable

Au sein de ces données il y a des données spécifiques, dites sensibles, notamment celles relatives à la santé des individus, vie / orientation sexuelle, raciales, ethniques, religieuses, ...

Qu'est ce qu'un traitement des données : toute opération portant sur des données personnelles quel que soit le procédé utilisé
Ex : tenue de fichier clients, fichier fournisseur, opération, commande, ....

Tout traitement doit être justifié :
- un contrat
- intérêt légitime
- consentement

la CNIL met en avant des outils pour s'assurer de la compitibilité avec le RGPD :
- registre des traitements et doc interne (le resp est le dirigeant de l'entreprise)
- la cybersécurité et notification de certains incidents
- analyses d'impacts sur la protec des données (AIPD) pour les traitements sensibles
- un délégué à la protection des données (DPO). Le/La pilote de la conformité à la RGPD

##### Chaine répressive de la CNIL
1. Le signalement d'une infraction (via plaintes sur le site de la CNIL, autosaisine de la CNIL elle-même, presse, coopération au niveau européen avec des entités équivalentes)
2. Le contrôle (sur place via des agents de la CNIL, en ligne, convocation dans les bureau de la CNIL, contrôle sur pièces)
3. Clôture du contrôle / En cas de manquements : rappels ou mise en demeure, sinon des sanctions
4. Mesures : rendre publique ses actions (mises en demeure, sanctions), ne pas rendre publique, sanctions pécuniaires ou non pécuniaires

en 2022, 345 contrôles. 21 sanctions pour un montant de 101 millions €
1/3 des sanctions concernent la sécurité des données personnelles

REX :
- Alan (Charles Gorintin)
- Fidzup (Olivier MAGNAN-SAURIN)

##### Interactions avec les users
Suite à la sortie de la loi, la CNIL a sorti un guide pour les devs.

Il faut être clair et transparent sur ce qu'on collecte et ce qu'on en fait
Il faut que ce soit facile d'accès

Permettre à l'utilisateur l'exercice de ses droits :
- droits d'accès à ses données
- droit de rectification
- droit d'opposition
- droit à l'effacement
- droit à la limitation (stopper temporairement le traitement des données)
- droit à la portabilité (récupérer ses données pour son propre usage)
- droit à l'intervention humaine

Pour ça il faut rendre visible les moyens d'effectuer les démarches pour les utilisateurs (contacts, formulaires, ....)
Egalement un suivi clair de où en est sa demande, de garder une trace

##### Reco techniques
authentification avant accès à la donnée
politique de gestion d'accès aux données différenciées suivant les personnnes
documenter / automatiser les procédures des mouvement des collaborateurs

Minmiser la collecte de données au strict minimum : adéquates pertinentes et nécessaires
Réduire la précision des données
Mettre en place des purges automatiques pour respecter les temporalités prévues
### 17h45-18h15 (amphi bleu) : Les post-mortems ou comment sortir heureux d’un carnage
#### Abstract
##### Talk
Une fonctionnalité, un projet, une réunion, ça ne se passe pas toujours bien. Loin de là. C’est parfois même un carnage. 😱 Que fait-on dans ces cas-là ? Ça s’est terminé dans les larmes, le sang et la sueur mais qu’importe ! On met tout ça sous le tapis, c’est fini, on en parle plus. 🙈

Pour que cela recommence encore la prochaine fois ? Pour que tout le monde en souffre sans oser en parler ? Pas la peine !

Alors on prend notre courage à deux mains, et on organise un POST-MORTEM. Mais qu'est-ce que c'est ? À quoi cela sert-il ? Et comment le mettre en place ? 🤔

Vous découvrirez dans ce talk les différentes étapes d'un post-mortem réussi et comment l'animer au mieux pour que chacun et chacune puisse être heureux.se après un tel carnage, sans pour autant être psychopathe ! 😌

##### Speaker
**Lise Quesnel**

Lise est développeuse web, et aime accompagner les équipes tant sur le plan humain que technique. Les bonnes pratiques de développement sont pour elle le ciment de tout projet. Grande curieuse, elle aime découvrir sans cesse de nouvelles choses.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Tools-in-Action |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |
| Keywords | team Agile Improvements mistake |

#### Notes

Quand ça merde, on organise un post mortem

Echange constructif organisé à la suite d'un événement problématique

Objectif : comprendre ce qu'il s'est passé et s'améliorer

Contrairement à la retrospectifve, le Post Mortem va être ponctuel, distinctif et intégrer des gens potentiellement au dela de l'équipe. Toutes les parties prenantes en fait.

###### Mise en place
Planifier la date et les intervenants : pas trop tard / ni trop tôt (2 semaines après la fin de l'événement à peu près). N'inviter que les personnes pertinentes, pas toute la boite
Prévoir entre 2h et 4h avec des pauses

*Définir des rôles*
- Animateur : prépare et anime le post mortem. Personne extérieure à la problématique
- time-keeper : vérifier qu'on ne perd pas de temps sur des sujets spécifiques
- scribe : prend les notes pour formater le compte-rendu

On peut combiner ces rôles mais c'est déconseillé

*Poser un cadre*

Définir les règles de l'échange:
- rappeler l'objectif (on veut s'améliorer)
- on critique des process et non des personnes
- on écoute activement (pas de coupure de parole)

On peut y mettre d'autres choses, mais ça c'est le minimum

Le cadre sert à construire un environnement sain pour que tout le monde puisse s'exprimer librement

*Revenir sur les faits*

Des faits datés, objectifs et chiffrés.
On va les poser sur une frise chronologique, trouvés avant par l'animateur. La frise sera complétée par les intervenants du Post Mortem

*Cibler*

Cibler les points de douleurs
Ne pas perdre du temps là où ce n'est pas le coeur du problème, pas les trucs parasites ou qui vont bien

Il y a plusieurs systèmes pour cibler.
Un système de votes par points (3 votes par personne)

Un autre système : vote par émotion, pour situer le sentiment de chaque intervenant vis à vis de chaque événement

*Analyser*

Identifier les problèmes

analyse:
- 5 pourquoi / analyse des causes racines
- diagramme arrête de poisson / diagramme d'ishikawa
- dérivé de l'experience map

*Solutionner*

Réfléchir aux solutions

On va brainstormer de manière timeboxée pour trouver des solutions. Il va là encore falloir choisir les solutions mises en place
Parfois il n'y a pas plein de solutions

*Agir*

S'améliorer

Les actions à entreprendre doivent être claires et précises
Chaque action doit avoir un responsable (pas forcément qui réalisera l'action, mais qui en tous cas la suivra)
On cale une date pour vérifier la mise en place

## Vendredi 14 avril 2023

### 09h00-09h20 (amphi bleu): Soyons optimiste
#### Abstract
##### Talk
Au temps des Fake News et de la Post-vérité, il est facile de se décourager et de penser que les humains sont foutus. Mais l’esprit critique réside souvent même là où on pense qu’il est absent.

##### Speaker
**Thomas Durand**

Vulgarisateur des sciences, animateur de la Tronche en Biais, Thomas C. Durand est auteur d’ouvrages sur la science et l’esprit critique (L’ironie de l’évolution, Seuil 2018. Connaissez-vous l’homéopathie ? Éditions Matériologiques 2019. Quand est-ce qu’on biaise ? Humenscience 2019. Dieu, la contre-enquête, 2022), et directeur de la rédaction de l’Association pour la Science et la Transmission de l’Esprit Critique (ASTEC).

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Keynote |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |

#### Notes
Il faut parier sur l'intelligence collective (communauté scientifique, jurés d'assises, logiciels open source, démocratie, ...)
Ex : wikipedia

On peut créer les conditions de plus de rationnalité pour l'humanité
Si on perd l'optimisme, qui cherchera des solutions ?
Car on doit continuer à changer la trajectoire et à s'améliorer
### 09h25-09h45 (amphi bleu): Géopolitique de la data
#### Abstract
##### Speaker
**Benjamin Bayart**

Expert en télécommunications, Benjamin Bayart milite pour les libertés fondamentales dans la société de l'information par la neutralité du net et le logiciel libre, ses prises de positions en font une personnalité remarquée de l'Internet français.

Il a été pendant quinze ans président de French Data Network, le plus ancien fournisseur d'accès à Internet (FAI) encore en exercice en France. Il est aujourd'hui consultant chez OCTO Technology et co-président de la Quadrature du Net, une association française de défense des droits et libertés sur Internet, qui lutte notamment pour la protection de la vie privée, la neutralité du net et la libre circulation de l'information. Fondée en 2008, elle sensibilise l'opinion publique et intervient auprès des institutions nationales et européennes pour promouvoir et défendre un Internet ouvert et libre.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Keynote |
| Track | Architecture, Performance and Security |
| Presentation level | beginner/novice |
| Keywords | Big Data Cloud. Open Source code |

#### Notes
L'ordinateur est fatal : il est inéluctable, au sens de la fatalité. C'est un problème.
Ce que l'ordinateur n'est pas capable de modéliser, ça n'existe plus. On est obligés de modifier la réalité pour la faire coller à ce que veut l'ordinateur

Tout fichier est une maltraitance. Les psy parlent de réhification : c'est le moment où on se met à considérer les gens comme des choses
Quand on met les gens en fiches, on traite des dossiers des fiches, et non plus des gens. C'est le début d'une maltraitance potentielle.

Les données sont la personne. Nous ne sommes que la somme des données sur nous.

Donc quand on fait de l'informatique on s'apprette à maltraiter des humains, de manière systémiques sans recours.
Il faut donc être prudent, qu'on le fait pour de bonnes raisons, de manière pas trop dégueulasse ni injuste.

D'où le droit sur les données.

La géopolitque c'est un rapport de puissance : imposer sa volonté à l'autre ou le subir.

Les données personelles sont en train de devnir un sujet de géopolitique.
Arrêt Scherms II : il est illégal de transférer des données européenne aux USA

Donc on créé un rapport de forces avec les USA car ils ne traitent pas correctement les données. Et donc on créer un levier en faisant la protection des données chez nous.

On ne peut pas faire une géopolitique bien quand on part du principe qu'on est des quiches. Il faut donc que l'on utilise du droit des personnes pour favoriser le business numérique en Europe.
### 09h50-10h10 (amphi bleu): Briser le plafond de glace
#### Abstract
##### Talk
Relever les défis, s’entourer des bonnes personnes, s’entrainer, échouer et s’entrainer encore… Telles sont les règles que Marion Poitevin s’est appliquée afin tracer une nouvelle voie professionnelle pour les femmes. Des expéditions lointaines de l’Himalaya au cercle polaire ou encore l’Antarctique, des pentes raides à ski à Chamonix aux parois déversantes de Californie, autant d’aventures qui lui ont permises d’atteindre le meilleur niveau féminin en alpinisme et de passer tous les diplômes des sports de montagne. Aujourd’hui secouriste CRS montagne en Savoie elle enchaine de nouveaux défis pour concilier ses activités avec sa vie de maman. Ce nouveau cheminement de la maternité l’amène à réfléchir à son environnement montagnard, non plus comme un espace de performance mais de Vie. Ou comment devenir féministe puis aussi écologiste !

##### Speaker
**Marion Poitevin**

Première dans un groupe d’élite d’alpinisme de l’armée de terre, 1ère instructrice montagne après 80 ans d’existence de l'Ecole Militaire de Haute Montagne et enfin 1ère secouriste CRS Montagne dans la police nationale. Marion Poitevin est aussi l’une des 35 femmes guides de haute montagne, monitrice de ski alpin et maman de deux enfants. A seulement 37 ans elle vous présente comment "Briser le plafond de glace" dans des mondes jusque là exclusivement masculins, les difficultés rencontrées, les solutions trouvées et les leçons apprises. Aujourd’hui son engagement est féministe mais aussi écologique face à la problématique du développement toujours grandissant des stations de sports d’hiver.


##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Keynote |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |
| Keywords | team inspiration optimizations |

#### Notes
Fait que des jobs où y'avait pas de femmes avant, donc il y avait tout à créer

Optique de performance dans l'équipe féminine de haut niveau d'alpinisme.
C'était plus facile le faire avec des femmes, que des hommes même s'ils n'avaient pas de mauvaises intentions.

Job de rêve qui s'arrête, à cause d'un supérieur qui sexualise la personne alors qu'elle ne l'était pas avant. Du coup il fermes les portes, même si c'est illégal
### 10h45-11h30 (amphi bleu): Clean as You Code your projects
#### Abstract
##### Talk
On veut tous un code de qualité - “Clean Code”. Mais à mesure que notre code et nos équipes s'agrandissent, il peut être difficile de maintenir cette norme. Dans cette présentation, nous aborderons les bénéfices du “Clean Code” et comment vous concentrer sur ce point aidera votre organisation et vous même à prospérer. Nous approfondirons le concept de “Clean as You Code” et les outils qui garantissent un code adapté au développement et à la production.


##### Speakers
**Nolwenn Cadic**

Nolwenn fait partie de l’équipe SonarCloud où elle fait du développement Java. Elle a rejoint l’équipe il y a un an. Elle y cultive son goût du Clean Code et le met en pratique tous les jours. Avant l’aventure SonarSource, elle a travaillé 2 ans comme développeur full stack Java et React.

**Marco Comi**

Marco Comi est un Chef de Produit avec plus de 13 ans d'expérience dans l'industrie du logiciel. Il a commencé sa carrière comme ingénieur logiciel, où il a développé une profonde appréciation du Clean Code et de l'importance de soutenir les développeurs dans leur quête pour l'écrire. Il a ensuite travaillé comme Scrum Master, Product Owner et finalement a effectué la transition vers la gestion de produits. Il travaille chez SonarSource et supervise le développement de SonarLint depuis 2020.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Conference |
| Track | Cloud, Containers & Infrastructure, DevOps |
| Presentation level | beginner/novice |
| Keywords | developer productivity code quality Java CI/CD practices DevOps |

#### Notes
Aujourd'hui le code source il est partout, même dans l'infra, et il faut donc bien s'en occuper

**Clean Code**
Différents types de codes :
- principal : l'application
- tiers : ce qui va aider, les libraires, dépendances, frameworks
- support : tests & co
- non conventionnel : mathlab & co

Le code doit être adapté à un but :

adapté aux devs :
- clair
- cohérent : respecter des conventions, architectures, indentation
- structuré : simples, indépendants, ...
- testé

adapté à la prod :
- robuste
- sécurisé : pas de vulnérabilités
- portable : du point de vue hardware et software
- conforme : coformité légale et autres

Le Clean Code minimse l'effort et le coût d'entretien de 40%
Réduit la friction pour les devs : 60%
Augmente la longévité du logiciel x3
réduit les risques opérationnels, de réputation, de sécurité de 90%

Mais pourquoi c'est pas du clean code du coup qu'on fait ?
- apprentissage continu
- différents niveaux
- standards évoluent
- vérification des standards fastidieuse
- nouvelles fonctionnalités à fournir

Analyseurs statiques qui vont faire sortir 1000 erreurs d'un coup, pas le temps, flemme

**Approche Clean code**

NE JAMAIS REPARTIR DE 0

Ne pas faire un gros refactoring

Clean Code c'est on règle le problème avant de gérer les conséquences

Le nouveau code doit être du Clean Code
Cela permet de régler le problème avant les conséquences.
On ne bloque pas les éovlutions. Plus de choix entre des sprints de qualité ou des sprints normaux

En plus se faisait, on va améliorer ses compétences et techniques de code

**Mise en place**

Aucun nouveau code Code Clean ne sera déployé en PROD

Comment on vérifie : on utilise une Quality Gate :
1. Utilisation de SonarLint
2. SonarCloud et SonarQube pour analyse les PR et vérifier la quality. La pipeline est configurée pour ne pas merger les PR qui ne passent pas les contrôles de qualité avec succès
3. Dernier check SonarCloud ou SonarQube avant le déploiement

Faire du bon code et du mauvais code prennent sensiblement le même temps, donc on ne gagne pas à faire du code de merde

**Combien ça coûte**

Du coup mécaniquement, on clean aussi le passé :
- 20% dans un an
- 35% au bout de 2 ans
- 50% au bout de 5 ans

Donc c'est intégré dans le processus de dev, ça ne coûte pas plus cher

#### Alternatives
Incident Management - Talk the Talk, Walk the Walk - 253
Qualité radicale - de Toyota à la tech - 241
Biais & Balivernes - 242 AB
Beyond cluster-admin: Getting Started with Kubernetes Users and Permissions - 243
### 11h45-12h30 (amphi bleu): SpringBoot 3 - :warning: A REVOIR :warning:
#### Abstract
##### Talk
Spring Framework 6 and Spring Boot 3 are here, and you know what that means. New baselines and new possibilities! Spring Framework implies a Java 17 and Jakarta EE baseline and offers new support for building GraalVM-native images and a compile-time component model in the new Spring AOT engine. It also offers a new observability layer, declarative HTTP and RSocket clients, preliminary Project Loom and CRaC support, Problem-Details support, and so much more. Join me, Spring Developer Advocate Josh Long (@starbuxman), and we'll look at next-gen Spring.

##### Speaker
**Josh Long**

Josh (@starbuxman) is a Spring Developer Advocate at VMware, an open-source hacker, book/video author, Java Champion, Kotlin Google Developer Expert, and speaker

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Conference |
| Track | Java, JVM |
| Presentation level | Intermediate |
| Keywords | developer productivity code quality Java CI/CD practices DevOps |

#### Notes

release lors du dernier ThanksGiving
De gros changements, très importants

Java 17+ pour SpringBoot 3, ne pas partir sur du plus vieux

#### Alternatives
Éviter les biais dans la conception de logiciels - Maillot
FoundationDB : le secret le mieux gardé des nouvelles architectures distribuées ! - 241
Miroir, miroir dis-moi si je suis observable? - 242 AB
### 12h30-13h30 : REPAS
### 13h00-13h15 (252 AB): Comment garder son job, quoi qu'il en coûte
#### Abstract
##### Talk
D'après le principe de Peter « dans une hiérarchie, tout employé a tendance à s'élever à son niveau d'incompétence ». En me basant sur le livre Simple Sabotage Field Manual, publié par le United States Office of Strategic Services (OSS, aujourd'hui la Central Intelligence Agency) en 1944 ainsi que d'autres papiers sur la toxicité au travail. Je vais donc vous apprendre comment vous pourrez garder votre travail le plus longtemps possible grâce à un ensemble de compétences et de techniques/biais cognitifs afin de vous rendre irremplaçable, ou de vous permettre de débusquer des personnes usant de ces techniques...


##### Speaker
**Raphael BLEMUS**

Ingénieur en informatique de formation, je travaille depuis plusieurs années sur des produit backend et frontend dans des secteurs variés (tourisme, banque/assurance, divertissement, publicité). Passionné par la programmation, je veille à appliquer les principes de software craftsmanship, tests automatisés (unitaires, fonctionnels, end-to-end) et DDD (Domain Driven Design) dans un souci d'amélioration continue. Je prône le pragmatisme et l'efficacité des méthodes de travail afin d'apporter le plus de valeur. A côté, j'adore apprendre et faire de nouvelles choses comme la modélisation 3d, l'escalade, la poterie, changer les couches, etc...

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Quickie |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |
| Keywords | methodology developer problems Management |

#### Notes
Principe de Peter : dans une hiérarchie tout employé tend à s'élever au niveau de son incompétence

1944 : Simple Sabotage Field Manual :closed_book: déclassifié en 2008

Comment ralentir les autres, plomber leur travail. Bien suivre les canaux hiérarchiques, siloter les silos. Bref ralentir un max
Obstruction et perturbation : causer de la confusion, pollution sonore et visuelle, propager des fausses infos
Mettre en place de l'inefficacité délibérement (pourrir du code, le complexifier)

Autre technique : Dungeon Master
Il est bien rattaché au sujet, maitrise le sujet. Il ne fait plus de développements, mais reste proche du sujet, pour son aide et expertise.
même si ce n'est pas son objectif il est néfaste car il garde le contrôle sur le projet

:closed_book: Blog d'Alberto Manolini pour plus d'infos

Mettre en place des objectifs flous :
- améliorer le designe de l'appli
- x2 sur le nombre d'utilisateurs

Vous êtes surs de garder votre boulot car la boite a besoin de vous, et aura besoin de vous garder, le temps de remplacement sera long !

**Comment inverser**
10k heure pour devenir expert, mais comment le devenir ?
- environnement stable
- répétition
- feedback rapide
- entrainement délibéré

**Faire des katas en tant que dev ça peut être très efficace pour progresser**

**Renverser le Dungeon Master : lui faire perdre le controle à tout prix.**

Tout rendre transparent, compréhensible, accessible. Partager la connaissance
Tracer les décisions importantes, décisions architectures

Faire attention à ne pas devenir l'idiot du village, ne pas challenger trop fort trop vite
Sinon le DM va vous sortir des sujets. Il faut y aller progressivement avec les autres

**Définir des bonnes métrics : SMART / OKR qui soient bien mesurables**

Choisir ses sujets et les prioriser

:closed_book: the productivity project

Matrice d'eisenhower si besoin pour savoir prioriser. Savoir déléguer ou supprimer au besoin

Mais voulez-vous être un saboteur ? Essayer de garder le bon environnement et surtout pas un environnement toxique

retrouver biblio de la pres

#### Alternatives
Le Maître du Donjon: Maîtriser la cybersécurité en créant des challenges CTF - 251
### 13h30-14h45 (amphi bleu): Les tests de charge sans être à côté de la plaque
#### Abstract
##### Talk
Bon, ça y est, vous devez vous mettre en place des tests de charge, probablement parce que votre application souffre en production, ou qu'un gros événement se prépare, comme une refonte, une feature majeure ou un gros pic de traffic.

On ne va pas se mentir: pour beaucoup, c'est un peu la poule qui a trouvé un couteau.

Depuis 12 ans que je travaille dans le domaine des tests de charge, j'observe trop souvent la même mauvaise approche: vouloir un nombre d'utilisateurs ou de requêtes par seconde et se demander quel outil utiliser et avec combien de machines. Bref, sauter complètement la phase de conception.

Ce talk propose de dérouler un ensemble de questions à se poser pour pouvoir monter une stratégie de test de charge pertinente, depuis la conception de traffics réalistes à la définition des critères d'acceptance.

##### Speaker
**Stéphane LANDELLE**

Je bricole depuis un bon moment en Java et Scala et trouve encore le temps m'amuser avec différents projets open-source: Gatling que j'ai créé et dont je suis le CTO, scala-maven-plugin et un peu Netty.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Conference |
| Track | Architecture, Performance and Security |
| Presentation level | beginner/novice |
| Keywords | load tests performance network |

#### Notes
Dans l'ingénierie, généralement, on construit des choses extrement complexes avec succès (avions, trains, vaccination, ...)
Mais dans l'IT on galère, on a souvent des soucis, même sur les grosses boites => donc ce sont nos pratiques qui nous gènent

Souvent QA conditions != Prod conditions. Donc on est souvent en mode YOLO

Avoir de mauvaises performances, ça peut impacter fortement votre business, votre marque, et à terme vos revenus
Vous risquez d'endommager la productivité de vos utilisateurs

Souvent on dit qu'on s'en fout car on n'a pas de grosse charge => C'est pas vrai
Il suffit de quelques users qui font de la merde pour faire tomber une app

- dead locks
- race conditions
- pagination DB
- fuites mémoire
- ...

Ou alors on entend qu'on est en autosacling. Sauf que ça met du temps à réagir, ça coûte de l'argent, ajoute de la complexité et autres

**Donc pourquoi du test de charge ?**

Ce n'est pas de la web perf (optimisation du chargement et des rendus)
Test de charge = problématique côté serveur sur du tanking d'activité de prod

Du coup ce sont des outils qui vont travailler au niveau réseau. on cherche à simuler des conditions existantes de prod, soit à simuler des scénarios plausible pour votre prod
Se fixer des objectifs de performance, analyser les erreurs et temps de réponse, bugs, et surtout on itère

Concepts :
- *virtual user* : user simulé avec son propre état mémoire, séparé des autres
- *scenario* : les parcours utilisateur que les virtual users vont jouer
- *injection profile* : la manière dont on va démarer des users virtuel à chaque seconde. Pour simuler diff cas de trafic

###### Comment faire des tests réalistes ?

**Scenario**

Application statefull / stateless ?

Stateless on peut facilement rejouer du traffic de prod réel. On se fiche de l'ordre d'execution (site de contenu)

Statefull => logique métier, on va impacter la DB et donc il y a un cheminement logique à réaliser. L'ordre a de l'importance. (e-commerce avec panier / commande ...)

Par où commencer :
- focus sur les parties critiques de l'app
- voir avec votre Project Owner
- verifier ses analytics
- ITERER constamment

**Data**

Attention de ne pas tester les caches applicatifs, web, de couches et donc avoir des super temps de réponse. Sauf que ce ne sera pas le cas en PROD
Eviter les valeurs en dures, injecter de la data dans les users virtuels

**Connection behaviour**
On a biai développeur, on va se focus sur notre stack. Mais en fait il y a le hardware, le reseau, load balancers, routeurs and co

En plus ce sont des soucis que vous ne verez pas dans votre monitoring car les machines sont après ces bottlenecks

Donc il faut voir ce qu'on cherche à simuler. Des centaines de browsers ? Mais on peut aussi simuler du traffic entre 2 applicatifs et non pas avec des clients
Faire des tunings qui soient réalistes par rapport aux uses cases

**Cas**

Dans un systeme ouvert
Les utilisateurs arrivent en continu
Le fait qu'il y ai de plus en plus d'utilisateurs concurrents c'est l'output du truc

Dans un système fermé
C'est le principe de la queue. Quand le systeme est à capacité full, les futures tâches sont mises en attente
Donc on a cappé le nombre de users concurrents. Donc on ralenti le système et l'entrée de nouveaux users

**Profils d'injection**

- Test en capacité => on augmente progressivement la charge jusqu'à l'écrouler
- Stress test => rush énorme d'un coup. On s'intéresse au comportement pendant le pic, et après pour voir si l'état redevient normal, ou alors elle est morte et il faut recover
- Soak Test (test endurance) => charge constante sur une longue période de temps. Observer des events qu'on ne voit pas sur le temps cours (fuite mémoire, fuite ressources, events périodiques non prévus)
- Smoke test => faux test de charge qui consiste à faire tourner le scénario avec un seul user, pour s'assurer que le test fonctionne. On doit maintenir les tests de charges

###### Mesurer la performance

Première métrique : taux d'échec. Identifier les cas à fixer

Les temps de réponse : ne pas utiliser la moyenne des temps de response comme métric, sauf s'il est mauvais bien entendu !
Du coup on utilise des limites via les percentiles
C'est parfait pour définir des SLA, c'est facile à grapher
Le calcul exact est complexe et lourd car il faut stocker énormément en mémoire
Donc on va partir sur des approximations, via par exemple du sampling (mais mauvaise idée, on perd beaucoup de données), en prédisant la distribution (ce qui est invariablement faux).
Ce qui marche bien est l'utilisation d'histogrammes, donc on se base sur des approximations (htttp://hdrhistogram.org)

Le danger va être d'agréger les centiles => ça ne marche pas du tout. Vous ferez disparaitre toutes vos mauvaises valeurs

###### Méthodologie

Tester le plus tôt possible, monter des pocs. Ne pas attendre la veille de la prod

Ne pas confier ça à des équipes centralisées et ignorantes. Ca doit faire partie du job de l'équipe projet. Il faut le confier aux développeurs car ils sont au plus proche du sujet.

Si l'archi est complexe, tester les appli indépendemment. Quand c'est ok, on envoie les tests E2E

Le plus de dépendances, le plus on a de risques que ça claque

Pour faire tout ça on a besoin de pas mal d'outils (provisionning, montoring, load generator)

##### Gatling

**Tests avec du code !**

Supporte plein de protocoles, pas que Web

Version entreprise pour des fonctionnalités en plus

[site gatling](https://gatling.io)

#### Alternatives
Bâtir des équipes d’ingénierie logicielle mémorables.  - 252 AB
Le théorème de Bayes : passés l'oubli et les polémiques, la formule qui a changé le monde - 241
### 14h30-15h15 (242 AB): Des millions d’évènements par minute pendant une Coupe du Monde, même pas peur !
#### Abstract
##### Talk
Chez Betclic nous avons mis en place une architecture basée sur les évènements. Après quelques années d’exploitations et une Coupe du Monde 2022 réussie, nous vous proposons un retour d’expérience sur les mécanismes que nous avons mis en place (gestion de la reprise des évènements, des dead letter, des logs, etc).

Nous détaillerons l’architecture, basée sur des services AWS, et les libraires que nous avons implémentées. Nous parlerons aussi de l’outil AsyncAPI qui nous permet de valider le format de nos messages. Nous reviendrons sur ce qui a bien fonctionné, les limites et les améliorations que nous avons apportées au fur et à mesure des années.

Nous vous présenterons également quelques métriques liées à la Coupe du Monde de football 2022 et comment nous monitorons cette architecture.

##### Speaker
**Nicolas JOZWIAK**

Nicolas est Senior Engineering Manager disposant de 16 ans d’expérience en conception et développement. Son parcours chez un éditeur et une société de consulting avant son entrée chez Betclic lui a notamment permis de développer de solides compétences dans le domaine de la qualité et de l’industrialisation (tests, intégration continue, gestion de configuration, contrôle qualité). Bénéficiant d’une expérience très solide de mise en place des méthodes agiles et d’accompagnement d’équipes sur le terrain, il s’attache à mettre à profit quotidiennement son expérience qui est reconnue pour son approche pragmatique, proactive et pédagogique.

**Guillaume Lannebère**

Responsable du Cloud Center Of Excellence au sein de Betclic en charge de toute la partie gouvernance, automatisation et bonnes pratiques. Egalement membre de la communauté AWS Community Builder, je me suis spécialisé et passionné par tout ce qui touche aux architectures cloud et l'automatisation des infrastructures.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Conference |
| Track | Architecture, Performance and Security |
| Presentation level | beginner/novice |
| Keywords | Event-Driven Microservices performance architecture |

#### Notes

Dans le cadre de la finale de coupe du monde 2022, match spécial de par son scénario (buts, prolongations, pénalties, ...).
Du coup pas un cas "normal", non prédictif et il a fallu encaisser

Avant c'était un monolithe distribué, une seule application, pleine de sous modules

Il y avait donc une dette non maîtrisée, mais surtout non maîtrisable

Des idées : DDD, microservices, mais comment faire ? Et on va regarder l'organisation de la société, car elle va impacter dans l'archi. On va essayer de le casser le monolithe mais il fallait en tenir compte

Dans l'ancienne équipe, une seule équipe de développement, pas de responsabilisation, de fortes disparités techniques et fonctionnelles

Du coup on va mettre des critères importants en place :
- construire une culture produit
- gagner en autonomie
- taille humaine
- amélioration continue
- you build it you run it

En reconstruisant l'équipe, les silos restaient présents => on va mettre des référents dans chaque équipe (Archi, Data, SRE, ...). L'objectif est que l'équipe soit autonome sur son périmetre

En faisant comme ça, la dette a été rattrapée et la velocité a augmenté

2 milliards de messages publié sur le mois, Du coup 9 milliards consommés par les différents micro services
Pic de plus de 1.5 million de messages simulatnés

Avant l'ancienne arcih faisait que si sur l'ensemble de la chaine il y avait du soucis, on introduisait une latence voir l'empêchement de parier. Or certaines étapes pouvaient être  asynchrones sans soucis.
Donc le choix a été fait d'une architecture EVENT via un bus EVENT

Les besoins techniques de l'archi identifiés à l'époque :
- lectures multiples
- filtrer les events (suivant par exemple les contraintes légales)
- remise d'un event au moins une fois (pas de perte !)
- reprise sur erreur
- facilité à monitorer
- registre de schema (on veut être sur que le message publié peut être consommé)
- indépendant du langage de programmation
- capacité d'analyse (facilement faire des stats et de l'analytique)
- entierement managé (on est pas des ops, laissons le taff à des spécialistes, pas nous)
- mise à l'échelle auto (auto scaling, car on ne sait pas prédire le traffic, ni les pics)
- peu latence et hautement dispo
- bien intégré avec AWS
- facile à utiliser

Chaque domaine du DDD a son propre compte AWS. Ils ne communiquent pas, on ne touche pas à ses ressources (AWS Lambda et Fargate, SQS).
Il y a également un compte centralisé qui contient Amazon SNS (facteur avec option photocopie, mais en plus il peut filrer au besoin)

Le betting ne s'occupe que de publier dans le topic betting, pas ailleurs. Pareil pour le paiement
Le betting ne va écouter que des messages des autres domaines.
Mise en place de Dead Letter Queues pour gérer les messages foireux

Après ça il manquait :
- remise au moins une fois de message
- reprise sur erreur
- schema registry
- capacité d'analyse

Du coup il faut palier à ces soucis
- remise au moins une fois -> écriture sur un fichier si SNS ne répond pas
- logger sur un espace partagé les message en erreurs
Tous ces messages arrivent sur SQS et trigger une lambda, regarder à qui les message appartiennent et les republier

Reprise sur erreur et analyse:
- service kiesis data file host, passer de données chaudes à froides, sur un S3
Ca part ensuite dans un datalake et après on peut faire des dashboards et requêter les données. Latence de 10 minutes actuellement
Il y a ensuite une lambda de replay en cas de besoin

Registre de schéma:
C'était uniquement hors production, nécessitant l'appel à une API, faire un contrat custom dans git et **créer une librairie de publication spécifique**
C'était compliqué, ça ne marchait pas très bien, mais ça allait
Maintenant tentative d'utiliser AsyncAPI pour contractualiser les messages. On va échantillonner les messages car on n'a pas la capaciter de tout vérifier

Du coup maintenant c'est facile de migrer / mettre à jour grâce au découplage.
### 15h30-16h15 (amphi bleu): Voyage au centre de la Veille : Apprendre en continu avec sa veille technologique
#### Abstract
##### Talk
React 32 ? Groovy 12 ? Kotlin 7 ? Docker 202 ? Des technologies inconnues déferlent chaque jour des sombres recoins de l'internet. Le temps de vous y intéresser est difficile à trouver : ni votre projet, ni votre organisation personnelle ne le permettent. Et finalement, quand arrive le moment de s’y atteler, vous manquez d’efficacité.

Vous aimeriez entretenir vos connaissances et monter régulièrement votre niveau sur votre domaine d'expertise. Ou encore, satisfaire votre curiosité en découvrant de nouvelles compétences. Un monde s'ouvre à vous avec une myriade de possibilités, mais comment savoir par quel bout commencer ?

Alors, acceptez la quête de votre veille personnelle et partons à la recherche du livre de la connaissance. Découvrons un processus et des outils pour transformer votre veille. Vous aurez alors les clés pour mettre en place votre propre contenu utile et exploitable pour aller plus loin dans votre quotidien. Le résultat : un second cerveau qui vous accompagnera toute votre vie.

##### Speakers
**Fabien Hiegel**

Développeur passionné ; quel que soit le langage de programmation, tout est prétexte à créer et à y prendre du plaisir. Par mes diverses expériences, j'ai découvert et aiguisé mes pratiques du craft, notamment par des ateliers de partageet de veille collective.

En animant des communautés de pratiques, en facilitant l'internalisation de formations ou encore, en ludifiant des ateliers de pratique,j'espère pouvoir transmettre cette passion aux personnes qui m'entourent et les aider à apprendre les compétences techniques qui les font rêver.

**David FRANCK**

Développeur chez Shodo à Nantes. Je fais principalement du back en Java. Je suis passioné de craft, de veille et de partage de connaissances. Je suis aussi auteur de jeux de société.

##### Information
| Information | Description |
| ----------- | ----------- |
| Presentation | Conference |
| Track | People & Culture, Soft Skills, Future of work, Innovation, Side projects |
| Presentation level | beginner/novice |
| Keywords | methodology learning techniques skills |

#### Notes

Non assisté

#### Alternatives
L'étonnante efficacité des petits pas - Maillot