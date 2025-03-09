# h6 Salataampa -raportti

_Tekijä: Toni Blom, 2025-03-02 (päivitetty 2025-03-09)_

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h6 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [^1].

Tein tehtävän seuraavalla kone- ja ohjelmistokokoonpanolla:

* Tietokone: Lenovo Yoga Slim 7 Pro 14ACH5 -kannettava tietokone
* Prosessori: AMD Ryzen 7 5800H with Radeon Graphics 3.20 GHz
* Isäntäkäyttöjärjestelmä: Windows 10 Home, versio versio 22H2, OS build 19045.5371
* Oracle VM VirtualBox versio 7.1.4 r165100 (Qt6.5.3)
* Virtuaalikoneen käyttöjärjestelmä: Debian 12.9.0

## x) Tiivistelmät lukumateriaaleista

### Let's Encrypt: Miten se toimii?

* Let's Encryptin tavoitteena on mahdollistaa selaimien luottaman sertifikaatin hankkiminen HTTPS-palvelimelle automaattisesti.
* Sertifikaatin hankkimisessa on kaksi vaihetta:
1. **Domainin validointi**
    * Todistettava Let's Encryptille, joka on Certificate Authority (CA), että palvelin hallinnoi sertifioitavaa domainia.
    * Todistaminen tapahtuu julkisen ja yksityisen avainparin avulla sekä CA:n asettaman "haasteen" suorittamisella.
2. **Sertifikaattipyyntöjen tekeminen**
    * Kun domain on validoitu, CA:lta voi pyytää uusia sertifikaatteja sekä sertifikaattien uusimista tai mitätöintiä.
    * CA jakaa tiedot uusista sertifikaateista tai mitätöinneistä julkisesti, jolloin viime kädessä selaimet saavat näistä tiedon.

