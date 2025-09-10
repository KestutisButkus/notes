#### Speedtest

```
speedtest-cli --simple
```
Išvestis atrodys maždaug taip:
```
Ping: 23.45 ms
Download: 93.21 Mbit/s
Upload: 42.11 Mbit/s
```
#
#### Stebėti išorines užklausas į Nginx serverį (ypač bandymus pasiekti .env, .git, ar kitus jautrius failus)

📄 1. Peržiūrėti Nginx log'us
Access log – rodo visas užklausas
```
sudo tail -f /var/log/nginx/access.log
```

Filtruojam tik įtartinas užklausas:
```
grep -Ei "\.env|\.git|config" /var/log/nginx/access.log
```

Error log – rodo blokuotas ar klaidingas užklausas
```
sudo tail -f /var/log/nginx/error.log
```
#
Filtruojam tik įtartinas užklausas realiu laiku iš access.log, galima naudoti grep kartu su tail -f. Štai keletas pavyzdžių, kurie padės tau stebėti galimus bandymus pasiekti jautrius failus:

Stebėti .env, .git, .config ir kitus pavojingus taikinius
```
sudo tail -f /var/log/nginx/access.log | grep -Ei "\.env|\.git|config|passwd|\.ht|\.bak"
```
- -f – stebi failą realiu laiku
- grep -Ei – ieško nepriklausomai nuo raidžių dydžio (-i) ir leidžia naudoti regex (-E)
- Regex – ieško .env, .git, config, passwd, .htaccess, .bak ir pan.

Matyti IP adresus, kurie daro šias užklausas
```
sudo tail -f /var/log/nginx/access.log | grep -Ei "\.env|\.git|config" | awk '{print $1}'
```
Ištraukia IP adresus, kurie bando pasiekti pavojingus failus.
#
#### Aalias komanda
Kad nereikėtų vis iš naujo rašyti ilgos komandos, galima ją pridėti į .bashrc:
```
alias nginxwatch='sudo tail -f /var/log/nginx/access.log | grep -Ei "\.env|\.git|config|passwd|\.ht|\.bak"'
```
Tada tiesiog paleidžiam:
```
nginxwatch
```
galima sukurti skriptą, kuris automatiškai užblokuoja IP, jei jie bando pasiekti .env ar .git.

