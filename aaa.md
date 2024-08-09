## Dotaz:

Ahojky :-)
Učím se VUE tím, že do někj postupně přepracovávám mojí aplikaci psanou čistě v Django. Aplikace je určená na vytváření a prohlížení multimediálních článků. Za sebou už mám funkční propojení mezi Django a Vue. Včera jsem  pracovali na základním rozložení vzhledu pro stránku se všemi články. Dneska budeme na této stránce pokračovat, ale stím, že na začátku se nejprve zaměříme na Django a na správnou přípravu dat pro VUE.

Tady je základní soubor pro původní verzi Django pohledu pro tuto stránku:

A tady je kod který momentálně používám pro VUE:

V konečném důsledku bych rád tento druhý pohled přepsal tak, aby odpovídal potřebám ve VUE, a zároveň mu nechyběla žádná funkčnost předchozího kodu.

První dotaz je, zda jdu správnou cestou, když používám třídu APIView a nebo zda bych měl použít pro to nějakou jinou třídu.
A pokud je APIView v pořádku, můžeš mi ji nějak přiblížit, co vše nabízí, v čem je jiná oproti ListView a v čem jsou si naopak podobné.

## Dotaz:

Paráda :-)

Rád bych šel krok po kroku. To první, čeho bych chtěl dosáhnout je, abych měl všechna potřebná data pro vykreslení samotných článků.

Pro to potřebuji následující:
1) Náhledový hlavní obrázek článku
- v moedlu Articles se pole pro tento obrázek jmenuje main_picture_preview, a to co bych zde potřeboval vyřešit, je abych pro něj získal absolutní cestu, tak aby šel ve vue zobrazit.
2) Datum publikace článku - Mám
3) Kategorie
- Zde se mi vrací číslo ID kategorie a já bych potřeboval její název. V jinja2 jsem to řešil přes model Article, kde jsem se přes navázané ID kategorie dostal až k jejímu názvu. To ale asi ve VUE nejde - takto pracovat s instancemi a jejich klíči, takže počítám, že název kategorie budu muset asi v procesu dohledat a číslo nahradit názvem.
4) Název článku - Mám
5) Overview článku - Mám
- Zde jen jeden malí pod dotaz, vzhledem k tomu, že se na této stránce nevyskytuje obsah článku a vzhledem k tomu, že obsah článku je hodně dat, asi by bylo správné tento obsah vůbec nevytahovat z databáze a zaměřit se jen na ty pole, o které mám zájem? Je to tak?
6) Miniatura profilového obrázku autora článku
- Zde mám zase jako u kategorie k dispozici pouze id autora a tak počítám, že profilový obrázek asi budu muset dle tohoto id dohledat a přidat do slovníku
7) Jméno autora
- to samé jako u předešlého bodu
8) Stáří článku 
- to asi budu vypočítávat až ve VUE dle data publikace (A nebo je lepší to vypočítat v Django a jen předat údaj?)
9) Počet komentářů
- zde je potřeba získat tento údaj dle id článku v modelu pro komentáře

Takže to je vše co potřebuji pro kartičku pro jeden náhled jednoho článku a od tebe bych chtěl návrh kodu, který by toto navrhl základní kostru.

A jako další pod dotaz bych se chtěl zeptat, jestli je lepší předávat údaje jako je název stránky v kontextu z django a nebo je lepší ho natvrdo vložit přímo do Vue komponenty?

## Dotaz:

OK, vezmeme to po kouskách.
Je lepší zadat dotaz na databázi, který načte všechny data o článcích a pak následně z těchto dat filtrovat ty která potřebuji pro VUE, a nebo je lepší poslat dotaz do databáze, který mi pro každý článek vybere jen obsah polí o která mám zájem?

## ChatGPT:

Je obecně lepší poslat dotaz do databáze, který vybere pouze pole, která skutečně potřebuješ. Tímto způsobem minimalizuješ přenos dat a zlepšíš výkon aplikace.

V Django ORM lze použít metody `only()` nebo `values()` k omezení načítaných polí. Následně můžeš tato data serializovat a poslat do Vue. Pojďme si to ukázat na příkladu.

## Dotaz:

Vysvětli mi prosím rozdíl mezi těmito dvěma zápisi:

Který z nic má potencionál být větší zátěží a nebo co se týká zátěže, jsou si rovnocenné?

## Dotaz:

OK, napiš mi tedy kod který by byl ve stylu druhého zápisu s values, ale který by načítal pouze tato data:

'id', 'main_picture_preview', 'publication_date', 'title', 'overview',

Rád bych to viděl jen pro to, abych dokázal oddělit, co je potřeba pro pole s cizími klíči a co je potřeba pro obyčejné pole s hodnotami.

