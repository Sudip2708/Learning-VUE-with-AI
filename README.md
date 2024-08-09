# Učení se VUE za asistence AI

### Smysl tohoto dokumentu:
Tento dokument vzniká z několika důvodů:  
•  Jednak pro mě, aby sloužil jako záznam mojí cesty a pro případné dohledávání postupů.  
•  Také jako ukázka stylu mé práce a komunikace s AI pro potenciální zadavatele.  
•  A možná i jako návod pro ty, kdo s programováním začínají a rádi by využili možnosti, které současné AI nabízí.  

### O mně:
Rok jsem se učil čistě Python.  
3 měsíce jsem se učil HTML, CSS a základy vývoje webu a práce s databází.  
4 měsíce jsem se učil Flask a vytvořil v něm svou první webovou aplikaci.  
6 měsíců jsem se učil Django a vytvořil v něm svou druhou webovou aplikaci.  

### O AI:
AI byla důvod, proč jsem se rozhodl studovat Python.  
Zatím ji neovládám, ale rozumím konstrukci hlubokého učení a plánuju se mu v budoucnu věnovat.  
Z uživatelského pohledu mi AI vstoupila do života na začátku roku 2024, kdy jsem ji aktivně začal používat pro učení se Django.  

### Můj pohled na AI:
AI je pro mě silný statistický nástroj se schopností porozumět kontextu.  
Na mé cestě je mi asistentem a zdrojem nezměrného množství dat.  

### Způsob práce s AI:
Tak trochu doufám, že to bude cítit i z níže přidaných dokumentů, ale tím hlavním, čím mě AI získala, je,
že jejím prostřednictvím mám možnost, kterou jsem dosud neměl - a to rozebírat určitou věc tak dlouho, dokud ji neporozumím.  
Toto je velká přednost, kterou AI nabízí, a dovolil bych si tipnout, že ve školství bude AI v budoucnu hrát velkou roli.  
Používáním AI člověk získává schopnost rozvíjet své znalosti rychleji a zároveň i hlouběji.  

### Co je to VUE:
VUE je stavebnice, ze které se dá poskládat viditelná část webových stránek.  
Výhodou použití VUE oproti vytváření té samé části pomocí šablonového systému Django je modularita a přehlednost založená na komponentovém přístupu.  

### Jak jsem se k VUE dostal:
Při hledání práce jsem narazil na nabídku, kde VUE byla podmínkou.  
Od prvního momentu jsem se do VUE zamiloval, protože nabízelo přesně to, co mi v Django chybělo.  
Přerušil jsem tedy vytváření testů pro svou aplikaci v Django a začal se naplno věnovat studiu VUE.  
A zde už se dostáváme k samotnému obsahu tohoto dokumentu, jenž má za úkol celý tento proces zmapovat.

# 1. krok - Seznámení se s VUE

### 18. - 20. 7. 2024 | První seznámení + Vytvoření taháků
(Část zde uvedených materiálů vznikala i během dalších dní, ale pro lepší přehlednost jsou všechny uvedené zde.)

[• Stručný tahák od Aria](preparation/VueCheatSheet_Aria.md)  
[• Podrobnější tahák od ChatGPT](preparation/VueCheatSheet_ChatGPT.md)  
[• Inicializace projektu VUE](preparation/VueInit.md)  
[• Základní struktura projektu Vue](preparation/VueBaseStrukture.md)  
[• Organizace VUE komponent](preparation/VueBasicComponents.md)  
[• Metody a funkce pro 'script setup' ve VUE](preparation/VueScript.md)  
[• Funkce a techniky pro 'style scoped' ve VUE](preparation/VueStyle.md)  
[• Direktivy a funkce v 'templates' ve VUE](preparation/VueTemplates.md)  
[• Dotaz na strukturu: Script - Templates - Style](preparation/VueStratSet.md)  
[• Psaní čárek, středníků a mezer v JavaScriptu a Vue.js](preparation/WritingCommasSemicolonsAndSpaces.md)  

# 2. krok - Osahání si VUE

