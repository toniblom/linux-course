# h01 Oma Linux - raportti

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h01 liittyen. Tehtävien tekopäivämäärä oli 19.1.2025. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/. [1]

## Raportin kirjoittaminen

Raportin kirjoittamisessa on tärkeää kuvata **täsmällisesti** mitä tehtiin ja mitä sitten tapahtui. Raportin perusteella raportin lukijan tulisi olla mahdollista **toistaa** tehdyt toimenpiteet samanlaisin tuloksin. Tämän vuoksi on oleellista raportoida myös esimerkiksi käytetyt laitteet ja ohjelmistot sekä tehtyjen toimenpiteiden kellonajat. Raportin tulisi olla **helppolukuinen**, joten raportissa kannattaa käyttää väliotsikoita ja oikeaoppista kieltä. Raportissa on myös oltava asianmukaiset **lähdeviitteet**. Asioiden sepittäminen tai plagiointi ei luonnollisestikaan ole sallittua. [2]

## Mitä ovat vapaat ohjelmistot?

Free Software Foundation asettaa vapaan ohjelmiston määritelmälle neljä keskeistä vapautta:
* Vapaus 0: Vapaus ajaa ohjelmistoa kuten haluat, mihin tahansa tarkoitukseen.
* Vapaus 1: Vapaus tutkia ohjelmiston toimintaperiaatteita ja muuttaa niitä.
* Vapaus 2: Vapaus levittää kopioita ohjelmistosta.
* Vapaus 3: Vapaus levittää kopioita omasta muunnellusta ohjelmiston versiosta.

Vapaudet 1 ja 3 edellyttävät, että ohjelmiston lähdekoodi on vapaasti nähtävillä ja muunneltavissa. Englannin kielessä sana "free software" voisi tarkoittaa sekä vapaata ohjelmistoa että ilmaista ohjelmistoa. Vapaa ohjelmisto ei kuitenkaan tarkoita ilmaista ohjelmistoa, ja vapaa ohjelmisto voi hyvinkin olla kaupallinen ohjelmisto. [3]

## Linux-virtuaalikoneen asentaminen

### Lähtötilanne

Käytin tehtävän tekemiseen Lenovo Yoga Slim 7 Pro 14 kannettavaa tietokonetta, jonka isäntäkäyttöjärjestelmänä oli Windows 10 Home versio 22H2. Oracle VM VirtualBox -ohjelmiston versio 7.0.6 oli jo asennettuna koneelle. Linux-virtuaalikoneen asentamisessa seurasin Tero Karvisen "Install Debian on Virtualbox - Updated 2024" -ohjetta soveltuvin osin [4].

### VirtualBoxin Päivittäminen

VirtualBox -ohjelmistosta oli julkaistu uudempi versio, joten päivitin sen uusimpaan versioon 7.1.4.

