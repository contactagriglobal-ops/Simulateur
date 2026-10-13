# Simulateur GONI Plus

Outil d'aide à la décision du Cabinet GONI Plus (Niamey, Niger) : il estime l'investissement de départ, la rentabilité, la trésorerie et le risque d'un projet agricole, et produit un projet de plan d'affaires. Références à jour au 13 octobre 2026.

Ce dépôt fabrique automatiquement :

- **l'application Android** (fichier `.apk` à installer, et fichier `.aab` pour le Google Play Store), publiée dans l'onglet **Releases** ;
- **le site web installable**, publié sur GitHub Pages (seulement si le dépôt est public).

## Ce que contient le dépôt

| Élément | Rôle |
|---|---|
| `www/` | Le simulateur lui-même. **C'est le seul dossier à modifier** pour une mise à jour (en pratique, le fichier `www/index.html`). |
| `android-res/` | Les icônes et l'écran de démarrage de l'application Android. |
| `.github/workflows/apk.yml` | Les commandes qui fabriquent l'application Android et la publient dans Releases. |
| `.github/workflows/pages.yml` | Les commandes qui mettent le site en ligne sur GitHub Pages. |
| `package.json`, `package-lock.json`, `capacitor.config.json` | Les réglages de l'outil Capacitor, qui transforme le simulateur en application Android. |

---

## Mise en place, pas à pas (une seule fois, environ 20 minutes)

### Étape 1 — Créer le compte et le dépôt

1. Sur **github.com**, créez un compte gratuit, de préférence au nom du cabinet (par exemple `goni-plus`).
2. En haut à droite, cliquez sur **+**, puis **New repository**.
3. Nom du dépôt : `simulateur` (par exemple).
4. Choisissez **Public** ou **Private** :
   - **Public** : l'application Android ET le site web sont fabriqués gratuitement, mais tout le monde peut voir le code et les fiches de référence.
   - **Private** : le code reste privé et l'application Android est fabriquée (dans la limite gratuite de 2 000 minutes par mois, largement suffisante) ; en revanche, le site web GitHub Pages n'est pas disponible avec un compte gratuit. Dans ce cas, supprimez le fichier `.github/workflows/pages.yml` et gardez Netlify pour le site.
5. Ne cochez rien d'autre et cliquez sur **Create repository**.

### Étape 2 — Déposer les fichiers

1. Sur la page du dépôt vide, cliquez sur le lien **uploading an existing file**.
2. Ouvrez le dossier `1-depot-github` sur votre ordinateur, sélectionnez **tout son contenu** (pas le dossier lui-même) et glissez-le dans la page.
3. Vérifiez que la liste contient bien `.github/workflows/apk.yml`. **Sur Mac**, le dossier `.github` est caché : appuyez sur Cmd + Maj + . (point) dans le Finder pour l'afficher avant de glisser. S'il manque quand même, voir « Si le dossier .github n'est pas passé » plus bas.
4. En bas, cliquez sur **Commit changes**.

### Étape 3 — Enregistrer la clé de signature

Sans cette étape, GitHub fabrique une application « d'essai » : elle s'installe, mais ses mises à jour obligeront à désinstaller l'ancienne version, ce qui efface les simulations enregistrées sur le téléphone.

1. Dans le dépôt, ouvrez **Settings**, puis **Secrets and variables**, puis **Actions**.
2. Cliquez sur **New repository secret** et créez les trois secrets indiqués dans le fichier `2-cle-de-signature-A-GARDER-SECRETE/LIRE-cle-de-signature.txt` :
   - `CLE_FICHIER` : collez tout le contenu du fichier `CLE_FICHIER.txt` ;
   - `CLE_MOT_DE_PASSE` : le mot de passe indiqué ;
   - `CLE_ALIAS` : `goniplus`.
3. Rangez le dossier de la clé en lieu sûr. **Ne le déposez jamais dans le dépôt.**

### Étape 4 — Lancer la première fabrication

