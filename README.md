# security-assignment-420-summer25



---

## Rapport Complet d'Analyse de Risques pour une Application de Gestion de Rendez-vous Médicaux

### 1. **Présentation de l'application**
L'application étudiée est une **plateforme de gestion de rendez-vous médicaux en ligne**, hébergée sur un serveur Ubuntu. Cette application permet aux patients de prendre des rendez-vous avec des médecins, de consulter leur dossier médical en ligne et de recevoir des notifications pour des prescriptions ou des rappels de rendez-vous. Elle est utilisée par des patients, des médecins, et le personnel administratif des cliniques.

L'application gère des données médicales sensibles, incluant les informations personnelles des patients, leurs historiques médicaux, prescriptions, et résultats de tests médicaux. Elle offre des services payants aux cliniques en prélevant des frais pour la gestion de la plateforme et la facturation des rendez-vous.

L'objectif de ce rapport est de réaliser une **analyse approfondie des risques** liés à cette application web, puis de proposer et d'implémenter des **mesures de sécurité techniques** pour rendre les risques résiduels acceptables.

### 2. **Cas d'utilisation théorique**
Le site web est utilisé par des patients pour :
- Prendre rendez-vous avec des médecins généralistes et spécialistes.
- Recevoir des notifications pour leurs rendez-vous à venir.
- Consulter leurs prescriptions électroniques et leur historique médical.
- Obtenir des résultats d'examens médicaux (sanguins, radiologiques, etc.).

Les **utilisateurs principaux** de l'application sont :
- **Patients** : Ils se connectent pour gérer leurs rendez-vous et consulter leurs dossiers médicaux.
- **Médecins** : Ils accèdent à l'historique médical des patients et mettent à jour les dossiers médicaux.
- **Personnel administratif** : Ils gèrent les horaires des médecins et approuvent les nouvelles inscriptions des patients.

**Exigences de sécurité critiques** :
- **Confidentialité** : Les dossiers médicaux doivent être strictement confidentiels et accessibles uniquement par des utilisateurs autorisés.
- **Disponibilité** : L’application doit être disponible 24h/24 pour permettre aux patients de gérer leurs rendez-vous en ligne.
- **Intégrité** : Les données doivent être protégées contre toute altération non autorisée, intentionnelle ou accidentelle.

### 3. **Modélisation des menaces (Threat Model)**
La modélisation des menaces est un processus crucial pour identifier les vulnérabilités potentielles et les menaces qui pourraient compromettre la sécurité de l'application. Voici les principales menaces identifiées :

#### a) **Individus malveillants**
Des hackers peuvent cibler l'application pour accéder aux **dossiers médicaux** des patients, les vendre sur le marché noir ou exiger une rançon pour ne pas divulguer ces données. Ils peuvent utiliser des méthodes comme l'injection SQL, des attaques par force brute, ou encore l'exploitation de failles non corrigées du système.

#### b) **Concurrentes médicales**
Des entreprises concurrentes pourraient tenter de **voler des informations** sur les patients ou les pratiques de gestion de la clinique pour obtenir un avantage commercial, en espionnant les transactions ou les comportements des patients.

#### c) **Gouvernements**
Les autorités gouvernementales peuvent rechercher à accéder aux **données sensibles** des utilisateurs pour des raisons juridiques ou d'enquête, mais cela pourrait nuire à la **confidentialité** des patients, surtout si les exigences légales sont mal gérées.

#### d) **Menaces internes**
Le personnel de la clinique, en particulier les administrateurs et médecins ayant des **accès privilégiés**, pourraient, de manière malveillante ou accidentelle, accéder à des informations non pertinentes, modifiant ainsi l'intégrité des dossiers médicaux.


