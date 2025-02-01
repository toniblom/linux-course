# h3 Hello Web Server - Raportti
Tekijä: Toni Blom

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h3 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [1].

## x) Name-based Virtual Host

* *Name-based virtual hosting* mahdollistaa sen, että useammalla virtuaalipalvelimella on sama IP-osoite. [2]
* Name-based virtual hosting säästää IP-osoitteiden määrää verrattuna *IP-based virtual hosting* -ratkaisuun, jossa jokaisella virtuaalipalvelimella on oma IP-osoite. [2]
* Apache HTTP-serverin voi asentaa esimerkiksi Debianissa komennolla `sudo apt-get install apache2`. [3]
* Apache HTTP-serverillä jokainen name-based virtual host tarvitsee .conf-konfiguraatiotiedostossa `<VirtualHost>` -blokin, jonka sisälle on sijoitettu ainakin `DocumentRoot` ja yleensä vähintään myös `ServerName` -direktiivi [2]. 
* Kun konfiguraatiotiedosto on valmis, seuraavat askeleet ovat virtual hostin käynnistäminen `sudo a2ensite example.com` -komennolla, Apache-serverin käynnistäminen uudelleen `sudo systemctl restart apache2` -komennolla ja itse sivuston luominen konfiguraatiotiedoston `DocumentRoot`-kohdassa määritettyyn sijaintiin. [4]


## Lähteet

[1] Karvinen, Tero. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Luettu 2025-01-25.

[2] The Apache Software Foundation. Name-based Virtual Host Support. https://httpd.apache.org/docs/2.4/vhosts/name-based.html. Luettu 2025-01-30.

[3] Karvinen, Tero: Oppitunnit 2015-01-28, Linux palvelimet -kurssi (https://terokarvinen.com/linux-palvelimet/). 

[4] Karvinen, Tero. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address. https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/. Luettu 2025-01-30.