## Dotaz:

Potřeboval bych to vzít ještě trošičku pomaleji, abych to správně pochopil. Mohl bys mi tedy tyto příklady rozdělit, tak abych vždy viděl nastavení pro jeden daný příklad.

Vycházejme tedy z tohoto kodu:

articles2 = Article.objects.values( 'id', 'main_picture_preview', 'published', 'title', 'overview', 'category', 'author', )

Který prochází všechny články a vytahuje z nich hodnoty pro dané klíče.

A nyní bych tě tedy poprosil o následující:

Přepiš mi tento základní kod pouze s jednou funkční změnou - a to:

1) Aby main_picture_preview, které momentálně obsahuje tento řetězec:

'images/articles/main_picture/article-00000001-0440.jpg'

Obsahovalo absolutní cestu.

To mi vytvoř jako jeden příklad.

2) Aby mi pole pro categoryi obsahovalo namísto její ID její název (category.name)

To mi vytvoř taky jako jeden oddělený příklad.

## Dotaz:
Z toho co píšeš jsem si udělal představu, že  select_related je způsob jak se vyhnout novému dotazu (například do modelu kategorie na základě jejího ID) a namísto toho používáme dotazu přez cizí klíč. Zajímalo by mě nějaké porovnání těchto metod. Proč je dotaz přes cizí klíč rychlejší než nový dotaz na databázi kategoriií?

A co se týká profilového obrázku, přemýšlím nad tím, zda je efektnější tuto absolutní cestu řešit v rámci dotazu, tak jak je to teď a nebo zda by bylo výhodnější ji v rámci dotazu neřešit a řešit ji až po načtení dat, například nějakým cyklem. Co je efektivnější přístup a proč?

## Dotaz:

Když zadám dotaz na databázy pro vyhledání všech článků:
articles = Article.objects.all()

Co se mi vrací s databáze? Vrací se mi všechna data (mám na mysli všechen obsah v podobě řetězců) a nebo se mi vrací pouze adresa na instance k článkům a k ostatním datům pak přistupuji přes ní bez nutnosti dalších databázových dotazů?

Jde mi o to, zda po zadání tohoto dotazu se předávají veškerá data instance a nebo jen nějaký malí přístupový řetězec (adresu) k datům.

Součástí tohoto dotazu, je tak i to, zda když mám v nějaké proměné načtenou určitou instanci kterou získán například dotazem:
article = Article.objects.first()

A pak budu přes tuto instanci přistupovat k jednotlivým polím:
title = article.title

Jestli tímto způsobem dochází k novému databázovému dotazu, anebo dojde jen k volání instanční adresy, který odkazuje na určitý řádek databáze + sloupec (title), takže se nespouští klasický databázový dotaz. A nebo se děje to, že při načtení instance příkazem:
article = Article.objects.first()

Se automaticky někde v paměti nějak vytvoří prostor, který si pamatuje všechny její data a tyto data jsou do něj přesunuty z databáze při tomto dotazu.

Jde mi o pochopení tohoto procesu.

## Dotaz:

A můžu se teda na instanci vytvořenou dotazem:

article = Article.objects.first()

Dívat jako na json slovník se všemi daty modelu? A nebo instance obsahuje ještě něco navíc?


## Dotaz:

Jasně, tak už tomu asi rozumím - když se vytváří instance dojde k volání daného modelu a vytváří se v paměti objekt, který neobsahuje pouze data ale všechny ostatní náležitosti definované v modelu - takže samotná instance je vlastně jen odkaz na umístnění těchto dat v rámci paměti. A zároveň v tomto místě paměti jsou přeneseny všechna data a to včetně i metod, takže instance zabírá více místa v paměti.

Kdežto, když použiji tento příkaz:

first_article = Article.objects.values('id', 'title', 'overview', 'main_picture_preview').frist()

pak first_article není instance modelu Article obsahující i jeho metody, ale jen json slovník, s vybranými daty.

Takže vytvoření instance je vhodné jen v těch případech, kdy víme, že bude skrze ni používat i další možnosti a metody modelu, a vytvoření slovníku je pak vhodné tam, kde chceme vytáhnout jen data pro zobrazení (například ve VUE). 

Je to tak?


## Dotaz:

Paráda a díky moc. Teď už mám pocit, že tomuto rozumím. A závěr který jsem si z toho odnesl, je že při vytváření dotazů v pohledu v Django, pro frontend je na zdroje méně náročnější vytváření slovníků namísto instancí.

Teď by mě ale zajímalo toto:

