# h5 Nimekäs -raportti
Tekijä: Toni Blom

Tein tämän raportin Linux-palvelimet -kurssin tehtävään h5 liittyen. Tehtävänanto löytyi osoitteesta https://terokarvinen.com/linux-palvelimet/ [1].

Tein tehtävän seuraavalla kone- ja ohjelmistokokoonpanolla:
* Tietokone: Lenovo Yoga Slim 7 Pro 14ACH5 -kannettava tietokone
* Prosessori: AMD Ryzen 7 5800H with Radeon Graphics 3.20 GHz
* Isäntäkäyttöjärjestelmä: Windows 10 Home, versio versio 22H2, OS build 19045.5371
* Oracle VM VirtualBox versio 7.1.4.
* Virtuaalikoneen käyttöjärjestelmä: Debian 12.9.0

## a) Domain-nimen hankkiminen

2025-16-02 klo 12:55 - 13:30

Päätin hankkia domain-nimen GitHub Education -paketin avulla Namecheapilta. Olin hankkinut GitHub Education -paketin jo aikaisemmin. Menin internetselaimella osoitteeseen https://education.github.com/pack ja seurasin linkkiä Namecheapin kohdasta "Get access by connecting your GitHub account on Namecheap".

![image](https://github.com/user-attachments/assets/b863adba-0914-4206-88c5-dea286ea2a1c)

Ilmaista domain-nimeä varten Namecheap halusi tiettyjä lupia GitHub-tiliini liittyen, annoin tarvittavat luvat. Namecheap for Education -sivusto avautui, josta pääsin valitsemaan domain-nimeä.

![image](https://github.com/user-attachments/assets/26d660c2-1151-4fca-baf7-433455da5f7f)

Haluamani domain-nimi oli käytettävissä. Namecheap tarjosi minulle muita maksullisia domain-nimiä samaan tarjoukseen, en nyt tarvinnut näitä, joten valitsin vain ilmaisen .me -loppuisen domain-nimen ja painoin "Complete order". Tämän jälkeen Namecheapin sivustolla pyydettiin vielä lisäämään GitHubiin liitetty sähköpostiosoitteeni ja luomaan tili Namecheapin sivustolle. Nämä tehtyäni kirjauduin namecheap.com -sivustolle luomallani tilillä.

Kirjauduttuani Namecheapiin näin, että tilaamani domain-nimi oli aktiivinen. Laitoin myös kaksivaihetunnistuksen päälle.

![image](https://github.com/user-attachments/assets/c3023c5f-0adc-457b-8887-afdca994855f)

Seuraavaksi muutin domain-nimeni tietueita siten, että ne osoittavat aiemmin vuokraamaani virtuaalipalvelimeen. Tämä tapahtui siirtymällä vasemman palkin "Dashboard" -kohtaan ja painamalla talokuvaketta domain-nimeni vieressä ja kohtaa "Advanced DNS". [4]

![image](https://github.com/user-attachments/assets/bacda192-2a6a-41e4-a635-0d9c68524dc0)


Avautui näkymä, jossa "Host Records" -kohdassa näkyi jo valmiina joitakin tietueita.

![image](https://github.com/user-attachments/assets/ed1b3369-497b-460e-bbd9-b384ef2402d4)

Lisäsin kaksi uutta A-tietuetta seuraavin tiedoin.
  * Record Type: A record / Host: @ / IP Address: virtuaalipalvelimeni IP-osoite / TTL: 5
  * Record Type: A record / Host: www / IP Address: virtuaalipalvelimeni IP-osoite / TTL: 5

 Muut valmiina olleet tietueet eivät poistuneet itsestään, joten poistin ne. [4]

![image](https://github.com/user-attachments/assets/22a2af1f-2ff1-46dc-9562-116b946108a8)

Tässä vaiheessa odottelin hetken, että tietuetiedot ehtisivät päivittyä ja selaimen välimuistiin jäisivät muokkaamani tiedot. Kokeilin toniblom.me -osoitetta ja aiemmin tekemäni sivu näkyi. [4]

![image](https://github.com/user-attachments/assets/e1f3990f-77c9-4749-ae1d-2a4b3483fdcd)


## b) Name-based virtual host uudella domain-nimelläni

2025-02-16 klo 14:00 - 15:20

Olin jo asentanut Apache-serverin virtuaalipalvelimelleni, tätä on kuvattu edellisessä raportissani. Etenin myös name-based virtual hostin konfiguroinnissa [edellisen raportin](https://github.com/toniblom/linux-course/blob/main/h4-raportti.md) vaiheita noudattaen. [5]

Loin uuden .conf -tiedoston Apacheen toniblom.me -domain-nimeä varten. Kirjasin siihen tarvittavat tiedot kuvan mukaisesti.
```
sudoedit /etc/apache2/sites-available/toniblom.me.conf
```

![image](https://github.com/user-attachments/assets/b7be6eb7-dffd-4330-8ffc-750ab0fb9a67)

Laitoin juuri tekemäni konfiguraatiotiedostoni aktiiviseksi ja tarkistin, että se näkyy sites-enabled -kansiossa. En deaktivoinut toista aiemmin tekemääni konfiguraatiotiedostoa, koska domain-nimeä käytettäessä sites-enabled -kansiossa voisi olla useampi .conf -tiedosto (toisin kuin IP-osoitetta käytettäessä).
```
sudo a2ensite toniblom.me.conf
ls /etc/apache2/sites-enabled
```

![image](https://github.com/user-attachments/assets/219f16d5-30b3-4e74-9d10-ff15ca284cc1)

Loin uuden konfiguraatiotiedoston DocumentRoot-asetusta vastaavan kansion käyttäjänimeni kotihakemistoon ja tein sinne yksinkertaisen html-tiedoston. Tässä kansiossa käyttäjä voi muokata internetsivujaan ilman sudo-oikeuksia. Käynnistin Apachen uudelleen.

```
mkdir -p /home/toni/public_sites/toniblom.me
micro /home/toni/public_sites/toniblom.me/index.html
sudo systemctl restart apache2
```

Ensimmäiseen uudelleenkäynnistyskomentooni pääsi kirjoitusvirhe, josta seurasi virheilmoitus. Korjasin kirjoitusvirheen komennossa, tästä seurasi erilainen virheilmoitus.

![image](https://github.com/user-attachments/assets/22a0e83f-df7d-45b6-a150-3fbd064cf697)

Kokeilin virheilmoituksen ehdottamia komentoja. Ensimmäisestä näkyi, että Apache ei ollut enää aktiivisena, vaan "failed" -tilassa.
```
systemctl status apache2.service
```

![image](https://github.com/user-attachments/assets/65d54144-5fff-4c32-9e1a-2db87cfee456)

Toisella ehdotetulla komennolla en saanut lokia näkyviin, koska käyttäjänimelläni ei ollut tarvittavia oikeuksia. Tämä saattoi myös johtua siitä, että Apachen lokit eivät ole vielä siirtyneet journalctl:iin [4].
```
journalctl -xeu apache2.service
```

![image](https://github.com/user-attachments/assets/b01d99fa-ac41-417b-a8a2-3b0a1d201dd4)

Päätin katsoa Apachen error.log -tiedostoa. Täällä näkyivät mm. virheilmoitukset "AH10244: invalid URI path" ja "AH00491: caught SIGTERM, shutting down".
```
sudo tail /var/log/apache2/error.log
```

![image](https://github.com/user-attachments/assets/6f32217d-2025-46e2-ae08-be7e8c7c8a7a)

En ymmärtänyt mistä kyseiset virheilmoitukset johtuivat, joten etsin tietoa internetin hakukoneilla. Soveltuvia selityksiä tai ratkaisuja ongelmaan en löytänyt. Katsoin Apachen tilaa ja yritin käynnistää sen uudelleen löytämieni komentojen avulla (https://www.geeksforgeeks.org/how-to-start-stop-or-restart-apache-server-on-ubuntu/) [2]. Näillä komennoilla näkyi lähinnä samanlaisia virheilmoituksia kuin aiemminkin.
```
sudo systemctl status apache2
sudo systemctl start apache2
```

![image](https://github.com/user-attachments/assets/c2108c60-28d5-4612-93cb-6ab94fa9ccc4)

![image](https://github.com/user-attachments/assets/799e377d-dbc5-4539-b026-cd78b65d3973)

Tässä vaiheessa päätin kokeilla poistaa ja asentaa Apachen uudelleen. Vaihdoin myös oletussivun. Kokeilin osoitetta toniblom.me ja oletussivun korvaava sivuni näkyi selaimella.
```
sudo apt-get purge apache2
sudo apt-get install apache2
echo Hello world! |sudo tee /var/www/html/index.html
```

![image](https://github.com/user-attachments/assets/b9e79016-8cf2-40fd-94de-c7862beee4ab)

Katsoin uudestaan Apachen tilaa komennolla `systemctl status apache2`. Nyt tila oli aktiivinen.

![image](https://github.com/user-attachments/assets/8048be69-c3eb-4e1c-a2e9-308f2f147676)

Kokeilin luoda jälleen .conf -tiedostoa toniblom.me.conf, tämä tiedosto olikin edelleen olemassa. Olin odottanut sen poistuvan purge-komennon myötä, mutta näin ei ollut. En tehnyt tiedostoon muutoksia. Aktivoin toniblom.me.conf -konfiguraatiotiedoston ja varmuuden vuoksi deaktivoin default-sivun konfiguraatiotiedoston.
```
sudoedit /etc/apache2/sites-available/toniblom.me.conf
sudo a2ensite toniblom.me.conf
sudo a2dissite 000-default.conf
```

  ![image](https://github.com/user-attachments/assets/274cfec5-2a07-48a8-a774-c01700cac65a)

Tarkistin, että myös aiemmin tekemäni kansiot ja html-tiedosto olivat käyttäjän kotihakemistossa.

![image](https://github.com/user-attachments/assets/35c6122c-d407-46b9-a2d4-0f35450020cc)

Käynnistin Apachen taas uudelleen. Sain saman virheilmoituksen kuin aiemminkin.
```
sudo systemctl restart apache2
```

![image](https://github.com/user-attachments/assets/6fde8fea-b908-4b6c-8033-e626670bc0a9)

Katsoin taas Apachen error.log -tiedostoa, sielläkin näkyi sama virhekoodi kuin aikaisemmin.

![image](https://github.com/user-attachments/assets/ffdbc4ec-4725-441a-a128-b91908866f0a)

Tarkastelin taas tekemääni .conf -tiedostoa. Huomasin siellä kirjoitusvirheen, joten korjasin tämän.

![image](https://github.com/user-attachments/assets/3a8d79d2-4637-49b2-b66b-55225c7d077c)

Käynnistin Apachen taas uudelleen. Tällä kertaa ei tullut virheilmoituksia ja selaimella tarkistettuna kotihakemistooni kirjoittaman html-tiedoston sisältö näkyi.

![image](https://github.com/user-attachments/assets/aaff846f-6766-4dee-81ce-4dffe52c3419)

Ongelma johtui siis ilmeisesti kirjoitusvirheestä konfiguraatiotiedostossani. Saamistani virheilmoituksistani en osannut päätellä, että tämä oli ongelman syynä.


## c) Validien html-sivujen luominen

2025-02-16 klo 18:25 - 18:40

Tein virtuaalikoneellani sivut index.html, blog.html, projects.html ja laitoin kaikki linkittämään toisiinsa. Käytin validaattoria sivujen html:n tarkistamiseen [3].

Käytin sivujen kopiointiin palvelimelle scp-komentoa [1]. Sekä kopioitavan virtuaalikoneellani olevan kansion, että virtuaalipalvelimella kohteessa olevan kansion nimi oli tässä vaiheessa toniblom.me, vaihdoin virtuaalipalvelimella olevan kansion nimeä, jotta tästä ei seuraisi komentoa ajaessa ongelmia. Komennon suoritus onnistui.
```
~publicsites$ scp -r toniblom.me/ toni@toniblom.me:public_sites/
```

![image](https://github.com/user-attachments/assets/fed578b2-8df6-4cd8-bcdb-caeaed3410ef)

Tarkistin selaimella, että tekemäni sivut näkyivät domain-nimelläni.

![image](https://github.com/user-attachments/assets/68eb8e21-ea45-4cf0-90d5-01b19cab3706)

## DNS-tietueiden teoriaa

Domain Name System (DNS) on ikään kuin internetin puhelinluettelo; se yhdistää helposti luettavat domain-nimet IP-osoitteiden (numero)merkkisarjoihin. **DNS-tietueet** sisältävät tietoja ja ohjeita, joiden perusteella DNS-palvelimet käsittelevät DNS-pyyntöjä. DNS-tietueet on kirjattu vyöhyketiedostoihin (zone files).

Yleisiä DNS-tietueita ovat mm. seuraavat:
* **A-tietue**: Yhdistää domain-nimen ja IPv4-osoitteen.
* **AAAA-tietue**: Yhdistää domain-nimen ja IPv6-osoitteen.
* **CNAME-tietue** (canonical name records): Linkittää alidomaineja A- tai AAAA-tietueisiin.
* **MX-tietue** (mail exchange records): Nämä tietueet yhdessä sähköpostipalvelimen kanssa mahdollistavat domain-nimeen linkittyneiden sähköpostitilien luomisen (esim. user@example.com -sähköpostiosoite example.com -domainista).
* **NS-tietue** (name server records): Kertoo domain-nimen auktoritatiivisen nimipalvelimen.
* **SOA-tietue** (start of authority records): Sisältää domainin hallinnollisia tietoja, mm. domainista vastaavan sähköpostiosoitteen.
* **TXT-tietue** (text records): Tekstimuotoista tietoa liittyen domainiin ja alidomaineihin. Vastaanotettujen sähköpostien luotettavuuden arviointiin ja verifikaatioon käytettävät SPF-tietueet ja DMARC-tietueet tallennetaan TXT-tietueisiin. [6]

## d) Alidomainien tekeminen

2025-02-16 klo 18:40 - 18:54

Olin jo aiemmin tehnyt www-alidomainin A-tietueella, testasin selaimella, että se toimi.

![image](https://github.com/user-attachments/assets/eceb3b40-7ae1-406f-a71c-480791b3a69e)

CNAME-tietueen tekemiseksi menin takaisin Namecheapin sivuille ja sieltä domain-nimeni kohtaan "Advanced DNS". Lisäsin uuden tietueen seuraavilla tiedoilla:
* Type: CNAME record / Host: home / Target: toniblom.me / TTL: 5 min

Hetken kuluttua kokeilin internetselaimella osoitetta home.toniblom.me, ja sivustoni näkyi.

![image](https://github.com/user-attachments/assets/db165b6c-9f5e-44f2-900c-10f05f66d585)


## dig ja host -komentojen teoriaa

Sekä dig että host -komento soveltuvat DNS-tietojen kyselyihin nimipalvelimilta [7][8]. host on yksinkertainen komento, jota yleensä käytetään selvittämään IP-osoite domain-nimen perusteella tai päinvastoin [7]. dig-komennolla on paljon erilaisia toiminnallisuuksia. Ilman parametrejä dig-komento näyttää vain A-tietueet, kaikkia tietueita voi kysellä antamalla komennolle ANY-parametrin. [8][9] Sekä dig että host-komennolle voi antaa argumenttina tietyn nimipalvelimen nimen tai IP-osoitteen, jolloin kysely kohdistuu tähän nimipalvelimeen. Muutoin näiden komentojen kyselyt kohdistuvat `/etc/resolv.conf` -tiedostossa listattuihin palvelimiin. [7][8]

## e) dig ja host -komentojen testaaminen

2025-02-26 klo 18:54 - 19:07

host-komennolla omalla domain-nimelläni tuli näkyviin IPv4-osoite ja sähköpostipalvelimien tietoja, jotka näyttivät samoilta kuin dig-komennon MX-tietueissa mainitut. dig-komennolla näkyi A-, NS-, MX- ja SOA-tietueita. Käytin dig-komennossa ANY -parametriä, jolloin kaikki tietueet näkyivät. Esim. aiemmin tekemääni CNAME-tietuetta ei kuitenkaan näkynyt tuloksissa.
```
host toniblom.me
dig toniblom.me ANY
```

![image](https://github.com/user-attachments/assets/c7ab118d-0e01-4137-bf85-d4519e8ed893)

![image](https://github.com/user-attachments/assets/b2640074-7c6f-4877-9aa7-494e3ac618cf)

dig-komennon tuloksien alussa oli tietoja DNS-pyyntöön liittyen, mm. tehtyjen kyselyiden (QUERY: 1) ja saatujen vastauksien (ANSWER: 9) määrä. OPT PSEUDOSECTION -osiossa näkyi mm. EDNS, joka tarkoittaa DNS:n laajennuksen versiota. QUESTION SECTION -osiossa näkyi tekemäni kyselyn tiedot. [9]

ANSWER SECTION-osiossa esim. viimeisellä rivillä näkyi seuraavaa:
* **toniblom.me**: Domain-nimi, johon kysely kohdistui.
* **366**: TTL eli Time to Live kertoo kuinka kauan tietuetta säilytetään välimuistissa.
* **IN**: Kyselyn luokka, IN tarkoittaa internetkyselyä.
* **NS**: Tietuetyyppi, jota kysyttiin. Tällä rivillä siis NS-tietue eli auktoritatiivisen nimipalvelimen tiedot. [9]

dig-komennon lopussa oli vielä tilastoja kyselyyn liittyen [9].

Kokeilin samoja komentoja pienen yrityksen internetsivuilla, tulokset vaikuttivat monilta osin samanlaisilta kuin omalla sivullani. host-komennolla näkyi IPv4-osoite ja sähköpostipalvelimien tietoja.

dig-komennon vastausosiossa näkyi A-, MX- ja SOA-tietueita, NS-tietueita sen sijaan ei tullut näkyviin. Sekä omalla domainillani että yrityksen domainilla MX-tietueiden TTL-arvo vaikutti olevan korkeampi verrattuna esim. A-tietueisiin, mikä tarkoittanee, että MX-tietueita päivitetään yleensä harvemmin kuin A-tietueita. Yrityksen sähköpostipalvelimet vaikuttivat MX-tietueiden mukaan olevan pitkälti Googlella.

![image](https://github.com/user-attachments/assets/eff0c2a2-ba06-4f2f-b763-a13d2dca7b96)

![image](https://github.com/user-attachments/assets/80b0dda7-cd84-4b26-be8a-8b56e8daa8d2)

Kokeilin samoja komentoja vielä Googlen sivuilla. host-komennossa näkyi nyt myös IPv6-osoite. dig-komennolla näkyi paljon enemmän erilaisia tietueita; myös AAAA-, HTTPS- ja paljon TXT-tietueita. Kuvausten perusteella TXT-tietueet liittyivät pitkälti erilaisiin verifikaatioihin. HTTPS-tietue kertoo, että google.com -osoitteeseen voi ottaa salatun yhteyden [10].

![image](https://github.com/user-attachments/assets/da14c921-321d-4a6c-8790-d31caf405f96)

![image](https://github.com/user-attachments/assets/449e2768-53c3-4237-a6f9-6b94f9d8cede)

## Tiivistelmä

Hankin onnistuneesti domain-nimen namecheap.com -sivustolta GitHub Education -paketin avulla ja laitoin domain-nimen tietueet osoittamaan aiemmin vuokraamaani virtuaalipalvelimeen. Asetin virtuaalipalvelimen Apache-serverille name-based virtual host -palvelun uudella domain-nimelläni. Tein yksinkertaiset html-sivut, jotka kopioin virtuaalipalvelimelle scp-komennolla. Laitoin domain-nimelleni kaksi alidomainia, toisen A-tietueella ja toisen CNAME-tietueella. Lopuksi testasin host- ja dig-komentoja kolmella erityyppisellä domainilla ja tutkin näiden tuloksia.

## Lähteet

[1] Karvinen, T. Linux Palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/. Luettu 2025-02-16.

[2] GeeksforGeeks. 2024-06-06. How to Start, Stop, or Restart Apache Server on Ubuntu?. https://www.geeksforgeeks.org/how-to-start-stop-or-restart-apache-server-on-ubuntu/. Luettu 2025-02-16.

[3] World Wide Web Consortium. W3C®. Markup Validation Service. https://validator.w3.org/#validate_by_input. Luettu 2025-02-16.

[4] Karvinen, T: Oppitunnit 2015-02-11, Linux palvelimet -kurssi (https://terokarvinen.com/linux-palvelimet/).

[5] Blom, T. h4 Maailma kuulee -raportti. https://github.com/toniblom/linux-course/blob/main/h4-raportti.md. Luettu 2025-02-16.

[6] Quiroz-Vázquez, C. Goodwin, M. 2024-01-22. What are DNS records? https://www.ibm.com/think/topics/dns-records. Luettu 2025-02-16.

[7] Canonical Ltd. 2019. https://manpages.ubuntu.com/manpages/noble/man1/host.1.html. Luettu 2025-02-16.

[8] Canonical Ltd. 2019. https://manpages.ubuntu.com/manpages/noble/man1/dig.1.html. Luettu 2025-02-16.

[9] McKay, D. 2024-02-05. How to Use the dig Command on Linux. https://www.howtogeek.com/663056/how-to-use-the-dig-command-on-linux/. Luettu 2025-02-16.

[10] Ghedini, A. 2020-09-30. Speeding up HTTPS and HTTP/3 negotiation with... DNS. https://blog.cloudflare.com/speeding-up-https-and-http-3-negotiation-with-dns/. Luettu 2025-02-16.
