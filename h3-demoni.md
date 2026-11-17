# h3 Demoni 

Tämän raportin kaikki tehtävät on tehty debian 13 virtuaalikoneella paikallisesti.

## x) Lue ja tiivistä

### Tiivistelmä [Karvinen 2026 Apache installed with Ansible - quick notes](https://terokarvinen.com/apache-ansible/)

* Ansiblen Apache2-asennus hyödyntää "package-file-service" -kaavaa: paketti asennetaan, asetustiedostot luodaan ja lopuksi palvelu käynnistetään uudelleen.
* Roolin hakemistorakenne sisältää tehtävät `tasks`, konfiguraatiotiedoston ja uudelleenkäsittelyn hoitavan handlerin

### Tiivistelmä Ansible Community Documentation: [Handlers: running operations on change](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html)

* Handlerit ovat tehtäviä, jotka käynnistyvät vain kun toinen tehtävä tekee muutoksen järjestelmään. Tällaisia ovat esimerkiksi palvelun uudelleenkäynnistys konfiguraatiomuutoksen jälkeen.
* Tehtävät yhdistetään handlereihin käyttämällä notify-komentoa
* Yksi tehtävä voi kutsua yhtä tai useita handlereita

### [Tiivistelmä ansible-doc service](https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/service_module.html#ansible-collections-ansible-builtin-service-module)

* `ansible.builtin.service` moduulilla hallitaan kohdekoneiden demoneita eli palveluita. Moduuli toimii välikätenä ohjattavien koneiden omille palvelunhallintajärjestelmille kuten `systemd`.
* Enabled on boolean arvo (joo/ei), jonka avulla määritetään käynnistyykö palvelu koneen käynnistyksen yhteydessä vai ei.
* `name` on hallittavan palvelun tarkka nimi kuten `apache2`
* `state` määrittää palvelun tilan, vaihtoehdoista `reloaded`, `restarted`, `started` ja `stopped`. `started` ja `stopped` ovat idempotentteja, mikä tarkoittaa että muutos ajetaan vain, jos kyseinen tila ei ole jo käytössä. `restarted` uudelleenkäynnistää palvelun.
* Esimerkki: `-name : Start service httpd, if not started`
  `ansible.builtin.service`
  `name:httpd`
  `state:started`

## y) Infra-uutiset

Tehtävänä oli etsiä jokin artikkeli sivustolta news.ycombinator.com, mikä liittyi keskitettyyn hallintaan. En löytänyt paljoakaan uusia artikkeleita aiheesta, joten nappasin aiheeksi postauksen, jossa oli linkki keskitettyyn ansiblella tehtyyn kovennus konfiguraatioon, mikä on tarkoitettu linuxille ssh:n, Nginx:n ja MYSQL:n koventamiseen eli suojaamiseen.  Linkki : [Ansible battle tested hardening for Linux, SSH, Nginx, MySQL](https://news.ycombinator.com/item?id=46529507)

* Toimii useilla Linux-versioilla, kuten `Debian 12/13`, `Ubuntu 22.04/24.04/26.04` ja `Rocky Linux 8/9/10`
* Toimii seuraavilla tietokannoilla `MariaDB >= 5.5.65, >= 10.1.45, >= 10.3.17`, `MySQL >= 5.7.31, >= 8.0.3`
* Toimii `OpenSSH 5.3 ->` ja `nginx 1.0.16 ->`


## a) Apassi. Asenna Apache 2 käsin

Tehtävänä oli asentaa apache2, joten aloitin tavanomaisesti päivittämällä pakettilistat ja sitten asentamalla apache2-paketin.
`sudo apt-get update` ja `sudo apt-get install apache2`.

Asennuksen jälkeen ajoin vielä komennon `sudo systemctl enable --now apache2` käynnistääkseni demonin ja varmistaakseni, että se käynnistyy myös automaattisesti koneen käynnistyessä.
Sitten katsoin palvelun tilan komennolla `sudo systemctl status apache2.service`. Tästä sain varmistuksen, että palvelu on ylhäällä `active(running)`. Tämän jälkeen menin vielä selaimella varmistamaan, että apachen automaattinen html-sivu näkyy.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/bea46375-d793-45f9-81a0-e504d912c313" />
Kuva 1. Apachen käynnistys ja varmistus.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/a6968fe8-84ba-4a73-bb7f-24f9b03c6075" />
Kuva 2. Apachen automaattinen kotisivu.

Tehtävänä oli tehdä weppisivusta tavallisen käyttäjän muokattavissa oleva ilman sudoa. Apache oletusarvoinen hakemisto on `/var/www/html` ja omistaja `root:root`, joten tavallinen käyttäjä ei pysty siellä olevia tiedostoja muokkaamaan.

En jaksanut muuttaa hakemistopolkua, sillä sitten olisi tarvinnut muuttaa apachen konfigurointitiedostoja, joten ratkaisin ongelman vaihtamalla omistajuuden omalle käyttäjälleni `tauski` ja lisäämällä tarvittavat käyttöoikeudet muillekkin.
`sudo chown -R tauski:www-data /var/www/html` ja `sudo chmod -R 775 /var/www/html/`. Suomeksi sanottuna omistaja tauski, ryhmä www-data , oikeudet 775 eli `-rwxrwxr-x`, Eli omistaja ja ryhmä luku, kirjoitus, suoritusoikeus, muut luku ja suoritusoikeus. -R suorittaa komennon rekursiivisesti

## b) Moottorix. Asenna Nginx käsin. Weppisivun tulee näkyä palvelimen etusivulla.

## c) Automoottorix. Automatisoi Nginx asennus Ansiblella. Ylläpitäjän osuus Ansiblella riittää, itse HTML-weppisivut voi tehdä käsin.

## Lähteet

Karvinen, T 2026 Palvelinten halinta https://terokarvinen.com/palvelinten-hallinta/ Luettu 17.11.2026

Karvinen, T 2026 Apache installed with Ansible - quick notes https://terokarvinen.com/apache-ansible/ Luettu 17.11.2026

Ansible-doc service https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/service_module.html#ansible-collections-ansible-builtin-service-module 
Luettu 17.11.2026

Ansible battle tested hardening for Linux, SSH, Nginx, MySQL https://news.ycombinator.com/item?id=46529507 Luettu 18.11.2026

