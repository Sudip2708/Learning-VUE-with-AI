## Dotaz:

Ahojky :-)
Učíme se VUE tím, že do něj předělávám frontend pro svůj projekt psaný čistě v Django.
Už máme za sebou stránku s výpisem všech článků a dneska bych chtěl začít se stránkou pro výpis jednoho článku.

To první, s čím bych chtěl začít je definovat k kartičce pro náhled jednoho článku pro stránku s  výpisem článků, aby část ArticleCardBody celá byla odkazem na:

Zde je kod pro tuto komponentu:

S tím že hodnota pro slug je uložena pod klíčem 'slug'

## Dotaz:

Super a díky :-)
Teď bych po tobě potřeboval dopsat kod pro pohled ve VUE pro stránku s jedním článkem, který by mi zatím jen zobrazil hlavní obrázek: frontend\src\views\ArticleDetailView.vue

Tady je její momentální podoba:


## Dotaz:

Nepodařilo se mi zobrazit stránku pro jeden článek a tak jsem skusil minimalizovat možné příčiny a pro vykreslení stránky použít jen tento kod:

Ale i tak je stránka naprosto prázdná a nikde v terminálu nevidím žádnou chybu. Takže počítám, že špatně bude něco s mým nastavením.

Zde je pro jistotu kod pro soubor frontend\src\router\index.js: 

Ten je ale podle mě v pořádku, protože nikde v terminálu a  ani konzoli nevidím žádný chybový oznam.

Na co jiného bych se měl zaměřit aby se mi povedlo zobrazit na stránce pro detail článku ty velká AAA?

## Dotaz:

Tak AAA se mi už zobrazuje, takže na stránku se dostanu - za to dík. Ale stále se mi nezobrazují data pro článek. 
Skusil jsem si do pohledu v Django dát skušební tisk:

Ale v terminálu se nic neobjevilo a tak počítám, že chyba bude někde ve volání z VUE na Django

Potřeboval bych tedy postupně projít celou cestu a chybu dohledat.

Začneme rozborem kodu, kterým je volaná stránka, a to odkazem uvnitř kartičky náhledu článku:
frontend\src\components\ArticlesListView\ArticleCard\ArticleCardBody.vue

Kde je tento kod:

A podle mě by mohla být chyba v samotném odkazu, protože mi příjde, že se chce přihlásit na stránku /article/${this.slug}/ což tedy vypadá v pořádku, akorát si řikám, že když je v frontend\src\router\index.js nastaveno:

Nemělo by se toto volání stránky provádět jinak?

## Dotaz:

Tak zatím bez úspěchu. Zkusíme jít naprosto pomalu a krok po kroku.
První - kontrola volání.

Na stránce se všemi články je v kartě pro článek část body (frontend\src\components\ArticlesListView\ArticleCard\ArticleCardBody.vue)

Která má v sobě zahrnut router-link pro volání stránky:

po kliknutí na tento odkaz, se podle mě zavolá frontend\src\router\index.js:

A dohledá se cesta s názvem ArticleDetailView:

Takže až sem to funguje, protože se mi skutečně objeví prázdná stránka s AAA.

Nyní bych od tebe chtěl zapsat sem kod, který udělá pouze to, že zavolá Django přes localhost8000/article/(+slug)/
A po obdržení dat je vytiskne do konzole.

## Dotaz:

Tak už asi vím kde je chyba. Ty počítáš s tím, že pro VUE používám předponu api, což momentálně v django nastavené nemám.
Ale uznávám, že bych to měl asi napravit, protože rozumím tomu, že se mi tím v django oddělí kod pro vue.

Ve svém projektu mám url ve více modulech.

Základní url je zde: main/urls.py

A má tento obsah:

A mě by zajímalo jak se zde správně definuje předpona api, a to tak aby mi ideálně zůstali i adresy, které používám pro django, jestli to tedy ničemu nevadí.

## Dotaz:

Tak api opraveno ale chybu to nevyřešilo.

Koukal jsem se na kod pro stránku se všemi články a a příjde mi, že tam je navíc tato část:

Která v nově vytvářeném kodu chybí.
Stránka provšechny články mi funguje dobře a dobře načítá a zobrazuje data z django, takže si říkám, jestli to, že se mi na stránce pro detail článku nic neobjeví, nemůže být kuli tomu, že zde chybí tato část?

## Dotaz:

AHojky :-)
Dělám projekt ve VUE a nyní jsem přejmenovával nějaké komponenty a cesty a dostal jsem tuto chybu:

Nedokážeš z ní vyčíst, co mi ji působí?

## Dotaz:

OK, tady je kod pro frontend\src\router\index.js:

Tady je kod pro frontend\src\views\ArticlesListView.vue:

Tady je kod pro frontend\src\components\ArticlesListView\Pagination.vue:

Vydíš v nich něco, co by to mohlo působit?

## Dotaz:

Když mám ve VUE v souboru frontend\src\router\index.js takto definovanou cestu:

Jak nyní správně vytvořit kod pro router-link, který by ji volal?

## Dotaz:

Toto je kod pro vue, který mi zobrazuje všechny články:

A já nyní vytvářím kod pro zobrazení aktuálního článku a zatím mám jen vytvořenou stránku bez vnitřní logiky:

mohl by jsi mi na základě předchozího kodu, napsat kod pro tuto stránku, který by získal data z api/article/slug/ (slug je možné získat z adresy vue stránky)

A po té mi jen na stránce zobrazil název článku {{ article.title }}

## Dotaz:

Tak namísto názvu mám text: Načítání článku...

A v konzoli tento oznam:

Co se dá z toho oznamu vyčíst?

## Dotaz:

Takže to podle výpisů v konzoli a v terminálu vypadá, že už jsem se provolal až do souboru na serveru django, který maá na starosti vypracování kodu pohledu articles/views/article_detail_api.py:

Ale zde se něco stane a v konzoli mám toto:

Dokážeš z toho něco vyčíst?

## Dotaz:

Proč na tento kod:

Dostávám tuto chybu:
