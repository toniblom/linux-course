# h3 Hello Web Server - Raportti
Tekijä: Toni Blom

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h3 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [^1].

Tein tehtävän seuraavalla kone- ja ohjelmistokokoonpanolla:
* Tietokone: Lenovo Yoga Slim 7 Pro 14ACH5 -kannettava tietokone
* Prosessori: AMD Ryzen 7 5800H with Radeon Graphics 3.20 GHz
* Isäntäkäyttöjärjestelmä: Windows 10 Home, versio versio 22H2, OS build 19045.5131
* Oracle VM VirtualBox versio 7.1.4.

## x) Name-based Virtual Host

* *Name-based virtual hosting* mahdollistaa sen, että useammalla virtuaalipalvelimella on sama IP-osoite. [^2]

* Name-based virtual hosting säästää IP-osoitteiden määrää verrattuna *IP-based virtual hosting* -ratkaisuun, jossa jokaisella virtuaalipalvelimella on oma IP-osoite. [^2]

* Apache HTTP-palvelimen voi asentaa esimerkiksi Debianissa komennolla `sudo apt-get install apache2`. Konfiguraatiotiedostojen sijainti on `/etc/apache2/sites-available/` -kansiossa. [^3]

* Apache HTTP-palvelimella jokainen name-based virtual host tarvitsee .conf-konfiguraatiotiedostossa `<VirtualHost>` -blokin, jonka sisälle on sijoitettu ainakin `DocumentRoot` ja yleensä vähintään myös `ServerName` -direktiivi [^2].

* Kun konfiguraatiotiedosto on valmis, seuraavat askeleet ovat virtual hostin käynnistäminen `sudo a2ensite example.com` -komennolla, Apache-serverin käynnistäminen uudelleen `sudo systemctl restart apache2` -komennolla ja itse sivuston luominen konfiguraatiotiedoston `DocumentRoot`-kohdassa määritettyyn sijaintiin. [^4]

## a) localhost

2025-02-01, klo 20:15 - 20:20

Olin asentanut Apache HTTP-palvelimen virtuaalikoneelleni jo aikaisemmin ja tehnyt edellä kuvattuja konfiguraatioita ja yksinkertaisen html-sivun. Avasin internetselaimella osoitteen http://localhost/, jolloin tekemäni sivu tuli näkyviin.

