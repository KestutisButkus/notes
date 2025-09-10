#### jei norite matyti tik greičio skaičius:

```python
speedtest-cli --simple
```


Išvestis atrodys maždaug taip:
```
Ping: 23.45 ms
Download: 93.21 Mbit/s
Upload: 42.11 Mbit/s
```

#### Jei norim stebėti išorines užklausas Nginx serverį (ypač bandymus pasiekti .env, .git, ar kitus jautrius failus)

📄 1. Peržiūrėk Nginx log'us
🔍 Access log – rodo visas užklausas
```
sudo tail -f /var/log/nginx/access.log
```

Arba, filtruojam tik įtartinas užklausas:
```
grep -Ei "\.env|\.git|config" /var/log/nginx/access.log
```

⚠️ Error log – rodo blokuotas ar klaidingas užklausas
```
sudo tail -f /var/log/nginx/error.log
```




Jei nori filtruoti tik įtartinas užklausas realiu laiku iš access.log, gali naudoti grep kartu su tail -f. Štai keletas pavyzdžių, kurie padės tau stebėti galimus bandymus pasiekti jautrius failus:

🔍 Stebėti .env, .git, .config ir kitus pavojingus taikinius
```
sudo tail -f /var/log/nginx/access.log | grep -Ei "\.env|\.git|config|passwd|\.ht|\.bak"


- -f – stebi failą realiu laiku
- grep -Ei – ieško nepriklausomai nuo raidžių dydžio (-i) ir leidžia naudoti regex (-E)
- Regex – ieško .env, .git, config, passwd, .htaccess, .bak ir pan.

📌 Jei nori matyti IP adresus, kurie daro šias užklausas
sudo tail -f /var/log/nginx/access.log | grep -Ei "\.env|\.git|config" | awk '{print $1}'


Tai ištrauks IP adresus, kurie bando pasiekti pavojingus failus.

🧠 Patarimas: sukurk alias komandą
Kad nereikėtų vis iš naujo rašyti ilgos komandos, gali ją pridėti į .bashrc:
alias nginxwatch='sudo tail -f /var/log/nginx/access.log | grep -Ei "\.env|\.git|config|passwd|\.ht|\.bak"'


Tada tiesiog paleisk:
nginxwatch



Jei nori, galim sukurti skriptą, kuris automatiškai užblokuoja IP, jei jie bando pasiekti .env ar .git. Ar norėtum tokio automatinio apsaugos mechanizmo?

