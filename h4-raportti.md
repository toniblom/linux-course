# h4 Maailma kuulee -raportti

Tekijä: Toni Blom

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h4 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [1].

Tein tehtävän seuraavalla kone- ja ohjelmistokokoonpanolla:
* Tietokone: Lenovo Yoga Slim 7 Pro 14ACH5 -kannettava tietokone
* Prosessori: AMD Ryzen 7 5800H with Radeon Graphics 3.20 GHz
* Isäntäkäyttöjärjestelmä: Windows 10 Home, versio versio 22H2, OS build 19045.5371
* Oracle VM VirtualBox versio 7.1.4.
* Virtuaalikoneen käyttöjärjestelmä: Debian 12.9.0

## x) Lue ja tiivistä

### Opiskelijan esimerkkiraportti

Tein seuraavan tiivistelmän Susanna Lehdon pilvipalvelimen vuokraamiseen liittyvästä raportista (https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/). [2]

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

Kokeilin ensin käyttää GitHubin Student Pack -krediittejä vuokratakseni palvelimen DigitalOceanin kautta.
* Menin osoitteeseen education.github.com/pack, ja painoin kohdasta "Get access by connecting your GitHub account on DigitalOcean".

![image](https://github.com/user-attachments/assets/44d999c7-4680-45b8-ab1b-33c3a7db50a3)

* Valitsin DigitalOceanin sivulta kohdan "Sign In with GitHub" ja "Authorize digitalocean". Tässä kohtaa tuli virheilmoitus "An unknown error has occurred." Tämä johtui siitä, etten ollut vielä rekisteröitynyt DigitalOceaniin.
* Valitsin kohdan "Sign Up with GitHub", vastailin muutamaan kysymykseen ja täytin pankkikorttini sekä muut tarvittavat tiedot. Painoin "Save", jonka jälkeen tuli viesti "Tapahtui käsittelyvirhe". Ongelma liittyi ilmeisesti jollain tavalla pankkikorttiini, tarkkaa syytä en lähtenyt selvittämään.

Päätin kokeilla vuokrata palvelimen UpCloudista, joka oli ollut esimerkkinä myös oppitunnilla. [4]
* Menin osoitteeseen upcloud.com ja valitsin "sign up". Tilin luominen onnistui ja pankkikorttinikin hyväksyttiin. Päätin laittaa tililleni vielä kaksivaihetunnistuksen kohdasta "Activate 2FA".
* Tämän jälkeen aloin tilaamaan uutta virtuaalipalvelinta. Painoin kohdasta "Server List" ja "Deploy server".

![image](https://github.com/user-attachments/assets/d3ac9802-11b0-498f-96bf-10d3316ee1cb)

* Valitsin **Location**-kohtaan FI-HEL1 (kuvassa on valittuna toinen sijainti).
* **Plan**-kohtaan valitsin Developer-tabin alta halvimman vaihtoehdon.

![image](https://github.com/user-attachments/assets/f51f3a1b-6a1e-43c8-a0f0-17f84aebc4e7)

* **Storage**-kohdassa jätin oletusasetuksen.
* **Automated backups** -kohdassa jätin oletusasetuksen, eli en laittanut tätä toimintoa päälle.
* **Operating system** -kohdassa valitsin Debian GNU/Linux 12 (Bookworm).
* **Network**-kohtaan jätin oletusasetuksen.
* **Optionals**-kohdassa en valinnut mitään lisäpalveluita.
* **Login Method** -kohdassa valitsin "SSH keys" ja tässä vaiheessa siirryin virtuaalikoneeni Terminal Emulatoriin luomaan SSH-avaimet. Tässä seurasin tehtävänannossa annettua ohjetta.
 	* Ajoin ensin komennot `sudo apt-get update` ja `sudo apt-get -y install openssh-client`. Openssh-client asentui onnistuneesti. [1]
  	* Generoin SSH-avaimet komennolla `ssh-keygen` ja painoin kolme kertaa Enteriä eli tallensin avaimet oletussijaintiin ilman salasanaa. [1]
	* Avasin julkisen avaimen `micro $HOME/.ssh/id_rsa.pub` -komennolla ja kopioin sen sisällön. [1]
* Tässä vaiheessa palasin takaisin UpCloudin palveluun ja kopioin julkisen avaimen kenttään ja painoin "Save the SSH key".
* **Initialization script** -kohtaan en laittanut mitään
* **Server configuration** -kohdassa vaihdoin sekä palvelin-, että host-nimen, koska muistelin, että oppitunnilla sanottiin, että olisi parempi jos nimessä ei viitata esim. palvelinsalin sijaintiin.
* Lopuksi painoin "Deploy". Hetken kuluttua virtuaalipalvelimeni oli valmis. [4]

![image](https://github.com/user-attachments/assets/89ef3aaa-4526-4b6f-bad2-87e4c9c599f0)


##  b) Alkutoimet virtuaalipalvelimella

2025-02-08, klo 20:50 - 21:15

### Palomuurin asentaminen

Palomuurin asentamiseksi käytin seuraaavia komentoja [1][2][3]:

```
ssh root@94.237.39.223		# Otin SSH-yhteyden virtuaalipalvelimeeni root-käyttäjänä
sudo apt-get update		# Päivitin paketinhallinnan
sudo apt-get install ufw	# Asensin ufw-palomuurin
sudo ufw allow 22/tcp		# Tein "reiän" palomuuriin porttiin 22
sudo ufw enable			# Kytkin palomuurin päälle
sudo ufw allow 80/tcp		# Tein "reiän" palomuuriin porttiin 80
```

### root-käyttäjän sulkeminen

Loin ensin uuden käyttäjän nimelläni ja lisäsin käyttäjän sudo-ryhmään [1].

```
sudo adduser toni 	# Loin käyttäjän ja annoin hyvän salasanan
sudo adduser toni sudo	# Lisäsin käyttäjän sudo-ryhmään
```

![image](https://github.com/user-attachments/assets/97dc9efd-08f6-4715-a65a-972e6d1743bc)

Seuraavaksi kopioin root-käyttäjän SSH-asetukset luomalleni uudelle toni-käyttäjälle ja kokeilin ottaa SSH-yhteyden virtuaalipalvelimeen toni-käyttäjänä [1].

```
sudo cp -rvn /root/.ssh /home/toni/	# Kopioin root-käyttäjän .ssh -kansion toni-käyttäjän kotihakemistoon
sudo chown -R toni:toni /home/toni/	# Vaihdoin toni-käyttäjän kotihakemiston sisältöjen omistajan
exit					# Poistuin virtuaalipalvelimelta
ssh toni@94.237.39.223			# Otin SSH-yhteyden virtuaalipalvelimeen toni-käyttäjänä
```

SSH-yhteys toni-käyttäjänä virtuaalipalvelimelle onnistui.

![image](https://github.com/user-attachments/assets/8793a354-c828-4110-9d00-9042eb1b229a)

Varmistuttuani uuden käyttäjänimen toimivuudesta suljin root-käyttäjän pääsyn virtuaalipalvelimelle [1].

```
sudo usermod --lock root 			# Lukitsin root-käyttäjätilin
sudo mv -nv /root/.ssh /root/DISABLED-ssh/	# Siirsin SSH-asetukset toiseen sijaintiin
```

![image](https://github.com/user-attachments/assets/6457734e-5fe4-4beb-a2b1-8ce7d1353221)

_Päivitys 2025-02-09 klo 15:30_

Kokeilin kirjautua root-käyttäjänä virtuaalipalvelimelle. Tämä ei onnistunut, kuten ei pitänytkään.

![image](https://github.com/user-attachments/assets/a81d3409-a29a-4e28-8520-f8fffc0cbe5d)

_Raportti jatkuu alla._

### Ohjelmien päivittäminen

Päivitin vielä virtuaalipalvelimen seuraavilla komennoilla [1]:

```
sudo apt-get update
sudo apt-get dist-upgrade
sudo systemctl reboot
```

## c) Apache-webpalvelimen asennus virtuaalipalvelimelle

2025-02-08, klo 21:16 - 21:22

Asensin Apache web-palvelimen virtuaalikoneelle ja korvasin oletussivun [2]:

```
ssh toni@94.237.39.223 					# Otin SSH-yhteyden virtuaalipalvelimeen, salasanaa ei tarvittu
sudo apt-get install apache2				# Asensin Apache-webpalvelimen
echo Hello world! |sudo tee /var/www/html/index.html	# Korvasin oletussivun sisällön
```

Menin internetselaimella virtuaalipalvelimen IP-osoitteeseen ja vaihdettu teksti näkyi sivulla.

![image](https://github.com/user-attachments/assets/9fab1614-ba03-4d3f-b399-a548f1857731)

## d) Name-based virtual host virtuaalipalvelimelle

2025-02-08, klo 21:22 - 22:38

Loin ensin esimerkkisivulleni .conf-tiedoston ja kirjoitin siihen kuvassa olevan sisällön.

```
sudoedit /etc/apache2/sites-available/toni.example.com.conf
```

![image](https://github.com/user-attachments/assets/c26911a3-3dd0-476b-abba-fd49e2f56511)

Laitoin luomani konfiguraatiotiedoston aktiiviseksi. Tarkistin oliko muita konfiguraatiotiedostoja /etc/apache2/sites-enabled/ -kansiossa aktiivisena. Deaktivoin oletussivun .conf-tiedoston.

```
sudo a2ensite toni.example.com.conf
ls /etc/apache2/sites-enabled
sudo a2dissite 000-default.conf
```

![image](https://github.com/user-attachments/assets/a16483e1-e306-4948-9d7a-21d8739bced7)

Loin kansion /home/toni/public_sites/toni.example.com/ ja kokeilin luoda sinne index.html-tiedoston. En ollutkaan vielä asentanut micro-editoria, joten asensin sen ja pari muutakin pakettia.

![image](https://github.com/user-attachments/assets/449b4f58-8300-4651-8521-5326adedb4a1)

Loin index.html-tiedoston käyttäen apuna Tero Karvisen artikkelista (https://terokarvinen.com/2012/short-html5-page/) löytyvää mallia ja tarkistin tekemäni html-tiedoston sisällön validaattorilla (https://validator.w3.org/#validate_by_input). [5][6]

Tässä vaiheessa kokeilin päivittää internetselaimella virtuaalipalvelimen IP-osoitetta. Näin kuitenkin edelleen aikaisemman "Hello world" -sisältöisen sivun, jolla olin korvannut Apachen oletussivun.

Suoritin komennon `sudo systemctl restart apache2` ja päivitin taas internetselaimella sivua. Nyt tuloksena oli virheilmoitus.

![image](https://github.com/user-attachments/assets/4d3093cf-06ff-4a2e-8a3d-81dd8e6ebf2d)

Katsoin asiaa tarkemmin lokeista. Suoritin komennon `sudo tail /var/log/apache2/error.log`.

![image](https://github.com/user-attachments/assets/c1ece6e0-ef3f-4edd-b984-03bab4f0a042)

Virheilmoitus kertoi jotakuinkin seuraaavaa: "search permissions are missing on a component of the path". En ymmärtänyt tällaisenaan mistä virhe johtui, joten etsin hakukoneella lokin virhekoodilla AH00035. Löysin erään selityksen (https://serverfault.com/questions/1041060/solving-apache-search-permissions-are-missing-on-a-component-of-the-path-issue) ongelmaan, jonka mukaan virhekoodi esiintyy mm. seuraavissa tilanteissa:
* Jostakin tiedostopolun kansioista puuttuu -x lupa.
* Apachen .conf-tiedostossa ei ole tarvittavia lupia.
* SELinux-ongelmat. [7]

Huomasin, että toni-käyttäjäni kotihakemistossa luvat olivat muotoa `drwx------` eli toni-käyttäjän lisäksi muilla ei ollut mitään oikeuksia hakemistoon. 

![image](https://github.com/user-attachments/assets/74355e48-0288-44a0-8ef7-e1907120e5d9)

Kokeilin muokata toni-hakemiston lupia löytämäni toisen ehdotuksen mukaisesti (https://askubuntu.com/questions/451922/apache-access-denied-because-search-permissions-are-missing) ja käynnistin Apachen uudelleen. [8]

```
chmod 755 /home/toni
sudo systemctl restart apache2
```

Tämä vaikutti ratkaisseen ongelman, koska kun päivitin internetselameni, tekemäni sivu näkyi.

![image](https://github.com/user-attachments/assets/986e6df2-a7cc-4f2c-aa3b-54cbde6b3a50)

Kokeilin vielä virtuaalipalvelimeni IP-osoitetta puhelimen nettiselaimella ja sielläkin sivu näkyi.

## Lähteet

[1] Karvinen, T. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Luettu 2025-02-08.

[2] Lehto, S. 2022-02-14. Teoriasta käytäntöön pilvipalvelimen avulla (h4). https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/. Luettu 2025-02-08.

[3] Karvinen, T. 2017-09-19. First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS. https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/. Luettu 2025-02-08.

[4] Karvinen, T: Oppitunnit 2015-02-04, Linux palvelimet -kurssi (https://terokarvinen.com/linux-palvelimet/).

[5] Karvinen, T. 2012-02-12. Short HTML5 page. https://terokarvinen.com/2012/short-html5-page/. Luettu 2025-02-08.

[6] World Wide Web Consortium. W3C®. Markup Validation Service. https://validator.w3.org/#validate_by_input. Luettu 2025-02-08.

[7] Stack Exchange Inc. / käyttäjänimi sastorsl 2024-08-04. Solving Apache "search permissions are missing on a component of the path" issue. https://serverfault.com/questions/1041060/solving-apache-search-permissions-are-missing-on-a-component-of-the-path-issue. Luettu 2025-02-08.

[8] Stack Exchange Inc. / käyttäjänimet Peter (2014-07-30) ja Cyrille (2022-12-03). Apache: access denied because search permissions are missing. https://askubuntu.com/questions/451922/apache-access-denied-because-search-permissions-are-missing. Luettu 2025-02-08.
