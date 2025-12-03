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