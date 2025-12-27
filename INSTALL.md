# Instalacja Claude Worktree Skills

Instrukcja krok po kroku jak zainstalować te komendy globalnie w Claude Code.

## Wymagania

- Claude Code (najnowsza wersja)
- Git zainstalowany
- Repozytorium tego projektu (można sklonować)

## Metoda 1: Automatyczna Instalacja (REKOMENDOWANA)

### Krok 1: Klonuj repozytorium

```bash
cd /tmp
git clone https://github.com/[TW0J_USERNAME]/claude-worktree-skills.git
cd claude-worktree-skills
```

### Krok 2: Uruchom instalator

Będąc w katalogu projektu, otwórz Claude Code i wpisz do prompt'u:

```
Zainstaluj globalnie te komendy worktree do ~/.claude/commands/ i powiedz mi gdy skończysz
```

Claude Code automatycznie:
- Przeczyta pliki z tym katalogu
- Skopiuje je do `~/.claude/commands/`
- Potwierdzi instalację

### Krok 3: Restart Claude Code

Zamknij i otwórz Claude Code. Komendy będą dostępne:

```
/worktree-setup
/worktree-commit
/worktree-merge
/worktree-cleanup
```

---

## Metoda 2: Ręczna Instalacja

Jeśli preferujesz robić to ręcznie:

### Krok 1: Stwórz katalog (jeśli nie istnieje)

```bash
mkdir -p ~/.claude/commands/
```

### Krok 2: Skopiuj pliki

```bash
# Opcja A: Z klonowanego repozytorium
cp claude-worktree-skills/worktree-*.md ~/.claude/commands/

# Opcja B: Pobierz bezpośrednio
curl -O https://raw.githubusercontent.com/[TW0J_USERNAME]/claude-worktree-skills/main/worktree-setup.md
curl -O https://raw.githubusercontent.com/[TW0J_USERNAME]/claude-worktree-skills/main/worktree-commit.md
curl -O https://raw.githubusercontent.com/[TW0J_USERNAME]/claude-worktree-skills/main/worktree-merge.md
curl -O https://raw.githubusercontent.com/[TW0J_USERNAME]/claude-worktree-skills/main/worktree-cleanup.md

mv worktree-*.md ~/.claude/commands/
```

### Krok 3: Weryfikacja

```bash
ls -la ~/.claude/commands/ | grep worktree
```

Powinieneś zobaczyć:
```
worktree-setup.md
worktree-cleanup.md
worktree-commit.md
worktree-merge.md
```

### Krok 4: Restart

Zamknij i otwórz Claude Code.

---

## Metoda 3: Instalacja Lokalna (tylko dla jednego projektu)

Jeśli chcesz mieć komendy tylko w konkretnym projekcie:

```bash
# W folderze twojego projektu:
mkdir -p .claude/commands/
cp /ścieżka/do/claude-worktree-skills/worktree-*.md .claude/commands/
```

Komendy będą dostępne tylko w tym projekcie.

---

## Sprawdzenie czy zainstalowano

Po restarcie Claude Code, wpisz w prompt:

```
/help
```

Powinieneś zobaczyć listę dostępnych komend, w tym:
- `/worktree-setup`
- `/worktree-commit`
- `/worktree-merge`
- `/worktree-cleanup`

---

## Troubleshooting

### Komendy nie pojawiają się po instalacji

1. **Sprawdź czy pliki są w dobrym miejscu:**
   ```bash
   ls -la ~/.claude/commands/
   ```

2. **Upewnij się że to pliki `.md`:**
   ```bash
   file ~/.claude/commands/worktree-setup.md
   # Powinno być: text/plain, UTF-8
   ```

3. **Sprawdź zawartość pierwszych linii:**
   ```bash
   head -5 ~/.claude/commands/worktree-setup.md
   # Powinno zaczynać się od: ---
   # description: ...
   ```

4. **Zamknij i otwórz Claude Code** - to jest konieczne!

### "Permission denied" przy kopowaniu

```bash
# Upewnij się że folder istnieje i masz do niego dostęp:
mkdir -p ~/.claude/commands/
chmod 755 ~/.claude/commands/

# Spróbuj kopować ponownie:
cp worktree-*.md ~/.claude/commands/
```

### Pliki pokazują się ale nie działają

- Pliki mogą być uszkodzone - spróbuj pobrać je ponownie
- Upewnij się że to jest UTF-8 encoding
- Przestart Claude Code

---

## Aktualizacja

Aby zaktualizować do najnowszej wersji:

```bash
# Pobierz najnowsze pliki
cd /tmp
rm -rf claude-worktree-skills
git clone https://github.com/[TW0J_USERNAME]/claude-worktree-skills.git
cd claude-worktree-skills

# Otwórz Claude Code w tym katalogu
# I wpisz: Zaktualizuj komendy worktree w ~/.claude/commands/
```

---

## Usunięcie

Aby usunąć komendy:

```bash
rm ~/.claude/commands/worktree-*.md
```

Zamknij i otwórz Claude Code.

---

## Pytania?

- Sprawdź pełną dokumentację w `README.md`
- Dowiedz się więcej o Claude Code na: https://claude.com/claude-code
- Raportuj problemy na GitHub Issues

---

## Ścieżka instalacji

Komendy Claude Code szukają plików w tych miejscach (w kolejności):

1. `.claude/commands/` - projekt lokalny (najwyższy priorytet)
2. `~/.claude/commands/` - globalne (dla wszystkich projektów)

Jeśli zainstalowałeś zarówno globalnie jak i lokalnie, będą używane wersje lokalne.
