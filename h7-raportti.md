# h7 Maalisuora -raportti
_Tekijä: Toni Blom, 2025-03-09_

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h7 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [^1].

Tein tehtävän seuraavalla kone- ja ohjelmistokokoonpanolla:

* Tietokone: Lenovo Yoga Slim 7 Pro 14ACH5 -kannettava tietokone
* Prosessori: AMD Ryzen 7 5800H with Radeon Graphics 3.20 GHz
* Isäntäkäyttöjärjestelmä: Windows 10 Home, versio versio 22H2, OS build 19045.5371
* Oracle VM VirtualBox versio 7.1.4 r165100 (Qt6.5.3)
* Virtuaalikoneen käyttöjärjestelmä: Debian 12.9.0

## a) "Hei maailma" kolmella kielellä

2025-03-08 klo 18:31 - 18:47

Ajoin ensin komennon `sudo apt-get update`. Päätin tehdä tehtäväni Pythonilla, C:llä ja bashilla. Pythonin ja C:n asentamista varten ajoin komennon `sudo apt-get install python3 gcc`. [^2]

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

Kopioin tiedoston pääkäyttäjänä `usr/local/bin/`, jolloin komento on kaikkien käyttäjien käytettävissä. [^3]
```
sudo cp -v orientoi /usr/local/bin/
```

