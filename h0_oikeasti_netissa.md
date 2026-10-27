# h0 oikeasti netissä

## x) Lue ja tiivistä

Tiivistelmä artikkelista: Niemi 2025: H4 Tehtävät, luettavissa: https://github.com/AONCODES/linux-course/blob/main/h4.md

* Palvelimen sijainnissa tulee ottaa huomioon lainsäädäntö ja viive, jotka määräytyvät palveluntarjoajan sijainnin ja valtion perusteella.
* ssh-yhteyden jälkeen uuden käyttäjän luominen `adduser kayttaja` (Least privilidged periaate, ei käytetä pääkäyttäjää).
* Luodun käyttäjän kotihakemiston omistajuuden muuttaminen  `chown -R kayttaja:kayttaja /home/kayttaja/.ssh/`. Myös huomiona että ssh estää sen käytön
  mikäli ssh-tiedostojen oikeudet ovat liian avoimet.
* sudoers oikeudet uudelle käyttäjälle `add kayttaja sudo`, julkisen avaimen kopioniti käyttäjän kotihakemistoon rootin kotihakemistosta.
* Palomuurin asennus: `sudo apt install ufw`, palomuuriin reikä ssh:lle `sudo ufw allow 22/tcp`, palomuuri päälle `sudo ufw enable`
* Huom portti 22 on avattava ssh:lle ennen ufw:n päälle laittamista, jotta ei lukitse itseään ulos. [s.a. a](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu)
* Palvelimen asennuksesen jälkeen pitää muistaa sallia portti 80 tai 443 (tai millä portilla ikinä pyörittääkään weppipalvelinta), jotta liikenne pääsee läpi sivulle asti.


Tiivistelmä artikkelista Partanen 2025: Harjoitus 5: Nimekäs: a) Nimi, luettavissa: https://partanennn.github.io/linux-servers/practices/fifth-week.html

* Namecheap tarjoaa halpoja domain-nimiä, muitakin kauppapaikkoja on.
* Domainien kohdalla kannattaa katsoa kyseisen TLD:n (top level domain) koskevan säännöt ja palveluehdot,
  sillä ne saattavat erota toisistaan domainista riippuen. (oma lisäykseni)
* Domaini saadaan osoittamaan haluamaamme ip-osoitteeseen namecheapin Domain list näkymästä, siten että määritellään
  A Record -kohtaan ip-osoite, johon haluamme viitata.

## a) Paikallista

Aloitin debian 13 *trixien* asennuksen lataamalla sen iso-tiedoston osoitteesta https://www.debian.org/download .
Virtualisointiympäristö missä tämä kone tulee pyörimään, on linux-pohjainen, vapaan lähdekoodin virtual machine manager. Hypervisorina toimii KVM,
josta olen pitänyt. Tälle asialle nyt ei ole muuta syytä kuin se, että läppärin host-käyttiksenä on linux mint, 
ja virtual machine manager + KVM on toiminut hyvin.

Asennus sujui varsin normaalisti, allokoin virtuaalikoneelle 6Gb muistia ja 4 prosessoria sekä 30Gb levytilaa. Vaihdoin firmwaren BIOS -> UEFI, jotta kone toimii ongelmitta. Asiaan sen syvällisemmin perehtymättä,
minulla on joskus allut tämän asetuksen kanssa ongelmia virtuaalikoneiden kanssa. UEFI on uudempi firmware. Graafisessa asennuksessa näppäimistön kielen sekä maantieteellisen sijainnin vaihdoin. Tämän jälkeen asetin pääkäyttäjän ja normaalikäyttäjän käyttäjänimet sekä salasanat ja muutoin käytin oletusasetuksia asennuksessa. 

