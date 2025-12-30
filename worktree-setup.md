---
description: Wizard do tworzenia nowego Git worktree. Krok po kroku pytania, validacja, i tworzenie izolowanego środowiska pracy. Po polsku z wyjaśnieniami.
argument-hint: [nazwa-branch'a] [ścieżka-worktree] [wiadomość-commit]
allowed-tools: Bash(git:*), Bash(cd:*), Bash(mkdir:*), Bash(pwd:*), Bash(test:*)
---

# Git Worktree Setup - Wizard

Stwórz nowe biurko robocze (worktree) z własną teczką (branch) i raportem (commit).

## 🎯 Analogia

- **BRANCH (teczka)** = folder z dokumentami projektu
- **WORKTREE (biurko)** = osobne miejsce pracy gdzie pracujesz nad teczką
- **COMMIT (raport)** = zapis pracy którą zrobiłeś

---

## 🚀 Proces Wizarda

### Krok 1: Weryfikacja Git Repository

Sprawdzenie czy jesteśmy w folderze z git repo. Jednocześnie wyodrębniamy nazwę projektu z bieżącego katalogu.

```
🔍 Szukam folderu .git...
📍 Nazwa projektu: agent-youtuber
```

**Jeśli `.git` ZNALEZIONY:**
```
✅ Znaleziono git repository!
📍 Lokalizacja: /Users/username/Projects/agent-youtuber
📁 Nazwa projektu: agent-youtuber

Możemy kontynuować → przejdź do Kroku 2
```

**Jeśli `.git` NIE ZNALEZIONY:**
```
❌ Nie znaleziono git repository!

📍 Bieżąca ścieżka: /Users/username/Projects/my-folder

Etap 1a - Co robić?

1️⃣  Zainicjalizuj git tutaj (git init)
2️⃣  Podaj ścieżkę do istniejącego repozytorium

Wybierz numer (1-2):
```

**Etap 1b - Jeśli opcja 1:**
```
Inicjalizuję git w: /Users/username/Projects/my-folder

✓ git init wykonane!
Możemy kontynuować → przejdź do Kroku 2
```

**Etap 1b - Jeśli opcja 2:**
```
Podaj ścieżkę do folderu z .git:
(np. /Users/username/Projects/my-project)

Wpisz ścieżkę:
```

---

### Krok 2: Nazwa Teczki (Branch Name)

Dwuetapowy wybór:

**Etap 2a - Typ teczki:**
```
📁 Jaki typ teczki (branch)?

1️⃣  feature/ - Nowa funkcjonalność
2️⃣  fix/ - Naprawa błędu
3️⃣  docs/ - Dokumentacja
4️⃣  refactor/ - Refaktor kodu
5️⃣  test/ - Testy
6️⃣  🔧 Custom/ - Wpisz własny typ

Wybierz numer (1-6):
```

**Etap 2b - Nazwa (po wyborze typu):**
```
Wpisz nazwę dla [wybrany-typ]:
```

Użytkownik wpisuje nazwę (bez sugestii, czysty input)

**Validacja:**
- ✓ Dozwolone: `feature/auth`, `fix/login-bug`, `docs/README`
- ✗ NIEDOZWOLONE: `feature/auth system` (spacje!), `Feature/Auth` (duże litery)

**Jeśli błąd:**
```
❌ BŁĄD: Nazwa zawiera niedozwolone znaki!

Branch name nie może zawierać:
- Spacji (użyj myślnika: auth-system zamiast auth system)
- Znaków specjalnych (!@#$%^&*)
- Dużych liter (tylko małe)

Spróbuj ponownie:
```

**Wynik:**
```
✓ Branch name: feature/auth-system
```

---

### Krok 3: Ścieżka Biurka (Worktree Path)

Menu wyboru ścieżki:

```
📍 Gdzie utworzyć biurko robocze (worktree)?

1️⃣  ../[project-name]--[branch-name] (domyślnie)
   Np: ../agent-youtuber--api-youtube-implementation

2️⃣  Inny folder...
   (wpisz własną ścieżkę)

Wybierz numer (1-2):
```

Jeśli wybierze 1 → użyj nazwy projektu + `--` + skonwertowana nazwa branch'a
Jeśli wybierze 2 → czekaj na custom ścieżkę

**Jeśli biurko już istnieje:**
```
❌ BŁĄD: Biurko już istnieje w: ../agent-youtuber--api-youtube-implementation

Opcje:
1️⃣  Usuń stare biurko (git worktree remove)
2️⃣  Podaj inną ścieżkę
3️⃣  Przerwij operację

Wybierz numer (1-3):
```

---

### Krok 4: Opis Zmian (Commit Message)

Dwuetapowy wybór:

**Etap 4a - Opis (menu):**
```
📝 Jaki opis zmian (commit message)?

1️⃣  [Typ] [nazwa ze spacjami] (auto)
    Np: Feature auth system

2️⃣  Custom - Wpisz własny opis

Wybierz numer (1-2):
```

**Jeśli opcja 1 (auto):**
```
Auto-generowany z branch'a:
Branch: feature/auth-system
Opis: Feature auth system
```

**Jeśli opcja 2 (custom):**
```
Wpisz opis zmian:
```

Użytkownik wpisuje bezpośrednio - brak sugestii

---

### Krok 5: Potwierdzenie i Wykonanie

Podsumowanie przed wykonaniem:

```
📋 PODSUMOWANIE:

Teczka (Branch):    feature/auth-system
Biurko (Worktree):  ../agent-youtuber--feature-auth-system
Raport (Commit):    Initial: feature/auth-system setup

Czy utworzyć? (t/n)
```

**Jeśli TAK (t):**

Wykonaj komendy:
```bash
# 1. Tworzenie biurka roboczego
git worktree add -b feature/auth-system ../agent-youtuber--feature-auth-system main

# 2. Przejście do biurka
cd ../agent-youtuber--feature-auth-system

# 3. Inicjalizacja git
git init

# 4. Dodanie plików
git add .

# 5. Napisanie raportu
git commit -m "Initial: feature/auth-system setup"
```

---

### Krok 6: Potwierdzenie Sukcesu

```
✅ SUKCES! Biurko gotowe!

📊 Informacje:
- Teczka (Branch): feature/auth-system
- Biurko (Worktree): /Users/username/Projects/agent-youtuber/../agent-youtuber--feature-auth-system
- Ostatni raport: Initial: feature/auth-system setup

🚀 Następnie:
cd ../agent-youtuber--feature-auth-system

Możesz teraz pracować na nowym biurku!
```

---

## ⚠️ Typowe Błędy i Rozwiązania

### ❌ "Spacja w nazwie branch'a"
```
Błąd: feature/auth system
✓ Poprawka: feature/auth-system (użyj myślnika)
```

### ❌ "Brak ukośnika w nazwie"
```
Błąd: feature-auth
✓ Poprawka: feature/auth
```

### ❌ "Wielkie litery"
```
Błąd: Feature/Auth
✓ Poprawka: feature/auth (małe litery)
```

### ❌ "Biurko już istnieje"
```
git worktree remove ../feature-auth
# Potem spróbuj znowu
```

---

## 💡 Porady

- Zawsze używaj format: `type/description`
- Typy: `feature`, `fix`, `docs`, `refactor`, `test`
- Opisy: łączyć myślnikami, nie spacjami
- Każde biurko = oddzielna gałąź = można pracować równolegle

