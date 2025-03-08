# h7 Maalisuora -raportti
_Tekijä: Toni Blom_

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h7 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [1].

Tein tehtävän seuraavalla kone- ja ohjelmistokokoonpanolla:

* Tietokone: Lenovo Yoga Slim 7 Pro 14ACH5 -kannettava tietokone
* Prosessori: AMD Ryzen 7 5800H with Radeon Graphics 3.20 GHz
* Isäntäkäyttöjärjestelmä: Windows 10 Home, versio versio 22H2, OS build 19045.5371
* Oracle VM VirtualBox versio 7.1.4 r165100 (Qt6.5.3)
* Virtuaalikoneen käyttöjärjestelmä: Debian 12.9.0

## a) Osaan sanoa kolmella kielellä "Hei maailma"

2025-03-08 klo 18:31 - 18:47

- sudo apt-get update
- Käyttämäni kielet, valitsin python, C ja bash
- Python ja C tarvitsee asentaa
- sudo apt-get install python3 gcc

![image](https://github.com/user-attachments/assets/b1e936b3-59c3-4dd6-9efc-27014066b70b)

- Nämä olivatkin jo valmiiksi asennettuna, ilmeisesti debianissa oletuksena

- Tehdään ensin bashilla
micro heimaailma.sh
cat heimaailma.sh
bash heimaailma.sh

![image](https://github.com/user-attachments/assets/bbf3ac2c-5324-4e83-996e-acb0203bc75b)

- Pythonilla
micro heimaailma.py
cat heimaailma.py
python3 heimaailma.py

![image](https://github.com/user-attachments/assets/02ace5d3-72cf-47e5-bf40-fd55bcc32d7d)

- C:llä
micro heimaailma.c
cat heimaailma.c
gcc heimaailma.c -o heimaailmac
./heimaailmac

![image](https://github.com/user-attachments/assets/fc3598a7-f7a5-4eef-ab46-a9dc13ac5edd)


## c)

2025-03-08 klo 18:48 - 19:16

- micro orientoi.sh
- cat orientoi.sh
  #!/usr/bin/bash
  echo "Nimesi on:"
  whoami
  echo
  echo "Vuosi on:"
  date "+%Y"
  echo
  echo "Käytät bashia versio:"
  echo $BASH_VERSION
    https://stackoverflow.com/questions/9450604/how-to-get-the-bash-version-number

![image](https://github.com/user-attachments/assets/396f311f-3069-4e1e-bfda-72b927ba11f4)


- chmod ugo+x orientoi.sh

![image](https://github.com/user-attachments/assets/d0fa8788-6649-48b7-a3d7-0d6dbc972281)

- ./orientoi.sh

![image](https://github.com/user-attachments/assets/97cadd66-e759-4404-92e3-1aab53101c60)

- mv orientoi.sh orientoi
- ./orientoi

![image](https://github.com/user-attachments/assets/5ba84a95-4d94-4a1d-b5c1-62b33f15c0b0)


- sudo cp -v orientoi /urs/local/bin/
- orientoi

![image](https://github.com/user-attachments/assets/4d88ce7c-0b7e-47b2-ad60-00d0756523bf)


Luo uusi käyttäjä
- sudo useradd mikkihiiri
- sudo passwd mikkihiiri
- su mikkihiiri
- orientoi
- exit
- sudo userdel mikkihiiri

![image](https://github.com/user-attachments/assets/efe63197-8260-4fbe-8207-4472cc769439)

Toimi


## d)


## Lähteet

[1] Karvinen, Tero. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Luettu 2025-03-08.
