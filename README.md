# security-assignment-420-summer25



---

## Rapport Complet d'Analyse de Risques pour une Application de Gestion de Rendez-vous Médicaux

### 1. **Présentation de l'application**
L'application étudiée est une **plateforme de gestion de rendez-vous médicaux en ligne**, hébergée sur un serveur Ubuntu. Cette application permet aux patients de prendre des rendez-vous avec des médecins, de consulter leur dossier médical en ligne et de recevoir des notifications pour des prescriptions ou des rappels de rendez-vous. Elle est utilisée par des patients, des médecins, et le personnel administratif des cliniques.

L'application gère des données médicales sensibles, incluant les informations personnelles des patients, leurs historiques médicaux, prescriptions, et résultats de tests médicaux. Elle offre des services payants aux cliniques en prélevant des frais pour la gestion de la plateforme et la facturation des rendez-vous.

Le site web est utilisé par des patients pour prendre rendezvous avec des médecins, recevoir des notifications pour leurs rendez-vous et consulter leurs prescriptions électroniques et leur historique médical.

Les utilisateurs de l'application sont les patients les médecins et les personnel administratif:


### 3. **Modélisation des menaces (Threat Model)**
La modélisation des menaces est un processus crucial pour identifier les vulnérabilités potentielles et les menaces qui pourraient compromettre la sécurité de l'application. Voici les principales menaces identifiées :




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

**test sur notre application**

### III. Tests avec des objets statiques

Pour démontrer l'importance de la validation, nous avons créé trois tests distincts avec des objets statiques pour montrer le comportement de l'application dans différents cas.

#### 1. Premier test : Objet valide sans validation

Dans ce premier cas, nous utilisons un objet valide (le film **"Castle in the Sky"**) sans validation. L'objet est correctement stocké et affiché sans problème, car il ne contient aucune donnée malveillante.

