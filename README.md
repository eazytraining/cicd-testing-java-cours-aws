# Tests Manuels et tests automatisés
#### Téléchargement des sources 
Forkez ce repos et clonnez le sur votre machine de test java : 
> **Repos :** https://github.com/Zerofiltre-Courses/cicd-testing-java-cours
```    
git clone ${votre_url_git}
```
    
!!! -- La machine java peut être celle windows ou alors la VM dans le cloud. Suivez les intructions de votre formteur à ce niveau.
#### Chargement du projet dans l'IDE (vscode)
Connectez vous en remote ssh à la VM java et ouvrer le repertoire de travail dans l'explorateur Vscode. 
    
!!! -- Si vous bossez en local, pas besoin de faire du remote-ssh, ouvrez juste le projet dans vscode.
#### Lancement de l'application et des tests
Vous pouvez lancer l"application java  graphiquement dans vscode, ou alors taper la commande maven suivante : 
```bash
    mvn clean package
    java -jar target/calculator.jar
```    
##### Tests Manuels
L'appli ecoute sur le port **8080**. Si vous êtes en local, alors tapez [ceci](http://localhost:8080) dans le navigateur : **http://localhost:8080**. Vous pouvez testez ce qui vous convient.

##### Test automatisés
Pareil, vous pouez lancer les tests graphiquement dans vscode, ou alors utiliser les commandes suivantes :   
```bash
    mvn test
```
    
Ensuite, déplacez vous sur la branche **m1ch1_OK**, puis lancez tous les tests et comparez les temps d'exécution. 

!!! -- Sur la branche m1ch1_OK, vous devrez voir apparaitre des tests End to End (E2E).