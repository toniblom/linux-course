# h3 Hello Web Server - Raportti
Tekijä: Toni Blom

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h3 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [1].

## x) Name-based Virtual Host

* *Name-based virtual hosting* mahdollistaa sen, että useammalla virtuaalipalvelimella on sama IP-osoite. [2]

* Name-based virtual hosting säästää IP-osoitteiden määrää verrattuna *IP-based virtual hosting* -ratkaisuun, jossa jokaisella virtuaalipalvelimella on oma IP-osoite. [2]

* Apache HTTP-palvelimen voi asentaa esimerkiksi Debianissa komennolla `sudo apt-get install apache2`. Konfiguraatiotiedostojen sijainti on `/etc/apache2/sites-available/` -kansiossa. [3]

* Apache HTTP-palvelimella jokainen name-based virtual host tarvitsee .conf-konfiguraatiotiedostossa `<VirtualHost>` -blokin, jonka sisälle on sijoitettu ainakin `DocumentRoot` ja yleensä vähintään myös `ServerName` -direktiivi [2].

* Kun konfiguraatiotiedosto on valmis, seuraavat askeleet ovat virtual hostin käynnistäminen `sudo a2ensite example.com` -komennolla, Apache-serverin käynnistäminen uudelleen `sudo systemctl restart apache2` -komennolla ja itse sivuston luominen konfiguraatiotiedoston `DocumentRoot`-kohdassa määritettyyn sijaintiin. [4]

## a) localhost

2025-02-01, klo 20:15 - 20:20

Olin asentanut Apache HTTP-palvelimen virtuaalikoneelleni jo aikaisemmin ja tehnyt edellä kuvattuja konfiguraatioita ja yksinkertaisen html-sivun. Avasin internetselaimella osoitteen http://localhost/, jolloin tekemäni sivu tuli näkyviin.

![image](https://github.com/user-attachments/assets/f41bb18b-e0a8-4cc0-804e-1d47222d1da4)

## b) Lokit

2025-02-01, klo 20:20 - 20:35

Suoritin terminaalissa komennot `sudo tail /var/log/apache2/access.log` ja `sudo tail /var/log/apache2/error.log`.

![image](https://github.com/user-attachments/assets/cb6ccd68-9545-4cc7-aaee-af5014bfbf04)

![image](https://github.com/user-attachments/assets/257ab9e2-eeb1-453a-8604-352708489dc2)

Suoritin `sudo tail -f /var/log/apache2/access.log` -komennon ja päivitin http://localhost/ -sivun internetselaimesta, lokitiedostoon ei kuitenkaan ilmestynyt mitään uutta.

## Lähteet

[1] Karvinen, Tero. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Luettu 2025-01-25.

[2] The Apache Software Foundation. Name-based Virtual Host Support. https://httpd.apache.org/docs/2.4/vhosts/name-based.html. Luettu 2025-01-30.

[3] Karvinen, Tero: Oppitunnit 2015-01-28, Linux palvelimet -kurssi (https://terokarvinen.com/linux-palvelimet/). 

[4] Karvinen, Tero. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address. https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/. Luettu 2025-01-30.

