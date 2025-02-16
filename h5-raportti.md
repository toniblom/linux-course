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

* klo 14:08 : kokeilin toniblom.me -osoitetta => aiemmin tekemäni sivu näkyi!

![image](https://github.com/user-attachments/assets/e1f3990f-77c9-4749-ae1d-2a4b3483fdcd)


## b) name based virtual host uudessa nimessäni
14:00 - 15:20


* Luodaan .conf tiedosto => sudoedit /etc/apache2/sites-available/toniblom.me.conf
![image](https://github.com/user-attachments/assets/b7be6eb7-dffd-4330-8ffc-750ab0fb9a67)


```
sudo a2ensite toniblom.me.conf      # konfiguraatiotiedosto aktiiviseksi
ls /etc/apache2/sites-enabled       # katsotaan onko muita .conf tiedostoja aktiivisena
sudo a2dissite 000-default.conf     # deaktivoidaan muut kuin haluttu, itse asiassa tätä ei tarvitse tehdä
```
![image](https://github.com/user-attachments/assets/4c151f12-5a7d-418d-a3c8-34ec7b5f7e27)

* luodaan kansio käyttäjän kotihakemistoon `mkdir -p /home/toni/public_sites/toniblom.me`
* Tänne luomani html-tiedostot => tässä vaiheessa random riittää
* `sudo systemctl restart apache2`

![image](https://github.com/user-attachments/assets/22a0e83f-df7d-45b6-a150-3fbd064cf697)

![image](https://github.com/user-attachments/assets/98e487a7-6bc7-472b-8457-cdb411459026)

![image](https://github.com/user-attachments/assets/65d54144-5fff-4c32-9e1a-2db87cfee456)

![image](https://github.com/user-attachments/assets/b01d99fa-ac41-417b-a8a2-3b0a1d201dd4)

![image](https://github.com/user-attachments/assets/6f32217d-2025-46e2-ae08-be7e8c7c8a7a)

https://www.geeksforgeeks.org/how-to-start-stop-or-restart-apache-server-on-ubuntu/

![image](https://github.com/user-attachments/assets/c2108c60-28d5-4612-93cb-6ab94fa9ccc4)

![image](https://github.com/user-attachments/assets/799e377d-dbc5-4539-b026-cd78b65d3973)

* En onnistunut löytämään ratkaisua tähän ongelmaan => päätin asentaa apachen uudelleen
* sudo apt-get purge apache2
* sudo apt-get install apache2
* echo Hello world! |sudo tee /var/www/html/index.html
![image](https://github.com/user-attachments/assets/b9e79016-8cf2-40fd-94de-c7862beee4ab)
![image](https://github.com/user-attachments/assets/8048be69-c3eb-4e1c-a2e9-308f2f147676)


* sudoedit /etc/apache2/sites-available/toniblom.me.conf => tiedostoni olikin edelleen olemassa
  ![image](https://github.com/user-attachments/assets/274cfec5-2a07-48a8-a774-c01700cac65a)

![image](https://github.com/user-attachments/assets/f7b7a108-1b55-465b-85ed-2420e495c58b)

![image](https://github.com/user-attachments/assets/35c6122c-d407-46b9-a2d4-0f35450020cc)

* sudo systemctl restart apache2

![image](https://github.com/user-attachments/assets/6fde8fea-b908-4b6c-8033-e626670bc0a9)

![image](https://github.com/user-attachments/assets/d2ac43c2-fef8-4c60-ad6b-a7dc8feabcac)

sudo adduser toni adm

![image](https://github.com/user-attachments/assets/7d02227e-f370-4665-aa62-3b704ac049d2)

![image](https://github.com/user-attachments/assets/ffdbc4ec-4725-441a-a128-b91908866f0a)

* huomasin .conf -tiedostossani virheen, ylimääräinen /

![image](https://github.com/user-attachments/assets/3a8d79d2-4637-49b2-b66b-55225c7d077c)


* sudo systemctl restart apache2 => komento onnistui => sivu näkyi!

![image](https://github.com/user-attachments/assets/aaff846f-6766-4dee-81ce-4dffe52c3419)



 



## c)

* Tein sivut index.html, blog.html, projects.html kaikki linkittämään toisiinsa
* w3-validaattorin käyttäminen sivuihin
* sivujen kopiointi palvelimelle `scp -r kansio/tero@example.com:public_html/`

## d)



## e)


## Lähteet