![image](https://github.com/user-attachments/assets/f41bb18b-e0a8-4cc0-804e-1d47222d1da4)

## b) Lokit

2025-02-01, klo 20:20 - 20:35

Suoritin terminaalissa komennot `sudo tail /var/log/apache2/access.log` ja `sudo tail /var/log/apache2/error.log`.

![image](https://github.com/user-attachments/assets/cb6ccd68-9545-4cc7-aaee-af5014bfbf04)

Analysoidaan error.log -tiedoston viimeistä riviä:
* **[Sat Feb 01 20:16:20.020103 2025]** on lokikirjauksen päivämäärä ja kellonaika.
* **[core:notice]** tarkoittaa virheilmoituksen tuottanutta moduulia (core) ja ilmoituksen "vakavuusastetta" (notice).
* **[pid 675:tid 675]** ovat process ID ja thread ID prosessille, joka kohtasi lokikirjauksen tilanteen.
* Loppuosa on kuvaus ilmoituksesta/virheestä. [^5]

![image](https://github.com/user-attachments/assets/257ab9e2-eeb1-453a-8604-352708489dc2)

Analysoidaan access.log -tiedoston ensimmäistä riviä:
* **127.0.0.1** on asiakkaan IP-osoite, joka teki pyynnön palvelimelle.
* **"-"** tarkoittaa, että pyydettyä tietoa ei ollut saatavilla.
  * Ensimmäinen "-" on asiakkaan RFC 1413 -identiteetti. Tätä tietoa ei yleensä lokissa käytetä.
  * Toinen "-" on pyynnön tekijän userid. Jos dokumenttia ei ole suojattu salasanalla, se on "-".
* **[28/Jan/2025: 19:48:34 +0200]** on pyynnön saapumisaika.
* **"GET /favicon.ico HTTP/1.1"** on asiakkaan tekemä pyyntö, jossa HTTP/1.1 on käytetty protokolla.
* **404** on palvelimen asiakkaalle lähettämä statuskoodi. 4-alkuinen koodi tarkoittaa asiakkaan aiheuttamaa virhettä.
* **487** on asiakkaalle palautetun objektin koko.
* **"http://localhost/"** on sivu, jolta on linkki /favicon.ico -sijaintiin. Englanniksi tästä käytetään sanaa Referer.
* **"Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"** on asiakkaan selaimen antamaa tietoa itsestään. [^5]

access.log -tiedostossa näkyi lähinnä vanhempia lokitapahtumia. Suoritin `sudo tail -f /var/log/apache2/access.log` -komennon ja päivitin http://localhost/ -sivun painamalla Shift-näppäintä ja reload-nappia internetselaimesta, lokitiedostoon ei kuitenkaan ilmestynyt mitään uutta.

## c) Uusi name-based virtual host

2025-02-02, klo 17:37 - 18:10

Loin lähteitä ([^3], [^4]) apuna käyttäen uuden name-based virtual hostin.

Suoritin komennon `sudoedit /etc/apache2/sites-available/hattu.example.com.conf`.

Kirjoitin konfiguraatiotiedostoon seuraavaa:
```
<VirtualHost *:80>
 ServerName hattu.example.com
 ServerAlias www.hattu.example.com
 DocumentRoot /home/toni/publicsites/hattu.example.com
 <Directory /home/toni/publicsites/hattu.example.com>
   Require all granted
 </Directory>
</VirtualHost>
```
Suoritin komennon `sudo a2ensite hattu.example.com.conf`, joka onnistui ongelmitta.

![image](https://github.com/user-attachments/assets/da0c8a4e-72d4-43c0-b4c8-997f890d45b0)

Katsoin `/etc/apache2/sites-enabled/` kansion sisältöä, jossa oli kaksi tiedostoa, joten deaktivoin ylimääräisen komennolla `sudo a2dissite toni.example.com.conf`.

![image](https://github.com/user-attachments/assets/2067257b-9b9e-4a58-9662-4ba442e5d30f)

Suoritin komennon `sudo systemctl restart apache2`.

Loin hakemistoon /home/toni/publicsites/ uuden hattu.example.com -kansion ja sen sisään index.html -tiedoston.

![image](https://github.com/user-attachments/assets/e079cb76-1ce0-4ef1-8094-b959d528eb4f)

Latasin internetselaimella http://localhost/ -osoitteen. Uusi sivu näkyi selaimessa.

![image](https://github.com/user-attachments/assets/145584b9-ae7f-42e7-b0ec-504e4019d201)

## e) Validi HTML5 sivu

2025-02-02, klo 18:13 - 18:30

Muokkasin hattu.example.com -kansioni index.html -tiedostoa sivulta https://terokarvinen.com/2012/short-html5-page/ löytyvää mallia hyväksi käyttäen. Käytin sivua https://validator.w3.org/#validate_by_input HTML:n validoimiseen.

![image](https://github.com/user-attachments/assets/b0ed58a9-e4d7-4e80-870a-84fc65869a20)

doctype-kohdassa oli ylimääräinen väli, joten poistin tämän, jolloin virheilmoitukset poistuivat (varoitusta lukuun ottamatta). Latasin sivuni internetselaimessa.

![image](https://github.com/user-attachments/assets/d16d4804-dd13-4586-ba37-2e46b6c780e4)

## f) curl-komento

2025-02-02, klo 18:35 - 19:18

Katsoin ensin `man curl` -komennolla mitä `curl` -komento ja sen `-l` -asetus tekevät. `curl` -komento siirtää dataa palvelimelta tai palvelimelle ja käyttää useita eri protokollia. `curl -l` -komento pakottaa tuloksessa näkyvään vain nimet. Manuaalissa `-l` optiosta oli esimerkkejä FTP- ja POP3-protokolliin liittyen, joten `-l` option käyttö esim. HTTP-protokollan kanssa tai yhteys tehtävänantoon ei vielä selvinnyt. [^8]

Kokeilin `curl http://localhost/` -komentoa, jolloin näkyviin tuli aiemmin tekemäni sivun html-tiedoston sisältö.

![image](https://github.com/user-attachments/assets/8612c2fe-5375-4bd1-937f-42d5e3ed42ff)

Etsin vielä tietoa termistä "response header", joka on HTTP-otsake (HTTP header), jota voidaan käyttää HTTP-vastauksessa antamaan lisätietoja vastauksen kontekstista. Esimerkkejä response headereista ovat esim. Age, Location ja Server. Puheessa kaikista HTTP headereista puhutaan usein response headereina, vaikka määritelmällisesti näin ei ole. [^9]

Kokeilin `curl -l http://localhost/` -komentoa, jolloin näkyviin tuli sama tulos kuin komennolla ilman `-l` optiota. Response headereita ei tullut näkyviin.

2025-02-02, klo 20:20 - 20:38

`curl`-komennon man-sivulta löysin, että `-L` -optioon liitettäessä `-i` optio näyttää otsakkeet. [^8]

![image](https://github.com/user-attachments/assets/4c7be9ea-a17d-44ec-b754-9287c2a26b90)

`curl -L -i http://localhost/` -komennolla näkyi otsakkeita, kuten päivämäärä ja kellonaika, Apache-palvelimen versio ja viimeisin muokkausajankohta.

## Lähteet

[^1]: [Karvinen, Tero. Linux Palvelimet 2025 alkukevät.](https://terokarvinen.com/linux-palvelimet/) Luettu 2025-02-01.

[^2]: [The Apache Software Foundation. Name-based Virtual Host Support.](https://httpd.apache.org/docs/2.4/vhosts/name-based.html) Luettu 2025-01-30.

[^3]: [Karvinen, Tero: Oppitunnit 2015-01-28, Linux palvelimet -kurssi.](https://terokarvinen.com/linux-palvelimet/)

[^4]: [Karvinen, Tero. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address.](https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/) Luettu 2025-02-02.

[^5]: [The Apache Software Foundation. Log Files.](https://httpd.apache.org/docs/2.4/logs.html) Luettu 2025-02-02.

[^6]: [Karvinen, Tero. Short HTML5 page.](https://terokarvinen.com/2012/short-html5-page/) Luettu 2025-02-02.

[^7]: [World Wide Web Consortium. W3C®. Markup Validation Service.](https://validator.w3.org/#validate_by_input) Luettu 2025-02-02.

[^8]: [Stenberg, D. et al. curl man page.](https://curl.se/docs/manpage.html) Luettu 2025-02-02.

[^9]: [Mozilla Corporation. Response header.](https://developer.mozilla.org/en-US/docs/Glossary/Response_header) Luettu 2025-02-02.
