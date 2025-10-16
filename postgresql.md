# 🧠 PostgreSQL duomenų bazės atmintinė

## 🧠 PostgreSQL prisijungimas ir pagrindinės komandos

### 🔹 Prisijungimas prie PostgreSQL konsolės

```bash
sudo -u postgres psql
```

**Paaiškinimas:**

* `sudo` — paleidžia komandą su administratoriaus teisėmis.
* `-u postgres` — nurodo, kad komanda vykdoma **postgres** sistemos vartotojo vardu (tai numatytasis PostgreSQL administratorius).
* `psql` — PostgreSQL konsolės (interaktyvaus terminalo) paleidimo komanda.

💡 Prisijungęs matai eilutę panašią į:

```
postgres=#
```

— tai reiškia, kad esi PostgreSQL aplinkoje.

---

### 🔹 Išėjimas iš PostgreSQL konsolės

```sql
\q
```

**Paaiškinimas:**

* `\q` — „quit“ — išeina iš `psql` konsolės atgal į Linux terminalą.

---

### 🔹 Rodyti visas duomenų bazes

```sql
\l
```

**Paaiškinimas:**

* `\l` arba `\list` — rodo **visų PostgreSQL duomenų bazių sąrašą**, jų savininkus, šifravimą ir teises.

---

### 🔹 Prisijungti prie konkrečios duomenų bazės

```bash
sudo -u postgres psql -d skaps_django_project
```

**Paaiškinimas:**

* `-d skaps_django_project` — nurodo, prie kurios **duomenų bazės** jungiamasi.
* Prisijungus, prompt’as pasikeis į `skaps_django_project=#`.

---

### 🔹 Rodyti lenteles prisijungus prie DB

```sql
\dt
```

**Paaiškinimas:**

* `\dt` — rodo visas lenteles esamoje duomenų bazėje (tik tas, kurias mato dabartinis vartotojas).


# Duomenų bazės (PostgreSQL) atsarginė kopija
Skriptas:
Sukuriame failą, pvz., /home/as/backup_pg.sh:
```
#!/bin/bash

# Nustatymai
DB_NAME="*"
DB_USER="*"
DB_HOST="localhost"
DB_PASSWORD="*"
BACKUP_DIR="/home/as/db_backups"
LOG_FILE="/home/as/backup_log.txt"
DATE=$(date +"%Y-%m-%d_%H-%M-%S")
BACKUP_FILE="${BACKUP_DIR}/${DB_NAME}_${DATE}.sql"

# Sukuriam katalogą jei neegzistuoja
mkdir -p "$BACKUP_DIR"

# Atliekam dump
export PGPASSWORD="$DB_PASSWORD"
if pg_dump -U "$DB_USER" -h "$DB_HOST" "$DB_NAME" > "$BACKUP_FILE"; then
    echo "$(date) – :) Backup sėkmingai sukurta: $BACKUP_FILE" >> "$LOG_FILE"
else
    echo "$(date) – !!! Klaida kuriant backup: $BACKUP_FILE" >> "$LOG_FILE"
fi
unset PGPASSWORD

# Paliekam tik paskutines 30 dienų kopijas
DAYS_TO_KEEP=30
find "$BACKUP_DIR" -type f -name "*.sql" -mtime +"$DAYS_TO_KEEP" -exec rm {} \;
```
# Padarom skriptą vykdomu:
```
chmod +x /home/as/backup_pg.sh
```
# arba
```
sudo chmod +x /home/as/backup_pg.sh
```
# Patikrinimas
```
/home/as/backup_pg.sh
```
Tada patikrink /home/as/db_backups/ ar atsirado failas su dabartine data.
---
# Sukuriame cron job
Kaip patikrinti, ar  yra įdiegtas
```
dpkg -l | grep cron
```
Jei nieko negrąžina —  nėra įdiegtas.
🛠️ Kaip įdiegti  ir 
```
sudo apt update
sudo apt install cron
```
Po to paleisk ir aktyvuok servisą:
```
sudo systemctl enable cron
sudo systemctl start cron
```
Ir tada  turėtų veikti be problemų.

Redaguok cron užduotis naudodamas crontab -e:
```
0 4 * * * /home/as/backup_pg.sh
```
Tai reiškia: kasdien 04:00 vykdyti skriptą.

Galima nukreipti output į log failą:
```
0 4 * * * /home/as/backup_pg.sh >> /home/as/db_backups/backup.log 2>&1
```
#### Arba galoma naudoti Ubuntu inregruotą "systemd":
`systemd timers`, yra modernesni ir lankstesni nei `cron`. Sukurti du failus, kad skriptas `/home/as/backup_pg.sh` būtų paleidžiamas **kasdien 4:00 ryte**:

>📁 1. Sukuriame `backup_pg.service`

```bash
sudo nano /etc/systemd/system/backup_pg.service
```

Įklijuojame:

