## Dotaz:

Ahojky :-)
Předělávám projekt v Django na VUE frontend a nyní pracuji na stránce se všemi články.
Zajímalo by mě, jaké jsou možnosti k řešení stránkování?
V Django jsem používal pro pohled třídu ListView a metodu get_paginate_by, ale nyní pro pohled pro vue používám třídu APIView a navíc si nejsem ani jistý jestli se to řeší s vue podobně jako jsem to měl teď, a nebo se ke stránkování přistupuje úplně jinak?

## ChatGPT:

Ahoj! Pokud přecházíš z Django na Vue.js pro frontend, budeš muset stránkování vyřešit jinak než s Django `ListView` a `get_paginate_by`. Vue.js je čistě frontendový framework, takže stránkování se obvykle řeší na úrovni API a klientského kódu.

### Stránkování v API

Pokud používáš Django REST framework (DRF) pro své API, můžeš využít vestavěné možnosti stránkování. Zde je, jak na to:

1. **Nastavení stránkování v Django REST framework**

   Ve svém souboru `settings.py` nastav `REST_FRAMEWORK` s konfigurací stránkování. Například:

   ```python
   REST_FRAMEWORK = {
       'PAGE_SIZE': 10,
       'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
   }
   ```

   Tímto způsobem se stránkování aplikuje na všechna API endpointy, které používají výchozí stránkovací třídu `PageNumberPagination`. Můžeš také použít jiné třídy stránkování, jako `LimitOffsetPagination` nebo `CursorPagination`, podle potřeby.

2. **Vytvoření API endpointu**

   Ve své API třídě (např. v `views.py`), použij `APIView` nebo `GenericViewSet` pro poskytování článků s podporou stránkování. Například:

   ```python
   from rest_framework import generics
   from .models import Article
   from .serializers import ArticleSerializer

   class ArticleListView(generics.ListAPIView):
       queryset = Article.objects.all()
       serializer_class = ArticleSerializer
   ```

   Tato třída automaticky podporuje stránkování podle tvého nastavení.

### Stránkování na frontendové straně ve Vue.js

Na frontendové straně, tj. ve Vue.js, budeš muset:

1. **Získat stránkovaná data z API**

   Použij například Axios nebo Fetch API k načtení stránkovaných dat. API ti vrátí stránkované výsledky, které můžeš následně zobrazit.

   ```javascript
   import axios from 'axios';

   export default {
       data() {
           return {
               articles: [],
               currentPage: 1,
               totalPages: 1,
           };
       },
       methods: {
           fetchArticles(page = 1) {
               axios.get(`/api/articles/?page=${page}`)
                   .then(response => {
                       this.articles = response.data.results;
                       this.totalPages = response.data.total_pages;
                   })
                   .catch(error => {
                       console.error('Error fetching articles:', error);
                   });
           },
           goToPage(page) {
               this.currentPage = page;
               this.fetchArticles(page);
           }
       },
       created() {
           this.fetchArticles();
       }
   };
   ```

2. **Vytvořit komponentu pro stránkování**

   Vytvoř komponentu pro stránkování, která umožňuje uživatelům přecházet mezi stránkami. Může to být jednoduchý seznam čísel stránek nebo pokročilejší navigační prvek.

   ```vue
   <template>
       <div>
           <ul>
               <li v-for="page in totalPages" :key="page">
                   <button @click="goToPage(page)">{{ page }}</button>
               </li>
           </ul>
       </div>
   </template>

   <script>
   export default {
       props: {
           totalPages: {
               type: Number,
               required: true
           },
           currentPage: {
               type: Number,
               required: true
           }
       },
       methods: {
           goToPage(page) {
               this.$emit('change-page', page);
           }
       }
   };
   </script>
   ```

   V tomto komponentu `totalPages` a `currentPage` jsou předávány z rodičovské komponenty a `change-page` událost je emitována zpět, když uživatel klikne na tlačítko.

### Shrnutí

- **Backend (Django REST framework)**: Nastav stránkování ve `settings.py` a použij `ListAPIView` nebo podobné třídy pro poskytování stránkovaných dat.
- **Frontend (Vue.js)**: Načti stránkovaná data z API a vytvoř komponentu pro stránkování, která umožňuje uživatelům navigovat mezi stránkami.

Doufám, že ti to pomůže! Pokud máš další dotazy, dej vědět.

## Dotaz:

A můžeš mi popsat jak funguje celý proces? Jde mi hlavně o to, zda se stránkování promítá i do dotazu na databázi a nebo zda se z databáze načtou všechny články na jeden dotaz a pak se v těchto datech stránkuje. Ptám se proto, že by mi tento druhý přístup přišel značně neekonomický, kdyby načítal z databáze všechny články například pro 10 stránek a uživatel by zhlídnul články pouze z první stránky. Takže bych si spíš představoval systém, kdy po kliknutí na další stránku by se vyslal dotaz na do načtení dat z databáze a tak by se načítali jen ty data, která jsou potřeba.

## ChatGPT:

Máte naprostou pravdu, že druhý přístup, kdy by se načítaly všechny články najednou, by byl neefektivní a neekonomický, zejména pokud máte velké množství dat. Správně implementované stránkování by mělo zahrnovat načítání pouze potřebných dat pro aktuální stránku, což snižuje zatížení serveru a zlepšuje výkon aplikace.

### Jak funguje stránkování na úrovni API a databáze

#### 1. Dotaz na databázi

Při stránkování by měl být dotaz na databázi upraven tak, aby načítal pouze data, která jsou potřebná pro aktuální stránku. Tento přístup zajistí, že se načítá jen omezený počet záznamů najednou. Tento způsob je efektivní a škálovatelný.

**Příklad v Django REST Framework (DRF):**

Představme si, že máš model `Article` a chceš stránkovat články.

1. **Nastavení stránkování v Django REST framework:**

   V `settings.py` nastavíš stránkování, například:

   ```python
   REST_FRAMEWORK = {
       'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
       'PAGE_SIZE': 10,
   }
   ```

2. **Vytvoření API endpointu s podporou stránkování:**

   Vytvoříš pohled, který bude používat stránkování:

   ```python
   from rest_framework import generics
   from .models import Article
   from .serializers import ArticleSerializer

   class ArticleListView(generics.ListAPIView):
       queryset = Article.objects.all()
       serializer_class = ArticleSerializer
   ```

   Třída `ListAPIView` automaticky použije stránkování definované v `settings.py`.

3. **Dotaz na databázi s stránkováním:**

   Django REST framework použije stránkovací třídu k omezení počtu vrácených záznamů podle aktuální stránky a velikosti stránky. Například, pokud je aktuální stránka 2 a velikost stránky je 10, dotaz na databázi bude omezen na záznamy 11–20.

#### 2. Dotaz na API z frontendu

Na frontendové straně, tj. ve Vue.js, provedeš dotaz na API a zpracováváš stránkovaná data.

**Příklad s Axios:**

1. **Načítání dat:**

   Při načítání dat z API po kliknutí na stránkovací tlačítko odešleš dotaz na server s parametrem pro stránku.

   ```javascript
   import axios from 'axios';

   export default {
       data() {
           return {
               articles: [],
               currentPage: 1,
               totalPages: 1,
           };
       },
       methods: {
           fetchArticles(page = 1) {
               axios.get(`/api/articles/?page=${page}`)
                   .then(response => {
                       this.articles = response.data.results;
                       this.totalPages = response.data.total_pages;
                   })
                   .catch(error => {
                       console.error('Error fetching articles:', error);
                   });
           },
           goToPage(page) {
               this.currentPage = page;
               this.fetchArticles(page);
           }
       },
       created() {
           this.fetchArticles();
       }
   };
   ```

   V tomto příkladu `fetchArticles` odešle dotaz na API a zpracovává odpověď, která obsahuje články pro aktuální stránku.

2. **Zpracování odpovědi:**

   API vrátí data pouze pro aktuální stránku. Například, pokud požaduješ stránku 2, API vrátí články 11–20 a informace o celkovém počtu stránek a článků.

### Shrnutí

1. **Dotaz na databázi:** Dotaz na databázi by měl být upraven tak, aby načítal pouze záznamy pro aktuální stránku. To se obvykle provádí pomocí stránkovacích tříd v rámci Django REST frameworku.
2. **Dotaz na API:** Frontendový kód odešle dotaz na API s parametrem pro stránku a zpracovává pouze data pro tuto stránku.

Tento přístup zajišťuje efektivní využití zdrojů a rychlou odezvu aplikace, protože se načítá pouze malá část dat na každém dotazu.

## Dotaz:

A je dobré toto stránkování (číslo stránky na které jsme) promítnout i do URL?

## ChatGPT:

Ano, promítnutí čísla stránky do URL je velmi dobrá praxe a má několik výhod:

### Výhody promítnutí čísla stránky do URL

1. **Jednoduché sdílení:** Pokud uživatelé chtějí sdílet konkrétní stránku s ostatními, mohou jednoduše sdílet URL, která obsahuje číslo stránky. Například: `https://example.com/articles?page=3`.

