# Partie 4 : Programmation multimédia avec SFML

## 1. Introduction

SFML (Simple and Fast Multimedia Library) est une bibliothèque puissante et facile à utiliser pour développer des applications multimédia, telles que des jeux vidéo ou des outils graphiques. Elle offre des fonctionnalités pour gérer les fenêtres, les entrées utilisateur, le rendu graphique, le son, et plus encore.

Dans cette partie, nous allons :
1. Configurer un projet SFML.
2. Manipuler les fenêtres et événements.
3. Dessiner des formes et utiliser des textures.
4. Gérer l’audio.
5. Conclure avec une étude de cas réel et un mini-projet.

---

## 2. Configuration de SFML

Avant de commencer, assurez-vous que SFML est correctement configuré dans votre environnement Visual Studio.

### 2.1. Installation

1. **Téléchargez SFML** : Rendez-vous sur le site officiel de SFML ([https://www.sfml-dev.org](https://www.sfml-dev.org)) et téléchargez la version correspondant à votre compilateur (par exemple, **Visual Studio 64-bit**).
2. **Ajoutez SFML au projet** :
   - Décompressez l'archive téléchargée dans un dossier accessible.
   - Dans Visual Studio, allez dans les **Propriétés du Projet** > **Configuration Properties** > **VC++ Directories** et :
     - Ajoutez le chemin du dossier `include` de SFML à **Include Directories**.
     - Ajoutez le chemin du dossier `lib` de SFML à **Library Directories**.

3. **Ajoutez les fichiers de lien** :
   - Allez dans **Propriétés du Projet** > **Configuration Properties** > **Linker > Input**.
   - Dans **Additional Dependencies**, ajoutez les fichiers suivants (selon vos besoins) :
     ```
     sfml-graphics.lib
     sfml-window.lib
     sfml-system.lib
     sfml-audio.lib
     ```

4. **Déplacez les fichiers DLL** :
   - Copiez les fichiers `.dll` correspondants (par exemple, `sfml-graphics-2.dll`) du dossier `bin` de SFML dans le répertoire `Debug` de votre projet.

### 2.2. Exemple simple

Pour tester la configuration, créons une fenêtre SFML qui affiche une forme géométrique.

```cpp
#include <SFML/Graphics.hpp>

int main() {
    // Création de la fenêtre
    sf::RenderWindow window(sf::VideoMode(800, 600), "Fenêtre SFML");

    // Création d'un cercle
    sf::CircleShape shape(50); // Rayon de 50 pixels
    shape.setFillColor(sf::Color::Green);

    // Boucle principale
    while (window.isOpen()) {
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed)
                window.close(); // Fermer la fenêtre
        }

        // Effacer la fenêtre
        window.clear();
        // Dessiner la forme
        window.draw(shape);
        // Afficher le contenu
        window.display();
    }

    return 0;
}
```

**Explication** :
- `sf::RenderWindow` : Crée une fenêtre.
- `sf::CircleShape` : Dessine un cercle avec un rayon spécifié.
- `window.clear()` : Efface le contenu précédent.
- `window.draw()` : Dessine les objets.
- `window.display()` : Affiche le contenu.

#### **Exercice** :
1. Modifiez le code pour afficher un rectangle rouge à la place du cercle.
2. Ajoutez un cercle bleu et un rectangle jaune côte à côte.

---

## 3. Gestion des entrées utilisateur

SFML offre un système simple pour gérer les entrées utilisateur via des événements, comme les clics de souris, les pressions de touches, ou les déplacements de souris.

### 3.1. Exemple : Gestion des événements clavier

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>

int main() {
    sf::RenderWindow window(sf::VideoMode(800, 600), "Gestion des Entrées");

    while (window.isOpen()) {
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed)
                window.close();

            if (event.type == sf::Event::KeyPressed) {
                if (event.key.code == sf::Keyboard::A)
                    std::cout << "La touche A a été pressée!" << std::endl;
                if (event.key.code == sf::Keyboard::Escape)
                    window.close(); // Fermer avec Échap
            }
        }

        window.clear();
        window.display();
    }

    return 0;
}
```

**Explication** :
- `sf::Event::KeyPressed` détecte quand une touche est appuyée.
- `sf::Keyboard` fournit une liste de touches (par exemple, `sf::Keyboard::A`).

#### **Exercice** :
1. Affichez un message pour chaque touche appuyée.
2. Ajoutez une action spécifique pour les flèches directionnelles.

---

## 4. Manipulation des textures et des images

SFML permet de charger des textures depuis des fichiers pour les afficher dans une fenêtre.

### 4.1. Exemple : Charger et afficher une texture

```cpp
#include <SFML/Graphics.hpp>

int main() {
    sf::RenderWindow window(sf::VideoMode(800, 600), "Affichage de Texture");

    // Charger une texture
    sf::Texture texture;
    if (!texture.loadFromFile("example.png")) {
        return -1; // Erreur si le fichier est introuvable
    }

    // Associer la texture à un sprite
    sf::Sprite sprite;
    sprite.setTexture(texture);

    while (window.isOpen()) {
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed)
                window.close();
        }

        window.clear();
        window.draw(sprite); // Dessiner le sprite
        window.display();
    }

    return 0;
}
```

**Explication** :
- `sf::Texture` charge une image depuis un fichier.
- `sf::Sprite` associe une texture pour l’afficher.

#### **Exercice** :
1. Ajoutez plusieurs sprites avec différentes textures.
2. Déplacez les sprites à l’aide des touches fléchées.

---

## 5. Gestion de l’audio

SFML prend en charge l’audio avec des fichiers `.wav` ou `.ogg`. Elle offre des classes comme `sf::Sound` et `sf::Music`.

### 5.1. Exemple : Lecture d’un fichier audio

```cpp
#include <SFML/Audio.hpp>
#include <iostream>

int main() {
    sf::SoundBuffer buffer;
    if (!buffer.loadFromFile("example.wav")) {
        std::cout << "Erreur : Impossible de charger le fichier audio." << std::endl;
        return -1;
    }

    sf::Sound sound;
    sound.setBuffer(buffer);
    sound.play();

    // Attendre que le son se termine
    while (sound.getStatus() == sf::Sound::Playing) {
        sf::sleep(sf::milliseconds(100));
    }

    return 0;
}
```

**Explication** :
- `sf::SoundBuffer` charge un fichier audio en mémoire.
- `sf::Sound` utilise le buffer pour jouer le son.

#### **Exercice** :
1. Chargez un fichier `.ogg` à la place d’un fichier `.wav`.
2. Ajoutez un contrôle de volume et une répétition du son.

---

## 6. Mini-projet : Jeu de Pong

### 6.1. Objectif

Créez un jeu de Pong simple où deux joueurs contrôlent des raquettes pour renvoyer une balle.

### 6.2. Étapes

1. **Créez les raquettes** : Deux rectangles contrôlables par le clavier.
2. **Ajoutez une balle** : Un cercle avec une vitesse et une direction.
3. **Ajoutez la logique du jeu** : Détections de collision et score.

---

## 7. Étude de cas réel : Visualiseur graphique

Développez une application qui charge une image et permet de la manipuler (rotation, agrandissement, déplacement). Utilisez les concepts de textures, d’entrées utilisateur et de dessin.

---

## 8. Conclusion

Avec SFML, vous pouvez facilement développer des applications multimédia interactives. Cette partie du cours vous offre une base solide pour explorer des projets plus avancés comme les jeux ou les simulations graphiques.