Asennus onnistui ja lisäsin peruskäyttäjälleni sudo-oikeudet. Ensin vaihdoin käyttäjää `su root` ja sitten ajoin komennon `/usr/sbin/usermod -aG sudo "kayttaja"` usermod ei komentona toiminut, 
joten ajoin koko polun. Aloin selvittämään että miksi usermod ei toiminut, ja selvisi että debian ei sisällytä /usr/sbin/ polussa olevia binäärejä normaalikäyttäjäien (ei root) `$PATH` [muuttujaan](https://en.wikipedia.org/wiki/PATH_(variable)). Vaikka olin pääkäyttäjänä ajamassa komentoja, pelkkä `su` vaihtaa vain käyttäjän, mutta säilyttää edellisen käyttäjän ympäristön. Kokeilin vastaavaa komennolla `su - root` (joka lataa käyttäjän oman ympäristön), ja usermod toimi normaalisti.

Vielä lopuksi ajoin komennon `sudo apt update && sudo apt full-upgrade -y` järjestelmän päivittämiseksi.


## b) VPS
Kurssin ensimmäisellä oppitunnilla 22.10.2026 oli tunnin lopussa aikaa vuokrata vps. Päädyin itse UpCloudiin, sillä kysessä on suomalainen
firma, ja itse palvelimen sai myös fyysisesti Suomesta. Myöskin hinta oli aikalailla samalla tasolla kuin muilla palveluntarjoajilla. Kuten kuvasta näkyy, palvelimen resurssien osalta menin halvimmalla mahdollisella vaihtoehdolla. Ehdin tunnilla myös generoimaan ssh-avainparin, liittämään julkisen avaimen palvelimeen ja ottamaan palvelimelle ssh-yhteyden pääkäyttäjänä.

<img width="1618" height="540" alt="image" src="https://github.com/user-attachments/assets/3eb88e33-2f98-4b7b-9906-e4ff149f772a" />

## c) Alkutoimet

Otin ssh-yhteyden palvelimeeni pääkäyttäjänä komennolla `ssh root@ip` ja tein itselleni uuden normaalikäyttäjän, ja lisäsin samalla sudo-oikeuden käyttäjälle komennoilla `adduser elmo && adduser elmo sudo`.
Tämän jälkeen ajoin komennot `cp -R .ssh /home/elmo/`, `chown -R elmo:elmo /home/elmo`. Ensimmäinen komento kopioi .ssh-kansion elmo-käyttäjän kotihakemistoon, jälkimmäinen komento vaihtaa /home/elmo kansion ja sen sisältöjen omistuksen käyttäjälle elmo ja ryhmään elmo. Tämän jälkeen kokeilin poistua serveriltä ja ottaa ssh-yhteyden suoraan elmo-käyttäjälle, joka onnistui hyvin.

Päivitin koneen ja poistin turhat riippuvuudet komennolla `sudo apt update && sudo apt full-upgrade -y && sudo apt autoremove --purge && sudo apt clean`. Tämän jälkeen asensin palomuurin komennolla 
`sudo apt install ufw`, avasin ssh:n käytön (tcp/22) komennolla `sudo ufw allow ssh` ja ajoin palomuurin käyttöön komennolla `sudo ufw enable`. Tämän jälkeen boottasin koneen komennolla `sudo reboot`

Lopuksi lukitsin root-tilin (salasana) kirjautumisen komennolla `sudo usermod --lock root`

## d) Weppipalvelin

Asensin Apache-palvelimen komennolla `sudo apt install apache2`



## Lähteet
Karvinen, T.2026 Oppitunti 22.10: Palvelinten Hallinta
Karvinen, T.2026 Palvelinten Hallinta, Luettavissa: https://terokarvinen.com/palvelinten-hallinta/ Luettu 26.10.2026

Digital Ocean. 19.9.2025. How to Set Up a Firewall with UFW on Ubuntu. 
Luettavissa: 
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu 
Luettu 26.10.2026
Wikipedia PATH(variable).s.a. a Luettavissa: https://en.wikipedia.org/wiki/PATH_(variable) Luettu 27.10.2026