1. Ouvrez l'onglet **Actions**. Si GitHub demande d'activer les actions, acceptez.
2. À gauche, cliquez sur **Application Android**, puis à droite sur **Run workflow**, puis sur le bouton vert **Run workflow**.
3. Patientez 5 à 10 minutes. Une coche verte indique la réussite ; une croix rouge, une erreur (voir « En cas de problème »).
4. Ouvrez l'onglet **Releases** (colonne de droite de la page d'accueil du dépôt) : la version apparaît avec le fichier `GONI-Plus-….apk`.

### Étape 5 — Activer le site web (dépôt public seulement)

1. **Settings**, puis **Pages**.
2. Dans **Source**, choisissez **GitHub Actions**.
3. Onglet **Actions**, puis **Site web (GitHub Pages)**, puis **Run workflow**.
4. L'adresse du site s'affiche dans **Settings > Pages** : `https://NOM-DU-COMPTE.github.io/simulateur/`.

---

## Installer l'application Android sur un téléphone

1. Sur le téléphone, ouvrez la page **Releases** du dépôt (ou envoyez le fichier `.apk` par WhatsApp).
2. Téléchargez le fichier `GONI-Plus-….apk` et ouvrez-le.
3. Le téléphone demande d'autoriser l'installation d'applications provenant de cette source (Chrome, WhatsApp ou Fichiers) : acceptez, puis **Installer**.
4. L'icône GONI Plus apparaît sur l'écran d'accueil. L'application fonctionne **sans connexion dès la première ouverture**.

Le bouton « Plan d'affaires (.docx) » ouvre le menu de partage du téléphone : choisissez « Enregistrer dans Fichiers », WhatsApp, Gmail…

## Mettre à jour les références ou les prix

1. Dans le dépôt, ouvrez le dossier `www`, puis **Add file > Upload files**.
2. Glissez le nouveau fichier `index.html` et cliquez sur **Commit changes**.
3. C'est tout : GitHub fabrique automatiquement la nouvelle version de l'application (onglet Actions) et la publie dans Releases ; le site web se met à jour aussi.
4. Pour le site web installable, pensez aussi à changer la date de la ligne `const VERSION = …` dans `www/sw.js` : sinon, les téléphones qui ont installé le site gardent l'ancienne version.

Une version signée avec la même clé s'installe par-dessus l'ancienne **sans effacer les simulations**.

## Publier sur le Google Play Store (plus tard)

Chaque version signée contient aussi un fichier `GONI-Plus-…-PlayStore.aab`. C'est lui qu'on dépose dans la **Google Play Console** (compte développeur Google, 25 dollars US une seule fois), avec la fiche de l'application, des captures d'écran et une politique de confidentialité. Un compte au nom d'une organisation évite l'obligation de faire tester l'application par 12 personnes pendant 14 jours, imposée aux nouveaux comptes personnels.

---

## En cas de problème

**Si le dossier .github n'est pas passé** : dans le dépôt, **Add file > Create new file**. Dans le nom du fichier, tapez exactement `.github/workflows/apk.yml`, puis collez le contenu du fichier `apk.yml` du dossier `3-copie-des-fichiers-de-commandes`. Validez avec **Commit changes**. Recommencez avec `.github/workflows/pages.yml` si vous voulez le site web.

**Croix rouge dans Actions** : cliquez sur la ligne en échec, puis sur l'étape marquée d'une croix, et copiez les dernières lignes du message d'erreur. Envoyez-les à Claude, qui vous dira quoi corriger.

**« Application non installée » lors d'une mise à jour** : l'ancienne version a été signée avec une autre clé (par exemple une version d'essai fabriquée avant l'étape 3). Désinstallez-la une fois, puis installez la nouvelle ; les suivantes s'installeront par-dessus.

**Où sont les données ?** Les simulations restent sur le téléphone de chaque utilisateur. Rien n'est envoyé au Cabinet GONI Plus.