2. **Zpětné navigace:** Uživatelé mohou snadno použít funkce pro zpětnou navigaci v prohlížeči (např. tlačítko „zpět“ nebo „vpřed“), aby se vrátili na předchozí nebo další stránku.

3. **Uložení stavu:** Pokud uživatel obnoví stránku, URL s číslem stránky umožní aplikaci automaticky načíst správné stránky. 

4. **SEO výhody:** URL s číslem stránky může být přívětivější pro vyhledávače, i když to může mít omezený dopad v závislosti na konkrétním případu použití.

5. **Kombinace s filtrací a tříděním:** Pokud máš na stránce také filtrování nebo třídění, můžeš do URL zahrnout i tyto parametry (např. `https://example.com/articles?page=3&sort=latest`), což umožní uživatelům snadno měnit a sdílet kombinace filtrů a třídění.

### Jak implementovat stránkování s číslem stránky v URL

#### 1. **Backend (Django REST Framework)**

Na backendu se stránkování provádí automaticky, pokud používáš Django REST framework a máš nastavené stránkování v `settings.py`. Například, s `PageNumberPagination` můžeš jednoduše přidat parametr `page` do URL a DRF automaticky použije tento parametr pro určení, kterou stránku má vrátit.

**Příklad API endpointu:**

```python
from rest_framework.pagination import PageNumberPagination
from rest_framework.response import Response
from rest_framework.views import APIView
from .models import Article
from .serializers import ArticleSerializer

class ArticleListView(APIView):
    def get(self, request, *args, **kwargs):
        paginator = PageNumberPagination()
        paginator.page_size = 10
        result_page = paginator.paginate_queryset(Article.objects.all(), request)
        serializer = ArticleSerializer(result_page, many=True)
        return paginator.get_paginated_response(serializer.data)
```

#### 2. **Frontend (Vue.js)**

Na frontendové straně můžeš použít Vue Router pro zpracování a zobrazení čísla stránky z URL.

**Příklad použití Vue Router:**

1. **Nastavení Vue Router:**

   V konfiguračním souboru routeru můžeš definovat cestu, která obsahuje parametr pro číslo stránky:

   ```javascript
   import Vue from 'vue';
   import Router from 'vue-router';
   import ArticleList from './components/ArticleList.vue';

   Vue.use(Router);

   export default new Router({
       routes: [
           {
               path: '/articles',
               name: 'ArticleList',
               component: ArticleList,
               props: route => ({ page: parseInt(route.query.page) || 1 })
           }
       ]
   });
   ```

2. **Zpracování URL v komponentě:**

   V komponentě `ArticleList` použij parametr `page` z URL pro načítání dat:

   ```vue
   <template>
       <div>
           <!-- Zde zobraz články -->
           <ul>
               <li v-for="article in articles" :key="article.id">{{ article.title }}</li>
           </ul>
           <!-- Tlačítka pro přechod mezi stránkami -->
           <button @click="goToPage(page - 1)" :disabled="page <= 1">Previous</button>
           <button @click="goToPage(page + 1)" :disabled="page >= totalPages">Next</button>
       </div>
   </template>

   <script>
   import axios from 'axios';

   export default {
       props: ['page'],
       data() {
           return {
               articles: [],
               totalPages: 1,
           };
       },
       methods: {
           fetchArticles(page) {
               axios.get(`/api/articles?page=${page}`)
                   .then(response => {
                       this.articles = response.data.results;
                       this.totalPages = response.data.total_pages;
                   })
                   .catch(error => {
                       console.error('Error fetching articles:', error);
                   });
           },
           goToPage(page) {
               this.$router.push({ query: { page } });
               this.fetchArticles(page);
           }
       },
       watch: {
           page: function(newPage) {
               this.fetchArticles(newPage);
           }
       },
       created() {
           this.fetchArticles(this.page);
       }
   };
   </script>
   ```

V tomto příkladu Vue Router zpracovává změny v URL a komponenta `ArticleList` reaguje na tyto změny načítáním příslušných dat.

### Shrnutí

Promítnutí čísla stránky do URL je doporučený přístup, který zlepšuje uživatelský zážitek a umožňuje snadnou navigaci a sdílení. Na backendu Django REST framework automaticky zpracovává parametry pro stránkování. Na frontendové straně Vue.js můžeš použít Vue Router k zpracování parametrů URL a načítání dat na základě aktuální stránky.

## Dotaz:

A jak se dělá to, že když se na stránku díváme na mobilu, tak namísto stránkování máme nekonečný seznam?

## ChatGPT:

Nekonečné scrollování, často označované jako „infinite scrolling“, je technika, kdy se další data načítají automaticky, jakmile uživatel dosáhne spodní části stránky. Tento přístup je obzvlášť užitečný pro mobilní zařízení, kde může být tradiční stránkování méně pohodlné. Zde je, jak tuto techniku implementovat:

### 1. **Backend (Django REST Framework)**

Pro nekonečné scrollování bys měl připravit API, které umožňuje načítat data v blocích. Můžeš použít stránkování s `limit` a `offset`, aby API vracelo pouze požadovaný blok dat.

**Příklad nastavení v Django REST framework:**

1. **Vytvoření vlastního stránkování:**

   Pokud používáš `LimitOffsetPagination` z Django REST frameworku, můžeš upravit API endpoint takto:

   ```python
   from rest_framework.pagination import LimitOffsetPagination
   from rest_framework.response import Response
   from rest_framework.views import APIView
   from .models import Article
   from .serializers import ArticleSerializer

   class ArticleListView(APIView):
       def get(self, request, *args, **kwargs):
           paginator = LimitOffsetPagination()
           paginator.default_limit = 10
           paginator.max_limit = 100
           result_page = paginator.paginate_queryset(Article.objects.all(), request)
           serializer = ArticleSerializer(result_page, many=True)
           return paginator.get_paginated_response(serializer.data)
   ```

   Tento přístup umožňuje uživateli načítat data s parametry `limit` a `offset` v dotazu na API.

### 2. **Frontend (Vue.js)**

Na frontendové straně implementuješ logiku pro nekonečné scrollování, která detekuje, kdy uživatel dosáhne spodní části stránky a poté načítá další data.

**Příklad implementace v Vue.js:**

1. **Komponenta pro nekonečné scrollování:**

   Vytvoř komponentu, která zpracovává nekonečné scrollování:

   ```vue
   <template>
       <div @scroll="handleScroll" ref="scrollContainer">
           <ul>
               <li v-for="article in articles" :key="article.id">{{ article.title }}</li>
           </ul>
           <div v-if="loading">Loading...</div>
       </div>
   </template>

   <script>
   import axios from 'axios';

   export default {
       data() {
           return {
               articles: [],
               loading: false,
               offset: 0,
               limit: 10,
               hasMore: true,
           };
       },
       methods: {
           fetchArticles() {
               if (this.loading || !this.hasMore) return;
               this.loading = true;
               axios.get(`/api/articles/?limit=${this.limit}&offset=${this.offset}`)
                   .then(response => {
                       this.articles = [...this.articles, ...response.data.results];
                       this.offset += response.data.results.length;
                       this.hasMore = response.data.results.length > 0;
                   })
                   .catch(error => {
                       console.error('Error fetching articles:', error);
                   })
                   .finally(() => {
                       this.loading = false;
                   });
           },
           handleScroll() {
               const container = this.$refs.scrollContainer;
               const bottom = container.scrollHeight === container.scrollTop + container.clientHeight;
               if (bottom) {
                   this.fetchArticles();
               }
           }
       },
       created() {
           this.fetchArticles();
       }
   };
   </script>

   <style>
   /* Stylování pro scrollování */
   </style>
   ```

   V tomto příkladu:
   - **`fetchArticles()`**: Načítá data z API a přidává je do aktuálně zobrazeného seznamu. Aktualizuje `offset` a `hasMore` podle toho, zda existují další články.
   - **`handleScroll()`**: Detekuje, když uživatel dosáhne spodní části kontejneru, a spouští načítání dalších článků.
   - **`loading`**: Zabraňuje opakovanému načítání, dokud se stávající data nenačtou.

2. **Stylování kontejneru pro scrollování:**

   Ujisti se, že kontejner, který obsahuje seznam článků, má nastavené CSS, které umožňuje scrollování:

   ```css
   /* Například */
   .scroll-container {
       height: 100vh; /* Nebo jiná výška */
       overflow-y: auto;
   }
   ```

### Shrnutí

- **Backend:** Nastav API endpoint s možností stránkování pomocí `limit` a `offset` pro načítání dat v blocích.
- **Frontend:** Implementuj logiku pro nekonečné scrollování, která načítá další data automaticky, když uživatel dosáhne spodní části stránky.

Tímto způsobem zajistíš, že uživatelé budou mít plynulý zážitek z procházení článků bez nutnosti klikání na stránkovací tlačítka.

## Dotaz:

A zkoumalo se někdy jestli je pro uživatele tento přístup přínosný i pro PC a nebo zda na PC lépe působí na uživatelský zážitek přepínání po stránkách?

## ChatGPT:

