# 🚀 Claude Worktree Commands

Zestaw 4 globalnych komend Claude Code do zarządzania Git worktree'ami.

---

## ⚡ Szybka Instalacja

### Krok 1: Klonuj repo
```bash
git clone https://github.com/ghost10101110/claude-worktree-commands.git
cd claude-worktree-commands
```

### Krok 2: Otwórz Claude Code i wpisz:
```
zainstaluj mi te bash komendy globalnie do ~/.claude/commands/
```

### Krok 3: Restart Claude Code

Gotowe! ✅

---

## 📚 Pełna Dokumentacja

Każda komenda ma szczegółową dokumentację:

## 🎯 Analogia - Zrozumienie Pojęć

- **BRANCH (teczka)** = folder z dokumentami projektu (np. `feature/auth`)
- **WORKTREE (biurko)** = osobne miejsce pracy gdzie pracujesz nad teczką
- **COMMIT (raport)** = zapis pracy którą zrobiłeś
- **MERGE** = przeniesienie raportu z biurka do teczki szefa (main)

---

## 📋 Workflow - Jak to Działa

```
1. /worktree-setup
   ↓
   Stwórz biurko robocze z feature branch'em

2. Rób zmiany w plikach
   ↓

3. /worktree-commit (powtarzaj tyle razy ile trzeba)
   ↓
   Commituj zmiany na swoim biurku

4. Gdy skończysz pracę:
   ↓

5. /worktree-merge
   ↓
   ├─ Przenieś zmiany do main
   ├─ [NOWE] Pytanie o tag
   │  ├─ Ręczny tag (np. v1.0.0)
   │  ├─ Auto-sugestia wersji
   │  └─ Pomiń tag
   └─ Push tagu do remote

6. /worktree-cleanup
   ↓
   ├─ [NOWE] Sprawdź czy są niezacommitowane zmiany
   ├─ [NOWE] Sprawdź czy są tagi (zachowaj/usuń)
   ├─ Usuń biurko robocze
   └─ [NOWE] Automatycznie usuń branch
```

---

## 🚀 Komendy Szczegółowo

### 1️⃣ `/worktree-setup` - Stwórz Biurko

**Co robi:**
Tworzy nowe biurko robocze (worktree) z własnym branch'em i pierwszym commit'em.

**Kroki:**
1. Walidacja: Sprawdza czy jesteśmy w git repo
2. Typ branch'a: Pyta `feature/`, `fix/`, `docs/`, `refactor/`, `test/` czy `custom/`
3. Nazwa: Pyta o konkretną nazwę (np. "auth-system")
4. Ścieżka: Domyślnie `../feature-auth-system` lub custom
5. Opis: Pyta o opis zmian (auto lub custom)
6. Potwierdzenie: Pytanie czy start
7. Wykonanie: Tworzy biurko i robi commit

**Git komendy:**
```bash
# 1. Tworzenie worktree z nowym branch'em
git worktree add -b feature/auth-system ../feature-auth-system main

# 2. Przejście do biurka
cd ../feature-auth-system

# 3. Inicjalizacja (jeśli potrzebna)
git init

# 4. Dodanie plików
git add .

# 5. Pierwszy commit
git commit -m "Initial: feature/auth-system setup"
```

**Użycie:**
```
/worktree-setup
```

---

### 2️⃣ `/worktree-commit` - Commituj Zmiany

**Co robi:**
Commituje zmiany w biurku roboczym **bez** mergowania do main.

**Kroki:**
1. Walidacja: Sprawdza czy jesteśmy w worktree
2. Status: Pokazuje zmienione pliki
3. Opis: Auto (`Work: feature/auth-system - progress`) lub custom
4. Potwierdzenie: Czy commitować
5. Wykonanie: Dodaje i commituje pliki
6. Rezultat: Pokazuje statystykę zmian

**Git komendy:**
```bash
# 1. Dodanie wszystkich zmian
git add .

# 2. Commit ze zmianami
git commit -m "Work: feature/auth-system - progress"
```

**Użycie:**
```
/worktree-commit
```

Możesz to robić **wiele razy** podczas pracy.

---

### 3️⃣ `/worktree-merge` - Zmerguj do Main + Tag

**Co robi:**
Automatycznie naviguje do głównego worktree, merguje zmiany i tworzy tag (opcjonalnie).