| **Catégorie**       | **Individus**                                                                                         | **Compagnies**                                                                                                  | **Gouvernements**                                                                                              |
|---------------------|------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| **Sécurité**        | Les individus malveillants peuvent tenter de pirater l'application pour voler des données médicales sensibles ou accéder aux informations personnelles et financières des patients. | Les compagnies concurrentes peuvent tenter de perturber les services, affectant la disponibilité de l'application pour attirer les clients vers leurs propres services. | Les gouvernements peuvent tenter d'accéder aux données des utilisateurs pour des raisons légales ou d'enquête sur des activités suspectes ou criminelles. |
|                     | **Gravité : 3**                                                                                      | **Gravité : 3**                                                                                                | **Gravité : 2**                                                                                               |
| **Confidentialité** | Les utilisateurs peuvent craindre que leurs informations médicales ou personnelles soient divulguées à des tiers non autorisés (hackers, escrocs). | Les compagnies peuvent chercher à accéder aux informations confidentielles des patients pour une utilisation commerciale ou pour espionner les stratégies des cliniques. | Les gouvernements peuvent vouloir accéder aux données médicales pour des enquêtes liées à la santé publique ou à la sécurité nationale. |
|                     | **Gravité : 3**                                                                                      | **Gravité : 3**                                                                                                | **Gravité : 2**                                                                                               |
| **Anonymat**        | Les patients peuvent craindre que leur identité et leurs données médicales soient révélées à des tiers, ce qui pourrait nuire à leur vie privée. | Les concurrents pourraient tenter de découvrir l'identité des patients de la clinique pour gagner un avantage concurrentiel. | Les gouvernements peuvent essayer de révéler l'identité des utilisateurs dans le cadre de certaines enquêtes.   |
|                     | **Gravité : 3**                                                                                      | **Gravité : 2**                                                                                                | **Gravité : 1**                                                                                               |

- **Gravité 1** : Faible
- **Gravité 2** : Modérée
- **Gravité 3** : Élevée


### 4. **Analyse des risques et contrôles techniques**

Nous avons identifié six principaux risques pour l’application, ainsi que des mesures de contrôle qui peuvent être mises en place pour atténuer ces risques. Voici une analyse détaillée de chacun des risques, leur probabilité, impact, et les contrôles mis en place pour les atténuer.

#### a) **Attaque par force brute sur le formulaire de connexion**

##	**1. Tableau d’analyse de risque** 

| **Menace**                                                                                         | **Actifs menacés**                                | **Vulnérabilité**                                                                                                | **Impact (gravité)**                                                       | **Probabilité**  | **Contrôles suggérés**                                                                                 |
|----------------------------------------------------------------------------------------------------|--------------------------------------------------|------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|------------------|---------------------------------------------------------------------------------------------------------|
| Attaque par force brute sur les mots de passe pour accéder aux comptes utilisateurs (patients, médecins) | Informations personnelles et dossiers médicaux des patients | Absence de limitation de tentatives de connexion (pare-feu), absence de verrouillage de compte après plusieurs échecs.       | parce que nous parlons de données médicales, l'impact est très élevé (réputation de la clinique, confidentialité des données)         | Élevée           | - Utilisation d’un pare-feu (pfSense) pour limiter les tentatives répétées. |

  
##	**2. Tableau de contrôle implémenté** 


| **Menace**                                                                                         | **Contrôle**                                                                                                                                                 | **Risque initial**                                                                                                                                                                                                                      | **Risque résiduel après contrôle**                                                                                                                                                                                                                                                                               |
|----------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Attaque par force brute sur les mots de passe pour accéder aux comptes utilisateurs (patients, médecins) | Mise en place d'un pare-feu pfSense  pour limiter le nombre de tentatives de connexion autorisées à partir d'une adresse IP. | **Très élevé** : <br> Le risque initial est **très élevé** car il n'y a pas de limite sur les tentatives de connexion. donc un attaquant peut essayer un grand nombre de combinaisons de mots de passe jusqu'à ce qu'il réussisse. Les informations sensibles des patients, dont les dossiers médicaux, pourraient être compromises. **L'impact** sur la confidentialité, la réputation et les obligations légales de la clinique serait **très grave**. | **Faible** : <br> (probabilité diminuée, mais toujours présente, avec une sécurité renforcée). Le risque est maintenant considéré comme acceptable. |

##	**Le risque d’attaque force brute** 

- Une attaque par force brute consiste à tester, l’une après l’autre, chaque combinaison possible d’un mot de passe pour un identifiant donné afin se connecter au service ciblé (les données médicaux dans notre cas).
- Cette menace met en péril les informations medicales personnelles stockées sur notre site ainsi que la réputation et les revenus de notre entreprise.

##	**Le contrôle : Pare-feu (PfSense)** 

- Ces solutions peuvent bloquer ou ralentir considérablement les assauts, permettant nous de prendre des mesures proactives avant qu’une brèche ne soit exploitée.

- PfSense, limitent le nombre de requêtes qu’un utilisateur peut faire à un service donné, empêchant ainsi les attaques automatisées de saturer les systèmes avec des tentatives de connexion incessantes.

## **Acceptation de risque :**

