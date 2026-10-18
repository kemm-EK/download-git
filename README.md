# download-git
Use this readme file as an instruction to install Git

# Installation af Git på macOS og Windows

En trinvis vejledning til at installere **Git** på Mac og Windows — med eller uden en pakkehåndtering. Alle kommandoer er beregnet til at kunne kopieres direkte ind i den angivne terminal.

> **Kort fortalt:** På Mac anbefales **Homebrew** eller Apples **Command Line Tools**. På Windows anbefales **WinGet** eller den officielle **Git for Windows**-installer. Homebrew på Windows kræver **WSL (Linux på Windows)** og installerer Git i Linux-miljøet, ikke direkte i Windows.

## Indhold

1. [Tjek, om Git allerede er installeret](#1-tjek-om-git-allerede-er-installeret)
2. [macOS: Git med Homebrew](#2-macos-git-med-homebrew)
3. [macOS: Git uden Homebrew](#3-macos-git-uden-homebrew)
4. [Windows: Git med WinGet](#4-windows-git-med-winget)
5. [Windows: Git uden WinGet](#5-windows-git-uden-winget)
6. [Windows: Homebrew via WSL (valgfrit)](#6-windows-homebrew-via-wsl-valgfrit)
7. [Grundlæggende Git-konfiguration](#7-grundlæggende-git-konfiguration)
8. [Test Git med et lokalt projekt](#8-test-git-med-et-lokalt-projekt)
9. [Opdatering og fejlfinding](#9-opdatering-og-fejlfinding)
10. [Officielle kilder](#10-officielle-kilder)

---

## 1. Tjek, om Git allerede er installeret

Åbn **Terminal** på macOS eller **PowerShell** på Windows, og skriv:

```bash
git --version
```

Hvis du får et versionsnummer, findes der allerede en Git-installation. På macOS kan kommandoen i stedet udløse en dialog om installation af Apples udviklingsværktøjer.

Du kan fortsætte med den installation, du allerede har, eller vælge en af metoderne nedenfor.

## 2. macOS: Git med Homebrew

**Terminal:** macOS Terminal (standard-shell: `zsh`).

Homebrew er en pakkehåndtering til macOS. Du behøver **ikke** installere hele Xcode for at bruge Git.

### Trin 1: Installer eventuelt Apples Command Line Tools

Dette er en nyttig forudsætning og kan være nødvendig afhængigt af Mac og installation. Hvis de allerede findes, får du besked.

```bash
xcode-select --install
```

Følg installationsdialogen, hvis den vises. **Command Line Tools er ikke det samme som hele Xcode.**

### Trin 2: Installer Homebrew

Brug Homebrews officielle installationskommando:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Læs installationsprogrammets beskeder, og følg især afsnittet **Next steps** til sidst.

### Trin 3: Tilføj Homebrew til PATH

Vælg **kun én** af de følgende to kommando-grupper, afhængigt af din Mac. Tjek først processorarkitekturen:

```bash
uname -m
```

- `arm64` = Apple Silicon (M1, M2, M3 osv.)
- `x86_64` = Intel Mac

**Apple Silicon** (standardplacering `/opt/homebrew`):

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

**Intel Mac** (standardplacering `/usr/local`):

```bash
echo 'eval "$(/usr/local/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/usr/local/bin/brew shellenv)"
```

> Tilføj kun disse linjer, hvis Homebrew ikke allerede er korrekt sat op i `~/.zprofile`. Ved en anden installationsplacering eller shell skal du følge de konkrete instruktioner under **Next steps** i stedet.

Kontrollér:

```bash
brew --version
which brew
```

### Trin 4: Installer Git

```bash
brew install git
```

Kontrollér installationen:

```bash
git --version
which git
```

Typiske Git-stier er `/opt/homebrew/bin/git` (Apple Silicon) og `/usr/local/bin/git` (Intel). Hvis `which git` stadig viser `/usr/bin/git`, bruges Apples Git i den aktuelle shell; se [fejlfinding](#9-opdatering-og-fejlfinding).

## 3. macOS: Git uden Homebrew

**Terminal:** macOS Terminal.

Den enkleste løsning uden Homebrew er Apples **Xcode Command Line Tools**, som blandt andet leverer Git.

```bash
xcode-select --install
```

Godkend installationen, hvis du bliver spurgt. Hvis værktøjerne allerede er installeret, kan macOS give dig besked om det.

Kontrollér derefter:

```bash
git --version
which git
```

Git findes typisk gennem `/usr/bin/git`. Denne metode kræver **ikke** hele Xcode-applikationen.

Alternativt kan du hente Git fra [git-scm.com/downloads/mac](https://git-scm.com/downloads/mac), men de tilgængelige installationsmetoder kan ændre sig.

## 4. Windows: Git med WinGet

**Terminal:** PowerShell eller Windows Terminal med en PowerShell-fane — **ikke** macOS Terminal eller Git Bash.

WinGet er Windows' pakkehåndtering og er det nærmeste almindelige Windows-alternativ til Homebrew.

Tjek, at WinGet findes:

```powershell
winget --version
```

Installer Git for Windows:

```powershell
winget install --id Git.Git -e --source winget
```

Følg eventuelle installationsdialoger, og **luk og genåbn terminalen** bagefter.

Kontrollér:

```powershell
git --version
Get-Command git
```

Git for Windows installerer normalt også **Git Bash**, som er en terminal med mange Unix-lignende kommandoer.

> Hvis `winget` ikke genkendes, kan du prøve at opdatere/installere **App Installer** via Microsoft Store eller bruge metoden i næste afsnit.

## 5. Windows: Git uden WinGet

Denne metode bruger den officielle grafiske installation og kræver ikke en pakkehåndtering.

1. Gå til **[git-scm.com/downloads/win](https://git-scm.com/downloads/win)**.
2. Download den relevante **Git for Windows**-installer (normalt x64; vælg ARM64 til en kompatibel ARM-baseret Windows-pc).
3. Kør installationsprogrammet.
4. Hvis du får valget om `PATH`, er **Git from the command line and also from 3rd-party software** normalt et fornuftigt valg. Andre standardindstillinger er typisk fine.
5. Åbn en **ny** PowerShell eller Git Bash.

Kontrollér installationen i PowerShell:

```powershell
git --version
Get-Command git
```

Hvis kommandoen ikke findes i PowerShell, så prøv Git Bash, og undersøg derefter PATH eller genstart terminalen.

## 6. Windows: Homebrew via WSL (valgfrit)

**Denne metode er kun relevant, hvis du vil arbejde i Linux på Windows.** Homebrew kører her *inde i WSL* og installerer en **Linux-version af Git**. Den erstatter ikke en Windows-installation af Git i PowerShell.

### Trin 1: Installer WSL med Ubuntu

Åbn **PowerShell som administrator**:

```powershell
wsl --install -d Ubuntu
```

Genstart Windows, hvis du bliver bedt om det, og gennemfør oprettelsen af din Linux-bruger. Åbn derefter **Ubuntu** (WSL). Resten af dette afsnit køres **inde i Ubuntu-terminalen**, ikke i PowerShell.

### Trin 2: Installer nødvendige Linux-pakker

```bash
sudo apt update
sudo apt install -y build-essential procps curl file git
```

### Trin 3: Installer Homebrew i WSL

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Tilføj Homebrew til PATH for Ubuntus almindelige Bash-shell (ved standardinstallationsplacering):

```bash
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.bashrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

Kontrollér og installer Git med Homebrew:

```bash
brew --version
brew install git
git --version
which git
```

> Ubuntu har allerede fået installeret Git i trin 2. `brew install git` tilføjer en Homebrew-administreret Git-version, hvis du specifikt ønsker det. Til almindelig Git-brug i WSL er Ubuntus `apt`-version ofte tilstrækkelig.

## 7. Grundlæggende Git-konfiguration

Disse kommandoer virker i **macOS Terminal, Windows PowerShell, Git Bash og WSL** efter installationen.

Git knytter navn og e-mail til dine commits. **Erstat eksempelværdierne** med dine egne oplysninger:

```bash
git config --global user.name "Dit Navn"
git config --global user.email "din-email@example.com"
```

Vælg `main` som standardnavn for nye branches:

```bash
git config --global init.defaultBranch main
```

Kontrollér indstillingerne:

```bash
git config --global --list
```

Du kan kontrollere navn og e-mail hver for sig:

```bash
git config --global user.name
git config --global user.email
```

> Din Git-e-mail behøver ikke være offentligt synlig. Hvis du bruger GitHub, kan du vælge en GitHub-tilknyttet `noreply`-adresse. Git-konfiguration er **ikke** det samme som login til GitHub; adgang til private repositories og `push` kræver separat godkendelse (fx browser-login via Git Credential Manager eller SSH).

## 8. Test Git med et lokalt projekt

Alle kommandoerne herunder fungerer i macOS Terminal, Git Bash og WSL. De fungerer også i nyere PowerShell.

Opret en mappe, og gå ind i den:

```bash
mkdir git-test
cd git-test
```

Initialiser et Git-repository:

```bash
git init
```

Opret en testfil direkte fra terminalen, og kontrollér Git-status:

```bash
echo "# Mit første Git-projekt" > README.md
git status
```

Gem den første ændring:

```bash
git add README.md
git commit -m "Første commit"
git status
```

Hvis `git commit` gennemføres, fungerer den lokale Git-installation.

## 9. Opdatering og fejlfinding

### Opdater Git installeret med Homebrew (Mac eller WSL)

```bash
brew update
brew upgrade git
```

### Opdater Git installeret med WinGet (Windows PowerShell)

```powershell
winget upgrade --id Git.Git -e --source winget
```

### Opdater Git installeret med Ubuntu APT (WSL)

```bash
sudo apt update
sudo apt install --only-upgrade git
```

### Hvilken Git-version bruger terminalen?

På macOS / Git Bash / WSL:

```bash
which git
git --version
```

På Windows PowerShell:

```powershell
Get-Command git
git --version
```

### Homebrew: `command not found: brew`

Homebrew er muligvis ikke blevet tilføjet til din `PATH`. På macOS skal du kontrollere kommandoerne under [trin 3](#trin-3-tilføj-homebrew-til-path). Luk og genåbn derefter Terminal.

På WSL skal du kontrollere, at du har kørt `brew shellenv` i **Ubuntu**, og at installationsstien stemmer med instruktionerne fra Homebrew.

### macOS bruger stadig Apples Git efter installation via Homebrew

```bash
which git
```

Viser den `/usr/bin/git`, kan du kontrollere, om Homebrew-mappen står foran `/usr/bin` i din PATH:

```bash
printf '%s\n' "$PATH"
```

Kontrollér derefter Homebrew:

```bash
brew --prefix
brew list git
```

Følg eventuelt igen Homebrews `shellenv`-anvisning for din Mac, og åbn en ny Terminal.

### Windows: `git` genkendes ikke

Luk alle PowerShell-/Terminal-vinduer, og åbn et nyt. Hvis Git stadig ikke genkendes, så undersøg PATH i Windows. Ved en standardinstallation ligger Git ofte under `C:\Program Files\Git\cmd`. Undlad at overskrive hele PATH-variablen med en enkelt mappe.

### Git er installeret, men GitHub kræver login

Det er normalt. Git er versionsstyringsprogrammet, mens GitHub er en onlinetjeneste til hosting af Git-repositories. Installation af Git logger dig ikke automatisk ind på GitHub.

---

## 10. Officielle kilder

- [Git — downloads og installation](https://git-scm.com/downloads)
- [Git for Windows](https://git-scm.com/downloads/win)
- [Homebrew — installation](https://docs.brew.sh/Installation)
- [Homebrew — officielt installationsscript](https://github.com/Homebrew/install)
- [Microsoft — Install WSL](https://learn.microsoft.com/windows/wsl/install)
- [Microsoft — WinGet](https://learn.microsoft.com/windows/package-manager/winget/)
- [Git — første konfiguration](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

*Vejledningen er udarbejdet oktober 2026. Installationsprogrammer og systemkrav ændrer sig over tid; brug de officielle kilder ved afvigelser.*