Ano, bylo provedeno několik studií a analýz, které porovnávají nekonečné scrollování a tradiční stránkování z hlediska uživatelského zážitku na různých zařízeních. Tyto studie často ukazují, že efektivita a přívětivost obou přístupů se liší v závislosti na kontextu, typu obsahu a cílové skupině. Zde je shrnutí hlavních poznatků:

### Výhody a nevýhody nekonečného scrollování

#### **Výhody:**

1. **Plynulý zážitek:** Nekonečné scrollování poskytuje plynulý zážitek bez nutnosti přepínat mezi stránkami, což může být výhodné pro uživatele, kteří chtějí procházet obsah bez přerušení.

2. **Vhodné pro mobilní zařízení:** Na mobilních zařízeních může nekonečné scrollování zlepšit použitelnost tím, že eliminuje potřebu klikání na stránkovací tlačítka, což je uživatelsky příjemnější.

3. **Zvýšení času stráveného na stránce:** Tento přístup může podpořit uživatele, aby strávili více času prohlížením obsahu, protože nevidí hranice mezi stránkami.

#### **Nevýhody:**

1. **Problémy s navigací:** Na velkých stránkách může být obtížné vrátit se na konkrétní místo, kde uživatel přestal, a může být těžké najít konkrétní obsah, pokud je vše načítáno na jedné stránce.

2. **Výkon:** Nekonečné scrollování může způsobit výkonové problémy, pokud není správně optimalizováno, zejména pokud se stránka stává velmi dlouhou.

3. **Ztráta kontextu:** Uživatelé mohou ztratit přehled o tom, kolik obsahu již viděli, a mohou mít problémy s nalezením konkrétních informací.

### Výhody a nevýhody stránkování

#### **Výhody:**

1. **Jednoduchá navigace:** Stránkování umožňuje uživatelům snadno navigovat mezi konkrétními stránkami a poskytuje jasnou strukturu, což může být užitečné při hledání specifických informací.

2. **Menší zatížení na serveru:** Stránkování může být efektivnější, pokud je dobře implementováno, protože načítá menší množství dat najednou.

3. **Lepší kontrola nad zobrazením:** Uživatelé mají lepší přehled o tom, kolik stránek ještě zbývá a jak daleko jsou v procházení obsahu.

#### **Nevýhody:**

1. **Přerušení zážitku:** Uživatelé musí aktivně kliknout na tlačítko pro přechod na další stránku, což může přerušit plynulost prohlížení.

2. **Více kliknutí:** Uživatelé musí kliknout vícekrát pro zobrazení dalších stránek, což může být méně pohodlné, zejména na mobilních zařízeních.

### Co říkají studie a výzkumy

- **Mobilní vs. Desktop:** Studie ukázaly, že nekonečné scrollování je často preferováno na mobilních zařízeních kvůli lepší použitelnosti a snadnějšímu ovládání. Na desktopu však může být tradiční stránkování stále výhodné, zejména pro uživatele, kteří hledají konkrétní informace nebo preferují jasnou strukturu.

- **Obsah typu:** Typ obsahu může také hrát roli. Například pro neomezený obsah, jako jsou sociální média a blogy, může být nekonečné scrollování atraktivní. Na druhé straně, pro e-commerce stránky nebo vyhledávače, kde uživatelé mohou hledat konkrétní produkty nebo informace, může být stránkování vhodnější.

### Doporučení

1. **Kombinace přístupů:** Některé stránky kombinují nekonečné scrollování s tradičním stránkováním. Například načítají další data automaticky, ale také umožňují uživatelům přejít na konkrétní stránku prostřednictvím navigačního menu.

2. **Testování s uživateli:** Nejdůležitější je testovat různé přístupy s cílovou skupinou uživatelů. A/B testování může pomoci zjistit, který přístup je nejefektivnější pro konkrétní aplikaci nebo web.

3. **Optimalizace výkonu:** Bez ohledu na zvolený přístup je důležité optimalizovat výkon, aby se zajistil plynulý a rychlý zážitek pro uživatele.

### Shrnutí

Když se rozhoduješ mezi nekonečným scrollováním a stránkováním, je důležité vzít v úvahu cílovou skupinu uživatelů, typ obsahu a kontext použití. Na mobilních zařízeních bývá nekonečné scrollování výhodnější, zatímco na desktopu může být tradiční stránkování stále užitečné, zejména pro obsah, kde je důležitá struktura a navigace.

## Dotaz:

Takže rád bych se pustil do stránkování a mám před tím pár dotazů.
Jestli jsem to pochopil správně , tak při přechodu z Dango na Vue je potřeba udělat v Django:

1) V settings.py přidat

         REST_FRAMEWORK = {
             'PAGE_SIZE': 10,
             'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
         }

Zde jsi uvedl, že kromě třídy pro stránkování PageNumberPagination, existují i jiné (LimitOffsetPagination, CursorPagination). 
Jaký je mezi nimy rozdíl a podle čeho by se měl člověk rozhodovat, jakou třídu použít? 

2) Vytvoření API endpointu
Zde jsi uvedl, že mohu využít třídy APIView nebo GenericViewSet. 
Jaký je mezi nimi rozdíl a podle čeho bych se měl rozhodnout, kterou z nich mám použít?
Dále jsem se chtěl ještě zeptat, zda mi bude fungovat stránkování, i když nepoužívám:

       queryset = Article.objects.all()
       serializer_class = ArticleSerializer

Ale namísto toho mám specifikovaná pole, které se mají načíst z databáze:

        articles = Article.objects.filter(
            status='publish'
        ).select_related(
            'category',
            'author'
        ).annotate(
            category_name=F('category__name'),
            category_slug=F('category__slug'),
            author_name=F('author__name'),
            author_slug=F('author__slug'),
            author_profile_picture_url=Concat(
                Value(settings.MEDIA_URL),
                F('author__profile_picture_thumbnail'),
                output_field=models.CharField()
            ),
            main_picture_preview_url=Concat(
                Value(settings.MEDIA_URL),
                F('main_picture_preview'),
                output_field=models.CharField()
            )
        ).values(
            'id',
            'slug',
            'main_picture_preview_url',
            'published',
            'title',
            'overview',
            'category_name',
            'category_slug',
            'author_name',
            'author_slug',
            'author_profile_picture_url'
        )

        serializer = ArticlePreviewSerializer(articles, many=True)



A nyní k nastavením na straně VUE:

1) Získat stránkovaná data s API pomocí Axio
Zde je můj aktuální kod z Axio, který jen deklaruje adresu Djanfo serveru:

         import axios from 'axios';
         
         const instance = axios.create({
           baseURL: 'http://localhost:8000/',  // Základní URL pro API
         });

export default instance;

A tebe bych tedy poprosil, aby jsi mi do něj dopsal potřebný kod a rovnou i s vysvětlivkami jednotlivých řádků, abych mu rozuměl.

2) Vytvořit komponentu pro stránkování
Zde bych tě poprosil o příklady, kde výsledkem má být:
šipka do leva pro předchozí stránku (pokud se nejedná o první stránku), číslo předešlé stránky (pokud je), číslo aktuální stránky, číslo následující stránky (pokud je), šipku doprava pro další stránku (pokud je) 

## ChatGPT:
 
### Django - Backend

#### 1. Nastavení `settings.py`

Ano, v `settings.py` přidáš nastavení pro stránkování:

```python
REST_FRAMEWORK = {
    'PAGE_SIZE': 10,
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
}
```

#### Rozdíly mezi `PageNumberPagination`, `LimitOffsetPagination` a `CursorPagination`

- **`PageNumberPagination`**: Umožňuje stránkování podle čísla stránky. Uživatel specifikuje číslo stránky, kterou chce zobrazit (např. `?page=2`). Toto je nejjednodušší a nejčastěji používaná metoda.

- **`LimitOffsetPagination`**: Umožňuje stránkování pomocí limitu a offsetu. Uživatel specifikuje, kolik položek chce zobrazit (limit) a odkud začít (offset) (např. `?limit=10&offset=20`). Je užitečné, když potřebuješ více kontroly nad stránkováním.

- **`CursorPagination`**: Používá se pro zabezpečenější a efektivnější stránkování u velkých datasetů. Načítá další sadu záznamů na základě ukazatele (cursor), což může být bezpečnější a rychlejší než `LimitOffsetPagination`, ale je složitější na implementaci.

#### 2. Vytvoření API endpointu

Rozdíly mezi `APIView` a `GenericViewSet`:

- **`APIView`**: Umožňuje plnou kontrolu nad tím, co se stane při každém HTTP požadavku. Je flexibilnější, ale vyžaduje více kódu.

- **`GenericViewSet`**: Zajišťuje více předdefinovaných funkcí a umožňuje snadno kombinovat různé mixiny pro běžné operace (např. list, create, retrieve, update, delete). Je méně flexibilní než `APIView`, ale zajišťuje rychlejší vývoj.

#### Příklad s `APIView`:

