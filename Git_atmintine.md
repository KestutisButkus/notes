# Git komandų atmintinė

## Bazinės komandos

| Komanda | Aprašymas | Naudojimo paskirtis |
|--------|-----------|---------------------|
| `"git init"` | Sukuria naują Git saugyklą | Pradedant naują projektą |
| `"git clone <url>"` | Nukopijuoja esamą nuotolinę saugyklą | Dirbant su jau egzistuojančiu projektu |
| `"git status"` | Parodo dabartinę saugyklos būseną | Prieš commit'inant, kad matytum pakeitimus |
| `"git add <failas>"` | Pažymi failą commit'ui | Įtraukti pakeitimus į būsimos versijos sąrašą |
| `"git commit -m \"žinutė\""` | Išsaugo pakeitimus su aprašymu | Sukuriant versijos tašką |
| `"git log"` | Rodo commit'ų istoriją | Peržiūrėti kas ir kada buvo pakeista |

## Darbas su nuotoline saugykla

| Komanda | Aprašymas | Naudojimo paskirtis |
|--------|-----------|---------------------|
| `"git remote -v"` | Parodo nuotolinių saugyklų adresus | Patikrinti, kur siunčiami pakeitimai |
| `"git push"` | Išsiunčia vietinius pakeitimus į nuotolinę saugyklą | Bendradarbiaujant su komanda |
| `"git pull"` | Atsisiunčia ir sujungia naujausius pakeitimus | Gauti naujausią versiją prieš darbą |
| `"git fetch"` | Atsisiunčia pakeitimus be automatinio sujungimo | Tik peržiūrėti, kas pasikeitė |

## Darbas su šakomis

| Komanda | Aprašymas | Naudojimo paskirtis |
|--------|-----------|---------------------|
| `"git branch"` | Parodo esamas šakas | Matyti projekto struktūrą |
| `"git branch <pavadinimas>"` | Sukuria naują šaką | Pradedant naują funkciją |
| `"git checkout <šaka>"` | Persijungia į kitą šaką | Dirbti kitoje projekto dalyje |
| `"git merge <šaka>"` | Sujungia šaką su dabartine | Baigus funkciją, integruoti ją į pagrindą |

## Konfliktų sprendimas ir valymas

| Komanda | Aprašymas | Naudojimo paskirtis |
|--------|-----------|---------------------|
| `"git diff"` | Parodo pakeitimus tarp versijų ar failų | Suprasti, kas buvo pakeista |
| `"git reset <failas>"` | Pašalina failą iš staging | Atšaukti netyčinį pridėjimą |
| `"git stash"` | Laikinai išsaugo necommit'intus pakeitimus | Pereiti prie kito darbo nepašalinant pakeitimų |
| `"git stash pop"` | Grąžina pakeitimus iš laikinos saugyklos | Tęsti darbą nuo sustojimo taško |

## Papildoma konfigūracija

| Komanda | Aprašymas |
|--------|-----------|
| `".gitignore"` | Failas, kuriame nurodoma, kokius failus Git turi ignoruoti |
| `"git config --global user.name \"Vardas\""` | Nustato naudotojo vardą visose saugyklose |
| `"git config --global user.email \"el.paštas\""` | Nustato naudotojo el. paštą visose saugyklose |