**Kroki:**
1. Pobiera bieżący branch (`feature/auth-system`)
2. Auto-znajduje główny worktree (np. `ai-social-media-manager`)
3. Auto-naviguje tam
4. Pyta do którego branch'a (main/develop/custom)
5. Pyta o wiadomość merge'a
6. Potwierdzenie
7. Wykonuje merge
8. **[NOWE]** Pyta o tag:
   - Ręczne wpisanie tagu (np. `v1.0.0`)
   - Pomiń tag
   - Auto-sugestia wersji (v1.0.0 → v1.0.1)
9. Jeśli tag: tworzy + pushuje do remote
10. Oferuje usunięcie worktree

**Git komendy:**
```bash
# 1. Przejście do głównego worktree (auto)
cd /Users/adrianmagiera/Documents/ai-team/_demo/AI_SMM/ai-social-media-manager

# 2. Checkout docelowego branch'a
git checkout main

# 3. Merge z feature branch'a
git merge feature/auth-system -m "Merge: feature/auth-system → main"

# 4. [NOWE] Tagowanie (opcjonalnie)
git tag -a v1.0.0 -m "Merge feature/auth-system do main"
git push origin v1.0.0
```

**Użycie:**
```
/worktree-merge
```

**Ważne:**
- Merge zawsze z **głównego worktree**, nie z feature'owego!
- Tagi są permanentne - mogą pozostać nawet po usunięciu branch'a

---

### 4️⃣ `/worktree-cleanup` - Usuń Biurko + Branch + Sprawdź Tagi

**Co robi:**
Bezpiecznie usuwa biurko robocze i branch z ochroną przed stratą danych i referencji.

**Kroki:**
1. Walidacja: Sprawdza czy jesteśmy w worktree
2. Auto-navigacja: Przechodzi do głównego
3. Sprawdzenie zmian: Czy są niezacommitowane zmiany
   - Jeśli TAK → Commituj / Wyrzuć (potwierdź 2x) / Anuluj
4. **[NOWE]** Sprawdzenie tagów: Czy są tagi na HEAD
   - Jeśli TAK → Usuń tagi / Zachowaj tagi / Anuluj
5. Potwierdzenie: Na pewno usunąć biurko i branch?
6. Wykonanie: Usuwa worktree + **automatycznie branch**
7. Rezultat: Podsumowanie co zostało

**Git komendy:**
```bash
# 1. Przejście do głównego (auto)
cd /Users/adrianmagiera/Documents/ai-team/_demo/AI_SMM/ai-social-media-manager

# 2. [NOWE] Sprawdzenie tagów
git tag --list --points-at HEAD

# 3. Usunięcie worktree
git worktree remove /Users/adrianmagiera/Documents/ai-team/_demo/AI_SMM/feature-auth-system

# 4. [NOWE] Automatyczne usunięcie branch'a
git branch -D feature/auth-system

# 5. [OPCJONALNIE] Jeśli chcesz usunąć tag
git tag -d v1.0.0
git push origin --delete v1.0.0
```

**Użycie:**
```
/worktree-cleanup
```

**Co się dzieje:**
- ✅ Sprawdza czy są niezacommitowane zmiany (oferuje commit)
- ✅ Sprawdza czy są tagi na branchu (oferuje ich zachowanie)
- ✅ **Automatycznie usuwa branch** (nie trzeba ręcznie)
- ✅ Tagi pozostają nawet po usunięciu branch'a (są permanentne)

---

## ⚠️ Typowe Błędy i Rozwiązania

### ❌ "Branch jest już używany przez inne worktree"
```
Błąd: 'main' is already used by worktree
```
**Rozwiązanie:** Merge zawsze z głównego worktree, nie z feature'owego.

### ❌ "Worktree zawiera niezacommitowane zmiany"
```
Błąd: contains modified or untracked files
```
**Rozwiązanie:**
- Commituj zmiany zanim usuniesz biurko
- Lub użyj `/worktree-cleanup` który pyta o commit

### ❌ "Fast-forward merge - brak commit'u"
```
Fast-forward (no commit created)
```
**To normalne!** Oznacza że merge przebiegł bez konfliktów.

### ❌ "Plik nie widać po merge'u"
```
Plik dodany ale commit pusty
```
**Rozwiązanie:** Pamiętaj aby commitować **po** dodaniu plików:
1. Dodaj plik
2. `/worktree-commit` ← tutaj!
3. `/worktree-merge`

---

## 💡 Best Practices

### Workflow Codzienny

```
Poniedziałek:
/worktree-setup feature/nowa-funkcja

Poniedziałek-Czwartek:
[Rób zmiany]
/worktree-commit (każdy dzień)

Piątek:
/worktree-merge
/worktree-cleanup
```

### Nazwowanie Branch'y

- `feature/auth` - nowa funkcja
- `fix/login-bug` - naprawa błędu
- `docs/readme` - dokumentacja
- `refactor/database` - refaktor kodu
- `test/integration` - testy

