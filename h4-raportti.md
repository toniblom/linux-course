# h4 Maailma kuulee -raportti

Tekijä: Toni Blom

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h4 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [1].

Tein tehtävän seuraavalla kone- ja ohjelmistokokoonpanolla:
* Tietokone: Lenovo Yoga Slim 7 Pro 14ACH5 -kannettava tietokone
* Prosessori: AMD Ryzen 7 5800H with Radeon Graphics 3.20 GHz
* Isäntäkäyttöjärjestelmä: Windows 10 Home, versio versio 22H2, OS build 19045.5131
* Oracle VM VirtualBox versio 7.1.4.
* Virtuaalikoneen käyttöjärjestelmä: Debian 12.9.0

## x) Lue ja tiivistä

### Opiskelijan esimerkkiraportti

Tein seuraavan tiivistelmän Susanna Lehdon pilvipalvelimen vuokraamiseen liittyvästä raportista (https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/).

**Pilvipalvelimen vuokraus ja asennus:**
* Pilvipalvelimen vuokraamisessa luodaan uusi virtuaalipalvelin, johon voi palveluntarjoajan palvelussa valita mm. haluamansa käyttöjärjestelmän ja muistin määrän.
* Pilvipalvelimelle pitää myös valita datakeskus, jossa pilvipalvelin sijaitsee. Tässä on hyvä ottaa huomioon mm. lainsäädännölliset seikat ja etäisyys palvelimen loppukäyttäjiin.
* Pilvipalvelimen autentikointimenetelmäksi voi valita SSH-avaimet tai salasanan.

**Palvelin suojaan palomuurilla:**
* Palomuurin voi asentaa komennolla `sudo apt-get install ufw`.
* Palomuuriin voi tehdä "reikiä", esim. komennolla `sudo ufw allow 22/tpc`.
* Palomuuri laitetaan päälle komennolla `sudo ufw enable`.

**Kotisivut palvelimelle:**
* Apache-webpalvelimen voi asentaa komennolla `sudo apt-get install apache2`.
* Testisivun voi poistaa esim. komennolla `echo Hello world! |sudo tee /var/www/html/index.html`
* Tämän jälkeen voi luoda käyttäjälle kansion julkisia html-sivuja varten.

**Palvelimen ohjelmien päivitys:**
* Palvelimen ohjelmat voi päivittää komennoilla:
  ```
  sudo apt-get update
  sudo apt-get upgrade
  sudo apt-get dist-upgrade
  ```

### Ensiaskeleet uudella virtuaalipalvelimella

Tein seuraavan tiivistelmän Tero Karvisen artikkelista (https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/). [3]

Virtuaalipalvelimen (virtual private server) käyttöönotossa tehdään seuraavia vaiheita:
* Palomuurin laittaminen päälle
* Oman käyttäjänimen luominen ja lisääminen sudo-ryhmään
* root-käyttäjän tilin lukitseminen ja SSH-kirjautumisen poistaminen
* Ohjelmien päivitys
* Apache-serverin asentaminen virtuaalipalvelimelle
* Julkisen DNS-nimen vuokraaminen

## a) Virtuaalipalvelimen vuokraaminen



## b) Alkutoimet virtuaalipalvelimella


## c) Apache-webpalvelimen asennus virtuaalipalvelimelle



## d) Name-based virtual host virtuaalipalvelimelle


## Lähteet

[1] Karvinen, T. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Luettu 2025-02-08.

[2] Lehto, S. 2022-02-14. Teoriasta käytäntöön pilvipalvelimen avulla (h4). https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/. Luettu 2025-02-08.

[3] Karvinen, T. 2017-09-19. First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS. https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/. Luettu 2025-02-08.

