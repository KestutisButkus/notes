## Kad aktyvuoti Django projekto virtualią aplinką (.venv) serveryje, reikia:

Atsidaryti projekto katalogą serveryje:
```
cd /home/as/Skaps_django_project
```
Aktyvuoti .venv aplinką:
```
source venv/bin/activate
```
## Jei naudojam systemd servisą (pvz. gunicorn), ten .venv turi būti nurodytas kaip pilnas kelias, pvz.:
```
ExecStart=/home/as/Skaps_django_project/venv/bin/gunicorn --workers 3 --bind 127.0.0.1:8000 skaps_project.wsgi:application
```
## Jeigu serveryje projektas kaip git repo, atnaujinimas daromas taip:
Nueinam į projekto katalogą:
```
cd /home/as/Skaps_django_project
```
Pasižiūrime ar nėra vietinių pakeitimų (kad git nelemtų konfliktų):
```
git status
```
Parsisiunčiam naujausius pakeitimus iš nuotolinio repo:
```
git pull
```
Dažniausiai po git pull dar reikia:
Atnaujinti priklausomybes (jei pakeistas requirements.txt):
```
source venv/bin/activate
```
```
pip install -r requirements.txt```
```
Paleisti migracijas (jei buvo modelių pakeitimų):
```
python manage.py migrate
```
Surinkti statinius failus (jei keitėsi front-end / static):
```
python manage.py collectstatic --noinput
```
Perkrauti Gunicorn / servisą:
```
sudo systemctl restart gunicorn
```
Gunicorn startuoja būtent iš šito serviso, tik jis nesivadina "gunicorn.service", o buvo sukurtas projekto vardu.
Taigi:
Vietoje sudo systemctl restart gunicorn naudojam:
```
sudo systemctl restart skaps.service
```
Logai:
```
journalctl -u skaps.service -f
```
