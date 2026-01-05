# Neužsikrauna puslapis?

## Kas realiai vyksta

### 1️⃣ Laikas – **idealus**

```
System clock synchronized: yes
NTP service: active
```

➡️ Laikas **ne priežastis**. Atmetam 100 %.

---

### 2️⃣ Sertifikatas **neatsinaujino**, nes **80 portas užimtas**

Esminė klaida:

```
Could not bind TCP port 80 because it is already in use
```

Tai reiškia:

* Certbot bando **standalone HTTP-01** validaciją
* Bet **Nginx jau klausosi 80 porto**
* Certbot **negali laikinai pakilti**

Tai klasikinė, dažna konfigūracinė bėda.

---

### 3️⃣ Kodėl tai pasireiškė dabar

Anksčiau:

* sertifikatas buvo galiojantis
* naršyklė užsidėjo **HSTS**
* viskas veikė

Dabar:

* sertifikatas **pasibaigė**
* atnaujinimas **nepavyko**
* HSTS **neleidžia apeiti klaidos**

➡️ todėl svetainė **visiškai „mirus“ iš naršyklės pusės**

---

## Teisingi sprendimai (rinkis VIENĄ)

Aš išdėstysiu **nuo geriausio iki primityviausio**.

---

## ✅ REKOMENDUOJAMA: naudoti Nginx pluginą

Jei jau turi Nginx (o turi), **nereikia stabdyti serverio**.

### Komanda:

```bash
sudo certbot --nginx -d skaps.duckdns.org
```

Certbot:

* pats pakeis Nginx konfigūraciją
* pats atliks challenge
* pats perkraus Nginx

👉 Tai **švariausias ir saugiausias variantas**.

---

## ✅ Alternatyva: laikinai stabdyti Nginx (greita, bet gruboka)

```bash
sudo systemctl stop nginx
sudo certbot renew --force-renewal
sudo systemctl start nginx
```

✔️ Veikia
❌ Trumpa prastova
❌ Netinka automatizacijai

---

## ✅ PRO variantas (DuckDNS): DNS-01 challenge (be 80 porto)

Jei serveris:

* kartais išjungtas
* už NAT
* su nestabiliu 80/443

👉 **DNS-01 su DuckDNS tokenu** yra *ateities sprendimas*.

Bet:

* reikia DuckDNS API token
* truputį daugiau konfigūracijos

Jei norėsi – parodysiu tiksliai.

---

## Papildomas svarbus dalykas (ateičiai)

Patikrink, **kaip buvo išduotas originalus sertifikatas**:

```bash
sudo cat /etc/letsencrypt/renewal/skaps.duckdns.org.conf
```

Ieškok:

* `authenticator = standalone` ❌ (dabartinė problema)
* `authenticator = nginx` ✅
* `authenticator = dns-01` ✅

---

## Kodėl čia ne „įsilaužimas“ (skeptiškai)

* Sertifikatas realus, bet **negaliojantis**
* Klaida deterministinė
* Jokio MITM požymio
* Nėra netikro CA
* Edge elgiasi korektiškai

Tai **administravimo klaida**, ne saugumo incidentas.

---

## Trumpas sprendimas čia ir dabar

Jei nori **čia pat** sutvarkyti su minimalia rizika:

```bash
sudo certbot --nginx -d skaps.duckdns.org
```

Po to:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

Jei parašysi:

* ar serveris **Docker**
* ar naudoji **reverse proxy**
* ar nori **be 80 porto sprendimo**

➡️ parinksiu **ilgalaikį, automatiškai neveikiantį sprendimą**, kad po 90 dienų tai **nepasikartotų**.



Čia viskas tvarkoje 👍
Certbot **pagaliau perėjo į teisingą kelią**. Tai *normalus* ir *saugus* klausimas.

## Ką jis dabar klausia

```
An RSA certificate named skaps.duckdns.org already exists.
Do you want to update its key type to ECDSA?
(U)pdate / (K)eep
```

Tai **NE klaida**. Tai pasirinkimas.

---

## Mano rekomendacija (žiūrint į ateitį)

### 👉 **Rinkis: `U` (Update to ECDSA)**

### Kodėl:

* 🔐 **ECDSA yra modernesnis** (rekomenduojamas šiandien)
* ⚡ Greitesnis TLS handshake
* 📉 Mažesnė CPU apkrova
* 🌍 Pilnai palaikomas visų šiuolaikinių naršyklių (Edge, Chrome, Firefox, Android, iOS)