```python
from rest_framework.pagination import PageNumberPagination
from rest_framework.response import Response
from rest_framework.views import APIView
from .models import Article
from .serializers import ArticlePreviewSerializer
from django.db.models import F, Value
from django.db.models.functions import Concat
from django.conf import settings

class ArticleListView(APIView):
    def get(self, request, *args, **kwargs):
        articles = Article.objects.filter(
            status='publish'
        ).select_related(
            'category',
            'author'
        ).annotate(
            category_name=F('category__name'),
            category_slug=F('category__slug'),
            author_name=F('author__name'),
            author_slug=F('author__slug'),
            author_profile_picture_url=Concat(
                Value(settings.MEDIA_URL),
                F('author__profile_picture_thumbnail'),
                output_field=models.CharField()
            ),
            main_picture_preview_url=Concat(
                Value(settings.MEDIA_URL),
                F('main_picture_preview'),
                output_field=models.CharField()
            )
        ).values(
            'id',
            'slug',
            'main_picture_preview_url',
            'published',
            'title',
            'overview',
            'category_name',
            'category_slug',
            'author_name',
            'author_slug',
            'author_profile_picture_url'
        )

        paginator = PageNumberPagination()
        result_page = paginator.paginate_queryset(articles, request)
        serializer = ArticlePreviewSerializer(result_page, many=True)
        return paginator.get_paginated_response(serializer.data)
```

### Vue.js - Frontend

#### 1. Získat stránkovaná data s API pomocí Axios

Aktualizuj svůj soubor s Axiosem:

```javascript
import axios from 'axios';

// Vytvoření instance Axios s nastavenou základní URL pro API
const instance = axios.create({
  baseURL: 'http://localhost:8000/',  // Základní URL pro API
});

// Funkce pro získání dat s stránkováním
instance.getArticles = (page = 1) => {
  return instance.get(`/api/articles/?page=${page}`);
};

export default instance;
```

#### 2. Vytvořit komponentu pro stránkování

Zde je příklad komponenty Vue.js pro stránkování:

```vue
<template>
  <div>
    <ul>
      <li v-for="article in articles" :key="article.id">{{ article.title }}</li>
    </ul>
    <div class="pagination">
      <button @click="changePage(currentPage - 1)" :disabled="currentPage <= 1">←</button>
      <span v-if="currentPage > 1">{{ currentPage - 1 }}</span>
      <span>{{ currentPage }}</span>
      <span v-if="currentPage < totalPages">{{ currentPage + 1 }}</span>
      <button @click="changePage(currentPage + 1)" :disabled="currentPage >= totalPages">→</button>
    </div>
  </div>
</template>

<script>
import axios from './axios-instance';  // Import Axios instance

export default {
  data() {
    return {
      articles: [],
      currentPage: 1,
      totalPages: 1,
    };
  },
  methods: {
    fetchArticles(page = 1) {
      axios.getArticles(page).then(response => {
        this.articles = response.data.results;
        this.currentPage = page;
        this.totalPages = Math.ceil(response.data.count / response.data.page_size);
      }).catch(error => {
        console.error('Error fetching articles:', error);
      });
    },
    changePage(page) {
      if (page > 0 && page <= this.totalPages) {
        this.fetchArticles(page);
      }
    }
  },
  created() {
    this.fetchArticles();
  }
};
</script>

<style>
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
}
.pagination button {
  margin: 0 5px;
}
.pagination span {
  margin: 0 5px;
}
</style>
```

### Vysvětlení kódu

#### Axios instance

- **`baseURL`**: Základní URL pro API.
- **`getArticles`**: Přidána metoda pro získání článků s stránkováním.

#### Vue komponenta

- **`data`**: Uchovává seznam článků, aktuální stránku a celkový počet stránek.
- **`fetchArticles`**: Načítá články z API pro danou stránku a aktualizuje `articles`, `currentPage` a `totalPages`.
- **`changePage`**: Změní stránku, pokud je nová stránka v platném rozsahu.
- **`created`**: Při vytvoření komponenty načte články pro první stránku.

Tento přístup zajišťuje, že stránkování bude fungovat jak na backendu, tak na frontendové straně s Vue.js.

## Dotaz:

Můžeš mi prosím ještě jednou přepsat kod pro vue komponentu s tím, že zde bude jen část pro stránkování:

      <template>
        <div class="pagination">
          <button @click="changePage(currentPage - 1)" :disabled="currentPage <= 1">←</button>
          <span v-if="currentPage > 1">{{ currentPage - 1 }}</span>
          <span>{{ currentPage }}</span>
          <span v-if="currentPage < totalPages">{{ currentPage + 1 }}</span>
          <button @click="changePage(currentPage + 1)" :disabled="currentPage >= totalPages">→</button>
        </div>
      </template>

A s tím ,že toto bude v samostatné komponentě: frontend\src\components\ArticlesView\Pagination.vue
Která pak bude importovaná do frontend\src\views\ArticlesView.vue:

      <template>
        <div class="my-container">
          <h1 class="main-title">All Articles</h1>
          <p class="sub-title">Here is the list of all articles</p>
          <div v-if="articles.length > 0" class="articles-grid">
            <ArticleCard v-for="article in paginatedArticles" :key="article.id" :article="article" />
          </div>
          <div v-else>
            <p>No articles available.</p>
          </div>
          <!-- Add pagination controls here later -->
        </div>
      </template>
      
      <script>
      import api from '../axios';
      import ArticleCard from '@/components/ArticlesView/ArticleCard.vue';
      
      export default {
        name: 'ArticlesView',
        components: {
          ArticleCard
        },
        data() {
          return {
            articles: [],
            currentPage: 1,
            pageSize: 6 // 3 articles per row * 2 rows
          };
        },
        computed: {
          paginatedArticles() {
            const start = (this.currentPage - 1) * this.pageSize;
            const end = start + this.pageSize;
            return this.articles.slice(start, end);
          }
        },
        created() {
          this.fetchArticles();
        },
        methods: {
          async fetchArticles() {
            try {
              const response = await api.get('articles/all');
              this.articles = response.data;
            } catch (error) {
              console.error('Error fetching articles:', error);
            }
          }
        }
      };
      </script>
      
      <style scoped>
      .articles-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 1rem;
      }
      
      @media (max-width: 768px) {
        .articles-grid {
          grid-template-columns: repeat(1, 1fr);
        }
      }
      
      .main-title {
        font-size: 2rem;
        margin-top: 6rem;
      }
      
      .sub-title{
        font-size: 1.5rem;
        color: #6c757d;
        margin-bottom: 4rem;
      }
      </style>

A zároveň mi prosím i odstraň z tohoto souboru kod, který se staral o vykreslení pouze 6-ti článků na stránku.

## ChatGPT:

### Komponenta pro stránkování: `frontend/src/components/ArticlesView/Pagination.vue`

Nejprve vytvoř komponentu pro stránkování v souboru `frontend/src/components/ArticlesView/Pagination.vue`:

```vue
<template>
  <div class="pagination">
    <button @click="changePage(currentPage - 1)" :disabled="currentPage <= 1">←</button>
    <span v-if="currentPage > 1">{{ currentPage - 1 }}</span>
    <span>{{ currentPage }}</span>
    <span v-if="currentPage < totalPages">{{ currentPage + 1 }}</span>
    <button @click="changePage(currentPage + 1)" :disabled="currentPage >= totalPages">→</button>
  </div>
</template>

<script>
export default {
  props: {
    currentPage: {
      type: Number,
      required: true
    },
    totalPages: {
      type: Number,
      required: true
    }
  },
  methods: {
    changePage(page) {
      if (page > 0 && page <= this.totalPages) {
        this.$emit('page-changed', page);
      }
    }
  }
};
</script>

<style scoped>
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
}
.pagination button {
  margin: 0 5px;
}
.pagination span {
  margin: 0 5px;
}
</style>
```

### Import a použití komponenty pro stránkování ve `frontend/src/views/ArticlesView.vue`

Nyní aktualizuj soubor `frontend/src/views/ArticlesView.vue`, aby zahrnoval komponentu pro stránkování a odstranil starý kód pro stránkování:

```vue
<template>
  <div class="my-container">
    <h1 class="main-title">All Articles</h1>
    <p class="sub-title">Here is the list of all articles</p>
    <div v-if="articles.length > 0" class="articles-grid">
      <ArticleCard v-for="article in articles" :key="article.id" :article="article" />
    </div>
    <div v-else>
      <p>No articles available.</p>
    </div>
    <Pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      @page-changed="fetchArticles"
    />
  </div>
</template>

<script>
import api from '../axios';
import ArticleCard from '@/components/ArticlesView/ArticleCard.vue';
import Pagination from '@/components/ArticlesView/Pagination.vue';

export default {
  name: 'ArticlesView',
  components: {
    ArticleCard,
    Pagination
  },
  data() {
    return {
      articles: [],
      currentPage: 1,
      totalPages: 1
    };
  },
  created() {
    this.fetchArticles(this.currentPage);
  },
  methods: {
    async fetchArticles(page) {
      try {
        const response = await api.get(`articles/all?page=${page}`);
        this.articles = response.data.results;
        this.currentPage = page;
        this.totalPages = Math.ceil(response.data.count / response.data.page_size);
      } catch (error) {
        console.error('Error fetching articles:', error);
      }
    }
  }
};
</script>

<style scoped>
.articles-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}

@media (max-width: 768px) {
  .articles-grid {
    grid-template-columns: repeat(1, 1fr);
  }
}

.main-title {
  font-size: 2rem;
  margin-top: 6rem;
}

.sub-title {
  font-size: 1.5rem;
  color: #6c757d;
  margin-bottom: 4rem;
}
</style>
```

