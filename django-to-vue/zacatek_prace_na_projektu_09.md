## Dotaz:

Ahojky :-)
Učíme se VUE tím, že do něj předělávám frontend pro svůj projekt psaný čistě v Django.
Už máme za sebou stránku s výpisem všech článků a dneska bych chtěl začít se stránkou pro výpis jednoho článku.

To první, s čím bych chtěl začít je definovat k kartičce pro náhled jednoho článku pro stránku s  výpisem článků, aby část ArticleCardBody celá byla odkazem na:

      {
        path: "/article/<slug:slug>/",
        name: "ArticleView",
        component: ArticleView,
      },

Zde je kod pro tuto komponentu:

    <template>
      <div class="article-details">
        <h4 class="article-title">{{ title }}</h4>
        <p class="article-overview">{{ overview }}</p>
      </div>
    </template>
    
    <script>
    export default {
      name: 'ArticleDetails',
      props: {
        title: {
          type: String,
          required: true
        },
        overview: {
          type: String,
          required: true
        }
      }
    };
    </script>
    
    <style scoped>
    .article-title {
      margin: 0.5rem 0;
      color: rgba(var(--bs-dark-rgb));
    }
    
    .article-overview {
      font-size: 0.875rem;
      color: #6c757d;
      margin-bottom: 1rem;
    }
    </style>

S tím že hodnota pro slug je uložena pod klíčem 'slug'

## ChatGPT:

Ahoj! Můžeme přidat odkaz na celou kartičku článku pomocí `router-link` ve Vue Routeru. `router-link` umožňuje vytvořit odkaz, který naviguje na specifikovanou cestu. Přizpůsobíme tvůj kód tak, aby celé tělo článku bylo klikatelné a vedlo na stránku s detailním zobrazením článku.

Tady je aktualizovaná verze komponenty:

```vue
<template>
  <router-link :to="articleLink" class="article-details-link">
    <div class="article-details">
      <h4 class="article-title">{{ title }}</h4>
      <p class="article-overview">{{ overview }}</p>
    </div>
  </router-link>
</template>

<script>
export default {
  name: 'ArticleDetails',
  props: {
    title: {
      type: String,
      required: true
    },
    overview: {
      type: String,
      required: true
    },
    slug: {
      type: String,
      required: true
    }
  },
  computed: {
    articleLink() {
      return `/article/${this.slug}/`;
    }
  }
};
</script>

<style scoped>
.article-details-link {
  text-decoration: none;
  color: inherit;
}

.article-title {
  margin: 0.5rem 0;
  color: rgba(var(--bs-dark-rgb));
}

.article-overview {
  font-size: 0.875rem;
  color: #6c757d;
  margin-bottom: 1rem;
}
</style>
```

**Co se změnilo:**

1. Přidali jsme `router-link` kolem celého obsahu kartičky.
2. Přidali jsme nový `prop` s názvem `slug`, který bude obsahovat slug článku.
3. Vytvořili jsme computed property `articleLink`, která vrací správnou URL pro článek.
4. Přidali jsme styl `.article-details-link` pro odstranění podtržení a zachování původní barvy textu.

Tímto způsobem bude celá kartička klikací a povede na stránku s detailem článku.

# Dotaz:

Super, takže momentálně jsem v bodu, kdy mám otevřenou stránku pro jeden článek:
http://localhost:8080/article/historie-vesmiru/
Stránka je momentálně prázdná, protože pro ni ještě nemám kod.
To první, co bych chtěl je přepsat tento pohled, který je pro stránku se všemi články, a v dotazu tedy vyhledává všechny publikované články:

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
        paginator.page_size = 6
        result_page = paginator.paginate_queryset(articles, request)
        serializer = ArticlePreviewSerializer(result_page, many=True)
        response = paginator.get_paginated_response(serializer.data)
        response.data['page_size'] = paginator.page_size
        return response

Na třídu ArticleDetailView, která bude mít stejný dotaz, akorát bude vyhledávat jeden článek a to podle slugu obdrženého v adrese.
Stránka nebude potřebovat stránkování, takže kod pro stránkování můžeš vynechat.

## ChatGPT:

Rozumím, můžeme vytvořit novou třídu `ArticleDetailView`, která bude vyhledávat jeden článek podle slugu a vrátí jeho detailní informace. Tady je kód pro tuto novou třídu:

```python
from django.db import models
from rest_framework.views import APIView
from rest_framework.response import Response
from django.db.models import F, Value
from django.db.models.functions import Concat
from django.conf import settings

from ..models.article import Article
from ..serializers.article_detail_serializer import ArticleDetailSerializer

class ArticleDetailView(APIView):
    '''
    API View for retrieving a single article by its slug.
    '''

    def get(self, request, slug, *args, **kwargs):
        '''
        Handle GET requests to retrieve a single article by its slug.
        '''

        try:
            article = Article.objects.filter(
                status='publish',
                slug=slug
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
                main_picture_url=Concat(
                    Value(settings.MEDIA_URL),
                    F('main_picture'),
                    output_field=models.CharField()
                )
            ).values(
                'id',
                'slug',
                'main_picture_url',
                'published',
                'title',
                'content',
                'category_name',
                'category_slug',
                'author_name',
                'author_slug',
                'author_profile_picture_url'
            ).first()

            if not article:
                return Response({"detail": "Article not found"}, status=404)

            serializer = ArticleDetailSerializer(article)
            return Response(serializer.data)

        except Article.DoesNotExist:
            return Response({"detail": "Article not found"}, status=404)
```

**Změny a nové části:**

1. **Vyhledávání článku podle slugu:** Používáme `slug` z URL pro vyhledávání článku.
2. **Vyloučení stránkování:** Vzhledem k tomu, že se vyhledává pouze jeden článek, stránkování není potřeba.
3. **Podrobnější informace o článku:** Kromě základních informací přidáme i obsah článku.
4. **Vytvoření serializéru pro detail článku:** Použijeme nový serializer `ArticleDetailSerializer`, který bude obsahovat všechny potřebné atributy článku.

Nyní musíme vytvořit `ArticleDetailSerializer`. Zde je ukázka:

```python
from rest_framework import serializers

class ArticleDetailSerializer(serializers.Serializer):
    id = serializers.IntegerField()
    slug = serializers.SlugField()
    main_picture_url = serializers.URLField()
    published = serializers.DateTimeField()
    title = serializers.CharField(max_length=255)
    content = serializers.CharField()
    category_name = serializers.CharField(max_length=255)
    category_slug = serializers.SlugField()
    author_name = serializers.CharField(max_length=255)
    author_slug = serializers.SlugField()
    author_profile_picture_url = serializers.URLField()
```

**Co se děje v tomto serializeru:**

- Definujeme všechna pole, která budou součástí detailu článku.
- Používáme `CharField`, `SlugField`, `IntegerField`, `DateTimeField` a `URLField` podle typu dat, která očekáváme.

Tímto způsobem by měl váš nový pohled pro detail článku fungovat správně. Můžete ho přidat do vašeho `urls.py` souboru jako novou cestu:

```python
from django.urls import path
from .views import ArticleDetailView

urlpatterns = [
    path('article/<slug:slug>/', ArticleDetailView.as_view(), name='article-detail'),
    # ostatní cesty...
]
```

Tímto způsobem by měla být stránka s detailem článku dostupná na `/article/<slug:slug>/`.

## Dotaz:

Super a díky :-)
Teď bych po tobě potřeboval dopsat kod pro pohled ve VUE pro stránku s jedním článkem, který by mi zatím jen zobrazil hlavní obrázek: frontend\src\views\ArticleDetailView.vue

Tady je její momentální podoba:

    <template>
      <div>
        <ArticleImage 
          :src="article.main_picture_preview_url" 
          :alt="article.title" 
        />
      </div>
    </template>
    
    <script>
    import ArticleImage from '@/components/ArticleDetailView/ArticleViewImage.vue';
    
    export default {
      name: 'ArticleView',
      components: {
        ArticleImage,
      }
    }
    </script>
    
    <style scoped>
    </style>

## ChatGPT:

Abychom mohli zobrazit hlavní obrázek článku, potřebujeme přidat do komponenty `ArticleDetailView` logiku pro získání dat článku z API. Následně tato data zobrazíme v šabloně.

Aktualizovaná podoba souboru `ArticleDetailView.vue` bude vypadat takto:

```vue
<template>
  <div>
    <ArticleImage 
      v-if="article"
      :src="article.main_picture_url" 
      :alt="article.title" 
    />
  </div>
</template>

<script>
import axios from 'axios';
import ArticleImage from '@/components/ArticleDetailView/ArticleViewImage.vue';

export default {
  name: 'ArticleView',
  components: {
    ArticleImage,
  },
  data() {
    return {
      article: null,
    };
  },
  methods: {
    fetchArticle() {
      const slug = this.$route.params.slug;
      axios.get(`/api/article/${slug}/`)
        .then(response => {
          this.article = response.data;
        })
        .catch(error => {
          console.error('There was an error fetching the article:', error);
        });
    },
  },
  created() {
    this.fetchArticle();
  }
};
</script>

<style scoped>
</style>
```