(https://letsencrypt.org/how-it-works/) [^2]

### lego: Käyttö olemassa olevan web-serverin kanssa

* lego on Let's Encrypt client, jota voi käyttää Linuxin komentokehotteessa [^5].
* Jos olemassa oleva web-palvelin toimii jo portissa 80, lego-komennolle voi antaa --http-webroot -argumentin sekä web-palvelimen tiedostopolun.
* Tällöin lego ei aloita uutta palvelinta, vaan kirjoittaa CA:n antaman http-01-haasteen tiedot annettuun tiedostopolkuun.

(https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server) [^3]

### Apache: Minimaalisen SSL/TLS-salauksen konfiguraatio

* Otettaessa sertifikaattia käyttöön Apachen SSL-konfiguraation pitää minimissään sisältää seuraavat:

```
LoadModule ssl_module modules/mod_ssl.so

Listen 443
<VirtualHost *:443>
    ServerName www.example.com
    SSLEngine on
    SSLCertificateFile "/path/to/www.example.com.cert"
    SSLCertificateKeyFile "/path/to/www.example.com.key"
</VirtualHost>
```

(https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample) [^4]

## a) TLS-sertifikaatin hankkiminen Let's Encryptiltä

2025-03-01 klo 19:11 - 19:55

### Alkutoimet

Aluksi kokeilin, että aiemmin tekemäni webbisivu toimii. Menin osoitteeseen http://toniblom.me Firefox-selaimella. Tekemäni sivu tuli näkyviin, ja Firefoxissa lukon kuva oli ruksattu yli, eli TLS-salausta ei ollut käytössä.

![image](https://github.com/user-attachments/assets/484b37f5-6db5-450c-ab20-f5cc60750df6)

Kirjauduin virtuaalipalvelimelleni `ssh`-komennolla, päivitin paketinhallinnan `sudo apt-get update` -komennolla ja käynnistin Apache-webserverin uudelleen `sudo systemctl restart apache2` -komennolla. Asensin lego-ohjelman, joka on terminaalissa toimiva Let's Encrypt -klientti (https://go-acme.github.io/lego/ [^5]), `sudo apt-get install lego` -komennolla. Nämä komennot suorittuivat ilman virheilmoituksia onnistuneesti.

### Sertifikaatin hankkiminen Staging Environmentissa

Seuraavaksi ajoin lego-komennon Let's Encryptin Staging Environmentissa terminaalissa luennolla käydyn ja kurssin sivulla olleiden ohjeiden mukaisesti [^1][^6]. Staging Environmentia voi käyttää Let's Encryptin palvelun testaamiseen. Tämä on hyödyllistä, koska Let's Encrypt rajoittaa tehtävien sertifikaattipyyntöjen ja auktorisointiyritysten määrää, ja rajojen ylittyessä seuraa karenssia uusien pyyntöjen tekemiseen (https://letsencrypt.org/docs/rate-limits/) [^7]. Tarkistin vielä Let's Encryptin sivuilta Staging Environmentin palvelimen osoitteen (https://letsencrypt.org/docs/staging-environment/ [^8]), jota tarvitaan lego-komennossa.

```
lego
--server=https://acme-staging-v02.api.letsencrypt.org/directory   # Staging Environmentin palvelin
--accept-tos                                                      # Hyväksytään käyttöehdot
--email="bgx054@myy.haaga-helia.fi"                               # Sähköpostiosoite
--domains="toniblom.me" --domains="www.toniblom.me"               # Sertifioitavat domainit
--http --http.webroot="/home/toni/public_sites/toniblom.me/"      # Portissa 80 toimivan webpalvelimeni tiedostopolku
--path="/home/toni/lego/certificates/"                            # Luotavien sertifikaattien tiedostopolku
--pem                                                             # Luo .pem-tiedoston, jossa yhdistetty .key ja .crt
run
```

Komennon lippujen selitykset: (https://go-acme.github.io/lego/usage/cli/options/index.html) [^9]

Komennossa on hyvä huomata, että `--http.webroot` ja `--path` tiedostopolkujen ei tulisi olla samoja, koska webroot on julkisesti näkyvä sijainti ja sertifikaatit tulisi pitää salassa (Karvinen T., oppitunti) [^6].

Suoritin komennon ja sain ilmoituksen "Server responded with a certificate."

 ![image](https://github.com/user-attachments/assets/e6ead68c-5fd6-4a6a-a017-d4cd51d62a45)

Tarkistin vielä tiedostopolusta `/home/toni/lego/certificates/certificates/`, että .crt ja .key -tiedostot olivat olemassa.

![image](https://github.com/user-attachments/assets/36de9833-7d61-40f0-89aa-7d97b8ef19f4)

Komento siis onnistui Staging Environmentissa, joten siirryin seuraavaassa vaiheessa tekemään komennon, jolla saisin varsinaisen sertifikaatin. Siirsin ensin Staging Environmentin sertifikaatiotiedostot toiseen kansioon.

`mv -n /home/toni/lego/certificates/ /home/toni/lego/DISABLEDcertificates/`

Loin uuden `/home/toni/lego/certificates/` -kansion varsinaisia sertifikaatteja varten. Komento näytti tosin Staging Environmentissa toimineen ilman kansion etukäteen luomistakin.

`mkdir /home/toni/lego/certificates/`

![image](https://github.com/user-attachments/assets/c74b70c2-d568-45c2-946f-a97d2333909b)

### Varsinaisen sertifikaatin hankkiminen

Hain varsinaisen sertifikaatin domainilleni suorittamalla muutoin saman komennon kuin Staging Environmentissa, mutta ilman `--server`-optiota.

Sain jälleen ilmoituksen "Server responded with a certificate." Komennon suorittaminen siis onnistui.

![image](https://github.com/user-attachments/assets/13a06359-3ce4-435a-a375-437037cd92ac)

### Sertifikaatin käyttöönotto name-based virtual hostin asetuksissa

Tarkistin, että saamani sertifikaattitiedostot olivat olemassa ja katsoin samalla .crt ja .key -tiedostojen tiedostopolut, sillä näitä tarvittiin name-based virtual hostin asetuksissa.

![image](https://github.com/user-attachments/assets/d0ecac2f-d6f2-4017-897a-0cb077f873ac)

Muokkasin name-based virtual hostin konfiguraatiotiedostoa komennolla `sudoedit /etc/apache2/sites-available/toniblom.me.conf` oppitunnin ja kurssin sivujen mukaisesti [^1][^6]. Tiedostoon lisättiin name-based virtual host porttiin 443. Erona portin 80 konfiguraatioihin oli, että konfiguraatioon lisättiin kohdat SSL:n päälle kytkemisestä ja tiedostopolut sertifikaatin ja sertifikaattiavaimen tiedostoihin.

![image](https://github.com/user-attachments/assets/2fdfdf60-2115-416e-9e73-36a44bf3ac45)

Suoritin komennon `sudo a2enmod ssl`, joka kytki Apachessa ssl-toiminnot päälle. Käynnistin Apachen uudelleen `sudo systemctl restart apache2` -komennolla.

![image](https://github.com/user-attachments/assets/db5fcfa6-742f-419b-a4e5-df771ef575fa)

Suoritin komennon `sudo apache2ctl configtest`, joka tarkistaa konfiguraation syntaksin.

![image](https://github.com/user-attachments/assets/8f1c2419-3f90-4cad-b34d-62e9d7845208)

Sain ilmoituksen "Syntax OK" sekä ilmoituksen "Could not reliably determine the server's fully qualified domain name". En tiennyt oliko ilmoituksella tässä kohtaa merkitystä, joten päätin palata asiaan tarvittaessa.

Lisäsin vielä ufw-palomuuriin säännön, että yhteydenotot porttin 443 sallitaan.

![image](https://github.com/user-attachments/assets/dcecf68b-67ca-4f43-b34a-3c264dc167d9)

Testasin toniblom.me webbisivuani uudelleen.

![image](https://github.com/user-attachments/assets/ff29d35d-99aa-426d-bb4e-bc60753100e1)

Nyt URL-kentän vieressä näkyi lukon kuva ja laittaessani hiiren kursorin lukon kuvan päälle tuli näkyviin teksti "Verified by Let's Encrypt." 

Testasin vielä isäntäkoneeni Chrome-selaimella webbisivuani. Selaimesta näkyi, että yhteys on turvallinen ja varmenne on voimassa.

![image](https://github.com/user-attachments/assets/56e8297b-0b8a-4ce7-a41f-99d9a0ac25ae)


## b) Sivun testaaminen SSL Labsissa

2025-03-01 klo 19:55 - 20:10

Menin sivustolle https://www.ssllabs.com/ssltest/ ja kirjoitin domainnimeni tekstikenttään ja painoin "Submit".

Hetken kuluttua sain tuloksia palvelimeni SSL-tiedoista. Kokonaisluokitus sivustollani oli A, mikä vaikutti oikein hyvältä tässä vaiheessa.

![image](https://github.com/user-attachments/assets/ec20cebc-a62f-48b8-970e-04c37d78158d)

Raportissa oli eritelty varsin kattavasti palvelimeni tietoja ja tehtyjä testejä. Muutamia poimintoja tästä:
* Sertifikaattini on voimassa 30.5.2025 asti eli noin kolme kuukautta.
* DNS CAA -kohta oli oranssina, joten painoin "more info" tekstiä. Tämä vei minut blogitekstiin (https://blog.qualys.com/product-tech/2017/03/13/caa-mandated-by-cabrowser-forum) [^10], jossa selitettiin CAA:sta. CAA on mekanismi, jolla domainnimen omistaja voi valita mitkä Certificate Authorityt saavat myöntää sertifikaatteja domainnimelle. [10] En ollut tehnyt tällaista valintaa eli ilmeisesti mikä tahansa CA voi myöntää domainnimelleni sertifikaatteja.

![image](https://github.com/user-attachments/assets/f537e42a-88cd-4fe0-9e2c-103f0ce28582)

* Virtuaalipalvelimeni käyttää protokollia TLS 1.2 ja TLS 1.3.
* Cipher Suites -kohdassa oli oransseja rivejä. Etsin asiasta lisää tietoa. Cipher suitet ovat algoritmikokoelmia, jotka osallistuvat TLS-handshake-prosessiin serverin ja clientin yhteyden muodostamisessa. Cipher suiteja on erilaisia ja palvelimelle voi tehdä asetuksia, jotka suosivat tiettyjä versioita. (https://www.namecheap.com/blog/beginners-guide-to-tls-cipher-suites/) [^11] Ilmeisesti virtuaalipalvelimeni TLS 1.2 -protokolla käyttää joitakin cipher suiteja, jotka SSLLabsin testi luokitteli heikkolaatuisiksi.

![image](https://github.com/user-attachments/assets/e2eeeed4-4592-42d2-abfd-38363a52672b)

* Handshake Simulation -kohdassa oli testejä erilaisilla selaimilla yhteyden muodostamisesta palvelimelleni. Nämä testit näyttivät sujuneen pääosin ongelmitta, lukuun ottamatta erästä vanhaa Chromen versiota.

![image](https://github.com/user-attachments/assets/750cf97a-980b-44e6-916e-3940b1fefab9)

* Protocol Details kohdassa oli paljon tietoa ilmeisesti erilaisista haavoittuvuuksista. Näitä ei näyttänyt testeissä ilmenneen.
* Kiinnostuin Zombie POODLE -haavoittuvuudesta ja etsin siitä lisää tietoa. Löytämäni kuvaus haavoittuvuudesta oli kuitenkin erittäin tekninen, mutta ymmärsin että haavoittuvuus liittyy TLS versio 1.2:n tukemiin cipher suiteihin, jotka ovat kryptografisesti vanhanaikaisia.  Haavoittuvuus ei koske TLS versio 1.3:a. (https://www.tripwire.com/state-of-security/zombie-poodle-goldendoodle) [^12]

![image](https://github.com/user-attachments/assets/618f5d92-e6f5-41ea-8450-b271d61c8ff7)

* Raportin lopussa oli vielä tietoa mm. testin ajankohdasta ja kestosta, HTTP-statuskoodi ja virtuaalipalvelimeni tietoja.

![image](https://github.com/user-attachments/assets/a92cc02e-aa43-4e16-b1af-d385530d0da0)

## Tiivistelmä

Hankin onnistuneesti TLS-sertifikaatin webbisivulleni Let's Encryptiltä käyttämällä lego-ohjelmaa. Ennen varsinaisen sertifikaatin hankkimista testasin lego-komennon onnistuvan Let's Encryptin testipalvelimella. Tein TLS:n käyttöön tarvittavat asetukset name-based virtual hostin konfiguraatiotiedostoon, kytkin SSL:n päälle Apachessa ja tein palomuuriin säännön porttia 443 koskien. Testasin web-palvelimeni SSL-tietoja SSL Labsin sivuilla ja analysoin raportin tuloksia.

## Lähteet

[^1]: [Karvinen, T. Linux Palvelimet 2025 alkukevät.](https://terokarvinen.com/linux-palvelimet/) Luettu 2025-03-01.

[^2]: [Internet Security Research Group. 2024-06-26. How It Works.](https://letsencrypt.org/how-it-works/) Luettu 2025-03-01.

[^3]: [Lange, Nick J. 2024-11-29. Obtain a Certificate.](https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server) Luettu 2025-03-01.

[^4]: [The Apache Software Foundation. 2025. SSL/TLS Strong Encryption: How-To.](https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample) Luettu 2025-03-01.

[^5]: [Fernandez, Ludovic. 2025-02-16.](https://go-acme.github.io/lego/) Luettu 2025-03-02.

[^6]: [Karvinen, T: Oppitunnit 2015-02-25, Linux palvelimet -kurssi.](https://terokarvinen.com/linux-palvelimet/)

[^7]: [Internet Security Research Group. 2024-12-17. Rate Limits.](https://letsencrypt.org/docs/rate-limits/) Luettu 2025-03-02.

[^8]: [Internet Security Research Group. 2024-06-11. Staging Environment.](https://letsencrypt.org/docs/staging-environment/) Luettu 2025-03-02.

[^9]: [Fernandez, Ludovic. 2025-02-04. Usage.](https://go-acme.github.io/lego/usage/cli/options/index.html) Luettu 2025-03-02.

[^10]: [Ristic, Ivan. 2017-03-13. CAA Mandated by CA/Browser Forum.](https://blog.qualys.com/product-tech/2017/03/13/caa-mandated-by-cabrowser-forum) Luettu 2025-03-02.

[^11]: [Quigley, Cora. 2020-12-22. A Beginner’s Guide to TLS Cipher Suites.](https://www.namecheap.com/blog/beginners-guide-to-tls-cipher-suites/) Luettu 2025-03-02.

[^12]: [Fortra, LLC. 2019-02-04. Introducing Zombie POODLE and GOLDENDOODLE.](https://www.tripwire.com/state-of-security/zombie-poodle-goldendoodle) Luettu 2025-03-02.
