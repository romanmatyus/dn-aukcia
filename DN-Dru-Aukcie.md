% Detailný návrh služby DRU - Aukcia
% Roman Mátyus

#Cieľ riešenia

Cieľom projektu je zabezpečiť firme DRU, a. s. (ďalej objednávateľ) webovú aplikáciu na vyhlasovanie reverzných aukcií a jej dodávateľom možnosť aukcie sa zúčastniť.

Podmienkou dodania je licencovanie produktu pod otvorenou licenciou BSD. Zdrojový kód musí byť jasne štrukturovaný a komentovaný.

#Charakteristika dokumentu

##Forma

V texte sa nachádzajú slová v zložených zátvorkách, ktoré budú vo výslednej aplikácií nahradené za zodpovedajúci údaj. Napríklad {MENO} bude v aplikácií nahradené za meno klienta, spoločnosti, prípadne iné podľa kontextu.

##Účel

Dokument slúži ako záväzný podklad pre vypracovanie aplikácie. Akékoľvek zmeny funkčnosti vykonané vykonávateľom musia byť vopred odsúhlasené objednávateľom. Zmeny by mali mať iba účel zjednodušenia implementácie alebo zvýšenia komfortu používania aplikácie.

Zmeny navrhnuté objednávateľom po odsúhlasení tohoto dokumentu budú spoplatnené zvlášť, resp. doplatkom.

Funkcionalita systému bude pred odovzdaním riadne preverená dodávateľom. Po odovzdaní predmetného diela je objednávateľ povinný skontrolovať dodané dielo a nahlásiť nezrovnalosti oproti detailnému návrhu. Tieto budú opravené na náklady dodávateľa. 

#Popis riešenia

##Schéma

Schémy popisovaného riešenia sú v prílohách:

- základné akcie používateľov - spolocne-akcie.png
- akcie klientov - klientske-akcie.png
- akcie operátorov - akcie-operatora.png
- akcie kontrolórov - akcie-kontrolora.png

##Lokalizácia

Aplikácia nebude disponovať možnosťou zmeny lokalizácie. Všetky zobrazované texty budú iba v slovenskom jazyku. Aplikácia môže obsahovať predprípravu pre budúcu lokalizáciu.

##Prístupové práva

Systém bude obsahovať detailné možnosti nastavenia prístupových práv. Preddefinovane bude obsahovať skupiny používateľov, ktoré budú môcť byť v systéme detailne nastavené, prípadne doplnené alebo odstránené. Práva budú môcť byť špecifické aj na úrovni jednotlivých používateľov.

Každá akcia, resp. stránka bude mať predvolene nastavené oprávnenie k prístupu (zákaz/povolenie) priamo v kóde aplikácie. V prípade existencie pravidla pre skupinu do ktorej používateľ patrí, pravidlo prepíše predvolené nastavenie prístupu. Ak je na konkrétnu akciu definované pravidlo pre konkrétneho používateľa, je aplikované, pretože má najvyššiu prioritu. Postupnosť priorít je:

1. Práva nastavené používateľovi
2. Práva nastavené skupine do ktorej používateľ patrí
3. Predvolene nastavené práva v konfigurácií aplikácie

V prípade snahy o prístup do zabezpečenej časti aplikácie neprihláseným používateľom je používateľ s vyzvaním "Pokúšate sa vykonať akciu na ktorú nemáte oprávnenie" presmerovaný na stránku s prihlasovacím formulárom. V prípade ak je používateľ prihlásený, tak je zobrazená chybová stránka 403 Access Denied s oznámením "Pokúšate sa zobraziť obsah na ktorý nemáte oprávnenie.". Takýto incident je automaticky archivovaný.

###Návštevník

Toto právo sa vzťahuje na neprihláseného návštevníka webaplikácie. Tento človek má právo vidieť pripravované aukcie (t.j. vytvorené ale neskončené), vrátane ich údajov, nesmie sa však zapojiť do aukcie.

###Súťažiaci

Do tejto skupiny používateľov patria prihlásené firmy. Cieľom skupiny je umožniť používateľom zapojenie sa do aukcií.

###Operátor

Túto skupinu prideľuje administrátor. Skupina je určená pre zamestnancov firmy - správcov jednotlivých kategórií aukcií. Môžu zadávať, upravovať a mazať aukcie svojej kategórie, počas priebehu aukcie však nemôžu vidieť priebeh súťaže s údajmi. Môžu vidieť len počet uchádzačov zúčastnených v aukcii.

