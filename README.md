# Installation de SonarQube
On va utiliser **Sonarkube**. Celà sous entend l'installation du serveur sonar et la configuration de l'agent Sonar. L'architecture de sonar est la suivante : 

![Archi SonarQube](Images/archi%20sonar.PNG)

### Démarrage du serveur sonar conteneurisé :
Tapez la commande suivante sur la VM aws: 

```
docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:lts
```

Il y aura téléchargement de l'image docker, ensuite lancement du conteneur. Il sera en écoute sur le port **9000** de l'IP machine.
![Launch SonarQube](Images/Lancement%20du%20conteneur%20SonarQube.png)

---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------

# Les environnements de déploiement
Il est question ici simuler le déploiement de l'application sur plusieurs environnements.Nous allons distinguer les environnements par les numéros de ports.

- La DEV : **8090**
- L' UAT : **8091**
- La PROD : **8092**

Déplacez vous sur la branche **m1ch3_OK**
```
git checkout m1ch3_OK
```
La configuration de ces environnements se fait dans les fichiers fichiers de propriété de l'application, les voici : 
![fichier_de_propriete.png](images/fichier_de_propriete.png)

### Creation de l'application
Dans le terminal, taper la commande suivante, sans prendre en compte les tests, puisqu'on sait déja qu'ils sont fonctionnels : 
```
mvn clean package -DskipTests
```
![build_jar.png](images/build_jar.png)

Le fichier jar est créé dans le répertoire target du projet.

![jar_file.png](images/jar_file.png)

On va le lancer en dev avec la commande suivante : 
- Sous windows
    ```
    java -jar "-Dspring.profiles.active=dev" target\calculator.jar
    ```

- Sous Linux     
    ```
    java -jar "-Dspring.profiles.active=dev" target/calculator.jar
    ```
![launch_app_dev.png](images/launch_app_dev.png)

Pour lancer sur l'uat ou la prod, il suffit de changer l'environnement dans la commande, tel que : 
> java -jar "-Dspring.profiles.active=**uar|prod**" target\calculator.jar

A présent on peut tester l'application dans le navigateur
![navi_launch_app_dev.png](images/navi_launch_app_dev.png)

-------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------

# Déploiement  multi-environnement

L'idée ici est de créer une image docker de notre application et de lancer chaque environnement (dev, uat, pod) dans un conteneur dédié.
On peut le faire sur n'importe quelle machine qui comporte docker. Nous allons la faire sur notre VM AWS qui contient docker installé.

Il faudrait cloner notre repos git de travail. 

Sur la branche actuelle **m1ch3**, lancer le scan sonar

```
mvn sonar:sonar -s .m2/settings.xml -Dsonar.login=<token> 
```  
Est-ce OK pour vous ?

#### Conteneurisation de l'application java
Empaquetter l'application dans une image docker, on lui donnera comme nom  : **calculator-light:1.0.0**
```
docker build -t calculator-light:1.0.0 .
```  
#### Déploiement des différents environnements
A présent on peut tester la creation d'un container et le déploiement de nos environnements. Pour rappel : 
- DEV : **8090**
- UAT : **8888**
- PROD : **9999**
```  
sudo docker run -d --name calculator-dev -p 8090:8090 --env SPRING_ACTIVE_PROFILES=dev  calculator-light:1.0.0
sudo docker run -d --name calculator-uat -p 8091:8888 --env SPRING_ACTIVE_PROFILES=uat  calculator-light:1.0.0
sudo docker run -d --name calculator-prod -p 8092:9999 --env SPRING_ACTIVE_PROFILES=prod  calculator-light:1.0.0
```  
