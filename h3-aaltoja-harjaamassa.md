# H3 – Aaltoja harjaamassa

## x) Lue ja tiivistä

### Hubacek 2019 – Universal Radio Hacker SDR Tutorial

- SDR-laitteella voidaan vastaanottaa ja tutkia radiosignaaleja.
- 433 MHz:n taajuutta käytetään esimerkiksi langattomissa pistorasioissa ja kaukosäätimissä.
- URH-ohjelmalla voidaan tarkastella signaaleja ja niiden sisältämiä bittejä.
- Signaalista voidaan selvittää esimerkiksi, mitä tapahtuu, kun kaukosäätimen nappia painetaan.

### Cornelius 2022 – Decode 433.92 MHz weather station data

- 433,92 MHz:n taajuudella toimivat esimerkiksi sääasemat ja erilaiset langattomat laitteet.
- rtl_433-ohjelmalla voidaan analysoida radiosignaaleja automaattisesti.
- Ohjelma voi näyttää esimerkiksi laitteen tunnisteen ja muita lähetettyjä tietoja.
- Kaikkia signaaleja ei kuitenkaan aina tunnisteta automaattisesti.

## b) rtl_433

Asensin rtl_433-ohjelman Debianiin VirtualBoxissa.

Käytin komentoa:

```bash
sudo apt install rtl-433 -y
```

Tämän jälkeen tarkistin ohjelman toiminnan komennolla:

```bash
rtl_433
```


Ohjelma käynnistyi ja sen versioksi tuli 25.02.

Terminaalissa näkyi ilmoitus `No supported devices found`, koska virtuaalikoneeseen ei ollut yhdistetty SDR-vastaanotinta. Tämä ei kuitenkaan estänyt valmiiden tiedostojen analysointia.

<img width="782" height="77" alt="Näyttökuva 2026-11-09 002132" src="https://github.com/user-attachments/assets/e079d2a9-503f-4d71-b7f9-9e5148a072a8" />

## c) Automaattinen analyysi

Latasin opettajan antaman tiedoston `Converted_433.92M_2000k.cs8`.

Ohjelma tunnisti signaalista kolme eri protokollaa:

- KlikAanKlikUit-Switch
- Proove-Security
- Nexa-Security

Tuloksissa näkyi myös seuraavia tietoja:

- ID: 8785315
- Channel: 3
- State: OFF
- Unit: 3
- Group Call: No

Huomasin, että sama tunniste näkyi useamman protokollan tuloksissa. Signaali liittyy todennäköisesti langattoman laitteen ohjaamiseen.

Komento `Off` tarkoittaa, että signaalilla lähetetään sammutuskäsky.

<img width="962" height="622" alt="Näyttökuva 2026-11-09 002630" src="https://github.com/user-attachments/assets/23e059f5-60e5-430a-818a-c5511445d977" />

## d) Too complex 16?

Tunnilla tallensimme radiosignaalin RTL-SDR-laitteella. Tallennettu tiedosto oli RTL-SDR-20261103_133951-433_92MHz-1MSps-1MHz.complex16s.

Näytteen taajuus oli 433,92 MHz ja sample rate oli 1 MSps.

Tiedosto oli complex16s-muodossa. Se sisältää I/Q-näytteitä, jotka pitää muuntaa rtl_433-ohjelmalle sopivaan muotoon.

Tämän jälkeen muunnettua tiedostoa voidaan analysoida rtl_433-ohjelmalla ja yrittää tunnistaa radiosignaalin sisältöä.

## e) Ultimate Radio Hacker

Asensin Universal Radio Hacker (URH) -ohjelman Debianiin.

sudo apt install urh -y

Asennuksen jälkeen käynnistin ohjelman ja avasin opettajan antaman signaalitiedoston.

Ohjelma käynnistyi onnistuneesti ja pystyin tarkastelemaan radiosignaalia.

## f) Yleiskuva

Avasin URH-ohjelmassa tiedoston 1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s.

Tarkastelin signaalia ja sen rakennetta.

Näytteen tiedot:

- Taajuus: 433,912 MHz
- Näytteenottotaajuus: 2 MSps
- Tallennuspäivä: 12.04.2025
- Tallennusaika: 11:38:05
- Tiedoston koko: 10 983 160 tavua
- Näytteen kesto: noin 1,37 sekuntia

Signaalissa näkyi kolme erillistä pulssiryhmää. Tehtävänannon mukaan Nexan kaukosäätimen ON-nappia painettiin kolme kertaa. 
Tämä näkyy myös signaalissa kolmena erillisenä ryhmänä. Ryhmien välissä näkyy taukoja.

<img width="1057" height="692" alt="Näyttökuva 2026-11-09 003915" src="https://github.com/user-attachments/assets/29064dfb-ce05-4e90-b47f-ecb7554de386" />

## g) Bittistä

Tutkin samaa signaalia URH-ohjelmassa ja tarkastelin sen raakabittejä.

Käytin ohjelman automaattista parametrien tunnistusta.

URH näytti signaalista nollia ja ykkösiä sekä taukoja signaaliryhmien välillä.

Ohjelmassa näkyivät seuraavat asetukset:

- Modulaatio: ASK
- Bits/Symbol: 1
- Samples/Symbol: 500
- Sample rate: 2 MSps

ASK tarkoittaa Amplitude Shift Keying -modulaatiota. Siinä tietoa lähetetään muuttamalla signaalin amplitudia.

Yhden raakabitin kesto:

500 / 2 000 000 = 0,00025 sekuntia.

Yhden raakabitin kesto on siis noin **250 mikrosekuntia eli 0,25 millisekuntia**.

Se on todella lyhyt aika, paljon lyhyempi kuin esimerkiksi silmänräpäys.

URH:n avulla pystyin näkemään, miten radiosignaali voidaan muuttaa nolliksi ja ykkösiksi.

<img width="245" height="86" alt="image" src="https://github.com/user-attachments/assets/aaafd343-1f89-4057-8260-82f8de402979" />


<img width="830" height="222" alt="Näyttökuva 2026-11-09 005034" src="https://github.com/user-attachments/assets/5a69c46c-de4a-40c6-b6c6-3cca93da032c" />




## Lähteet

- Hubacek, 2019. Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs. Luettu 8.11.2026
- Cornelius, 2022. Decode 433.92 MHz weather station data. Luettu 8.11.2026
- Karvinen, T. Kurssimateriaalit ja tehtävänanto. Luettu 8.11.2026
- ChatGPT:tä käytetty apuna komentojen ymmärtämisessä, ohjelmien asentamisessa, tulosten tulkitsemisessa ja raportin tekstin korjaamisessa.
- https://telcochisel.sourceforge.io/tools/urh/ Luettu 8.11.2026
