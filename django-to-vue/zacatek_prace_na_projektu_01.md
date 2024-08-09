# Začátek práce na projektu
[*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#28-7-2024)   

## Obsah:
[• *Seznámení se stromovou strukturou projektu*](#dotaz)  
[• *Analýza aplikace a struktury templates pro Vue*](#dotaz-1)  
[• *První kroky po analýze projektu*](#dotaz-2)  
[• *První komponenty a routování ve Vue*](#dotaz-3)  
[• *Obsah souboru App.vue*](#dotaz-4)  
[• *Řešení chyb ESLint a Prettier ve Vue*](#dotaz-5)  
[• *Význam export default v App.vue*](#dotaz-6)  


## Dotaz:

OK, tak tady už je asi dobré, aby jsi věděl něco více o mém projektu.

Zde je stromová struktura projektu s vysvětlením jednotlivých složek:

    ### Souborová struktura celého projektu 
        
    ###### Main
        main
        ├─ db.sqlite3
        ├─ manage.py
        ├─ requirements.txt
    
    ###### Articles
        ├─ articles
        │  ├─ __init__.py
        │  ├─ admin.py
        │  ├─ apps.py
        │  ├─ forms
        │  │  ├─ __init__.py
        │  │  ├─ article_content_form.py			   / Formulář ArticleContentForm - pro vytvoření a úpravu článku - pro záložku "Content".
        │  │  ├─ article_overview_form.py			   / Formulář ArticleOverviewForm - pro vytvoření a úpravu článku - pro záložku "Overview".
        │  │  ├─ article_settings_form.py			   / Formulář ArticleSettingsForm - pro vytvoření a úpravu článku - pro záložku "Settings".
        │  │  ├─ comment_form.py				   / Formulář ArticleCommentForm - pro přidání komentáře k článku.
        │  │  └─ search_form.py					   / Formulář ArticleSearchForm - pro vyhledávání článků.
        │  ├─ migrations
        │  │  ├─ __init__.py
        │  │  └─ 0001_initial.py
        │  ├─ models
        │  │  ├─ __init__.py
        │  │  ├─ article.py					   / Databázový model Article - pro uchování dat článku.
        │  │  ├─ article_author.py				   / Databázový model ArticleAuthor - pro uchování dat o autorovi článku.
        │  │  ├─ article_category.py				   / Databázový model ArticleCategory - pro uchování dat o kategoriích článku.
        │  │  ├─ article_comment.py				   / Databázový model ArticleComment - pro uchování dat o komentářích článku.
        │  │  ├─ article_view.py				   / Databázový model ArticleView - pro uchování dat o počtu zhlédnutí článku.
        │  │  ├─ article_data
        │  │  │  ├─ __init__.py
        │  │  │	 ├─ main_picture_processing.py			   / Metoda modelu Article - pro úpravu hlavního obrázku článku (ve 4 velikostech).
        │  │  │	 ├─ mixin_dates_and_status.py			   / Mixin modelu Article - pro přidání polí pro datum a stav.
        │  │  │	 ├─ mixin_foreign_key.py			   / Mixin modelu Article - pro přidání polí odkazujících na jiné tabulky.
        │  │  │	 └─ mixin_main_picture.py			   / Mixin modelu Article - pro přidání polí a správu hlavního obrázku.
        │  │  └─ article_author_data
        │  │     ├─ __init__.py
        │  │	 └─ profile_picture_processing.py		   / Metoda modelu ArticleAuthor - pro úpravu profilového obrázku (ve 2 velikostech).
        │  ├─ schema
        │  │  ├─ __init__.py
        │  │  ├─ article_schema.py				   / Definice třídy ArticleSchema - pro indexování článků v systému Whoosh.
        │  │  ├─ data
        │  │  │  ├─ __init__.py
        │  │  │  ├─ delete_article_from_index.py		   / Metoda třídy ArticleSchema - pro odstranění indexu článku.
        │  │  │  ├─ index_article.py				   / Metoda třídy ArticleSchema - pro zaindexování článku.
        │  │  │  ├─ print_indexed_articles.py			   / Metoda třídy ArticleSchema - pro vypsání názvů všech indexovaných článků.
        │  │  │  ├─ rebuild_schema.py				   / Funkce pro odstranění a opětovné vytvoření celého schématu.
        │  │  │  └─ update_field_in_index.py			   / Metoda třídy ArticleSchema - pro aktualizaci specifického pole v indexu.
        │  │  └─ article_index
        │  │     ├─ _MAIN_17.toc
        │  │     ├─ MAIN_f28cj67c6ri29vi8.seg
        │  │     ├─ MAIN_q59f8pk8e3q55cwe.seg
        │  │	 └─ MAIN_WRITELOCK
        │  ├─ signals
        │  │  ├─ __init__.py
        │  │  ├─ article_signals.py				   / Zachycení signálů spojených s modelem Article.
        │  │  ├─ article_author_signals.py			   / Zachycení signálů spojených s modelem ArticleAuthor.
        │  │  ├─ article_handlers
        │  │  │  ├─ __init__.py
        │  │  │  ├─ default_values_handler.py			   / Pre-save handler modelu Article - pro kontrolu výchozích hodnot.
        │  │  │  ├─ delete_article_handler.py			   / Pre-delete/post-delete handler modelu Article - pro smazání článku.
        │  │  │  ├─ delete_unused_tags_handler.py		   / Post-save handler modelu Article - pro kontrolu a případné smazání tagů.
        │  │  │  ├─ main_picture_handler.py			   / Post-save handler modelu Article - pro zpracování hlavního obrázku článku.
        │  │  │  └─ status_update_handler.py			   / Pre-save/post-save handler modelu Article - pro změnu statusu článku.
        │  │  └─ article_author_handlers
        │  │	 ├─ __init__.py
        │  │	 ├─ default_values_handler.py			   / Pre-save handler modelu ArticleAuthor - pro kontrolu výchozích hodnot.
        │  │	 └─ profile_picture_handler.py			   / Post-save handler modelu ArticleAuthor - pro zpracování profilového obrázku.
        │  ├─ tests
        │  │  ├─ __init__.py
        │  │  ├─ test_article_author_model.py			   / Testy modelu ArticleAuthor.
        │  │  └─ test_article_model.py		                   / Testy modelu Article.
        │  ├─ urls
        │  │  ├─ __init__.py
        │  │  ├─ article.py					   / Definice URL adres začínajících s prefixem 'article/'.
        │  │  ├─ articles.py					   / Definice URL adres začínajících s prefixem 'articles/'.
        │  │  ├─ my_articles.py					   / Definice URL adres začínajících s prefixem 'my-articles/'.
        │  │  └─ search.py					   / Definice URL adres začínajících s prefixem 'search/'.
        │  └─ views
        │	  ├─ __init__.py
        │	  ├─ article_create.py				   / Pohled ArticleCreateView - pro vytvoření článku.
        │	  ├─ article_delete.py				   / Pohled ArticleDeleteView - pro smazání článku.
        │	  ├─ article_detail.py				   / Pohled ArticleDetailView - pro zobrazení jednoho článku.
        │	  ├─ article_list.py				   / Pohled ArticleListView - pro zobrazení více článků.
        │	  ├─ article_update.py				   / Pohled ArticleUpdateView - pro úpravu vytvořeného článku.
        │	  ├─ my_articles.py				   / Pohled MyArticlesView - pro zobrazení článků vytvořených přihlášeným uživatelem.
        │	  ├─ search.py					   / Pohled SearchView - pro zpracování zadaných parametrů pro vyhledávání v článcích.
        │	  ├─ search_input.py				   / Pohled SearchInputView - pro zadání parametrů pro vyhledávání v článcích.
        │	  ├─ article_create_data
        │	  │  ├─ __init__.py
        │	  │  ├─ get_context_data.py			   / Metoda pohledu ArticleCreateView - pro vytvoření kontextu.
        │	  │  ├─ get_form_class.py			   / Metoda pohledu ArticleCreateView - pro výběr formuláře dle záložky stránky.
        │	  │  ├─ get_or_create_author.py			   / Funkce pro získání instance autora na základě uživatelského jména.
        │	  │	 └─ get_success_url.py			   / Metoda pohledu ArticleCreateView - pro vytvoření návratové adresy.
        │	  ├─ article_list_data
        │	  │  ├─ __init__.py
        │	  │  ├─ get_context_data.py			   / Metoda pohledu ArticleListView - pro vytvoření kontextu.
        │	  │  ├─ get_queryset.py				   / Metoda pohledu ArticleListView - pro vytvoření seznamu článků zobrazených na stránce.
        │	  │	 └─ get_queryset_data
        │	  │		├─ __init__.py
        │	  │		├─ get_queryset_for_articles.py    / Metoda pohledu ArticleListView - pro vytvoření seznamu článků pro stránku se všemi články.
        │	  │		├─ get_queryset_for_categories.py  / Metoda pohledu ArticleListView - pro vytvoření seznamu článků pro vybranou kategorii.
        │	  │		├─ get_queryset_for_tags.py        / Metoda pohledu ArticleListView - pro vytvoření seznamu článků pro konkrétní tag a pro podobné články.
        │	  │		└─ get_tag_data.py                 / Metoda pohledu ArticleListView - pro vytvoření seznamu článků pro konkrétní tag.
        │	  ├─ common_data
        │	  │	 ├─ __init__.py
        │	  │	 ├─ get_category_data.py		   / Sdílená metoda - pro vytvoření seznamu článků pro konkrétní kategorii.
        │	  │	 ├─ get_paginate_by.py			   / Sdílená metoda - pro určení počtu článků na stránce při stránkování výsledků vyhledávání.
        │	  │	 └─ get_similar_data.py			   / Sdílená metoda - pro vytvoření atributů pro zobrazení podobných článků.
        │	  ├─ my_articles_data
        │	  │	 ├─ __init__.py
        │	  │	 ├─ get_context_data.py			   / Metoda pohledu MyArticlesView - pro vytvoření kontextu.
        │	  │	 └─ get_queryset.py			   / Metoda pohledu MyArticlesView - pro vytvoření seznamu článků zobrazených na stránce.
        │	  └─ search_data
        │		 ├─ __init__.py
        │		 ├─ get_article_ids.py			   / Funkce pro získání ID článků na základě zadaných parametrů hledání.
        │		 ├─ get_context_data.py			   / Metoda pohledu SearchView - pro vytvoření kontextu.
        │		 ├─ get_queryset.py			   / Metoda pohledu SearchView - pro vytvoření seznamu článků zobrazených na stránce.
        │		 ├─ get_search_parameters.py		   / Funkce pro zpracování očištěných dat získaných z formuláře.
        │		 └─ get_article_ids_data
        │			├─ __init__.py
        │			├─ search_in_author.py		   / Funkce pro vytvoření dotazu a popisného textu pro hledání článků podle autora.
        │			├─ search_in_date.py		   / Funkce pro vytvoření dotazu a popisného textu pro hledání článků podle data.
        │			└─ search_in_query.py		   / Funkce pro vytvoření dotazu a popisného textu pro hledání článků na základě zadaného dotazu.
    
    ###### Common data
        ├─ common_data
        │  ├─ __init__.py
        │  ├─ base_view.py					   / Definice základní třídy BaseView, ze které dědí všechny ostatní pohledy.
        │  ├─ get_unique_value.py				   / Funkce pro ověření jedinečnosti dané hodnoty v rámci pole modelu.
        │  ├─ image_processing.py				   / Funkce na zpracování nového obrázku a nahrazení původního.
        │  └─ image_resize_and_crop.py				   / Funkce pro změnu velikosti a oříznutí obrázku.
    
    ###### Homepage
        ├─ homepage
        │  ├─ __init__.py
        │  ├─ admin.py
        │  ├─ apps.py
        │  ├─ forms
        │  │  ├─ __init__.py
        │  │  ├─ divider_section_form.py			   / Formulář DividerSectionForm - pro nastavení Divider sekce domácí stránky.
        │  │  ├─ featured_section_form.py			   / Formulář FeaturedArticlesForm - pro nastavení sekce Featured Articles na domácí stránce.
        │  │  ├─ footer_section_form.py				   / Formulář FooterSettingsForm - pro editaci nastavení patičky.
        │  │  ├─ gallery_section_form.py			   / Formulář GallerySectionForm - pro editaci nastavení Gallery sekce domácí stránky.
        │  │  ├─ hero_section_form.py				   / Formulář HeroSectionForm - pro nastavení Hero sekce domácí stránky.
        │  │  ├─ intro_section_form.py				   / Formulář IntroSectionForm - pro nastavení Intro sekce domácí stránky.
        │  │  ├─ latest_section_form.py				   / Formulář LatestArticlesForm - pro nastavení sekce Nejnovější články na domácí stránce.
        │  │  └─ newsletter_section_form.py			   / Formulář NewsletterSectionForm - pro nastavení sekce Novinek na domácí stránce.
        │  ├─ migrations
        │  │  ├─ __init__.py
        │  │  └─ 0001_initial.py
        │  ├─ models
        │  │  ├─ __init__.py
        │  │  ├─ divider_section.py				   / Databázový model HomePageDividerSection - pro data sekce.
        │  │  ├─ featured_section.py				   / Databázový model HomePageFeaturedArticles - pro data sekce.
        │  │  ├─ footer_section.py				   / Databázový model FooterSettings - pro data sekce.
        │  │  ├─ gallery_section.py				   / Databázový model HomePageGallerySection - pro data sekce.
        │  │  ├─ hero_section.py				   / Databázový model HomePageHeroSection - pro data sekce.
        │  │  ├─ intro_section.py				   / Databázový model HomePageIntroSection - pro data sekce.
        │  │  ├─ latest_section.py				   / Databázový model HomePageLatestArticles - pro data sekce.
        │  │  ├─ newsletter_section.py				   / Databázový model HomePageNewsletterSection - pro data sekce.
        │  │  ├─ newsletter_subscriber.py			   / Databázový model NewsletterSubscriber - pro správu emailů pro zasílání novinek.
        │  │  └─ data
        │  │	 ├─ __init__.py
        │  │	 ├─ footer_section_default.py			   / Výchozí hodnoty pro model FooterSettings - pro patičku stránky.
        │  │	 ├─ save_footer_data.py				   / Funkce pro uložení dat z formuláře do modelu FooterSettings.
        │  │	 └─ singleton_model.py				   / Základní abstraktní třída pro editační modely homepage - pro implementaci Singleton návrhového vzoru.
        │  ├─ tests.py
        │  ├─ urls
        │  │  ├─ __init__.py
        │  │  ├─ homepage.py					   / Definice URL adres - začínajících s prefixem ''.
        │  │  └─ homepage_edit.py				   / Definice URL adres - začínajících s prefixem 'edit/'.
        │  └─ views
        │	  ├─ __init__.py
        │	  ├─ edit_divider_section.py			   / Pohled EditDividerSection - pro správu sekce.
        │	  ├─ edit_featured_section.py			   / Pohled EditFeaturedArticlesSection - pro správu sekce.
        │	  ├─ edit_footer_section.py			   / Pohled EditFooterSection - pro správu sekce.
        │	  ├─ edit_gallery_section.py			   / Pohled EditGallerySection - pro správu sekce.
        │	  ├─ edit_hero_section.py			   / Pohled EditHeroSection - pro správu sekce.
        │	  ├─ edit_intro_section.py			   / Pohled EditIntroSection - pro správu sekce.
        │	  ├─ edit_latest_section.py			   / Pohled EditLatestArticlesSection - pro správu sekce.
        │	  ├─ edit_newsletter_section.py			   / Pohled EditNewsletterSection - pro správu sekce.
        │	  ├─ home_page_view.py				   / Pohled HomePageView - pro zobrazení obsahu domovské stránky.
        │	  └─ home_page_view_edit.py			   / Pohled HomePageEditView - pro úpravu obsahu domovské stránky.
    
    ###### Main
        ├─ main
        │  ├─ __init__.py
        │  ├─ asgi.py
        │  ├─ settings.py
        │  ├─ urls.py
        │  └─ wsgi.py
    
    ###### Media
        ├─ media
        │  └─ images
        │	  ├─ articles
        │	  │	 ├─ no-image.jpg			   / Defaultní obrázek - použit při vytvoření článku, není-li definován vlastní.
        │	  │	 └─ main_picture			   / Složka pro 4 velikostní varianty hlavního obrázku článku:
        │	  │		├─ article-00000001-0150.jpg	   / 150px/150px - miniatura obrázku (použito na homepage).
        │	  │		├─ article-00000001-0440.jpg	   / 440px/330px - náhledová velikost (použito v náhledech článků).
        │	  │		├─ article-00000001-0800.jpg	   / Maximální šířka 800px - velikost pro článek (použito na stránce s článkem).
        │	  │		└─ article-00000001-1920.jpg	   / Maximální šířka 1920px - maximální velikost (použito při zvětšení obrázku na celou stránku).
        │	  ├─ homepage
        │	  │	 └─ default				   / Obrázky z původní šablony použité na stránce homepage.
        │	  │		├─ divider-bg.jpg
        │	  │		├─ featured-pic-1.jpeg
        │	  │		├─ featured-pic-2.jpeg
        │	  │		├─ featured-pic-3.jpeg
        │	  │		├─ gallery-1.jpg
        │	  │		├─ gallery-2.jpg
        │	  │		├─ gallery-3.jpg
        │	  │		├─ gallery-4.jpg
        │	  │		├─ hero.jpg
        │	  │		├─ hero-bg.jpg
        │	  │		├─ small-thumbnail-1.jpg
        │	  │		├─ small-thumbnail-2.jpg
        │	  │		└─ small-thumbnail-3.jpg
        │	  └─ profile_pictures
        │		 ├─ default.jpg				   / Defaultní obrázek pro uživatele bez vlastního profilového obrázku.
        │		 ├─ authors				   / Složka pro 2 velikostní varianty profilového obrázku autora:
        │		 │	├─ author-00000001-0150.jpg	   / 150px/150px - miniatura obrázku (pro běžné použití).
        │		 │	└─ author-00000001-0440.jpg	   / 440px/440px - náhledová velikost (použito na stránce správy účtu autora).
        │		 └─ users				   / Složka pro 2 velikostní varianty profilového obrázku uživatele:
        │			├─ user-00000001-0150.jpg	   / 150px/150px - miniatura obrázku (pro běžné použití).
        │			└─ user-00000001-0440.jpg	   / 440px/440px - náhledová velikost (použito na stránce správy účtu uživatele).
    
    ###### Static
        ├─ static
        │  ├─ css
        │  │  ├─ github.css					   / CSS sloužící k úpravě vzhledu webových stránek, které jsou hostovány na platformě Github.
        │  │  ├─ my_custom.css					   / Vlastní CSS (definováno autorem projektu).
        │  │  ├─ style.default.css				   / Defaultní CSS (obdržené s šablonou stránky).
        │  │  ├─ style.default.min.css				   / Minifikovaná verze defaultního CSS (pro produkční prostředí).
        │  │  └─ style.default.min.css.map			   / Mapovací soubor pro minifikovaný CSS k původnímu nezminifikovanému defaultnímu CSS kódu.
        │  ├─ img
        │  │  ├─ default.jpg					   / Defaultní obrázek pro uživatele bez vlastního profilového obrázku (záloha).
        │  │  ├─ favicon.ico					   / Ikona pro zobrazení v záložce webu.
        │  │  ├─ hide_side_panel[150].jpg			   / Obrázek použitý pro schování bočního panelu.
        │  │  ├─ login_picture.svg				   / Obrázek použitý na stránce pro přihlášení.
        │  │  ├─ no-image.jpg					   / Defaultní obrázek - použit při vytvoření článku, není-li definován vlastní (záloha).
        │  │  └─ signup_picture.jpg				   / Obrázek použitý na stránce pro registraci.
        │  └─ js
        │	  ├─ _tagify_input.js				   / Skript pro našeptávač tagů Tagify - použito na stránce pro vytvoření a úpravu článku.
        │	  ├─ _tinymce_article_settings.js		   / Skript pro nastavení editoru TinyMCE - použito pro obsah článku.
        │	  ├─ _tinymce_mini_settings.js			   / Skript pro nastavení editoru TinyMCE - použito pro editaci některých položek z homepage.
        │	  └─ jquery-3.7.1.min.js			   / Skript pro implementaci jQuery - použito pro zasílání AJAX požadavků.
    
    ###### Šablony (pro svou rozsáhlost jsou umístněné ve vlastním souboru)
        ├─ templates
    [>>> Prohlédnout si souborovou strukturu templates.](05_tree_templates_[cz].md)
    
    ###### Users
        └─ users
           ├─ __init__.py
           ├─ admin.py
           ├─ apps.py
           ├─ tests.py
           ├─ forms
           │  ├─ __init__.py
           │  ├─ author_profile_form.py				   / Formulář AuthorProfileForm - pro správu účtu autora.
           │  └─ user_profile_form.py				   / Formulář UserProfileForm - pro správu uživatelského účtu.
           ├─ migrations
           │  ├─ __init__.py
           │  └─ 0001_initial.py
           ├─ models
           │  ├─ __init__.py
           │  ├─ anonymous_user_middleware.py			   / Třída AnonymousUserMiddleware - middleware pro správu nepřihlášeného uživatele.
           │  ├─ custom_user.py					   / Databázový model CustomUser - pro data uživatele.
           │  ├─ anonymous_user_data
           │  │	 ├─ __init__.py
           │  │	 ├─ anonymous_user_with_settings.py		   / Třída AnonymousUserWithSettings - pro vytvoření instance pro nepřihlášeného uživatele s nastavením pro postranní panely.
           │  │	 └─ mixin_property.py				   / Mixin pro třídu AnonymousUserWithSettings - přidávající třídě property.
           │  ├─ common_data
           │  │	 ├─ __init__.py
           │  │	 ├─ default_setting_values.py			   / Výchozí hodnoty pro nastavení bočních panelů.
           │  │	 ├─ get_sorted_sidebar_panels.py		   / Sdílená metoda - pro vytvoření seznamu setříděných panelů s daty potřebnými pro jejich vykreslení.
           │  │	 ├─ change_bool_value.py			   / Funkce pro změnu boolean hodnot slovníků pro nastavení bočních panelů uživatele.
           │  │	 └─ change_sidebar_order.py			   / Funkce pro změnu pořadí postranních panelů.
           │  └─ custom_user_data
           │	 ├─ __init__.py
           │	 ├─ managers.py					   / Třída CustomUserManager - pro změnu nastavení identifikace uživatele a superuživatele.
           │	 ├─ mixin_settings.py				   / Mixin pro třídu CustomUser - přidávající třídě nastavení postranního panelu.
           │	 ├─ profile_picture_processing.py		   / Metoda modelu ArticleAuthor pro úpravu profilového obrázku (2 velikosti).
           │	 └─ setting_values_check.py			   / Funkce pro kontrolu položek pro nastavení bočních panelů dle defaultních hodnot.
           ├─ signals
           │  ├─ __init__.py
           │  ├─ user_signals.py				   / Zachycení signálů spojených s modelem CustomUser.
           │  └─ user_handlers
           │	 ├─ __init__.py
           │	 ├─ default_values_handler.py			   / Pre-save handler modelu CustomUser - pro kontrolu výchozích hodnot.
           │	 └─ profile_picture_handler.py			   / Post-save handler modelu ArticleAuthor - pro zpracování profilového obrázku.
           ├─ urls
           │  ├─ __init__.py
           │  ├─ accounts.py					   / Definice URL adres - začínajících s prefixem 'accounts/'
           │  ├─ profile.py					   / Definice URL adres - začínajících s prefixem 'profile/'
           │  └─ sidebar_settings.py				   / Definice URL adres - začínajících s prefixem 'settings/'
           └─ views
                ├─ __init__.py
                ├─ profile_update_author.py			   / Pohled AuthorProfileView - pro úpravu a nastavení dat autora.
                ├─ profile_update_user.py			   / Pohled UserProfileView - pro úpravu a nastavení dat uživatele.
                ├─ user_navigation_settings.py			   / Pohled user_navigation_settings - pro nastavení viditelnosti navigačních panelů a postranního panelu.
                ├─ user_sidebar_appearance.py			   / Pohled user_sidebar_appearance - pro změnu boolean hodnot v databázi pro nastavení bočního panelu.
                └─ user_sidebar_movements.py			   / Pohled user_sidebar_movements - pro změnu pořadí bočních panelů.

A zde je stromová struktura pro složku templates:

    ### Souborová struktura šablon projektu
    
        ├─ templates
    
    ###### Základní šablona pro všechny stránky projektu
    
        │  ├─ 0_base
        │  │  ├─ 00__base__.html
        │  │  ├─ 01__head__.html
        │  │  ├─ 02__header__.html
        │  │  ├─ 03__messages__.html
        │  │  ├─ 04__footer__.html
        │  │  ├─ 05__body_scripts__.html
        │  │  ├─ _footer
        │  │  │  ├─ __content__.html
        │  │  │  ├─ __edit_section__.html
        │  │  │  ├─ __end_line_section__.html
        │  │  │  ├─ _content_data
        │  │  │	 │	├─ _address_and_social_.html
        │  │  │	 │	├─ _articles_.html
        │  │  │	 │	└─ _selected_links_.html
        │  │  │	 └─ _edit_data
        │  │  │		├─ _column_1_.html
        │  │  │		├─ _column_2_.html
        │  │  │		├─ _column_3_.html
        │  │  │		└─ _column_4_.html
        │  │  ├─ _head
        │  │  │  ├─ __links__.html
        │  │  │  ├─ __meta__.html
        │  │  │  └─ __scripts__.html
        │  │  └─ _header
        │  │	 ├─ __dropdown_display__.html
        │  │	 ├─ __navigation__.html
        │  │	 ├─ _dropdown_data
        │  │	 │	├─ __dropdown_menu__.html
        │  │	 │	├─ _button_.html
        │  │	 │	├─ _category_dropdown_.html
        │  │	 │	├─ _create_article_and_favorite_.html
        │  │	 │	├─ _homepage_articles_search_.html
        │  │	 │	├─ _profile_and_login_.html
        │  │	 │	├─ _show_sidebar_.html
        │  │	 │	└─ _special_offer_.html
        │  │	 └─ _navigation_data
        │  │		├─ _navigation_items_.html
        │  │		├─ _search_and_language_.html
        │  │		└─ _special_offer_.html
    
    ###### Šablona pro domácí stránku
    
        │  ├─ 1_home
        │  │  ├─ 10__base__.html
        │  │  ├─ 11__hero__.html
        │  │  ├─ 12__intro__.html
        │  │  ├─ 13__featured_articles__.html
        │  │  ├─ 14__divider__.html
        │  │  ├─ 15__latest_articles__.html
        │  │  ├─ 16__newsletter__.html
        │  │  ├─ 17__gallery__.html
        │  │  ├─ _home_data
        │  │  │  ├─ __featured_articles__.html
        │  │  │  ├─ __latest_articles__.html
        │  │  │  ├─ _display_background_image_.html
        │  │  │  ├─ _display_div_text_.html
        │  │  │  ├─ _display_h1_text_.html
        │  │  │  ├─ _display_h2_text_.html
        │  │  │  ├─ _display_link_.html
        │  │  │  ├─ _edit_button_.html
        │  │  │  ├─ _gallery_articles_.html
        │  │  │  ├─ _input_email_field_.html
        │  │  │  ├─ _main_picture_.html
        │  │  │	 └─ _section_display_.html
        │  │  └─ _home_edit
        │  │	 ├─ __divider__.html
        │  │	 ├─ __featured__.html
        │  │	 ├─ __gallery__.html
        │  │	 ├─ __hero__.html
        │  │	 ├─ __intro__.html
        │  │	 ├─ __latest__.html
        │  │	 ├─ __newsletter__.html
        │  │	 └─ _edit_data
        │  │		├─ _edit_article_.html
        │  │		├─ _edit_background_image_.html
        │  │		├─ _edit_html_field_.html
        │  │		├─ _edit_link_.html
        │  │		└─ _submit_button_with_display_checkbox_.html
    
    ###### Šablona pro stránky zobrazující postranní panel
    
        │  ├─ 2_main
        │  │  ├─ 20__base__.html
        │  │  ├─ 21__sidebar__.html
        │  │  ├─ 22__search__.html
        │  │  ├─ 23__profile_update__.html
        │  │  ├─ _search_data
        │  │  │  ├─ _form_.html
        │  │  │	 └─ _form_data
        │  │  │		├─ _autor_input_.html
        │  │  │		├─ _collapse_button_.html
        │  │  │		├─ _date_inputs_.html
        │  │  │		├─ _error_messages_.html
        │  │  │		├─ _check_box_inputs_.html
        │  │  │		├─ _input_field_.html
        │  │  │		└─ _submit_button_md_.html
        │  │  └─ _sidebars
        │  │	 ├─ __category__.html
        │  │	 ├─ __search__.html
        │  │	 ├─ __tags__.html
        │  │	 ├─ __user__.html
        │  │	 └─ _data
        │  │		├─ _author_dropdown_menu_.html
        │  │		├─ _search_options_items_.html
        │  │		├─ _sidebar_movement_buttons_.html
        │  │		└─ _user_dropdown_items_.html
    
    ###### Šablona pro stránky zobrazující více článků
    
        │  ├─ 3_articles
        │  │  ├─ 30__base__.html
        │  │  ├─ 31__page_title__.html
        │  │  ├─ 32__page_navigation__.html
        │  │  ├─ 33__articles__.html
        │  │  ├─ 34__pagination__.html
        │  │  ├─ _articles_data
        │  │  │  ├─ _article_view_setup_.html
        │  │  │	 └─ _view_setup_data
        │  │  │		├─ _author_.html
        │  │  │		├─ _category_.html
        │  │  │		├─ _comment_count_.html
        │  │  │		├─ _main_picture_.html
        │  │  │		├─ _overview_.html
        │  │  │		├─ _published_ago_.html
        │  │  │		├─ _published_date_.html
        │  │  │		├─ _tags_.html
        │  │  │		└─ _title_.html
        │  │  └─ _page_navigation_data
        │  │	 ├─ _categories_.html
        │  │	 ├─ _my_articles_.html
        │  │	 ├─ _search_results_.html
        │  │	 ├─ _tags_.html
        │  │	 ├─ _search_result_data
        │  │	 │	├─ _tab_for_categories_.html
        │  │	 │	├─ _tab_for_results_and_similar_.html
        │  │	 │	├─ _tab_to_show_categories_.html
        │  │	 │	└─ _tab_to_show_results_and_similar_.html
        │  │	 └─ _tags_data
        │  │		├─ _tab_for_categories_.html
        │  │		├─ _tab_for_tag_and_similar_.html
        │  │		├─ _tab_to_show_categories_.html
        │  │		└─ _tab_to_show_tag_and_similar_.html
    
    ###### Šablona pro stránku zobrazující jeden článek
    
        │  ├─ 4_article
        │  │  ├─ 40__base__.html
        │  │  ├─ 41__article__.html
        │  │  ├─ 42__previous_and_next__.html
        │  │  ├─ 43__display_comments__.html
        │  │  ├─ 44__add_comment__.html
        │  │  └─ _article_data
        │  │	 ├─ _content_.html
        │  │	 ├─ _main_picture_.html
        │  │	 ├─ _next_.html
        │  │	 ├─ _overview_.html
        │  │	 ├─ _previous_.html
        │  │	 ├─ _tags_.html
        │  │	 ├─ _title_.html
        │  │	 └─ _update_article_.html
    
    ###### Šablona pro vytvoření a úpravu článku
    
        │  ├─ 5_create_article
        │  │  ├─ 50__base__.html
        │  │  ├─ 51__overview__.html
        │  │  ├─ 52__content__.html
        │  │  ├─ 53__settings__.html
        │  │  ├─ 54__confirm_leave__.html
        │  │  ├─ 55__confirm_delete__.html
        │  │  └─ _data
        │  │	 ├─ _buttons_both_.html
        │  │	 ├─ _input_category_.html
        │  │	 ├─ _input_next_article_.html
        │  │	 ├─ _input_previous_article_.html
        │  │	 ├─ _input_status_.html
        │  │	 ├─ _input_tags_.html
        │  │	 └─ _main_picture_view_.html
    
    ###### Přepsání vzhledu stránek AllAuth pro přihlášení a registraci
    
        │  ├─ account
        │  │  ├─ login.html
        │  │  ├─ signup.html
        │  │  ├─ _login
        │  │  │	 ├─ _email_input_.html
        │  │  │	 ├─ _help_links_.html
        │  │  │	 ├─ _login_with_google_.html
        │  │  │	 ├─ _or_separator_.html
        │  │  │	 ├─ _password_input_.html
        │  │  │	 ├─ _remember_me_.html
        │  │  │	 └─ _submit_button_.html
        │  │  ├─ _shared
        │  │  │  ├─ _base_.html
        │  │  │  ├─ _error_messages_.html
        │  │  │  ├─ _picture_.html
        │  │  │  ├─ _redirect_value_.html
        │  │  │	 └─ _script_for_showing_password_.html
        │  │  └─ _signup
        │  │	 ├─ _email_input_.html
        │  │	 ├─  _help_links_.html
        │  │	 ├─ _password_input1_.html
        │  │	 ├─ _password_input2_.html
        │  │	 ├─ _submit_button_.html
        │  └─ socialaccount	
        │	  └─ login.html

Vem si na prozkoumání těchto dvou struktur klidně i více času a rád bych potom nejprve slyšel slovní rozbor obou struktur, abych věděl, že jsi je pochopil, a poté se budu ptát dál.

## ChatGPT:

Díky za podrobnou strukturu projektu. Je zřejmé, že pracuješ na komplexním projektu s více komponenty a funkcionalitami. Jaké konkrétní otázky nebo problémy máš ohledně této struktury? Co potřebuješ konkrétně vyřešit nebo doladit?

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#28-7-2024)  
## Dotaz:

Nejprve mi prosím popiš mou aplikaci dle toho, co dokážeš ze stromové struktury vyčíst.
A poté mi zhodnoť i strukturu pro templates a zda pro Vue bude struktura podobná.


## ChatGPT:

Samozřejmě! Nejprve se podíváme na popis aplikace podle stromové struktury a poté se zaměříme na strukturu pro Vue.js.

### Popis aplikace podle stromové struktury

Na základě poskytované stromové struktury by mohla být vaše aplikace strukturována následujícím způsobem:

1. **`src/`** - Tento adresář obvykle obsahuje hlavní kód aplikace.

   - **`assets/`** - Zde jsou uloženy statické soubory, jako jsou obrázky, fonty a styly.
   
   - **`components/`** - Tento adresář obsahuje jednotlivé komponenty aplikace. Komponenty jsou malé, znovupoužitelné části uživatelského rozhraní.
   
   - **`views/`** - Tento adresář obsahuje větší komponenty, které často představují různé stránky nebo hlavní části aplikace.
   
   - **`router/`** - Zde se nachází konfigurace pro směrování (routing) aplikace, což umožňuje navigaci mezi různými částmi aplikace.
   
   - **`store/`** - Tento adresář obvykle obsahuje stavovou správu aplikace (např. Vuex pro Vue.js), která spravuje a sdílí stav mezi komponentami.
   
   - **`styles/`** - Zde jsou umístěny soubory se styly (CSS, SCSS apod.), které se aplikují na komponenty.
   
   - **`utils/`** - Tento adresář může obsahovat pomocné funkce, utilitární kódy a jiné nástroje, které jsou používány napříč aplikací.
   
   - **`App.vue`** - Hlavní komponenta aplikace, která obvykle obsahuje základní strukturu a rámec celé aplikace.
   
   - **`main.js`** - Vstupní bod aplikace, kde se obvykle inicializuje Vue instance a připojují se všechny globální konfigurace a pluginy.

2. **`public/`** - Tento adresář obsahuje statické soubory, které jsou přístupné přímo, např. `index.html` soubor, který slouží jako základní HTML šablona pro aplikaci.

### Struktura pro Vue.js

Struktura vaší aplikace pro Vue.js by byla velmi podobná tomu, co jste popsali. Zde je, jak by se mohla jednotlivá část struktury překrývat s Vue.js specifickými konvencemi:

1. **`assets/`** - Tento adresář zůstává stejný, kde uložíte statické soubory.

2. **`components/`** - V Vue.js se tento adresář používá pro uložení jednotlivých Vue komponentů. Každý Vue komponent může být uložen v samostatném `.vue` souboru.

3. **`views/`** - V Vue.js se tento adresář používá pro komponenty, které reprezentují různé stránky nebo hlavní části aplikace, podobně jako u větších komponent.

4. **`router/`** - Vue.js používá Vue Router pro směrování, takže tento adresář bude obsahovat konfiguraci směrování, např. `index.js` soubor s definicí cest.

5. **`store/`** - Pokud používáte Vuex pro správu stavu, tento adresář bude obsahovat Vuex moduly, akce a mutace.

6. **`styles/`** - Zde můžete ukládat globální styly nebo specifické styly pro komponenty.

7. **`utils/`** - Tento adresář může obsahovat pomocné funkce, které používáte napříč aplikací, stejně jako v jiných JavaScriptových aplikacích.

8. **`App.vue`** - Hlavní komponenta aplikace, kde se skládají všechny ostatní komponenty a která slouží jako základní rámec.

9. **`main.js`** - Vstupní bod aplikace, kde se inicializuje Vue instance a připojují se globální pluginy a konfigurace.

Celkově vzato, struktura složek a souborů vaší aplikace bude velmi podobná tomu, jak se používá ve Vue.js. Klíčové rozdíly mohou spočívat v tom, že Vue.js používá komponenty s `.vue` příponou, které kombinují HTML, CSS a JavaScript do jednoho souboru, a že Vue Router a Vuex mají specifické struktury a konvence pro konfiguraci.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#28-7-2024)  
## Dotaz:

Tak co bych teď tedy měl udělat jako úplně první věc?

## ChatGPT:

Pokud plánujete začít s vývojem aplikace ve Vue.js a máte již připravenou strukturu složek, zde je seznam kroků, které byste měli udělat jako první:

### 1. **Nastavení Vývojového Prostředí**
   - **Nainstalujte Node.js**: Pokud ještě nemáte nainstalovaný Node.js, stáhněte a nainstalujte ho z [oficiálních stránek](https://nodejs.org/). Node.js obsahuje `npm`, což je správce balíčků pro JavaScript.

   - **Nainstalujte Vue CLI**: Vue CLI (Command Line Interface) je nástroj pro rychlé vytváření a správu Vue.js projektů. Instalaci provedete pomocí příkazu:
     ```bash
     npm install -g @vue/cli
     ```
     Nebo, pokud preferujete Yarn:
     ```bash
     yarn global add @vue/cli
     ```

### 2. **Inicializace Nového Vue Projektu**
   - **Vytvořte nový projekt**: Použijte Vue CLI k vytvoření nového Vue projektu. Otevřete terminál a zadejte:
     ```bash
     vue create my-project
     ```
     Následujte pokyny pro výběr předvoleb (např. TypeScript, Vuex, Vue Router). CLI vytvoří strukturu projektu podle výběru.

### 3. **Nastavení Kódového Editoru**
   - **Otevřete projekt v editoru**: Použijte oblíbený kódový editor (např. VS Code) a otevřete složku projektu. Pokud nemáte VS Code, můžete ho stáhnout z [oficiálních stránek](https://code.visualstudio.com/).

   - **Nainstalujte rozšíření**: Doporučuji nainstalovat rozšíření pro Vue.js, například "Vetur" pro VS Code, které poskytuje syntaktické zvýraznění a další užitečné funkce pro Vue.js.

### 4. **Spuštění Aplikace**
   - **Spusťte vývojový server**: V terminálu přejděte do složky projektu a spusťte příkaz:
     ```bash
     npm run serve
     ```
     nebo
     ```bash
     yarn serve
     ```
     Tento příkaz spustí vývojový server a aplikaci budete moci zobrazit v prohlížeči na adrese `http://localhost:8080` (nebo na jiném portu, pokud je tento port již obsazen).

### 5. **Nastavení Verze Kontroly**
   - **Inicializujte Git**: Pokud ještě nemáte inicializovaný Git, udělejte to v kořenovém adresáři projektu:
     ```bash
     git init
     ```
   - **Vytvořte `.gitignore`**: Ujistěte se, že máte správně nastavený soubor `.gitignore`, který vylučuje složky jako `node_modules` a soubory, které nechcete zahrnout do verzovacího systému.

### 6. **Organizace Projektu**
   - **Zkontrolujte strukturu složek**: Ujistěte se, že složky a soubory odpovídají vašim potřebám. Pokud jste vygenerovali projekt pomocí Vue CLI, struktura by měla být již dobře nastavena.

### 7. **První Komponenty a Routování**
   - **Vytvořte první komponenty**: Začněte vytvářet a přidávat komponenty do adresáře `src/components/`.
   
   - **Nastavte routování**: Pokud jste při vytváření projektu zvolili Vue Router, můžete nastavit různé cesty a stránky ve vašem projektu v souboru `src/router/index.js`.

### 8. **Základy Stavu (Volitelné)**
   - **Nastavte Vuex**: Pokud potřebujete správu stavu, můžete začít přidávat Vuex moduly do adresáře `src/store/`.

Tyto kroky by vám měly poskytnout solidní základ pro začátek práce na vaší aplikaci ve Vue.js. Jakmile máte vše nastaveno, můžete začít s implementací specifických funkcí a designu vaší aplikace.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#28-7-2024)  
## Dotaz:

Super :-) Tak Body 1 - 6 mám za sebou a nyní bych od tebe chtěl co nejvíce slyšet k bodu č. 7:
První Komponenty a Routování

Popiš mi celý proces tak abych věděl, co vše je v něm třeba a pak navrhni první kroky, kterými bych měl začít.


## ChatGPT:

Perfektní, že máte základní nastavení hotové! Pojďme se tedy podívat na bod č. 7 – První Komponenty a Routování. Tento bod se zaměřuje na tvorbu komponentů, které tvoří uživatelské rozhraní vaší aplikace, a na nastavení routování, které vám umožní přecházet mezi různými stránkami.

### **První Komponenty a Routování**

#### **1. Vytvoření Prvních Komponentů**

**Komponenty ve Vue.js** jsou základními stavebními bloky aplikace. Každý komponent je samostatná část uživatelského rozhraní, která může obsahovat HTML, CSS a JavaScript.

##### **A. Struktura Komponentu**

Každý komponent obvykle obsahuje tři části:
- **Šablona (Template)**: HTML kód, který definuje strukturu a obsah komponentu.
- **Skript (Script)**: JavaScript, který obsahuje logiku komponentu.
- **Styl (Style)**: CSS, které stylizuje vzhled komponentu.

##### **B. Vytvoření Komponentu**

1. **Vytvořte soubor komponentu**: V adresáři `src/components/` vytvořte nový soubor, například `HelloWorld.vue`. Struktura souboru by mohla vypadat takto:
   ```vue
   <template>
     <div>
       <h1>{{ message }}</h1>
     </div>
   </template>

   <script>
   export default {
     data() {
       return {
         message: 'Hello, Vue!'
       };
     }
   };
   </script>

   <style scoped>
   h1 {
     color: #42b983;
   }
   </style>
   ```

2. **Importujte a použijte komponent**: Otevřete soubor `src/App.vue` nebo jiný komponent, kde chcete nový komponent použít, a přidejte import a použití:
   ```vue
   <template>
     <div id="app">
       <HelloWorld />
     </div>
   </template>

   <script>
   import HelloWorld from './components/HelloWorld.vue';

   export default {
     components: {
       HelloWorld
     }
   };
   </script>
   ```

3. **Stylizujte a přizpůsobte komponent**: Můžete přidat další CSS styly a přizpůsobit komponent podle potřeby.

#### **2. Nastavení Routování**

**Routování** ve Vue.js umožňuje vytvářet více stránek v jedné aplikaci a řídit, co se zobrazuje na základě URL.

##### **A. Instalace Vue Router**

Pokud jste při vytváření projektu zvolili Vue Router, měl by být již nainstalován. Pokud ne, můžete ho nainstalovat pomocí:
```bash
npm install vue-router
```
nebo
```bash
yarn add vue-router
```

##### **B. Konfigurace Vue Router**

1. **Vytvořte soubor pro konfiguraci routeru**: Otevřete `src/router/index.js` a nastavte základní routy:
   ```javascript
   import Vue from 'vue';
   import Router from 'vue-router';
   import Home from '../components/Home.vue';
   import About from '../components/About.vue';

   Vue.use(Router);

   export default new Router({
     mode: 'history',
     routes: [
       {
         path: '/',
         name: 'Home',
         component: Home
       },
       {
         path: '/about',
         name: 'About',
         component: About
       }
     ]
   });
   ```

2. **Vytvořte komponenty pro různé stránky**: Vytvořte soubory `Home.vue` a `About.vue` v adresáři `src/components/`, podobně jako jste vytvořili `HelloWorld.vue`.

3. **Nastavte Router View**: Otevřete soubor `src/App.vue` a přidejte `<router-view>` pro zobrazení aktuálně vybrané komponenty:
   ```vue
   <template>
     <div id="app">
       <router-view/>
     </div>
   </template>
   ```

4. **Přidejte navigaci**: Pokud chcete přidat odkazy pro navigaci mezi stránkami, použijte komponent `<router-link>`:
   ```vue
   <template>
     <div>
       <nav>
         <router-link to="/">Home</router-link>
         <router-link to="/about">About</router-link>
       </nav>
       <router-view/>
     </div>
   </template>
   ```

#### **První Kroky, Které Byste Měli Udělat**

1. **Vytvořte a nastavte první komponenty**: Začněte s jednoduchými komponenty, jako je `HelloWorld.vue`, `Home.vue` a `About.vue`.

2. **Nastavte základní routování**: Ujistěte se, že vaše router konfigurace odpovídá struktura komponent a vytvořte základní trasy.

3. **Testujte aplikaci**: Spusťte aplikaci a zkontrolujte, zda se komponenty správně zobrazují a routování funguje podle očekávání.

4. **Přidejte navigační prvky**: Přidejte navigační odkazy nebo menu, které umožní uživatelům přecházet mezi stránkami.

5. **Zaměřte se na detailní ladění**: Jakmile máte základní komponenty a routování na místě, začněte ladit detaily, jako jsou styly a interakce.

Tento proces vám pomůže vytvořit základní strukturu vaší aplikace ve Vue.js, a vy budete mít možnost pokračovat v přidávání funkcionality a vylepšení podle potřeby.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#28-7-2024)  
## Dotaz:

Co by měl obsahovat soubor App.vue?


## ChatGPT:

Soubor `App.vue` je hlavní komponenta vaší Vue.js aplikace a funguje jako základní "kostra" pro celou aplikaci. Zde se obvykle nachází struktura, která obklopuje a zahrnuje všechny ostatní komponenty, které aplikace používá. 

### **Typický Obsah Souboru `App.vue`**

#### **1. Šablona (Template)**

V šabloně definujete, jak bude vypadat hlavní rozhraní aplikace. Obvykle obsahuje strukturu, jako je hlavička, navigace a obsah, který se mění podle aktuální stránky nebo komponenty.

```vue
<template>
  <div id="app">
    <header>
      <h1>Moje Aplikace</h1>
      <nav>
        <router-link to="/">Domů</router-link>
        <router-link to="/about">O nás</router-link>
      </nav>
    </header>
    <main>
      <router-view/>
    </main>
    <footer>
      <p>&copy; 2024 Moje Aplikace</p>
    </footer>
  </div>
</template>
```

- **`<header>`**: Může obsahovat navigační odkazy, logo nebo jiný obsah, který je zobrazován na všech stránkách.
- **`<main>`**: Zde se zobrazuje aktuální komponenta, kterou Vue Router určí na základě URL. `<router-view/>` je speciální komponent, který se dynamicky mění podle aktuální trasy.
- **`<footer>`**: Obvykle obsahuje informace o copyrightu nebo další informace, které jsou zobrazovány na všech stránkách.

#### **2. Skript (Script)**

Skripty v komponentě `App.vue` obvykle obsahují definice a logiku, která se vztahuje na celou aplikaci. Můžete sem zahrnout importy, konfigurace nebo životní cyklus hooky.

```vue
<script>
export default {
  name: 'App',
  // Můžete přidat další vlastnosti a metody, pokud je potřeba
}
</script>
```

#### **3. Styl (Style)**

V sekci stylů definujete, jak bude aplikace vypadat. Můžete přidat globální styly, které ovlivní celé uživatelské rozhraní. Použití `scoped` zajistí, že styly se aplikují pouze na tento komponent.

```vue
<style scoped>
#app {
  text-align: center;
  color: #2c3e50;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

header {
  background-color: #42b983;
  padding: 20px;
}

nav {
  margin: 20px 0;
}

footer {
  background-color: #42b983;
  padding: 10px;
  color: white;
}
</style>
```

#### **Další Možnosti**

- **Globální nastavení**: Pokud potřebujete globální nastavení nebo inicializaci (např. konfiguraci globálních knihoven), můžete je přidat do `App.vue`.
- **Mixiny a Direktivy**: Pokud používáte mixiny nebo vlastní direktivy, můžete je v `App.vue` registrovat.

### **Příklad Kompletního Souboru `App.vue`**

```vue
<template>
  <div id="app">
    <header>
      <h1>Moje Aplikace</h1>
      <nav>
        <router-link to="/">Domů</router-link>
        <router-link to="/about">O nás</router-link>
      </nav>
    </header>
    <main>
      <router-view/>
    </main>
    <footer>
      <p>&copy; 2024 Moje Aplikace</p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style scoped>
#app {
  text-align: center;
  color: #2c3e50;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

header {
  background-color: #42b983;
  padding: 20px;
}

nav {
  margin: 20px 0;
}

footer {
  background-color: #42b983;
  padding: 10px;
  color: white;
}
</style>
```

### **Závěr**

Soubor `App.vue` je klíčovým komponentem, který definuje strukturu vaší aplikace a zahrnuje základní navigaci, rozložení a vzhled. Je to místo, kde se definují globální styly a struktura, která obklopuje ostatní komponenty. Jakmile budete mít tento základní soubor, můžete začít přidávat další komponenty a zpracovávat specifické funkce a vzhled jednotlivých stránek vaší aplikace.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#28-7-2024)  
## Dotaz:

Když dostanu takovouto chybu:

    Compiled with problems:
    ×
    ERROR
    [eslint] 
    C:\Users\Sudip2708\Dropbox\__DjangoBlogWeb\frontend\src\App.vue
      11:19  error  Insert `·`                     prettier/prettier
      21:9   error  Replace `'App'` with `"App",`  prettier/prettier
      22:2   error  Insert `;`                     prettier/prettier
    
    ✖ 3 problems (3 errors, 0 warnings)
      3 errors and 0 warnings potentially fixable with the `--fix` option.

Co to znamená a co mám udělat?

## ChatGPT:

Tato chyba je způsobena problémem s formátováním kódu podle pravidel, která jsou definována v konfiguraci ESLint a Prettier. ESLint je nástroj pro analýzu kódu v JavaScriptu a Prettier je nástroj pro automatické formátování kódu. Když tyto nástroje zjistí, že váš kód neodpovídá definovaným pravidlům, zobrazí chyby, které vám pomohou kód upravit tak, aby byl konzistentní.

### **Vysvětlení Chybových Hlásení**

1. **Chyba na řádku 11:**
   - **"Insert `·`"** znamená, že Prettier očekává, že na daném místě bude mezera (nebo jiný formátovací znak). Obvykle se to stává u nevhodně zarovnaných nebo nesprávně formátovaných bloků kódu.

2. **Chyba na řádku 21:**
   - **"Replace `'App'` with `"App",`"** znamená, že byste měli změnit uvozovky kolem `'App'` na dvojité uvozovky `"` a přidat čárku na konec. Toto pravidlo se týká stylu, který používá Prettier pro konzistentní formátování.

3. **Chyba na řádku 22:**
   - **"Insert `;`"** znamená, že chybí středník na konci řádku. Prettier doporučuje použití středníků pro lepší čitelnost a prevenci potenciálních chyb.

### **Jak Opravit Tyto Chyby**

1. **Opravit Manuálně:**
   - Otevřete soubor `App.vue` a opravte formátování podle chybových hlášení.
   - Příklad oprav:
     - Přidat mezery na řádcích, kde je to požadováno.
     - Změnit uvozovky podle doporučení.
     - Přidat chybějící středníky.

2. **Použít Automatické Opravy:**
   - **Příkaz pro automatické opravy:** Spusťte `eslint --fix` nebo `prettier --write` v terminálu, pokud máte tyto nástroje nainstalované. Tento příkaz automaticky opraví chyby podle pravidel konfigurace.
     ```bash
     npx eslint --fix src/App.vue
     npx prettier --write src/App.vue
     ```

### **Příklad Opraveného Kódu**

Pokud máte následující kód:

```vue
<template>
  <div id='app'>
    <header>
      <h1>Moje Aplikace</h1>
      <nav>
        <router-link to="/">Domů</router-link>
        <router-link to="/about">O nás</router-link>
      </nav>
    </header>
    <main>
      <router-view/>
    </main>
    <footer>
      <p>&copy; 2024 Moje Aplikace</p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style scoped>
#app {
  text-align: center;
  color: #2c3e50;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

header {
  background-color: #42b983;
  padding: 20px;
}

nav {
  margin: 20px 0;
}

footer {
  background-color: #42b983;
  padding: 10px;
  color: white;
}
</style>
```

Měl byste upravit soubor takto:

```vue
<template>
  <div id="app">
    <header>
      <h1>Moje Aplikace</h1>
      <nav>
        <router-link to="/">Domů</router-link>
        <router-link to="/about">O nás</router-link>
      </nav>
    </header>
    <main>
      <router-view />
    </main>
    <footer>
      <p>&copy; 2024 Moje Aplikace</p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
}
</script>

<style scoped>
#app {
  text-align: center;
  color: #2c3e50;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

header {
  background-color: #42b983;
  padding: 20px;
}

nav {
  margin: 20px 0;
}

footer {
  background-color: #42b983;
  padding: 10px;
  color: white;
}
</style>
```

### **Kroky pro Údržbu**

1. **Přidejte `eslint` a `prettier` do projektu:**
   - Ujistěte se, že máte nainstalované a správně nakonfigurované ESLint a Prettier v projektu.

2. **Pravidelně Spouštějte Automatické Opravy:**
   - Používejte `eslint --fix` a `prettier --write` pravidelně, abyste udrželi kód čistý a dobře formátovaný.

3. **Zkontrolujte Konfiguraci:**
   - Ujistěte se, že konfigurace ESLint a Prettier odpovídá vašim preferencím. Můžete si přizpůsobit pravidla ve souborech jako `.eslintrc.js` a `.prettierrc`.

Pokud budete mít další dotazy nebo problémy s formátováním kódu, dejte mi vědět!

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#28-7-2024)  
## Dotaz:

Takže jsem si App.vue zjednodušil na tento zápis:
    
    <template>
      <div>
        <header>
          <p>Header</p>
        </header>
        <main>
          <p>Main</p>
        </main>
        <footer>
          <p>Footer</p>
        </footer>
      </div>
    </template>
    
    <script>
    export default {
      name: "App",
    };
    </script>
    
    <style scoped>
    p {
      font-size: 20px;
    }
    </style>


A nejprve by mě zajímalo, co znamená:

    export default {
      name: "App",
    };

???

## ChatGPT:

V kontextu Vue.js je zápis:

```javascript
export default {
  name: "App",
};
```

součástí `script` bloku souboru Vue komponenty. Tento kód slouží k definici a exportu Vue komponenty. Rozepíšu vám jednotlivé části:

### **Co znamená `export default { name: "App" }`**

1. **`export default`:**
   - `export default` je syntaktická konstrukce v JavaScriptu, která umožňuje exportovat jednotlivé objekty nebo hodnoty z modulu. V kontextu Vue.js to znamená, že komponenta, kterou definujete, bude dostupná pro importování v jiných částech aplikace.

2. **`{ name: "App" }`:**
   - Tento objekt je hlavním nastavením vaší Vue komponenty. V tomto případě obsahuje pouze jedno pole `name`, které je string `"App"`. 

### **Co Dělá `name: "App"`**

- **`name`:**
  - `name` je vlastnost objektu komponenty, která specifikuje název komponenty. Tento název je užitečný pro různé účely:
    - **Debugging:** Při ladění v nástroji pro vývojáře (např. Chrome DevTools) se komponenty zobrazí pod jejich názvem, což usnadňuje identifikaci.
    - **Dynamické Komponenty:** Pokud používáte Vue’s dynamic components (např. `component :is="componentName"`), název komponenty může být užitečný.
    - **Význam v Route Definitions:** Pokud používáte Vue Router a chcete mapovat názvy komponent na různé cesty, název komponenty je důležitý.

### **Co Můžete Přidat do `export default { ... }`**

Objekt, který exportujete, může obsahovat další vlastnosti a metody, které definují chování komponenty. Například:

- **`data`:** Definuje data komponenty.
- **`methods`:** Obsahuje metody, které můžete volat z vaší komponenty.
- **`props`:** Definuje vlastnosti, které komponenta přijímá z rodičovské komponenty.
- **`computed`:** Obsahuje computed properties, které jsou hodnoty závislé na datech.
- **`watch`:** Definuje funkce, které se spouštějí při změně dat.

Příklad rozšířené komponenty:

```javascript
export default {
  name: "App",
  data() {
    return {
      message: "Hello Vue!"
    };
  },
  methods: {
    greet() {
      alert("Hello!");
    }
  },
  computed: {
    reversedMessage() {
      return this.message.split('').reverse().join('');
    }
  }
};
```

### **Shrnutí**

- `export default { name: "App" };` exportuje Vue komponentu s názvem `App`.
- Tento název je užitečný pro ladění, dynamické použití a v některých případech při práci s routerem.

Pokud máte další dotazy ohledně komponent, kódu nebo Vue.js, neváhejte se ptát!

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#28-7-2024)  