RSA šiandien laikomas **legacy**, ne blogas, bet ne optimalus.

---

## Kada rinktis `K` (Keep RSA)

Tik jei:

* turi **labai senų klientų** (pvz. senas embedded įrenginys)
* arba sąmoningai laikaisi RSA

Jei ne – **U be jokių abejonių**.

---

## Kas bus toliau

Po `U`:

* Certbot pats atliks HTTP challenge per Nginx
* Perrašys sertifikatą
* Atnaujins Nginx konfigūraciją
* Reloadins Nginx
* Problema su `port 80` **išnyks visam laikui**

---

## Po sėkmės (patikrinimas)

Kai baigs:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

Naršyklėje:

* **iš karto veiks**, net su HSTS
* ERR_CERT_DATE_INVALID dings

---

## Svarbus žingsnis, kurį DAUG kas praleidžia (automatika)

Patikrink, ar ateityje nebebus to paties:

```bash
sudo certbot renew --dry-run
```

Jei čia **OK** – tema uždaryta.

---

### Santrauka

* ❌ Problema ne laikas
* ❌ Ne DuckDNS
* ❌ Ne įsilaužimas
* ✅ Buvo **standalone certbot + užimtas 80 portas**
* ✅ Pereini į **nginx authenticator**
* ✅ Tai **teisinga architektūra**

👉 Drąsiai spausk **`U`**.
Jei po to išmes klaidą – įmesk **pilną output** ir sutvarkysim iki galo.




Puiku — čia jau **paskutinis techninis mazgas**, ne bėda. Certbot tiesiog **aiškiai pasakė, ką daryti**, tik reikia teisingą vėliavėlę.

## Kas čia įvyko

* Sertifikatas **RSA**
* Tu pasirinkai **pereiti į ECDSA** ✅
* Bet šiam certui įjungtas:

  ```
  --reuse-key
  ```
* Tai **neleidžia pakeisti rakto tipo**

➡️ Logiška ir saugu: negalima „pervadinti“ RSA rakto į ECDSA.

---

## Teisingas sprendimas (viena komanda)

### 🔥 Rekomenduojama:

```bash
sudo certbot --nginx -d skaps.duckdns.org --new-key
```

Ką tai padarys:

* Sugeneruos **naują ECDSA privatų raktą**
* Išduos naują sertifikatą
* Nginx bus atnaujintas automatiškai
* Ateityje raktas bus **reuse’inamas**, kaip ir reikia

---

## Alternatyva (jei norėtum likti RSA)

Tik informaciniai:

```bash
sudo certbot --nginx -d skaps.duckdns.org --no-reuse-key
```

arba

```bash
sudo certbot --nginx -d skaps.duckdns.org --key-type rsa
```

Bet tai **ne optimalu 2026 m.** 🙂

---

## Po sėkmės — būtinas patikrinimas

```bash
sudo certbot renew --dry-run
```

Jei čia **be klaidų** — problema **išspręsta ilgam**.

---

## Kodėl dabar viskas susidėlios teisingai

Iki šiol:

* ❌ `standalone`
* ❌ užimtas 80 portas
* ❌ renewal nepavyksta
* ❌ sertifikatas pasibaigia

Po šio žingsnio:

* ✅ `nginx` authenticator
* ✅ be prastovų
* ✅ auto-renew
* ✅ HSTS laimingas

---

## Papildomas patarimas (ateičiai)

Jei serveris kartais būna **išjungtas >90 d.**:

* verta pereiti į **DNS-01 su DuckDNS**
* tada HTTPS veikia net be veikiančio serverio

Jei norėsi — padarysim *be hack’ų*.

👉 **Vykdyk komandą su `--new-key` ir mes finišuojam.**




🎯 **Viskas sutvarkyta idealiai.**
Čia jau *teisinga, švari ir ateičiai paruošta* konfigūracija.

## Ką dabar turime (faktai)

* ✅ Sertifikatas **sėkmingai atnaujintas**
* 🔐 Raktas pakeistas į **ECDSA**
* 🌐 Galioja iki **2026-04-04**
* ⚙️ Certbot naudoja **nginx authenticator**
* 🔄 **Automatinis atnaujinimas veikia** (`renew --dry-run` – OK)
* 🧱 HSTS problema **išnyks iš karto**

