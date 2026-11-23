#h4 Pizza Fantasia

## x) Lue ja tiivistä

## a) Räpylä Asenna itse valitsemasi demoni käsin. Jokin muu kuin tunnilla tai kotitehtävissä aiemmin asennettu, eli ei apache, ngninx eikä openssh-server. Kuten aina, testaa lopputulos.

## b) Automaatti Automatisoi valitsemasi demonin asennus Ansiblella.

## c) Asetus Muuta asetustiedostoa herralla (master, "control node") ja aja ansible uudestaan. Osoita, että asetukset tulivat käyttöön.

## d) Paikka remonttiin. Riko jotain asetuksia. Voit esimerkiksi poistaa demonin 'sudo apt-get purge foobar' (purge poistaa myös asetustiedostoja), poistaa asennuksen yhteydessä tulevan kansion (sudo rm -r /etc/foobar/ # vaarallista) tms. Ja sitten ajaa ansible-roolisi uudestaan ja todeta, että se korjaa tilanteen.

## e) Idempotentti. Osoita, että tilasi on idempotentti.

## f) (Vain Pasilan ryhmä -3015) Hae pari projektiin Moodlen keskustelusta. (Tästä alakohdasta f ei tarvitse tehdä vaiheittaista teknistä raporttia, riittää kun toteat, että pari on hankittu.)

## Lähteet

Karvinen, T 2024. PHD thesis. Configuration Management of Distributed Systems over Unreliable and Hostile Networks kohdat 4.12.1, 4.12.2 ja 4.12.3.1 Luettu 23.11.2026. 
Luettavissa: https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks

