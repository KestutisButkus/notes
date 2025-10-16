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