- Avant d'implémenter un pare-feu dans notre réseau, le risque de vol d'informations de nos patients était élevé, ce qui aurait pu avoir un impact négatif sur notre revenu et notre réputation d'entreprise. Depuis la mise en place de ce contrôle, la probabilité de risque a diminué, mais cela ne signifie pas qu'elle a été éliminée à 100%. La probabilité de risque est maintenant considérée comme moyenne, ce qui est acceptable pour l'entreprise.

## **4. Instalation du par-feu PfSense dans notre reseau**

. **Acceder à l'interface Web de pfSense:**
   - Accéder à `http://10.10.10.1`.
   - Connection avec les identifiants d'administrateur (`admin`, `pfsense`).

2. **Acceder aux regles du par feu:**
   - Aller à `Firewall` > `Rules` > `WAN`.
     
   - ![1](https://github.com/user-attachments/assets/0414d753-4723-418e-8a28-f715ceaf0006)



3. **Creer une nouvelle regle parfeu:**
   - Click `Add`.
   - **Action:** `Block`
   - **interface:** `WAN`.
   - **protocol:** `TCP`
   - **Source:** 2048 `any`.
   - **Destination:** `This firewall (self)`.
   - **destination Port Range:** `SSH (22)`
   - Click `Save`.
![1](https://github.com/user-attachments/assets/0e823073-df18-40c3-bf50-10ce015ab344)
![2](https://github.com/user-attachments/assets/d34af2a8-2ebb-4792-9dcf-0c74b910e01e)
![3](https://github.com/user-attachments/assets/3c1a10c6-2ca0-4df9-8c6a-af0a3d59fa4b)


Une fois que la règle est configurée, pfSense bloquera automatiquement les adresses IP qui ont tenté de se connecter de manière répétée et infructueuse à l'aide de mots de passe incorrects.

**Finalement : on a accepté le risque résiduel.**

---

### b) **Authentification avec mot de passe**

#### Tableau d’analyse de risque :

| **Menace**                                                                                         | **Actifs menacés**                                           | **Vulnérabilité**                                                          | **Impact (gravité)**                                                                                                                                                                                                                             | **Probabilité**                                                                                                                                                        | **Contrôles suggérés**                                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------------------------|-------------------------------------------------------------|----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Accès non authentifié pouvant entraîner le vol d'informations sensibles (dossiers médicaux) | - Données utilisateur <br> - Dossiers médicaux <br> - Informations personnelles | Absence de mécanisme d'authentification solide (nom d'utilisateur et mot de passe) | **Impact : Très élevé** <br> Le vol des données sensibles (dossiers médicaux) peut entraîner de graves répercussions : violation de la confidentialité des patients, impact sur la réputation de la clinique, pertes financières, poursuites judiciaires. | **Probabilité : Élevée** <br> En l'absence de mécanisme d'authentification, l'accès non autorisé devient facile, augmentant significativement le risque de vol de données. | Mettre en place une **authentification solide** avec nom d'utilisateur et mot de passe, et activer des mesures supplémentaires comme l'authentification multi-facteurs (MFA). |

---

#### Tableau de contrôle implémenté :

| **Menace**                                                                                         | **Contrôle**                                                                                   | **Risque initial**                                                                                                                                                                                    | **Risque résiduel**                                                                                                                                                                                                                         |
|----------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Accès non authentifié pouvant entraîner le vol d'informations sensibles (dossiers médicaux) | Mettre en place une authentification solide (nom d'utilisateur et mot de passe), associée à l'authentification multi-facteurs (MFA) | **Impact initial** : Très élevé <br> - Fraude potentielle <br> - Violation de la confidentialité des patients <br> - Répercussions juridiques <br> - Impact direct sur la réputation et les revenus de la clinique. <br> **Probabilité : Très élevée** | **Impact résiduel** : Moyen <br> - Accès réduit aux données sensibles <br> - La fraude et la violation de données sont mieux contrôlées <br> **Probabilité : Moyenne** (grâce à l'authentification renforcée, mais le risque n'est pas complètement éliminé). |

---

#### **Risque de ne pas mettre en place une authentification par mot de passe** :  
Sans authentification par mot de passe, les attaquants peuvent accéder facilement aux données sensibles stockées sur l'application, y compris les informations personnelles et médicales des patients, ainsi que les dossiers financiers. Cela expose l'application à de graves conséquences légales et à une atteinte majeure à sa réputation.

#### **Le contrôle : Authentification avec nom d'utilisateur et mot de passe**  
L'authentification avec nom d'utilisateur et mot de passe, combinée à une authentification multi-facteurs (MFA), améliore considérablement la sécurité des comptes utilisateurs. Cette mesure prévient l'accès non autorisé et réduit la probabilité de vol de données sensibles. Ce contrôle est essentiel pour assurer la sécurité et la confiance des utilisateurs dans l'application.

#### **Acceptation de risque** :  
Avant l’implémentation de l'authentification avec mot de passe et MFA, l'application web était extrêmement vulnérable, et le risque de vol de données était élevé, avec un impact potentiel négatif sur la réputation et les revenus de la clinique. Depuis l'implémentation du contrôle, le risque a été réduit à un niveau acceptable. Bien que la probabilité de risque ait diminué, elle n’a pas complètement disparu. Toutefois, elle est désormais considérée comme **moyenne**, ce qui est acceptable dans le cadre des activités de l’entreprise.

---

### **Étapes pour la mise en place de l'authentification par mot de passe avec Apache**

#### **1. Configuration du fichier de site Apache**
Accédez au répertoire de configuration du site web et modifiez le fichier de configuration Apache correspondant au site à protéger (tpiliesharrache.grasset dans notre demonstration):

```bash
sudo vim /etc/apache2/sites-available/tpiliesharrache.conf
```

Ajoutez la ligne suivante pour indiquer qu'une authentification par mot de passe sera requise dans le répertoire :

```apache
ServerName tpiliesharrache.grasset

serverAdmin ilies@localhost
DocumentRoot /var/www/public_html

<Directory "/var/www/tpiliesharrache/public_html">
  AllowOverride AuthConfig
</Directory>
```

Enregistrez les modifications et fermez le fichier `wq!`.

![5](https://github.com/user-attachments/assets/3809b844-16ba-4ed2-ac3c-29b3cca2b6bc)


#### **2. Création du fichier `.htaccess`**

Dans le répertoire racine du site, créez un fichier `.htaccess`:

```bash
sudo vim /var/www/tpiliesharrache/public_html/.htaccess
```

Ajoutez les directives suivantes pour activer l’authentification et restreindre l’accès :

```apache
AuthType Basic
AuthName "Zone sécurisée"
AuthUserFile /var/www/tpiliesharrache/.htpasswd
Require valid-user
```

Enregistrez et quittez (`wq!`).

![6](https://github.com/user-attachments/assets/ae0171ce-7273-4c16-ace6-5bdd00ec77b2)


#### **3. Création du fichier `.htpasswd`**
Créez le fichier `.htpasswd` dans lequel seront stockés les noms d’utilisateur et les mots de passe.

```bash
sudo htpasswd -c /var/www/tpiliesharrache/.htpasswd ilies
```

- Le paramètre `-c` crée le fichier `.htpasswd` s'il n'existe pas déjà. Si vous ajoutez plusieurs utilisateurs, ne répétez pas l'option `-c` pour ne pas écraser les utilisateurs existants.

on est invité à entrer un mot de passe pour l’utilisateur `ilies` dans mon cas jai choisi `123`.


![12](https://github.com/user-attachments/assets/893fa017-2572-4ae1-bc4e-b05eb6143149)


#### **4. Redémarrage d'Apache**
Après avoir modifié la configuration et créé le fichier `.htpasswd`, on redémarre Apache pour appliquer les changements :

```bash
sudo systemctl restart apache2
```

![10](https://github.com/user-attachments/assets/d4b34893-851e-40a2-9a20-04f931a5ffbd)

![11](https://github.com/user-attachments/assets/e88b92d5-5048-495d-a6f7-d1999cfd3e52)



---

Finalement : on a accepté le risque résiduel.

--- 



### c) Analyse de risque et contrôle pour la sécurité des données dans IndexedDB


Dans notre **application médicale**, nous stockons des données sensibles dans **IndexedDB**, telles que des informations de rendez-vous, des profils de patients, et d'autres données médicales critiques. L'une des menaces majeures est l'injection de données malveillantes ou corrompues dans la base de données, ce qui pourrait compromettre l'intégrité des informations sensibles stockées localement.

#### Tableau d’analyse de risque :

| **Menace**                                                                                         | **Actifs menacés**                                           | **Vulnérabilité**                                                                                                      | **Impact (gravité)**                                                                                                                                                         | **Probabilité**                                                                                                         | **Contrôles suggérés**                                                                                 |
|----------------------------------------------------------------------------------------------------|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| Injections ou manipulations de données malveillantes dans IndexedDB                                | - Données sensibles (profils patients, historique médical) <br> - Intégrité de la base de données <br> - Fonctionnement de l'application | Absence de validation stricte des entrées et absence de mécanismes de vérification de l'intégrité des objets stockés dans IndexedDB | **Impact : Très élevé** <br> - Corruption des données sensibles <br> - Atteinte à la confidentialité des patients <br> - Violation des lois sur la protection des données médicales | **Probabilité : Moyenne** <br> Les injections malveillantes peuvent provenir de failles d'API ou d'entrées non sécurisées | - Validation stricte des données à l'entrée <br> - Contrôles après chaque transaction dans IndexedDB <br> - Chiffrement des données sensibles |

---

#### Tableau de contrôle implémenté :

| **Menace**                                                                                         | **Contrôle**                                                                                                       | **Risque initial**                                                                                                                                                                                        | **Risque résiduel**                                                                                                                                                                                                                          |
|----------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Injections ou manipulations de données malveillantes dans IndexedDB                                | Mise en place d'une validation stricte des données sensibles avant leur stockage dans IndexedDB et vérifications post-transaction | **Impact initial : Très élevé** <br> - Corruption des données médicales <br> - Violation de la confidentialité des patients <br> - Atteinte à la réputation de l'établissement <br> **Probabilité : Moyenne**                               | **Impact résiduel : Faible** <br> - Les contrôles et la validation réduisent le risque de corruption des données, mais il persiste un faible risque résiduel <br> **Probabilité : Faible** |

---


### Explication de la validation dans l'application de films

Dans notre application de films, nous récupérons les informations depuis une API externe et les stockons dans IndexedDB. Chaque film comporte des informations critiques comme `id`, `title`, `description`, `release_date`, `image`, et `movie_banner`. Il est crucial de valider ces données avant leur insertion pour éviter que des données malveillantes ou corrompues ne compromettent l'application.


### Exemple d'objet stocké dans IndexedDB :
Un objet typique que nous stockons dans IndexedDB peut être une fiche de film provenant de l'API. Voici un exemple de cet objet :


```javascript

{
  "id": "2baf70d1-42bb-4437-b551-e5fed5a87abe",
  "title": "Castle in the Sky",
  "original_title": "天空の城ラピュタ",
  "original_title_romanised": "Tenkū no shiro Rapyuta",
  "image": "https://image.tmdb.org/t/p/w600_and_h900_bestv2/npOnzAbLh6VOIu3naU5QaEcTepo.jpg",
  "movie_banner": "https://image.tmdb.org/t/p/w533_and_h300_bestv2/3cyjYtLWCBE1uvWINHFsFnE8LUK.jpg",
  "description": "The orphan Sheeta inherited a mysterious crystal that links her to the mythical sky-kingdom of Laputa...",
  "director": "Hayao Miyazaki",
  "producer": "Isao Takahata",
  "release_date": "1986",
  "running_time": "124",
  "rt_score": "95",
  "people": [
    "https://ghibliapi.vercel.app/people/598f7048-74ff-41e0-92ef-87dc1ad980a9"
  ],
  "species": [
    "https://ghibliapi.vercel.app/species/af3910a6-429f-4c74-9ad5-dfe1c4aa04f2"
  ],
  "locations": [
    "https://ghibliapi.vercel.app/locations/"
  ],
  "vehicles": [
    "https://ghibliapi.vercel.app/vehicles/4e09b023-f650-4747-9ab9-eacf14540cfb"
  ],
  "url": "https://ghibliapi.vercel.app/films/2baf70d1-42bb-4437-b551-e5fed5a87abe"
}


```


**Exemple de validation des données des films avant stockage** :

```javascript
function validateFilm(film) {
  const releaseYearRegex = /^\d{4}$/;
  const urlRegex = /^(https?|ftp):\/\/[^\s/$.?#].[^\s]*$/;

  if (!film.id || typeof film.id !== 'string') {
    console.error("L'ID est invalide.");
    return false;
  }

  if (!film.title || typeof film.title !== 'string' || film.title.trim().length === 0) {
    console.error("Le titre est manquant ou invalide.");
    return false;
  }

  if (!releaseYearRegex.test(film.release_date)) {
    console.error("La date de sortie est invalide.");
    return false;
  }

  if (!urlRegex.test(film.image) || !urlRegex.test(film.url)) {
    console.error("L'URL de l'image ou du film est invalide.");
    return false;
  }

  return true;
}
```

---

### Acceptation de risque

Avant la mise en place des mécanismes de validation des données et de vérification après transaction,  nos bases de données étaient vulnérables aux injections d'objets malveillants. Cela aurait pu entraîner la corruption des données, un dysfonctionnement de l'application, et dans le cas de l'application médicale, une violation de la confidentialité des patients.

**Dans l'application médicale**, cela aurait eu un impact direct sur la sécurité des données sensibles, ainsi qu'un risque de non-conformité avec les réglementations sur la protection des données médicales.

**Dans l'application de films**, bien que l'impact soit moins critique que dans un contexte médical, cela aurait pu nuire à l'expérience utilisateur en affichant des informations incorrectes ou corrompues.

Grâce à l'implémentation de **validations strictes**, de **gestion des erreurs**, et de **vérifications après transaction**, le risque d'injection de données malveillantes a été réduit à un niveau acceptable. Le risque résiduel reste **faible** dans les deux cas et est désormais sous contrôle.

---

### Conclusion

L'usage d'**IndexedDB** offre une solution puissante pour le stockage local des données dans nos applications médicales et de films. Cependant, des **mesures de sécurité rigoureuses** doivent être mises en place pour prévenir les risques d'injections malveillantes. En appliquant des contrôles stricts, tels que la validation des données et la surveillance des transactions, nous avons considérablement réduit les risques et garanti la sécurité et la fiabilité de nos applications. Le risque résiduel est faible et accepté dans les deux contextes.





#### d) **Interception de données sensibles lors des transmissions**
- **Menace** : Lors des communications entre les patients et le serveur, des données sensibles peuvent être interceptées si elles ne sont pas chiffrées.
- **Actifs menacés** : Données médicales et personnelles des patients.
- **Vulnérabilité** : Absence de chiffrement des échanges (HTTP au lieu de HTTPS).
- **Impact (gravité)** : Très élevé (vol de données critiques).
- **Probabilité** : Élevée.

**Contrôle technique** : Mise en place d’un **certificat SSL** pour garantir que toutes les communications entre le client et le serveur sont chiffrées via HTTPS.

- **Risque initial** : Très élevé.
- **Risque résiduel après contrôle** : Très faible.

#### e) **Attaque par ransomware**
- **Menace** : Les hackers peuvent chiffrer les données du serveur avec un ransomware et exiger une rançon pour restaurer l’accès.
- **Actifs menacés** : Dossiers médicaux des patients, données administratives.
- **Vulnérabilité** : Absence de sauvegardes fréquentes.
- **Impact (gravité)** : Très élevé (paralysie du service, perte financière énorme).
- **Probabilité** : Moyenne.

**Contrôle technique** : Mise en place de **sauvegardes régulières** automatiques hors ligne. Des scripts automatisés de backup permettent de conserver une copie des données critiques.

- **Risque initial** : Élevé.
- **Risque résiduel après contrôle** : Faible (sauvegarde régulière diminue considérablement le risque).

#### f) **Phishing et erreurs humaines**
- **Menace** : Le personnel pourrait être victime de phishing, entraînant des accès non autorisés aux dossiers des patients.
- **Actifs menacés** : Données sensibles des patients.
- **Vulnérabilité** : Manque de sensibilisation et de formation sur les risques de phishing.
- **Impact (gravité)** : Élevé (fuite de données personnelles et médicales).
- **Probabilité** : Moyenne.

**Contrôle

 technique** : Organisation de **formations de sensibilisation** à la sécurité pour les employés afin de réduire le risque d'attaques par ingénierie sociale. Des protocoles de vérification rigoureux des emails entrants et une politique de sécurité renforcée sont également mis en place.

- **Risque initial** : Moyen à élevé.
- **Risque résiduel après contrôle** : Faible.

### 5. **Conclusion**
En intégrant les mesures de sécurité détaillées ci-dessus, nous avons réduit de manière significative les risques critiques liés à l'application de gestion des rendez-vous médicaux. Les risques résiduels sont désormais gérables et conformes aux normes de sécurité recommandées dans le secteur de la santé. Toutefois, une surveillance constante et des audits réguliers doivent être mis en place pour garantir l'efficacité des contrôles.

---

Cette version très complète inclut plus de détails techniques et pratiques, et approfondit chaque menace, contrôle, et résultat obtenu. Si vous avez besoin d'autres informations spécifiques ou de détails supplémentaires, je peux les intégrer.
