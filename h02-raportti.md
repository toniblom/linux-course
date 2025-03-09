# h02 Komentaja Pingviini -raportti
Tekijä: Toni Blom

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h02 liittyen. Tehtävien tekopäivämäärä oli 27.1.2025. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/. [^1]

Tein tehtävän seuraavalla kone- ja ohjelmistokokoonpanolla:
* Tietokone: Lenovo Yoga Slim 7 Pro 14ACH5 -kannettava tietokone
* Prosessori: AMD Ryzen 7 5800H with Radeon Graphics 3.20 GHz
* Isäntäkäyttöjärjestelmä: Windows 10 Home, versio versio 22H2, OS build 19045.5131
* Oracle VM VirtualBox versio 7.1.4.

## x) Komentokehotteen perusteita

Tiivistin seuraavan komentokokoelman osoitteesta https://terokarvinen.com/2020/command-line-basics-revisited/ löytyvään artikkeliin perustuen. [^2]

* Komentokehotteella liikkuminen:
```
$ ls                # Listaa tiedostot
$ pwd               # Näytä työskentelykansio
$ cd directoryname/ # Vaihda työskentelykansiota
$ less file.txt     # Näytä tiedoston sisältö
```
* Tiedostojen manipulointi:
```
$ mkdir newfolder/    # Luo kansio
$ mv oldname newname  # Siirrä tai nimeä uudelleen (tiedosto tai kansio)
$ cp original copy    # Kopioi (tiedosto tai kansio)
$ rmdir emptyfolder/  # Poista tyhjä kansio
$ rm text.txt         # Poista tiedosto
```
* Ohjeita ja apua:
```
$ man commandname    # Näytä komennon ohje
```
* Paketinhallinta:
```
$ sudo apt-get update              # Päivitä paketinhallinta
$ apt-cache search examplegamename # Etsi ohjelmistoja hakusanalla
$ sudo apt-get install coolgame    # Asenna ohjelmisto
$ sudo apt-get remove coolgame     # Poista ohjelmisto
```
* Huom! Jos ohjelmiston lisäksi halutaan poistaa myös ohjelmistoon liittyvät asetukset, tulee poistettaessa käyttää komentoa `sudo apt-get purge`.

## a) Micro-editorin asentaminen

Kello 11:03

Avasin aiemmin asentamani Debian-virtuaalikoneen ([^3]) terminal emulatorin ja ajoin ensin komennon `sudo apt-get update` päivittääkseni paketinhallinnan. Olin jo aiemmin asentanut micro-editorin virtuaalikoneelle, joten poistin sen ensin `sudo apt-get remove micro` -komennolla voidakseni sitten asentaa sen uudelleen `sudo apt-get install micro` -komennolla.