```ini
[Unit]
Description=PostgreSQL atsarginės kopijos skriptas

[Service]
Type=oneshot
ExecStart=/home/as/backup_pg.sh
```

> ✅ `Type=oneshot` reiškia, kad skriptas bus paleistas vieną kartą ir baigsis.

>⏰ 2. Sukuriame `backup_pg.timer`

```bash
sudo nano /etc/systemd/system/backup_pg.timer
```

Įklijuojame:

```ini
[Unit]
Description=Kasdien 4:00 paleisti PostgreSQL atsarginę kopiją

[Timer]
OnCalendar=*-*-* 04:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

> ✅ `Persistent=true` reiškia, kad jei kompiuteris buvo išjungtas 4:00, skriptas bus paleistas kitą kartą įjungus.

>🚀 3. Aktyvuok laikmatį

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable --now backup_pg.timer
```
>🔍 4. Patikrink statusą

```bash
systemctl list-timers --all | grep backup_pg
systemctl status backup_pg.timer
journalctl -u backup_pg.service
```
---

### 💡 Papildoma rekomendacija:
Jei duomenų bazė didelė, galima naudoti pg_dump -Fc formatu (custom dump), tada restore daroma su pg_restore.
Jei serveris veikia su Docker ar kitomis aplinkomis, reikia atitinkamai koreguoti komandas.

## – skripte, kuris daro atsarginę kopiją, reikia naudoti tuos pačius DB autentifikacijos duomenis, kurie veikia tavo Django projekte, t. y.:

1. Vartotojo vardą (DB_USER) – tas pats, kurį naudoja Django settings.py DATABASES['default']['USER'].

2. Slaptažodį (PGPASSWORD) – tas pats, kaip DATABASES['default']['PASSWORD'].

3. Duomenų bazės pavadinimą (DB_NAME) – tas pats kaip settings.py NAME.

4. Hostas (DB_HOST) – dažniausiai 'localhost' arba '127.0.0.1', priklausomai nuo tavo PostgreSQL konfigūracijos.

5. Portas (DB_PORT) – jei nenaudoji standartinio 5432, reikia nurodyti.

###### 💡 Taip užtikrinsi, kad skriptas turės teises jungtis prie DB ir atlikti pg_dump.

### Alternatyvos, kad nereikėtų slaptažodžio rašyti skripte:

.pgpass failas – PostgreSQL leidžia sukurti failą ~/.pgpass, kuriame saugomas vartotojas, hostas, portas, DB ir slaptažodis. Tuomet pg_dump nereikalauja PGPASSWORD kintamojo. Formatas:
```
hostname:port:database:username:password
```

Failo leidimai turi būti griežti: chmod 600 ~/.pgpass

Tai dažnai laikoma saugesniu nei slaptažodžio rašymas skripte.

Jei nori, galiu parašyti pilną pg_dump skriptą su .sql.gz suspaudimu ir naudojant .pgpass, kuris veiktų saugiai serverio cron.

Puiku — labai gera idėja turėti **aiškią PostgreSQL atmintinę (cheat sheet)**, ypač kai dirbi su produkcine baze.
Žemiau pateikiu **pilną, aiškiai sugrupuotą atmintinę** su **komandomis, komentarais, pavyzdžiais ir paaiškinimais**.

---

## 🏗️ STRUKTŪROS ATKŪRIMAS (be duomenų)

Jei reikia **atkurti tik lentelių struktūrą, be duomenų**:

### Sukurti atsarginę kopiją tik struktūros:

```bash
sudo -u postgres pg_dump -s skaps_django_project > db_structure_only.sql
```

*(parametras `-s` reiškia „schema only“)*

### Atkurti tik struktūrą:

```bash
sudo -u postgres psql -d skaps_django_project < db_structure_only.sql
```

👉 Šis variantas **nepakeičia duomenų**, tik perrašo lentelių, indeksų, ryšių struktūrą.

---

## ➕ PAPILDYMAS NAUJAIS DUOMENIMIS (append only)

Jei nori **papildyti duomenų bazę**, bet **neperrašyti esamų įrašų**:

### Sukurk dump su duomenimis (be DROP/CREATE):

```bash
sudo -u postgres pg_dump --data-only --inserts skaps_django_project > data_only.sql
```

### Importuok papildomus duomenis:

```bash
sudo -u postgres psql -d skaps_django_project < data_only.sql
```

🟡 **Pastaba:** ši komanda *prideda* įrašus, bet **jei pirminiai raktai sutampa** – įvyks klaidos („duplicate key value violates unique constraint“).
Tam atvejui naudok:

```bash
sudo -u postgres psql -v ON_ERROR_STOP=1 -d skaps_django_project < data_only.sql
```

kad sustabdytų importą ties klaida (saugiau).

---

## 💣 PILNAS ATKŪRIMAS (perrašyti viską)

Kai reikia **pilnai atstatyti duomenų bazę iš dump failo** (pvz., po gedimo ar perkėlimo):

### 1. Sustabdyk Django servisą:

