# Introduction

Le PaaS (Platform-as-a-Service) est une forme de cloud computing dans laquelle la plateforme logicielle est fournie par un tiers. D'abord destiné aux développeurs et aux programmeurs, le PaaS permet à l'utilisateur de développer, d'exécuter et de gérer ses propres applications, sans avoir à créer ni entretenir l'infrastructure ou la plateforme généralement associée au processus.

Les plateformes PaaS peuvent s'exécuter dans le cloud ou sur site. En ce qui concerne les offres gérées, le fournisseur héberge le matériel et les logiciels sur sa propre infrastructure et met à disposition de l'utilisateur une plateforme, sous la forme d'une solution intégrée, d'une pile de solutions ou d'un service.

## Description de la plateforme

Cloud π Native est une plateforme de services à destination des équipes DevOps et une [console](https://github.com/cloud-pi-native/console) permettant de consommer ses services pour la construction et le déploiement des ressources applicatifs (projets, membres, environnements, etc).

Pour pouvoir enregistrer des services custom, une architecture à plugins est utilisée pour la console. Chaque plugin s'enregistre sur des hooks liés au cycle de vie du projet (création d'un projet, d'un environnement ou d'un dépôt, ajout d'un membre, etc...). Les plugins enregistrés recoivent l'ensemble des informations liées aux actions sur les projet (Par le gestionnaire des plugins).

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

## Ambitiom

L'ambition de cette plateforme est de proposer aux équipes projet une offre de services large, compléte permettant de réaliser la construction et le déploiement de vos applications en assurant les standards de qualité et sécurité. 

Pour pouvoir manipuler les services de cette plateforme, la première étape consister à synchroniser le code applicatif depuis un repo externe . 


La chaine de CI/CD sur le Gitlab de la plateforme permet:

- Lancer les jeux de tests applicatif (unitaires, de bout en bout, ...).
- Effectuer une analyse de la qualité du code source à l'aide du service [Sonarqube](https://www.sonarqube.org/).
- Construire les images de conteneur de l'application dans la CI/CD du service [Gitlab](https://about.gitlab.com/).
- Scanner les images et le code source à l'aide de [Trivy](https://aquasecurity.github.io/trivy).
- Stocker ces images dans un [Harbor](https://goharbor.io/) hébergé par la plateforme.

Une fois l'application construite et les images de cette dernière stockées dans la registry de la plateforme, le déploiement sur un cluster kubernetes pourra être effectué selon le modèle gitops à l'aide de [ArgoCD](https://argo-cd.readthedocs.io/en/stable/).
