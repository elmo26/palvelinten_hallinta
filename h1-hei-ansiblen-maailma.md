# h1 Hei Ansiblen maailma

## x) Lue ja tiivistä

### Tiivistelmä SSH public key - Login without password [(Karvinen 2026)](https://terokarvinen.com/ssh-public-key-login-without-password/)
* SSH julkisen avaimen käyttö helpottaa monien työkalujen, kuten gitin ja ansiblen käyttöä, sillä salasanaa ei tarvitse käyttää joka kerta kun haluaa tehdä jotakin,
* OpenSSH-server paketin asennus `sudo apt-get update && sudo apt-get install ssh`
* SSH daemon starttaa käynnistettäessä `sudo systemctl enable ssh` Käynnistä myös nyt `--now` flagillä
* Kokeile SSH toimivuus ottamalla yhteyden localhostille `ssh localhost`
* SSH avainten generointi `ssh-keygen`, kopioi julkinen avain hostille `ssh-copy-id host`

### Tiivistelmä Hello Ansible[(Karvinen 2026)] (https://terokarvinen.com/hello-ansible/)
* Asenna Ansible (ja pari ylimääräistä työkalua) `sudo apt-get update && sudo apt-get install ansible micro bash-completion tree` 
* 

## a)

## b)

## c)

## d)

## e)

## Lähteet
Karvinen, T 2026 Palvelinten Hallinta h1 Hei Ansiblen maailma: https://terokarvinen.com/palvelinten-hallinta/#h1-hei-ansiblen-maailma Luettu 2.11.2026
Karvinen, T 2026 SSH public key - login without password: https://terokarvinen.com/ssh-public-key-login-without-password/ Luettu 2.11.2026
Karvinen, T 2026 Hello Ansible: https://terokarvinen.com/hello-ansible/ Luettu 2.11.2026
