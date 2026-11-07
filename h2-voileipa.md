# h2 voileipä

## x) lue ja tiivistä

### xkcd 149 (Munroe 2006) - [linkki](https://xkcd.com/149/)
* Sudolla saa asioita tapahtumaan

### Tiivistelmä Sudo without password (Karvinen 2026) - linkki [linkki](https://terokarvinen.com/passwordless-sudo/)

* Ansible tarvitsee root-oikeuksia orjakoneilla - yksi tapa toteuttaa on sallia sudo ilman salasanaa.
* sudoless ryhmä ja sääntö mahdollistaa sudon ajamisen ilman salasanaa.
* `%sudoless ALL = (ALL) NOPASSWD: ALL`.
* 

### Tiivistelmä Passwordless sudo with Ansible (Karvinen 2026) - [linkki](https://terokarvinen.com/passwordless-sudo-with-ansible/)

* Paras tapa saada asiat toimimaan: ensin käsin sitten automatisoituna.
* luo ansibleen rooli, joka lisää itselleen sudoless sudo-oikeudet ja lisää ssh-avaimen authorized_keys tiedostoon ajettaessa.
* `become:true` tarkoittaa sudoa - lisää site.yml automatisoidaksesi asian.
* ensimmäisellä ajokerralla tarvitsee vielä sudoa ajettaessa: `--ask-become-password`.
* 

### Ansible dokumentaatio apt `ansible-doc apt`
* Hallitsee apt paketteja (esim Debian tai Ubuntu)
* Esimerkki optioista: autoremove - jos `true`, poistaa turhat riippuvuudet. Oletuksena `false`
* 

## a) sudoless

Aloitin luomalla uuden käyttäjän ja annoin käyttäjälle nimeksi `rauski`. `sudo adduser rauski`. Loin käyttäjäle salasanan, ja tämän jälkeen loin uuden ryhmän sudoless komennolla `sudo adddgroup sudoless`.
Sitten lisäsin käyttäjän rauski sudoless-ryhmään komennolla `sudo adduser sudoless rauski`.

Avasin taustalle root shellin `sudo su root`, jos jotain menee pieleen niin roottina voi ainakin yrittää fiksata tilanteen. Sitten vaihdoin takaisin normaaliin ikkunaan.

Tämän jälkeen avasin sudoless tiedoston komennolla `sudo visudo /etc/sudoers.d/sudoless` ja lisäsin sinne seuraavan komennon `%sudoless ALL = (ALL) NOPASSWD:ALL`. Käytännössä tämä tarkoittaa
seuraavaa: `%sudoless` ryhmä sudoers (jonka tein aikaisemmin), `ALL` kaikki koneet `(ALL)` voi tulla miksi käyttähäksi tahansa `NOPASSWD` älä kysy salasanaa `ALL` kaikki komennot sallittuja.

Nyt sudon pitäisi toimia ilman salasanaa rauski-käyttäjällä. Kokeilin printata tekstiä terminaaliin sudona ja salasanaa ei kysytty joten homma toimii.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/a5af3cc9-f72b-48e9-b5c8-a0bbcb7c33c2" />
Kuva 1. sudo ilman salasanaa.


## b) Antero

## d) File

## e) Jotain muuta

## Lähteet

Karvinen, T 2026 palvelinten hallinta. https://terokarvinen.com/palvelinten-hallinta/ Luettu 7.11.2026

Karvinen, T 2026 Sudo without password. https://terokarvinen.com/passwordless-sudo/ Luettu 7.11.2026

Karvinen, T 2026 Passwordless Sudo with Ansible. https://terokarvinen.com/passwordless-sudo-with-ansible/ Luettu 7.11.2026