### Vysvětlení změn:

#### Pagination.vue
- **`props`**: Přijímá `currentPage` a `totalPages` jako vstupní parametry od nadřazené komponenty.
- **`methods`**: `changePage` kontroluje, zda nová stránka je v platném rozsahu, a vyvolává událost `page-changed`, kterou zpracovává nadřazená komponenta.

#### ArticlesView.vue
- **`components`**: Importuje a registruje komponentu `Pagination`.
- **`data`**: Uchovává seznam článků, aktuální stránku a celkový počet stránek.
- **`created`**: Při vytvoření komponenty načte články pro aktuální stránku.
- **`methods`**: `fetchArticles` aktualizuje seznam článků, aktuální stránku a celkový počet stránek na základě odpovědi z API.
- **`template`**: Komponenta `Pagination` je přidána pod seznam článků a zpracovává událost `page-changed` pro změnu stránky.

Tímto způsobem máš oddělenou komponentu pro stránkování, kterou lze snadno používat a udržovat.

## Dotaz:

Super a díky :-)
A jenom tak bokem, prosím vysvětli mi kdy je ve VUE v script / export default dobré použít 'name' a kdy to není potřeba?

## ChatGPT:

V Vue.js můžeš použít `name` v rámci `export default` v komponentě, ale není to povinné. Pojďme si vysvětlit, kdy a proč je vhodné `name` použít a kdy to není nutné.

### Kdy použít `name`

1. **Debugování**: Pomocí `name` můžeš snadněji identifikovat komponentu v nástrojích pro vývojáře, jako je Vue Devtools. Komponenty s pojmenováním budou mít název v hierarchii komponent, což usnadňuje sledování a ladění.

2. **Globální komponenty**: Pokud registruješ komponentu globálně, je dobré ji pojmenovat, aby byla snadno dostupná a identifikovatelná.

3. **Rekurze**: Pokud má komponenta obsahovat sama sebe (rekurzivní komponenta), potřebuješ ji pojmenovat, aby se mohla referencovat.

4. **Server-side rendering (SSR)**: Při použití SSR může být pojmenování komponent užitečné pro zlepšení výkonu a sledování komponent.

### Kdy `name` není potřeba

1. **Jednoduché projekty**: Pokud pracuješ na malém projektu nebo jednoduché aplikaci, kde máš jen pár komponent, nemusíš nutně používat `name`.

2. **Lokalizované komponenty**: Když komponenta není sdílena nebo používána na více místech, ale je specifická pro danou část aplikace, není nutné ji pojmenovávat.

### Příklad použití `name`

```vue
<template>
  <div>
    <h1>{{ title }}</h1>
  </div>
</template>

<script>
export default {
  name: 'MyComponent', // Pojmenování komponenty
  data() {
    return {
      title: 'Hello, Vue!'
    };
  }
};
</script>

<style scoped>
/* Stylování komponenty */
</style>
```

### Shrnutí

- **Použití `name` je doporučeno pro lepší identifikaci komponent, zejména v komplexnějších projektech.**
- **Pro malé a lokalizované komponenty to není nutnost, ale může to pomoci při debugování.**

Použití `name` je více o zlepšení vývojového procesu a udržovatelnosti kódu, než o technické nutnosti. Pokud cítíš, že by ti pojmenování komponent mohlo pomoci při ladění a organizaci projektu, rozhodně ho používej.

## Dotaz:

Takže z toho, co píšeš usuzuji že je dobré přidat 'name' i do tohoto kodu:

      <template>
        <div class="pagination">
          <button @click="changePage(currentPage - 1)" :disabled="currentPage <= 1">←</button>
          <span v-if="currentPage > 1">{{ currentPage - 1 }}</span>
          <span>{{ currentPage }}</span>
          <span v-if="currentPage < totalPages">{{ currentPage + 1 }}</span>
          <button @click="changePage(currentPage + 1)" :disabled="currentPage >= totalPages">→</button>
        </div>
      </template>
      
      <script>
      export default {
        props: {
          currentPage: {
            type: Number,
            required: true
          },
          totalPages: {
            type: Number,
            required: true
          }
        },
        methods: {
          changePage(page) {
            if (page > 0 && page <= this.totalPages) {
              this.$emit('page-changed', page);
            }
          }
        }
      };
      </script>
      
      <style scoped>
      .pagination {
        display: flex;
        justify-content: center;
        align-items: center;
      }
      .pagination button {
        margin: 0 5px;
      }
      .pagination span {
        margin: 0 5px;
      }
      </style>


A prakticky mi z toho vyplívá, že není chybou, když ho použiju ve všech svých komponentách. Je to tak?

## ChatGPT:

Ano, přesně tak. Přidání `name` do všech tvých komponent není chybou, naopak je to dobrá praxe. Usnadní ti to debugování a udržovatelnost kódu, zvláště pokud pracuješ na větších projektech nebo v týmu.

### Aktualizovaný kód s `name`

#### Pagination.vue

```vue
<template>
  <div class="pagination">
    <button @click="changePage(currentPage - 1)" :disabled="currentPage <= 1">←</button>
    <span v-if="currentPage > 1">{{ currentPage - 1 }}</span>
    <span>{{ currentPage }}</span>
    <span v-if="currentPage < totalPages">{{ currentPage + 1 }}</span>
    <button @click="changePage(currentPage + 1)" :disabled="currentPage >= totalPages">→</button>
  </div>
</template>

<script>
export default {
  name: 'Pagination', // Pojmenování komponenty
  props: {
    currentPage: {
      type: Number,
      required: true
    },
    totalPages: {
      type: Number,
      required: true
    }
  },
  methods: {
    changePage(page) {
      if (page > 0 && page <= this.totalPages) {
        this.$emit('page-changed', page);
      }
    }
  }
};
</script>

<style scoped>
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
}
.pagination button {
  margin: 0 5px;
}
.pagination span {
  margin: 0 5px;
}
</style>
```

#### ArticlesView.vue

```vue
<template>
  <div class="my-container">
    <h1 class="main-title">All Articles</h1>
    <p class="sub-title">Here is the list of all articles</p>
    <div v-if="articles.length > 0" class="articles-grid">
      <ArticleCard v-for="article in articles" :key="article.id" :article="article" />
    </div>
    <div v-else>
      <p>No articles available.</p>
    </div>
    <Pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      @page-changed="fetchArticles"
    />
  </div>
</template>

<script>
import api from '../axios';
import ArticleCard from '@/components/ArticlesView/ArticleCard.vue';
import Pagination from '@/components/ArticlesView/Pagination.vue';

export default {
  name: 'ArticlesView', // Pojmenování komponenty
  components: {
    ArticleCard,
    Pagination
  },
  data() {
    return {
      articles: [],
      currentPage: 1,
      totalPages: 1
    };
  },
  created() {
    this.fetchArticles(this.currentPage);
  },
  methods: {
    async fetchArticles(page) {
      try {
        const response = await api.get(`articles/all?page=${page}`);
        this.articles = response.data.results;
        this.currentPage = page;
        this.totalPages = Math.ceil(response.data.count / response.data.page_size);
      } catch (error) {
        console.error('Error fetching articles:', error);
      }
    }
  }
};
</script>

<style scoped>
.articles-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}

@media (max-width: 768px) {
  .articles-grid {
    grid-template-columns: repeat(1, 1fr);
  }
}

.main-title {
  font-size: 2rem;
  margin-top: 6rem;
}

.sub-title {
  font-size: 1.5rem;
  color: #6c757d;
  margin-bottom: 4rem;
}
</style>
```

### Výhody pojmenování komponent

1. **Lepší debugování**: V nástrojích jako Vue Devtools vidíš jasně pojmenované komponenty, což ti pomůže rychleji identifikovat a řešit problémy.
2. **Konzistence**: Mít pojmenované všechny komponenty přispívá k jednotnému stylu kódování, což je zvlášť užitečné při práci v týmu.
3. **Rekurze**: Pokud někdy potřebuješ komponentu, která se sama volá (rekurzivní komponenta), musí být pojmenovaná.
4. **Jasnější kód**: Pojmenování komponent dělá kód čitelnější a přehlednější, což usnadňuje jeho údržbu a rozšiřování.

Celkově je tedy použití `name` ve všech tvých komponentách dobrá praxe a můžeš ho klidně používat.

## Dotaz:

Teď bych rád aplikoval, aby se mi do URL promítalo číslo stránky.

Zde máš kody z Django, které se asi budou muset měnit:

1) articles/urls/articles.py:

         from django.urls import path
         
         from ..views.article_list_view import ArticleListView
         
         
         # Defining URLs starting with the prefix 'articles/'
         urlpatterns = [
         
             # Display all articles.
             path('all', ArticleListView.as_view(), name='article-list'),
         ]


