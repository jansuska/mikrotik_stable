#   Projekt z programovania
#   Návrh a realizácia monitorovacích skriptov v MikroTik RouterOS

[![mikrotik.com](https://i.mt.lv/img/mt/v2/logo.svg)](https://mikrotik.com/)

[![mikrotik.wiki](https://wiki.mikrotik.com/images/thumb/1/18/Ros.png/48px-Ros.png)](https://wiki.mikrotik.com/wiki/Main_Page)

Zadanie práce:
## Oboznámiť sa s programovacím jazykom mikrotik skript, naprogramovať funkčný a zároveň užitočný mikrotik skript, zdokumentovať svoju prácu formou príručky a prezentácie.

Vytvorené scripty:
  - Záloha nastavenia mikrotik routra do suboru .bat .rsc a odoslanie ako prilohu e-mailu
  - Sledovanie zaťaženia DHCP pool-u a posielat upozornenie o naplnení na e-mail
  - script scheduler, ktorý po vložení do mikrotik terminálu vytvorí udalosť scheduler

## Úvod
Moja práca na tomto projekte začala oboznámením sa s prostredím mikrotik. Pretože som sa s ním doposiaľ nestretol. To obnášalo následné študovanie mikrotik skript, pretože mi bol cudzí, kedže to bola pre mňa úplne nová vec a tak som musel začat s vypracovaním tejto práce od úplného začiatku. Moje prvé skripty boli jednoduché ako je ping na danú IP adresu či vo Wan alebo Lan sieti. Ďalej kontrola interface. Následne som začal skúmať zapisovanie do LOG -ov systému mikrotik a odosielanie infromácii na e-mail, pričom som používal rôzne súčasti mikrotik systému, ktoré bližšie popíšem.
## Log
Systémovou súčasťou je funkcia LOG. Táto funkcnia napíše správu do systémového denníka. Danú spávu môžeme označit podľa priority ako sú napríklad info, warning, error. Tieto informácie sú v sýstémovom denníku odlíšené podľa priority tiež farbou textu a to nasledovne: info - čierna, warning - modrá, error červená.
### Príklad kódu zo sckriptu:
:log info ("tento text uvidim v log-och")
## Tool mail
Pred odosielanim e-mailov zo skriptu je nutné nastaviť e-mailového klienta v mikrotik routre v časti tools e-mail kde sú požadované nasledujúce parametre.
#### server
- ip adresa servere. Túto adresu zistíme vyhľadaním na internete alebo jednoducho ping-om na adresu v mojom prípade smtp.gmail.com kde vráti adresu: 64.233.167.109
#### port
- port, na ktorom pracuje smtp protokoloch nášho klienta, jednoducho vyhľadám na internete. V našom prípade zadáme číslo portu 587
#### start tls
- jedná sa o formu zabezpečenia pre e-maily, pri konfigurácii máme na výber yes, tls only, no.
#### from
- toto pole vyplníme našou e-mailovou adresou
#### user
- zvolíme si ľubovolný používaťelský názov, z pravidla časť e-mailovej adresy pred znakom @
#### password
- toto pole vyplníme heslom našej e-mailovej schránky

Po vyplnení týchto atribútov môžeme použiť tool mail v skripte, a tak zaslať e-mail.
### Príklad kódu zo sckriptu:
/tool e-mail send to=meno@server.com subject="predmet spravy" body="telo mailu" start-tls=yes
#### tool e-mail
- funkcnia mikrotik systému, ktorý spúšťame v skripte
#### send to
- e-mailová adresa, na ktorú chceme d-mail odoslať, sa môže zhodovať s e-mailovou adresou, z ktorej posielame (adresa vyplnená v tool mail)
#### subjest
-  predmet správy vyplníme textom, ktorý chceme vidieť v predmete
#### body
- telo e-mailu vyplníme textom, ktorý chceme vidieť v tele prípadne použijeme ďaľšie funkcie
#### start tls
- jedná sa o formu zabezpečenia pre e-maily, ktorú si želáme, preto zvolíme yes

#### Formát zápisu premenných
Na danom zariadení si môžeme zvoliť tzv. lokálnu premennú a to nasledovne
:local adminMail "alarmmasterjs@gmail.com"

lokálna premenná s názvom adminMail. Táto premenná obsahuje: alarmmasterjs@gmail.com
túto premennú môžeme následne v kóde použiť nasledovne:
/tool e-mail send to=$adminMail ...
