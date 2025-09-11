Kaip įdiegti ir naudoti Zabbix Ubuntu serveryje
1. 	Paruošk sistemą

2. 	Įdiek Zabbix komponentus

3. 	Sukurk duomenų bazę

4. 	Importuok Zabbix schemą

5. 	Konfigūruok Zabbix serverį Redaguok:


6. 	Paleisk paslaugas

7. 	Prisijunk prie Zabbix web sąsajos
• 	Naršyklėje eik į http://server_ip/zabbix
• 	Prisijungimo duomenys:
• 	Vartotojas: Admin
• 	Slaptažodis: zabbix

Kaip naudoti Zabbix kasdien
• 	Pridėk host'us: tavo serverį, kitus įrenginius, Docker konteinerius
• 	Sukurk trigerius: pvz., „jei RAM > 90%, siųsti įspėjimą“
• 	Naudok šablonus: Zabbix turi paruoštus šablonus Nginx, PostgreSQL, Docker ir kt.
• 	Stebėk grafikus: CPU, diskas, tinklas — viskas vizualiai
Saugumo patarimai
• 	Keisk numatytą slaptažodį
• 	Naudok HTTPS (galima su Certbot)
• 	Apribok prieigą prie Zabbix web sąsajos pagal IP
Jei nori, galim kartu sukonfigūruoti pirmąjį host'ą, sukurti trigerį, ir net integruoti Telegram įspėjimus. Ar Zabbix naudoji tik savo serveriui, ar planuoji stebėti ir kitus įrenginius tinkle?
