# h5 Nimekäs -raportti
Tekijä: Toni Blom

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h4 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [1].

Tein tehtävän seuraavalla kone- ja ohjelmistokokoonpanolla:
* Tietokone: Lenovo Yoga Slim 7 Pro 14ACH5 -kannettava tietokone
* Prosessori: AMD Ryzen 7 5800H with Radeon Graphics 3.20 GHz
* Isäntäkäyttöjärjestelmä: Windows 10 Home, versio versio 22H2, OS build 19045.5371
* Oracle VM VirtualBox versio 7.1.4.
* Virtuaalikoneen käyttöjärjestelmä: Debian 12.9.0

## a) Domain-nimen hankkiminen

2025-16-02 klo 12:55 - 13:30


* https://education.github.com/pack => Get access by connecting your GitHub account on Namecheap

![image](https://github.com/user-attachments/assets/b863adba-0914-4206-88c5-dea286ea2a1c)


* => Authorize namecheapedu

![image](https://github.com/user-attachments/assets/571f25f9-bae1-4454-9c81-6e00d3d0d84a)

* => toniblom.me

![image](https://github.com/user-attachments/assets/26d660c2-1151-4fca-baf7-433455da5f7f)


* => en valinnut lisävaihtoehtoja => add  => complete order

![image](https://github.com/user-attachments/assets/89dfbb92-f93c-4ad4-9de6-a4759d151ccf)

* Choose a free option: GitHubPages => add email => finish up
* Create a new namecheap account => osoitetiedot

![image](https://github.com/user-attachments/assets/574f5717-6547-4cea-b733-711a29d74560)

=> kirjauduin namecheapiin namecheap.com -osoitteessa
* laitoin kaksivaihetunnistuksen päälle

![image](https://github.com/user-attachments/assets/c3023c5f-0adc-457b-8887-afdca994855f)

=> Domain List vasemmasta sivupalkista, oli Domain Privacy protection ON

![image](https://github.com/user-attachments/assets/6dd5d60d-6e69-4d12-ab00-af52df0bfc7b)

* => En laittanut auto renew vaihtoehtoa päälle, koska tuskin uusin tätä nimeä => Manage => en löytänyt advanced DNS kohtaa täältä
* => Dashboard => kotikuvake domainnimen vieressä => Advanced DNS

![image](https://github.com/user-attachments/assets/bacda192-2a6a-41e4-a635-0d9c68524dc0)

* Täällä oli valmiina joitakin tietuita ilmeisesti githubiin liittyen

![image](https://github.com/user-attachments/assets/ed1b3369-497b-460e-bbd9-b384ef2402d4)

* Add new record
  * A record => host: @ => value: IP-osoite => TTL 5 => => save changes =>  vihreä check-merkki
  * A record => host: www => value: IP-osoite => TTL 5 => vihreä check-merkki

![image](https://github.com/user-attachments/assets/bd093d96-330a-48db-aea7-bc333d3ee486)

* muut tietueet eivät poistuneet itsestään, joten poistin ne

![image](https://github.com/user-attachments/assets/881b8d7c-57f5-4c8a-aa14-ecd54ced1cf7)

![image](https://github.com/user-attachments/assets/22a2af1f-2ff1-46dc-9562-116b946108a8)


* Tässä vaiheessa odottelin, ettei väärät tiedot jää selaimen välimuistiin

* klo : kokeilin toniblom.me -osoitetta => 

## b) name based virtual host uudessa nimessäni



* Luodaan .conf tiedosto => sudoedit /etc/apache2/sites-available/toniblom.me.conf
```
sudo a2ensite toniblom.me.conf # konfiguraatiotiedosto aktiiviseksi
ls /etc/apache2/sites-enabled       # katsotaan onko muita .conf tiedostoja aktiivisena
sudo a2dissite 000-default.conf     # deaktivoidaan muut kuin haluttu, itse asiassa tätä ei tarvitse tehdä
```
* luodaan kansio käyttäjän kotihakemistoon `mkdir -p /home/toni/public_sites/toniblom.me`
* Tänne luomani html-tiedostot => tässä vaiheessa random riittää
* `sudo systemctl restart apache2`
* Kokeillaan toniblom.me => näkyykö tekemäni sivu


## c)

* Tein sivut index.html, blog.html, projects.html kaikki linkittämään toisiinsa
* w3-validaattorin käyttäminen sivuihin
* sivujen kopiointi palvelimelle `scp -r kansio/tero@example.com:public_html/`

## d)



## e)


## Lähteet