**Vysvětlení:**

1. **Import Axiosu:** Používáme `axios` pro HTTP požadavky.
2. **Data:** Přidáváme reaktivní `data` s proměnnou `article`, která bude obsahovat informace o článku.
3. **Metoda `fetchArticle`:** Tato metoda získá slug z URL a zavolá API pro získání detailu článku. Po úspěšném získání dat je uložíme do `article`.
4. **`created` Hook:** Tato metoda se zavolá při vytvoření komponenty a zavolá `fetchArticle` pro získání dat.
5. **Podmíněné vykreslování:** Používáme `v-if="article"` v šabloně, abychom zajistili, že se komponenta `ArticleImage` zobrazí pouze tehdy, když jsou data článku k dispozici.

Tímto způsobem bude komponenta `ArticleDetailView` získávat a zobrazovat hlavní obrázek článku podle slugu z URL. Ujistěte se, že API endpoint `/api/article/<slug>/` funguje a vrací správná data.

## Dotaz:

Nepodařilo se mi zobrazit stránku pro jeden článek a tak jsem skusil minimalizovat možné příčiny a pro vykreslení stránky použít jen tento kod:

<template>
  <div class="my-container">
    <p>AAAAAAAAAAAAAAAAAAAAAAA</p>
  </div>
</template>

<script>
</script>

<style scoped>
</style>

Ale i tak je stránka naprosto prázdná a nikde v terminálu nevidím žádnou chybu. Takže počítám, že špatně bude něco s mým nastavením.

Zde je pro jistotu kod pro soubor frontend\src\router\index.js: 

        import { createRouter, createWebHistory } from "vue-router";
        import HomeView from "../views/HomeView.vue";
        import ArticlesListView from "../views/ArticlesListView.vue";
        import ArticleDetailView from "../views/ArticleDetailView.vue";
        
        const routes = [
          {
            path: "/",
            name: "HomeView",
            component: HomeView,
          },
          {
            path: '/articles/all/:page?',
            name: 'ArticlesListView',
            component: ArticlesListView,
          },
          {
            path: "/article/<slug:slug>/",
            name: "ArticleDetailView",
            component: ArticleDetailView,
          },
        ];
        
        const router = createRouter({
          history: createWebHistory(process.env.BASE_URL),
          routes,
        });
        
        export default router;


Ten je ale podle mě v pořádku, protože nikde v terminálu a  ani konzoli nevidím žádný chybový oznam.

Na co jiného bych se měl zaměřit aby se mi povedlo zobrazit na stránce pro detail článku ty velká AAA?

## ChatGPT:

Rozumím. Zkontrolujme několik věcí, které mohou způsobovat, že se stránka nezobrazuje správně.

1. **Cesta k souboru `ArticleDetailView.vue`:** Ujistěte se, že cesta k souboru je správná a Vue Router jej správně importuje.
2. **Route Matching:** Ujistěte se, že cesta `/article/<slug:slug>/` je správně definována a Vue Router ji správně rozpoznává.

Především opravíme definici cesty ve Vue Routeru. Vue Router používá dynamické segmenty cesty bez úhlových závorek. Změňme to takto:

```javascript
import { createRouter, createWebHistory } from "vue-router";
import HomeView from "../views/HomeView.vue";
import ArticlesListView from "../views/ArticlesListView.vue";
import ArticleDetailView from "../views/ArticleDetailView.vue";

const routes = [
  {
    path: "/",
    name: "HomeView",
    component: HomeView,
  },
  {
    path: '/articles/all/:page?',
    name: 'ArticlesListView',
    component: ArticlesListView,
  },
  {
    path: "/article/:slug/",
    name: "ArticleDetailView",
    component: ArticleDetailView,
  },
];

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes,
});

export default router;
```

**Ujisti se, že soubor `ArticleDetailView.vue` je ve správném adresáři `frontend/src/views/` a že se jmenuje přesně `ArticleDetailView.vue`.

Také zkontroluj, že v hlavním souboru `App.vue` je `<router-view>`:

```vue
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
/* Vaše styly */
</style>
```

Nyní by aktualizovaný router a minimální kód pro `ArticleDetailView.vue` měly vypadat takto:

`frontend/src/views/ArticleDetailView.vue`:

```vue
<template>
  <div class="my-container">
    <p>AAAAAAAAAAAAAAAAAAAAAAA</p>
  </div>
</template>

<script>
export default {
  name: 'ArticleDetailView',
};
</script>

<style scoped>
.my-container {
  padding: 20px;
}
</style>
```

Po těchto úpravách zkontroluj, zda se stránka pro detail článku zobrazuje správně. Měla by zobrazit text "AAAAAAAAAAAAAAAAAAAAAAA" při přístupu na cestu `/article/some-slug/`. Pokud se stále nezobrazuje, zkontroluj konzoli v prohlížeči a ujisti se, že neexistují žádné chyby JavaScriptu nebo problémy s importem.

## Dotaz:

Tak AAA se mi už zobrazuje, takže na stránku se dostanu - za to dík. Ale stále se mi nezobrazují data pro článek. 
Skusil jsem si do pohledu v Django dát skušební tisk:

    from django.db import models
    from rest_framework.views import APIView
    from rest_framework.response import Response
    from django.db.models import F, Value
    from django.db.models.functions import Concat
    from django.conf import settings
    
    from ..models.article import Article
    from ..serializers.article_detail_serializer import ArticleDetailSerializer
    
    class ArticleDetailView(APIView):
        '''
        API View for retrieving a single article by its slug.
        '''
    
        def get(self, request, slug, *args, **kwargs):
            '''
            Handle GET requests to retrieve a single article by its slug.
            '''
            print("################ get-slug", slug)
    
            try:
                article = Article.objects.filter(
                    slug=slug
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
                    main_picture_url=Concat(
                        Value(settings.MEDIA_URL),
                        F('main_picture'),
                        output_field=models.CharField()
                    )
                ).values(
                    'id',
                    'slug',
                    'main_picture_url',
                    'published',
                    'title',
                    'content',
                    'category_name',
                    'category_slug',
                    'author_name',
                    'author_slug',
                    'author_profile_picture_url'
                ).first()
    
                if not article:
                    return Response({"detail": "Article not found"}, status=404)
    
                print("########### Article: ", article)
                serializer = ArticleDetailSerializer(article)
                return Response(serializer.data)
    
            except Article.DoesNotExist:
                return Response({"detail": "Article not found"}, status=404)


Ale v terminálu se nic neobjevilo a tak počítám, že chyba bude někde ve volání z VUE na Django

Potřeboval bych tedy postupně projít celou cestu a chybu dohledat.

Začneme rozborem kodu, kterým je volaná stránka, a to odkazem uvnitř kartičky náhledu článku:
frontend\src\components\ArticlesListView\ArticleCard\ArticleCardBody.vue

Kde je tento kod:

    <template>
      <router-link :to="articleLink" class="article-details-link">
        <div class="article-details">
          <h4 class="article-title">{{ title }}</h4>
          <p class="article-overview">{{ overview }}</p>
        </div>
      </router-link>
    </template>
    
    <script>
    export default {
      name: 'ArticleDetails',
      props: {
        title: {
          type: String,
          required: true
        },
        overview: {
          type: String,
          required: true
        },
        slug: {
          type: String,
          required: true
        }
      },
      computed: {
        articleLink() {
          return /article/${this.slug}/;
        }
      }
    };
    </script>
    
    <style scoped>
    .article-details-link {
      text-decoration: none;
      color: inherit;
    }
    
    .article-title {
      margin: 0.5rem 0;
      color: rgba(var(--bs-dark-rgb));
    }
    
    .article-overview {
      font-size: 0.875rem;
      color: #6c757d;
      margin-bottom: 1rem;
    }
    </style>


A podle mě by mohla být chyba v samotném odkazu, protože mi příjde, že se chce přihlásit na stránku /article/${this.slug}/ což tedy vypadá v pořádku, akorát si řikám, že když je v frontend\src\router\index.js nastaveno:

    import { createRouter, createWebHistory } from "vue-router";
    import HomeView from "../views/HomeView.vue";
    import ArticlesListView from "../views/ArticlesListView.vue";
    import ArticleDetailView from "../views/ArticleDetailView.vue";
    
    const routes = [
      {
        path: "/",
        name: "HomeView",
        component: HomeView,
      },
      {
        path: '/articles/all/:page?',
        name: 'ArticlesListView',
        component: ArticlesListView,
      },
      {
        path: "/article/:slug/",
        name: "ArticleDetailView",
        component: ArticleDetailView,
      },
    ];
    
    const router = createRouter({
      history: createWebHistory(process.env.BASE_URL),
      routes,
    });
    
    export default router;


