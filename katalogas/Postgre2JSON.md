## PostgreSQL į SQLite konvertavimas
---
###### Patikrinkite, ar aplankas egzistuoja
```
ls -la
```

###### Aktyvuokite (pakeiskite 'venv' į savo aplanko pavadinimą)
```
source venv/bin/activate
```

###### Sukuriame duomenų kopiją JSON formatu
```
python3 manage.py dumpdata --natural-foreign --natural-primary -e contenttypes -e auth.Permission > data.json
```

###### Failo parsisiuntimas į savo kompiuterį
```
scp vartotojas@serverio_ip:/kelias/iki/projekto/data.json E:\Duomenai\Kestys\Desktop\
```
Štai komanda, kurią paleiskite savo Windows terminale (ne SSH viduje), kad parsisiųstumėte failą:
```
scp -P 2222 as@192.168.1.200:/home/as/Skaps_django_project/data.json E:\Duomenai\Kestys\Desktop\CodeAcademy\PythonProject\django_excel\
```

###### Išvalykite/Sukurkite SQLite bazę:
```
del db.sqlite3
```
```
python manage.py migrate
```

###### Įkelkite duomenis:
```
python manage.py loaddata E:\Duomenai\Kestys\Desktop\data.json
```
Kadangi Django krauna duomenis objektas po objekto, bet koks automatinis kodas (signalas), kuris bando daryti užklausas į duomenų bazę krovimo metu, „lūžta“.

**Greičiausias sprendimas: Visiškas signalų atjungimas**

Užuot ieškoję visų funkcijų po vieną, tiesiog laikinai neleiskite Django „pamatyti“ jūsų signalų failo:

1.  **Atsidarykite `skaps/apps.py`** (arba bet kurį kitą `apps.py` failą jūsų aplikacijose).
2.  Raskite `def ready(self):` metodą.
3.  **Užkomentuokite** eilutę, kuri importuoja signalus (pvz., `import skaps.signals`).
    ```python
    def ready(self):
        # import skaps.signals  <-- Užkomentuokite šitą
        pass
    ```
4.  **Ištrinkite nepavykusią bazę ir bandykite iš naujo:**
    ```powershell
    del db.sqlite3
    python manage.py migrate
    python manage.py loaddata data.json
    ```

---

### Kodėl tai vyksta?

Klaidos priežastis yra objektų krovimo tvarka:
1. `loaddata` bando įrašyti `Order` arba `Payment` įrašą.
2. Suveikia `pre_save` signalas.
3. Signalas klausia: „Kas yra šio užsakymo klientas (`instance.customer`)?“.
4. SQLite atsako: „Nežinau, aš dar neturiu jokių klientų įkeltų“.
5. Programa sustoja.



### Kai `loaddata` baigsis sėkmingai:
Nepamirškite **atstatyti** (atkomentuoti) signalų importo `apps.py` faile, nes be jų jūsų svetainė neatliks „snapshot“ funkcijų kurdama naujus užsakymus realiu laiku.

**Patarimas ateičiai:** Visuose savo signaluose pridėkite `if raw: return` patikrą pačioje pradžioje. Tai apsaugos nuo tokių problemų bet kokios migracijos metu.

```python
def set_payment_snapshots(sender, instance, raw, **kwargs):
    if raw:
        return
    # ... visas kitas kodas
```
