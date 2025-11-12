Puiku, Kęstuti — tavo pateiktame „Group Policy Editor“ lange jau aktyvuotas **„Turn off auto-restart for updates during active hours“**, o tai reiškia, kad sistema neperkraus kompiuterio aktyviu metu. Tačiau jei nori **visiškai išjungti automatinį perkrovimą ir palikti tik pranešimą**, reikia dar vieno žingsnio, kurio šiuo metu nematau tarp tavo politikų:

---

### 🔍 Trūkstama politika:  
**„No auto-restart with logged on users for scheduled automatic updates installations“**

Ši politika **visiškai blokuoja automatinį perkrovimą**, jei vartotojas yra prisijungęs, nepriklausomai nuo aktyvių valandų. Ji veikia kartu su pranešimų sistema, todėl vartotojas matys įspėjimą, bet perkrovimas neįvyks automatiškai.

---

### 🛠️ Kaip ją pridėti rankiniu būdu per registrą

Kadangi jos nematai tarp politikų, gali ją įjungti per registrą:

1. Paspausk `Win + R`, įvesk `regedit` → Enter.
2. Eik į:
   ```
   HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU
   ```
   Jei raktas `AU` neegzistuoja, sukurk jį.
3. Sukurk naują **DWORD (32-bit)** reikšmę:
   ```
   NoAutoRebootWithLoggedOnUsers
   ```
   Nustatyk reikšmę į `1`.

---

### 🔔 Pranešimų rodymai

Kad vartotojas tikrai matytų įspėjimą:

- Eik į **Settings → System → Notifications**.
- Įsitikink, kad **Windows Update notifications** yra įjungtos.
- Galima papildomai aktyvuoti politiką **„Display options for update notifications“**, jei nori valdyti pranešimų elgseną.

---

### 🧠 Ką tai duoda?

- **Automatinis perkrovimas išjungtas visam laikui**, net jei atnaujinimas įdiegtas.
- **Vartotojas informuojamas**, bet sprendimą dėl perkrovimo priima pats.
- **Saugus sprendimas serveriams ir darbo stotims**, kur netikėtas perkrovimas gali sukelti problemų.

Jei nori, galiu paruošti `.reg` failą arba PowerShell skriptą, kuris tai pritaiko automatiškai. Pasakyk, jei norėtum tokio sprendimo.


### 🔧 Kaip užtikrinti, kad veiktų?

1. **Configure Automatic Updates → Enabled**
   - Nustatyk režimą: „Auto download and notify for install“ arba „Scheduled install“.
2. **No auto-restart with logged on users → Enabled**
   - Užtikrina, kad perkrovimas neįvyks automatiškai.
3. **Turn off auto-restart during active hours → Enabled**
   - Papildoma apsauga darbo metu.
4. **Notifications → įjungtos**
   - Kad vartotojas matytų, jog reikia perkrauti.

---
Puiku, Kęstuti — tai **būtent ta politika**, kurios tau reikia norint **visiškai išjungti automatinį perkrovimą ir palikti tik pranešimą vartotojui**.

### ✅ „No auto-restart with logged on users for scheduled automatic updates installations“

Kai ši politika nustatyta kaip **Enabled**:

- 💡 **Automatinis perkrovimas neįvyks**, jei vartotojas yra prisijungęs.
- 🔔 **Vartotojas gaus pranešimą**, kad reikia perkrauti kompiuterį rankiniu būdu.
- 🧠 **Atnaujinimai nebus aktyvuoti**, kol vartotojas pats neperkraus sistemos.

---

### ⚠️ Svarbi pastaba

Kaip teisingai pastebėjai, ši politika veikia **tik jei „Configure Automatic Updates“ yra aktyvuota**. Kad ji turėtų efektą, reikia:

1. Grupės politikoje eiti į:
   **Computer Configuration → Administrative Templates → Windows Update**
