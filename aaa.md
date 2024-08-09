## Dotaz:

Ahojky :-)
Učím se VUE a nyní si do něj převádím projekt psaný v Django. Projekt je webová sránka pro prohlížení a správu multimediálních článků a do VUE už mám přenesenou stránku s výpisem všech článků a nyní pracuji na stránce pro jeden článek.
Včera už jsme úspěšně rozchodili tuto stránku, takže se mi přenáší a vykreslují data z databáze v Django.

Dnes začneme tím, co jsme včera už nestihli, a to vykreslení hlavního obsahu článku. Ten je vytvářen modulem TinyMCE a je teda v html souboru. V modelu je definovaný takto:

To první co by mě asi zajímalo je, jaký serializer mám pro totopole použít? Zda CharField a nebo nějaké jiné.

K tomuto dotazu bude ještě pod dotaz, a to, zda záleží, a jak moc, na to jaké použiji seliarizéry pro jednotlivá pole. Chápu že z databáze si vytahuji pouze řetězce, takže čistě teoreticky by na vše měl stačit serializers.CharField(), tak jak je to teď, ale zajímalo by mě zda je dobré více specifikovat jednotlivá pole a případně i návrhy jak.

Zde je můj aktuální kod pro pohled v Django pro stránku s detailem článku:

A toto je kod pro jeho serializer:

## Dotaz:

Zajímalo by mě ještě kde je hranice kdy se rozhodnout mezi CharField() a TextField() v kontextu například s polem content. Díval jsem se na jeden článek v databázi a ten obsahuje v tomto poli 208490 znaků.

## Dotaz:

A měl bych tedy použít TextField pro pole overview, které má u kontrolovaného článku 648 znaků?

## Dotaz:

Proč když zapíšu tento kod:

from rest_framework import serializers class ArticleDetailSerializer(serializers.Serializer): id = serializers.IntegerField() slug = serializers.SlugField() main_picture_url = serializers.URLField() published = serializers.DateTimeField() title = serializers.CharField() overview = serializers.CharField() content = serializers.TextField() category_name = serializers.CharField() category_slug = serializers.SlugField() author_name = serializers.CharField() author_slug = serializers.SlugField() author_profile_picture_url = serializers.URLField()

Tak mi PyCharm zažlutí tento text: TextField

?

## Dotaz:

Takže to chápu tak, že pokud nemám na pole v serializéru žádné validační požadavky - seliarizér slouží pouze pro předání dat z Django databáze do VUE - tak v tu chvíli by teoreticky mohli být všechny pole CharField, a když bych chtěl definovat serializér, který by se používal pro převod dat od uživatele do databáze, pak přesná specifika polí mi pomůže s validací. Je to tak?
Pokud ano, napiš mi všechny druhy serializéru z rest_framework, které mohu použít a co se dá vše validovat v serializeru. A jestli je to tak, že když příjdou špatná data jsou vrácena, podobně jako z formuláře Django a jeho metody clean()?

## Dotaz:

A jen abych měl klid.
Plyne nějaká výhoda z toho, kdyby serializer pro stránku která pouze vykresluje data z databáze měl blíže specifikované pole, tak jak je to zde:

A nebo v případě, kdy se jedná pouze o přenos dat z databáze do vue, tak je lepší použít pro všechna pole (kromě id) čistě jen CharField() ?

Jaké jsou zaběhlé postupy a výhody a nevýhody obou přístupů?

## Dotaz:

Super toto pomohlo :-)

teď bych po tobě chtěl vytvořit základní kod pro VUE komponentu, která by vykreslovala pole content.

## Dotaz:

Super a díky.
Teď bych potřeboval přidat obsah z dalších dvou polí modelu Article:

Jedná se o pole pro případný předchozí a následující článek.
Počítám, že pro jeho vykreslení budu potřebovat pouze pole article.title a pro jeho volání budu potřebovat pole article.slug.

Můžeš mi tato pole připsat do tohoto pohledu:

## Dotaz:

Super a díky.

Napiš mi prosím základní VUE kod pro komponentu ConnectedArticles, která bude vypadat následovně:
Na jedné lince budou dvě obdelníková pole jedno přichycené nalevo a jedno na pravo.