### 21. - 23. 7. 2024 | Zhlédnutí výukového videa + vytvoření v něm uvedených projektů

Výuková videa jsou výborným zdrojem pro základní informace k danému tématu.  
Pro VUE existuje mnoho různých výukových videí, a tak má člověk možnost vybrat si to, které mu nejvíce vyhovuje.  
V mém případě vyhrál tento 6 hodinový kurz, který jsem si rozložil do 3 dní:  
[Laith Academy - The Ultimate Vue 3 Tutorial](https://www.youtube.com/watch?v=I_xLMmNeLDY&t=231s)


# 3. Krok - Převod Django na VUE

### 27. 7. 2024
[**Integrace VUE do Django - úvod od ChatGPT**](django-to-vue/integrace_vue_chatgpt.md)  
[• *Úvodní kroky pro integraci Vue do Django projektu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz)  
[• *Kontrola instalace Node.js a npm*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-1)  
[• *Použití příkazu npm install vue@latest*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-2)  
[• *Řešení chyby při spuštění vue create frontend*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-3)  
[• *Průvodce nastavením při vue create frontend*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-4)  
[• *Výběr funkcí při tvorbě projektu ve Vue!*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-5)  
[• *Pozdější aktivace funkcí ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-6)  
[• *Dopad výběru jednotlivých funkcí ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-7)  
[• *Shrnutí volby funkcí a další otázky*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-8)  
[• *Výběr CSS pre-processoru ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-9)  
[• *Výběr linter/formatter konfigurace*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-10)  
[• *Volba linting funkcí*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-11)  
[• *Umístění konfiguračních souborů*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-12)  
[• *Další kroky po vytvoření základního prostředí ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_chatgpt.md#dotaz-13)  

[**Integrace VUE do Django - úvod od Claude**](django-to-vue/integrace_vue_claude.md)  
[• *Úvodní kroky pro integraci Vue do Django projektu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_claude.md#dotaz)  
[• *Instalace Vue CLI globálně*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_claude.md#dotaz-1)  
[• *Umístění projektu Vue v rámci struktury Django*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_claude.md#dotaz-2)  
[• *Příkazy pro vytvoření frontendu ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/integrace_vue_claude.md#dotaz-3)  

### 28. 7. 2024
[**Začátek práce na projektu**](django-to-vue/zacatek_prace_na_projektu_01.md)  
[• *Seznámení se stromovou strukturou projektu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_01.md#dotaz)  
[• *Analýza aplikace a struktury templates pro Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_01.md#dotaz-1)  
[• *První kroky po analýze projektu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_01.md#dotaz-2)  
[• *První komponenty a routování ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_01.md#dotaz-3)  
[• *Obsah souboru App.vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_01.md#dotaz-4)  
[• *Řešení chyb ESLint a Prettier ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_01.md#dotaz-5)  
[• *Význam export default v App.vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_01.md#dotaz-6)  

### 29. 7. 2024
[**Převod základní šablony**](django-to-vue/zacatek_prace_na_projektu_02.md)  
[• *Přenos základní šablony z Django do Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz)  
[• *Význam chybových hlášení*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-1)  
[• *Další význam chybových hlášení*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-2)  
[• *Doporučený postup při oddělení frontendu z Django do Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-3)  
[• *Výběr umístění statických souborů ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-4)  
[• *Návrh umístění souborů z Django static a media ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-5)  
[• *Přenesení nastavení z Django HEAD do Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-6)  
[• *Ponechání noscript tagu v public/index.html*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-7)  
[• *Kontrola struktury souboru pro rozčlenění*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-8)  
[• *Zajištění integrity odkazů pro externí zdroje*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-9)  
[• *Přenos hlavičky a struktury navigace z Django do Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-10)  
[• *Převod šablon do Vue a oddělení komponent*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-11)  
[• *Dočasné vypnutí sémantických chyb od Prettier*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-12)  
[• *Dočasné vypnutí chyb od ESLint*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_02.md#dotaz-13)  

### 30. 7. 2024
[**Práce s terminálem**](django-to-vue/zacatek_prace_na_projektu_03.md)  
[• *Vliv více spuštěných terminálů na běh systému*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz)  
[• *Přehled kroků k propojení Vue a Django pro výpis článků*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-1)  
[• *Vysvětlení procesu komunikace mezi Django a Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-2)  
[• *Jak se Vue dozví o adrese serveru Django*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-3)  
[• *Shrnutí postupu pro propojení Django a Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-4)  
[• *Detaily pro nastavení django-cors-headers*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-5)  
[• *Kontrola nastavení a umístění konfigurace v settings.py*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-6)  
[• *Kde umístit nastavení URL pro spolupráci s Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-7)  
[• *Význam URL endpointů v Django a jejich použití*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-8)  
[• *Návrh na úpravu Django view pro vracení JSON dat*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-9)  
[• *Konfigurace URL API endpointů ve Vue.js*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-10)  
[• *Kroky pro přidání nové stránky ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-11)  
[• *Umístění souborů ve Vue do src/views vs. src/components*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-12)  
[• *Analýza chyby při načítání dat z API ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-13)  
[• *Problém s lomítkem na konci URL ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-14)  
[• *Shrnutí propojení Django s Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_03.md#dotaz-15)  

### 31. 7. 2024
[**Vytváření opakovaně použitelných komponent**](django-to-vue/zacatek_prace_na_projektu_04.md)  
[• *Přehled použití vzorových komponent ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_04.md#dotaz)  
[• *Stylizace HTML pomocí vlastních tříd vs. Bootstrap*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_04.md#dotaz-1)  
[• *Použití `:root` a proměnných z CSS ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_04.md#dotaz-2)  
[• *Kontrola CSS pro třídu `container` a umístění specifických stylů*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_04.md#dotaz-3)  
[• *Import stylů přímo do komponent*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_04.md#dotaz-4)  
[• *Vytváření specifických stylů, které obsahují více importů*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_04.md#dotaz-5)  
[• *Význam stylů pro `*`, `*::before`, `*::after` a `scroll-behavior`*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_04.md#dotaz-6)  
[• *Rozhodování mezi umístěním importu do `main.js` nebo `App.vue`*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_04.md#dotaz-7)  
[• *Určení, zda styly pro `<body>` patří do globálního stylu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_04.md#dotaz-8)  
[• *Význam stylu pro `<abbr>` s atributem `title`*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_04.md#dotaz-9)  
[• *Vazba stylu na tag `<abbr>` a jeho potřeba v projektu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_04.md#dotaz-10)  

### 1. 8. 2024
[**Přechod na Vue bez závislosti na Bootstrap**](django-to-vue/zacatek_prace_na_projektu_05.md)  
[• *Přiřazení CSS pro `<body>` ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_05.md#dotaz)  
[• *Alternativní způsoby zápisu kódu ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_05.md#dotaz-1)  
[• *Výhody a nevýhody inline stylů ve Vue vs. globální CSS soubory*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_05.md#dotaz-2)  
[• *Alternativy pro `collapse` a `navbar-collapse` ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_05.md#dotaz-3)  
[• *Vytváření Vue komponent s oddělením stylů a responzivity*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_05.md#dotaz-4)  
[• *Potřeba použití `!important` ve style scoped*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_05.md#dotaz-5)  
[• *Náhrada `<a>` za `<router-link>` ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_05.md#dotaz-6)  
[• *Jednoduchý kód pro zobrazení obrázku ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_05.md#dotaz-7)  
[• *Použití nástrojů ve Vue pro vytváření komponent podobných Bootstrapu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_05.md#dotaz-8)  
[• *Porovnání používání Vue komponent a Bootstrapu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-9)  

### 2. 8. 2024
[**Přispůsobení se VUE struktuře + stránka se všemi články**](django-to-vue/zacatek_prace_na_projektu_06.md)  
[• *Vytvoření Vue komponenty pro hlavičku webu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz)  
[• *Možnosti nastavení šířky hlavičky webu ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-1)  
[• *Zátěž 12000 řádkového CSS a přechod na style scoped ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-2)  
[• *Import CSS stylů do Vue komponenty a struktura DOMu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-3)  
[• *Výhody globálních CSS souborů vs. scoped styly ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-4)  
[• *Vysvětlení kódu Vue šablony pro zobrazení seznamu článků*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-5)  
[• *Rozdíl mezi `response` a `response.data` v Axiosu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-6)  
[• *Struktura a zobrazení dat v seznamu článků ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-7)  
[• *Přepis kódu pro zobrazení klíčů prvního článku*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-8)  
[• *Přístup k datům autora článku přes Vue a API*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-9)  
[• *Vytvoření Vue komponenty pro zobrazení článků podle specifikací*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-10)  
[• *Rozdělení kódu Vue šablony do menších komponent*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_06.md#dotaz-11) 

### 4. 8. 2024
[**Úprava Django pro VUE**](django-to-vue/zacatek_prace_na_projektu_07.md)  
[• *Dotaz na použití třídy `APIView` vs. `ListView` v Django*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz)  
[• *Získání absolutní cesty pro náhledový obrázek článku*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-1)  
[• *Získání názvu kategorie na základě ID v Django*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-2)  
[• *Přístup k profilovému obrázku autora článku v Django*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-3)  
[• *Výpočet stáří článku: Django vs. Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-4)  
[• *Získání počtu komentářů pro článek v Django*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-5)  
[• *Předávání názvu stránky z Django do Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-6)  
[• *Optimalizace dotazu: filtrovat data v dotazu vs. filtrovat po načtení*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-7)  
[• *Rozdíl mezi dotazem s cizím klíčem a novým dotazem v Django ORM*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-8)  
[• *Zpracování absolutní cesty k obrázkům: dotaz vs. cyklus*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-9)
[• *Přenášení slugu kategorie do Vue pro tvorbu URL*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-10)  
[• *Filtrace článků podle statusu publikace v dotazu Django*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-11)  
[• *Jak metoda `values()` využívá `annotate()` v Django ORM*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-12)  
[• *Zajištění absolutních cest pro obrázky v dotazu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-13)  
[• *Vytvoření metod pro vracení absolutních URL obrázků v modelu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-14)  
[• *Použití `Concat` v `annotate` pro absolutní URL obrázků*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-15)  
[• *Jak Vue zpracovává URL obrázku získané jako řetězec*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-16)  
[• *Nahrazení dotazu pro zpracování dat v Django*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-17)  
[• *Význam a přínos serializace dat*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-18)  
[• *Co všechno zahrnuje serializace dat*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-19)  
[• *Serializace v případě, kdy přenášíte pouze řetězce*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-20)  
[• *Možnost dělení tříd pohledů a serializerů v Django*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-21)  
[• *Umístění serializerů v projektu s více pohledy*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-22)  
[• *Optimalizace dotazů a serializace pouze potřebných polí*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-23)  
[• *Použití serializace pro validaci dat v Django a Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07.md#dotaz-24)

[**Dotazy z procházky**](django-to-vue/zacatek_prace_na_projektu_07_prochazka.md)  
[• *Úvod do JavaScriptu: Základy, struktura a logika*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz)  
[• *Proč prohlížeče podporují JavaScript a ne jiný jazyk*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-1)  
[• *Možnost vývoje mobilních aplikací ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-2)  
[• *Srovnání Vue a Kivy pro různé platformy*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-3)  
[• *Vue pro webové aplikace vs. Kivy pro mobilní a desktopové aplikace*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-4)  
[• *Srovnání učící křivky mezi Kivy, JavaScriptem a Pythonem*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-5)  
[• *Naučení Kivy při znalosti Pythonu a JavaScriptu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-6)  
[• *Příklady Kivy Language a specifických widgetů Kivy*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-7)  
[• *Historie Kivy: Kdo, proč a jak vznikl*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-8)  
[• *Vývoj jazyka Python a jeho postavení mezi ostatními jazyky*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-9)  
[• *Python a jeho postavení v moderních programovacích jazycích*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-10)  
[• *Python jako angličtina programovacích jazyků*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-11)  
[• *Znalosti potřebné k úspěchu jako programátor webových aplikací v Pythonu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-12)  
[• *Význam a definice RESTful API*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_07_prochazka.md#dotaz-13)

### 5. 8. 2024
[**Stránkování ve VUE**](django-to-vue/zacatek_prace_na_projektu_08.md)  
[• *Možnosti stránkování v Django a Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz)  
[• *Jak stránkování ovlivňuje dotazy na databázi*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-1)  
[• *Zahrnutí čísla stránky do URL*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-2)  
[• *Nekonečné scrollování vs. stránkování*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-3)  
[• *Přínos nekonečného scrollování pro PC*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-4)  
[• *Rozdíl mezi stránkovacími třídami v Django, a příklady kódu pro Axio*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-5)
[• *Kód komponenty pro stránkování v Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-6)  
[• *Použití 'name' v komponentách Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-7)  
[• *Použití 'name' ve všech komponentách Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-8)  
[• *Přidání čísla stránky do URL v Django a Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-9)  
[• *Dvě varianty adres v Django URLs*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-10)  
[• *Rozpis kódu pro stránkování v Django views*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-11)  
[• *Nastavení `page_size` v Django a jeho vliv na `settings.py`*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-12)  
[• *Rozdíl mezi zápisy pro stránkování v `index.js`*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_08.md#dotaz-13)

[**Dohledání chyby**](django-to-vue/zacatek_prace_na_projektu_08_hledani_chyby.md)  

### 6. 8. 2024
[**Stránka pro detail článku 1/2**](django-to-vue/zacatek_prace_na_projektu_09.md)  
[• *Jak nastavit, aby `ArticleCardBody` odkazoval na detail článku*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz)  
[• *Jak doplnit zobrazení hlavního obrázku na stránce s detailem článku*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-1)  
[• *Co může způsobovat prázdnou stránku*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-2)  
[• *Jak zobrazit data pro článek na detailní stránce*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-3)  
[• *Jak ověřit a opravit volání API v kódu Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-4)  
[• *Jak přidat předponu `api` k URL v Django*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-5)  
[• *Co může chybět, pokud stránka pro všechny články funguje*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-6)  
[• *Jak diagnostikovat chyby po přejmenování komponent a cest*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-7)  
[• *Jak ověřit chyby v kódu Vue v konkrétních souborech*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-8)  
[• *Jak správně vytvořit `router-link` ve Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-9)  
[• *Zobrazení detailu článku na základě slug a API odpovědi*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-10)  
[• *Co znamená „Načítání článku...“*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-11)  
[• *Oprava zpracování pohledu článku na Django serveru*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-12)  
[• *Oprava chybu v kódu Vue*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_09.md#dotaz-13)

### 7. 8. 2024
[**Stránka pro detail článku 2/2**](django-to-vue/zacatek_prace_na_projektu_10.md)  
[• *Serializer pro pole s HTML obsahem v TinyMCE*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz)  
[• *Výběr mezi CharField a TextField pro dlouhé texty*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-1)  
[• *Použití TextField pro krátké texty v Django Serializeru*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-2)  
[• *Varování v PyCharmu u TextField v Django Serializeru*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-3)  
[• *Účel specifikace polí v Serializeru a validační možnosti*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-4)  
[• *Výhody a nevýhody specifikace polí v Serializeru*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-5)  
[• *Základní Vue komponenta pro vykreslení pole content*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-6)  
[• *Přidání polí pro předchozí a následující článek do pohledu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-7)  
[• *Základní Vue kód pro komponentu ConnectedArticles*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-8)  
[• *Přidání tagů do komponenty s tagy v článku*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-9)  
[• *Chyba při použití TaggableManager v Django*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-10)  
[• *Optimalizace dotazů pro získání tagů a dat článku*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-11)  
[• *Význam a řešení chyb v konzoli*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-12)  
[• *Chyba s bullet-icon.svg v HTML obsahu*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-13)  
[• *Řešení problému s bullet-icon.svg a úprava odrážek*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI/blob/main/django-to-vue/zacatek_prace_na_projektu_10.md#dotaz-14)  


