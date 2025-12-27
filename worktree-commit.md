---
description: Wizard do tworzenia commit'u w worktree. Sprawdza zmiany, pyta o wiadomość, commituje. Bez merge'a. Po polsku z wyjaśnieniami.
argument-hint: [commit-message]
allowed-tools: Bash(git:*), Bash(cd:*), Bash(pwd:*)
---

# Git Worktree Commit - Wizard

Napisz raport (commit) z pracy na biurku roboczym (worktree) bez mergowania do głównej teczki.

## 🎯 Analogia

- **Worktree (biurko)** = osobne biurko gdzie pracujesz
- **Commit (raport)** = zapis co zrobiłeś na tym biurku
- **Stage (przygotowanie)** = zbieranie raportu do wysłania
- **Brak merge'a** = raport zostaje na twoim biurku, nie idzie szefowi

---

## 🚀 Proces Wizarda

### Krok 1: Weryfikacja Worktree

Sprawdź czy jesteśmy w folderze worktree.

```
🔍 Sprawdzam czy jesteśmy w worktree...
```

**Jeśli NIE jesteśmy w worktree:**
```
❌ To nie jest worktree!

Musisz być w folderze worktree aby commitować.
Aktualna ścieżka: /Users/username/my-project

Przerwij operację i wróć do worktree:
cd ../[worktree-path]
```

**Jeśli jesteśmy w worktree:**
```
✅ Jesteśmy w worktree!

📍 Bieżąca ścieżka: /Users/username/my-project/../worktree-feature
📁 Bieżący branch: feature/auth-system

Możemy kontynuować → przejdź do Kroku 2
```

---

### Krok 2: Status Zmian

Pokaż co się zmieniło:

```
📊 Status zmian na biurku:

Zmodyfikowane pliki:
- src/auth.ts
- src/login.ts

Nowe pliki:
- tests/auth.test.ts

Podsumowanie:
- Pliki zmienione: 3
- Insertions: +45
- Deletions: -12

Możemy kontynuować → przejdź do Kroku 3
```

**Jeśli brak zmian:**
```
⚪ Brak zmian do commitowania!

Wszystko już jest w poprzednim commit'e.

Opcje:
1️⃣  Wróć i rób więcej zmian
2️⃣  Przerwij

Wybierz numer (1-2):
```

---

### Krok 3: Wiadomość Commit'a

Menu wyboru opisu commit'a:

```
📝 Jaki opis zmian (commit message)?

1️⃣  Work: [branch-name] - progress
    (auto-generowana)

2️⃣  Custom - Wpisz własny opis

Wybierz numer (1-2):
```

**Jeśli opcja 1 (auto):**
```
Auto-generowana z branch'a:
Branch: feature/auth-system
Opis: Work: feature/auth-system - progress
```

**Jeśli opcja 2 (custom):**
```
Wpisz opis zmian (co zrobiłeś?):
```

---

### Krok 4: Przygotowanie do Commit'u

Sprawdzenie przed commitowaniem:

```
📋 Przygotowanie...

✓ Wszystkie pliki będą dodane (git add .)
✓ Gotowe do commit'u

Możemy kontynuować → przejdź do Kroku 5
```

---

### Krok 5: Potwierdzenie i Wykonanie

Podsumowanie przed commit'em:

```
📋 PODSUMOWANIE COMMIT'U:

Biurko (Worktree): /Users/username/my-project/../worktree-feature
Bieżący branch:    feature/auth-system
Wiadomość:         Work: feature/auth-system - progress

Pliki do dodania:  3
Zmiany:            +45 -12

Czy commitować? (t/n)
```

**Jeśli TAK (t):**

Wykonaj komendy:
```bash
# 1. Dodanie wszystkich zmian
git add .

# 2. Commit ze zmianami
git commit -m "Work: feature/auth-system - progress"
```

---

### Krok 6: Potwierdzenie Sukcesu

```
✅ COMMIT WYKONANY!

📊 Informacje:
- Biurko (Worktree): feature/auth-system
- Wiadomość:         Work: feature/auth-system - progress
- Status:            zmieniony

📈 Statystyki:
- Pliki zmienione: 3
- Insertions: +45
- Deletions: -12

🎯 Następnie:
- Rób więcej zmian i commituj ponownie
- Lub zmerguj do głównego branch'a (/worktree-merge)
- Lub usuń biurko (/worktree-cleanup)
```

---

## ⚠️ Typowe Problemy i Rozwiązania

### ❌ "Nie jesteśmy w worktree"
```
❌ BŁĄD: To nie jest worktree!
✓ Rozwiązanie: cd ../[path-to-worktree]
```

### ❌ "Brak zmian do commitowania"
```
⚪ Working tree clean - brak zmian

Zrób zmiany w plikach potem spróbuj ponownie
```

### ❌ "Błąd przy git add"
```
❌ BŁĄD: Nie mogę dodać pliku X

Sprawdź czy plik nie jest zignorowany (.gitignore)
```

---

## 💡 Porady

- Commituj regularnie podczas pracy
- Każdy commit = logiczna część pracy
- Później łatwo zmergować do głównego branch'a
- Możesz commitować wiele razy zanim merge'asz

