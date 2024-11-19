
# Cours API Windows en C++

## Objectifs du cours
1. Comprendre les bases de l’API Windows.
2. Être capable de créer une fenêtre interactive.
3. Manipuler les événements utilisateur (clavier, souris).
4. Dessiner avec le GDI.
5. Utiliser des contrôles Windows standard comme les boutons, cases à cocher, et zones de texte.
6. Réaliser un projet complet en fin de formation.

---

## Partie 1 : Introduction et premier programme

### Théorie : Comprendre l’architecture des applications Windows
- Les concepts importants :
  - **Messages** : Les événements sont transmis sous forme de messages.
  - **Fonction principale `WinMain`** : Point d'entrée de l'application.
  - **Boucle de messages** : Gère les interactions avec le système.
  - **`WndProc`** : Fonction qui traite les messages (ex. clics souris, touches clavier).

### Exemple pratique : Première fenêtre Windows

```cpp
#include <windows.h>

// Déclaration de la fonction de traitement des messages
LRESULT CALLBACK WndProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam);

// Point d'entrée de l'application
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow) {
    // 1. Définir la classe de fenêtre
    WNDCLASS wc = {};
    wc.lpfnWndProc = WndProc;            // Pointeur vers la fonction de traitement des messages
    wc.hInstance = hInstance;           // Instance du programme
    wc.lpszClassName = L"MainWindow";   // Nom unique pour la classe de fenêtre
    wc.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1); // Couleur de fond

    // 2. Enregistrer la classe
    if (!RegisterClass(&wc)) {
        MessageBox(NULL, L"Erreur : Enregistrement de la classe échoué.", L"Erreur", MB_ICONERROR);
        return 0;
    }

    // 3. Créer la fenêtre
    HWND hwnd = CreateWindowEx(
        0,                              // Pas de styles étendus
        L"MainWindow",                  // Nom de la classe
        L"Ma première application",     // Titre de la fenêtre
        WS_OVERLAPPEDWINDOW,            // Style de la fenêtre
        CW_USEDEFAULT, CW_USEDEFAULT,   // Position par défaut
        800, 600,                       // Taille de la fenêtre
        NULL,                           // Pas de fenêtre parent
        NULL,                           // Pas de menu
        hInstance,                      // Instance
        NULL                            // Pas de paramètres supplémentaires
    );

    if (hwnd == NULL) {
        MessageBox(NULL, L"Erreur : Création de la fenêtre échouée.", L"Erreur", MB_ICONERROR);
        return 0;
    }

    ShowWindow(hwnd, nCmdShow); // Affiche la fenêtre
    UpdateWindow(hwnd);         // Met à jour la fenêtre

    // 4. Boucle des messages
    MSG msg = {};
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg); // Traduit les messages clavier
        DispatchMessage(&msg);  // Envoie les messages à WndProc
    }

    return (int)msg.wParam;
}

// Fonction de traitement des messages
LRESULT CALLBACK WndProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam) {
    switch (uMsg) {
        case WM_CLOSE:
            DestroyWindow(hwnd);
            break;

        case WM_DESTROY:
            PostQuitMessage(0); // Quitte l'application
            break;

        default:
            return DefWindowProc(hwnd, uMsg, wParam, lParam); // Traitement par défaut
    }
    return 0;
}
```

#### Explication du code
- **`RegisterClass`** : Enregistre une classe de fenêtre que le système utilisera pour créer des fenêtres.
- **`CreateWindowEx`** : Crée une instance de fenêtre.
- **`WndProc`** : Traite les messages comme `WM_CLOSE` (fermeture) et `WM_DESTROY` (destruction de la fenêtre).

### Exercice 1 :
- Créez une fenêtre de dimensions personnalisées (400x300 pixels).
- Changez la couleur de fond en bleu (indice : utilisez `CreateSolidBrush`).

---

## Partie 2 : Interactions utilisateur avec la souris et le clavier

### Théorie : Gestion des événements utilisateur
- **Messages souris** : `WM_LBUTTONDOWN`, `WM_RBUTTONDOWN`, `WM_MOUSEMOVE`.
- **Messages clavier** : `WM_KEYDOWN`, `WM_CHAR`.

### Exemple : Afficher les clics souris

```cpp
case WM_LBUTTONDOWN:
    MessageBox(hwnd, L"Vous avez cliqué avec le bouton gauche de la souris !", L"Souris", MB_OK);
    break;

case WM_RBUTTONDOWN:
    MessageBox(hwnd, L"Vous avez cliqué avec le bouton droit de la souris !", L"Souris", MB_OK);
    break;

case WM_MOUSEMOVE:
    wchar_t buffer[50];
    swprintf(buffer, 50, L"Position de la souris : (%d, %d)", GET_X_LPARAM(lParam), GET_Y_LPARAM(lParam));
    SetWindowText(hwnd, buffer); // Met à jour le titre de la fenêtre
    break;
```

### Exemple : Gestion des touches clavier

```cpp
case WM_KEYDOWN:
    if (wParam == VK_ESCAPE) { // Touche Échap
        PostQuitMessage(0);
    } else if (wParam == 'A') { // Touche 'A'
        MessageBox(hwnd, L"Vous avez appuyé sur A !", L"Clavier", MB_OK);
    }
    break;
```
### Linker une console à mon application
```cpp
    //// WinMain
    AllocConsole();
    FILE* file;
    freopen_s(&file, "CONOUT$", "w", stdout);
    freopen_s(&file, "CONOUT$", "w", stderr);
    freopen_s(&file, "CONIN$", "r", stdin);
    std::wcout.clear();
    std::cout.clear();
    std::cerr.clear();
    //// Code de mon appli
    FreeConsole(); // avant le return de fin d'appli


```
### Exercice 2 :
- Affichez un message dans la console (`std::wcout`) chaque fois que l'utilisateur déplace la souris.
- Changez la couleur de la fenêtre à chaque pression sur une touche (R = rouge, G = vert, B = bleu).

---

## Partie 3 : Interface utilisateur avec des contrôles standard

### Exemple : Ajouter un bouton interactif

```cpp
HWND hButton = CreateWindowEx(
    0, L"BUTTON", L"Appuyez ici",
    WS_TABSTOP | WS_VISIBLE | WS_CHILD | BS_DEFPUSHBUTTON,
    50, 50, 150, 50, hwnd, (HMENU)1, NULL, NULL
);

case WM_COMMAND:
    if (LOWORD(wParam) == 1) { // ID du bouton
        MessageBox(hwnd, L"Vous avez cliqué sur le bouton !", L"Interaction", MB_OK);
    }
    break;
```

### Exercice 3 :
- Ajoutez une zone de texte (`EDIT`) et un bouton. Lorsque l'utilisateur clique sur le bouton, affichez le contenu de la zone de texte dans une `MessageBox`.

---

## Partie 4 : Projet final

### Objectif : Construire une mini-calculatrice
#### Fonctionnalités :
- Une interface avec :
  - Deux zones de texte (`EDIT`) pour les nombres.
  - Un bouton pour chaque opération (+, -, *, /).
  - Un label (`STATIC`) pour afficher le résultat.

#### Instructions de développement :
1. Créez une fenêtre avec tous les contrôles nécessaires.
2. Récupérez les entrées utilisateur avec `GetWindowText`.
3. Calculez le résultat en fonction du bouton cliqué.
4. Affichez le résultat dans un label.