Nemělo by se toto volání stránky provádět jinak?

## ChatGPT:

Vzhledem k tomu, že se stránka s detaily článku zobrazuje správně s textem "AAAAAAAAAAAAAAAAAAAAAAA", vypadá to, že problém pravděpodobně není v routeru, ale spíše v tom, jak se volá API a jak jsou data zpracovávána ve Vue. Pojďme krok za krokem zkontrolovat a vylepšit stávající nastavení.

### 1. Zkontroluj URL pro API volání

Nejdříve zkontrolujeme, zda je volání API správné. Ujistěme se, že cesta v Axios volání odpovídá cestě ve vašem Django backendu.

### 2. Úprava Vue komponenty pro detail článku

Přidáme více detailů do Vue komponenty pro detail článku a zajistíme, že Axios volání API je správně nastaveno:

`frontend/src/views/ArticleDetailView.vue`:

```vue
<template>
  <div class="article-container" v-if="article">
    <ArticleImage 
      :src="article.main_picture_url" 
      :alt="article.title" 
    />
    <h1>{{ article.title }}</h1>
    <p>{{ article.content }}</p>
  </div>
</template>

<script>
import axios from 'axios';
import ArticleImage from '@/components/ArticleDetailView/ArticleViewImage.vue';

export default {
  name: 'ArticleDetailView',
  components: {
    ArticleImage,
  },
  data() {
    return {
      article: null,
    };
  },
  methods: {
    fetchArticle() {
      const slug = this.$route.params.slug;
      console.log('Fetching article with slug:', slug); // Debugging
      axios.get(`/api/article/${slug}/`)
        .then(response => {
          console.log('Article data received:', response.data); // Debugging
          this.article = response.data;
        })
        .catch(error => {
          console.error('There was an error fetching the article:', error);
        });
    },
  },
  created() {
    this.fetchArticle();
  }
};
</script>

<style scoped>
.article-container {
  padding: 20px;
}
</style>
```

### 3. Kontrola API volání v Django backendu

Přidejme tiskové výstupy do Django pohledu, abychom viděli, zda se API volání skutečně provádí:

`backend/views/article_detail_view.py` (předpokládáme název souboru):

```python
from django.db import models
from rest_framework.views import APIView
from rest_framework.response import Response
from django.db.models import F, Value
from django.db.models.functions import Concat
from django.conf import settings

from ..models.article import Article
from ..serializers.article_detail_serializer import ArticleDetailSerializer

class ArticleDetailView(APIView):
    '''
    API View for retrieving a single article by its slug.
    '''

    def get(self, request, slug, *args, **kwargs):
        '''
        Handle GET requests to retrieve a single article by its slug.
        '''
        print("################ get-slug", slug)

        try:
            article = Article.objects.filter(
                slug=slug
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
                main_picture_url=Concat(
                    Value(settings.MEDIA_URL),
                    F('main_picture'),
                    output_field=models.CharField()
                )
            ).values(
                'id',
                'slug',
                'main_picture_url',
                'published',
                'title',
                'content',
                'category_name',
                'category_slug',
                'author_name',
                'author_slug',
                'author_profile_picture_url'
            ).first()

            if not article:
                print("########### Article not found for slug:", slug)
                return Response({"detail": "Article not found"}, status=404)

            print("########### Article: ", article)
            serializer = ArticleDetailSerializer(article)
            return Response(serializer.data)

        except Article.DoesNotExist:
            print("########### Article.DoesNotExist exception for slug:", slug)
            return Response({"detail": "Article not found"}, status=404)
```

### 4. Kontrola konzole v prohlížeči

Zkontroluj konzoli v prohlížeči (F12), jestli se tam objevují nějaké chybové hlášky ohledně API volání nebo jiných problémů.

### 5. Kontrola adres URL

Ujisti se, že URL pro API volání v Axios odpovídá nastavení vašeho Django backendu. Pokud používáš proxy nebo máš API na jiné adrese, uprav volání Axiosu podle toho:

```javascript
axios.get(`http://localhost:8000/api/article/${slug}/`)
```

### 6. Zkontroluj, zda Vue Router správně předává parametry

Ujisti se, že Vue Router správně předává slug jako parametr do `ArticleDetailView`.

Pokud stále nemůžeš najít chybu, dej mi vědět, jaké chyby (pokud nějaké) se objeví v konzoli prohlížeče nebo terminálu, a můžeme pokračovat v hledání problému.

## Dotaz:

Tak zatím bez úspěchu. Zkusíme jít naprosto pomalu a krok po kroku.
První - kontrola volání.

Na stránce se všemi články je v kartě pro článek část body (frontend\src\components\ArticlesListView\ArticleCard\ArticleCardBody.vue)

Která má v sobě zahrnut router-link pro volání stránky:

    <template>
      <router-link :to="{ name: 'ArticleDetailView', params: { slug: slug } }" class="article-details-link">
        <div class="article-details">
          <h4 class="article-title">{{ title }}</h4>
          <p class="article-overview">{{ overview }}</p>
        </div>
      </router-link>
    </template>
    
    <script>
    export default {
      name: 'ArticleDetails',
      props: {
        title: {
          type: String,
          required: true
        },
        overview: {
          type: String,
          required: true
        },
        slug: {
          type: String,
          required: true
        }
      }
    };
    </script>
    
    <style scoped>
    .article-details-link {
      text-decoration: none;
      color: inherit;
    }
    
    .article-title {
      margin: 0.5rem 0;
      color: rgba(var(--bs-dark-rgb));
    }
    
    .article-overview {
      font-size: 0.875rem;
      color: #6c757d;
      margin-bottom: 1rem;
    }
    </style>


po kliknutí na tento odkaz, se podle mě zavolá frontend\src\router\index.js:

A dohledá se cesta s názvem ArticleDetailView:

    import { createRouter, createWebHistory } from "vue-router";
    import HomeView from "../views/HomeView.vue";
    import ArticlesListView from "../views/ArticlesListView.vue";
    import ArticleDetailView from "../views/ArticleDetailView.vue";
    
    const routes = [
      {
        path: "/",
        name: "HomeView",
        component: HomeView,
      },
      {
        path: '/articles/all/:page?',
        name: 'ArticlesListView',
        component: ArticlesListView,
      },
      {
        path: "/article/:slug/",
        name: "ArticleDetailView",
        component: ArticleDetailView,
      },
    ];
    
    const router = createRouter({
      history: createWebHistory(process.env.BASE_URL),
      routes,
    });
    
    export default router;
    
    
    Ta zavolá tento soubor:
    frontend\src\views\ArticleDetailView.vue
    
    Kde jsem pro dohledání chyb zase zjednodušil zápis jen na to, aby se mi zobrazovala AAA:
    
    <template>
      <div class="my-container">
        <p>AAAAAAAAAAAAAAAAAAAAAAA</p>
    
      </div>
    </template>
    
    <script>
    import axios from 'axios';
    
    export default {
      name: 'ArticleDetailView',
    };
    </script>
    
    <style scoped>
    </style>

Takže až sem to funguje, protože se mi skutečně objeví prázdná stránka s AAA.

Nyní bych od tebe chtěl zapsat sem kod, který udělá pouze to, že zavolá Django přes localhost8000/article/(+slug)/
A po obdržení dat je vytiskne do konzole.

## ChatGPT:

Rozumím. Začněme tím, že přidáme kód do komponenty `ArticleDetailView.vue`, který provede volání API na Django backend a vytiskne obdržená data do konzole. 

Postup bude následující:
1. Získáme slug z URL pomocí Vue Routeru.
2. Provedeme volání API na Django backend.
3. Vytiskneme obdržená data do konzole.

Aktualizovaný soubor `frontend/src/views/ArticleDetailView.vue` bude vypadat takto:

```vue
<template>
  <div class="my-container">
    <p>AAAAAAAAAAAAAAAAAAAAAAA</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ArticleDetailView',
  data() {
    return {
      article: null,
    };
  },
  methods: {
    fetchArticle() {
      const slug = this.$route.params.slug;
      console.log('Fetching article with slug:', slug); // Debugging
      axios.get(`http://localhost:8000/api/article/${slug}/`)
        .then(response => {
          console.log('Article data received:', response.data); // Debugging
          this.article = response.data;
        })
        .catch(error => {
          console.error('There was an error fetching the article:', error);
        });
    },
  },
  created() {
    this.fetchArticle();
  }
};
</script>

