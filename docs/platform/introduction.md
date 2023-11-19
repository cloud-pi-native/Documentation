# Introduction

Le PaaS (Platform-as-a-Service) est une forme de cloud computing dans laquelle la plateforme logicielle est fournie par un tiers. D'abord destiné aux développeurs et aux programmeurs, le PaaS permet à l'utilisateur de développer, d'exécuter et de gérer ses propres applications, sans avoir à créer ni entretenir l'infrastructure ou la plateforme généralement associée au processus.

Les plateformes PaaS peuvent s'exécuter dans le cloud ou sur site. En ce qui concerne les offres gérées, le fournisseur héberge le matériel et les logiciels sur sa propre infrastructure et met à disposition de l'utilisateur une plateforme, sous la forme d'une solution intégrée, d'une pile de solutions ou d'un service.

## Description de la plateforme

La plateforme qui s'appelle "Cloud π Native" est composée d'une **plateforme de services** à destination des équipes DevSecOps et hébergée au sein d'un **cloud souverain**. 
La plateforme est constituée de: 

- Cluster d'orchestrateur de conteneur qui est Kubernetes
- Une usine de logicielle sécurisée et interconnectée composant l'offre de services DevSecOps. Cette offre de services est **Open Source**. 
- Une [console](https://github.com/cloud-pi-native/console) Web **Open Source** consommant ses services afin de construire et déployer vos ressources applicatifs (projets, membres, environnements, etc). Il est aussi possible d'enregistrer des services custom gràce à son architecture à plugins. Chaque plugin s'enregistre sur des hooks liés au cycle de vie du projet (création d'un projet, d'un environnement ou d'un dépôt, ajout d'un membre, etc...). Les plugins enregistrés recoivent l'ensemble des informations liées aux actions sur les projet (Par le gestionnaire des plugins).

Le rajout d'un nouveau plugin est détaillé [ici]()

## Architecture fonctionnelle de la plateforme

![archi](/img/architecture.png)

## Services core proposés par la plateforme

Liste des services de la plateforme :

| Service        | Description                                | Obligatoire |
| -------------- | ------------------------------------------ | ----------- |
| Argocd         | Outil de déploiement automatique (GitOps)  | Oui         |
| Harbor / Trivy | Hébergement / analyse d'image de conteneur | Oui         |
| Gitlab         | Hébergement de code et pipeline CI/CD      | Oui         |
| Kubernetes     | Création des ressources kubernetes         | Oui         |
| Nexus          | Hébergement d'artefacts                    | Oui [1]     |
| Sonarqube      | Analyse de qualité de code                 | Oui         |
| Vault          | Hébergement de secrets                     | Oui         |

[1] : *Instanciation au niveau du projet obligatoire mais utilisation selon le besoin.*

## Ambition

L'ambition de cette plateforme est de proposer aux équipes projets une offre de services large, complète déployée au sein d'un cloud souverain et permettant d'automatiser la construction et le déploiement de vos projets en assurant les standards de qualité et sécurité. 

Pour pouvoir manipuler les services de cette plateforme, la première étape consister à synchroniser le code applicatif depuis un repo externe accéssible depuis la plateforme. 

La chaine de CI/CD sur le Gitlab de la plateforme permet:

- Lancer vos jeux de tests applicatif (unitaires, de bout en bout, ...).
- Effectuer une analyse de la qualité de votre code source à l'aide du service [Sonarqube](https://www.sonarqube.org/).
- Construire vos images de conteneur de l'application dans la CI/CD du service [Gitlab](https://about.gitlab.com/).
- Scanner vos images et le code source à l'aide de [Trivy](https://aquasecurity.github.io/trivy).
- Stocker vos images dans un registry de conteneurs dans la plateforme: [Harbor](https://goharbor.io/).

Une fois l'application construite et les images de cette dernière stockées dans la registry de la plateforme, le déploiement sur un cluster kubernetes pourra être effectué selon le modèle gitops à l'aide du service [ArgoCD](https://argo-cd.readthedocs.io/en/stable/).
