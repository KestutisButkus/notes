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