**Format:** `type/description` (małe litery, myślniki zamiast spacji)

### Wiadomości Commit'ów

- `Initial: feature/auth setup` - pierwszy commit
- `Work: feature/auth - progress` - intermediate
- `Feature: auth - complete` - końcowy

### Bezpieczeństwo

1. **Zawsze commituj** zanim zmienisz branch
2. **Pulluj main** zanim zaczynsz pracę na nowym feature
3. **Reviewuj zmiany** przed merge'em do main
4. **Testuj** na feature branch'u zanim mergowaniem
5. **Taguj ważne merge'i** - umożliwia powrót do konkretnych stanów
6. **Tagi > branche** - jeśli chcesz zachować referencję, użyj tagu nie branch'a

---

## 🔧 Troubleshooting

### Komendy nie działają

1. Sprawdź czy pliki są w `~/.claude/commands/`
2. Zamknij i otwórz Claude Code
3. Wpisz `/help` aby zobaczyć dostępne komendy

### Merge się nie powiedzie

1. Sprawdź czy jesteś w worktree (`git branch --show-current`)
2. Sprawdź czy główny worktree istnieje (`git worktree list`)
3. Spróbuj merge ręcznie:
   ```bash
   git checkout main
   git merge feature/auth-system
   ```

### Worktree się nie tworzy

1. Sprawdź czy jesteś w git repo (`git status`)
2. Sprawdź czy branch już istnieje (`git branch -a`)
3. Spróbuj ręcznie:
   ```bash
   git worktree add -b feature/test ../test-worktree main
   ```

---

## 📚 Dodatkowe Zasoby

### Git Worktree Dokumentacja
```bash
man git-worktree
git worktree --help
```

### Przydatne Komendy

```bash
# Pokaż wszystkie worktree
git worktree list

# Pokaż historię commit'ów
git log --oneline

# Sprawdź status
git status

# Pokaż gałęzie
git branch -a

# Usuń lokalny branch
git branch -d feature/test

# Usuń branch'ę siłą
git branch -D feature/test
```

---

## 📞 Pytania?

Jeśli komendy nie działają jak się spodziewasz:

1. Sprawdź czy jesteś w git repo
2. Sprawdź `git status` aby zobaczyć obecny stan
3. Spróbuj ręcznych komend z dokumentacji powyżej
4. Sprawdź czy Claude Code jest najnowszej wersji

---

## 🏷️ Tagging (Nowe)

### Dlaczego tagi?

- **Permanentna referencja** - tag żyje nawet po usunięciu branch'a
- **Wersjonowanie** - łatwo znaleźć konkretny stan kodu
- **Release markers** - oznaczenie oficjalnych wydań
- **Backup history** - historia żyje w tagach, nie branchy

### Strategie Tagowania

#### Opcja 1: Semantic Versioning
```
v1.0.0 - major.minor.patch
v1.0.1 - patch fix
v1.1.0 - minor feature
v2.0.0 - major break
```

#### Opcja 2: Feature-Based
```
instagram-agent-v1
auth-system-v1
database-migration-v1
```

#### Opcja 3: Combined
```
v1.0.0-instagram-agent
v1.1.0-auth-system
```

### Workflow z Tagowaniem

```bash
# Normalna praca
/worktree-setup feature/instagram-agent
# ... rób zmiany ...
/worktree-commit
/worktree-commit
/worktree-merge
  → "Ostatni tag: brak"
  → "Wybierz opcję: [1] Ręczny tag [2] Pomiń [3] Auto-sugestia"
  → Wybierasz: 1
  → Wpisujesz: instagram-agent-v1
  → Tag stworzony: instagram-agent-v1 ✅

# Potem cleanup - tagi są bezpieczne
/worktree-cleanup
  → "Czy usunąć tagi na branchu?"
  → "instagram-agent-v1"
  → Wybierasz: 2 (Zachowaj)
  → Branch usunięty, tag żyje ✅
```

### Przydatne Komendy do Tagów

```bash
# Pokaż wszystkie tagi
git tag -l

# Pokaż szczegóły konkretnego tagu
git show instagram-agent-v1

# Powróć do tagu (checkout)
git checkout instagram-agent-v1

# Usuń lokalnie
git tag -d instagram-agent-v1

# Usuń z remote
git push origin --delete instagram-agent-v1

# Pokaż co jest w tagu
git show v1.0.0
```

---

**Wersja:** 2.0 (z Tagowaniem)
**Data:** Grudzień 2025
**Autor:** Claude Code
