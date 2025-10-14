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


Puiku, dabar aišku. Štai atnaujinta Git komandų atmintinė Markdown formatu, kur visos komandos pateiktos taip, kad būtų lengvai nukopijuojamos — naudojant trijų kabučių bloką (code block) kiekvienai komandai:

# Git komandų atmintinė

## Bazinės komandos

**Sukurti naują Git saugyklą**
```
git init
```

**Klonuoti esamą nuotolinę saugyklą**
```
git clone <url>
```

**Patikrinti būseną (kas pakeista, kas paruošta commit'ui)**
```
git status
```

**Pridėti failą prie commit'o**
```
git add <failas>
```

**Sukurti commit'ą su žinute**
```
git commit -m "žinutė"
```

**Peržiūrėti commit'ų istoriją**
```
git log
```

---

## Darbas su nuotoline saugykla

**Peržiūrėti nuotolinių saugyklų adresus**
```
git remote -v
```

**Išsiųsti pakeitimus į nuotolinę saugyklą**
```
git push
```

**Atsisiųsti ir sujungti naujausius pakeitimus**
```
git pull
```

**Atsisiųsti pakeitimus be sujungimo**
```
git fetch
```

---

## Darbas su šakomis

**Parodyti visas šakas**
```
git branch
```

**Sukurti naują šaką**
```
git branch <pavadinimas>
```

**Persijungti į kitą šaką**
```
git checkout <šaka>
```

**Sujungti šaką su dabartine**
```
git merge <šaka>
```

---

## Konfliktų sprendimas ir valymas

**Peržiūrėti skirtumus tarp versijų**
```
git diff
```

**Pašalinti failą iš staging**
```
git reset <failas>
```

**Laikinai išsaugoti necommit'intus pakeitimus**
```
git stash
```

**Grąžinti pakeitimus iš stash**
```
git stash pop
```

---

## Papildoma konfigūracija

**Nustatyti naudotojo vardą**
```
git config --global user.name "Vardas"
```

**Nustatyti naudotojo el. paštą**
```
git config --global user.email "el.paštas"
```

**Failas ignoruojamiems failams nurodyti**
```
.gitignore
```



# Git komandų atmintinė

## Bazinės komandos

| Komanda | Aprašymas | Kada naudoti |
|--------|-----------|--------------|
| `git init` | Sukuria naują Git saugyklą | Pradedant naują projektą |
| `git clone <url>` | Nukopijuoja esamą nuotolinę saugyklą | Dirbant su jau egzistuojančiu projektu |
| `git status` | Parodo dabartinę būseną | Prieš commit'inant, kad matytum pakeitimus |
| `git add <failas>` | Pažymi failą commit'ui | Įtraukti pakeitimus į būsimos versijos sąrašą |
| `git commit -m "žinutė"` | Išsaugo pakeitimus su aprašymu | Sukuriant versijos tašką |
| `git log` | Rodo commit'ų istoriją | Peržiūrėti kas ir kada buvo pakeista |

## Darbas su nuotoline saugykla

| Komanda | Aprašymas | Kada naudoti |
|--------|-----------|--------------|
| `git remote -v` | Parodo nuotolinių saugyklų adresus | Patikrinti, kur siunčiami pakeitimai |
| `git push` | Išsiunčia vietinius pakeitimus į nuotolinę saugyklą | Bendradarbiaujant su komanda |
| `git pull` | Atsisiunčia ir sujungia naujausius pakeitimus | Gauti naujausią versiją prieš darbą |
| `git fetch` | Atsisiunčia pakeitimus be automatinio sujungimo | Tik peržiūrėti, kas pasikeitė |

## Darbas su šakomis

| Komanda | Aprašymas | Kada naudoti |
|--------|-----------|--------------|
| `git branch` | Parodo esamas šakas | Matyti projekto struktūrą |
| `git branch <pavadinimas>` | Sukuria naują šaką | Pradedant naują funkciją |
| `git checkout <šaka>` | Persijungia į kitą šaką | Dirbti kitoje projekto dalyje |
| `git merge <šaka>` | Sujungia šaką su dabartine | Baigus funkciją, integruoti ją į pagrindą |

## Konfliktų sprendimas ir valymas

| Komanda | Aprašymas | Kada naudoti |
|--------|-----------|--------------|
| `git diff` | Parodo pakeitimus tarp versijų ar failų | Suprasti, kas buvo pakeista |
| `git reset <failas>` | Pašalina failą iš staging | Atšaukti netyčinį pridėjimą |
| `git stash` | Laikinai išsaugo necommit'intus pakeitimus | Pereiti prie kito darbo nepašalinant pakeitimų |
| `git stash pop` | Grąžina pakeitimus iš laikinos saugyklos | Tęsti darbą nuo sustojimo taško |

## Papildoma konfigūracija

| Komanda | Aprašymas |
|--------|-----------|
| `.gitignore` | Failas, kuriame nurodoma, kokius failus Git turi ignoruoti |
| `git config --global user.name "Vardas"` | Nustato naudotojo vardą visose saugyklose |
| `git config --global user.email "el.paštas"` | Nustato naudotojo el. paštą visose saugyklose |
