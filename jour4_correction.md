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

## Mini Projet: Pong

### Solution
```cpp

#include <SFML/Graphics.hpp>
#include <SFML/Window.hpp>
#include <SFML/System.hpp>
#include <iostream>

// Dimensions de la fenêtre
const int WINDOW_WIDTH = 800;
const int WINDOW_HEIGHT = 600;

// Dimensions des raquettes
const float PADDLE_WIDTH = 10.f;
const float PADDLE_HEIGHT = 100.f;

// Rayon de la balle
const float BALL_RADIUS = 10.f;

// Vitesse de la balle
sf::Vector2f ballVelocity(0.4f, 0.4f);

int main() {
    // Créer la fenêtre
    sf::RenderWindow window(sf::VideoMode(WINDOW_WIDTH, WINDOW_HEIGHT), "Pong");
    window.setFramerateLimit(60);

    // Créer les raquettes
    sf::RectangleShape leftPaddle(sf::Vector2f(PADDLE_WIDTH, PADDLE_HEIGHT));
    leftPaddle.setFillColor(sf::Color::White);
    leftPaddle.setPosition(10.f, (WINDOW_HEIGHT - PADDLE_HEIGHT) / 2);

    sf::RectangleShape rightPaddle(sf::Vector2f(PADDLE_WIDTH, PADDLE_HEIGHT));
    rightPaddle.setFillColor(sf::Color::White);
    rightPaddle.setPosition(WINDOW_WIDTH - PADDLE_WIDTH - 10.f, (WINDOW_HEIGHT - PADDLE_HEIGHT) / 2);

    // Créer la balle
    sf::CircleShape ball(BALL_RADIUS);
    ball.setFillColor(sf::Color::White);
    ball.setPosition((WINDOW_WIDTH - BALL_RADIUS) / 2, (WINDOW_HEIGHT - BALL_RADIUS) / 2);

    // Variables pour le mouvement des raquettes
    float paddleSpeed = 0.5f;

    while (window.isOpen()) {
        // Gérer les événements
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed) {
                window.close();
            }
        }

        // Contrôles des raquettes
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::W) && leftPaddle.getPosition().y > 0) {
            leftPaddle.move(0, -paddleSpeed);
        }
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::S) && leftPaddle.getPosition().y + PADDLE_HEIGHT < WINDOW_HEIGHT) {
            leftPaddle.move(0, paddleSpeed);
        }
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Up) && rightPaddle.getPosition().y > 0) {
            rightPaddle.move(0, -paddleSpeed);
        }
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Down) && rightPaddle.getPosition().y + PADDLE_HEIGHT < WINDOW_HEIGHT) {
            rightPaddle.move(0, paddleSpeed);
        }

        // Mouvement de la balle
        ball.move(ballVelocity);

        // Collision avec les murs supérieur et inférieur
        if (ball.getPosition().y <= 0 || ball.getPosition().y + BALL_RADIUS * 2 >= WINDOW_HEIGHT) {
            ballVelocity.y = -ballVelocity.y;
        }

        // Collision avec les raquettes
        if (ball.getGlobalBounds().intersects(leftPaddle.getGlobalBounds()) ||
            ball.getGlobalBounds().intersects(rightPaddle.getGlobalBounds())) {
            ballVelocity.x = -ballVelocity.x;
        }

        // Réinitialiser la balle si elle sort de l'écran
        if (ball.getPosition().x < 0 || ball.getPosition().x > WINDOW_WIDTH) {
            ball.setPosition((WINDOW_WIDTH - BALL_RADIUS) / 2, (WINDOW_HEIGHT - BALL_RADIUS) / 2);
            ballVelocity.x = -ballVelocity.x; // Changer de direction
        }

        // Dessiner les éléments
        window.clear();
        window.draw(leftPaddle);
        window.draw(rightPaddle);
        window.draw(ball);
        window.display();
    }

    return 0;
}
```
