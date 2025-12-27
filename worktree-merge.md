---
description: Zmerguj zmiany z worktree do głównego branch'a. Auto-wykrywa main, naviguje tam, i wykonuje merge.
argument-hint: [target-branch] [merge-message]
allowed-tools: Bash(git:*), Bash(cd:*)
---

# Git Worktree Merge

Automatycznie znajdź główny worktree, przejdź tam, i zmerguj.

## Kroki do wykonania:

### Krok 1: Pobierz bieżący branch
```bash
CURRENT_BRANCH=$(git branch --show-current)
```

Pokaż: "Bieżący branch: $CURRENT_BRANCH"

### Krok 2: Znajdź główny worktree
```bash
git worktree list
```

Szukaj worktree który ma `(bare)` lub jest głównym repo (path kończy się na `/ai-social-media-manager` lub podobnie)

Jeśli znalazłeś → zapisz MAIN_PATH
Jeśli nie znalazłeś → powiedz że nie ma głównego worktree, przerwij

### Krok 3: Przejdź do głównego
```bash
cd $MAIN_PATH
```

Pokaż: "Przechodzę do: $MAIN_PATH"

### Krok 4: Pytaj o target branch
```
Do którego branch'a chcesz zmergować $CURRENT_BRANCH?

1. main (domyślnie)
2. develop
3. Custom (wpisz nazwę)

Wybierz:
```

Czekaj na odpowiedź (1, 2, lub 3)
Jeśli 3 → pytaj o nazwę branch'a

### Krok 5: Merge message
```
Jaki opis merge'a?

1. Merge: $CURRENT_BRANCH → $TARGET (auto)
2. Custom - wpisz

Wybierz:
```

Jeśli 1 → użyj auto
Jeśli 2 → czekaj na wpisanie

### Krok 6: Potwierdzenie
```
Podsumowanie:
- Z branch: $CURRENT_BRANCH
- Do branch: $TARGET
- Wiadomość: $MESSAGE
- Lokalizacja: $MAIN_PATH

Czy kontynuować? (t/n)
```

### Krok 7: Wykonaj merge
Jeśli "t":
```bash
git checkout $TARGET
git merge $CURRENT_BRANCH -m "$MESSAGE"
```

Pokaż wynik merge'a

Jeśli "n":
Przerwij operację

### Krok 8: Tagowanie (nowe)

Po udanym merge'u, sprawdź czy chcesz dodać tag:

```bash
# Pobierz ostatni tag
LAST_TAG=$(git describe --tags --abbrev=0 2>/dev/null || echo "brak")
```

Pokaż:
```
📌 Ostatni tag: $LAST_TAG

Czy chcesz dodać tag dla tego merge'a?
1. Tak - chcę stworzyć tag
2. Nie - pomiń
3. Spróbuj zasugerować wersję (beta)

Wybierz:
```

**Jeśli 1 (Tak):**
```
Wpisz nazwę tagu (np. v1.0.0 lub instagram-agent-v1):
```

Czekaj na wpisanie → pokaż podsumowanie:
```
📋 Podsumowanie tagu:

Branch: $CURRENT_BRANCH
Commit: [hash]
Tag: [user-input]
Wiadomość: Merge feature $CURRENT_BRANCH do $TARGET

Czy stworzyć tag? (t/n)
```

Jeśli "t":
```bash
git tag -a [tag-name] -m "Merge feature $CURRENT_BRANCH do $TARGET"
git push origin [tag-name]
```

Pokaż:
```
✅ Tag stworzony i pushowany!

Tag: [tag-name]
Lokalizacja: [commit-hash]
```

**Jeśli 2 (Nie):**
```
OK, pominąłem tagowanie. Możesz dodać tag później:
git tag -a [nazwa] -m "opis"
```

**Jeśli 3 (Zasugeruj wersję - BETA):**

```bash
# Sparsuj ostatni tag
LAST_VERSION=$(git describe --tags --abbrev=0 2>/dev/null | sed 's/v//')
# Zaproponuj wersję patch
SUGGESTED=$(echo $LAST_VERSION | awk -F. '{print $1"."$2"."$3+1}')
```

Pokaż:
```
💡 Sugestia wersji:

Ostatnia: $LAST_VERSION
Następna: $SUGGESTED

Opcje:
1. Zaakceptuj: v$SUGGESTED
2. Patch (micro-bump)
3. Minor-bump
4. Custom - wpisz własnie

Wybierz:
```

Jeśli 1: użyj v$SUGGESTED
Jeśli 2: bump micro (+0.0.1)
Jeśli 3: bump minor (+0.1.0)
Jeśli 4: czekaj na wpisanie

Potem ten workflow co wyżej (potwierdzenie → push)

### Krok 9: Pytaj o cleanup
```
Czy chcesz usunąć biurko worktree?
1. Tak - git worktree remove [feature-path]
2. Nie

Wybierz:
```

## WAŻNE:
- Auto-nawigacja do głównego worktree
- Jeśli nie znajdziesz main → powiedz użytkownikowi
- Pokaż wszystkie komendy które wykonujesz
- Jeśli błąd → wyjaśnij co się stało
- **TAGOWANIE**: Pytaj o tag ZARAZ PO merge'u, zanim cleanup
- **PUSH**: Tagi muszą być pushowane do remote (`git push origin [tag-name]`)
