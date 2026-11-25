# h4 Pizza Fantasia

### Harjoituksen ympäristö

Tein kaikki tehtävät paikallisella debian 13 "trixie" virtuaalikoneella. Host-järjestelmänä Linux Mint ja virtuaalisointiympäristönä toimi QEMU, hypervisorina kvm. Virtuaalikoneelle allokoituna 4 prosessoria ja 6gb muistia sekä 50gb levytilaa.

## x) Lue ja tiivistä - Karvinen, T 2024 Configuration Management of Distributed Systems over Unreliable and Hostile Networks

### 4.12.1 Size and complexity of some DSLs

* DSL eli domain spesific language tarkoittaa johonkin tiettyyn käyttötarkoitukseen tehtyä kieltä, esimerkiksi keskitetyn hallintajärjestelmän omaa kieltä (Karvinen 2026).
* DSL-kielet saattavat olla monimutkaisia ja sisältää suuren määrän funktioita, sekä niiden dokumentaatio on usein pitkä.
* DSL-kielten ominaisuudet ja toiminta eroavat usein toisistaan ja tunnetuista ohjelmointikielistä kuten Pythonista tai Gosta.

### 4.12.2 Use of DSL Functions in Case Configuration

* Vaikka DSL-kielet ovat suuria kokonaisuuksia, pieni määrä ominaisuuksia (funktioita, komentoja) kattaa prosentuaalisesti hyvin suuren osan kielen käytössä käytetyistä ominaisuuksista: vaikka kielessä saattaa olla satoja funktioita, suuri osa kirjoitetusta koodista voi käyttää vain muutamaa näistä funktioista.
* Tietyissä tilanteissa käyttäjä voi tehdä muutokset manuaalisesti, jolloin idempotenssin saavuttaminen on käyttäjän vastuulla.

### 4.12.3.1 Dependencies Between Main Functions

* Konfiguraatiohallinnassa (configuration management) tärkeimpiä ominaisuuksia ovat Daemonien, sovellusten, käyttäjien ja tiedostojen hallinta ja manipulaatio.
* Konfiguraatiohallinnassa käytetyn järjestelmän tulisi olla idempotentti, Tämä tarkoittaa, että muutoksia tehdään vain silloin kun järjestelmä ei jo ole halutussa tilassa.
* Tärkeimmät ominaisuudet saadaan toteutettua pienellä määrällä funktioita: `package, file, service, user, group, directory, symlink`.
* Muutaman ydinfunktion avulla voidaan rakentaa haluttuja alemman tason funktioita.

## a) Räpylä. Asenna itse valitsemasi demoni käsin. Jokin muu kuin tunnilla tai kotitehtävissä aiemmin asennettu.

Päätin asentaa mariadb tietokantojen hallintajärjestlemän. Päivitin pakettilistat, päivitin nykyiset paketit ja sitten asensin mariadb-paketin käsin.

````
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install mariadb-server
````

Sitten tarkistin vielä asennuksen onnistumisen katsomalla mariadb:n statuksen komennolla `sudo systemctl status mariadb`.

mariadb.service: enabled. Paketin asennus onnistui.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/3046e383-e29a-42c5-94bf-b9f8920d44de" />

Kuva 1. mariadb status.

Lopuksi poistin vielä mariadb:n asennuksen komennolla `sudo apt-get purge mariadb`, sillä asentaisin sen uudelleen ansiblella.

## b) Automaatti. Automatisoi valitsemasi demonin asennus Ansiblella.

Siirryin ansiblen kotihakemistoon eli `pwd=home/user/ansible` Loin ansibleen uuden roolin mariadb:lle ja kansiot `tasks`, `handlers` sekä `file` sen asennusta ja hallintaa varten komennolla `mkdir -p roles/mariadb/{tasks,handlers,files}`. Sitten loin uuden main.yml -tiedoston komennolla `micro roles/mariadb/tasks/main.yml` ja lisäsin sinne seuraavat asiat:

````
- apt:
    name: mariadb-server
    state: present

- service:
    name: mariadb
    state: started
    enabled: true
````
Tässä siis valittu paketti aptilla, varmistettu että palvelu on päällä `state:started`.

Sitten lisäsin vielä roolin `mariadb` `site.yml` -tiedostoon ja ajoin komennon `ansible-playbook site.yml`.

<img width="2880" height="1720" alt="image" src="https://github.com/user-attachments/assets/0b3b4e79-014f-46c7-bde1-7949f3b72f62" />

Kuva 2. mariadb:n asennus ansiblella.

Kuten kuvasta näkyy, mariadb:n asennus onnistui ansiblen avulla `changed=1`.


## c) Asetus. Muuta asetustiedostoa herralla ja aja ansible uudestaan. Osoita, että asetukset tulivat käyttöön.

