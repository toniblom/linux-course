# h7 Maalisuora -raportti
_Tekijä: Toni Blom_

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h7 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [1].

Tein tehtävän seuraavalla kone- ja ohjelmistokokoonpanolla:

* Tietokone: Lenovo Yoga Slim 7 Pro 14ACH5 -kannettava tietokone
* Prosessori: AMD Ryzen 7 5800H with Radeon Graphics 3.20 GHz
* Isäntäkäyttöjärjestelmä: Windows 10 Home, versio versio 22H2, OS build 19045.5371
* Oracle VM VirtualBox versio 7.1.4 r165100 (Qt6.5.3)
* Virtuaalikoneen käyttöjärjestelmä: Debian 12.9.0

## a) "Hei maailma" kolmella kielellä

2025-03-08 klo 18:31 - 18:47

Ajoin ensin komennon `sudo apt-get update`. Päätin tehdä tehtäväni Pythonilla, C:llä ja bashilla. Pythonin ja C:n asentamista varten ajoin komennon `sudo apt-get install python3 gcc`.

![image](https://github.com/user-attachments/assets/b1e936b3-59c3-4dd6-9efc-27014066b70b)

Nämä olivatkin jo valmiiksi asennettuna, eli ne ilmeisesti ovat Debianissa oletuksena asennettuna, koska en muistanut aiemmin niitä asentaneeni.

Loin heimaailma-tiedoston ja suoritin ensin bashilla.

![image](https://github.com/user-attachments/assets/bbf3ac2c-5324-4e83-996e-acb0203bc75b)

Seuraavaksi tein saman Pythonilla.

![image](https://github.com/user-attachments/assets/02ace5d3-72cf-47e5-bf40-fd55bcc32d7d)

Lopuksi tein vielä saman C:llä. C:n tapuksessa koodi täytyi kääntää `gcc heimaailma.c -o heimaailmac` -komennolla.

![image](https://github.com/user-attachments/assets/fc3598a7-f7a5-4eef-ab46-a9dc13ac5edd)

## c) Uusi komento

2025-03-08 klo 18:48 - 19:16

Loin uuden tiedoston ja kirjoitin micro-editorilla siihen alla olevan kuvan kuvan mukaisen sisällön.

![image](https://github.com/user-attachments/assets/396f311f-3069-4e1e-bfda-72b927ba11f4)

Lisäsin suoritusoikeuden tiedostoon käyttäjälle, ryhmälle ja muille komennolla `chmod ugo+x orientoi.sh`

![image](https://github.com/user-attachments/assets/d0fa8788-6649-48b7-a3d7-0d6dbc972281)

Kokeilin suorittaa tiedostoa komennolla `./orientoi.sh` Komento toimi odotetusti.

![image](https://github.com/user-attachments/assets/97cadd66-e759-4404-92e3-1aab53101c60)

Vaihdoin tiedoston nimeä komennolla `mv orientoi.sh orientoi`, jotta komennon voi ajaa ilman .sh-päätettä.

![image](https://github.com/user-attachments/assets/5ba84a95-4d94-4a1d-b5c1-62b33f15c0b0)

Kopioin tiedoston pääkäyttäjänä `usr/local/bin/`, jolloin komento on kaikkien käyttäjien käytettävissä.
```
sudo cp -v orientoi /urs/local/bin/
```

![image](https://github.com/user-attachments/assets/4d88ce7c-0b7e-47b2-ad60-00d0756523bf)


Loin uuden käyttäjän komennolla `sudo useradd mikkihiiri` ja annoin käyttäjälle salasanan komennolla `sudo passwd mikkihiiri`. Vaihdoin käyttäjää komennolla `su mikkihiiri` ja kokeilin uutta komentoani. Se toimi odotetunlaisesti. Lopuksi poistin luomani uuden käyttäjän komennolla `sudo userdel mikkihiiri`.

![image](https://github.com/user-attachments/assets/efe63197-8260-4fbe-8207-4472cc769439)

Huomasin kirjoitusvirheen komennon tulostuksessa, joten korjasin tiedostoa ensin kotikansiossani ja sitten kopioin sen uudelleen `/usr/local/bin/` -kansioon. Tämän jälkeen komennon tulos luki oikein.

![image](https://github.com/user-attachments/assets/9496064a-575e-46dd-9f3b-737f41e51079)

## d) Vanha laboratorioharjoitus

2025-08-09 klo 17:10 - 18:35

Tein kevään 2024 kurssin harjoitusta, joka löytyi osoitteesta https://terokarvinen.com/2024/arvioitava-laboratorioharjoitus-2024-linux-palvelimet/.

Aluksi loin tyhjän virtuaalikoneen 
* Luo tyhjä virtuaalikone
* Käytin tässä apuna raporttiani h1
* päivitin koneen, asensin palomuurin ja kytkin sen päälle

* Kotihakemistoon report/index.md => tänne myös jpg tai png

![image](https://github.com/user-attachments/assets/bfee9953-ee01-40b0-b5e1-9893624bfc3b)



a)
- Oma nimi
- Opiskelijanumero
- Linkki omaan kotitehtäväpakettiin

![image](https://github.com/user-attachments/assets/74892eee-25dc-4168-af87-344080687059)


b) Tiivistelmä
- Mikä toimii: toimivien palveluiden osoitteet tai polut komentoihin
- Mikä ei toimi: luettelo kohdista, joita ei ratkaistu

![image](https://github.com/user-attachments/assets/b3461655-b9ff-4d86-a4bc-195626f73b71)


c) Ei kolmea sekoseiskaa
- suojaa raportti Linux-oikeuksilla niin, että vain oma käyttäjäsi pystyy katselemaan raporttia
- // chmod go-rwx /report/index.md

![image](https://github.com/user-attachments/assets/5e113a08-c748-415d-a7f8-b9279cf9fff0)


d) 'howdy'
- Tee kaikkien käyttäjien käyttöön komento 'howdy'
- Tulostaa päivämäärän, koneen osoitteen
- testaa toisella käyttäjällä => sudo useradd user => sudo passwd user => su user => howdy

![image](https://github.com/user-attachments/assets/ba3b9072-26a3-41cf-913f-a1fdb7958f4d)

![image](https://github.com/user-attachments/assets/eb2b8d42-0a00-4746-b01c-2a23c183ba16)



e) etusivu uusiksi
- asenna apache-weppipalvelin => sudo apt-get install apache2
- tee sivu, joka näkyy koneen IP-osoitteella suoraan etusivulla
- sivua pitää päästä muokkaamaan ilman sudoa

sudoedit /etc/apache2/sites-available/example.com.conf

![image](https://github.com/user-attachments/assets/ce4d1b47-cc99-4dbc-b37b-8308fe8d1b3e)




<VirtualHost *:80>
 ServerName pyora.example.com
 ServerAlias www.pyora.example.com
 DocumentRoot /home/xubuntu/publicsites/pyora.example.com
 <Directory /home/xubuntu/publicsites/pyora.example.com>
   Require all granted
 </Directory>
</VirtualHost>

$ sudo a2ensite pyora.example.com.conf
sudo a2dissite whatever.com.conf

![image](https://github.com/user-attachments/assets/3fe30176-a939-408e-acef-f2bdfcfed328)


$ mkdir -p /home/xubuntu/publicsites/pyora.example.com/

![image](https://github.com/user-attachments/assets/084bf431-992f-4114-baab-48482fe4dd33)

sudo systemctl restart apache2
=> näkyi vain Default-sivu

![image](https://github.com/user-attachments/assets/b50bf701-2cfd-467b-96ad-acfe2534ab89)

Asia ei selvinnyt.

g) salattua hallintaa
- asenna ssh-palvelin
- Tee uusi käyttäjä omalla nimelläsi: "Tero Karvinen test", login name: "terote01"
- Automatisoi ssh-kirjautuminen julkisen avaimen menetelmällä niin, että et tarvitse salasanoja, voit käyttää kirjautumiseen localhost-osoitetta

![image](https://github.com/user-attachments/assets/835998d5-9d61-48b7-8e63-6d918ebe8fda)

sudo apt-get install ssh

![image](https://github.com/user-attachments/assets/404df4e6-ed3a-4e93-b1e8-66eb2bff8a3e)



$ ssh root@10.0.0.1

$ sudo ufw allow 22/tcp
$ sudo ufw enable

$ sudo adduser tero
$ sudo adduser tero sudo
$ sudo adduser tero adm
$ sudo adduser tero admin

root# sudo cp -rvn /root/.ssh/ /home/jurpo/
root# sudo chown -R jurpo:jurpo /home/jurpo/

root# exit
local$ ssh jurpo@10.0.0.1

jurpo$ sudo usermod --lock root
jurpo$ sudo mv -nv /root/.ssh /root/DISABLED-ssh/



h) Djangoasioita ei tehty

## Lähteet

[1] Karvinen, Tero. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Luettu 2025-03-08.