![image](https://github.com/user-attachments/assets/df1041c7-11e9-4c3a-bdc1-4b1b0d8a892f)

## b) apt-komento

Kello 11.15

Etsin esimerkkejä terminaaliohjelmista internetistä, ja valitsin kolme ohjelmaa kokeiltavaksi [^2][^4]. Valitsemani ohjelmat olivat tree (näyttää tiedostopuun visuaalisessa muodossa), chafa (näyttää kuvatiedoston terminaalissa) ja inxi (näyttää tietoja tietokoneen tilasta). Asensin nämä kolme ohjelmaa komennolla `sudo apt-get -y install tree chafa inxi`.

### tree

![image](https://github.com/user-attachments/assets/7d2bc308-202b-4d9e-8621-fbf1c19c61e7)

### chafa

![image](https://github.com/user-attachments/assets/782f3e68-d64a-4fee-81de-cac9692ca2a6)

Kuvan lähde: Keith Kissel, CC BY 2.0 <https://creativecommons.org/licenses/by/2.0>, via Wikimedia Commons, https://commons.wikimedia.org/wiki/File:June_odd-eyed-cat_cropped.jpg. [^5]

### inxi

![image](https://github.com/user-attachments/assets/c017b3d1-354f-430c-83cd-3630b0f2a6dc)

## c) Linuxin oleellisia hakemistoja

Kello 11:35

* */* on root- eli juurihakemisto, jonka sisällä kaikki muut hakemistot ovat. [^2]

![image](https://github.com/user-attachments/assets/b22c86e4-692c-437a-abcb-41cca3eb6d95)

* */home/* sisältää kaikkien käyttäjien kotihakemistot. Omassa virtuaalikoneessani oli vain yksi käyttäjä. [^2]

![image](https://github.com/user-attachments/assets/71c39739-7a30-4ae2-aa91-c71870a04f21)

* */home/toni/* on käyttäjänimeni kotikansio. Se sisältää mm. työpöydän tiedostot ja ladatut tiedostot omissa kansioissaan. [^2]

![image](https://github.com/user-attachments/assets/9f3952e8-ca89-4a41-9c5c-7fcb4df5041a)

* */etc/* hakemisto sisältää järjestelmän laajuisia asetuksia  [^2]. Hakemisto sisälsi esim. bluetooth-kansion, jossa on asetuksia Bluetoothiin liittyen.

![image](https://github.com/user-attachments/assets/6a4769e8-be78-43bf-8a92-6b701bfea949)

* */media/* hakemisto sisältää esim. järjestelmään kytkettyjen CD-ROMien tai USB-muistien tiedostot. [^2]

* */var/log/* hakemisto sisältää järjestelmän laajuisia lokitiedostoja [^2]. Hakemisto sisältää esim. /var/log/apt -kansion, jossa on apt-komentoon liittyviä lokitiedostoja [^6].

![image](https://github.com/user-attachments/assets/7bb7ced4-c9fb-471f-87d0-9feab3a2ea77)

## d) grep-komento

Kello 11:55

* Etsin sanaa "kala" tiedostosta kala.txt komennolla `grep -ni "kala" kala.txt` eli vain yhdestä tiedostosta. "-n" asetus näyttää myös rivit, jolta sana löytyy ja "-i" asetuksella hakusanan kirjainkoolla ei ole väliä. [^7]

![image](https://github.com/user-attachments/assets/bdcdb3ef-cd51-416c-88c1-7b359399a5a3)

* Etsin työskentelykansiosta kaikkien niiden tiedostojen nimet, joista löytyy hakusana "kala". Tähän tarvitaan komennon "-l" optiota. [^7]

![image](https://github.com/user-attachments/assets/31aeb87f-803c-4153-a93e-d1b0a453b4e2)

* Etsin työskentelykansiosta ja sen alikansioista rekursiivisesti hakusanalla "kala". Tähän tarvitaan komennon "-r" optiota. [^8]

![image](https://github.com/user-attachments/assets/026f8bb1-9f0c-4f92-baea-c4af7c736019)

## e) Pipe

Kello 12:08

Tein pari esimerkkikomentoa pipen käytöstä. Ensimmäisessä komennossa `ls -l` komennon tulos syötetään `nl` -komennolle, jolloin tuloksen eteen lisätään rivinumerot. Toisessa komennossa ensimmäisen komennon tulos syötetään `grep` -komennolle, joka etsii syötteestä rivit, joissa esiintyy teksti "19:44". 

![image](https://github.com/user-attachments/assets/0dd75174-bb6d-4104-8e42-e0bbb25f9dc7)

## f) lshw-komento

Kello 12:15

`lshw`-komentoa ei ollut asennettuna valmiiksi, joten asensin sen ensin `sudo apt-get install lshw` -komennolla. Tämän jälkeen suoritin `sudo lshw -short -sanitize` -komennon.

![image](https://github.com/user-attachments/assets/f0a8c73c-63ad-421c-b949-8d3e858bdfa9)

Komennon tuloksena näkyy neljä saraketta, mm. kunkin komponentin luokka ja lyhyt kuvaus. Listauksessa näkyi esim.:
* Virtuaalikoneen käytettävissä oleva prosessori AMD Ryzen 7 5800H.
* 4GiB system memory, joka oli virtuaalikoneelle allokoitu RAM-muisti.
* 64 GB VBOX HARDDISK, joka oli virtuaalikoneelle allokoitu kovalevytila.
* 128 KiB BIOS, joka sisältää virtuaalikoneen BIOSin.

## Lähteet

[^1]: [Karvinen, Tero. Linux Palvelimet 2025 alkukevät.](https://terokarvinen.com/linux-palvelimet/) Luettu 2025-01-25.

[^2]: [Karvinen, Tero. Command Line Basics Revisited.](https://terokarvinen.com/2020/command-line-basics-revisited/) Luettu 2025-01-25.

[^3]: [Blom, T. h01 Oma Linux -raportti.](https://github.com/toniblom/linux-course/blob/main/h01-oma-linux.md) Luettu 2025-01-27.

[^4]: [LinuxLinks. 100 Great and Must-Have CLI Linux Applications.](https://www.linuxlinks.com/100-great-must-have-cli-linux-applications/) Luettu 2025-01-27.

[^5]: [Keith Kissel, CC BY 2.0 <https://creativecommons.org/licenses/by/2.0>, via Wikimedia Commons.](https://commons.wikimedia.org/wiki/File:June_odd-eyed-cat_cropped.jpg) Katsottu 2025-01-27.

[^6]: [Stack Exchange Inc. Where are the logs for apt-get?.](https://askubuntu.com/questions/425809/where-are-the-logs-for-apt-get) Luettu 2025-01-27.

[^7]: [Hira, Z. Grep Command in Linux – Usage, Options, and Syntax Examples.](https://www.freecodecamp.org/news/grep-command-in-linux-usage-options-and-syntax-examples/) Luettu 2025-01-27.

[^8]: [Stack Exchange Inc. How do I recursively grep all directories and subdirectories?.](https://stackoverflow.com/questions/1987926/how-do-i-recursively-grep-all-directories-and-subdirectories) Luettu 2025-01-27.