MariaDB:n konfiguraatiotiedosto on polussa `etc/mysql/my.cnf` (MariaDB 2026). Katsoin konffitiedostosta että mitä siellä jo on ja päätin lisätä sinne kommentin "tämä on tehty ansiblella" sekä muuttaa palvelun portin `3306 - > 3307`. Muuten pidin tiedoston oletusarvoissa enkä poistanut tai muuttanut mitään muuta.

Ensin kopioin konfiguraatiotiedoston komennolla `cp /etc/mysql/my.cnf roles/mariadb/files/my.cnf` ansibleen, muokkasin ansibleen kopiomaani konfiguraatiotiedostoa lisäämällä sinne kommentin "tämä on tehty ansiblella".
sekä muutin kohdasta [client-server] oletusportin `3306` portiksi `3307` Tämän jälkeen lisäsin `roles/mariadb/tasks/main.yml` tiedostoon seuraavat asiat:

````
-name: kopioi mariadb konfiguraatiotiedosto
 copy:
   src: my.cnf
   dest: /etc/mysql/my.cnf
   owner: rauski
   group: rauski
   mode: 0644
 notify: foo
````

Tässä kopioin muokatun version konfiguraatiotiedostosta `roles/mariadb/files/my.cnf` konfiguraatiotiedoston polkuun `/etc/mysql/my.cnf`, sekä asetin omistajan ja ryhmän `rauski` ja oikeudet `0644` eli `-rw-r--r--`.
Nofiy-kohta kutsuu handleria (alla handler) ja uudelleenkäynnistää palvelun ajettaessa.

Sitten lisäsin vielä mariadb:n handlers-kansion `main.yml` tiedostoon seuraavat asiat:

````
- name: foo
  service:
    name: mariadb
    state: restarted
````
Tämän tein siksi, että kaikki asetukset varmasti menevät päälle uudelleenkäynnistyksen myötä.

Tämän jälkeen ajoin komennon `ansible-playbook site.yml`

<img width="2880" height="1720" alt="image" src="https://github.com/user-attachments/assets/70d0ab48-345b-4679-b7eb-5a5fbdb28c49" />
Kuva 3. Ansiben ajo

Kuvassa näkyy konfiguraatiotiedostoon ajettu kommentti. Alemmassa kuvassa puolestaan näkyy kuunteleva portti `3307`, joka konfiguroitiin tiedostoon myös.

<img width="2880" height="1800" alt="mariadb" src="https://github.com/user-attachments/assets/7e87b560-9ee5-4c17-bc39-ee7bd2a10241" />

Kuva 4.

Tässä vielä todennus siitä, että mariadb, pyörii asetetun portin päällä kuten konfiguraatiossa määrittelin.


## d) Paikka remonttiin. Riko jotain asetuksia.

Tässä kohtaa lähdin hajoittamaan luotua konfiguraatiota polussa `/etc/mysql/my.cnf` eli mariadb:n konfiguraatiotiedostossa. Vaihdoin kommentiksi siansaksaa ja portiksi 1111. Kuvan alareunassa myöskin lukee tiedoston polku sekä se, että tiedosto tallennettiin.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/d8617e98-5caf-46ae-baeb-d2b35d0cbb35" />

Kuva 5. Rikottu konffitiedosto.

Tämän jälkeen ajoin taas komennon `ansible-playbook site.yml` ja varmistin, että asetukset tulivat voimaan. Kuvasta näkee, että portti millä palvelu pyörii, on 3307 ja tiedostosta on grepattu myös tekstikohta, joka sinne lisättiin ansiblen avulla. Rikottu tiedosto palautettiin siis ansiblen avulla alkuperäiseen tilaansa.


<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/9e3160d1-adc2-45f6-8e08-7b04d92d189b" />

Kuva 6. Ansiblen ajo, asetusten muuttaminen rikkomisen jälkeen.



## e) Idempotentti. Osoita, että tilasi on idempotentti.

Ajoin ansiblen heti edellisen ajon jälkeen uudelleen todistaakseni että tilani on idempotentti. Kuten kuvasta näkyy, mitään muutoksia ei toisella ajokerralla tehty `changed = 0`, joten tila on idempotentti.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/7fe0be8e-4b36-405b-b8df-cc1cbf2948c7" />
kuva 7. tila osoitettu idempotentiksi.

## f) Hae pari projektiin Moodlen keskustelusta.

Pari ilmoitettu moodlen keskustelupalstalle.

## Lähteet

Karvinen, T 2024. PHD thesis. Configuration Management of Distributed Systems over Unreliable and Hostile Networks. Kohdat 4.12.1, 4.12.2 ja 4.12.3.1 Luettu 24.11.2026. 
Ladattavissa: https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks

Karvinen, T 2026 Palvelinten Hallinta h4 Pizza Fantasia. Luettu 24.11.2026. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/

MariaDB 2026. Configuring MariaDB with Option Files, Luettu 25.11.2026. 
Luettavissa: https://mariadb.com/docs/server/server-management/install-and-upgrade-mariadb/configuring-mariadb/configuring-mariadb-with-option-files
