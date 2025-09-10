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

#### Jei norim stebėti išorines užklausas į savo Nginx serverį (ypač bandymus pasiekti .env, .git, ar kitus jautrius failus), štai keletas būdų, kaip tai padaryti efektyviai:

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





