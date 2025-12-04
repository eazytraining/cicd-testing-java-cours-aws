# Installation de Jenkins

Jenkins est un serveur d'integration. Il nous permettra de mettre en place notre chaine CI/CD. Nous allons l'installer sous forme de conteneur dockern sur notre machine aws.

####  Téléchargement de l'image jenkins
```
docker pull jenkins/jenkins
```

####  Lancement du conteneur jenkins
```
docker run -d -u root --name jenkins-launch1 -p 8080:8080 -p 50000:50000 -v jenkins_launch1:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkins/jenkins:latest
```
![launch_jenkins_server](images/launch_jenkins_server.png)
 
 Une fois le conteneur up, connectez vous au serveur jenkins dans le navigateur pour finaliser l'installation, il est disponible sur le port **8080**
 ![jenkins_first_connexion](images/jenkins_first_connexion.png)

 Maintenant, aller récupérer le token à l'endroit précisé et renseignez le dans le formulaires
 ```
 docker exec -it jenkins-launch1 cat /var/jenkins_home/secrets/initialAdminPassword
 ```
 ![jenkins_token_install](images/jenkins_token_install.png)
 ![plugins_install](images/install_initial_plugins.png)
 
 Attendre que l'installation se termine. Ca prendra envirrons **2 min** selon votre machine.

 ![plugins_installing](images/jenkins_installing_plugin.png)

 Une fois terminé, créer votre utilisateur et ses crédentials.
 On prendra ceci pour être uniforme : 
 - Nom : **jenkins**
 - Password : **jenkins_devops**
 - Nom complet : **jenkins Devops**
 - Couriel : **jenkins@jenkins.jenkins**

![create_jenkins_use](images/create_jenkins_user.png)
![jenkins_url](images/jenkins_url.png)
![jenkins_ready](images/jenkins_ready.png)

Jenkins est tout prêt à l'emploie à présent
![jenkins_dashboard](images/jenkins_dashboard.png)