A tady jen pro jistotu ještě část spřízněného kodu z main/urls.py:

         from django.contrib import admin
         from django.urls import path, include
         from django.conf import settings
         from django.conf.urls.static import static
         
         
         ### Definice URL patternů pro aplikaci
         urlpatterns = [
         
             ### Adresy pro stránky s články
             # Adresy pro stránku s více články
             path('articles/', include('articles.urls.articles')),
         
         ]
         
         # Přidání URL patternů pro statické a média soubory, pokud je nastavený DEBUG mód
         if settings.DEBUG:
             urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
             urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)


2) articles/views/article_list_view.py:

         from django.db import models
         from rest_framework.pagination import PageNumberPagination
         from rest_framework.views import APIView
         from django.db.models import F, Value
         from django.db.models.functions import Concat
         from django.conf import settings
         
         from ..models.article import Article
         from ..serializers.article_preview_serializer import ArticlePreviewSerializer
         
         class ArticleListView(APIView):
             '''
             API View for retrieving a list of articles.
             '''
         
             def get(self, request, *args, **kwargs):
                 '''
                 Handle GET requests to retrieve a list of articles.
                 '''
         
                 articles = Article.objects.filter(
                     status='publish'
                 ).select_related(
                     'category',
                     'author'
                 ).annotate(
                     category_name=F('category__name'),
                     category_slug=F('category__slug'),
                     author_name=F('author__name'),
                     author_slug=F('author__slug'),
                     author_profile_picture_url=Concat(
                         Value(settings.MEDIA_URL),
                         F('author__profile_picture_thumbnail'),
                         output_field=models.CharField()
                     ),
                     main_picture_preview_url=Concat(
                         Value(settings.MEDIA_URL),
                         F('main_picture_preview'),
                         output_field=models.CharField()
                     )
                 ).values(
                     'id',
                     'slug',
                     'main_picture_preview_url',
                     'published',
                     'title',
                     'overview',
                     'category_name',
                     'category_slug',
                     'author_name',
                     'author_slug',
                     'author_profile_picture_url'
                 )
         
                 paginator = PageNumberPagination()
                 result_page = paginator.paginate_queryset(articles, request)
                 serializer = ArticlePreviewSerializer(result_page, many=True)
                 return paginator.get_paginated_response(serializer.data)


A nyní kody z VUE, které se asi budou měnit:

1) frontend\src\router\index.js:

         import { createRouter, createWebHistory } from "vue-router";
         import HomeView from "../views/HomeView.vue";
         import ArticlesView from "../views/ArticlesView.vue";
         import PostView from "../views/PostView.vue";
         
         const routes = [
           {
             path: "/",
             name: "homeView",
             component: HomeView,
           },
           {
             path: '/articles/all',
             name: 'articlesView',
             component: ArticlesView,
           },
           {
             path: "/post",
             name: "postView",
             component: PostView,
           },
         ];
         
         const router = createRouter({
           history: createWebHistory(process.env.BASE_URL),
           routes,
         });
         
         export default router;


2) frontend\src\views\ArticlesView.vue:

         <template>
           <div class="my-container">
             <h1 class="main-title">All Articles</h1>
             <p class="sub-title">Here is the list of all articles</p>
             <div v-if="articles.length > 0" class="articles-grid">
               <ArticleCard v-for="article in articles" :key="article.id" :article="article" />
             </div>
             <div v-else>
               <p>No articles available.</p>
             </div>
             <Pagination
               :currentPage="currentPage"
               :totalPages="totalPages"
               @page-changed="fetchArticles"
             />
           </div>
         </template>
         
         <script>
         import api from '../axios';
         import ArticleCard from '@/components/ArticlesView/ArticleCard.vue';
         import Pagination from '@/components/ArticlesView/Pagination.vue';
         
         export default {
           name: 'ArticlesView',
           components: {
             ArticleCard,
             Pagination
           },
           data() {
             return {
               articles: [],
               currentPage: 1,
               totalPages: 1
             };
           },
           created() {
             this.fetchArticles(this.currentPage);
           },
           methods: {
             async fetchArticles(page) {
               try {
                 const response = await api.get(`articles/all?page=${page}`);
                 this.articles = response.data.results;
                 this.currentPage = page;
                 this.totalPages = Math.ceil(response.data.count / response.data.page_size);
               } catch (error) {
                 console.error('Error fetching articles:', error);
               }
             }
           }
         };
         </script>
         
         <style scoped>
         .articles-grid {
           display: grid;
           grid-template-columns: repeat(3, 1fr);
           gap: 1rem;
         }
         
         @media (max-width: 768px) {
           .articles-grid {
             grid-template-columns: repeat(1, 1fr);
           }
         }
         
         .main-title {
           font-size: 2rem;
           margin-top: 6rem;
         }
         
         .sub-title {
           font-size: 1.5rem;
           color: #6c757d;
           margin-bottom: 4rem;
         }
         </style>


Můžeš mi tedy tyto kody poupravit tak, aby se mi do URL promítalo číslo aktuální stránky?

## ChatGPT:

Jasně, zde jsou úpravy kódů, které ti umožní promítnout číslo aktuální stránky do URL.

### Úpravy v Django

#### 1. `articles/urls/articles.py`

Zde musíme aktualizovat URL tak, aby přijímala parametr stránky.

```python
from django.urls import path
from ..views.article_list_view import ArticleListView

# Defining URLs starting with the prefix 'articles/'
urlpatterns = [
    # Display all articles with optional page parameter.
    path('all/', ArticleListView.as_view(), name='article-list'),
]
```

#### 2. `articles/views/article_list_view.py`

Musíme zajistit, aby `APIView` používala správný paginator a vracela výsledky podle čísla stránky z URL.

```python
from django.db import models
from rest_framework.pagination import PageNumberPagination
from rest_framework.views import APIView
from django.db.models import F, Value
from django.db.models.functions import Concat
from django.conf import settings
from rest_framework.response import Response

from ..models.article import Article
from ..serializers.article_preview_serializer import ArticlePreviewSerializer

class ArticleListView(APIView):
    '''
    API View for retrieving a list of articles.
    '''

    def get(self, request, *args, **kwargs):
        '''
        Handle GET requests to retrieve a list of articles.
        '''
        articles = Article.objects.filter(
            status='publish'
        ).select_related(
            'category',
            'author'
        ).annotate(
            category_name=F('category__name'),
            category_slug=F('category__slug'),
            author_name=F('author__name'),
            author_slug=F('author__slug'),
            author_profile_picture_url=Concat(
                Value(settings.MEDIA_URL),
                F('author__profile_picture_thumbnail'),
                output_field=models.CharField()
            ),
            main_picture_preview_url=Concat(
                Value(settings.MEDIA_URL),
                F('main_picture_preview'),
                output_field=models.CharField()
            )
        ).values(
            'id',
            'slug',
            'main_picture_preview_url',
            'published',
            'title',
            'overview',
            'category_name',
            'category_slug',
            'author_name',
            'author_slug',
            'author_profile_picture_url'
        )

        paginator = PageNumberPagination()
        result_page = paginator.paginate_queryset(articles, request)
        serializer = ArticlePreviewSerializer(result_page, many=True)
        return paginator.get_paginated_response(serializer.data)
```

### Úpravy ve Vue

#### 1. `frontend\src\router\index.js`

Musíme zajistit, aby URL mohla obsahovat parametr stránky.

```javascript
import { createRouter, createWebHistory } from "vue-router";
import HomeView from "../views/HomeView.vue";
import ArticlesView from "../views/ArticlesView.vue";
import PostView from "../views/PostView.vue";

const routes = [
  {
    path: "/",
    name: "homeView",
    component: HomeView,
  },
  {
    path: '/articles/all/:page(\\d+)?',
    name: 'articlesView',
    component: ArticlesView,
    props: true
  },
  {
    path: "/post",
    name: "postView",
    component: PostView,
  },
];

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes,
});

export default router;
```

#### 2. `frontend\src\views\ArticlesView.vue`

Aktualizujeme komponentu tak, aby reagovala na změny v URL.

```vue
<template>
  <div class="my-container">
    <h1 class="main-title">All Articles</h1>
    <p class="sub-title">Here is the list of all articles</p>
    <div v-if="articles.length > 0" class="articles-grid">
      <ArticleCard v-for="article in articles" :key="article.id" :article="article" />
    </div>
    <div v-else>
      <p>No articles available.</p>
    </div>
    <Pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      @page-changed="changePage"
    />
  </div>
</template>

<script>
import api from '../axios';
import ArticleCard from '@/components/ArticlesView/ArticleCard.vue';
import Pagination from '@/components/ArticlesView/Pagination.vue';

export default {
  name: 'ArticlesView',
  components: {
    ArticleCard,
    Pagination
  },
  props: {
    page: {
      type: Number,
      default: 1
    }
  },
  data() {
    return {
      articles: [],
      currentPage: this.page,
      totalPages: 1
    };
  },
  watch: {
    $route(to, from) {
      if (to.params.page !== from.params.page) {
        this.currentPage = to.params.page ? parseInt(to.params.page) : 1;
        this.fetchArticles(this.currentPage);
      }
    }
  },
  created() {
    this.fetchArticles(this.currentPage);
  },
  methods: {
    async fetchArticles(page) {
      try {
        const response = await api.get(`articles/all?page=${page}`);
        this.articles = response.data.results;
        this.currentPage = page;
        this.totalPages = Math.ceil(response.data.count / response.data.page_size);
      } catch (error) {
        console.error('Error fetching articles:', error);
      }
    },
    changePage(page) {
      this.$router.push({ name: 'articlesView', params: { page } });
    }
  }
};
</script>

<style scoped>
.articles-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}

@media (max-width: 768px) {
  .articles-grid {
    grid-template-columns: repeat(1, 1fr);
  }
}

.main-title {
  font-size: 2rem;
  margin-top: 6rem;
}

.sub-title {
  font-size: 1.5rem;
  color: #6c757d;
  margin-bottom: 4rem;
}
</style>
```

