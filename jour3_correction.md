
# Correction des exercices : API Windows en C++

## **Exercice 1 : Créez une fenêtre personnalisée**

### Objectif :
- Modifier la taille de la fenêtre à 400x300 pixels.
- Changer la couleur de fond en bleu.

### Code corrigé :
```cpp
#include <windows.h>

LRESULT CALLBACK WndProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam);

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow) {
    WNDCLASS wc = {};
    wc.lpfnWndProc = WndProc;
    wc.hInstance = hInstance;
    wc.lpszClassName = L"MainWindow";
    wc.hbrBackground = CreateSolidBrush(RGB(0, 0, 255)); // Fond bleu

    if (!RegisterClass(&wc)) {
        MessageBox(NULL, L"Erreur : Enregistrement de la classe échoué.", L"Erreur", MB_ICONERROR);
        return 0;
    }

    HWND hwnd = CreateWindowEx(
        0, L"MainWindow", L"Fenêtre Personnalisée",
        WS_OVERLAPPEDWINDOW, CW_USEDEFAULT, CW_USEDEFAULT, 400, 300, NULL, NULL, hInstance, NULL
    );

    if (hwnd == NULL) {
        MessageBox(NULL, L"Erreur : Création de la fenêtre échouée.", L"Erreur", MB_ICONERROR);
        return 0;
    }

    ShowWindow(hwnd, nCmdShow);
    UpdateWindow(hwnd);

    MSG msg = {};
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }

    return (int)msg.wParam;
}

LRESULT CALLBACK WndProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam) {
    switch (uMsg) {
        case WM_CLOSE:
            DestroyWindow(hwnd);
            break;
        case WM_DESTROY:
            PostQuitMessage(0);
            break;
        default:
            return DefWindowProc(hwnd, uMsg, wParam, lParam);
    }
    return 0;
}
```

---

## **Exercice 2 : Gestion des entrées utilisateur**

### Objectif :
- Afficher un message dans la console chaque fois que la souris bouge.
- Changer la couleur de la fenêtre en fonction des touches R, G ou B.

### Code corrigé :
```cpp
#include <windows.h>
#include <windowsx.h>
#include <iostream>

LRESULT CALLBACK WndProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam);

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow) {
    AllocConsole();
    FILE* file;
    freopen_s(&file, "CONOUT$", "w", stdout);
    freopen_s(&file, "CONOUT$", "w", stderr);
    freopen_s(&file, "CONIN$", "r", stdin);
    std::wcout.clear();
    std::cout.clear();
    std::cerr.clear();

    WNDCLASS wc = {};
    wc.lpfnWndProc = WndProc;
    wc.hInstance = hInstance;
    wc.lpszClassName = L"MainWindow";
    wc.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1); // Fond par défaut

    if (!RegisterClass(&wc)) {
        MessageBox(NULL, L"Erreur : Enregistrement de la classe échoué.", L"Erreur", MB_ICONERROR);
        return 0;
    }

    HWND hwnd = CreateWindowEx(
        0, L"MainWindow", L"Gestion des Entrées",
        WS_OVERLAPPEDWINDOW, CW_USEDEFAULT, CW_USEDEFAULT, 800, 600, NULL, NULL, hInstance, NULL
    );

    if (hwnd == NULL) {
        MessageBox(NULL, L"Erreur : Création de la fenêtre échouée.", L"Erreur", MB_ICONERROR);
        return 0;
    }

    ShowWindow(hwnd, nCmdShow);
    UpdateWindow(hwnd);

    MSG msg = {};
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    FreeConsole();
    return (int)msg.wParam;
}

HBRUSH hBrush = (HBRUSH)(COLOR_WINDOW + 1); // Couleur initiale

LRESULT CALLBACK WndProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam) {
    switch (uMsg) {
    case WM_MOUSEMOVE:
        std::wcout << L"Position de la souris : (" << GET_X_LPARAM(lParam) << L", " << GET_Y_LPARAM(lParam) << L")" << std::endl;
        break;

    case WM_KEYDOWN:
        if (wParam == 'R') {
            hBrush = CreateSolidBrush(RGB(255, 0, 0)); // Rouge
            InvalidateRect(hwnd, NULL, TRUE);
        }
        else if (wParam == 'G') {
            hBrush = CreateSolidBrush(RGB(0, 255, 0)); // Vert
            InvalidateRect(hwnd, NULL, TRUE);
        }
        else if (wParam == 'B') {
            hBrush = CreateSolidBrush(RGB(0, 0, 255)); // Bleu
            InvalidateRect(hwnd, NULL, TRUE);
        }
        break;

        case WM_ERASEBKGND:
        {
            HDC hdc = (HDC)wParam;
            RECT rect;
            GetClientRect(hwnd, &rect);
            FillRect(hdc, &rect, hBrush); // Dessine l'arrière-plan avec la brosse active
            return 1; // Indique que nous avons traité l’effacement du fond
        }

    case WM_CLOSE:
        DestroyWindow(hwnd);
        break;

    case WM_DESTROY:
        PostQuitMessage(0);
        break;

    default:
        return DefWindowProc(hwnd, uMsg, wParam, lParam);
    }
    return 0;
}
```
## **Exercice 3 : Interface avec un bouton et une zone de texte**

### Objectif :
- Ajouter une zone de texte et un bouton.
- Afficher le texte saisi dans une `MessageBox` lorsqu'on clique sur le bouton.

### Code corrigé :
```cpp
#include <windows.h>

LRESULT CALLBACK WndProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam);

HWND hEdit, hButton;

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow) {
    WNDCLASS wc = {};
    wc.lpfnWndProc = WndProc;
    wc.hInstance = hInstance;
    wc.lpszClassName = L"MainWindow";
    wc.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);

    if (!RegisterClass(&wc)) {
        MessageBox(NULL, L"Erreur : Enregistrement de la classe échoué.", L"Erreur", MB_ICONERROR);
        return 0;
    }

    HWND hwnd = CreateWindowEx(
        0, L"MainWindow", L"Interface Utilisateur",
        WS_OVERLAPPEDWINDOW, CW_USEDEFAULT, CW_USEDEFAULT, 500, 200, NULL, NULL, hInstance, NULL
    );

    if (hwnd == NULL) {
        MessageBox(NULL, L"Erreur : Création de la fenêtre échouée.", L"Erreur", MB_ICONERROR);
        return 0;
    }

    ShowWindow(hwnd, nCmdShow);
    UpdateWindow(hwnd);

    MSG msg = {};
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }

    return (int)msg.wParam;
}

LRESULT CALLBACK WndProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam) {
    switch (uMsg) {
        case WM_CREATE:
            hEdit = CreateWindowEx(0, L"EDIT", L"", WS_CHILD | WS_VISIBLE | WS_BORDER, 50, 50, 300, 20, hwnd, NULL, NULL, NULL);
            hButton = CreateWindowEx(0, L"BUTTON", L"Afficher", WS_CHILD | WS_VISIBLE, 360, 50, 80, 20, hwnd, (HMENU)1, NULL, NULL);
            break;

        case WM_COMMAND:
            if (LOWORD(wParam) == 1) { // Bouton cliqué
                wchar_t buffer[256];
                GetWindowText(hEdit, buffer, 256); // Récupère le texte
                MessageBox(hwnd, buffer, L"Contenu de la zone de texte", MB_OK);
            }
            break;

        case WM_CLOSE:
            DestroyWindow(hwnd);
            break;

        case WM_DESTROY:
            PostQuitMessage(0);
            break;

        default:
            return DefWindowProc(hwnd, uMsg, wParam, lParam);
    }
    return 0;
}
```

---
