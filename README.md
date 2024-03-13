# Projet admin-app deploiement avec kubernetes

Ce projet est un projet Spring Boot avec une base de données mysql et keycloak pour la securité. Il utilise Java 8, maven, docker, logs,...

# Explication generale sur le Déploiement de MySQL sur Kubernetes de keycloak et l'application springboot

Ce projet utilise Kubernetes pour déployer un service MySQL avec un volume persistant.

## Description

### PersistentVolumeClaim

Il décrit la revendication de volume persistant (PVC) pour le stockage utilisé par MySQL.

- **Name**: Nom de la revendication de volume persistant.
- **AccessModes**: Modes d'accès au volume (ReadWriteOnce dans ce cas).
- **Resources**: Configuration de la capacité de stockage.

### Deployment

Il définit le déploiement de MySQL.

- **Name**: Nom du déploiement.
- **Replicas**: Nombre de réplicas de conteneur MySQL (1 dans ce cas).
- **Containers**: Configuration du conteneur MySQL, y compris l'image Docker, les variables d'environnement (comme le mot de passe root et le nom de la base de données), les ports et les points de montage du volume.
- **Volumes**: Configuration du volume persistant monté dans le conteneur MySQL.

### Service

il définit le service Kubernetes pour exposer MySQL.

- **Name**: Nom du service.
- **Type**: Type de service (NodePort dans ce cas).
- **Ports**: Configuration du port du service.
- **Selector**: Correspondance des labels pour cibler les pods.

## Instructions pour Déployer

1. Assurez-vous que Kubernetes est correctement configuré et en cours d'exécution. Il daut donc au préalable installé minikube et le démarrer avec `minikube start`
2. Utilisez `kubectl apply -f db-deployment.yaml` pour créer le déploiement MySQL.
3. Utilisez `kubectl apply -f keycloak-deployment.yaml` pour créer le déploiement Keycloak.
4. Utilisez `kubectl apply -f app-deployment.yaml` pour créer le déploiement de l'application

## IMPORTANT
Pour que la configuration passe, il est important que dans l'application springboot, au niveau de `src/main/resources/application.yml` de remplacer les hosts par les noms des services respectifs deployés dans kubenetes pour mysql et pour keycloak comme suit:

```
  datasource:
    url: jdbc:mysql://mysql-admin-db:3306/adminapp-db?
```
et 
```
provider:
          keycloak:
            issuer-uri: http://keycloak-service:8080/realms/SpringBootKeycloak
            user-name-attribute: preferred_username
      resourceserver:
        jwt:
          issuer-uri: http://keycloak-service/realms/SpringBootKeycloak
```



### Bien à vous `M.N.21`