### Vysvětlení změn

1. **Django**:
   - V URL konfiguraci jsme zajistili, aby cesta `articles/all/` podporovala volitelný parametr stránky.
   - Aktualizovali jsme `ArticleListView`, aby používala `PageNumberPagination` a správně zpracovávala stránkování.

2. **Vue**:
   - Aktualizovali jsme router, aby podporoval volitelný parametr `page` v URL.
   - V komponentě `ArticlesView` jsme zajistili, že aktuální stránka se bude měnit na základě parametru `page` z URL.
   - Přidali jsme watch na `$route` pro sledování změn URL a odpovídající aktualizaci stránky.
   - Metoda `changePage` nyní aktualizuje URL pomocí `$router.push`, což zajistí, že se aktuální stránka promítne do URL.

## Dotaz:

Promiň jdeme na to moc rychle a já potřebuji trochu zpomalit a udělat si jasno.
Začneme nejprve pouze kodem v souboru articles/urls/articles.py:

rom django.urls import path

from ..views.article_list_view import ArticleListView


# Defining URLs starting with the prefix 'articles/'
urlpatterns = [

    # Display all articles.
    path('all/', ArticleListView.as_view(), name='article-list'),
    # URL path to handle pagination with page number in the path
    path('all/<int:page>/', ArticleListView.as_view(), name='article-list-page'),
]

Je dobré zde mít uvedeny takto dvě varianty adres, jedna s číslem stránky a druhá bez něj?
Napadlo mě že je to tu třeba proto, kdyby některý obsah měl pouze jednu stránku, ale i tak, by zde mohla být jednička na konci. Pak mě napadlo, že to tak je protože, když bych byl na mobilu a měl nekonečné srolování, tak číslo stránky se neuvádí.
Je jeda z těchto teorií pravdivá a nebo by zde měla být uvedena adresa na všechny články pouze v jednom provedení (např. se stránkováním)?

## ChatGPT:

Vaše úvahy jsou validní a můžeme to analyzovat hlouběji. Uvedení dvou variant URL (jedna s číslem stránky a druhá bez) může mít různé důvody. Zde jsou některé faktory, které by mohly ovlivnit toto rozhodnutí:

### Důvody pro dvě varianty URL:
1. **Flexibilita pro různé použití**:
   - **Bez čísla stránky**: Toto může být užitečné pro případy, kdy chcete zobrazit výchozí nebo první stránku článků bez specifikování stránky v URL. Například pokud uživatelé přijdou na stránku článků poprvé, mohou být přesměrováni na první stránku.
   - **S číslem stránky**: Toto je užitečné pro explicitní navigaci mezi stránkami. Když uživatelé přepínají mezi stránkami, každá stránka má specifickou URL, což může být užitečné pro záložky nebo sdílení konkrétní stránky.

2. **SEO a uživatelská zkušenost**:
   - Mít různé URL pro každou stránku umožňuje vyhledávačům indexovat obsah efektivněji a také umožňuje uživatelům sdílet konkrétní stránky.

### Důvody pro jednu variantu URL (se stránkováním):
1. **Jednoduchost**:
   - Pouze jedna varianta URL může zjednodušit kód a logiku aplikace.
   - Když se stránka načte bez čísla stránky, můžete ji přesměrovat na první stránku pomocí JavaScriptu nebo serverového přesměrování.

2. **Konzistence**:
   - Konzistentní URL struktura může zlepšit uživatelskou zkušenost a navigaci.

### Doporučení:
Vzhledem k tomu, že stránkování je běžnou praxí a poskytuje lepší kontrolu nad tím, která stránka je zobrazena, doporučil bych použít pouze jednu variantu URL, která explicitně obsahuje číslo stránky. Tímto způsobem je každá stránka jednoznačně identifikována a je možné jednoduše spravovat stránkování.

Zde je tedy upravený kód pro `articles/urls/articles.py` pouze s jednou variantou URL se stránkováním:

```python
from django.urls import path

from ..views.article_list_view import ArticleListView

# Defining URLs starting with the prefix 'articles/'
urlpatterns = [
    # URL path to handle pagination with page number in the path
    path('all/<int:page>/', ArticleListView.as_view(), name='article-list-page'),
]
```

Pokud chcete mít výchozí cestu bez čísla stránky, která automaticky přesměruje na první stránku, můžete přidat přesměrování v pohledu nebo použít Django `redirect`:

```python
from django.urls import path
from django.shortcuts import redirect
from ..views.article_list_view import ArticleListView

# Defining URLs starting with the prefix 'articles/'
urlpatterns = [
    # Redirect to the first page if no page number is specified
    path('all/', lambda request: redirect('article-list-page', page=1)),
    # URL path to handle pagination with page number in the path
    path('all/<int:page>/', ArticleListView.as_view(), name='article-list-page'),
]
```

Tímto způsobem se zajistí, že pokud uživatelé přijdou na stránku bez specifikovaného čísla stránky, budou přesměrováni na první stránku.

## Dotaz:

Ok nyní se můžeme vrhnout na kod soboru articles/views/article_list_view.py.

Zde jsi mi uvedl toto:

        paginator = PageNumberPagination()
        paginator.page_size = 10
        result_page = paginator.paginate_queryset(articles, request)
        serializer = ArticlePreviewSerializer(result_page, many=True)
        return paginator.get_paginated_response(serializer.data)

Můžeš mi rozepsat jednotlivé řádky a napsat k nim co který dělá.

## ChatGPT:

Samozřejmě, zde je podrobné vysvětlení jednotlivých řádků kódu ve `views/article_list_view.py`:

```python
paginator = PageNumberPagination()
```
- **Vytvoření instance `PageNumberPagination`**: Tento řádek vytvoří novou instanci třídy `PageNumberPagination` z Django REST frameworku. Tato třída je zodpovědná za stránkování dat, tedy rozdělení výsledků do více stránek.

```python
paginator.page_size = 10
```
- **Nastavení velikosti stránky**: Tento řádek nastaví, kolik položek bude zobrazeno na jedné stránce. V tomto případě je to 10 položek na stránku.

```python
result_page = paginator.paginate_queryset(articles, request)
```
- **Stránkování querysetu**: Tento řádek použije `paginator` na queryset `articles` a zohlední aktuální požadavek (`request`). `paginate_queryset` vezme v úvahu aktuální stránku (například `page=2`), kterou klient požaduje, a vrátí pouze tu část querysetu, která odpovídá této stránce. Výsledkem je `result_page`, což je podmnožina `articles`, která odpovídá požadované stránce.

```python
serializer = ArticlePreviewSerializer(result_page, many=True)
```
- **Serializace dat**: Tento řádek vezme `result_page` a předá jej do serializátoru `ArticlePreviewSerializer`, aby se data převedla do formátu JSON. Parametr `many=True` říká serializátoru, že má zpracovat více instancí (v tomto případě články).

```python
return paginator.get_paginated_response(serializer.data)
```
- **Vrácení stránkované odpovědi**: Tento řádek použije metodu `get_paginated_response` na `paginator` k vytvoření odpovědi, která zahrnuje nejen serializovaná data, ale také metadata o stránkování (například celkový počet položek, počet stránek, aktuální stránku atd.). Tato metoda vrátí kompletní HTTP odpověď, která bude odeslána klientovi.

### Celkový přehled:
1. **Vytvoření paginatoru**: Inicializuje se `PageNumberPagination`.
2. **Nastavení velikosti stránky**: Definuje se, kolik položek bude na jedné stránce.
3. **Stránkování querysetu**: Na základě požadavku se vybere konkrétní stránka dat z querysetu `articles`.
4. **Serializace dat**: Vybraná data se převedou do formátu JSON pomocí serializátoru.
5. **Vrácení stránkované odpovědi**: Odpověď se odesílá klientovi, včetně dat a metadat o stránkování.


## Dotaz:

A je to tak, že řádek paginator.page_size = 10 přebíjí nastavení v settings.py:
      
      REST_FRAMEWORK = {
          'PAGE_SIZE': 6,
          'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
      }

A čistě teoreticky, když v kodu použiji paginator.page_size = 10, znamená to že bych v settings.py aninemusel mít tento řádek:

    'PAGE_SIZE': 6,

