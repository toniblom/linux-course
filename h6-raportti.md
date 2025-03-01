# h6 Salataampa -raportti

Tekijä: Toni Blom

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h5 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [1].

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

(https://letsencrypt.org/how-it-works/) [2]

### lego: Käyttö olemassa olevan web-serverin kanssa

* lego on Let's Encrypt client, jota voi käyttää Linuxin komentokehotteessa.
* Jos olemassa oleva web-palvelin toimii jo portissa 80, lego-komennolle voi antaa --http-webroot -argumentin sekä web-palvelimen tiedostopolun.
* Tällöin lego ei aloita uutta palvelinta, vaan kirjoittaa CA:n antaman http-01-haasteen tiedot annettuun tiedostopolkuun.

(https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server) [3]

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

(https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample) [4]

## a)



## b)




## Lähteet

[1] Karvinen, T. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Luettu 2025-03-01.

[2] Internet Security Research Group. 2024-06-26. How It Works. https://letsencrypt.org/how-it-works/. Luettu 2025-03-01.

[3] Lange, Nick J. 2024-11-29. Obtain a Certificate. https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server. Luettu 2025-03-01.

[4] The Apache Software Foundation. 2025. SSL/TLS Strong Encryption: How-To. https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample. Luettu 2025-03-01.