###Kontrolór

Ľudia čo budú schvaľovať zverejnenie aukcie ako takej. Ďalej budú ako prvý informovaní o výsledkoch aukcie a odsúhlasia zverejnenie výsledkov aukcie pre operátorov. t.j. operátor si bude môcť zobraziť výsledok aukcie až po odsúhlasení touto kategóriou.

###Správca

Toto právo sa vzťahuje na poverenú osobu spravujúcu aplikáciu bez obmedzení.

#Popis podstránok

##Používateľský účet

###Prihlasovanie

Ku stránke bude mať prístup iba Návštevník.

Prihlasovací formulár:

- Textové pole "Prihlasovacie meno"
	- s kontrolou na vyplnenie: "Zadajte prosím prihlasovacie meno."
	- s kontrolou na existenciu: "Zadali ste neexistujúce prihlasovacie meno."
- Textové pole "Heslo"
	- s kontrolou na vyplnenie: "Zadajte prosím heslo."
	- s kontrolou na správnosť: "Zadali ste nesprávne heslo."
- Odosielacie tlačidlo "Prihlásiť"

Odkaz na stránku [Registrácia](#registrácia).

Odkaz na stránku [Zabudnuté heslo](#zabudnuté-heslo).

Formulár bude obsahovať protispamové prvky.

Po odoslaní je používateľ prihlásený s oznámením "Boli Ste prihlásený.". Ak prihlásenie prebehne v poriadku, tak je používateľ presmerovaný na stránku [Zoznam aukcií](#zoznam-aukcií).

###Registrácia

Ktorýkoľvek neprihlásený používateľ.

Registračný formulár:

- Textové pole "IČO"
	- s kontrolou na vyplnenie: "Zadajte prosím IČO."
	- s kontrolou číselného typu: "IČO musí obsahovať iba čísla."
	- s kontrolou na počet znakov: "IČO musí byť 6 až 8 miestne číslo."
- Odosielacie tlačidlo "Načítať údaje podla IČO"
	- po kliknutí sa načítajú údaje do formulára z [orsr.sk](http://orsr.sk)/[zrsr.sk](http://www.zrsr.sk)
- Textové pole "Obchodné meno"
	- s kontrolou na vyplnenie: "Zadajte prosím Obchodné meno."
- Textové pole "Ulica"
	- s kontrolou na vyplnenie: "Zadajte prosím ulicu."
- Textové pole "Mesto"
	- s kontrolou na vyplnenie: "Zadajte prosím Mesto."
- Textové pole "PSČ"
	- s kontrolou na vyplnenie: "Zadajte prosím PSČ."
	- s kontrolou na počet znakov a čísla: "PSČ musí obsahovať 5 čísel."
- Textové pole "Meno oprávnenej osoby"
	- s kontrolou na vyplnenie: "Zadajte prosím meno oprávnenej osoby."
- Textové pole "Telefonický kontakt"
	- s kontrolou na vyplnenie: "Zadajte prosím telefónne číslo."
	- s kontrolou na správny formát telefónneho čísla: "Zadajte prosím platné telefónne číslo."
	  Číslo môže byť vo formáte 0907 123 456 (10 čísel) alebo +421 907 123 456 (+ a 12 čísel)
	- s medzerami aj bez nich
- Textové pole "Mobil"
	- s kontrolou na vyplnenie: "Zadajte prosím telefónne číslo mobilu."
	- s kontrolou na správny formát telefónneho čísla: "Zadajte prosím platné telefónne číslo."
	  Číslo môže byť vo formáte 0907 123 456 (10 čísiel) alebo +421 907 123 456 (+ a 12 čísiel)
	- s medzerami aj bez nich
- Viacriadkové textové pole "Referencie"
- Certifikáty a osvedčenia (polia na upload skenovaných osvedčení) -ak sa dá tak neobmedzený počet ak nie tak asi 4 polia
- Textové pole "Prihlasovacie meno"
	- s kontrolou na vyplnenie: "Zadajte prosím Prihlasovacie meno."
	- s kontrolou na počet znakov: "Prihlasovacie meno musí mať minimálne 5 znakov."
- Textové pole "E-mail"
	- s kontrolou na vyplnenie: "Zadajte prosím e-mail."
	- s kontrolou na správny formát mailu: "Zadajte prosím platný e-mail."
- Textové pole "Heslo"
	- s kontrolou na vyplnenie: "Zadajte prosím heslo."
	- s kontrolou na počet znakov: "Heslo musí mať minimálne 5 znakov."
- Textové pole "Zopakovať heslo"
	- s kontrolou na vyplnenie: "Zopakujte prosím heslo."
	- s kontrolou na rovnosť s prvým zadaným heslom: "Heslá sa musia zhodovať."
- Odosielacie tlačidlo "Registrovať"

Pri registrácii si bude môcť súťažiaci vybrať, či chce dostávať notifikácie o nových aukciách, pričom si bude môcť vybrať typy aukcií zo zoznamu.

Pri registrácií sa odošle [e-mail o registrácií](#mail-s-aktiváciou-účtu) a vyplnené údaje sa uložia.

V prípade ak sa používateľ pokúsi registrovať na už registrované prihlasovacie meno zobrazí sa oznámenie, "Prihlasovacie meno už existuje.". Ak je vyplnený mail už v systéme, je používateľ upozornený oznámením "Zvolený mail sa už v systéme nachádza." V prípade úspešnej registrácie sa zobrazí oznámenie "Registrácia bola úspešná.". Ak registrácia prebehne v poriadku, používateľ bude presmerovaný na stránku Aktivácia účtu.

Všetky údaje sú povinné okrem nahratia certifikátov. Pri odoslaní by sa malo overiť IČO voči www.orsr.sk. Formulár bude obsahovať protispamové prvky.

###Aktivácia účtu

Ku stránke bude mať prístup iba Návštevník.

Nadpis "Aktivácia účtu".

Popis na stránke:

"Pre aktiváciu účtu je potrebné vložiť autorizačný kód. Svoj autorizačný kód nájdete v maily ktorý Vám bol odoslaný po registrácií."

Formulár pre aktiváciu účtu:

- Textové pole "Aktivačný kód"
	- s kontrolou na vyplnenie: "Zadajte prosím Aktivačný kód."
- Odosielacie tlačidlo "Aktivovať účet"

Po odoslaní kódu sa aktivuje účet, ku ktorému bol aktivačný kód odoslaný. Po odoslaní je zobrazené oznámenie "Účet bol úspešne aktivovaný."

###Stránka pre zabudnuté heslo

Ku stránke bude mať prístup iba Návštevník.

Nadpis "Zaslanie nového hesla".

Formulár pre odoslanie nového hesla:
- Textové pole "Vaše prihlasovacie meno"
	- s kontrolou na vyplnenie: "Zadajte prosím Prihlasovacie meno."
	- s kontrolou na existenciu: "Zadali ste neregistrované Prihlasovacie meno."
- Odosielacie tlačidlo "Zaslať nové heslo"

Po odoslaní formulára sa vygeneruje nové heslo a na e-mailovú adresu patriacu k prihlasovaciemu menu sa odošle [e-mail](#mail-s-novým-heslom).

Po odoslaní e-mailu sa zobrazí oznámenie "Na email {EMAIL} Vám bolo zaslané nové heslo.". Používateľ nebude nikam presmerovaný.

###Údaje o zákazníkovi

Ku stránke bude mať prístup každý okrem Návštevníka.

Nadpis "Moje údaje".

Formulár pre aktualizáciu údajov o používateľovi. Formulár bude takmer totožný s registračným formulárom. Polia "E-mail", "Prihlasovacie meno", "Heslo" a "Zopakovať heslo" budú vynechané. Validačné pravidlá budú taktiež zhodné s registračným formulárom.

V prípade zobrazenia Správcom bude pred odosielacie tlačidlo pridaný selectbox "Skupina" s výberom skupiny oprávnenia používateľa.

Odosielacie tlačidlo bude nahradené za text "Uložiť".

Po odoslaní sa zmenené údaje uložia. Po spracovaní sa zobrazí oznámenie "Údaje boli uložené.". 

###Prihlasovacie údaje zákazníka

Ku stránke bude mať prístup každý okrem Návštevníka.

Nadpis "Prihlasovacie údaje".

Podnadpis "Zmena prihlasovacieho mena".

**Formulár pre zmenu prihlasovacieho mena:**

- Textové pole "Vaše aktuálne prihlasovacie meno"
	- meno je vyplnené, ale nedá sa zmeniť
- Textové pole "Vaše nové prihlasovacie meno"
	- s kontrolou na vyplnenie: "Zadajte prosím Nové prihlasovacie meno."
	- s kontrolou na existenciu: "Zadali ste neregistrované Prihlasovacie meno."
- Textové pole "Heslo"
	- s kontrolou na vyplnenie: "Zadajte prosím heslo."
	- s kontrolou na správnosť hesla: "Heslo nie je správne."
- Odosielacie tlačidlo "Zmeniť prihlasovacie m"no.

**Formulár pre zmenu hesla:**

- Textové pole "Aktuálne heslo"
	- s kontrolou na vyplnenie: "Zadajte prosím aktuálne heslo."
	- s kontrolou na správnosť hesla: "Aktuálne heslo nie je správne."
- Textové pole "Nové heslo"
	- s kontrolou na vyplnenie: "Zadajte prosím nové heslo."
	- s kontrolou na počet znakov: "Heslo musí mať minimálne 5 znakov."
- Textové pole "Zopakovať nové heslo"
	- s kontrolou na vyplnenie: "Zopakujte prosím nové heslo."
	- s kontrolou na rovnosť s prvým zadaným heslom: "Polia pre nové heslo sa musia zhodovať."
- Odosielacie tlačidlo "Zmeniť heslo."

Po úspešnom spracovaní niektorého z formulárov sa následne odošle na kontaktnú mailovú adresu [mail](#mail-s-novými-prihlasovacími-údajmi).

###Zoznam používateľov

Ku stránke bude mať prístup iba Správca.

Nadpis "Zoznam používateľov".

Obsahom bude tabuľka (GRID) obsahujúca zoznam používateľov systému so stĺpcami:

- Obchodné meno, obsahom bude "Obchodné meno" používateľa zadané pri registrácií
	- Možnosť zoradenia
	- Textový filter
- Kontakt, obsahom je "Meno oprávnenej osoby" používateľa zadané pri registrácií
- Mail, obsahom je "E-mail" používateľa zadaný pri registrácií
- Telefón, obsahom je "Telefonický kontakt" používateľa zadané pri registrácií
- Akcie
	- Upraviť
		- Po kliknutí bude Správca presmerovaný na Údaje o zákazníkovi.
	- Zmazať
		- Po kliknutí je používateľ zo systému odstránený.
		- Pred odstránením, je nutné potvrdiť kontrolnú otázku "Skutočne chcete odstrániť používateľa {MENO}?"
- Stránkovanie po 25, 50, 100

###Odhlásenie

Ku stránke môže pristupovať ktokoľvek.

Používateľ je odhlásený a presmerovaný na hlavnú stránku.

##Skupiny používateľov

Sekcia slúži na správu skupín používateľov. Skupiny môže poverená osoba v systéme spravovať a nastavovať im rôzne úrovne oprávnenia.

###Zoznam skupín

Ku stránke môže pristupovať Správca.

Nadpis "Zoznam skupín".

Obsahom bude tabuľka (GRID) obsahujúca zoznam skupín v systéme so stĺpcami:

- Skupina, obsahom bude "Názov skupiny"
	- Možnosť zoradenia
	- Textový filter
- Akcie
	- Upraviť práva
		- Po kliknutí bude Správca presmerovaný na Úpravu práv skupiny.
	- Premenovať
		- Po kliknutí sa zobrazí inline editácia názvu skupiny. Názov je možné zmeniť a uložiť.
	- Zmazať
		- Po kliknutí je skupina zo systému odstránená.
		- V systéme bude možné odstrániť iba skupiny do ktorých nepatria žiadny používatelia.
		- Pred odstránením, je nutné potvrdiť kontrolnú otázku "Skutočne chcete odstrániť skupinu {NÁZOV SKUPINY}?"
- Stránkovanie po 25, 50, 100

###Úprava práv skupiny

Ku stránke môže pristupovať Správca.

Nadpis "Úprava práv skupiny {Názov skupiny}".

Obsahom bude Komponenta pre úpravu ACL pravidiel skupiny pre konkrétnu skupinu.

Komponenta pre úpravu ACL pravidiel skupiny/používateľa
Komponenta bude slúžiť na správu oprávnení.

Obsahom bude tabuľka (GRID) obsahujúca zoznam oprávnení pre konkrétnu entitu so stĺpcami:

- Oprávnenie, obsahom bude "Názov oprávnenia"
	- V prípade inline editácie a pridávania pravidla sa zobrazí selectbox so zoznamom možných oprávnení.
- Typ
	- Bude nadobúdať hodnoty "povolené" alebo "zakázané"
	- V prípade inline editácie a pridávania pravidla budú na výber radioboxy s popismi "povolené" a "zakázané". 
- Akcie
	- Upraviť
		- Po kliknutí sa zobrazí inline editácia
	- Zmazať
		- Po kliknutí je oprávnenie zo systému odstránené.
		- Pred odstránením, je nutné potvrdiť kontrolnú otázku "Skutočne chcete odstrániť oprávnenie {NÁZOV OPRÁVNENIA}?"	
- Stránkovanie po 25, 50, 100.
- Inline pridávanie pravidiel.

Vstupy budú ošetrené tak, aby nebolo možné definovať pre jednu entitu viac ako jedno oprávnenie toho istého typu.

###Kategorizácia

Kategorizácia bude nerekurzná - bez zanorenia kategórií.

###Zoznam kategórií

Ku stránke môže pristupovať Správca.

Nadpis "Kategórie aukcií".

Obsahom bude tabuľka (GRID) obsahujúca zoznam kategórií aukcií so stĺpcami:

- Kategória, obsahom bude "Názov kategórie"
	- V prípade inline editácie a pridávania kategórie sa zobrazí text input.
	- s kontrolou na vyplnenie: "Zadajte prosím Názov kategórie."
- Akcie
	- Upraviť
		- Po kliknutí sa zobrazí inline editácia
	- Zmazať
		- Po kliknutí je kategória zo systému odstránená. Spolu s kategóriou budú odstránené aj aukcie patriace do predmetnej kategórie.
		- Pred odstránením, je nutné potvrdiť kontrolnú otázku "Skutočne chcete odstrániť kategóriu {NÁZOV KATEGÓRIE}?"
- Stránkovanie po 25, 50, 100.
- Inline pridávanie kategórií.

##Aukcia

Ihneď po skončení aukcie dostane kontrolór [mail o skončení aukcie](#mail-kontrolórom-o-skončení-aukcie).

###Pridanie aukcie

Ku stránke môže predvolene pristupovať Správca, následne aj používateľ s oprávnením v ACL.

Osoba s poverením bude môcť pri zobrazení ktorejkoľvek aukcie kliknúť na "duplikuj". Následne bude presmerovaná na Pridanie aukcie s predvyplnenými údajmi zduplikovanými z pôvodnej aukcie. Načítajú sa údaje predchádzajúcej aukcie, vrátane predchádzajúcich súťažiacich.

Formulár pre pridanie aukcie:

- Textové pole "Názov aukcie"
	- s kontrolou na vyplnenie: "Zadajte prosím Názov aukcie."
- Textové pole "Súťaží sa o"
- Textové pole "Hlavná požiadavka"
- Textové pole "Ostatné požiadavky"
- Textové pole "Termín vyhodnotenia aukcie"
- Textové pole "Platnosť aukcie od:"
	- s kontrolou na vyplnenie: "Zadajte prosím od kedy je aukcia platná."
	- pole bude obsahovať komponentu na výber dátumu a času
- Textové pole "Platnosť aukcie do:"
	- s kontrolou na vyplnenie: "Zadajte prosím do kedy je aukcia platná."
	- pole bude obsahovať komponentu na výber dátumu a času
- Textové pole "Maximálna cena:"
	- s kontrolou na číselný vstup (na dve desatinné čísla)
- Textové pole "Garancia ceny (v dňoch)"
	- s kontrolou na celočíselný vstup
- Textové pole so zoznamom súťažiacich
- Zaškrtávacie pole "Súkromná aukcia"
- Výberové pole "Kontrolór aukcie"
	- bude obsahovať zoznam používateľov
- Vloženie parametrov ponúk:
	- Textové pole
		- názov
		- kontrola vyplnenosti
		- predvolená hodnota
		- placeholder
	- Textové pole pre čísla
		- názov
		- kontrola vyplnenosti
		- rozmedzie
		- predvolená hodnota
		- placeholder
	- Zaškrtávacie tlačidlo
		- názov
		- predvolená hodnota
		- zoznam možností
	- Prepínacie tlačidlo
		- názov
		- predvolená hodnota
		- zoznam možností
	- Výberový zoznam
		- názov
		- predvolená hodnota
		- zoznam možností

Po uložení sa vygeneruje pozívací list a pošle sa [notifikačný mail](#mail-kontrolórom-o-pridaní-aukcie) kontrolórom.

###Úprava aukcie

V prípade úpravy už schválenej aukcie bude nutné zvoliť medzi možnosťami "Oprava preklepu" a "Zmena podmienok aukcie". Ak bude zvolená voľba "Oprava preklepu", tak sa zmenená aukcia uloží bez dodatočných akcií. 

V prípade potvrdenia voľby "Zmena podmienok aukcie" pri úprave iným používateľom ako kontrolórom je aukcia deaktivovaná. O tomto sú následne informovaný [súťažiaci](#mail-súťažiacim-o-zmene-pravidiel-aukcie) aj [kontrolóri mailom](#mail-kontrolórom-o-zmene-pravidiel-aukcie). Aukcia bude znovu aktivovaná až po odsúhlasení kontrolórom.

Ak voľbu "Zmena podmienok aukcie" potvrdí kontrolór, budú [súťažiaci informovaní iba o zmene podmienok](#mail-súťažiacim-o-zmene-pravidiel-aukcie). Ak túto volbu potvrdí operátor, rozošlú sa [súťažiacim maily o pozastavení](#mail-súťažiacim-o-pozastavení-aukcie) a [kontrolórom so žiadosťou o schválenie](#mail-kontrolórom-o-zmene-pravidiel-aukcie). Túto aukciu bude musieť kontrolór znovu schváliť.

###Schválenie aukcie

K akcií bude mať prístup iba kontrolór.

Kontrolór bude mať možnosť aukciu schváliť. Po schválení sa [pozívací mail](#mail-súťažiacim-o-pridaní-aukcie) rozošle súťažiacim. Ak to je verejná aukcia zverejní sa v RSS a tiež sa pošle [mail](#mail-sledujúcim-o-pridaní-aukcie) všetkým čo si vybrali, že chcú byť informovaní o danom type aukcie.

###Schválenie priebehu aukcie

K akcií bude mať prístup iba kontrolór.

Po schválení je správca kontrolór presmerovaný na stránku "Zoznam ponúk" kde následne vyberie výhercu. Ak bol zvolený víťaz, je mu automaticky zaslaný [gratulačný mail](#mail-súťažiacemu-o-výhre). Ostatným zúčastneným je odoslaný [mail s poďakovaním za účasť](#mail-súťažiacemu-s-poďakovaním-za-účasť).

O schválení priebehu aukcie bude [operátor informovaný mailom](#mail-operátorovi-o-schválení-priebehu-aukcie).

###Zoznam aukcií

- Návštevník vidí:
	- zoznam neukončených (aj nezačatých aukcií)
	- či je aukcia verejná alebo nie
	- odkaz na RSS
	- odkaz na registráciu a prihlásenie
	- ak je aukcia verejná, kliknutím na názov aukcie sa zobrazí detail aukcie
- Operátor vidí: 
	- Zoznam aukcií svojho prideleného typu, tlačidlo pridať aukciu, a pri jednotlivých aukciách upraviť aukciu. Zmazať aukciu ak ešte nebeží.

###Detail aukcií

Návštevník vidí túto stránku iba ak je aukcia verejná.

- názov
- súťaží sa o:
- hlavná požiadavka:
- časové obmedzenie	
- vyvolávacia cena ak je zadané horné ohraničenie ceny
- požiadavky podľa pozývacieho listu

Operátor okrem toho vidí:

- Počet pozvaných súťažiacich, počet zúčastnených, počet ponúk, ktoré zadali. (iba čísla nevidí konkrétne ponuky)
- Po skončení aukcie vidí buď oznam, že sa čaká na schválenie kontrolórom alebo vidí tlačidlo na zobrazenie výsledkov.

Súťažiaci vidí:

- Svoje aktuálne poradie v aukcií
- Ak aukcia prebieha Tlačidlo Pridať/Upraviť ponuku

Detail aukcie je možné exportovať do PDF.

###Zmazanie aukcie

Zmazanie aukcie je možné iba povereným používateľom a iba v prípade ak aukcia aktuálne nebeží.

O zmazaní už schválenej aukcie budú súťažiaci informovaný mailom.

##Ponuky

Po pridaní/úprave/zmazaní ponuky sa v prípade zmeny poradia súťažiacich rozošle [mail informujúci o zmene](#mail-súťažiacim-o-zmene-ich-poradia-v-aukcií) poradia súťažiacim ktorých poradie sa zmenilo.

###Pridanie ponuky

Možné zadať iba ak aukcia práve prebieha.

Formulár pre pridanie ponuky bude obsahovať polia zvolené pri vytváraní aukcie. Parametre musí byť validné podľa nastavenia aukcie.

###Úprava ponuky

Tlačidlo "Upraviť ponuku" pridá novú ponuku zobrazí údaje aktuálnej ponuky. Po stlačení sa údaje uložia vnútorne ako nová ponuka, ktorá v prehľade ponúk nahradí súčasnú ponuku. Pôvodná (stará ponuka) sa uloží do histórie. Históriu ponúk si bude môcť pozerať operátor. Nová ponuka nebude môcť obsahovať vyššiu sumu ako posledná pridaná ponuka od toho istého používateľa.

###Zoznam ponúk

Iba pre účty s oprávnením.

Zoznam všetkých podaných ponúk. 

Export do PDF.

###Zmazanie ponuky

Iba pre účty s oprávnením. Preddefinovane bude mať oprávnenie operátor.

Ponuku je možné zmazať iba počas behu aukcie. Po zmazaní sú súťažiaci informovaný [mailom](#mail-súťažiacim-o-zrušení-aukcie).

###Detail ponuky

V prípade verejnej aukcie prístupné každému. Ak je aukcia neverejná, je jej detail prístupný iba účom s oprávnením, operátorom, kontrolórom a pozvaným súťažiacim.

Export do PDF.

#Prílohy

##Maily

###Mail s aktiváciou účtu

Mail je odoslaný po registrácií. Adresátom je maiová adresa práve registrovaného účtu.

Predmet: **Váš účet bol vytvorený**

>Vážený {MENO},
>
>bol Vám vytvorený účet. K účtu sa môžete prihlásiť použitím prihlasovacích údajov:
>
>Prihlasovacie meno: {MENO}
>Heslo: {HESLO}.
>
>Pre aktiváciu účtu prosím kliknite na odkaz: {ODKAZ}
>
>Prípadne môžete zadať overovací kód {10 MIESTNY KÓD} na stránku {ODKAZ}.
>
>S pozdravom DRU, a. s.

###Mail s novými prihlasovacími údajmi

Predmet: **Nové prihlasovacie údaje**

>Vážený {MENO},
>
>na žiadosť Vám boli zmenené prihlasovacie údaje k Vášmu účtu:
>
>Prihlasovacie meno: {MENO}
>Heslo: {HESLO}.
>
>S pozdravom DRU, a. s.

###Mail s novým heslom

Predmet: **Nové heslo**

>Vážený {MENO},
>
>na žiadosť Vám bolo k Vášmu účtu vygenerované nové heslo: {HESLO}
>
>V prípade, že Ste o nové heslo nežiadali, nič sa nedeje, stále môžete používať Vaše staré heslo.
>
>S pozdravom DRU, a. s.

###Mail sledujúcim o pridaní aukcie

Predmet: **Nová aukcia**

>Dobrý deň,
>
>bola pridaná nová aukcia {NÁZOV AUKCIE S AKTÍVNYM ODKAZOM NA DETAIL AUKCIE}.
>
>S pozdravom DRU, a. s.

###Mail súťažiacim o pridaní aukcie

Predmet: **Nová aukcia**

>Vážený {MENO},
>
>boli Ste pozvaný do novej aukcie {NÁZOV AUKCIE S AKTÍVNYM ODKAZOM NA DETAIL AUKCIE}.
>
>S pozdravom DRU, a. s.

###Mail súťažiacim o zrušení aukcie

Predmet: **Zrušenie aukcie**

>Vážený {MENO},
>
>aukcia {NÁZOV AUKCIE} bola zrušená.
>
>S pozdravom DRU, a. s.

###Mail súťažiacim o zmene ich poradia v aukcií

Predmet: **Zmena Vášho poradia v aukcií**

>Vážený {MENO},
>
>v aukcí {NÁZOV AUKCIE S AKTÍVNYM ODKAZOM NA DETAIL AUKCIE} v ktorej súťažíte sa zmenilo Vaše poradie z **{STARÉ ČÍSLO PORADIA}.** na **{NOVÉ ČÍSLO PORADIA}.**
>
>S pozdravom DRU, a. s.

###Mail súťažiacim o zmene pravidiel aukcie

Predmet: **Zmena pravidiel aukcie**

>Vážený {MENO},
>
>upozorňujeme, že v aukcí {NÁZOV AUKCIE S AKTÍVNYM ODKAZOM NA DETAIL AUKCIE} boli zmenené pravidlá.
>
>S pozdravom DRU, a. s.

###Mail súťažiacim o pozastavení aukcie

Predmet: **Pozastavenie aukcie**

>Vážený {MENO},
>
>aukcia {NÁZOV AUKCIE} bola pozastavená. O jej opätovnom sprístupnení Vás budeme informovať.
>
>S pozdravom DRU, a. s.

###Mail súťažiacemu o výhre

Predmet: **Vyhrali Ste**

>Vážený {MENO},
>
>gratulujeme, vyhrali Ste v aukcií {NÁZOV AUKCIE S AKTÍVNYM ODKAZOM NA DETAIL AUKCIE}.
>
>Čoskoro Vás kontaktujeme.
>
>S pozdravom DRU, a. s.

###Mail súťažiacemu s poďakovaním za účasť

Predmet: **Vaša ponuka nebola vybraná**

>Vážený {MENO},
>
>žiaľ, v aukcií {NÁZOV AUKCIE} sme si nevybrali Vašu ponuku.
>
>Ďakujeme za účasť a dúfame, že sa zúčastníte aj budúcej aukcie.
>
>S pozdravom DRU, a. s.

###Mail operátorovi o schválení priebehu aukcie

Predmet: **Priebeh aukcie bol schválený**

>Vážený {MENO},
>
>aukcia {NÁZOV AUKCIE S AKTÍVNYM ODKAZOM NA DETAIL AUKCIE} bola schválená.
>
>S pozdravom DRU, a. s.

###Mail kontrolórom o pridaní aukcie

Predmet: **Nová aukcia**

>Vážený {MENO},
>
>bola pridaná nová aukcia {NÁZOV AUKCIE S AKTÍVNYM ODKAZOM NA DETAIL AUKCIE}. Aukcia čaká na schválenie.
>
>Do aukcie boli pozvaný nasledovný súťažiaci:
>{ZOZNAM SÚŤAŽIACICH}
>
>S pozdravom DRU, a. s.

###Mail kontrolórom o zmene pravidiel aukcie

Predmet: **Zmena pravidiel**

>Vážený {MENO},
>
>v aukcí {NÁZOV AUKCIE S AKTÍVNYM ODKAZOM NA DETAIL AUKCIE} boli zmenené pravidlá. Aukcia čaká na schválenie.
>
>S pozdravom DRU, a. s.

###Mail kontrolórom o skončení aukcie

Predmet: **Ukončenie aukcie**

>Vážený {MENO},
>
>aukcia {NÁZOV AUKCIE S AKTÍVNYM ODKAZOM NA DETAIL AUKCIE} bola ukončená. Aukcia čaká na schválenie výsledkov a výber víťaza.
>
>S pozdravom DRU, a. s.

##Návod na prevod detailného návrhu do PDF

###Inštalácia prostredia
	
	$ sudo apt-get install dvipdfmx haskell-platform nbibtex texlive-latex-base
	texlive-latex-recommended texlive-latex-extra preview-latex-style dvipng 
	texlive-fonts-recommended

	$ cabal update

	$ cabal install pandoc

###Prevod do PDF

	$ ~/.cabal/bin/pandoc -N --template=template.tex --variable mainfont=Georgia 
	--variable sansfont=Arial --variable monofont="DejaVu Sans Mono" 
	--variable fontsize=12pt --variable lang=slovak DN-Dru-Aukcie.md
	--latex-engine=xelatex --toc -o DN-Dru-Aukcie.pdf

##Pozývací list

Šablóna pozývacieho listu sa nachádza v súbore pozyvaci-list.pdt.