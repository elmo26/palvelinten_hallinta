# h3 Demoni 

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

## b) Moottorix. Asenna Nginx käsin. Weppisivun tulee näkyä palvelimen etusivulla.

## c) Automoottorix. Automatisoi Nginx asennus Ansiblella. Ylläpitäjän osuus Ansiblella riittää, itse HTML-weppisivut voi tehdä käsin.

## Lähteet

Karvinen, T 2026 Palvelinten halinta https://terokarvinen.com/palvelinten-hallinta/ Luettu 17.11.2026

Karvinen, T 2026 Apache installed with Ansible - quick notes https://terokarvinen.com/apache-ansible/ Luettu 17.11.2026

Ansible-doc service https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/service_module.html#ansible-collections-ansible-builtin-service-module 
Luettu 17.11.2026

Ansible battle tested hardening for Linux, SSH, Nginx, MySQL https://news.ycombinator.com/item?id=46529507 Luettu 18.11.2026

