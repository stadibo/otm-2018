# EI AJANTASALLA, lukeminen omalla vastuulla

# Viikon 2 tehtävät

## 1 

* tutustu [JUnit-ohjeeseen](https://github.com/mluukkai/otm-2018/blob/master/web/junit.md)
* lukiessasi tee testit myös itse
* lisää lopuksi maksukortille seuraavat testit:
  * maukkaan lounaan syöminen ei vie saldoa negatiiviseksi, ota tähän mallia testistä syoEdullisestiEiVieSaldoaNegatiiviseksi
  * negatiivisen summan lataaminen ei muuta kortin saldoa
  * kortilla pystyy ostamaan edullisen lounaan kun kortilla rahaa vain edullisen lounaan verran (eli 2.5e)
  * kortilla pystyy ostamaan maukkaan lounaan kun kortilla rahaa vain maukkaan lounaan verran (eli 4e)

**HUOM1** on suositeltavaa, että yksi testi testaa vaan "yhtä asiaa" kerrallaan. Tee siis jokaisesta ylläolevasta oma testinsä.

**HUOM2** Kirjoita assertEquals-komennot aina siten, että ensimmäisenä parametrina on odotettu tulos ja toisena parametrina testatun metodin antama tulos.
## 2

Ohjelmoinnin perusteiden eräässä tehtävässä (ks. [tehtävänanto](https://www.cs.helsinki.fi/group/java/s16-materiaali/viikko4/#91kello_laskurin_avulla) ) ohjelmoitiin luokka YlhaaltaRajoitettuLaskuri:

``` java
public class YlhaaltaRajoitettuLaskuri {
 
    private int arvo;
    private int ylaraja;
 
    public YlhaaltaRajoitettuLaskuri(int ylarajanAlkuarvo) {
        this.ylaraja = ylarajanAlkuarvo;
        this.arvo = 0;
    }
 
    public void seuraava() {
        if (this.arvo == this.ylaraja) {
            this.arvo = 0;
        } else {
            this.arvo++;
        }
    }
 
    public int arvo() {
        return this.arvo;
    }
 
    public void asetaArvo(int uusiArvo) {
        if (uusiArvo < 0 || uusiArvo > this.ylaraja) {
            return;
        }
 
        this.arvo = uusiArvo;
    }
 
    @Override
    public String toString() {
        String etunolla = "0";
        if (this.arvo > 9) {
            etunolla = "";
        }
        return etunolla + this.arvo;
    }
}
```

Tee laskurille seuraavat testit:
* luodun laskurin alkuarvo on 0
  * huom: arvo kannattaa varmistaa testeissä kutsumalla laskurin metodia _arvo_
* kun laskuri etenee kerran, sen arvo on 1
* kun laskuri etenee kaksi kertaa, sen arvo on 2
* jos laskurin yläraja on n ja laskuri etenee n kertaa, on laskurin arvo n
  * korvaa testeissä n jollain konkreettisella arvolla, esim. 4
* jos laskurin yläraja on n ja laskuri etenee n+1 kertaa, on laskurin arvo 0
* jos laskurin yläraja on n ja laskuri etenee n+2 kertaa, on laskurin arvo 1
* metodi asetaArvo asettaa laskurin arvon oikein jos parametrin arvo on välillä 0 - laskurin yläraja
* metodi asetaArvo ei tee mitään jos parametrin arvo ei ole välillä 0 - laskurin yläraja
* toString tuottaa etunollan jos laskurin arvo on alle 10
* toString ei tuota etunollaa jos laskurin arvo on vähintään 10

**HUOM** on suositeltavaa, että yksi testi testaa vaan "yhtä asiaa" kerrallaan. Tee siis jokaisesta ylläolevasta oma testinsä.

## 2 Maksukortti ja kassapääte: testit kortille

Ohjelmoinnin perusteiden viikolla [5 tehtävissä 101](https://www.cs.helsinki.fi/group/java/s16-materiaali/viikko5/#101maksukortti_ja_kassapaate) toteutettiin "tyhmä" Maksukortti ja Kassapääte. 

Rahan käsittely double-tyyppisenä on hiukan ongelmallista. Seuraavassa rahaa käsitellän kokonaislukuna ja kaikki rahamäärät talletetaan eurojen sijasta sentteinä.

Ohjelmointiuransa aloittelevan tuttavasi vastaus seuraavassa:

``` java
public class Maksukortti {
 
    private int saldo;
 
    public Maksukortti(int saldo) {
        this.saldo = saldo;
    }
 
    public int saldo() {
        return saldo;
    }
 
    public void lataaRahaa(int lisays) {
        this.saldo += lisays;
    }
 
    public boolean otaRahaa(int maara) {
        if (this.saldo < maara)
            return false;
 
        this.saldo = this.saldo - maara;
        return true;
    }

    @Override
    public String toString() {
        int euroa = saldo/100;
        int senttia = saldo%100;
        return "saldo: "+euroa+"."+senttia;
    }    
}
```

Kassapäätteelle on lisätty metodit, jotka mahdollistavat myytyjen lounaiden määrän ja kassassa olevan rahamäärän kysymisen, toString()-metodi on poistettu:

``` java
public class Kassapaate {
 
    private int kassassaRahaa;
    private int edulliset;
    private int maukkaat;
 
    public Kassapaate() {
        this.kassassaRahaa = 100000;
    }
 
    public int syoEdullisesti(int maksu) {
        if (maksu >= 240) {
            this.kassassaRahaa = kassassaRahaa + 240;
            ++this.edulliset;
            return maksu - 240;
        } else {
            return maksu;
        }
    }
 
    public int syoMaukkaasti(int maksu) {
        if (maksu >= 400) {
            this.kassassaRahaa = kassassaRahaa + 400;
            this.maukkaat++;
            return maksu - 400;
        } else {
            return maksu;
        }
    }
 
    public boolean syoEdullisesti(Maksukortti kortti) {
        if (kortti.saldo() >= 240) {
            kortti.otaRahaa(240);
            this.edulliset++;
            return true;
        } else {
            return false;
        }
    }
 
    public boolean syoMaukkaasti(Maksukortti kortti) {
        if (kortti.saldo() >= 400) {
            kortti.otaRahaa(400);
            this.maukkaat++;
            return true;
        } else {
            return false;
        }
    }
 
    public void lataaRahaaKortille(Maksukortti kortti, int summa) {
        if (summa >= 0) {
            kortti.lataaRahaa(summa);
            this.kassassaRahaa += summa;
        } else {
            return;
        }
    }
 
    public int kassassaRahaa() {
        return kassassaRahaa;
    }
 
    public int maukkaitaLounaitaMyyty() {
        return maukkaat;
    }
 
    public int edullisiaLounaitaMyyty() {
        return edulliset;
    }
}
```

Jos olet toiminut tämän viikon [kotona tehtävien laskareiden tehtävän 4](https://github.com/mluukkai/OTM2016/wiki/Viikon-2-kotitehtavat#4) ohjeiden mukaan, koodin pitäisi löytyä kotihakemistosi alta hakemistopolulta _kurssit/otm2016/viikko2_

**HUOM: jos teit tehtävän ennen keskiviikkoaamua, hakemasi projektin versio oli väärä ja on parasta että haet zip-paketin uudelleen.**

Jos et tehnyt tehtävää (tai teit sen ennen keskiviikkoa...), niin avaa terminaali ja luo sopiva hakemisto. Mene hakemistoon ja anna seuraavat komennot 

``` bash
wget https://www.cs.helsinki.fi/u/mluukkai/otm2016/Unicafe.zip
unzip Unicafe.zip
```

Avaa projekti NetBeansilla.

Olemme tähän asti suorittaneet testit NetBeansilla. Kokeillaan nyt miten testit voidaan suorittaa _komentoriviltä_.
* avaa terminaali (joka löytyy avaamalla vasemman yläkulman etsintäikkuna ja kirjoittamalla _terminal_)
* mene hakemistoon, jossa projekti sijaitsee
* suorita testit antamalla komento _mvn test_
  * huomaa, että komento _mvn_ tulee antaa aina projektin juurihakemistossa, eli samassa hakemistossa, jossa sijaitsee tiedosto _pom.xml_
  * muista, että näet hakemiston sisällön komennolla _ls_

*Tee valmiiseen testiluokkaan MaksukorttiTest* testit, jotka testaavat ainakin seuraavia asioita:
* saldo alussa oikein
* rahan lataaminen kasvattaa saldoa oikein
* rahan ottaminen toimii:
  * saldo vähenee oikein jos rahaa on tarpeeksi
  * saldo ei muutu jos rahaa ei ole tarpeeksi
  * metodi palauttaa _true_ jos rahat riittivät ja muuten _false_

Voit suorittaa testit NetBeansilla tai komentoriviltä

## Maven

Suoritimme testit komentoriviltä komennolla _mvn test_. Mistä on kyse? Maksukortin sisältämä projekti on määritelty [maven](https://maven.apache.org)-formaatissa. Maven on työkalu, jonka avulla voidaan hallita Javalla tehtyjen ohjelmien kääntämistä, testien suorittamista ja paljon muitakin työvaiheita, kuten pian tulemme näkemään. Ohjelmoinnin perusteiden ja jatkokurssien tehtäviä ei ole määritelty mavenilla vaan hieman vanhemmassa [ant](http://ant.apache.org)-formaatissa. Maven on kuitenkin nykyään suositeltavampi, ja tekee monet asiat käyttäjän kannalta helpommaksi. 

Maven-projektien juuressa on tiedosto _pom.xml_, joka sisältää projektin konfiguraatiot. Katso miltä tiedosto näyttää. Tiedosto löytyy NetBeansista kohdan _project files_ alta.

Tiedostossa on määritelty, että projektilla on testejä suoritettaessa _riippuvuutena_ JUnit-kirjaston versio 4.10:

``` java
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

Tiedostossa _plugin_-osiossa on määritelty, että koodi käännetään javan uusinta versiota, eli "java kasia" käyttäen. Maven käyttää java kasista numeroa 1.8 (kohdat _source_ ja _target_):

``` java
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
                <version>2.5</version>
            </plugin>
            ...
        </plugins>
    </build>
``` 

## 3 Testauskattavuus

Olemme tyytyväisiä, uskomme että testitapauksia on nyt tarpeeksi. Onko tosiaan näin? Onneksi on olemassa työkaluja, joilla voidaan tarkastaa testien lause- ja haarautumakattavuus. __Lausekattavuus__ mittaa mitä koodirivejä testien suorittaminen on tutkinut. Täydellinen lausekattavuuskaan ei tietenkään takaa että ohjelma toimii oikein, mutta on parempi kuin ei mitään. __Haarautumakattavuus__ taas mittaa mitä eri suoritushaaroja koodista on käyty läpi. Suoritushaaroilla tarkoitetaan esim. if-komentojen valintatilanteita. 

**FIX: vaihda jacocoon**

Projektiin on valmiiksi konfiguroitu käytettäväksi [Cobertura](http://cobertura.github.io/cobertura/) joka mittaa sekä lause- että haarautumakattavuuden. Määrittely on tiedoston pom.xml osiossa _plugins_:

``` java
    <build>
        <plugins>
            ...
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>cobertura-maven-plugin</artifactId>
                <version>2.7</version>
            </plugin>
            ...
        </plugins>
    </build>
``` 

_cobertura_ suoritetaan komentoriviltä (projektihakemistossa ollessasi) komennolla <code>mvn cobertura:cobertura</code>

Tulokset tulevat projektihakemistosi alihakemistoon __target/site/cobertura/index.html__. Avaa tulokset web-selaimella. Firefoxilla tämä tapahtuu komennolla __open file__. Voit myös avata selaimen terminaalissa menemällä ensin projektihakemistoon ja antamalla komennon __chromium-browser target/site/cobertura/index.html__ 

**Jos maksukortin koodissa on vielä rivejä tai haarautumia (merkitty punaisella) joille ei ole testiä, kirjoita sopivat testit.**

## 4 Maven-projektin hakemistorakenne

Maven-projektien hakemiston rakenne noudattaa aina samaa logiikkaa. Hakemiston juuressa on projektin konfiguraatiot sisältävä tiedosto _pom.xml_. Ohjelman lähdekoodi on hakemistossa _src/java_ ja testit hakemistossa _src/test_. Hakemistoon _target_ generoituvat kaikki maven-komentojen aikaansannokset. 

Suorita komento _mvn clean_. Komento poistaa hakemiston _target_. 

Suorita projektin koodin käännöksen suorittava komento _mvn compile_. Tutki mitä hakemiston _target_ sisälle syntyy.

Suorita testit komennolla _mvn test_. Tutki mitä hakemistosta _target_ löytyy komennon suorittamisen jälkeen.

## 5 Kassapäätteen testit

Laajennetaan Unicafen testaus kattamaan myös kassapääte.

*Tee testiluokka _KassapaateTest_ ja tee testit jotka testaavat ainakin seuraavia asioita:*
* luodun kassapäätteen rahamäärä ja myytyjen lounaiden määrä on oikea (rahaa 1000, lounaita myyty 0)
* käteisosto toimii sekä edullisten että maukkaiden lounaiden osalta
  * jos maksu riittävä: kassassa oleva rahamäärä kasvaa lounaan hinnalla ja vaihtorahan suuruus on oikea
  * jos maksu on riittävä: myytyjen lounaiden määrä kasvaa
  * jos maksu ei ole riittävä: kassassa oleva rahamäärä ei muutu, kaikki rahat palautetaan vaihtorahana ja myytyjen lounaiden määrässä ei muutosta
* _seuraavissa testeissä tarvitaan myös Maksukorttia jonka oletetaan toimivan oikein_
* korttiosto toimii sekä edullisten että maukkaiden lounaiden osalta
  * jos kortilla on tarpeeksi rahaa, veloitetan summa kortilta ja palautetaan true
  * jos kortilla on tarpeeksi rahaa, myytyjen lounaiden määrä kasvaa
  * jos kortilla ei ole tarpeeksi rahaa, kortin rahamäärä ei muutu, myytyjen lounaiden määrä muuttumaton ja palautetaan false
  * kassassa oleva rahamäärä ei muutu kortilla ostettaessa
* kortille rahaa ladattaessa kortin saldo muuttuu ja kassassa oleva rahamäärä kasvaa ladatulla summalla

Huomaat että kassapääte sisältää melkoisen määrän "copypastea". Nyt kun kassapäätteellä on automaattiset testit, on sen rakennetta helppo muokata eli refaktoroida siistimmäksi koko ajan kuitenkin varmistaen, että testit menevät läpi. Tulemme tekemään refaktoroinnin myöhemmin kurssilla.

## 6

Varmista että kassapäätteen teksteillä on 100% lause- ja haarautumakattavuus.