Karta pro zobrazení jednoho článku na stránce s články obsahuje pole pro název kategorie, pod který daný článek spadá. A v mém původním kodu v Django, toto pole bylo kotvou, která po kliknutí na kategorii volala adresu pro zobrazení článků pro danou kategorii a předávala této adrese hodnotu slug pro danou kategorii.

Počítám tedy, že pokud budu chtít podobnou funkčnost i ve VUE, pak asi i kromě jména kategorie, bych měl přenášet i její slug, podle kterého pak budu vytvářet dotaz pro stránku s články dané kategorie. To by tedy znamenalo, že pro všechny tyto případy, kdy nějaká hodnota je zároveň i router-link, pak si musím hodnoty podle kterých se vytvářejí dané URL přidat do slovníku json s daty pro daný článek taky, a být tak na to připraven. 

Je to tak a nebo se to dělá ve VUE nějakým jiným způsobem?

## Dotaz:

Takže stále pracuji na optimalizaci dotazu do databáze.

Toto je to co zatím mám:

A nyní bych chtěl aby se filtorvali články pouze se statusem publish (article.status == publish), kde je dobré místo v tomto dotazu přidat toto omezení a proč?

## Dotaz:

Jak metoda values() pozná, zda určitá data pro daný klíč má hledat v modelu a nebo v metodě annotate()? Prochází nejprve annotate() a pak pole modelu? A co by se stalo, kdyby nějaký klíč nebyl ani tam a ani tam? 

## Dotaz:

Super a díky :-)

takže momentálně můj kod pro dotaz na data pro stránku se všemi publikovanými články vypadá takto:

A nyní bych potřeboval zařídit, aby hodnoty pro main_picture_preview a author_profile_picture byli absolutní cesty.

Jaké jsou nejčastěji používané způsoby jak tohoto dosáhnout?

## Dotaz:

Když mám v modelu část pro profilové obrázky zpracovanou takto:

Takže mám zde 4 varianty obrázků, které se používají, pak mám asi pro každou variantu vytvořit metodu, která by vracela absolutní url obrázku, že? 

Dopiš mi prosím do toho kodu metody, které by mě vraceli absolutní url, tak abych ji pak mohl použít v rámci dotazu.

## Dotaz:

A nemohl bych jednoduše použít v annotate něco jako je toto:

    absolute_main_picture=Concat(
        Value(settings.BASE_URL),
        'main_picture_preview',
        output_field=models.CharField()
    )

A pokud ano, mohl bys mi vysvětlit tento příkaz? 

## Dotaz:

Nerozumím tomu procesu, když VUE dostane řetězec:
"/media/images/articles/main_picture/article-00000045-0440.jpg"

Jak s ním dál nakládá aby se nakonec dostal až k umístění obrázku.

## Dotaz:

Takže když včera jsem používal pro vytažení a přenosu dat do vue tento kod pro pohled v Django:

A dneska bych ho rád nahradil tak, abych mohl využít tento upravený a specifikovaný dotaz:

Jak bych toho měl ideálně dosáhnout? A pokud je více než jede dobrý způsob, zmiň i je.

## Dotaz:

Pořád nějak nerozumím tomu, proč bych měl chtít provádět serializaci dat? Proč je to dobré, co mi to přináší a co tím získávám? Jaká je za tím myšlenka a nebo potřeba?

Můžeš mi uvést i nějaké příklady? 

## Dotaz:

Je serilizace proces, kdy data se převádějí na řetězec a přidává se k němu datový format, anebo to dělá ještě i něco víc.

## Dotaz:

A dá se říct, že když přináším z Django do Vue JSON slovník, který obsahuje řetězce, a u kterých nepotřebuju deklarovat typ, stačí mi, že jsou to řetězce, že pak serializaci nepotřebuju, které nepotřebuju.

## Dotaz:

A dá se, že by třída pro pohled dětěla i z třídy pro serializer. A sterilizovaní dat bych proved uvnitř pohledu?

## Dotaz:

Ve svém projektům mám něco mezi deseti a dvaceti pohledy, co znamená to, že pro každý pohled budu muset takhle vytvářet celý arizér. A pokud ano, teda, kde by měly být úložní kysly ve složce pro pohledy jako podsložka anebo v kořenovém adresáři.

## Dotaz:

A dá se říct, že když z modelu nepotřebuji všechny polé a potřebuji jen jich pár, tak je lepší provízt Select na tyto polé a serializovat až tento výsledek, než vytáhnout celou instanci a serializovat ji.


## Dotaz:

Napiš mi něco více o tom, jak se může serializace použít pro validaci dát. Zkus to rozebrat z obou strán, jak z Django, tak z Vue.




