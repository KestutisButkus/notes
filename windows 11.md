

---

### Norint **visiškai išjungti automatinį perkrovimą ir palikti tik pranešimą**
reikia aktyvuoti **„No auto-restart with logged on users for scheduled automatic updates installations“**

Grupės politikoje eiti į:
   **Computer Configuration → Administrative Templates → Windows Components → Windows Update → Legacy Polices**

Ši politika **visiškai blokuoja automatinį perkrovimą**, jei vartotojas yra prisijungęs, nepriklausomai nuo aktyvių valandų. Ji veikia kartu su pranešimų sistema, todėl vartotojas matys įspėjimą, bet perkrovimas neįvyks automatiškai.

---

### 🛠️ Pridėjimas rankiniu būdu per registrą

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

- Einam į **Settings → System → Notifications**.
- Įsitikinam, kad **Windows Update notifications** yra įjungtos.
- Galima papildomai aktyvuoti politiką **„Display options for update notifications“**.

---

### 🧠 Ką tai duoda?

- **Automatinis perkrovimas išjungtas visam laikui**, net jei atnaujinimas įdiegtas.
- **Vartotojas informuojamas**, bet sprendimą dėl perkrovimo priima pats.
- **Saugus sprendimas serveriams ir darbo stotims**, kur netikėtas perkrovimas gali sukelti problemų.

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

**Štai PowerShell skriptas, kuris išjungia automatinį perkrovimą po „Windows Update“ ir palieka tik pranešimą vartotojui. Jis veikia tiek „Windows 11 Pro“, tiek „Home“ versijose.**

---

### 🧩 PowerShell skriptas: `Disable-AutoRestart.ps1`

Sukuriame failą:

'''
Disable-AutoRestart.ps1
'''

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

1. Atidarom **PowerShell kaip administratorius**.
2. Paleidžiam skriptą:
   ```powershell
   Disable-AutoRestart.ps1
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
