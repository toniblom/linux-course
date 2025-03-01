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

2025-03-01 klo 19:11 - 19:55

- Kokeile, että webbisivu toimii => toniblom.me => totea ettei näy lukon kuvaa eli salausta ei ole

![image](https://github.com/user-attachments/assets/484b37f5-6db5-450c-ab20-f5cc60750df6)

- Aloitustoimet: ssh toni@toniblom.me, sudo apt-get update, sudo systemctl restart apache2
- Asenna lego => sudo apt-get install lego
=> nämä ilman virheilmoituksia onnistuneesti

- lets encrypt staging environment -nettisivu (https://letsencrypt.org/docs/staging-environment/), täältä löytyy tarvittava komennon --server -kohtaan

https://acme-staging-v02.api.letsencrypt.org/directory

lego
--server=https://acme-staging-v02.api.letsencrypt.org/directory
--accept-tos
--email="bgx054@myy.haaga-helia.fi"
--domains="toniblom.me" --domains="www.toniblom.me"
--http --http.webroot="/home/toni/public_sites/toniblom.me/"
--path="/home/toni/lego/certificates/"
--pem
run

   => selitä tämä komento

- Hae sertifikaatti legolla, ensin testipalvelimella => 
   - path ei saa olla samassa kansiossa kuin webroot, koska webroot on julkinen, certificate pitää pitää salassa (oppitunti)
   - Testi tarvitaan, koska virheellisistä pyynnöistä tulee oikealle palvelimelle bannia

 => tuli server responded with a certificate!

 ![image](https://github.com/user-attachments/assets/e6ead68c-5fd6-4a6a-a017-d4cd51d62a45)

* ls -lR /home/toni/lego/certificates => näkyi .crt tiedosto

![image](https://github.com/user-attachments/assets/36de9833-7d61-40f0-89aa-7d97b8ef19f4)


- Kun toimii, poista/disabloi testipalvelimen --path kansio
   - mv -n /home/toni/lego/certificates/ /home/toni/lego/DISABLEDcertificates/
   - mkdir /home/toni/lego/certificates/ # tee kansio varsinaisia sertifikaatteja varten # tätä steppiä ei itse asiassa varmaankaan tarvitisi tehdä

![image](https://github.com/user-attachments/assets/c74b70c2-d568-45c2-946f-a97d2333909b)


- Hae oikea sertifikaatti, sama komento ilman --server= -argumenttia

![image](https://github.com/user-attachments/assets/513121bf-60c2-4a0c-ba02-0ca7fc98cb33)

=> server responded with a certificate => ilmeisesti onnistui

![image](https://github.com/user-attachments/assets/13a06359-3ce4-435a-a375-437037cd92ac)


- Ota sertifikaatti käyttöön name based virtual host -asetuksissa => kirjaa tarvittavat .conf -tiedostoon
   - sudoedit example.com.conf
   - sudo a2enmod ssl # miksi tämä tehdään? => laittaa ssl:n käyttöön
   - sudo systemctl restart apache2
   - sudo apache2ctl configtest # miksi tämä tehdään? => tarkistaa syntaksin

sudoedit /etc/apache2/sites-available/toniblom.me.conf

![image](https://github.com/user-attachments/assets/d0ecac2f-d6f2-4017-897a-0cb077f873ac)

/home/toni/lego/certificates/certificates/toniblom.m.crt
/home/toni/lego/certificates/certificates/toniblom.m.key

![image](https://github.com/user-attachments/assets/2fdfdf60-2115-416e-9e73-36a44bf3ac45)

![image](https://github.com/user-attachments/assets/db5fcfa6-742f-419b-a4e5-df771ef575fa)

![image](https://github.com/user-attachments/assets/8f1c2419-3f90-4cad-b34d-62e9d7845208)

 => could not reliably determine the server's fully qualified domain name => onko tällä merkitystä?

- Tee reikä palomuuriin, sudo ufw allow 443/tcp

![image](https://github.com/user-attachments/assets/dcecf68b-67ca-4f43-b34a-3c264dc167d9)


- testaa toniblom.me, myös eri koneelta

![image](https://github.com/user-attachments/assets/ff29d35d-99aa-426d-bb4e-bc60753100e1)
 
 => Lukon kuva näkyi! hiiren päälle laittaessa siinä luki verified by Let's Encrypt 

Testaan vielä isäntäkoneelta Chrome-selaimella => ei herjoja, ohjasi https-sivulle vaikka kirjoitinkin kenttään http://
=> yhteys on turvallinen!

![image](https://github.com/user-attachments/assets/56e8297b-0b8a-4ce7-a41f-99d9a0ac25ae)


## b)

2025-03-01 klo 19:55 - 20:10

https://www.ssllabs.com/ssltest/

![image](https://github.com/user-attachments/assets/d64b1d8c-2f11-46f8-a9dc-e8ee1db9497c)


Tulokset

![image](https://github.com/user-attachments/assets/ec20cebc-a62f-48b8-970e-04c37d78158d)

DNS CAA oranssina => https://blog.qualys.com/product-tech/2017/03/13/caa-mandated-by-cabrowser-forum?_ga=2.2053747.399624477.1740846347-27975123.1740846347

* CAA creates a DNS mechanism that enables domain name owners to whitelist CAs that are allowed to issue certificates for their hostnames. It operates via a new DNS resource record (RR) called CAA (type 257).
* => en ollut tehnyt mitään tällaista whitelistausta, mistä tuo oranssi väri kertoili

![image](https://github.com/user-attachments/assets/f537e42a-88cd-4fe0-9e2c-103f0ce28582)

![image](https://github.com/user-attachments/assets/e2eeeed4-4592-42d2-abfd-38363a52672b)

![image](https://github.com/user-attachments/assets/750cf97a-980b-44e6-916e-3940b1fefab9)

![image](https://github.com/user-attachments/assets/618f5d92-e6f5-41ea-8450-b271d61c8ff7)

![image](https://github.com/user-attachments/assets/a92cc02e-aa43-4e16-b1af-d385530d0da0)



## Lähteet

[1] Karvinen, T. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Luettu 2025-03-01.

[2] Internet Security Research Group. 2024-06-26. How It Works. https://letsencrypt.org/how-it-works/. Luettu 2025-03-01.

[3] Lange, Nick J. 2024-11-29. Obtain a Certificate. https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server. Luettu 2025-03-01.

[4] The Apache Software Foundation. 2025. SSL/TLS Strong Encryption: How-To. https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample. Luettu 2025-03-01.
