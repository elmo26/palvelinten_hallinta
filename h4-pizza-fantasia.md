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

Sitten tarkistin vielä asennuksen onnistumisen katsomalla mariadb:n statuksen komennolla `sudo systemctl status mariadb`
<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/3046e383-e29a-42c5-94bf-b9f8920d44de" />
Kuva 1. mariadb status.


## b) Automaatti. Automatisoi valitsemasi demonin asennus Ansiblella.

## c) Asetus. Muuta asetustiedostoa herralla ja aja ansible uudestaan. Osoita, että asetukset tulivat käyttöön.

## d) Paikka remonttiin. Riko jotain asetuksia.

## e) Idempotentti. Osoita, että tilasi on idempotentti.

## f) Hae pari projektiin Moodlen keskustelusta.

Minun ei tarvinnut hakea paria moodlesta, sillä sovin jo tunnilla kaverin kanssa että teemme projektin yhdessä. Ilmoitin parin moodlen keskustelupalstalle.

## Lähteet

Karvinen, T 2024. PHD thesis. Configuration Management of Distributed Systems over Unreliable and Hostile Networks. Kohdat 4.12.1, 4.12.2 ja 4.12.3.1 Luettu 24.11.2026. 
Ladattavissa: https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks

Karvinen, T 2026 Palvelinten Hallinta h4 Pizza Fantasia. Luettu 24.11.2026. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/
