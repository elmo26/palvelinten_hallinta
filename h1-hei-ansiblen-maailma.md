# h1 Hei Ansiblen maailma

## x) Lue ja tiivistä

### Tiivistelmä SSH public key - Login without password [(Karvinen 2026)](https://terokarvinen.com/ssh-public-key-login-without-password/)
* SSH julkisen avaimen käyttö helpottaa monien työkalujen, kuten gitin ja ansiblen käyttöä, sillä salasanaa ei tarvitse käyttää joka kerta kun haluaa tehdä jotakin,
* OpenSSH-server paketin asennus `sudo apt-get update && sudo apt-get install ssh`
* SSH daemon starttaa käynnistettäessä `sudo systemctl enable ssh` Käynnistä myös nyt `--now` flagillä
* Kokeile SSH toimivuus ottamalla yhteyden localhostille `ssh localhost`
* SSH avainten generointi `ssh-keygen`, kopioi julkinen avain hostille `ssh-copy-id host`

### Tiivistelmä Hello Ansible [(Karvinen 2026)](https://terokarvinen.com/hello-ansible/)
* Asenna Ansible (ja pari ylimääräistä työkalua) `sudo apt-get update && sudo apt-get install ansible micro bash-completion tree` 
* Tehdään kansio `ansible` konfiguraatiota varten ja sinne tiedosto `hosts.ini`, jonne lisätään kontrolloitavat hostit. localhost on hyvä testatessa.
* Jos ansible valittaa ajettaessa pythonista, lisätään hosts.ini tiedostoon python kääntäjän polku `/usr/bin/python3` kaikille koneille, ja ansible ei enää valita.
* Ansiblen konfiguraatiotiedostoon `ansible.cfg` voi lisätä oletukseksi `hosts.ini` tiedoston, jolloin sitä ei tarvitse kirjoittaa jokaisella ajokerralla.
* `Site.yml` tiedostoon lisätään roolit koneryhmittäin - eli mitkä roolit halutaan millekkin koneille.
* Rooleille oma kansio `roles`, jonka sisällä rooleille omat kansionsa. Esim `roles/apache2`
* tree-ohjelma on kätevä piirtämään tiedostorakenteen loogisesti terminaaliin. Sillä on hyvä aloittaa ongelmien etsiminen, kun joku ei toimi tai tiedosto x on unohtunut.

## a) Paikallista Tehty tehtävässä h0 oikeasti netissä, kohdassa a) Linkki: https://github.com/elmo26/palvelinten_hallinta/blob/main/h0_oikeasti_netissa.md

## b) Sshecrets
Olin tehnyt tämän tunnilla joten pysäytin ensin ssh demonin komennolla `sudo systemctl stop ssh` ja varmistin, että se on pois päältä katsomalla statuksen komennolla `systemctl status ssh`.
Poistin ssh-demonin komennolla `sudo apt remove openssh-server`. Tämän jälkeen asensin sen takaisin komennolla `sudo apt install openssh-server`.

## c)

## d)

## e)

## Lähteet
Karvinen, T 2026 Palvelinten Hallinta h1 Hei Ansiblen maailma: https://terokarvinen.com/palvelinten-hallinta/#h1-hei-ansiblen-maailma Luettu 2.11.2026

Karvinen, T 2026 SSH public key - login without password: https://terokarvinen.com/ssh-public-key-login-without-password/ Luettu 2.11.2026

Karvinen, T 2026 Hello Ansible: https://terokarvinen.com/hello-ansible/ Luettu 2.11.2026
