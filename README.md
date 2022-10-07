# Installation du dépôt hub-core

Installation du répertoire _hub-core_, utilisation par l’IDE _IntelliJ IDEA_ sur _Windows_, test des requête avec _Postman_

-   [ ] Télécharger [JDK 11](https://jdk.java.net/archive/)
    
-   [ ] Cloner le projet _hub-core_ :<br>`git clone https://gitlab.com/sinay-lab/hub/hub-core`
    
-   [ ] Ouvrir le projet avec IntelliJ
    
-   [ ] Ajouter le JDK téléchargé à IntelliJ :
    
    **File → Project Structure → SDKs**
    
    Cliquer sur **+→ Add JDK** et renseigner le chemin vers le JDK téléchargé
    
-   [ ] Définir le JDK du projet, pour cela mettre _SDK_ au JDK 11 ajouté et _Language level_ à 11 dans :
    
    **File → Project Structure → Project**
    
-   [ ] Installer le plugin _Lombok sur IntelliJ_, si _IntelliJ_ vous met un message “lombok require enabled annotation processing” cliquer sur accepter
    
-   [ ] Cloner les projets _hub-core-api_ et _user-management-api_ au même endroit ou vous avez cloné _hub-core_ :
    
    `git clone https://gitlab.com/sinay-lab/hub/hub-core-api`
    
    `git clone https://gitlab.com/sinay-lab/hub/user-management-api`
    
-   [ ] Ouvrir ces projets avec IntelliJ
    
-   [ ] Vérifier que pour ces projets JDK 11 est utilisé
    
-   [ ] Pour chacun des deux projets exécuter cette commande (appuyer deux fois sur _Ctrl_, un champ avec marqué _Run Anything_ devrait s’ouvrir) :
    
    `mvn clean install`
    
-   [ ] Ensuite, exécuter sur _hub-core_ la commande suivante :<br> `mvn clean install -Dmaven.test.skip=true`
    
-   [ ] Modifier ces lignes dans le fichier hub-core\src\main\resources\\**application.properties** :
    
    ```
    # Database
    spring.datasource.url=${SPRING_DATASOURCE_URL:jdbc:mysql://127.0.0.1:3307/hub-core}
    spring.datasource.username=${SPRING_DATASOURCE_USERNAME:db-hub-core}
    spring.datasource.password=${SPRING_DATASOURCE_PASSWORD:VGZ2KZ6n8s6mve}
    
    ```
    
    ```
    # User management
    hub.client.user.management.url=${HUB_CLIENT_USER_MANAGEMENT_URL:<http://localhost:9851>}
    
    ```
    
    ```
    # Keycloak
    keycloak.auth-server-url=${KEYCLOAK_AUTH_INTERNAL_SERVER_URL:<http://localhost:8081/auth>}
    keycloak.realm=${KEYCLOAK_REALM:sinay-dev}
    
    ```
    
    **IMPORTANT** : Il ne faudra pas commit ce fichier
    
-   [ ] Si dans le pom.xml la dépendance d’un répertoire n’est pas trouvé, vérifier que la version de ce répertoire est la même que celle déclaré dans le pom.xml de _hub-core_
    
-   [ ] Lancer ces commandes dans des terminaux cmd, dans l’ordre elles permettent l’accès à la BDD, Keycloak et user-mangement :
    

(tips : vous pouvez télécharger Terminal sur Windows store pour avoir des terminaux à plusieurs onglets)

`ssh [gw-gcp.sinay.ai](<http://gw-gcp.sinay.ai/>) -L 127.0.0.1:3307:127.0.0.1:3306`

`ssh gw-gcp.sinay.ai -L 0.0.0.0:8081:sk1:8081`

`ssh gw-gcp.sinay.ai -L 0.0.0.0:9851:platform-dev:9851`

-   [ ] Sur Postman, créer une variable _token_

-   [ ] Sur les requêtes Postman dans _authorization_, mettre _Type_ à _bearer token_, et à droite dans _Token_ mettre _{{token}}_

![postman_autorization](https://cdn.discordapp.com/attachments/630492112562946048/1027873690777571369/Untitled.png)

-   [ ] Récupérer un token via cette requête (ils sont temporaires, il faudra réexécuter la requête ultérieurement) :
`https://sk.sinay.ai/auth/realms/sinay-dev/protocol/openid-connect/token`

-   [ ] Pour tester le bon fonctionnement+, récupérer les workspaces avec un HTPP GET de l’url suivante :
    `http://127.0.0.1:8082/core/api/v1/workspaces/`
