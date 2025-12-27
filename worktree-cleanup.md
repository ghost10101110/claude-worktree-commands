---
description: Usuń biurko robocze (worktree). Auto-naviguje do głównego, sprawdza zmiany, pyta o commit, usuwa. Po polsku z ochroną danych.
argument-hint: [force-delete]
allowed-tools: Bash(git:*), Bash(cd:*)
---

# Git Worktree Cleanup

Bezpiecznie usuń biurko robocze (worktree).

## Kroki do wykonania:

### Krok 1: Sprawdzenie gdzie jesteśmy
```bash
git rev-parse --git-common-dir
```

Jeśli jesteśmy w worktree → przejdź do Kroku 2
Jeśli NIE → powiedz że nie jesteśmy w worktree, przerwij

### Krok 2: Pobierz info o worktree
```bash
CURRENT_PATH=$(pwd)
CURRENT_BRANCH=$(git branch --show-current)
```

Pokaż: "Biurko do usunięcia: $CURRENT_PATH (branch: $CURRENT_BRANCH)"

### Krok 3: Znajdź główny worktree
```bash
git worktree list
```

Szukaj głównego repo (path do ai-social-media-manager lub podobnie)
Zapisz MAIN_PATH

### Krok 4: Przejdź do głównego
```bash
cd $MAIN_PATH
```

Pokaż: "Przechodzę do głównego: $MAIN_PATH"

### Krok 5: Sprawdź czy są niezacommitowane zmiany w feature worktree

```bash
cd $CURRENT_PATH
git status --short
```

Jeśli JEST output (są zmiany):
```
⚠️  UWAGA: Masz niezacommitowane zmiany w biurku!

Pliki:
- src/auth.ts (zmieniony)
- tests/test.ts (nowy)

Opcje:
1. Commituj zmiany teraz (git add . && git commit)
2. Wyrzuć zmiany (STRATA DANYCH!)
3. Anuluj cleanup

Wybierz (1/2/3):
```

Jeśli BRAK zmian:
Przejdź do Kroku 5b

### Krok 5b: Sprawdź czy są tagi na tym branchu (NOWE)

```bash
cd $CURRENT_PATH
TAGS=$(git tag --list --points-at HEAD 2>/dev/null)
```

Jeśli SĄ tagi:
```
⚠️  UWAGA: Na tym branchu są tagi!

Tagi:
- v1.0.0-instagram
- agent-v1

Czy chcesz je usunąć?

Opcje:
1. Skasuj tagi (STRATA REFERENCJI!)
2. Zachowaj tagi, tylko usuń worktree
3. Anuluj cleanup

Wybierz (1/2/3):
```

Jeśli 1:
```bash
git tag -d [tag-name]
git push origin --delete [tag-name]
```
Pokaż: "✅ Tagi skasowane. Przechodzę do Kroku 6"

Jeśli 2:
Przejdź do Kroku 6 (tagi zostaną, branch będzie usunięty)

Jeśli 3:
Anuluj cleanup

Jeśli BRAK tagów:
Przejdź do Kroku 6

### Krok 6: Potwierdzenie usunięcia i branch

```
📋 PODSUMOWANIE:

Biurko do usunięcia: $CURRENT_PATH
Branch:             $CURRENT_BRANCH
Główne repo:        $MAIN_PATH

Niezacommitowane zmiany: [TAK/NIE]
Tagi na branchu:        [jeśli są]
```

**Jeśli SĄ niezacommitowane zmiany (ale już poprzednio nie commitowane):**

```
⚠️  OSTRZEŻENIE: Biurko robocze "$CURRENT_BRANCH" zawiera niezacommitowane zmiany!

Jeśli usuniesz biurko i branch - WSZYSTKO ZGINIĘ BEZPOWROTNIE!

Opcje:
1️⃣  Zcommituj zmiany, potem usuń biurko i branch
2️⃣  Wyrzuć zmiany i usuń biurko + branch (STRATA DANYCH!)
3️⃣  Anuluj (nic nie rób)

Wybierz (1/2/3):
```

**Jeśli 1 (Commituj):**
```bash
cd $CURRENT_PATH
git add .
git commit -m "Work: $CURRENT_BRANCH - final commit before cleanup"
```

Pokaż wynik → przejdź do Kroku 7

**Jeśli 2 (Wyrzuć):**
```
❌ POTWIERDZENIE USUNIĘCIA:

Biurko:  $CURRENT_PATH
Branch:  $CURRENT_BRANCH
Status:  BĘDZIE USUNIĘTE BEZPOWROTNIE

Ostatnia szansa - czy na pewno? (WYRZUC/anuluj):
```

Jeśli "WYRZUC": przejdź do Kroku 7 (bez commita)
Jeśli inne: anuluj operację

**Jeśli 3 (Anuluj):**
Przerwij operację

**Jeśli BRAK niezacommitowanych zmian:**

```
📋 Biurko czysty, gotowe do usunięcia.

Czy chcesz usunąć biurko i branch?
1. Tak - usuń biurko i branch "$CURRENT_BRANCH"
2. Nie - zachowaj wszystko

Wybierz (1/2):
```

Jeśli 1: przejdź do Kroku 7
Jeśli 2: anuluj operację

### Krok 7: Usuń worktree i branch

```bash
cd $MAIN_PATH
git worktree remove $CURRENT_PATH
git branch -D $CURRENT_BRANCH
```

Pokaż wynik:
```
✅ CZYSZCZENIE ZAKOŃCZONE!

Usunięte:
- Biurko robocze: $CURRENT_PATH
- Branch: $CURRENT_BRANCH

Pozostało:
- Tagi (jeśli były)
- Commity w main (jeśli były merge'owane)
- Kod dostępny w historii git

Możesz teraz pracować nad innym branchem.
```

## WAŻNE:
- Auto-navegacja do głównego worktree
- Sprawdzenie niezacommitowanych zmian PRZED usunięciem
- Pytanie o commit jeśli są zmiany
- **TAGI**: Sprawdź tagi ZARAZ PRZED usunięciem worktree
- Ochrona przed stratą danych i referencji
- Pokaż wszystkie komendy które wykonujesz
- Przypomnienie: Tagi przetrwają nawet po usunięciu brancha