<style scoped>
.my-container {
  padding: 20px;
}
</style>
```

**Co děláme:**
1. **`data`:** Přidáváme reaktivní `data` s proměnnou `article`, která bude obsahovat informace o článku.
2. **`fetchArticle` metoda:** Tato metoda získá slug z URL, provede volání API na `http://localhost:8000/api/article/${slug}/` a vytiskne obdržená data do konzole.
3. **`created` hook:** Tento hook se zavolá při vytvoření komponenty a provede volání `fetchArticle` pro získání dat.

### Důležité:

- Ujisti se, že Django server běží na `http://localhost:8000`.
- Ujisti se, že API endpoint pro získání detailu článku je dostupný na `http://localhost:8000/api/article/<slug>/`.

### Kontrola API endpointu v Django

Pokud ještě nemáš endpoint nakonfigurovaný, přidej jej do `urls.py`:

```python
from django.urls import path
from .views.article_detail_view import ArticleDetailView

urlpatterns = [
    path('api/article/<slug:slug>/', ArticleDetailView.as_view(), name='article-detail'),
]
```

Teď by měla být cesta `/api/article/<slug>/` dostupná a připravená pro volání z Vue komponenty.

Po těchto úpravách zkontroluj konzoli v prohlížeči (F12) a podívej se, zda se v ní objeví výstup s obdrženými daty článku. Pokud se v konzoli zobrazí chyby nebo chybové hlášky, dej mi vědět, jaké konkrétní chyby se objevily, a můžeme pokračovat v jejich řešení.

## Dotaz:

Tak už asi vím kde je chyba. Ty počítáš s tím, že pro VUE používám předponu api, což momentálně v django nastavené nemám.
Ale uznávám, že bych to měl asi napravit, protože rozumím tomu, že se mi tím v django oddělí kod pro vue.

Ve svém projektu mám url ve více modulech.

Základní url je zde: main/urls.py

A má tento obsah:

    from django.contrib import admin
    from django.urls import path, include
    from django.conf import settings
    from django.conf.urls.static import static
    
    
    ### Definice URL patternů pro aplikaci
    urlpatterns = [
    
        # URL pro administrátorské rozhraní Django
        path('admin/', admin.site.urls),
    
        # URL pro TinyMCE editor
        path('tinymce/', include('tinymce.urls')),
    
    
        ### Adresy pro domácí stránku
        # URL's pro domácí stránku
        path('', include('homepage.urls.homepage')),
    
        # URL pro editaci domácí stránky
        path('edit/', include('homepage.urls.homepage_edit')),
    
    
        ### Adresy pro stránky s články
        # Adresy pro stránku s více články
        path('articles/', include('articles.urls.articles')),
    
        # Adresy pro stránku s jedním článkem
        path('article/', include('articles.urls.article')),
    
        # Adresy pro vyhledávání v článcích
        path('search/', include('articles.urls.search')),
    
        # Adresy pro vlastní články
        path('my-articles/', include('articles.urls.my_articles')),
    
    
        ### Adresy pro správu uživatelů
        # Adresy pro správu uživatelů
        path('accounts/', include('users.urls.accounts')),
    
        # Adresy pro správu uživatelského účtu
        path('profile/', include('users.urls.profile')),
    
        # Adresy pro nastavení postranního panelu
        path('settings/', include('users.urls.sidebar_settings')),
    
    ]

# Přidání URL patternů pro statické a média soubory, pokud je nastavený DEBUG mód

    if settings.DEBUG:
        urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
        urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)


A mě by zajímalo jak se zde správně definuje předpona api, a to tak aby mi ideálně zůstali i adresy, které používám pro django, jestli to tedy ničemu nevadí.

## ChatGPT:

Rozumím. Přidání předpony `api` k URL endpointům, které používá tvůj Vue frontend, je dobrý způsob, jak oddělit API endpointy od běžných URL pro Django šablony a další stránky. Můžeš zachovat existující URL pro Django šablony a současně přidat nové URL pro API.

### Krok 1: Aktualizace `main/urls.py`

Začneme aktualizací souboru `main/urls.py`, aby zahrnoval předponu `api` pro všechny API endpointy.

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

### Definice URL patternů pro aplikaci
urlpatterns = [

    # URL pro administrátorské rozhraní Django
    path('admin/', admin.site.urls),

    # URL pro TinyMCE editor
    path('tinymce/', include('tinymce.urls')),

    ### Adresy pro domácí stránku
    # URL's pro domácí stránku
    path('', include('homepage.urls.homepage')),

    # URL pro editaci domácí stránky
    path('edit/', include('homepage.urls.homepage_edit')),

    ### Adresy pro stránky s články
    # Adresy pro stránku s více články
    path('articles/', include('articles.urls.articles')),

    # Adresy pro stránku s jedním článkem
    path('article/', include('articles.urls.article')),

    # Adresy pro vyhledávání v článcích
    path('search/', include('articles.urls.search')),

    # Adresy pro vlastní články
    path('my-articles/', include('articles.urls.my_articles')),

    ### Adresy pro správu uživatelů
    # Adresy pro správu uživatelů
    path('accounts/', include('users.urls.accounts')),

    # Adresy pro správu uživatelského účtu
    path('profile/', include('users.urls.profile')),

    # Adresy pro nastavení postranního panelu
    path('settings/', include('users.urls.sidebar_settings')),

    ### API URL's
    path('api/articles/', include('articles.api.urls')),  # URL pro API článků
    # Přidej další API URL's podle potřeby
]

# Přidání URL patternů pro statické a média soubory, pokud je nastavený DEBUG mód
if settings.DEBUG:
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

### Krok 2: Vytvoření API URL's

Vytvoříme nové soubory pro API endpointy. 

1. **Vytvoř adresář pro API URL's: `articles/api/urls.py`**

```python
from django.urls import path
from ..views.article_detail_view import ArticleDetailView
from ..views.article_list_view import ArticleListView

urlpatterns = [
    path('article/<slug:slug>/', ArticleDetailView.as_view(), name='api-article-detail'),
    path('list/', ArticleListView.as_view(), name='api-article-list'),
]
```

### Krok 3: Aktualizace VUE

Ve Vue kódu aktualizuj URL endpoint pro získání detailu článku.

```javascript
import axios from 'axios';

export default {
  name: 'ArticleDetailView',
  data() {
    return {
      article: null,
    };
  },
  methods: {
    fetchArticle() {
      const slug = this.$route.params.slug;
      console.log('Fetching article with slug:', slug); // Debugging
      axios.get(`http://localhost:8000/api/articles/article/${slug}/`)
        .then(response => {
          console.log('Article data received:', response.data); // Debugging
          this.article = response.data;
        })
        .catch(error => {
          console.error('There was an error fetching the article:', error);
        });
    },
  },
  created() {
    this.fetchArticle();
  }
};
```

### Kontrola

1. **Ujisti se, že Django server běží na `http://localhost:8000`.**
2. **Zkontroluj, že Vue aplikace běží správně a endpointy jsou správně nastaveny.**
3. **Testuj volání API a zkontroluj, zda jsou data správně načtena a zobrazena.**

Po těchto úpravách by mělo být API endpointy správně oddělené a Vue aplikace by měla správně komunikovat s Django backendem přes nové `api` endpointy.

## Dotaz:

Tak api opraveno ale chybu to nevyřešilo.

Koukal jsem se na kod pro stránku se všemi články a a příjde mi, že tam je navíc tato část:

  // Metoda, která se spustí při vytvoření komponenty
  created() {
    // Získá parametr stránky z URL (pokud existuje) a zavolá funkci pro načtení článků
    const page = parseInt(this.$route.params.page) || 1;
    console.log("### page", page);
    this.fetchArticles(page);
  },

Která v nově vytvářeném kodu chybí.
Stránka provšechny články mi funguje dobře a dobře načítá a zobrazuje data z django, takže si říkám, jestli to, že se mi na stránce pro detail článku nic neobjeví, nemůže být kuli tomu, že zde chybí tato část?

## ChatGPT:

Dobře, pojďme tedy přidat část kódu pro získání parametru `slug` z URL a volání metody `fetchArticle` při vytvoření komponenty, stejně jako je to u stránky se všemi články. Toto by mělo zajistit, že se data pro detail článku správně načtou.

### Aktualizace `ArticleDetailView.vue`

Zde je aktualizovaný kód pro soubor `frontend/src/views/ArticleDetailView.vue`:

```vue
<template>
  <div class="my-container">
    <p>AAAAAAAAAAAAAAAAAAAAAAA</p>
    <div v-if="article">
      <h1>{{ article.title }}</h1>
      <img :src="article.main_picture_url" :alt="article.title" />
      <p>{{ article.content }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ArticleDetailView',
  data() {
    return {
      article: null,
    };
  },
  methods: {
    fetchArticle() {
      const slug = this.$route.params.slug;
      console.log('Fetching article with slug:', slug); // Debugging
      axios.get(`http://localhost:8000/api/articles/article/${slug}/`)
        .then(response => {
          console.log('Article data received:', response.data); // Debugging
          this.article = response.data;
        })
        .catch(error => {
          console.error('There was an error fetching the article:', error);
        });
    },
  },
  created() {
    this.fetchArticle();
  }
};
</script>

