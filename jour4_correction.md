# Corrections des Exercices - Cours SFML

## Exercice 1 : Création d'une fenêtre
### Énoncé
Créez une fenêtre SFML de taille 800x600 avec le titre "Ma Première Fenêtre".

### Solution
```cpp
#include <SFML/Graphics.hpp>

int main() {
    // Création d'une fenêtre
    sf::RenderWindow window(sf::VideoMode(800, 600), "Ma Première Fenêtre");

    // Boucle principale
    while (window.isOpen()) {
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed)
                window.close(); // Fermer la fenêtre
        }

        window.clear(); // Effacer le contenu de la fenêtre
        window.display(); // Afficher le contenu de la fenêtre
    }

    return 0;
}
```

**Explications :**
- Une fenêtre est créée avec `sf::RenderWindow`.
- La boucle principale permet de maintenir la fenêtre ouverte jusqu'à ce qu'elle soit fermée.
- `window.clear()` nettoie l'écran pour préparer un nouveau rendu.
- `window.display()` affiche le contenu de la fenêtre.

---

## Exercice 2 : Dessiner une forme
### Énoncé
Créez un rectangle de taille 200x100 pixels, de couleur rouge, et dessinez-le dans la fenêtre.

### Solution
```cpp
#include <SFML/Graphics.hpp>

int main() {
    sf::RenderWindow window(sf::VideoMode(800, 600), "Dessiner une Forme");

    // Création d'un rectangle
    sf::RectangleShape rectangle(sf::Vector2f(200.f, 100.f));
    rectangle.setFillColor(sf::Color::Red); // Remplissage rouge
    rectangle.setPosition(300.f, 250.f);    // Position centrale

    while (window.isOpen()) {
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed)
                window.close();
        }

        window.clear();
        window.draw(rectangle); // Dessiner le rectangle
        window.display();
    }

    return 0;
}
```

**Explications :**
- Le rectangle est créé avec `sf::RectangleShape` en spécifiant sa taille.
- La couleur est définie avec `setFillColor`.
- `setPosition` permet de placer le rectangle dans la fenêtre.

---

## Exercice 3 : Mouvement d'une forme
### Énoncé
Faites en sorte qu'un cercle de couleur bleue puisse se déplacer vers la droite en appuyant sur la flèche droite.

### Solution
```cpp
#include <SFML/Graphics.hpp>

int main() {
    sf::RenderWindow window(sf::VideoMode(800, 600), "Déplacer une Forme");

    // Création d'un cercle
    sf::CircleShape circle(50.f); // Rayon de 50 pixels
    circle.setFillColor(sf::Color::Blue);
    circle.setPosition(100.f, 300.f); // Position initiale

    while (window.isOpen()) {
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed)
                window.close();
        }

        // Vérification des touches
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Right)) {
            circle.move(5.f, 0.f); // Déplacer vers la droite
        }

        window.clear();
        window.draw(circle);
        window.display();
    }

    return 0;
}
```

**Explications :**
- `sf::Keyboard::isKeyPressed` vérifie si une touche est enfoncée.
- `circle.move` permet de déplacer le cercle de 5 pixels vers la droite à chaque image.

---
