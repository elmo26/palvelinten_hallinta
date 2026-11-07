# h2 voileipä

## x) lue ja tiivistä

### Tiivistelmä Sudo without password (Karvinen 2026) - linkki [linkki](https://terokarvinen.com/passwordless-sudo/)

* Ansible tarvitsee root-oikeuksia orjakoneilla - yksi tapa toteuttaa on sallia sudo ilman salasanaa.
* sudoless ryhmä ja sääntö mahdollistaa sudon ajamisen ilman salasanaa.
* `%sudoless ALL = (ALL) NOPASSWD: ALL`.

### Tiivistelmä Passwordless sudo with Ansible (Karvinen 2026) - [linkki](https://terokarvinen.com/passwordless-sudo-with-ansible/)

* Paras tapa saada asiat toimimaan: ensin käsin sitten automatisoituna.
* luo ansibleen rooli, joka lisää itselleen sudoless sudo-oikeudet ja lisää ssh-avaimen authorized_keys tiedostoon ajettaessa.
* `become:true` tarkoittaa sudoa - lisää site.yml automatisoidaksesi asian.
* ensimmäisellä ajokerralla tarvitsee vielä sudoa ajettaessa: `--ask-become-password`.

### 

## a) sudoless

## b) Antero

## d) File

## e) Jotain muuta

## Lähteet

Karvinen, T 2026 palvelinten hallinta. https://terokarvinen.com/palvelinten-hallinta/ Luettu 7.11.2026

Karvinen, T 2026 Sudo without password. https://terokarvinen.com/passwordless-sudo/ Luettu 7.11.2026

Karvinen, T 2026 Passwordless Sudo with Ansible. https://terokarvinen.com/passwordless-sudo-with-ansible/ Luettu 7.11.2026

