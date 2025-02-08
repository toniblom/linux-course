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

2025-02-08, klo 20:03 - 20.45

* Menin osoitteeseen education.github.com/pack, ja painoin kohdasta "Get access by connecting your GitHub account on DigitalOcean".

![image](https://github.com/user-attachments/assets/44d999c7-4680-45b8-ab1b-33c3a7db50a3)

* Valitsin DigitalOceanin sivulta kohdan "Sign In with GitHub" => "Autrhorize digitalocean" => "An unknown error has occurred." => En ollut rekisteröitynyt käyttäjäksi.
* Valitsin kohdan Sign up => Sign Up with GitHub => Vastailin erilaisiin kysymyksiin => täytin pankkikorttini ja muut tiedot => Save => Tapahtui käsittelyvirhe => Sign Out
* En osaa sanoa mistä ongelma johtui.
* Kokeilen UpCloudia => upcloud.com => sign up => Tilin luominen onnistui
* Verify account => Korttitietoni hyväksyttiin
* Päätin laittaa tililleni vielä kaksivaihetunnistuksen => Activate 2FA
* Painoin kohdasta Server List => Deploy server

![image](https://github.com/user-attachments/assets/d3ac9802-11b0-498f-96bf-10d3316ee1cb)

* Valitsin Location = FI-HEL1
* Plan =  Developer

![image](https://github.com/user-attachments/assets/f51f3a1b-6a1e-43c8-a0f0-17f84aebc4e7)

* Storage kohta => en tehnyt muutoksia
* Automated backups => en laittanut päälle
* Operating system => Degian GNU/Linux 12 (Bookworm)
* Network -kohta => En tehnyt muutoksia
* Optionals => En valinnut mitään
* Login Method => SSH keys => Tässä vaiheessa siirryin virtuaalikoneelleni

* Save the SSH key
* Initialization script => En laittanut mitään
* Server configuration => vaihdoin nimen => muistelin että tunnilla sanottiin, että olisi parempi jos nimessä ei viitata esim. palvelinsalin sijaintiin
* deploy

![image](https://github.com/user-attachments/assets/89ef3aaa-4526-4b6f-bad2-87e4c9c599f0)



##  b) Alkutoimet virtuaalipalvelimella

20.50 - 21:15

Palomuuri
* Yhteys serveriin: ssh root@94.237.39.223
* sudo apt-get update
* sudo apt-get install ufw
* sudo ufw allow 22/tcp
* sudo ufw enable
* sudo ufw allow 80/tcp

Rootin sulku
* sudo adduser toni => salasana
* sudo adduser toni sudo

![image](https://github.com/user-attachments/assets/97dc9efd-08f6-4715-a65a-972e6d1743bc)

kopioi rootin asetukset
* sudo cp -rvn /root/.ssh /home/toni/
* sudo chown -R toni:toni /home/toni/
* exit
* ssh toni@94.237.39.223

Näytti toimivan

![image](https://github.com/user-attachments/assets/8793a354-c828-4110-9d00-9042eb1b229a)


* sudo usermod --lock root # => annoin tonin salasanan
* sudo mv -nv /root/.ssh /root/DISABLED-ssh/

![image](https://github.com/user-attachments/assets/6457734e-5fe4-4beb-a2b1-8ce7d1353221)

sudo apt-get update
sudo apt-get dist-upgrade
sudo systemctl reboot

## c) Apache-webpalvelimen asennus virtuaalipalvelimelle

21:16 - 21:22

* ssh toni@94.237.39.223 => salasanaa ei tarvittu
* sudo apt-get install apache2
* echo Hello world! |sudo tee /var/www/html/index.html
* menin selaimella virtuaalipalvelimen IP-osoitteeseen => vaihdettu teksti näkyi!

![image](https://github.com/user-attachments/assets/9fab1614-ba03-4d3f-b399-a548f1857731)





## d) Name-based virtual host virtuaalipalvelimelle

21.22 - 

* sudoedit /etc/apache2/sites-available/toni.example.com.conf

Asetustiedostoon kirjataan:
<VirtualHost *:80>
	ServerName jurpo.example.com
	DocumentRoot /home/tero/public_sites/jurpo.example.com
	<Directory /home/tero/public_sites/jurpo.example.com>
		Require all granted
	</Directory>
</VirtualHost>

![image](https://github.com/user-attachments/assets/c26911a3-3dd0-476b-abba-fd49e2f56511)


* sudo a2ensite toni.example.com.conf
* ls /etc/apache2/sites-enabled => sudo a2dissite 000-default.conf

![image](https://github.com/user-attachments/assets/a16483e1-e306-4948-9d7a-21d8739bced7)

* luo /home/toni/public_sites/toni.example.com
* asentelin micron samalla
* sudo apt-get -y install micro bash-completion wget curl

![image](https://github.com/user-attachments/assets/449b4f58-8300-4651-8521-5326adedb4a1)

* loin sinne index.html => malli tero karvisen artikkelista => tarkistin w3-validaattorilla
* kokeillaan nettiselaimella => mitään ei ollut muuttunut, näin hello worldin
* sudo systemctl restart apache2 => taas nettiselaimeen

![image](https://github.com/user-attachments/assets/4d3093cf-06ff-4a2e-8a3d-81dd8e6ebf2d)

* Katsotaan lokeja
* sudo tail /var/log/apache2/error.log =>

![image](https://github.com/user-attachments/assets/c1ece6e0-ef3f-4edd-b984-03bab4f0a042)

* Permissions are missing on a component of the path => en ymmärtänyt viestiä
* Etsin errorkoodilla AH00035 => https://serverfault.com/questions/1041060/solving-apache-search-permissions-are-missing-on-a-component-of-the-path-issue
* puuttuuko jostain kansiosta -x lupa => toni-kansiossa luvat 700

![image](https://github.com/user-attachments/assets/74355e48-0288-44a0-8ef7-e1907120e5d9)

* Apachen .conf tiedostossa require all granted => mielestäni on, en huomannut kirjoitusvirhettä
* SElinux-ongelma => en osaa ratkaista

* Kokeillaan muokata toni-kansion lupia
* chmod 755 /home/toni
* sudo systemctl restart apache2
* Tämä näytti ratkaisseen ongelman => nyt nettisivu näkyi

![image](https://github.com/user-attachments/assets/986e6df2-a7cc-4f2c-aa3b-54cbde6b3a50)

* Kokeilin vielä puhelimen nettiselaimella => näkyi!



## Lähteet

[1] Karvinen, T. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Luettu 2025-02-08.

[2] Lehto, S. 2022-02-14. Teoriasta käytäntöön pilvipalvelimen avulla (h4). https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/. Luettu 2025-02-08.

[3] Karvinen, T. 2017-09-19. First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS. https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/. Luettu 2025-02-08.