<style scoped>
</style>
```

### Co jsme přidali:
1. **Data Property:** Přidali jsme `article`, které bude obsahovat načtený článek.
2. **Metoda `fetchArticle`:** Tato metoda získá parametr `slug` z URL a použije ho k načtení detailu článku pomocí axios.
3. **`created` Hook:** Při vytvoření komponenty zavoláme metodu `fetchArticle` pro načtení dat článku.
4. **Template:** Přidali jsme podmínku `v-if` pro zobrazení obsahu článku, pokud je načten.

### Kontrola

1. **Ujisti se, že Django server běží na `http://localhost:8000`.**
2. **Zkontroluj, že Vue aplikace běží správně.**
3. **Otevři stránku detailu článku a sleduj konzoli pro výpisy z `console.log`, abys viděl, zda se data správně načítají a zpracovávají.**

Pokud i po těchto úpravách stále nevidíš data, zkontroluj síťové požadavky v konzoli prohlížeče (Network tab), zda se API požadavek odesílá správně a zda se vrací očekávaná data.

## Dotaz:

AHojky :-)
Dělám projekt ve VUE a nyní jsem přejmenovával nějaké komponenty a cesty a dostal jsem tuto chybu:

    Uncaught runtime errors:
    ×
    ERROR
    No match for {"name":"articlesListView","params":{"page":"2"}} Error: No match for {"name":"articlesListView","params":{"page":"2"}} at createRouterError (webpack-internal:///./node_modules/vue-router/dist/vue-router.mjs:948:19) at Object.resolve (webpack-internal:///./node_modules/vue-router/dist/vue-router.mjs:1480:27) at resolve (webpack-internal:///./node_modules/vue-router/dist/vue-router.mjs:2987:34) at pushWithRedirect (webpack-internal:///./node_modules/vue-router/dist/vue-router.mjs:3073:46) at Object.push (webpack-internal:///./node_modules/vue-router/dist/vue-router.mjs:3036:12) at Proxy.changePage (webpack-internal:///./node_modules/babel-loader/lib/index.js??clonedRuleSet-40.use[0]!./node_modules/vue-loader/dist/index.js??ruleSet[0].use[0]!./src/views/ArticlesListView.vue?vue&type=script&lang=js:65:20) at callWithErrorHandling (webpack-internal:///./node_modules/@vue/runtime-core/dist/runtime-core.esm-bundler.js:358:19) at callWithAsyncErrorHandling (webpack-internal:///./node_modules/@vue/runtime-core/dist/runtime-core.esm-bundler.js:365:17) at emit (webpack-internal:///./node_modules/@vue/runtime-core/dist/runtime-core.esm-bundler.js:5608:5) at Proxy.changePage (webpack-internal:///./node_modules/babel-loader/lib/index.js??clonedRuleSet-40.use[0]!./node_modules/vue-loader/dist/index.js??ruleSet[0].use[0]!./src/components/ArticlesListView/Pagination.vue?vue&type=script&lang=js:17:14)

Nedokážeš z ní vyčíst, co mi ji působí?

## Claude:

Ahoj! Chápu, že máte problém s Vue aplikací po přejmenování komponent a cest. Pojďme se podívat na chybovou zprávu a zkusit identifikovat problém:

Chyba naznačuje, že router nemůže najít shodu pro cestu s názvem "articlesListView" a parametrem "page" nastaveným na "2". To může být způsobeno několika důvody:

1. Přejmenování komponenty: Pokud jste přejmenovali komponentu ArticlesListView, ujistěte se, že jste aktualizovali všechny odkazy na tuto komponentu v celé aplikaci, včetně konfigurace routeru.

2. Změna v konfiguraci routeru: Zkontrolujte soubor s konfigurací routeru (obvykle router/index.js nebo podobný) a ujistěte se, že cesta "articlesListView" je správně definována a že odpovídá novému názvu komponenty.

3. Nekonzistence v názvech: Ujistěte se, že název "articlesListView" je konzistentní v celé aplikaci - v router konfiguraci, v komponentách a všude, kde se na tuto cestu odkazujete.

4. Chybějící nebo špatně definovaný parametr: Zkontrolujte, zda je parametr "page" správně definován v konfiguraci routeru pro cestu "articlesListView".

5. Chyba v komponentě Pagination: Chyba se objevuje v komponentě Pagination při volání metody changePage. Zkontrolujte implementaci této metody a ujistěte se, že správně pracuje s routerem.

Doporučuji projít tyto body a zkontrolovat příslušné části kódu. Pokud byste potřebovali další pomoc, můžete poskytnout relevantní části kódu (např. konfiguraci routeru, komponentu ArticlesListView a Pagination) a já se pokusím problém blíže identifikovat.

## Dotaz:

OK, tady je kod pro frontend\src\router\index.js:

    import { createRouter, createWebHistory } from "vue-router";
    import HomeView from "../views/HomeView.vue";
    import ArticlesListView from "../views/ArticlesListView.vue";
    import ArticleDetailView from "../views/ArticleDetailView.vue";
    
    const routes = [
      {
        path: "/",
        name: "homeView",
        component: HomeView,
      },
      {
        path: '/articles/all/:page?',
        name: 'ArticlesListView',
        component: ArticlesListView,
      },
      {
        path: "/article/<slug:slug>/",
        name: "ArticleDetailView",
        component: ArticleDetailView,
      },
    ];
    
    const router = createRouter({
      history: createWebHistory(process.env.BASE_URL),
      routes,
    });
    
    export default router;

Tady je kod pro frontend\src\views\ArticlesListView.vue:

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
    // Importuje instanci Axios pro HTTP požadavky
    import api from '../axios';
    // Importuje komponenty, které se použijí v této stránce
    import ArticleCard from '@/components/ArticlesListView/ArticleCard.vue';
    import Pagination from '@/components/ArticlesListView/Pagination.vue';
    
    export default {
      // Název komponenty
      name: 'ArticlesListView',
      // Komponenty, které budou použity v této komponentě
      components: {
        ArticleCard,
        Pagination
      },
      // Data komponenty, která se uloží do jejího stavu
      data() {
        return {
          articles: [], // Pole pro uložení článků
          currentPage: 1, // Aktuální stránka pro stránkování
          totalPages: 1 // Celkový počet stránek pro stránkování
        };
      },
      // Metoda, která se spustí při vytvoření komponenty
      created() {
        // Získá parametr stránky z URL (pokud existuje) a zavolá funkci pro načtení článků
        const page = parseInt(this.$route.params.page) || 1;
        console.log("### page", page);
        this.fetchArticles(page);
      },
      methods: {
        // Asynchronní metoda pro načtení článků z API
        async fetchArticles(page) {
          try {
            // Odeslání GET požadavku na server pro získání článků pro zvolenou stránku
            const response = await api.get(articles/all/?page=${page});
            console.log("### response", response);
    
            // Uložení článků do stavu komponenty
            this.articles = response.data.results;
            console.log("### this.articles", this.articles);
    
            // Aktualizace aktuální stránky
            this.currentPage = page;
            console.log("### this.currentPage", this.currentPage);
    
            // Výpočet celkového počtu stránek
            this.totalPages = Math.ceil(response.data.count / response.data.page_size);
            console.log("### this.totalPages", this.totalPages);
    
          } catch (error) {
            // Zpracování chyb v případě, že požadavek selže
            console.error('Error fetching articles:', error);
          }
        },
        // Metoda pro změnu stránky
        changePage(page) {
          // Aktualizace URL pro změnu stránky a opětovné načtení článků
          this.$router.push({ name: 'articlesListView', params: { page: page } });
          this.fetchArticles(page);
        }
      },
      watch: {
        // Sledování změny parametru stránky v URL a opětovné načtení článků při změně
        '$route.params.page'(newPage) {
          const page = parseInt(newPage) || 1;
          this.fetchArticles(page);
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

Tady je kod pro frontend\src\components\ArticlesListView\Pagination.vue:

    <template>
      <div class="pagination">
        <!-- Tlačítko pro první stránku -->
        <button 
          class="pagination-item"
          @click="changePage(1)" 
          :disabled="currentPage === 1"
        >
          <i class="fa fa-angle-left"></i>
        </button>
    
        <!-- Tlačítko pro předchozí stránku, skryté pokud je aktuální stránka první -->
        <button 
          class="pagination-item"
          @click="changePage(currentPage - 1)" 
          :disabled="currentPage <= 1"
          v-if="currentPage > 1"
        >
          {{ currentPage - 1 }}
        </button>
    
        <!-- Zobrazení aktuální stránky, ne tlačítko -->
        <span class="current-page">
          {{ currentPage }}
        </span>
    
        <!-- Tlačítko pro následující stránku, skryté pokud je aktuální stránka poslední -->
        <button 
          class="pagination-item"
          @click="changePage(currentPage + 1)" 
          :disabled="currentPage >= totalPages"
          v-if="currentPage < totalPages"
        >
          {{ currentPage + 1 }}
        </button>
    
        <!-- Tlačítko pro poslední stránku -->
        <button 
          class="pagination-item"
          @click="changePage(totalPages)" 
          :disabled="currentPage === totalPages"
        >
          <i class="fa fa-angle-right"></i>
        </button>
      </div>
    </template>
    
    <script>
    export default {
      name: 'Pagination',
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
      gap: 5px;
      margin-top: 50px;
      margin-bottom: 50px;
    }
    .pagination-item, .current-page {
      width: 40px;
      height: 40px;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 50%;
      border: none;
      background: none;
      cursor: pointer;
      transition: background-color 0.3s;
    }
    .pagination-item:hover, .current-page {
      background-color: #e9ecef; 
    }
    
    </style>

Vydíš v nich něco, co by to mohlo působit?

## Claude:

Díky za poskytnutí kódu. Po jeho prozkoumání jsem našel pravděpodobný zdroj problému. Chyba je v názvu routy v souboru ArticlesListView.vue. Pojďme se na to podívat blíže:

1. V souboru router/index.js máte definovanou routu takto:

```javascript
{
  path: '/articles/all/:page?',
  name: 'ArticlesListView',
  component: ArticlesListView,
},
```

2. Ale v souboru ArticlesListView.vue, v metodě changePage(), se snažíte navigovat na routu s názvem 'articlesListView' (s malým 'a'):

```javascript
changePage(page) {
  this.$router.push({ name: 'articlesListView', params: { page: page } });
  this.fetchArticles(page);
}
```

Toto je pravděpodobně příčina chyby. Názvy rout jsou case-sensitive (rozlišují velká a malá písmena), takže 'ArticlesListView' a 'articlesListView' jsou považovány za různé routy.

Pro opravu tohoto problému máte dvě možnosti:

1. Změňte název routy v router/index.js na 'articlesListView':

```javascript
{
  path: '/articles/all/:page?',
  name: 'articlesListView',
  component: ArticlesListView,
},
```

2. Nebo změňte název routy v metodě changePage() v ArticlesListView.vue na 'ArticlesListView':

```javascript
changePage(page) {
  this.$router.push({ name: 'ArticlesListView', params: { page: page } });
  this.fetchArticles(page);
}
```

Doporučuji použít druhou možnost, protože je obecně dobrou praxí používat PascalCase pro názvy komponent a jejich odpovídající routy.

Po provedení této změny by chyba měla zmizet. Pokud by problém přetrvával, dejte mi vědět a můžeme to dále prozkoumat.

## Dotaz:

Když mám ve VUE v souboru frontend\src\router\index.js takto definovanou cestu:

      {
        path: "/article/:slug/",
        name: "ArticleDetailView",
        component: ArticleDetailView,
      },

Jak nyní správně vytvořit kod pro router-link, který by ji volal?

## Claude:

Aby se správně vytvořil router-link pro cestu definovanou ve vašem router souboru, můžete použít následující syntaxi:

```vue
<router-link :to="{ name: 'ArticleDetailView', params: { slug: articleSlug } }">
  Zobrazit článek
</router-link>
```

Zde je několik důležitých bodů:

1. Používáme `:to` místo `to`, protože chceme vyhodnotit JavaScript výraz.

2. Uvnitř objektu `:to` používáme `name` pro odkaz na jméno cesty, které jste definovali v routeru.

3. V `params` objektu definujeme parametr `slug`, který odpovídá `:slug` v definici cesty. Hodnota `articleSlug` by měla být proměnná nebo vlastnost obsahující skutečnou hodnotu slugu pro daný článek.

Příklad použití v komponentě:

```vue
<template>
  <div>
    <router-link 
      v-for="article in articles" 
      :key="article.id"
      :to="{ name: 'ArticleDetailView', params: { slug: article.slug } }"
    >
      {{ article.title }}
    </router-link>
  </div>
</template>

<script>
export default {
  data() {
    return {
      articles: [
        { id: 1, title: 'První článek', slug: 'prvni-clanek' },
        { id: 2, title: 'Druhý článek', slug: 'druhy-clanek' },
        // ...
      ]
    }
  }
}
</script>
```

V tomto příkladu vytváříme router-link pro každý článek v seznamu, přičemž používáme `article.slug` jako hodnotu pro parametr `slug` v URL.

Pokud byste chtěli vytvořit odkaz programově (například v metodě nebo computed property), můžete použít `this.$router.push()`:

```javascript
methods: {
  goToArticle(slug) {
    this.$router.push({ name: 'ArticleDetailView', params: { slug: slug } });
  }
}
```

Tímto způsobem vytvoříte správný odkaz na vámi definovanou cestu v routeru.

## Dotaz:

Toto je kod pro vue, který mi zobrazuje všechny články:

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
    // Importuje instanci Axios pro HTTP požadavky
    import api from '../axios';
    // Importuje komponenty, které se použijí v této stránce
    import ArticleCard from '@/components/ArticlesListView/ArticleCard.vue';
    import Pagination from '@/components/ArticlesListView/Pagination.vue';
    
    export default {
      // Název komponenty
      name: 'ArticlesListView',
      // Komponenty, které budou použity v této komponentě
      components: {
        ArticleCard,
        Pagination
      },
      // Data komponenty, která se uloží do jejího stavu
      data() {
        return {
          articles: [], // Pole pro uložení článků
          currentPage: 1, // Aktuální stránka pro stránkování
          totalPages: 1 // Celkový počet stránek pro stránkování
        };
      },
      // Metoda, která se spustí při vytvoření komponenty
      created() {
        // Získá parametr stránky z URL (pokud existuje) a zavolá funkci pro načtení článků
        const page = parseInt(this.$route.params.page) || 1;
        console.log("### page", page);
        this.fetchArticles(page);
      },
      methods: {
        // Asynchronní metoda pro načtení článků z API
        async fetchArticles(page) {
          try {
            // Odeslání GET požadavku na server pro získání článků pro zvolenou stránku
            const response = await api.get(api/articles/all/?page=${page});
            console.log("### response", response);
    
            // Uložení článků do stavu komponenty
            this.articles = response.data.results;
            console.log("### this.articles", this.articles);
    
            // Aktualizace aktuální stránky
            this.currentPage = page;
            console.log("### this.currentPage", this.currentPage);
    
            // Výpočet celkového počtu stránek
            this.totalPages = Math.ceil(response.data.count / response.data.page_size);
            console.log("### this.totalPages", this.totalPages);
    
          } catch (error) {
            // Zpracování chyb v případě, že požadavek selže
            console.error('Error fetching articles:', error);
          }
        },
        // Metoda pro změnu stránky
        changePage(page) {
          // Aktualizace URL pro změnu stránky a opětovné načtení článků
          this.$router.push({ name: 'ArticlesListView', params: { page: page } });
          this.fetchArticles(page);
        }
      },
      watch: {
        // Sledování změny parametru stránky v URL a opětovné načtení článků při změně
        '$route.params.page'(newPage) {
          const page = parseInt(newPage) || 1;
          this.fetchArticles(page);
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

A já nyní vytvářím kod pro zobrazení aktuálního článku a zatím mám jen vytvořenou stránku bez vnitřní logiky:

    <template>
      <div class="my-container">
        <p>AAAAAAAAAAAAAAAAAAAAAAA</p>
    
      </div>
    </template>
    
    <script>
    import axios from 'axios';
    
    export default {
      name: 'ArticleDetailView',
    };
    </script>
    
    <style scoped>
    </style>

mohl by jsi mi na základě předchozího kodu, napsat kod pro tuto stránku, který by získal data z api/article/slug/ (slug je možné získat z adresy vue stránky)

A po té mi jen na stránce zobrazil název článku {{ article.title }}

## Claude:

Jistě, rád vám pomůžu vytvořit kód pro zobrazení detailu článku. Zde je upravená verze vaší komponenty ArticleDetailView, která získá data z API a zobrazí název článku:

```vue
<template>
  <div class="my-container">
    <h1 v-if="article">{{ article.title }}</h1>
    <p v-else>Načítání článku...</p>
  </div>
</template>

<script>
import api from '../axios';

export default {
  name: 'ArticleDetailView',
  data() {
    return {
      article: null,
    };
  },
  created() {
    this.fetchArticle();
  },
  methods: {
    async fetchArticle() {
      try {
        const slug = this.$route.params.slug;
        const response = await api.get(`api/article/${slug}/`);
        this.article = response.data;
      } catch (error) {
        console.error('Chyba při načítání článku:', error);
      }
    },
  },
  watch: {
    '$route.params.slug'() {
      this.fetchArticle();
    },
  },
};
</script>

<style scoped>
.my-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}
</style>
```

Vysvětlení kódu:

1. V `<template>` zobrazujeme název článku, pokud je článek načten. Pokud se článek ještě načítá, zobrazí se zpráva "Načítání článku...".

2. V `data()` definujeme `article` jako null. Tato proměnná bude obsahovat data článku po jejich načtení.

3. Metoda `fetchArticle()` získává slug z URL pomocí `this.$route.params.slug` a pak používá tento slug k získání dat článku z API.

4. V `created()` hook voláme `fetchArticle()`, aby se článek načetl při vytvoření komponenty.

5. Přidali jsme `watch` na `$route.params.slug`, který zajistí, že se článek znovu načte, pokud se změní slug v URL. To je užitečné, pokud by uživatel přecházel mezi různými články bez obnovení stránky.

6. Používáme `api` místo `axios` přímo, předpokládaje, že máte nastavenu instanci axios podobně jako v předchozím příkladu.

Tento kód předpokládá, že máte správně nastavené routy ve vašem Vue Router. Měli byste mít routu podobnou této:

```javascript
{
  path: '/article/:slug',
  name: 'ArticleDetailView',
  component: ArticleDetailView
}
```

Pokud byste chtěli přidat více informací o článku nebo stylovat stránku, můžete jednoduše rozšířit template a přidat další styly. Například:

```vue
<template>
  <div class="my-container" v-if="article">
    <h1 class="article-title">{{ article.title }}</h1>
    <p class="article-date">Publikováno: {{ article.published_date }}</p>
    <div class="article-content" v-html="article.content"></div>
  </div>
  <div v-else class="loading">Načítání článku...</div>
</template>
```

Nezapomeňte upravit styly podle potřeby a ošetřit případné chyby při načítání dat.

## Dotaz:

Tak namísto názvu mám text: Načítání článku...

A v konzoli tento oznam:

    :8000/api/article/historie-vesmiru/:1 
            
            
           Failed to load resource: the server responded with a status of 500 (Internal Server Error)
    index.js??clonedRuleSet-40.use[0]!./node_modules/vue-loader/dist/index.js??ruleSet[0].use[0]!./src/views/ArticleDetailView.vue?vue&type=script&lang=js:24 Chyba při načítání článku: AxiosError

Co se dá z toho oznamu vyčíst?

Z tohoto oznámení můžeme vyčíst několik důležitých informací:

1. Chyba 500 (Internal Server Error):
   Server vrací chybu 500, což znamená, že došlo k nějakému internímu problému na straně serveru při zpracování požadavku.

2. URL požadavku:
   Požadavek byl odeslán na URL "/api/article/historie-vesmiru/". Zdá se, že "historie-vesmiru" je slug článku, který se snažíte načíst.

3. AxiosError:
   Toto naznačuje, že Axios (HTTP klient, který používáte pro API požadavky) zachytil chybu při pokusu o provedení požadavku.

Možné příčiny problému:

1. Chyba na backendu: Nejpravděpodobnější příčinou je nějaký problém na straně serveru. Server může mít problém s zpracováním požadavku pro tento konkrétní článek.

2. Nesprávná URL: Ověřte, zda je formát URL správný. Mělo by to být `api/article/<slug>/`, ale ujistěte se, že toto odpovídá tomu, co backend očekává.

3. Autentizace: Pokud článek vyžaduje autentizaci pro přístup, ujistěte se, že posíláte správné autentizační údaje s požadavkem.

4. CORS problémy: Pokud frontend a backend běží na různých doménách nebo portech, může to být problém s CORS (Cross-Origin Resource Sharing).

Co můžete udělat:

1. Zkontrolujte logy na backendu. Chyba 500 by měla zanechat nějaké informace v serverových lozích.

2. Ověřte, zda článek s tímto slugem skutečně existuje v databázi.

3. Zkuste načíst jiný článek, abyste zjistili, zda je problém specifický pro tento konkrétní článek nebo zda se týká všech článků.

4. Použijte nástroje pro vývojáře v prohlížeči (Network tab) pro podrobnější informace o požadavku a odpovědi.

5. Zkontrolujte, zda je backend správně nakonfigurován pro zpracování požadavků na jednotlivé články.

6. Ověřte, zda používáte správnou base URL pro vaše API požadavky.

Pokud byste mohli poskytnout více informací o backendu (např. kód pro zpracování požadavku na detail článku) nebo o konfiguraci Axios ve vašem Vue projektu, mohl bych vám poskytnout konkrétnější radu.

## Dotaz:

Takže to podle výpisů v konzoli a v terminálu vypadá, že už jsem se provolal až do souboru na serveru django, který maá na starosti vypracování kodu pohledu articles/views/article_detail_api.py:

    from django.db import models
    from rest_framework.views import APIView
    from rest_framework.response import Response
    from django.db.models import F, Value
    from django.db.models.functions import Concat
    from django.conf import settings
    from ..models.article import Article
    from ..serializers.article_detail_serializer import ArticleDetailSerializer
    class ArticleDetailView(APIView):
        '''
        API View for retrieving a single article by its slug.
        '''
        def get(self, request, slug, *args, **kwargs):
            '''
            Handle GET requests to retrieve a single article by its slug.
            '''
            print("################ get-slug", slug)
            try:
                article = Article.objects.filter(
                    slug=slug
                ).select_related(
                    'category',
                    'author'
                ).annotate(
                    category_name=F('categoryname'),
                    category_slug=F('categoryslug'),
                    author_name=F('authorname'),
                    author_slug=F('authorslug'),
                    author_profile_picture_url=Concat(
                        Value(settings.MEDIA_URL),
                        F('author__profile_picture_thumbnail'),
                        output_field=models.CharField()
                    ),
                    main_picture_url=Concat(
                        Value(settings.MEDIA_URL),
                        F('main_picture'),
                        output_field=models.CharField()
                    )
                ).values(
                    'id',
                    'slug',
                    'main_picture_url',
                    'published',
                    'title',
                    'content',
                    'category_name',
                    'category_slug',
                    'author_name',
                    'author_slug',
                    'author_profile_picture_url'
                ).first()
                if not article:
                    print("########### Article not found for slug:", slug)
                    return Response({"detail": "Article not found"}, status=404)
                print("########### Article: ", article)
                serializer = ArticleDetailSerializer(article)
                return Response(serializer.data)
            except Article.DoesNotExist:
                print("########### Article.DoesNotExist exception for slug:", slug)
                return Response({"detail": "Article not found"}, status=404)

Ale zde se něco stane a v konzoli mám toto:

    [06/Aug/2024 15:46:11] "GET /api/article/historie-vesmiru/ HTTP/1.1" 500 149361
    ################ get-slug historie-vesmiru
    Internal Server Error: /api/article/historie-vesmiru/
    Traceback (most recent call last):
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\core\handlers\exception.py", line 55, in inner
        response = get_response(request)
                   ^^^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\core\handlers\base.py", line 197, in _get_response
        response = wrapped_callback(request, *callback_args, **callback_kwargs)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\views\decorators\csrf.py", line 65, in _view_wrapper
        return view_func(request, *args, **kwargs)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\views\generic\base.py", line 104, in view
        return self.dispatch(request, *args, **kwargs)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\rest_framework\views.py", line 509, in dispatch
        response = self.handle_exception(exc)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\rest_framework\views.py", line 469, in handle_exception
        self.raise_uncaught_exception(exc)
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\rest_framework\views.py", line 480, in raise_uncaught_exception
        raise exc
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\rest_framework\views.py", line 506, in dispatch
        response = handler(request, *args, **kwargs)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\Dropbox\__DjangoBlogWeb\main\articles\views\article_detail_api.py", line 28, in get
        ).annotate(
          ^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\db\models\query.py", line 1630, in annotate
        return self._annotate(args, kwargs, select=True)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\db\models\query.py", line 1680, in _annotate
        clone.query.add_annotation(
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\db\models\sql\query.py", line 1198, in add_annotation
        annotation = annotation.resolve_expression(self, allow_joins=True, reuse=None)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\db\models\expressions.py", line 975, in resolve_expression
        c.source_expressions[pos] = arg.resolve_expression(
                                    ^^^^^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\db\models\expressions.py", line 975, in resolve_expression
        c.source_expressions[pos] = arg.resolve_expression(
                                    ^^^^^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\db\models\expressions.py", line 854, in resolve_expression
        return query.resolve_ref(self.name, allow_joins, reuse, summarize)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\db\models\sql\query.py", line 2014, in resolve_ref
        join_info = self.setup_joins(
                    ^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\db\models\sql\query.py", line 1867, in setup_joins
        path, final_field, targets, rest = self.names_to_path(
                                           ^^^^^^^^^^^^^^^^^^^
      File "C:\Users\Sudip2708\__DjangoBlogWeb\Lib\site-packages\django\db\models\sql\query.py", line 1772, in names_to_path
        raise FieldError(
    django.core.exceptions.FieldError: Cannot resolve keyword 'main_picture' into field. Choices are: author, author_id, author_name, author_profile_picture_url, author_slug, category, category_id, category_name, category_slug, comments, content, created, featured_article_1, featured_article_2, featured_article_3, gallery_article_1, gallery_article_2, gallery_article_3, gallery_article_4, id, latest_article_1, latest_article_2, latest_article_3, main_picture_for_article, main_picture_max_size, main_picture_preview, main_picture_thumbnail, next, next_article, next_article_id, overview, previous, previous_article, previous_article_id, published, slug, status, tagged_items, tags, title, updated, view_count
    [06/Aug/2024 15:46:12] "GET /api/article/historie-vesmiru/ HTTP/1.1" 500 149242

Dokážeš z toho něco vyčíst?

## Claude:

Ano, z poskytnutého výpisu chyby můžeme vyčíst následující:

1. Chyba se vyskytuje v souboru `article_detail_api.py` na řádku 28, v metodě `get`.

2. Konkrétní chyba je `django.core.exceptions.FieldError`, která říká:
   "Cannot resolve keyword 'main_picture' into field."

3. Chyba se objevuje při pokusu o anotaci querysetu.

4. Django poskytuje seznam dostupných polí, které můžete použít, a 'main_picture' mezi nimi není.

Hlavní problém je v této části kódu:

```python
main_picture_url=Concat(
    Value(settings.MEDIA_URL),
    F('main_picture'),
    output_field=models.CharField()
)
```

Zdá se, že pole 'main_picture' neexistuje v modelu Article. Místo toho existují podobná pole:

- main_picture_for_article
- main_picture_max_size
- main_picture_preview
- main_picture_thumbnail

Řešení:
Upravte váš kód tak, aby používal správné jméno pole. Například, pokud chcete použít 'main_picture_for_article', změňte kód takto:

```python
main_picture_url=Concat(
    Value(settings.MEDIA_URL),
    F('main_picture_for_article'),
    output_field=models.CharField()
)
```

Také se ujistěte, že v části `.values()` používáte správné jméno pole:

```python
.values(
    'id',
    'slug',
    'main_picture_url',  # Toto by mělo odpovídat názvu, který jste použili v anotaci
    # ... ostatní pole ...
)
```

Po provedení těchto změn by měla chyba zmizet a API by mělo být schopné načíst detail článku. Pokud by problém přetrvával, možná budete muset zkontrolovat definici modelu Article a ujistit se, že všechna pole, která používáte v querysetu, skutečně existují v modelu.

## Dotaz:

Proč na tento kod:

    <template>
      <div class="my-container">
        <p>{{ article }}</p>
        <ArticleViewImage 
        :src="article.main_picture_url" 
        :alt="article.title" 
        />
        <h1 v-if="article">{{ article.title }}</h1>
        <p v-else>Načítání článku...</p>
      </div>
    </template>
    
    <script>
    import api from '../axios';
    import ArticleViewImage from '@/components/ArticleDetailView/ArticleViewImage.vue';
    
    export default {
      name: 'ArticleDetailView',
      components: {
        ArticleViewImage,
      },
      data() {
        return {
          article: null,
        };
      },
      created() {
        this.fetchArticle();
      },
      methods: {
        async fetchArticle() {
          try {
            const slug = this.$route.params.slug;
            console.log("### slug", slug);
    
            const response = await api.get(api/article/${slug}/);
            console.log("### response", response);
    
            this.article = response.data;
          } catch (error) {
            console.error('Chyba při načítání článku:', error);
          }
        },
      },
      watch: {
        '$route.params.slug'() {
          this.fetchArticle();
        },
      },
    };
    </script>
    
    <style scoped>
    .my-container {
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
    }
    </style>

Dostávám tuto chybu:

    main.js:10 [Vue warn]: Unhandled error during execution of render function at <ArticleDetailView onVnodeUnmounted=fn<onVnodeUnmounted> ref=Ref< undefined > > at <RouterView> at <App>
    main.js:10 [Vue warn]: Unhandled error during execution of component update at <RouterView> at <App>
    index.js??clonedRule…113c&scoped=true:20 Uncaught (in promise)
    TypeError: Cannot read properties of null (reading 'main_picture_url') at Proxy.render (index.js??clonedRule…c&scoped=true:20:24) at renderComponentRoot (runtime-core.esm-bundler.js:5708:38) at ReactiveEffect.componentUpdateFn [as fn] (runtime-core.esm-bundler.js:4567:46) at ReactiveEffect.run (reactivity.esm-bundler.js:238:19) at instance.update (runtime-core.esm-bundler.js:4680:16) at setupRenderEffect (runtime-core.esm-bundler.js:4690:5) at mountComponent (runtime-core.esm-bundler.js:4484:7) at processComponent (runtime-core.esm-bundler.js:4450:9) at patch (runtime-core.esm-bundler.js:4125:11) at ReactiveEffect.componentUpdateFn [as fn] (runtime-core.esm-bundler.js:4649:9)

## Claude:

Tato chyba vzniká, protože se snažíte přistupovat k vlastnostem `article` (konkrétně `article.main_picture_url` a `article.title`) ještě předtím, než jsou data načtena. Když je komponenta poprvé vykreslena, `article` je `null`, ale šablona se snaží přistoupit k jeho vlastnostem.

Zde je několik způsobů, jak tento problém vyřešit:

1. Použití v-if pro celý obsah závislý na `article`:

```vue
<template>
  <div class="my-container">
    <div v-if="article">
      <p>{{ article }}</p>
      <ArticleViewImage 
        :src="article.main_picture_url" 
        :alt="article.title" 
      />
      <h1>{{ article.title }}</h1>
    </div>
    <p v-else>Načítání článku...</p>
  </div>
</template>
```

2. Použití volitelného zřetězení (optional chaining):

```vue
<template>
  <div class="my-container">
    <p>{{ article }}</p>
    <ArticleViewImage 
      :src="article?.main_picture_url" 
      :alt="article?.title" 
    />
    <h1>{{ article?.title }}</h1>
    <p v-if="!article">Načítání článku...</p>
  </div>
</template>
```

3. Vytvoření computed property pro kontrolu, zda je článek načten:

```vue
<template>
  <div class="my-container">
    <template v-if="isArticleLoaded">
      <p>{{ article }}</p>
      <ArticleViewImage 
        :src="article.main_picture_url" 
        :alt="article.title" 
      />
      <h1>{{ article.title }}</h1>
    </template>
    <p v-else>Načítání článku...</p>
  </div>
</template>

<script>
// ... ostatní kód ...

export default {
  // ... ostatní kód ...

  computed: {
    isArticleLoaded() {
      return this.article !== null;
    }
  },

  // ... ostatní kód ...
};
</script>
```

Osobně bych doporučil první nebo třetí přístup, protože jsou nejčitelnější a nejbezpečnější. Druhý přístup s volitelným zřetězením je také funkční, ale může vést k tichému selhání, pokud se data neočekávaně nenahrajou.

Také bych doporučil přidat indikátor načítání a zpracování chyb:

```vue
<template>
  <div class="my-container">
    <div v-if="isLoading">Načítání článku...</div>
    <div v-else-if="error">Chyba při načítání článku: {{ error }}</div>
    <div v-else-if="article">
      <p>{{ article }}</p>
      <ArticleViewImage 
        :src="article.main_picture_url" 
        :alt="article.title" 
      />
      <h1>{{ article.title }}</h1>
    </div>
    <div v-else>Článek nebyl nalezen.</div>
  </div>
</template>

<script>
// ... ostatní kód ...

export default {
  // ... ostatní kód ...

  data() {
    return {
      article: null,
      isLoading: false,
      error: null,
    };
  },

  methods: {
    async fetchArticle() {
      this.isLoading = true;
      this.error = null;
      try {
        const slug = this.$route.params.slug;
        console.log("### slug", slug);

        const response = await api.get(`api/article/${slug}/`);
        console.log("### response", response);

        this.article = response.data;
      } catch (error) {
        console.error('Chyba při načítání článku:', error);
        this.error = error.message;
      } finally {
        this.isLoading = false;
      }
    },
  },

  // ... ostatní kód ...
};
</script>
```

Toto řešení poskytuje lepší uživatelskou zkušenost tím, že jasně ukazuje stav načítání a případné chyby.



