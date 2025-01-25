# h02 Komentaja Pingviini -raportti
Tekijä: Toni Blom

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h02 liittyen. Tehtävien tekopäivämäärä oli 25.1.2025. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/. 

## x) Komentokehotteen perusteita

Tiivistin seuraavan komentokokoelman osoitteesta https://terokarvinen.com/2020/command-line-basics-revisited/ löytyvään artikkeliin perustuen. [2]

* Komentokehotteella liikkuminen
```
$ ls                # Listaa tiedostot
$ pwd               # Näytä työskentelykansio
$ cd directoryname/ # Vaihda työskentelykansiota
$ less file.txt     # Näytä tiedoston sisältö
```
* Tiedostojen manipulointi
```
$ mkdir newfolder/    # Luo kansio
$ mv oldname newname  # Siirrä tai nimeä uudelleen (tiedosto tai kansio)
$ cp original copy    # Kopioi (tiedosto tai kansio)
$ rmdir emptyfolder/  # Poista tyhjä kansio
$ rm text.txt         # Poista tiedosto
```
* Ohjeita ja apua
```
$ man commandname    # Näytä komennon ohje
```
* Paketinhallinta
```
$ sudo apt-get update              # Päivitä paketinhallinta
$ apt-cache search examplegamename # Etsi ohjelmistoja hakusanalla
$ sudo apt-get install coolgame    # Asenna ohjelmisto
$ sudo apt-get remove coolgame     # Poista ohjelmisto
```
* Huom! Jos ohjelmiston lisäksi halutaan poistaa myös ohjelmistoon liittyvät asetukset, tulee poistettaessa käyttää komentoa `sudo apt-get purge`.

## a) 

## Lähteet

[1] Karvinen, Tero. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Luettu 2025-01-25.

[2] Karvinen, Tero. Command Line Basics Revisited. https://terokarvinen.com/2020/command-line-basics-revisited/. Luettu 2025-01-25.