2. Atidaryti **Configure Automatic Updates**.
3. Pasirinkti **Enabled**.
4. Nustatyti, pvz., **Auto download and notify for install** (arba kitą tau tinkamą režimą).

Tik tada „No auto-restart…“ politika bus taikoma.

---

### 🧠 Ką tai reiškia tavo atveju?

Kadangi valdai sistemą, kur svarbu išvengti netikėtų perkrovimų (pvz., serveriai, dokumentacijos redagavimas, mokėjimų UX), šis derinys:

- **Configure Automatic Updates → Enabled**
- **No auto-restart with logged on users → Enabled**

…leidžia tau **išlaikyti pilną kontrolę**: atnaujinimai įdiegiami, bet perkrovimas vyksta tik tada, kai tu nusprendi.

Jei nori, galiu paruošti `.reg` arba PowerShell skriptą, kuris tai pritaiko automatiškai. Ar norėtum tokio sprendimo?


### 🧩 Alternatyva: registro reikšmė

Jei nori tai pritaikyti ir „Windows Home“ versijoje:

```reg
[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU]
"NoAutoRebootWithLoggedOnUsers"=dword:00000001
```

Tai veikia identiškai ir yra palaikoma „Windows 11“.

---
**Štai PowerShell skriptas, kuris išjungia automatinį perkrovimą po „Windows Update“ ir palieka tik pranešimą vartotojui. Jis veikia tiek „Windows 11 Pro“, tiek „Home“ versijose.**

---

### 🧩 PowerShell skriptas: `Disable-AutoRestart.ps1`

```powershell
# Patikrina, ar skriptas vykdomas kaip administratorius
If (-NOT ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)) {
    Write-Warning "Šis skriptas turi būti vykdomas kaip administratorius."
    Break
}

# Sukuria reikiamą registro raktą, jei jo nėra
$regPath = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
If (-Not (Test-Path $regPath)) {
    New-Item -Path $regPath -Force | Out-Null
}

# Nustato reikšmę, kuri išjungia automatinį perkrovimą
Set-ItemProperty -Path $regPath -Name "NoAutoRebootWithLoggedOnUsers" -Value 1 -Type DWord

# Informacija vartotojui
Write-Host "✅ Automatinis perkrovimas išjungtas. Vartotojas gaus tik pranešimą apie būtinybę perkrauti." -ForegroundColor Green
```

---

### 🛠️ Kaip naudoti

1. Atidaryk **PowerShell kaip administratorius**.
2. Įklijuok skriptą tiesiai į langą arba išsaugok kaip `Disable-AutoRestart.ps1`.
3. Paleisk skriptą:
   ```powershell
   .\Disable-AutoRestart.ps1
   ```

---

### 🔒 Kas tiksliai vyksta?

- Sukuriamas arba atnaujinamas registro raktas:
  ```
  HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU
  ```
- Įrašoma reikšmė:
  ```
  NoAutoRebootWithLoggedOnUsers = 1
  ```
- Tai **užtikrina, kad „Windows“ neperkraus kompiuterio automatiškai**, jei vartotojas yra prisijungęs.
- **Pranešimas apie perkrovimą vis tiek bus rodomas**, todėl vartotojas žinos, kad reikia perkrauti rankiniu būdu.

---

### 📌 Papildoma rekomendacija

Jei nori dar daugiau kontrolės, gali pridėti ir šiuos papildomus PowerShell veiksmus:

```powershell
# Nustatyti aktyvias valandas (pvz., 08:00–23:00)
$startHour = 8
$endHour = 23
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings" -Name "ActiveHoursStart" -Value $startHour
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings" -Name "ActiveHoursEnd" -Value $endHour
```

Tai padeda dar labiau apriboti, kada sistema gali bandyti perkrauti (jei vartotojas atsijungęs).

---

Jei nori, galiu padėti su `.bat` failu, kuris paleidžia šį skriptą automatiškai po kiekvieno „feature update“. Ar tai būtų naudinga?