![image](https://github.com/user-attachments/assets/4d88ce7c-0b7e-47b2-ad60-00d0756523bf)


Loin uuden käyttäjän komennolla `sudo useradd mikkihiiri` ja annoin käyttäjälle salasanan komennolla `sudo passwd mikkihiiri`. Vaihdoin käyttäjää komennolla `su mikkihiiri` ja kokeilin uutta komentoani. Se toimi odotetunlaisesti. Lopuksi poistin luomani uuden käyttäjän komennolla `sudo userdel mikkihiiri`. [^4][^5][^6]

![image](https://github.com/user-attachments/assets/efe63197-8260-4fbe-8207-4472cc769439)

Huomasin kirjoitusvirheen komennon tulostuksessa, joten korjasin tiedostoa ensin kotikansiossani ja sitten kopioin sen uudelleen `/usr/local/bin/` -kansioon. Tämän jälkeen komennon tulos luki oikein.

![image](https://github.com/user-attachments/assets/9496064a-575e-46dd-9f3b-737f41e51079)

## d) Vanha laboratorioharjoitus

2025-08-09 klo 17:10 - 18:35

Tein kevään 2024 kurssin harjoitusta, joka löytyi osoitteesta https://terokarvinen.com/2024/arvioitava-laboratorioharjoitus-2024-linux-palvelimet/. [^7]

Aluksi loin tyhjän virtuaalikoneen. Käytin tässä apuna aiempaa raporttiani h1 (https://github.com/toniblom/linux-course/blob/main/h01-oma-linux.md). Päivitin myös virtuaalikoneen, asensin ufw-palomuurin ja kytkin sen päälle. [^8]

Loin kotihakemistoon `report/index.md` -tiedoston.

![image](https://github.com/user-attachments/assets/bfee9953-ee01-40b0-b5e1-9893624bfc3b)

### Kohta a)

Kirjoitin `index.md` -tiedostoon pyydetyt tiedot.

![image](https://github.com/user-attachments/assets/74892eee-25dc-4168-af87-344080687059)

### Kohta b)

Tein onnistuneesti kohdat a, b, c, e ja e.

Kohtaa g en saanut ratkaistua.

Kohta h käsitteli Djangoa, jota ei kurssilla käyty läpi, joten jätin tämän tekemättä.

### Kohta c)

Muokkasin raportin pääsyoikeuksia komennolla `chmod go-rwx /report/index.md`, jonka jälkeen vain toni-käyttäjä pystyi kirjoittamaan ja lukemaan tiedostoa.

![image](https://github.com/user-attachments/assets/5e113a08-c748-415d-a7f8-b9279cf9fff0)


### Kohta d)

Tein komennon `howdy`, joka oli kaikkien käyttäjien käytettävissä. Käytin tässä apuna tämän raportin aiemmin kirjoittamaani kohtaa c).

![image](https://github.com/user-attachments/assets/ba3b9072-26a3-41cf-913f-a1fdb7958f4d)

Testasin komentoa toisella käyttäjällä. Komento toimi myös toisella käyttäjällä.

![image](https://github.com/user-attachments/assets/eb2b8d42-0a00-4746-b01c-2a23c183ba16)

### Kohta e)

Asensin apache-web-palvelimen komennolla `sudo apt-get install apache2`

Loin uuden konfiguraatiotiedoston Apacheen tulevaa sivuani varten komennolla `sudoedit /etc/apache2/sites-available/harjoitus7d.com.conf`.

![image](https://github.com/user-attachments/assets/ce4d1b47-cc99-4dbc-b37b-8308fe8d1b3e)

Käynnistin sivuni komennolla `sudo a2ensite harjoitus7d.com.conf` ja otin pois käytöstä default-sivun konfiguraatiotiedoston. sites-enabled-kansiossa oli nyt vain uusi konfiguraatiotiedosto.

![image](https://github.com/user-attachments/assets/3fe30176-a939-408e-acef-f2bdfcfed328)

Tein kotihakemistooni kansioon `/home/toni/publicsites/harjoitus7d.com/` tiedoston `index.html`.

![image](https://github.com/user-attachments/assets/084bf431-992f-4114-baab-48482fe4dd33)

Käynnistin Apachen uudelleen komennolla `sudo systemctl restart apache2`.

Avasin internetselaimen osoitteesta http://localhost ja 127.0.0.1. Näistä molemmista tuli näkyviin Default-tekstillä korvaamani oletussivu eikä kotikansiossani sijaitsevaa index.html -tiedostoa. Toiminta ei siis ollut sellaista mihin olin pyrkinyt.

Katsoin lokeja, josko näissä olisi jotain, joka auttaisi asiaa eteenpäin. error.log -tiedostossa oli lähinnä Apachen normaaliin toimintaan ja uudelleenkäynnistykseen liittyviä rivejä. Muitakaan lokeja tarkastellen en päässyt asiassa eteenpäin. [^9]

![image](https://github.com/user-attachments/assets/b50bf701-2cfd-467b-96ad-acfe2534ab89)

Lisäys 2025-03-09 klo 19:39 - 19:43

Raporttia oikolukiessani huomasin kirjoitusvirheen konfiguraatiotiedostossa. Korjasin tämän.

![image](https://github.com/user-attachments/assets/c85902e4-7183-4bfe-a14b-3a1ea0af6677)

Käynnistin Apachen uudelleen ja avasin internetselaimessa sivun 127.0.0.1. Nyt tekemäni sivu näkyi kuten oli tarkoitus.

![image](https://github.com/user-attachments/assets/2e3ea67c-421d-4a63-bfa4-0a995063bbda)

Lisäys loppuu.

### Kohta g)

Yritin ottaa yhteyttä ssh:lla localhost-osoitteeseen. ssh-komentoa ei kuitenkaan löytynyt.

![image](https://github.com/user-attachments/assets/835998d5-9d61-48b7-8e63-6d918ebe8fda)

Ajoin komennon `sudo apt-get install ssh` asentaakseni ssh:n.

Kokeilin uudelleen ottaa yhteyttä ssh:lla local-host osoitteeseen.

![image](https://github.com/user-attachments/assets/404df4e6-ed3a-4e93-b1e8-66eb2bff8a3e)

Tehtävässä pyydettiin asentamaan ssh-palvelin ja että ssh-kirjautumiseen voi käyttää julkisen avaimen menetelmää localhost-osoitteella. En täysin ymmärtänyt tehtävänantoa, joten jätin tehtävän kesken.

## Lähteet

[^1]: [Karvinen, Tero. Linux Palvelimet 2025 alkukevät.](https://terokarvinen.com/linux-palvelimet/) Luettu 2025-03-08.

[^2]: [Karvinen, Tero. 2018-09-27. Hello World Python3, Bash, C, C++, Go, Lua, Ruby, Java – Programming Languages on Ubuntu 18.04.](https://terokarvinen.com/2018/hello-python3-bash-c-c-go-lua-ruby-java-programming-languages-on-ubuntu-18-04/) Luettu 2025-03-08.

[^3]: [Karvinen, Tero: Oppitunnit 2015-03-04, Linux palvelimet -kurssi.](https://terokarvinen.com/linux-palvelimet/)

[^4]: [GeeksForGeeks. 2024-07-12. How to add User in Linux | useradd Command.](https://www.geeksforgeeks.org/useradd-command-in-linux-with-examples/) Luettu 2025-03-08.

[^5]: [GeeksForGeeks. 2024-01-19. Switch Users on Linux with the su Command.](https://www.geeksforgeeks.org/switch-users-on-linux-with-the-su-command/) Luettu 2025-03-08.

[^6]: [GeeksForGeeks. 2024-09-19. How to Delete User in Linux | userdel Command.](https://www.geeksforgeeks.org/userdel-command-in-linux-with-examples/) Luettu 2025-03-08.

[^7]: [Karvinen, Tero. Final Lab for Linux Palvelimet 2024 Spring.](https://terokarvinen.com/2024/arvioitava-laboratorioharjoitus-2024-linux-palvelimet/) Luettu 2025-03-09.

[^8]: [Blom, Toni. h01 Oma Linux - raportti.](https://github.com/toniblom/linux-course/blob/main/h01-oma-linux.md) Luettu 2025-03-09.

[^9]: [Blom, Toni. h3 Hello Web Server - Raportti.](https://github.com/toniblom/linux-course/blob/main/h3-raportti.md) Luettu 2025-03-09.
