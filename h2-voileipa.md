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

Tässä oli tehtävänä tehdä käyttäjä, jolla voi ajaa komentoja sudona ilman salasanaa.

Aloitin luomalla uuden käyttäjän nimeltä rauski `sudo adduser rauski`. Loin käyttäjäle salasanan, ja tämän jälkeen loin uuden ryhmän nimeltä sudoless komennolla `sudo adddgroup sudoless`.
Sitten lisäsin käyttäjän rauski sudoless-ryhmään komennolla `sudo adduser sudoless rauski`.

Tämän jälkeen avasin sudoless tiedoston komennolla `sudo visudo /etc/sudoers.d/sudoless` ja lisäsin sinne seuraavan komennon `%sudoless ALL = (ALL) NOPASSWD:ALL`. Käytännössä tämä tarkoittaa
seuraavaa: `%sudoless` ryhmä sudoers (jonka tein aikaisemmin), `ALL` kaikki koneet `(ALL)` voi tulla miksi käyttähäksi tahansa `NOPASSWD` älä kysy salasanaa `ALL` kaikki komennot sallittuja.

Nyt sudon pitäisi toimia ilman salasanaa rauski-käyttäjällä. Vaihdoin käyttäjää ja kokeilin printata tekstiä terminaaliin sudona ja salasanaa ei kysytty joten homma toimii.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/a5af3cc9-f72b-48e9-b5c8-a0bbcb7c33c2" />
Kuva 1. sudo ilman salasanaa.


## b) Antero

Aloitin tehtävän katsomalla, että nykyinen h1 tehtävässä tehty konfiguraatio toimii, ajamalla komennon `ansible-playbook site.yml` kaksi kertaa. Kaikki näytti olevan kunnossa, ensimmäisellä kerralla muutos
tuli kun olin poistanut `/tmp/hello-ansible` kansion.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/9d498495-d112-4784-9837-a1ea88b3b577" />

Kuva 2. Aloitustilanne ja nykyinen ansiblen konfiguraatio.

Tämän jälkeen tein uuden roolin, johon on tarkoitus automatisoida salasanaton sudon käyttö rauski-käyttäjän alle .

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/917f1d13-1a2c-43fd-9955-2b882f7fa675" />

Kuva 3. Uusi rooli

Sitten aloin tekemään rauski-roolin alle `main.yml` tiedostoon automatisoitua toteutusta samasta asiasta mitä a-kohdassa tein manuaalisesti: kopioin pohjan toteutukseen [Passwordless Sudo with Ansible artikkelista](https://terokarvinen.com/passwordless-sudo-with-ansible/). Kuvassa 4. näkyy toteutus: lyhykäisyydessään ryhmänä on sudoless,
käyttäjä rauski, käyttäjän ryhmät sudoless, sudo ja adm. Ssh authorized_key on käyttäjän "tauski" eli oman lokaalin käyttäjäni avain. 

Huomiona että tämä mahdollistaa sen, että käyttäjä tauski pystyy kontrolloimaan ansiblen kautta etäkoneita, joilla käyttäjä "rauski" tekee ssh:n yli asioita. Eli siis käyttäjän tauski julkinen avain laitetaan "rauskin" authorized_keys tiedostoon. Nyt kun kohteena on localhost, kesti itsellänikin hetki miettiä tämä toteutus läpi että ymmärsin mitä tarkalleen tapahtuu.

Sitten pistin  `site.yml` tiedostoon `become=true` ja roolin `rauski`. Become = true tarkoittaa, että käytetään sudoa, ja rooli lisättiin normaalisti.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/7bfd8158-3090-408e-bb16-d34f60b6f355" />

Kuva 4. rauski-roolin sisältö.


Lopuksi muutin vielä `hosts.ini` tiedostoon `localhost` tilalle `rauski@localhost`, jotta ansible ajaa oikealla käyttäjällä. Alla näkyy kuvassa toisen kerran ajettu ansible-playbook.
Ensimmäisellä kerralla ansible-playbook site.yml piti ajaa `--ask-become-pass` parametrin kanssa, koska sudoa ei oltu vielä siinä kohdassa automatisoitu, ja sudon automatisoiminen vaati sudoa itsessään. Jätin kuvan 
ensimmäisestä ajosta pois, sillä en halunnut tehdä raportista aivan liian pitkää.


<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/dcaccc7d-dd53-4729-a2ca-48acdf6f69de" />
Kuva 5. ansible-playbookin ajo uusilla asetuksilla toisen kerran.


## c) Package

Aloitin tekemällä oman polun ja roolin paketeille polkuun `roles/paketit/tasks/main.yml`. Määrittelin tiedostoon `main.yml` `- name Asenna paketit` task nameksi. Sitten määrittelin `ansible.builtin.apt: name: - apache2 - curl` eli asennettavat paketit, sekä `update cache: True state: latest`. 

Update cache tekeee saman kuin sudo apt-get update (ansiblen dokumentaatio, ansible-doc apt) ja state: latest päivittää viimeisimpään saatavaan pakettiin.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/4148fb96-141f-4e0b-a54b-f415e1cdf282" />
Kuva 6. hakemistorakenne ja uuden roolin sisältö.


Tämän jälkeen poistin curlin ja apache2 manuaalisesti komennolla `sudo apt-get remove curl -y && sudo apt-get remove apache2 -y` , jotta jotakin muutoksia tulisi kun ajan ansible-playbookin.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/0b34e99a-3ac1-4232-bae0-2286283f754b" />
Kuva 7. ansible-playbookin ajo ja muutosten varmistus.

Kuten kuvasta näkyy, muutos tapahtui ja curl ja apache2 on asennettuna. Ne siis asentuivat ansible-playbookin ajon yhteydessä.



## d) File

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/6e07598c-f591-4459-8387-9ec01a4dadf6" />
Kuva 8. Monirivinen tekstitiedosto, omistaja ryhmä ja oikeudet.

## e) Jotain muuta

## Lähteet

Karvinen, T 2026 palvelinten hallinta. https://terokarvinen.com/palvelinten-hallinta/ Luettu 7.11.2026

Karvinen, T 2026 Sudo without password. https://terokarvinen.com/passwordless-sudo/ Luettu 7.11.2026

Karvinen, T 2026 Passwordless Sudo with Ansible. https://terokarvinen.com/passwordless-sudo-with-ansible/ Luettu 7.11.2026

