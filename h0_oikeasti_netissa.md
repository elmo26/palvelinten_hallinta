# h0 oikeasti netissä

## x) Lue ja tiivistä

Tiivistelmä artikkelista: Niemi 2025: H4 Tehtävät, luettavissa: https://github.com/AONCODES/linux-course/blob/main/h4.md

* Palvelimen sijainnissa tulee ottaa huomioon lainsäädäntö ja viive, jotka määräytyvät palveluntarjoajan sijainnin ja valtion perusteella.
* ssh-yhteyden jälkeen uuden käyttäjän luominen `adduser kayttaja` (Least privilidged periaate, ei käytetä pääkäyttäjää).
* Oikeuksien antaminen käyttäjän kotihakemistoon `chown `
* sudoers oikeudet uudelle käyttäjälle `add kayttaja sudo`, julkisen avaimen kopioniti käyttäjän kotihakemistoon rootin kotihakemistosta.
* Palomuurin asennus: `sudo apt install ufw`, palomuuriin reikä ssh:lle `sudo ufw allow 22/tcp`, palomuuri päälle `sudo ufw enable`
* Huom portti 22 on avattava ssh:lle ennen ufw:n päälle laittamista, jotta ei lukitse itseään ulos. [s.a. a](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu)
* Palvelimen asennuksesen jälkeen pitää muistaa sallia portti 80 tai 443 (tai millä portilla ikinä pyörittääkään weppipalvelinta), jotta liikenne pääsee läpi sivulle asti.


Tiiviste artikkelista Partanen 2025: Harjoitus 5: Nimekäs: a) Nimi, luettavissa: https://partanennn.github.io/linux-servers/practices/fifth-week.html


## Lähteet
Karvinen, T.2026 Palvelinten Hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/ Luettu 26.10.2026

Digital Ocean. 19.9.2025. How to Set Up a Firewall with UFW on Ubuntu. 
Luettavissa: 
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu 
Luettu 26.10.2026
