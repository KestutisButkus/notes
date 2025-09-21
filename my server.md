### Kad aktyvuotum savo Django projekto virtualią aplinką (.venv) serveryje, reikia:

Atsidaryti projekto katalogą serveryje:

"""
cd /home/as/Skaps_django_project
"""

Aktyvuoti .venv aplinką:

"""
source venv/bin/activate
"""

arba, jei pavadinai .venv:

"""
source .venv/bin/activate
"""

Po aktyvavimo komandų eilutėje pamatysi prefiksą, pvz.:
"""
(.venv) as@server:~/Skaps_django_project$
"""

### Jei naudoji systemd servisą (pvz. gunicorn), ten .venv turi būti nurodytas kaip pilnas kelias, pvz.:

"""
ExecStart=/home/as/Skaps_django_project/venv/bin/gunicorn --workers 3 --bind 127.0.0.1:8000 skaps_project.wsgi:application
"""
