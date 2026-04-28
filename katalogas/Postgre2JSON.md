## PostgreSQL į SQLite konvertavimas
---
### Patikrinkite, ar aplankas egzistuoja
```
ls -la
```

### Aktyvuokite (pakeiskite 'venv' į savo aplanko pavadinimą)
```
source venv/bin/activate
```

### Sukuriame duomenų kopiją JSON formatu
```
python3 manage.py dumpdata --natural-foreign --natural-primary -e contenttypes -e auth.Permission > data.json
```

### Failo parsisiuntimas į savo kompiuterį
```
scp vartotojas@serverio_ip:/kelias/iki/projekto/data.json E:\Duomenai\Kestys\Desktop\
```
Štai komanda, kurią paleiskite savo Windows terminale (ne SSH viduje), kad parsisiųstumėte failą:
```
scp -P 2222 as@192.168.1.200:/home/as/Skaps_django_project/data.json E:\Duomenai\Kestys\Desktop\CodeAcademy\PythonProject\django_excel\
```

### Išvalykite/Sukurkite SQLite bazę:
```
del db.sqlite3
```
```
python manage.py migrate
```

### Įkelkite duomenis:
```
python manage.py loaddata E:\Duomenai\Kestys\Desktop\data.json
```