Tyto dvě pole by také klidně mohli být vnitřní komponenty komponentu ConnectedArticles, ale to jen pokud je dobré to takto ještě rozdělit.

Levá komponenta se bude skládat:
Na levé straně šipka do leva, která je zároveň odkazem na předchozí článek.
Za šipkou pak zbytek pole rozdělit na dva řádky, kde:
Nahoře bude text: Previouse Article 
A pod ním bude název článku

Pravá komponenta bude zrcadlově přetočaná napravo as bude sloužit pro následující článek, takže text komponenty bude znít: Next Article

Komponenty by měli mít kontrolu, zda je předchozí a následující článek obsažen, a pokud ne, tak tlačítko, nebo tlačítka nezobrazovat.

A následně mi prosím komponent ConnectedArticles vlož do VUE pohledu pro detail článku:

## Dotaz:

Super a díky.

Nyní bych potřeboval přidat komponentu s tagama.
Každý článek má tak kolem 5-ti tagů a představoval bych si to tedy tak, že tagy budou v jednom řádku. Tagy budou do budoucna i klikací, že se objeví stránka se všemi články pro daný tag, ale tu zatím nemá, takže si je jen takto předpřipravím.

V modelu Article je pole pro tagy definované takto:

A od tedy bych zase potřeboval nejprve upravit strukturu dotazu a serializer.

Zde je aktuální podoba dotazu:

A zde aktuální podoba serializeru:

## Dotaz:

Na tento kod:

Jsem dostal tuto chybu:

která je podle mě způsobena tím, že pole pro tag jsou cizí klíč pro taggit:
from taggit.managers import TaggableManager

A tak je asi potřeba je získat jinak, než je uvedeno nyní v kodu.
Nebo je ta chyba o něčem jiným?

## Dotaz:

Rozumím tedy tomu tak, že seznam tagů si nemůžu vytáhnou najednou se všemi daty a rozumím i tvému návrhu, nejprve si vytáhnout instance článků a pak pro ně hledat tagy.

S čím si ale nejsem jistý, jestli toto rozdělení dotazu nejde proti tomu, co bych primárně chtěl, a to maximálně optimalizovat dotaz, tak aby působil co nejmenší zátěž. A tady podle tohoto navrhnutého postupu, bych si udělal představu, že nejprve dojde k vytvoření instance - což je jeden dotaz, a následně bude tato instance použita k vytažení tagů, což je druhé volání, a následně dojde z této instance k vyselektováni polí, což je další dotaz. Tedy jestli si ten proces představuji správně.

Kdyby to bylo možné a i výhodné, tak by se mi více líbil první přístup, který vytáhne rovnou všechny potřebná data do slovníku. A následně by dle id došlo k dohledání seznamu jmen tagů, které by se přidali do již vytvořeného slovníku.
Takto by to byli dotazy jenom dva.

Dává to smysl?

## Dotaz:

Co znamenají tyto dvě chyby v konzoli:

## Dotaz:

Takže Violation oznam zatím řešit nebudu, ale díval jsem se více na tu chybu s bullet-icon.svg.
Zjistil jsem že chybu způsobuje html obsah pro pole content, kde se vyskytují podobné řádky jako je i tento:

Z toho, co bych usoudil se jedná o nějaký znak, či něco podobného.

Články jsou dumpt data a přebrané z wikipedie, takže je možné že tento odkaz už nefunguje.

Nedokážeš z tohoto vyčíst k čemu tato bullet-icon.svg měla být a případně co bych s tím měl dělat? Jestli třeba by stačilo jen tyto řádky smazat?

Zde je pro ilustraci pár výskytů:

## Dotaz:

Super. Vybral jsem si variantu nahrazení odkazů změnou stylu na 'disc' a je po problému. Stejně podle mě šlo jen o odrážku. I když si jistý nejsem. 

Každopádně stránku pro jeden článek máme hotovou na mě je ji už jen destilovat dle předlohy.

Takže pro toto vlákno moc díky a ozvu se zase v novém :-)
