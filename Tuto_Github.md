# Tuto : Utiliser GitHub avec Visual Studio 2022

---

## **Étape 1 : Lier Visual Studio 2022 à GitHub**

### **Configurer Git globalement** :
1. Accédez à **Outils > Options > Source Control > Git Global Settings**.
2. Entrez votre nom et votre email :
   ```plaintext
   Nom : John Doe
   Email : john.doe@example.com
   ```

---

## **Étape 2 : Cloner un dépôt GitHub**

### **Cloner un dépôt existant** :
1. Allez dans **Git > Cloner un dépôt**.
2. Entrez l’URL de votre dépôt GitHub.
3. Choisissez un répertoire local pour stocker le dépôt.
4. Cliquez sur **Cloner**.

### **Vérifier le dépôt** :
Une fois cloné, le dépôt s’ouvre dans l’Explorateur de solutions. Vous pouvez voir les fichiers, branches et commits associés.

---

## **Étape 3 : Créer un nouveau projet et le pousser sur GitHub**

### **Créer un nouveau projet** :
1. Dans Visual Studio, cliquez sur **Fichier > Nouveau projet**.
2. Sélectionnez un modèle de projet (exemple : Console App .NET).
3. Une fois le projet créé, allez dans le menu **Git > Créer un dépôt Git**.

### **Publier sur GitHub** :
1. Une fois le dépôt local créé, allez dans **Git > Publier sur GitHub**.
2. Connectez-vous si ce n’est pas déjà fait.
3. Remplissez les informations du dépôt (nom, visibilité publique/privée).
4. Cliquez sur **Publier**.

---

## **Étape 4 : Gérer les modifications et les commits**

### **Faire des modifications** :
Modifiez vos fichiers dans l'éditeur de Visual Studio.

### **Stager et commiter les changements** :
1. Allez dans l'onglet **Git Changes**.
2. Ajoutez un message de commit décrivant vos changements.
3. Cliquez sur **Commit All** pour valider localement les modifications.

### **Pousser les changements sur GitHub** :
Après le commit, cliquez sur **Push** pour envoyer les modifications sur le dépôt distant (GitHub).

---

## **Étape 5 : Travailler avec des branches**

### **Créer une nouvelle branche** :
1. Dans le menu **Git**, cliquez sur **Branches > Nouvelle branche**.
2. Donnez un nom à la branche (exemple : `feature-ajout-login`).
3. Sélectionnez la branche de base (souvent `main`).

### **Switcher entre les branches** :
Utilisez l'onglet **Branches** ou le menu **Git > Branches** pour passer d’une branche à l’autre.

### **Fusionner une branche** :
1. Une fois le travail terminé sur une branche, vous pouvez la fusionner dans `main`.
2. Dans **Git > Branches**, sélectionnez la branche cible et cliquez sur **Merge**.

---

## **Étape 6 : Résoudre les conflits**

### **En cas de conflits lors d'une fusion** :
1. Visual Studio affiche les fichiers en conflit.
2. Cliquez sur un fichier en conflit pour ouvrir l'outil de résolution de conflit.
3. Choisissez les modifications à conserver (votre version, la version distante ou une combinaison).

Une fois les conflits résolus, **stager** les modifications et commiter.

---

## **Étape 7 : Tirer (Pull) les mises à jour**

Si d'autres contributeurs travaillent sur le dépôt, tirez leurs modifications avant de pousser les vôtres :
1. Cliquez sur **Pull** dans l'onglet **Git Changes**.

---

## **Astuces supplémentaires**

### **Raccourcis Git** :
- **Commit rapide** : `Ctrl+0, G, Enter`
- **Créer une branche** : `Ctrl+Shift+Alt+B`
  
---