A nebo je doporučený postup používat hlavně nastavení v settings.py a v samotném kodu pohledu pouze pokud se tento údaj nějak mění?

## ChatGPT:

V Django REST frameworku se dají nastavit výchozí hodnoty pro stránkování v `settings.py` a zároveň je možné tyto hodnoty přepsat přímo v pohledech (views). Pojďme si to vysvětlit podrobněji:

### Jak funguje nastavení `PAGE_SIZE` v `settings.py`

V `settings.py` můžete definovat výchozí hodnoty pro stránkování:

```python
REST_FRAMEWORK = {
    'PAGE_SIZE': 6,
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
}
```

Tento kód říká, že:
- Výchozí velikost stránky je 6 položek.
- `PageNumberPagination` bude použit jako výchozí třída pro stránkování, pokud nebude specifikováno jinak.

### Jak přepisovat výchozí nastavení v pohledech

Pokud chcete v konkrétním pohledu použít jinou velikost stránky, než je ta, která je nastavena v `settings.py`, můžete to udělat přímo v pohledu. Například:

```python
paginator = PageNumberPagination()
paginator.page_size = 10
```

### Vztah mezi nastavením v `settings.py` a přepsáním v pohledu

1. **Pokud přepíšete `page_size` v pohledu**: Hodnota, kterou nastavíte v pohledu (`paginator.page_size = 10`), přebíjí výchozí hodnotu nastavenou v `settings.py`. Tedy v tomto případě bude velikost stránky 10, i když `settings.py` říká 6.

2. **Pokud v pohledu nenastavíte žádnou velikost stránky**: Použije se výchozí hodnota z `settings.py`. V tomto případě bude velikost stránky 6.

### Doporučený postup

- **Používejte `settings.py` pro globální výchozí hodnoty**: Je dobré mít výchozí nastavení v `settings.py`, protože to zajišťuje konzistentní chování napříč celou aplikací.

- **Přepisujte hodnoty v pohledech, pokud je to potřeba**: Pokud konkrétní pohled vyžaduje jinou velikost stránky než ta, která je globálně nastavena, můžete ji přepsat. Tímto způsobem můžete mít specifické nastavení pro různé části aplikace, kde je to vhodné.

### Shrnutí

- **Nastavení v `settings.py`**: Definuje výchozí hodnoty pro celou aplikaci.
- **Přepsání v pohledu**: Přepisuje výchozí hodnoty pouze pro konkrétní pohled.

### Praktické použití

Pokud například ve většině částí aplikace chcete, aby byla velikost stránky 6, ale v nějakém specifickém pohledu chcete, aby byla velikost stránky 10, můžete použít přepisování v pohledu. Pokud ale všechny pohledy budou mít stejnou velikost stránky, můžete definovat `page_size` pouze v `settings.py` a v pohledech tuto hodnotu nenastavovat.

### Příklad s komentáři:

```python
# Nastavení v settings.py
REST_FRAMEWORK = {
    'PAGE_SIZE': 6,  # Výchozí velikost stránky pro všechny pohledy
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
}

# V konkrétním pohledu můžete přepsat výchozí nastavení
class ArticleListView(APIView):
    def get(self, request, *args, **kwargs):
        paginator = PageNumberPagination()
        paginator.page_size = 10  # Přepsání výchozí velikosti stránky na 10
        result_page = paginator.paginate_queryset(articles, request)
        serializer = ArticlePreviewSerializer(result_page, many=True)
        return paginator.get_paginated_response(serializer.data)
```

Tímto způsobem můžete flexibilně přizpůsobit stránkování podle potřeb jednotlivých částí vaší aplikace.

## Dotaz:

Paráda, a nyní se tedy můůžeme vrhnout na kody pro VUE.
Nejprve tedy frontend\src\router\index.js:

Zde jsi mi jednou uvedl zápis pro stránkování takto:

     {
       path: '/articles/all/:page(\\d+)?',
       name: 'articlesView',
       component: ArticlesView,
     },

A jednou takto:

     {
       path: '/articles/all',
       name: 'articlesView',
       component: ArticlesView,
       props: route => ({ page: parseInt(route.query.page) || 1 })  // Přidání podpory pro query parametry
     },

Jaký je mezi nimi rozdíl a jak se rozhodnout, který použít?

## ChatGPT:

V rámci Vue Routeru existují dva hlavní způsoby, jak zpracovat stránkování v URL. Každý přístup má své výhody a nevýhody. Pojďme se podívat na oba způsoby, které jsi zmínil, a na to, jak se rozhodnout, který použít.

### 1. **URL s parametrem cesty (Path Parameter)**

```javascript
{
  path: '/articles/all/:page(\\d+)?',
  name: 'articlesView',
  component: ArticlesView,
}
```

**Popis:**
- Tento zápis používá parametr cesty (path parameter). `:page` je dynamická část URL, která může obsahovat číslo stránky.
- `\\d+` je regulární výraz, který říká, že `:page` musí být číslo (nebo může být prázdný, protože `?` znamená, že parametr je volitelný).

**Výhody:**
- URL vypadá čistěji a je přehledná (např. `/articles/all/2` pro druhou stránku).
- Snadnější pro SEO, protože parametry jsou součástí cesty.

**Nevýhody:**
- Parametry v cestě jsou méně flexibilní než dotazové parametry, pokud potřebujete použít více než jeden parametr.
- Může být obtížnější přidat další parametry do URL bez změny struktury cesty.

### 2. **URL s dotazovými parametry (Query Parameters)**

```javascript
{
  path: '/articles/all',
  name: 'articlesView',
  component: ArticlesView,
  props: route => ({ page: parseInt(route.query.page) || 1 })  // Přidání podpory pro query parametry
}
```

**Popis:**
- Tento zápis používá dotazové parametry (query parameters). Číslo stránky je předáváno jako dotazový parametr (`?page=2`).
- `props: route => ({ page: parseInt(route.query.page) || 1 })` přetváří dotazový parametr na `page` a poskytuje ho komponentě jako prop.

**Výhody:**
- Flexibilní a snadné přidat více parametrů bez změny struktury cesty (např. `/articles/all?page=2&sort=recent`).
- Dotazové parametry jsou vhodné pro ne-navigační data, která nejsou součástí hlavní struktury URL.

**Nevýhody:**
- URL může být méně přehledná a delší (např. `/articles/all?page=2`).
- Dotazové parametry mohou být méně přívětivé pro SEO ve srovnání s parametry v cestě.

### Rozhodování, který přístup použít

**Zvolte parametr cesty (Path Parameter) pokud:**
- Chcete mít čistší a přehlednější URL.
- Chcete mít URL, která lépe podporuje SEO.
- Nepotřebujete použít více dotazových parametrů.

**Zvolte dotazové parametry (Query Parameters) pokud:**
- Potřebujete flexibilitu pro více než jeden parametr.
- Máte komplexní požadavky na filtry nebo řazení.
- Preferujete jednodušší úpravy URL bez změny struktury cesty.

### Příklad Implementace

#### Parametr cesty (Path Parameter)

**`frontend/src/router/index.js`**

```javascript
{
  path: '/articles/all/:page(\\d+)?',
  name: 'articlesView',
  component: ArticlesView,
  props: route => ({ page: parseInt(route.params.page) || 1 })  // Předání parametru `page` jako prop
}
```

**`frontend/src/views/ArticlesView.vue`**

```javascript
export default {
  name: 'ArticlesView',
  props: ['page'],
  data() {
    return {
      articles: [],
      currentPage: this.page,
      totalPages: 1
    };
  },
  watch: {
    page(newPage) {
      this.currentPage = newPage;
      this.fetchArticles(newPage);
    }
  },
  created() {
    this.fetchArticles(this.currentPage);
  },
  methods: {
    async fetchArticles(page) {
      try {
        const response = await api.get(`articles/all/${page}`);
        this.articles = response.data.results;
        this.totalPages = Math.ceil(response.data.count / response.data.page_size);
      } catch (error) {
        console.error('Error fetching articles:', error);
      }
    }
  }
};
```

#### Dotazové parametry (Query Parameters)

**`frontend/src/router/index.js`**

```javascript
{
  path: '/articles/all',
  name: 'articlesView',
  component: ArticlesView,
  props: route => ({ page: parseInt(route.query.page) || 1 })  // Předání dotazového parametru `page` jako prop
}
```

**`frontend/src/views/ArticlesView.vue`**

```javascript
export default {
  name: 'ArticlesView',
  props: ['page'],
  data() {
    return {
      articles: [],
      currentPage: this.page,
      totalPages: 1
    };
  },
  watch: {
    page(newPage) {
      this.currentPage = newPage;
      this.fetchArticles(newPage);
    }
  },
  created() {
    this.fetchArticles(this.currentPage);
  },
  methods: {
    async fetchArticles(page) {
      try {
        const response = await api.get(`articles/all?page=${page}`);
        this.articles = response.data.results;
        this.totalPages = Math.ceil(response.data.count / response.data.page_size);
      } catch (error) {
        console.error('Error fetching articles:', error);
      }
    }
  }
};
```

Obě metody jsou platné, a rozhodnutí závisí na specifických požadavcích tvého projektu.