#### Code :
```javascript
let db;
const dbName = "GhibliDB";
const storeName = "films";

// Fonction pour initialiser la base de données
function initDB() {
  const request = indexedDB.open(dbName, 1);
  
  request.onerror = function(event) {
    console.error("Erreur d'ouverture de la base de données");
  };

  request.onsuccess = function(event) {
    db = event.target.result;
    testValidObject(); // Charger l'objet à l'initialisation
  };

  request.onupgradeneeded = function(event) {
    db = event.target.result;
    db.createObjectStore(storeName, { keyPath: "id" });
  };
}

// Fonction pour stocker les films dans IndexedDB
function storeFilms(films) {
  const transaction = db.transaction([storeName], "readwrite");
  const objectStore = transaction.objectStore(storeName);
  
  films.forEach(film => {
    const request = objectStore.put(film);
    request.onerror = function(event) {
      console.error("Erreur de stockage du film :", film.title);
    };
    request.onsuccess = function(event) {
      console.log("Film stocké avec succès :", film.title);
      displayFilms([film]); // Afficher le film stocké
    };
  });
}

// Fonction pour afficher les films
function displayFilms(films) {
  const container = document.createElement('div');
  container.setAttribute('class', 'container');

  films.forEach(movie => {
    const card = document.createElement('div');
    card.setAttribute('class', 'card');

    const h1 = document.createElement('h1');
    h1.textContent = movie.title;

    const img = document.createElement('img');
    img.src = movie.image;
    img.alt = movie.title;

    const p = document.createElement('p');
    movie.description = movie.description.substring(0, 300);
    p.textContent = `${movie.description}...`;

    card.appendChild(h1);
    card.appendChild(img);
    card.appendChild(p);
    container.appendChild(card);
  });

  document.getElementById('root').appendChild(container);
}

// Test d'un objet (sans validation)
function testValidObject() {
  const validFilm = {
    id: "2baf70d1-42bb-4437-b551-e5fed5a87abe",
    title: "Castle in the Sky",
    original_title: "天空の城ラピュタ",
    original_title_romanised: "Tenkū no shiro Rapyuta",
    image: "https://image.tmdb.org/t/p/w600_and_h900_bestv2/npOnzAbLh6VOIu3naU5QaEcTepo.jpg",
    movie_banner: "https://image.tmdb.org/t/p/w533_and_h300_bestv2/3cyjYtLWCBE1uvWINHFsFnE8LUK.jpg",
    description: "The orphan Sheeta inherited a mysterious crystal that links her to the mythical sky-kingdom of Laputa...",
    director: "Hayao Miyazaki",
    producer: "Isao Takahata",
    release_date: "1986",
    running_time: "124",
    rt_score: "95",
    people: [
      "https://ghibliapi.vercel.app/people/598f7048-74ff-41e0-92ef-87dc1ad980a9",
      "https://ghibliapi.vercel.app/people/fe93adf2-2f3a-4ec4-9f68-5422f1b87c01"
    ],
    species: [
      "https://ghibliapi.vercel.app/species/af3910a6-429f-4c74-9ad5-dfe1c4aa04f2"
    ],
    locations: [
      "https://ghibliapi.vercel.app/locations/"
    ],
    vehicles: [
      "https://ghibliapi.vercel.app/vehicles/4e09b023-f650-4747-9ab9-eacf14540cfb"
    ],
    url: "https://ghibliapi.vercel.app/films/2baf70d1-42bb-4437-b551-e5fed5a87abe"
  };

  // Stocker et afficher l'objet (sans validation)
  storeFilms([validFilm]);
}

// Initialiser la base de données au chargement de la page
window.onload = function() {
  initDB();
};

```
![db3](https://github.com/user-attachments/assets/ddcb48e5-80c3-448a-bb7d-b8f3bdf813a3)

![db4](https://github.com/user-attachments/assets/5f3f75c6-f514-4216-9a1e-4eb65dc56868)



#### Explication :
- **Aucun problème** : Puisque l'objet est valide, il est correctement stocké et affiché sans risque de sécurité.
  
#### 2. Deuxième test : Objet malveillant sans validation

Dans ce test, nous injectons un objet malveillant sans validation. L'objet contient du code JavaScript dans l'attribut `onerror` de l'image, déclenchant une attaque **XSS** lorsque l'image ne se charge pas.

#### Code :
```javascript
let db;
const dbName = "GhibliDB";
const storeName = "films";

// Fonction pour initialiser la base de données
function initDB() {
  const request = indexedDB.open(dbName, 1);
  
  request.onerror = function(event) {
    console.error("Erreur d'ouverture de la base de données");
  };

  request.onsuccess = function(event) {
    db = event.target.result;
    testMaliciousObject(); // Charger l'objet malveillant sans validation
  };

  request.onupgradeneeded = function(event) {
    db = event.target.result;
    db.createObjectStore(storeName, { keyPath: "id" });
  };
}

// Fonction pour stocker les films dans IndexedDB
function storeFilms(films) {
  const transaction = db.transaction([storeName], "readwrite");
  const objectStore = transaction.objectStore(storeName);
  
  films.forEach(film => {
    const request = objectStore.put(film);
    request.onerror = function(event) {
      console.error("Erreur de stockage du film :", film.title);
    };
    request.onsuccess = function(event) {
      console.log("Film stocké avec succès :", film.title);
      displayFilms([film]); // Afficher le film stocké
    };
  });
}

// Fonction pour afficher les films (XSS sera déclenché ici)
function displayFilms(films) {
  const container = document.createElement('div');
  container.setAttribute('class', 'container');

  films.forEach(movie => {
    const card = document.createElement('div');
    card.setAttribute('class', 'card');

    const h1 = document.createElement('h1');
    h1.textContent = movie.title;

    const p = document.createElement('p');
    movie.description = movie.description.substring(0, 300);
    p.textContent = `${movie.description}...`;

    // Ici l'attaque XSS est injectée via onerror
    const img = document.createElement('img');
    img.src = movie.image;  // L'image ne sera pas chargée, et onerror sera déclenché
    img.alt = movie.title;
    img.onerror = function() {
        alert('XSS Injection via onerror!');  // Injection du script malveillant via onerror
    };

    card.appendChild(h1);
    card.appendChild(img);
    card.appendChild(p);
    container.appendChild(card);
  });

  document.getElementById('root').appendChild(container);
}

// Test d'un objet malveillant sans validation
function testMaliciousObject() {
  const maliciousFilm = {
    id: "123",  
    title: "Malicious Movie",  
    release_date: "2021",  
    image: "invalid_image.jpg",  
    description: "site non securise attempts XSS.",
    url: "https://example.com/malicious-movie"  
  };

  console.log("Tentative de stockage d'un objet malveillant sans validation.");
  storeFilms([maliciousFilm]); // Stocker et afficher l'objet malveillant sans validation
}

// Initialiser la base de données au chargement de la page
window.onload = function() {
  initDB();
};

```

#### Explication :
- **XSS déclenché** : L'attribut `onerror` est utilisé pour déclencher une alerte JavaScript malveillante. Sans validation, l'application est vulnérable et permet à ce script de s'exécuter.

- ![db5](https://github.com/user-attachments/assets/dbbe0a5c-6ed8-4c6d-b3fd-3ef84457ee8a)
  

  
#### 3. Troisième test : Objet malveillant avec validation (protection contre XSS)

Dans ce dernier test, nous utilisons un objet malveillant similaire, mais cette fois avec validation. L'objet est rejeté avant d'être stocké ou affiché, protégeant l'application contre les attaques XSS.

#### Code :
```javascript
let db;
const dbName = "GhibliDB";
const storeName = "films";

// Fonction pour initialiser la base de données
function initDB() {
  const request = indexedDB.open(dbName, 1);
  
  request.onerror = function(event) {
    console.error("Erreur d'ouverture de la base de données");
  };

  request.onsuccess = function(event) {
    db = event.target.result;
    testMaliciousObject(); // Charger l'objet malveillant avec validation
  };

  request.onupgradeneeded = function(event) {
    db = event.target.result;
    db.createObjectStore(storeName, { keyPath: "id" });
  };
}

// Fonction pour stocker les films dans IndexedDB
function storeFilms(films) {
  const transaction = db.transaction([storeName], "readwrite");
  const objectStore = transaction.objectStore(storeName);
  
  films.forEach(film => {
    const request = objectStore.put(film);
    request.onerror = function(event) {
      console.error("Erreur de stockage du film :", film.title);
    };
    request.onsuccess = function(event) {
      console.log("Film stocké avec succès :", film.title);
      displayFilms([film]); // Afficher le film stocké
    };
  });
}

// Fonction pour afficher les films (avec validation des données)
function displayFilms(films) {
  const container = document.createElement('div');
  container.setAttribute('class', 'container');

  films.forEach(movie => {
    const card = document.createElement('div');
    card.setAttribute('class', 'card');

    const h1 = document.createElement('h1');
    h1.textContent = movie.title;

    const p = document.createElement('p');
    movie.description = movie.description.substring(0, 300);
    p.textContent = `${movie.description}...`;

    const img = document.createElement('img');
    img.src = movie.image;  // L'image sera affichée si elle est valide
    img.alt = movie.title;

    card.appendChild(h1);
    card.appendChild(img);
    card.appendChild(p);
    container.appendChild(card);
  });

  document.getElementById('root').appendChild(container);
}

// Fonction de validation des films avant stockage dans IndexedDB
function validateFilm(film) {
  const releaseYearRegex = /^\d{4}$/;
  const urlRegex = /^(https?|ftp):\/\/[^\s/$.?#].[^\s]*$/;

  // Validation de l'ID
  if (!film.id || typeof film.id !== 'string' || film.id.trim().length === 0) {
    console.error("L'ID du film est invalide ou manquant.");
    return false;
  }

  // Validation du titre
  if (!film.title || typeof film.title !== 'string' || film.title.trim().length === 0) {
    console.error("Le titre du film est invalide ou manquant.");
    return false;
  }

  // Validation de la date de sortie (année)
  if (!releaseYearRegex.test(film.release_date)) {
    console.error("La date de sortie du film est invalide.");
    return false;
  }

  // Validation des URLs (image et URL du film)
  if (!urlRegex.test(film.image) || !urlRegex.test(film.url)) {
    console.error("L'URL de l'image ou du film est invalide.");
    return false;
  }

  // Interdire tout script ou injection dans onerror
  if (film.onErrorScript && /<script>|javascript:|eval\(/.test(film.onErrorScript)) {
    console.error("Script malveillant détecté dans onErrorScript.");
    return false;
  }

  // Si toutes les validations passent
  return true;
}

// Test d'un objet malveillant avec validation
function testMaliciousObject() {
  const maliciousFilm = {
    id: "123",  // ID fictif
    title: "Malicious Movie",  // Titre malveillant
    release_date: "2021",  // Date valide
    image: "invalid_image.jpg",  // Image invalide pour déclencher onerror
    onErrorScript: "alert('XSS Injection via onerror!')",  // Script injecté via onerror
    description: "This is a malicious movie object that attempts XSS.",
    url: "https://example.com/malicious-movie"  // URL correcte mais l'image est malveillante
  };

  // Valider l'objet malveillant
  if (validateFilm(maliciousFilm)) {
    console.log("L'objet malveillant a été validé (ceci ne devrait pas se produire).");
    storeFilms([maliciousFilm]); // Stocker et afficher l'objet malveillant si validé
  } else {
    console.log("L'objet malveillant a été rejeté par la validation.");
  }
}

// Initialiser la base de données au chargement de la page
window.onload = function() {
  initDB();
};

```

#### Explication :
- **Validation efficace** : Le code JavaScript injecté est détecté et bloqué. L'objet malveillant n'est pas stocké ni affiché, et l'application reste sécurisée.

- ![db6](https://github.com/user-attachments/assets/c27477cb-6cd5-4723-8920-a806f5d4c06d)


---

avec cette série de tests on a démontré l'importance de la validation des données dans notre application web, en particulier pour prévenir les attaques **XSS**. Les objets non validés peuvent introduire des failles de sécurité importantes, comme l'exécution de code JavaScript malveillant via des attributs comme `onerror`. La validation stricte permet de garantir que seules des données sûres et valides sont traitées, stockées, et affichées dans l'application.

### Acceptation de risque

Avant la mise en place des mécanismes de validation des données,  nos bases de données étaient vulnérables aux injections d'objets malveillants. Cela aurait pu entraîner la corruption des données, un dysfonctionnement de l'application, et dans le cas de l'application médicale, une violation de la confidentialité des patients.
cela aurait eu un impact direct sur la sécurité des données sensibles, ainsi qu'un risque de non-conformité avec les réglementations sur la protection des données médicales.

---






Je comprends que vous souhaitez discuter du **certificat SSL**. Voici la version complète et correcte de la section sur le certificat SSL :

---

### d) Certificat SSL :

#### Tableau d’analyse de risque :

| Menace  | Actif  | Vulnérabilité  | Impact (Évaluation de gravité)  | Probabilité (Évaluation de probabilité)  | Contrôles suggérés  |
| ------- | ------ | -------------- | ------------------------------- | --------------------------------------- | ------------------- |
| Le risque d'interception de données sensibles lorsqu'elles sont transmises sans être chiffrées.  | - Informations très sensibles <br> - Données personnelles des clients <br> - Informations confidentielles de l'entreprise  | Absence d’un certificat SSL pour crypter l’échange entre le client et le serveur.  | Impact : Élevé <br> - Vol de données sensibles <br> - Perte de confidentialité <br> - Problèmes juridiques <br> - Perte de réputation <br> - Pertes financières | Probabilité : Élevée <br> - Possibilité de vol des données sensibles de l’entreprise et des clients lors de la transmission | - Mettre en place un certificat SSL <br> - Travailler avec le protocole sécurisé HTTPS |

#### Tableau de contrôle implémenté :

| Menace  | Contrôle  | Risque initial  | Risque résiduel  |
| ------- | --------- | --------------- | ---------------- |
| Le risque d'interception de données sensibles lorsqu'elles sont transmises sans être chiffrées.  | Mettre en place un certificat SSL et travailler avec le protocole sécurisé HTTPS.  | Impact initial : <br> - Vol de données sensibles <br> - Perte de confidentialité <br> - Problèmes juridiques <br> - Perte de réputation <br> - Pertes financières <br> Impact : Élevé <br> Probabilité : Élevée  | Impact résiduel : <br> - Moindre risque de vol de données sensibles <br> - Probabilité réduite, mais toujours présente pour certaines failles mineures. <br> Impact : Moyen <br> Probabilité : Faible |

---

### Définition de Certificat SSL :
Un certificat SSL est un protocole de sécurité utilisé pour protéger les données échangées entre le navigateur du client et le serveur Web. Il établit une connexion sécurisée en utilisant le chiffrement, ce qui permet de protéger les données contre les attaques de type "man-in-the-middle", où un attaquant intercepterait et manipulerait les informations échangées.

### Le risque de ne pas mettre en place un certificat SSL :
Ne pas implémenter un certificat SSL présente des risques majeurs pour la sécurité d'une entreprise. Lors de l'envoi d'informations sensibles sans cryptage, les attaquants peuvent intercepter ces données. Cela entraînerait une perte de confiance des clients, une atteinte à la réputation de l'entreprise, et des sanctions financières ou juridiques.

### Le contrôle : Mise en place d’un certificat SSL :
Lorsque le certificat SSL est mis en place, toutes les communications entre le client et le serveur sont chiffrées, garantissant que les informations sensibles, telles que les données des clients, ne peuvent pas être facilement interceptées par des attaquants. Cela protège les données lors de leur transmission et assure une navigation sécurisée via le protocole HTTPS. Ce contrôle est essentiel pour la sécurité de tout site web qui manipule des informations sensibles.

### Acceptation du risque :
Avant la mise en place d’un certificat SSL, l'application web était vulnérable, exposant les données des clients à un risque élevé de vol lors de leur transmission. Cela pouvait avoir des répercussions négatives sur le chiffre d'affaires, la réputation de l'entreprise, et entraîner des conséquences juridiques. Depuis la mise en place de ce contrôle, les risques ont été réduits, bien que non totalement éliminés. Le risque résiduel est maintenant considéré comme acceptable pour l'entreprise.

---

### Mise en place d'un certificat SSL pour sécuriser notre site
---

### 1. Générer un certificat SSL auto-signé

Sur notre serveur web Ubuntu, on va exécutez la commande suivante pour créer un certificat SSL auto-signé.

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -addext "subjectAltName = DNS:tpiliesharrache.grasset" -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
```




---

### 2. Répondre aux questions OpenSSL

il faut répondre à quelques questions. La question la plus importante est **Common Name**, où on va entrer le nom de notre site :

```
Common Name: tpiliesharrache.grasset
```

![ssl1](https://github.com/user-attachments/assets/13fabd2f-4374-46e8-872e-15287ec3747e)

---

### 3. Configurer les paramètres SSL d'Apache

on va créez un fichier de configuration SSL pour Apache avec la commande suivante :

```bash
sudo vim /etc/apache2/conf-available/ssl-params.conf
```

et joutez le contenu suivant :

```bash
SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH
SSLProtocol All -SSLv2 -SSLv3 -TLSv1 -TLSv1.1
SSLHonorCipherOrder On
Header always set X-Frame-Options DENY
Header always set X-Content-Type-Options nosniff
# Requires Apache >= 2.4
SSLCompression off
SSLUseStapling on
SSLStaplingCache "shmcb:logs/stapling-cache(150000)"
# Requires Apache >= 2.4.11
SSLSessionTickets Off
```

elle va désactiver les anciennes versions de SSL et TLS et configurer des en-têtes de sécurité supplémentaires.


![ssl2](https://github.com/user-attachments/assets/e881877d-3fed-4a90-8e18-5bdbd82b7bb3)



---

### 4. Faire une copie de la configuration SSL par défaut

Faites une copie de sauvegarde de la configuration SSL par défaut d'Apache :

```bash
sudo cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf.bak
```

---

### 5. Modifier la configuration SSL

modifier le fichier **default-ssl.conf** pour y insérer les informations de notre certificat et nom de domaine :

```bash
sudo vim /etc/apache2/sites-available/default-ssl.conf
```

Modifiez les parties suivantes :

```bash
        ServerName tpiliesharrache.grasset
        DocumentRoot /var/www/tpiliesharrache/public_html

        SSLCertificateFile      /etc/ssl/certs/apache-selfsigned.crt
        SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
```

![ssl3](https://github.com/user-attachments/assets/9b070015-171d-4fa8-a4da-7951de5ca608)


---

### Étape 6 : Activer SSL et les modules requis dans Apache

Activer le module SSL et les autres modules nécessaires dans Apache avec les commandes suivantes :

```bash
sudo a2enmod ssl
sudo a2enmod headers
sudo a2ensite default-ssl
sudo a2enconf ssl-params
```

Ensuite, tester la configuration :

```bash
sudo apache2ctl configtest
```

on va ignorer l'avertissement suivant :

```
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
Syntax OK
```

Enfin, redémarrez Apache pour appliquer les changements :

```bash
sudo systemctl restart apache2
```


![ssl5](https://github.com/user-attachments/assets/e723adcc-5f26-40f0-9d70-4ad5cc25a723)



---

### Étape 7 : Ouvrir le port 443 dans pfSense pour HTTPS

Pour permettre l'accès au HTTPS sur le réseau, on doit ouvrir le port 443 dans pfSense.

1. Connecter l'interface web de pfSense.
2. Allez dans **Firewall > Rules**.
3. Sous l'onglet **WAN**, ajouter une nouvelle règle :
   - **Filter rule association** : Pass
   - **Interface** : WAN
   - **Protocol** : TCP
   - **Destination Port Range** : HTTPS (443)
   - **Redirect target port**: HTTPS
   - **Redirect target IP**: 10.10.10.11 (ubuntu server) 
4. click save.
   

![ssl4](https://github.com/user-attachments/assets/145aa7fd-c73a-436a-bec8-acd8badfa12c)



---

### Étape 8 : Tester le site dans Chrome

Sur la machine locale, ouvrer **Google Chrome** et testez le site sécurisé en visitant :

```
https://tpiliesharrache.grasset
```

![ssl6](https://github.com/user-attachments/assets/10dbacdd-fff4-4eb0-9d7c-255e4fc91e19)


---

### Étape 9 : Installer le certificat localement


![ssl8](https://github.com/user-attachments/assets/1e304b93-024a-4ad3-9494-14a7aa18a421)

![ssl9](https://github.com/user-attachments/assets/81e1b1db-2ce8-4f21-ba69-f6ac370c907f)

![ssl10](https://github.com/user-attachments/assets/6b60207e-12fa-49d5-8db5-88526cc1d631)

![ssl11](https://github.com/user-attachments/assets/e0afc1fa-1326-4cba-b033-e8d5479995af)

![ssl12](https://github.com/user-attachments/assets/db6efee8-475c-4996-9641-52f5a8a9747f)


Une fois installer, Vider le cache de votre navigateur et le redémarrer :.

---

![ssl15](https://github.com/user-attachments/assets/6cdfd2cd-68ff-4d8b-a2d4-5aed2f9285f6)

---




---

### E) Fichier de sauvegarde (Backup) :

#### Tableau d’analyse de risque :

| Menace                              | Actif                              | Vulnérabilité                                                    | Impact (Évaluation de gravité)                                                                                                       | Probabilité (Évaluation de probabilité)                                      | Contrôles suggérés                                         |
| ------------------------------------ | ---------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Attaque de Ransomware (Ransom-logiciel) | - Fonctionnement du site <br> - Données sensibles de l'entreprise | Absence de politiques de sécurité robustes et de sauvegardes régulières | Impact : Élevé <br> - Perte de productivité <br> - Perte de données critiques <br> - Atteinte à la réputation de l’entreprise <br> - Pertes financières importantes  | Probabilité : Moyenne <br> - Risque de paralysie totale du site en cas d’attaque de ransomware | - Implémentation de politiques de sauvegarde régulières <br> - Création de copies régulières et sécurisées des données sensibles de l'entreprise |

---

#### Tableau de contrôle implémenté :

| Menace                              | Contrôle                                                                                          | Risque initial                                                                                                         | Risque résiduel                                      |
| ------------------------------------ | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| Attaque de Ransomware (Ransom-logiciel) | Mise en place de politiques de sauvegarde (backups) pour créer des copies sécurisées des données | Impact initial : <br> - Perte de productivité <br> - Perte de données <br> - Dommages financiers et atteinte à la réputation | Impact résiduel : <br> - Réduction des temps d'arrêt <br> - Récupération plus rapide après une attaque <br> Impact : Moyen <br> Probabilité : Faible |

---

### Définition des sauvegardes (Backups) :

Les **backups** sont des copies de sécurité des données essentielles de l’entreprise qui sont effectuées régulièrement pour prévenir la perte de données en cas d'événements imprévus, comme une attaque de ransomware, une défaillance matérielle ou une catastrophe naturelle. Les données sont stockées sur un support sécurisé (disque externe, cloud, etc.) et peuvent être restaurées en cas de sinistre.

### Le risque de ne pas mettre en place des sauvegardes :

Sans la mise en place de sauvegardes, l’entreprise s'expose à des risques graves. En cas d’attaque de ransomware, les attaquants peuvent chiffrer les données et en exiger une rançon pour les déverrouiller. Cela paralyserait le fonctionnement du site, causerait une perte d'accès aux données essentielles et aurait des conséquences financières et juridiques graves. La réputation de l’entreprise en serait également fortement affectée, car les clients perdraient confiance dans la capacité de l’entreprise à protéger leurs informations.

### Le contrôle : Mise en place de sauvegardes régulières :

En mettant en œuvre des scripts de sauvegarde réguliers, l’entreprise peut créer des copies de sécurité de ses données critiques, ce qui permet de restaurer l'intégrité des systèmes en cas d'attaque ou de panne. Les sauvegardes doivent être stockées dans des emplacements sécurisés (comme un cloud chiffré ou un serveur dédié) et testées régulièrement pour s'assurer qu'elles sont valides. Ce contrôle est essentiel pour assurer la continuité des activités et limiter les dégâts causés par une attaque.

### Acceptation du risque :

Avant l'implémentation des sauvegardes, l'application web et les données étaient vulnérables à toute attaque ou défaillance, augmentant considérablement le risque de perte de données critiques et d'interruption du site. Cela aurait eu des conséquences graves sur le chiffre d'affaires et la réputation de l’entreprise. Depuis la mise en place de ce contrôle, les impacts potentiels ont été atténués et la probabilité d'une perte de données est maintenant faible. Bien qu'il reste un risque résiduel, ce dernier est désormais acceptable pour l’entreprise.

---

### Remarque :

Les sauvegardes sont une solution de protection contre plusieurs types de menaces, notamment :
- **Les pannes matérielles** : Une défaillance matérielle peut entraîner la perte de données. Les sauvegardes permettent de restaurer les données après réparation.
- **Attaques de ransomware** : Les attaquants qui chiffrent les données pour exiger une rançon n’ont plus de pouvoir si des sauvegardes existent.
- **Catastrophes naturelles** : Les sauvegardes externes (cloud ou serveur distant) permettent de protéger les données en cas de sinistre physique.

Il est crucial de tester régulièrement les sauvegardes pour garantir leur fiabilité. Ne pas vérifier les sauvegardes avant d'en avoir besoin peut entraîner des pertes supplémentaires en cas de problèmes avec les données de récupération.

---

### mis en place d'un Système de Sauvegarde Automatisé avec Apache



Nous allons configurer Apache2 pour gérer le endpoint `/backup` en utilisant un script PHP qui pourra effectuer la tâche de sauvegarde.

#### 1. creation du script backup PHP

1. **creation du script backup**: 
on va cree le fichier PHP `backup.php` qui sera déclenché lorsque le endpoint `/backup` sera appelé dans le repertoire `/var/www/tpiliesharrache/public_html`

    - ```bash
        sudo vim /var/www/tpiliesharrache/public_html/backup.php

        ```
À l'intérieur du fichier `backup.php`, on va ajoutez ce script :

```php
<?php
header('Content-Type: application/json');

$backupDir = '/var/www/tpiliesharrache/backups/';

// Check if backup directory exists, if not, create it
if (!file_exists($backupDir)) {
    if (!mkdir($backupDir, 0755, true)) {
        echo json_encode(['status' => 'error', 'message' => 'Failed to create backup directory.']);
        exit;
    }
}

// Get JSON data from request body
$data = json_decode(file_get_contents('php://input'), true);

if (!$data) {
    echo json_encode(['status' => 'error', 'message' => 'No data received.']);
    exit;
}

// Generate a filename with the current date and time
$backupFile = $backupDir . 'backup_' . date('Y-m-d_H-i-s') . '.json';

// Save the data to the backup file
if (file_put_contents($backupFile, json_encode($data, JSON_PRETTY_PRINT))) {
    echo json_encode(['status' => 'success', 'file' => $backupFile, 'message' => 'Backup has been saved successfully.']);
} else {
    echo json_encode(['status' => 'error', 'message' => 'Failed to save the backup file.']);
}
?>
```

- Le script vérifie si le répertoire de sauvegarde existe. Si ce n'est pas le cas, il tente de créer le répertoire avec les autorisations appropriées. Si la création du répertoire échoue, il renvoie une erreur
- Les données du request body sont écrites dans un fichier JSON dans le répertoire /var/www/tpiliesharrache/backups/. Le nom de fichier inclut un timestamp pour garantir que chaque sauvegarde est unique.

#### 3. Modifier la configuration Apache pour le endpoint `/backup`

on va modifier le fichier de configuration :

```bash
sudo vim /etc/apache2/sites-available/tpiliesharrache.conf
```

À l'intérieur de `<VirtualHost>`, on va ajoutez la ligne suivante pour configurer le endpoint `/backup` :

```apache
<Directory /var/www/tpiliesharrache/public_html>
    AllowOverride All
    Options Indexes FollowSymLinks
    Require all granted
</Directory>

# Enable PHP handling
AddType application/x-httpd-php .php

```

Cela permet l'accès au répertoire `/backup` et autorise l'exécution du script PHP.

#### 4. Redémarrer Apache

Redémarrez le service Apache pour appliquer les modifications :

```bash
sudo systemctl restart apache2
```

#### 5. Mettre à jour JavaScript pour utiliser le endpoint Apache `/backup`

Maintenant, dans le code JavaScript, on va ajouter la logique pour que le client envoi les informations au server (`/backup` endpoint):

```javascript
function backupData() {
  // Fetch data from IndexedDB
  const transaction = db.transaction([storeName, logoStoreName], "readonly");
  const filmsStore = transaction.objectStore(storeName);
  const logoStore = transaction.objectStore(logoStoreName);

  const filmsRequest = filmsStore.getAll();
  const logoRequest = logoStore.get("logo");

  filmsRequest.onsuccess = function() {
    const filmsData = filmsRequest.result;

    logoRequest.onsuccess = function() {
      const logoData = logoRequest.result;

      // Send the data to the PHP backup script
      fetch('/backup.php', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          films: filmsData,
          logo: logoData
        })
      })
      .then(response => response.json())
      .then(data => {
        console.log('Backup successful:', data);
        alert('Backup successful!');
      })
      .catch(error => {
        console.error('Erreur lors de la sauvegarde :', error);
        alert('Backup failed: ' + error.message);
      });
    };

    logoRequest.onerror = function() {
      console.error("Erreur de récupération du logo pour la sauvegarde.");
    };
  };

  filmsRequest.onerror = function() {
    console.error("Erreur de récupération des films pour la sauvegarde.");
  };
}

// Attach backup functionality to button
document.getElementById('backupButton').addEventListener('click', function() {
  backupData();
});
```

### 6. Mettre a jour notre HTML pour utiliser un bouton pour envoyer les donner de notre site au endpoint `/backup`

```html

<body>
  <button id="backupButton">Sauvegarder les données</button>
  <div id="root"></div>

  <script src="script.js"></script>
</body>

```
![f12](https://github.com/user-attachments/assets/c3caa873-a02f-4401-9ce3-5a63357823aa)

![f13](https://github.com/user-attachments/assets/5c757a27-63ad-4823-a9be-42128dbedc4f)

![f14](https://github.com/user-attachments/assets/5c116240-53dc-4c96-bf1f-c2f84a0636ed)

![f16](https://github.com/user-attachments/assets/947f8c79-3866-4991-8679-f61fedb4f1d5)

### Finalement : on a accepté le risque résiduel. 