```bash
sudo systemctl stop skaps.service
```

### 2. Ištrink seną DB:

```bash
sudo -u postgres dropdb skaps_django_project
```

### 3. Sukurk naują:

```bash
sudo -u postgres createdb skaps_django_project
```

### 4. Importuok pilną dump:

```bash
sudo -u postgres psql -d skaps_django_project < /home/as/db_backups/skaps_django_project_2025-10-14_04-00-15.sql
```

### 5. Paleisk servisą atgal:

```bash
sudo systemctl start skaps.service
```

---


## 🧩 NAUDINGOS PATIKRINIMO KOMANDOS

*(visos vykdomos per `psql` arba per `sudo -u postgres psql -c "..."` iš terminalo)*

---

### 🔹 Rodyti duomenų bazių sąrašą

```bash
sudo -u postgres psql -c "\l"
```

**Paaiškinimas:**

* `-c` — leidžia perduoti vieną komandą be įėjimo į interaktyvią konsolę.
* `"\l"` — SQL komanda, kurią paleidžia `psql` (rodo duomenų bazes).

---

### 🔹 Rodyti lenteles konkrečioje duomenų bazėje

```bash
sudo -u postgres psql -d skaps_django_project -c "\dt"
```

**Paaiškinimas:**

* `-d` — nurodo duomenų bazės pavadinimą.
* `\dt` — „display tables“ – išveda lentelių sąrašą.

---

### 🔹 Patikrinti įrašų skaičių lentelėje

```bash
sudo -u postgres psql -d skaps_django_project -c "SELECT COUNT(*) FROM auth_user;"
```

**Paaiškinimas:**

* `SELECT COUNT(*)` — SQL užklausa, kuri suskaičiuoja visus įrašus lentelėje.
* `FROM auth_user` — lentelės pavadinimas (čia: Django vartotojų lentelė).
* `-c` leidžia vykdyti užklausą be atskiro prisijungimo.

---

### 🔹 Patikrinti duomenų bazės dydį

```bash
sudo -u postgres psql -c "SELECT pg_size_pretty(pg_database_size('skaps_django_project'));"
```

**Paaiškinimas:**

* `pg_database_size()` — funkcija, skaičiuojanti konkrečios DB dydį baitais.
* `pg_size_pretty()` — paverčia dydį į žmoniškai skaitomą formatą (MB, GB).

---

### 🔹 Patikrinti aktyvius prisijungimus prie PostgreSQL

```bash
sudo -u postgres psql -c "SELECT datname, usename, client_addr, backend_start FROM pg_stat_activity;"
```

**Paaiškinimas:**

* `pg_stat_activity` — sistemos lentelė, rodanti aktyvias sesijas.
* `datname` — duomenų bazės pavadinimas.
* `usename` — vartotojo vardas.
* `client_addr` — kliento IP adresas.
* `backend_start` — kada prisijungimas buvo pradėtas.

---

### 🔹 Patikrinti PostgreSQL versiją

```bash
psql --version
```

arba

```bash
sudo -u postgres psql -c "SELECT version();"
```

**Paaiškinimas:**

* `--version` — parodo įdiegtos `psql` kliento versiją.
* `SELECT version();` — SQL funkcija, grąžinanti tiek serverio, tiek kliento PostgreSQL versiją.

---

### 🔹 Rodyti aktyvius procesus / jungtis (naudinga diagnostikai)

```bash
sudo -u postgres psql -c "SELECT pid, datname, usename, client_addr, state, query FROM pg_stat_activity WHERE state != 'idle';"
```

**Paaiškinimas:**

* `pid` — proceso ID.
* `query` — paskutinė vykdoma SQL užklausa.
* `state != 'idle'` — filtruoja tik aktyvias jungtis.

---

### 🔹 Patikrinti, kiek vietos užima visos duomenų bazės

```bash
sudo -u postgres psql -c "SELECT datname, pg_size_pretty(pg_database_size(datname)) FROM pg_database;"
```

**Paaiškinimas:**

* `pg_database_size(datname)` — paskaičiuoja kiekvienos bazės dydį.
* `pg_size_pretty()` — konvertuoja į suprantamą formatą (MB/GB).
* Naudinga matant, kuri DB daugiausiai užima vietos.

---

### 🔹 Rodyti vartotojų sąrašą

```bash
sudo -u postgres psql -c "\du"
```

**Paaiškinimas:**

* `\du` — rodo visus PostgreSQL vartotojus (roles) ir jų teises (pvz., `Superuser`, `Login`, `Create DB`).

---

Ar norėtum, kad šią atnaujintą dalį integruočiau į visą ankstesnę **pilną PostgreSQL atmintinę**, kad gautum vieną vientisą dokumentą (pvz. Markdown arba PDF formatu)?
Tuomet galėčiau įtraukti spalvotą žymėjimą, skyrių numeraciją ir aiškiai atskirtas „struktūros“, „duomenų“ ir „pilno atkūrimo“ sekcijas.

