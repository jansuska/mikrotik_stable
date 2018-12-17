#   Projekt z programovania
#   Návrh a realizácia monitorovacích skriptov v MikroTik RouterOS

[![miktorik.com](https://i.mt.lv/img/mt/v2/logo.svg)](https://mikrotik.com/)

[![miktorik.wiki](https://wiki.mikrotik.com/images/thumb/1/18/Ros.png/48px-Ros.png)](https://wiki.mikrotik.com/wiki/Main_Page)

Zadanie práce:
## Oboznámiť sa s programovacím jazykom miktorik script, naprogramovať funkčný a zároveň užitočný miktorik script, zdokumentovať svoju prácu formou príručky a prezentácie.

Vytvorené scripty:
  - Záloha nastavenia miktorik routra do suboru .bat .rsc a odoslanie ako prilohu mailu
  - Sledovanie zaťaženia DHCP pool-u a posielat upozornenie o naplnení na mail
  - script scheduler, ktorý po vložení do miktorik terminálu vytvorí udalosť scheduler

## Úvod
Moja práca na tomto projekte začala oboznámením sa s prostredím miktorik. Pretože som sa s ním doposiaľ nestretol. To obnášalo následné študovanie miktorik script pretože mi bol cudzí kedže to bola pre mňa úplne nová vec a tak som musel začat s vypracovaním tejto práce od úplného začiatku. Moje prvé skripty noli jednoduché ako je ping na danú IP adresu ci vo Wan alebo Lan sieti. Dalej kontrola interface. Následne som začal skúmať zapisovanie do LOG -ov systému mikrotik a odosielanie infromácii na mail. Pri čom som používal rôzne súčasti miktorik systému ktoré bližie popíšem.
## Log
Systémovou súčasťou je funkcia LOG. Táto funkcnia napíše správu do systémového denníka. Danú spávu možeme označit podľa priority ako sú napríklad info, warning, error. Tieto informácie sú v sýstémovom denníku odlíšené podľa priority tiež farbou textu a to nasledovne: info - čierna, warning - modrá, error červená.
### Príklad kódu zo sckriptu:
:log info ("tento text uvidim v log-och")
## Tool mail
Pred odosielanim mailov zo skriptu je nutné nastaviť mailového klienta v miktorik routre v časti tools email kde sú požadované nasledujúce parametre.
#### Server
- ip adresa servere. Túto adresu zistíme vyhladaním na internete alebo jednoducho ping-om na adresu v mojom prípade smtp.gmail.com kde vráti adresu: 64.233.167.109
#### Port
- port na ktorom pracuje smtp protokoloch nášho klienta, jednoducho vyhľadám na internete. V našom prípade zadáme číslo portu 587
#### Start tls
- jedná sa o formu zabezpečenia pre maily, pri konfigurácii máme na výber yes, tls only, no.
#### From
- toto pole vyplníme našou mailovou adresou
#### User
- zvolíme si ľubovolný používaťelský názov, z pravidla časť mailovej adresy pred znakom @
#### Password
- toto pole vyplníme heslom našej mailovej schránky

Po vyplnení týchto atribútov možeme použit tool mail v skripte a tak zaslať mail.
### Príklad kódu zo sckriptu:
/tool e-mail send to=meno@server.com subject="predmet spravy" body="telo mailu" start-tls=yes
#### tool e-mail
- funkcnia miktorik systému ktorý spúšťame v skripte
#### send to
- mailová adresa na ktorú chceme mail odoslať, može sa zhodovať s mailovou adresou z ktorej posielame (adresa vyplnená v tool mail)
#### subjest
-  predmet správy vyplníme textom ktorý chceme vidieť v predmete
#### body
- telo mailu vyplníme textom ktorý chceme vidieť v tele prípadne použijeme ďaľšie funkcie
#### Start tls
- jedná sa o formu zabezpečenia pre maily, ktorú si želáme preto zvolíme yes

#### Formát zápisu premenných
Na danom zariadení si môžeme zvoliť tzv. lokálnu premennú a to nasledovne
:local adminMail "alarmmasterjs@gmail.com"

lokálna premenná s názvom adminMail. Táto premenná obsahuje: alarmmasterjs@gmail.com
túto premennú možeme následne v kóde použiť nasledovne:
/tool e-mail send to=$adminMail ...
