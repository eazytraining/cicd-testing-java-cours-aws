# Déploiement de la VM de labs 
La machine de TP doit etre suffisament robuste. Nous recommandons une VM avec les ressources suivantes : 
    
- **16 GO** de RAM minimum
- **4 vCPU** minimum

1. Si vous travaillez sur des VMs locales (**Virtualbox**): 
    - Un vagrantfile et son script de provisionning (**install_java.sh**) sont fournis. 
    - Utilisez les pour déployer la VM java dans Virtualbox

2. Si vous travaillez dans le cloud (**AWS dans notre cas**) : 
    - Classe d'instance : **t2.xlarge**
    - user_data:  vous sont fournis dans le fichier **user_data.sh**
    - User de connexion : **ubuntu**
    - Security Group : Initialement, autoriser les ports suivants : 
      -  **22** (ssh)
      -  **9000-9010**
      -  **5433-5440**
      -  **3306-3310**
      -  **80** (http par defaut)
      -  **8080-8099** (port de l'application)

    - Balises de la VM : 
      - Name: <votre_prenom>
## Installer firefox         
**Firefox** est un prérequis pour certains tests faits dans le projet. Il faudrait installer ce navigateur afin que les test passent.

##  Configuration de Vscode 
- Installer VS code en local.  [Voici le lien](https://code.visualstudio.com/docs/?dv=win)

- Installer l'extension : **Remote SSH de Microsoft**
    * Puis : Ctrl + shift+ P ==> tapez  **remote-ssh** ==> entrer ==> ssh vagrant@<ip_host>  ou **ssh -i <keys.pem> centos@<ip_host>** si AWS  ==> entrer ==> Choisissez **linux** si le choix est demandé ==> coller le mot de passe ==> entrer
    * Open folder (**ouvrir un dossier**): Entrer les memes infos de connexion si de nouveau demandé

- Installer l'extensions: **Extension Pack for JAVA** 
  
  !!! -- Ne télécharger aucune JDK en local

- Installer l'extensions: **Git Graph** 
  
  !!! -- il faudrait que git soit déja installé sur la VM pour pas avoir de warning d'erreur


##  Configuration de Git / GitHub
- On aura besoin de **git**, du coup vérifiez qu'il est bien installé sur la VM java. Normalement le provisionning  ou les user data l'ont déja installés pour vous, il faut juste vérifier qu'il est installé
    ```    
    git --version
    ```
- Eventuellement créer un compte sur github.com si vous n'en avez pas un

- Générer un Personal **Access token** dans github:

    User ===> settings ===> glisser tout en bas à gauche: Developer Settings
    ==> Personal Access Token ===> donner un nom au token (Ex: Centos AWS for CICD) ===>    Générer le token, le copier et le GARDER JALOUSEMENT!!!


##  Configuration de Java 11 (Ubuntu 24.04)
- Vérifier que Java 11 est bien installé sur la VM (normalement le provisioning / user-data l’a déjà fait, on vérifie juste) :

```bash
    java -version
```
Si Java 11 n’est pas présent, installer :
```bash
    sudo apt update -y
    sudo apt install -y openjdk-11-jdk
```

Si vous etes sur **windows**, télécharger l'executable a [cette adresse](https://repo1.maven.org/maven2/org/apache/maven/apache-maven/3.9.4/apache-maven-3.9.4-bin.tar.gz).

## Configuration de Maven 3.9.11 (Ubuntu 24.04)

- Vérifier que le package maven est bien installé sur la VM java. Normalement le provisionning ou les user data l'ont déja installés  pour vous, il faut juste vérifier qu'il est installé.
    ```bash    
        mvn --version
    ```    
    ![Launch SonarQube](images/maven_version.png)
On doit avoir Maven 3.9.x minimum et Java 11 minimum.

Si ce n’est pas le cas, voici l’installation manuelle (recommandée) : 
    
    - Sous **Linux** (en user **root**) : 
      - Tapez les commandes suivantes : 

        ```bash
            cd /usr/local/src/
            sudo apt install -y wget tar
        ```
      - Téléchargement + installation :
        ```bash
            MAVEN_VERSION="3.9.11"
            MAVEN_URL="https://dlcdn.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz"

            sudo wget $MAVEN_URL
            sudo tar -xzf apache-maven-${MAVEN_VERSION}-bin.tar.gz
            sudo mkdir -p /usr/local/maven
            sudo mv apache-maven-${MAVEN_VERSION}/* /usr/local/maven/
            sudo rm -rf apache-maven-${MAVEN_VERSION}
        ``` 
      - Configuration système :
        ```bash
            sudo tee /etc/profile.d/maven.sh > /dev/null <<EOF
            export M2_HOME=/usr/local/maven
            export PATH=\${M2_HOME}/bin:\${PATH}
            EOF

            sudo chmod +x /etc/profile.d/maven.sh
            source /etc/profile.d/maven.sh
        ``` 
      - Ajout aussi pour l’utilisateur courant (pratique en SSH) :
        ```bash
                    if ! grep -q "M2_HOME" ~/.bashrc; then
            echo "" >> ~/.bashrc
            echo "# Configuration Maven" >> ~/.bashrc
            echo "export M2_HOME=/usr/local/maven" >> ~/.bashrc
            echo "export PATH=\${M2_HOME}/bin:\${PATH}" >> ~/.bashrc
            fi
            source ~/.bashrc
        ```
      - Vérification finale :
        ```bash
            mvn --version
        ```
    Si le lien de téléchargement Apache ne répond pas, consultez : https://downloads.apache.org/maven 
 et reconstruisez une URL valide à partir de la dernière version.
      - Forcer **maven** à utiliser **java 11**

        ```
        sudo alternatives --config java 
        ```    
          puis taper 1 + **entrer**
        ```    
        sudo alternatives --config javac 
        ```    
          puis taper 1 + **entrer**
          
          !!! -- **PS** :  Ce n'est pas une repétition, il y'a un '**c**' après **java**

    - Sous Windows 11 — Installation / configuration de Java 11 et Maven 3.9.11
    1) Java 11 (JDK)

    Télécharger le JDK 11

    Option simple : Adoptium (Temurin 11) (MSI/ZIP) : https://adoptium.net/temurin/releases/?version=11

    Installer

    Si tu prends le MSI : double-clic, Next → Install.

    À la fin, note le chemin d’installation, souvent du type :

    C:\Program Files\Eclipse Adoptium\jdk-11.x.x.x-hotspot\

    Configurer les variables d’environnement

    Ouvre : Paramètres → Système → Informations système → Paramètres système avancés

    Variables d’environnement…

    Dans Variables système :

    Crée (ou modifie) JAVA_HOME = chemin du JDK, ex :

    C:\Program Files\Eclipse Adoptium\jdk-11.0.xx.x-hotspot

    Modifie Path et ajoute :

    %JAVA_HOME%\bin

    Vérifier
    Ouvre un nouveau PowerShell / CMD :

    java -version
    javac -version

    2) Maven 3.9.11

    Télécharger Maven (ZIP)

    https://dlcdn.apache.org/maven/maven-3/3.9.11/binaries/apache-maven-3.9.11-bin.zip

    Extraire

    Exemple :

    C:\Tools\apache-maven-3.9.11

    Configurer les variables d’environnement

    Variables d’environnement…

    Dans Variables système :

    Crée M2_HOME (ou MAVEN_HOME) :

    C:\Tools\apache-maven-3.9.11

    Modifie Path et ajoute :

    %M2_HOME%\bin (ou %MAVEN_HOME%\bin si tu as choisi ce nom)

    Vérifier
    Dans un nouveau PowerShell / CMD :

    mvn -version


✅ Attendu : Java 11 minimum et Maven 3.9.x.

## Configuration de Docker
- Créer un compte sur Docker Hub :  https://hub.docker.com/ si vous n'en avez pas un
- Installez docker sur votre VM

### Démarrage du serveur sonar conteneurisé :
Tapez la commande suivante sur la VM aws: 

```
docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:lts
```