Tai reiškia: **Edge, Chrome, Firefox – be jokių perspėjimų.**

---

## Kodėl dabar tai nebesugrius po 90 dienų

Iki šiol:

* `standalone` → konfliktas su 80 portu ❌
* renewal nepavykdavo ❌
* sertifikatas pasibaigdavo ❌

Dabar:

* `authenticator = nginx` ✅
* Nginx pats aptarnauja challenge
* Certbot turi systemd timer / cron
* Jokių prastovų

---

## Rekomenduojamas galutinis patikrinimas (nebūtina, bet „pro“)

```bash
sudo systemctl list-timers | grep certbot
```

Turėtum matyti kažką panašaus:

```
certbot.timer
```

Tai garantija, kad **net jei pamirši – sistema nepamirš**.

---

## Jei žiūrėti dar labiau į ateitį (nebūtina)

Vienintelė situacija, kur dar verta keisti architektūrą:

* serveris būna **išjungtas savaitėmis / mėnesiais**
* keičiasi IP
* yra NAT / CGNAT

Tada:

* DNS-01 su DuckDNS API
* HTTPS be priklausomybės nuo veikiančio serverio

Bet **dabartinis sprendimas 100 % tinkamas**.

---

## Santrauka vienu sakiniu

Tai **nebuvo saugumo incidentas**, o klasikinė **Let’s Encrypt + standalone + Nginx** kombinacijos klaida, kuri dabar **ištaisyta teisingai ir ilgam**.

Jei ateityje:

* pamatysi keistą HTTPS elgesį
* ar planuosi Docker / reverse proxy
* ar norėsi centralizuoti sertifikatus

— žinosi, kad čia jau tvirta bazė.



Trumpas atsakymas: **ne, rankiniu būdu atnaujinti nereikės**, jei sistema veiks taip, kaip dabar.

Paaiškinu tiksliai ir be „marketingo“.

---

## Ką reiškia „This certificate expires on 2026-04-04“

Tai **maksimali galiojimo data**, iki kurios **šis konkretus sertifikatas** galioja.

Svarbu:

* Let’s Encrypt sertifikatai **VISADA galioja ~90 dienų**
* Tai sąmoningas saugumo sprendimas
* Sertifikatas **nėra „pratęsiamas“** – jis **pakeičiamas nauju**

---

## Ar reikės atnaujinti rankiniu būdu?

### ❌ Ne, jei:

* serveris įjungtas bent kartą per kelias savaites
* Nginx veikia
* 80 arba 443 portas pasiekiamas iš interneto
* certbot timer veikia (pas tave **veikia**)

Pas tave:

```
certbot renew --dry-run → success
```

Tai yra **auksinis standartas** – reiškia, kad automatinis atnaujinimas **tikrai pavyks**.

---

## Kaip tai vyksta automatiškai (kasdienybėje)

1. Systemd (`certbot.timer`) **kas ~12 val.** bando:

   ```bash
   certbot renew
   ```
2. Jei iki galiojimo pabaigos liko ~30 dienų:

   * išduodamas **naujas sertifikatas**
   * pakeičiami failai:

     ```
     fullchain.pem
     privkey.pem
     ```
3. Certbot **automatiškai reloadina Nginx**
4. Tu nieko nepastebi
5. Data naršyklėje tiesiog „nusikelia į priekį“

---

## Kada VISGI gali tekti kištis rankiniu būdu

Tik šiais atvejais (realistiškai):

* 🔌 serveris buvo **išjungtas >90 dienų**
* 🌐 80 / 443 portas nepasiekiamas iš išorės
* 🔥 Nginx neveikia tuo metu, kai certbot bando atnaujinti
* 🧱 pakeitei Nginx konfigūraciją ir „užmušei“ challenge location
* 📦 ištrinei `/etc/letsencrypt`

Jei nieko iš to nedarai – **pamiršti gali visam laikui**.

---

## Kaip pačiam pasitikrinti po mėnesio ar metų

Bet kada:

```bash
sudo certbot renew --dry-run
```

Jei **OK** – ramybė.

---

## Esmė vienu sakiniu

**2026-04-04 nėra „terminas, kai tu turi kažką daryti“.**
Tai tiesiog **dabartinio sertifikato viršutinė riba**, kuri **bus automatiškai pastumta į priekį**, dar jam nepasibaigus.

Tu padarei viską **teisingai ir ateičiai**.