Kello 14:15
* Latasin VirtualBoxin asennusohjelmiston osoitteesta https://www.virtualbox.org/wiki/Downloads, Windows hosts versio.
* Ajoin asennusohjelman:
  * Welcome to the Oracle VirtualBox 7.1.4. Setup Wizard => painoin Next
  * End-User License Agreement => valitsin "I accept the terms in the License Agreement" => painoin Next
  * Custom Setup => en tehnyt tähän muutoksia => painoin Next
  
  ![image](https://github.com/user-attachments/assets/5e0fb003-0364-4163-9533-6a33f4f8db48)

  * Warning: Network Interfaces => painoin Yes
  * Missing Dependencies Python Core / win32api => painoin Yes
  * Install => painoin Finish
* Asennus vaikutti onnistuneen ja ohjelmisto avautui normaaliin tapaan.

### Debian ISO-kuvan lataaminen

Kello 15:05
* Latasin ISO-kuvan osoitteesta https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/debian-live-12.9.0-amd64-xfce.iso.

### Uuden virtuaalikoneen luominen

Kello 15:23
* Avasin VirtualBoxin => valitsin Machine => valitsin New...
* Avautui Create Virtual Machine -ikkuna

![image](https://github.com/user-attachments/assets/b490689f-0b67-4541-ad92-bb3829ba13e9)

  * En löytänyt ikkunasta Expert Mode -valintaa ja Skip Unattended Installation -kohta ei ollut käytettävissä.
  * Unattended Install -kohdassa en tehnyt muutoksia.
  
  ![image](https://github.com/user-attachments/assets/3cdc5df5-e228-4d82-99b4-b323623ed923)

  * Hardware-kohdassa valitsin Base Memory 4000 MB, Processors 1 CPU. En vaihtanut prosessorien määrää.
  ![image](https://github.com/user-attachments/assets/cfaae6cc-cf70-448d-962a-277bcab4c4e1)

  * Hard Disk -kohdassa valitsin muistin kooksi 60 GB. Tässä kohdassa ei ollut valintaa dynaamisesta allokaatiosta, oletin sen olevan käytössä, kun en ruksannut Pre-allocate Full Size -kohtaa.

  ![image](https://github.com/user-attachments/assets/2cdc7df3-de04-4c21-98ce-8f4a0d3e0f51)

* Lopuksi painoin Finish.

### Debian ISO-kuvan käyttäminen virtuaalisena CDROMina

Kello 15:30
* Valitsin luomani DebianToniBlom -virtuaalikoneen => valitsin Settings => Storage-tab => Controller IDE => CDROM Empty
  * Attributes -kohdasta valitsin Choose/Create a Virtual Optical Disk
* Avautui Optical Disk Selector -kohta => valitsin lataamani debian-live ISO-kuvan => painoin Choose
* Tässä vaiheessa näkymä oli seuraavanlainen:

  ![image](https://github.com/user-attachments/assets/65808921-0373-4edf-bc60-c581590833ed)

### Live Desktopin avaaminen

Kello 15:40
* Avasin virtuaalikoneen kohdasta Start
* Ilmestyi Boot menu => valitsin Live System (amd64) => painoin Enter
* Avautui Live Desktop -näkymä:

![image](https://github.com/user-attachments/assets/87c5015a-3efd-472b-9d78-8d06f6b6d08b)

* Kokeilin web-selaimen toimintaa. Tämä vaikutti toimivan normaalisti.

### Asennusohjelman ajaminen

Kello 15:45
* Klikkasin Live Desktopilta Install Debian
* Aukesi Debian GNU/Linux Installer, johon tein seuraavat valinnat:
  * Language: American English
  * Location: Finland
  * Keyboard: Finnish, Default
  * Partitions: Erase disk - yes, Encrypt - no, Bootloader location - Master Boot Record of VBOX HARDDISK(/dev/sda) (oletuksena)
  * Users:
  
  ![image](https://github.com/user-attachments/assets/592d61fd-3d38-43fa-9fb4-07860ce9056e)

  * Summary => painoin Install
* Asennuksen valmistumisen jälkeen käynnistin koneen uudelleen

### Ensimmäinen sisäänkirjautuminen
Kello 16:00
* Uudelleen käynnistyksen jälkeen ilmaantui kirjautumisikkuna => kirjauduin sisään luomillani tunnuksilla
* Kokeilin internetselainta, joka vaikutti toimivan normaaliin tapaan.

![image](https://github.com/user-attachments/assets/76c7c7a1-fc37-4fe6-a563-2789a3743950)

### Ensimmäisiä toimenpiteitä
Kello 16:05
* Avasin Terminal Emulator -ohjelman ja ajoin seuraavat komennot:
  * `sudo apt-get update`
  * `sudo apt-get -y dist-upgrade`
  * `sudo apt-get -y install ufw`
  * `sudo ufw enable`
* Käynnistin virtuaalikoneen uudelleen ja kirjauduin sisään uudelleen

## Suosikkiohjelmani Linuxilla (vapaaehtoinen)

Katselin läpi Debianin mukana asennettuja ohelmistoja. Näistä minulle tutuin oli LibreOffice, joka sisältää mm. taulukkolaskentaohjelman ja dokumenttien kirjoitusohjelman. Olen joskus käyttänyt näitä Microsoftin tuotteiden sijasta, ja omiin tarpeisiini LibreOffice on toiminut erinomaisesti.

![image](https://github.com/user-attachments/assets/a2da6e41-935e-4348-b8fd-919ee90e4bed)


## Lähteet

[1] Karvinen, Tero. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Katsottu 2025-01-19.

[2] Karvinen, Tero. Raportin kirjoittaminen. https://terokarvinen.com/2006/raportin-kirjoittaminen-4/. Katsottu 2025-01-16.

[3] Free Software Foundation, Inc. What is Free Software? https://www.gnu.org/philosophy/free-sw.html. Katsottu 2025-01-16.

[4] Karvinen, Tero. Install Debian on Virtualbox - Updated 2024. https://terokarvinen.com/2021/install-debian-on-virtualbox/. Katsottu 2025-01-19.
