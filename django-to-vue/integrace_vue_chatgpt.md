# Obsah:

[• Úvodní kroky pro integraci Vue do Django projektu](#dotaz-1)  
[• Kontrola instalace Node.js a npm](#dotaz-2)  
[• Použití příkazu npm install vue@latest](#dotaz-3)  
[• Řešení chyby při spuštění vue create frontend](#dotaz-4)  
[• Průvodce nastavením při vue create frontend](#dotaz-5)  
[• Výběr funkcí při tvorbě projektu ve Vue!](#dotaz-6)  
[• Pozdější aktivace funkcí ve Vue](#dotaz-7)  
[• Dopad výběru jednotlivých funkcí ve Vue](#dotaz-8)  
[• Shrnutí volby funkcí a další otázky](#dotaz-9)  
[• Výběr CSS pre-processoru ve Vue](#dotaz-10)  
[• Výběr linter/formatter konfigurace](#dotaz-11)  
[• Volba linting funkcí](#dotaz-12)  
[• Umístění konfiguračních souborů](#dotaz-13)  
[• Další kroky po vytvoření základního prostředí ve Vue](#dotaz-14)  



## Dotaz:

Ahojky :-)

Už pár dní se tu učím VUE a dnes bych si chtěl poprvé vyzkoušet ho integrovat do mého projektu dělaném v Dando.

Projektem je webová aplikace, která umožňuje vytváření a zprávu multimediálních článků a v současné době je veškerý frontend součástí Django.

Později ti sem nahraji stromovou strukturu projektu a šablon a budu se ptát už na konkrétní kroky pro můj projekt, ale nejprve bych rád pro lepší představu seznam kroků, tak jak budou následovat od začátku až po dokončení oddělení frontendu. A ideálně mi rovnou u každého kroku napiš, proč je tento krok důležitý, jaké části aplikace se týká, a na co je potřeba v daném kroku dávat pozor.


## ChatGPT:

Ahoj! Integrace Vue.js do existujícího Django projektu je skvělý způsob, jak modernizovat frontend a využít výhod JavaScriptového frameworku. Zde je seznam kroků, které bys měl následovat, spolu s vysvětlením jejich důležitosti a toho, na co si dát pozor:

### 1. Příprava prostředí
#### Proč:
- Abys měl prostředí pro práci s Vue.js a mohl spravovat závislosti.
#### Jak:
- Nainstaluj Node.js a npm (Node Package Manager).
- Inicializuj projekt s Vue CLI.
```bash
npm install -g @vue/cli
vue create my-vue-project
```
#### Na co dávat pozor:
- Ujisti se, že máš správné verze Node.js a npm.

### 2. Struktura projektu
#### Proč:
- Aby byly jasně odděleny části aplikace (frontend a backend).
#### Jak:
- Vytvoř ve svém Django projektu složku pro Vue aplikaci, například `frontend`.
- Přesuň nově vytvořený Vue projekt do této složky.
```bash
mv my-vue-project /path/to/your/django/project/frontend
```
#### Na co dávat pozor:
- Dodržuj správnou strukturu adresářů, aby byla aplikace přehledná.

### 3. Nastavení Django pro statické soubory
#### Proč:
- Aby Django mohlo sloužit statické soubory vytvořené Vue aplikací.
#### Jak:
- V `settings.py` přidej cesty ke statickým souborům.
```python
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "frontend/dist"),
]
```
#### Na co dávat pozor:
- Správné nastavení cest, aby Django našlo a správně servírovalo statické soubory.

### 4. Integrace Vue do Django šablon
#### Proč:
- Aby Vue komponenty byly součástí Django šablon.
#### Jak:
- V šablonách vlož Vue komponenty a inicializuj Vue instanci.
```html
<!DOCTYPE html>
<html lang="en">
<head>
  ...
</head>
<body>
  <div id="app"></div>
  <script src="{% static 'path/to/your/vue-built-file.js' %}"></script>
</body>
</html>
```
- Ve Vue projektu nastav `build` cestu na Django statickou složku.
```javascript
// vue.config.js
module.exports = {
  outputDir: '../static/dist',
  assetsDir: 'static',
  indexPath: '../../templates/index.html'
};
```
#### Na co dávat pozor:
- Správná cesta k souborům a synchronizace s Django šablonami.

### 5. API integrace
#### Proč:
- Pro komunikaci mezi frontendem (Vue) a backendem (Django).
#### Jak:
- Vytvoř API pomocí Django REST framework.
- Ve Vue použij Axios nebo jiný HTTP klient pro komunikaci s API.
```javascript
import axios from 'axios';
axios.get('/api/your-endpoint')
  .then(response => {
    console.log(response.data);
  });
```
#### Na co dávat pozor:
- Správné nastavení URL a bezpečnostní politiky (CORS).

### 6. Vývoj a ladění
#### Proč:
- Aby byla aplikace optimalizovaná a bez chyb.
#### Jak:
- Používej Vue DevTools pro ladění Vue komponent.
- Django debug mód pro backend.
#### Na co dávat pozor:
- Zabezpečení a výkon aplikace, odhalení a oprava chyb.

### 7. Nasazení do produkce
#### Proč:
- Aby aplikace byla dostupná uživatelům.
#### Jak:
- Nastav produkční build pro Vue.
- Nasazení statických souborů a API na produkční server.
```bash
npm run build
```
- Aktualizuj `settings.py` pro produkční prostředí.
```python
DEBUG = False
ALLOWED_HOSTS = ['yourdomain.com']
```
#### Na co dávat pozor:
- Zabezpečení, výkon a správné nastavení produkčního prostředí.

Toto jsou základní kroky pro integraci Vue.js do Django projektu. Jakmile budeš mít stromovou strukturu projektu, můžeme se podívat na konkrétní kroky a detaily. Pokud máš nějaké otázky nebo nejasnosti, neváhej se zeptat!

[*zpět na obsah*](#obsah) 
## Dotaz:

OK, začneme tedy prvním bodem. Zde mi vypiš vše co bych měl zkontrolovat a jak. K práci používám PyCharm a terminál powershlell a dle mého Node.js a npm nainstalované mám, ale i to bych chtěl ověřit.

## ChatGPT:

Skvěle, pojďme začít prvním bodem – přípravou prostředí. Zde jsou podrobné kroky, které bys měl následovat, včetně kontroly aktuální instalace Node.js a npm.

### 1.1 Ověření instalace Node.js a npm
Nejprve ověřme, že máš Node.js a npm správně nainstalované.

#### 1.1.1 Kontrola verze Node.js a npm
Otevři terminál PowerShell a spusť následující příkazy:

```powershell
node -v
npm -v
```

Tyto příkazy by měly vrátit verze nainstalovaného Node.js a npm. Výstup by měl vypadat například takto:

```
v16.15.0
7.20.3
```

Pokud jsou verze uvedeny, máš Node.js a npm správně nainstalované.

#### 1.1.2 Instalace nebo aktualizace Node.js a npm
Pokud nejsou nainstalované nebo chceš aktualizovat na nejnovější verzi, stáhni si instalátor z oficiální stránky Node.js: [Node.js ke stažení](https://nodejs.org/).

### 1.2 Instalace Vue CLI
Vue CLI je nástroj pro rychlou a jednoduchou tvorbu Vue projektů.

#### 1.2.1 Instalace Vue CLI
V terminálu PowerShell spusť následující příkaz:

```powershell
npm install -g @vue/cli
```

Tento příkaz nainstaluje Vue CLI globálně. Ověření úspěšné instalace můžeš provést pomocí:

```powershell
vue --version
```

Tento příkaz by měl vrátit verzi Vue CLI, například `4.5.13`.

### 1.3 Vytvoření Vue projektu
Nyní, když máš nainstalované potřebné nástroje, můžeme vytvořit nový Vue projekt.

#### 1.3.1 Vytvoření nového Vue projektu
V terminálu PowerShell přejdi do složky, kde chceš vytvořit svůj Vue projekt, například:

```powershell
cd C:\path\to\your\django\project
```

Spusť příkaz pro vytvoření nového Vue projektu:

```powershell
vue create frontend
```

Tento příkaz spustí průvodce, který tě provede nastavením projektu. Můžeš si vybrat z přednastavených šablon nebo si přizpůsobit projekt podle svých potřeb.

#### 1.3.2 Přesun Vue projektu do Django složky
Po vytvoření projektu můžeš složku `frontend` přesunout do struktury tvého Django projektu, pokud již není ve správné složce.

```powershell
mv frontend C:\path\to\your\django\project\
```

### 1.4 Kontrola funkčnosti Vue projektu
#### 1.4.1 Spuštění vývojového serveru
Přejdi do složky s Vue projektem a spusť vývojový server, abys ověřil, že vše funguje správně:

```powershell
cd C:\path\to\your\django\project\frontend
npm run serve
```

Pokud vše proběhne správně, vývojový server by měl být spuštěn a ty bys měl vidět výstup podobný tomuto:

```
App running at:
- Local:   http://localhost:8080/
- Network: http://192.168.1.10:8080/
```

Otevři prohlížeč a zadej adresu `http://localhost:8080/`, kde bys měl vidět úvodní stránku Vue projektu.

### Shrnutí
Tímto jsme dokončili první bod a ověřili, že máš správně nastavené prostředí pro práci s Vue.js. Pokud máš jakékoli otázky nebo narazíš na problém, dej vědět!

[*zpět na obsah*](#obsah) 
## Dotaz:

Když jsem si skoušel dělat nějaké projekty ve VUE podle výukových videí, tk tam se začínalo příkazem: npm instal vue@latest

Ten tedy v tomto procesu nepoužiji?
Zajímalo by mě proč a kdy je to dobré použít? 


## ChatGPT:

Příkaz `npm install vue@latest` je určen k instalaci nejnovější verze Vue.js do projektu. Tento příkaz je užitečný v určitých scénářích, například když potřebuješ aktualizovat stávající projekt nebo pokud chceš ručně spravovat závislosti projektu bez použití Vue CLI. 

### Kdy použít `npm install vue@latest`
1. **Aktualizace stávajícího projektu**:
   - Pokud máš již existující projekt a chceš aktualizovat Vue.js na nejnovější verzi, můžeš použít tento příkaz.
   
2. **Ruční správa závislostí**:
   - Pokud nechceš použít Vue CLI a chceš si nastavit projekt ručně (např. pro specifické potřeby nebo pokročilou konfiguraci), můžeš tímto příkazem nainstalovat Vue.js jako závislost.

3. **Jednoduché projekty**:
   - Pro velmi jednoduché projekty, kde nepotřebuješ všechny funkce a nastavení, které Vue CLI nabízí.

### Proč nepoužívat `npm install vue@latest` v procesu s Vue CLI
Vue CLI poskytuje kompletní řešení pro vytváření a správu Vue.js projektů s mnoha výhodami:
- **Scaffoldování projektů**: Vue CLI automaticky nastaví všechny potřebné konfigurace pro nový projekt.
- **Pluginy a přednastavení**: Vue CLI umožňuje snadnou integraci s různými pluginy a knihovnami (např. Vue Router, Vuex).
- **Vývojový server**: Vue CLI poskytuje hotový vývojový server s hot-reloadingem.
- **Jednoduchá správa**: Vue CLI zjednodušuje správu závislostí a projektových nastavení.

### Proces s Vue CLI (jak jsme jej popsali dříve):
1. **Instalace Vue CLI**:
   ```powershell
   npm install -g @vue/cli
   ```
2. **Vytvoření nového Vue projektu**:
   ```powershell
   vue create frontend
   ```

### Jak integrovat `npm install vue@latest` do ručního procesu
Pokud chceš ručně nastavit projekt bez Vue CLI, postupoval bys takto:

1. **Inicializace projektu**:
   ```powershell
   mkdir my-vue-project
   cd my-vue-project
   npm init -y
   ```

2. **Instalace Vue.js**:
   ```powershell
   npm install vue@latest
   ```

3. **Vytvoření struktury projektu**:
   - Vytvoř složky `src` a `public`.
   - V `src` vytvoř soubory `index.html` a `main.js`.

4. **Konfigurace webpacku (nebo jiného bundleru)**:
   - Vytvoř konfigurační soubory pro webpack.

5. **Nastavení vývojového serveru**:
   - Přidání skriptů do `package.json`.

Tento ruční proces je flexibilnější, ale také složitější a náchylnější k chybám. Proto se ve většině případů doporučuje použití Vue CLI, pokud nejsou specifické důvody pro ruční nastavení.

### Shrnutí
Příkaz `npm install vue@latest` je užitečný pro ruční správu závislostí nebo aktualizaci stávajících projektů. Pro nové projekty je však použití Vue CLI doporučeno, protože poskytuje robustní nástroje a usnadňuje celý proces vývoje.

[*zpět na obsah*](#obsah)      
## Dotaz:

Proč jsem dostal tuto chybu:
      
      PS C:\Users\Sudip2708\Dropbox\__DjangoBlogWeb> vue create frontend
      vue : File C:\Users\Sudip2708\AppData\Roaming\npm\vue.ps1 cannot be loaded. The file C:\Users\Sudip27
      08\AppData\Roaming\npm\vue.ps1 is not digitally signed. You cannot run this script on the current sys
      tem. For more information about running scripts and setting execution policy, see about_Execution_Pol
      icies at https:/go.microsoft.com/fwlink/?LinkID=135170.
      At line:1 char:1
      + vue create frontend
        + ~~~
            + CategoryInfo          : SecurityError: (:) [], PSSecurityException
            + FullyQualifiedErrorId : UnauthorizedAccess
      PS C:\Users\Sudip2708\Dropbox\__DjangoBlogWeb>


## ChatGPT:

Tato chyba je způsobena nastavením politiky PowerShellu na vašem systému. PowerShell má bezpečnostní mechanismus, který omezuje spouštění skriptů, které nejsou digitálně podepsány. 

Pro vyřešení tohoto problému je potřeba změnit politiku spouštění skriptů (Execution Policy) v PowerShellu. Níže jsou kroky, jak to udělat:

### 1. Otevření PowerShellu jako administrátor
- Klikni pravým tlačítkem na ikonu PowerShellu a vyber "Run as administrator".

### 2. Kontrola aktuální politiky
Spusť následující příkaz, abys zjistil aktuální politiku:

```powershell
Get-ExecutionPolicy
```

### 3. Změna politiky spouštění skriptů
Pro změnu politiky na méně restriktivní hodnotu, spusť následující příkaz:

```powershell
Set-ExecutionPolicy RemoteSigned
```

Pokud tě PowerShell požádá o potvrzení, stiskni "Y" a poté "Enter".

### 4. Opakování příkazu Vue CLI
Po změně politiky by měl být příkaz `vue create` spustitelný. Zkus znovu spustit příkaz:

```powershell
vue create frontend
```

### Poznámka
Nastavení politiky `RemoteSigned` umožňuje spouštění skriptů, které jsou lokálně vytvořené bez digitálního podpisu, ale vyžaduje digitální podpis pro skripty stažené z internetu. To je bezpečnější než nastavení `Unrestricted`, které povoluje spouštění všech skriptů.

Pokud bys chtěl vrátit politiku zpět na původní hodnotu po dokončení práce, můžeš použít příkaz:

```powershell
Set-ExecutionPolicy Restricted
```

### Shrnutí
Chyba, kterou jsi obdržel, je způsobena omezením spouštění skriptů v PowerShellu. Nastavením politiky spouštění na `RemoteSigned` můžeš tento problém vyřešit a pokračovat v práci s Vue CLI. Pokud máš jakékoli další otázky nebo narazíš na další problémy, dej vědět!

[*zpět na obsah*](#obsah) 
## Dotaz:

Můžeš mi napsat manuál pro průvodce pro vytvoření fronendu, který se mi objeví po zadání příkazu: vue create frontend

Tak abych věděl co jednotlivé možnosti znamenají a podle čeho se mám rozhodnout?

## ChatGPT:

Samozřejmě! Průvodce `vue create` nabízí několik možností konfigurace nového Vue projektu. Pojďme si projít jednotlivé kroky a možnosti, které se ti zobrazí, abys mohl lépe pochopit, co každá možnost znamená a jak se rozhodnout.

### Krok 1: Výběr šablony (Preset)
Když zadáš příkaz `vue create frontend`, první otázka, kterou uvidíš, bude:

```
? Please pick a preset:
  Default ([Vue 3] babel, eslint)
  Default (Vue 2) ([Vue 2] babel, eslint)
  Manually select features
```

**Výběr**:
- **Default ([Vue 3] babel, eslint)**: Vytvoří projekt s Vue 3, Babel (pro transpilaci kódu) a ESLint (pro kontrolu kódu).
- **Default (Vue 2) ([Vue 2] babel, eslint)**: Vytvoří projekt s Vue 2, Babel a ESLint.
- **Manually select features**: Umožní ti ručně vybrat konkrétní funkce, které chceš zahrnout do projektu.

Pokud chceš mít plnou kontrolu nad tím, co bude v projektu zahrnuto, zvol **Manually select features**.

### Krok 2: Výběr funkcí
Pokud zvolíš "Manually select features", dostaneš další otázku:

```
? Check the features needed for your project:
 ◉ Choose Vue version
 ◉ Babel
 ◉ TypeScript
 ◉ Progressive Web App (PWA) Support
 ◉ Router
 ◉ Vuex
 ◉ CSS Pre-processors
 ◉ Linter / Formatter
 ◉ Unit Testing
 ◉ E2E Testing
```

**Výběr**:
- **Choose Vue version**: Umožňuje vybrat mezi Vue 2 a Vue 3.
- **Babel**: Přidává Babel pro transpilaci kódu.
- **TypeScript**: Přidává podporu TypeScriptu.
- **Progressive Web App (PWA) Support**: Přidává podporu pro Progressive Web Apps.
- **Router**: Přidává Vue Router pro směrování v aplikaci.
- **Vuex**: Přidává Vuex pro správu stavu aplikace.
- **CSS Pre-processors**: Umožňuje výběr předprocesoru pro CSS (např. Sass, Less).
- **Linter / Formatter**: Přidává nástroje pro lintování a formátování kódu (např. ESLint, Prettier).
- **Unit Testing**: Přidává podporu pro jednotkové testování.
- **E2E Testing**: Přidává podporu pro end-to-end testování.

Zaškrtni funkce, které chceš zahrnout, a pokračuj stisknutím Enter.

### Krok 3: Výběr verze Vue
Pokud jsi zaškrtl možnost "Choose Vue version":

```
? Choose a version of Vue.js that you want to start the project with
  3.x
  2.x
```

**Výběr**:
- **3.x**: Vyber, pokud chceš použít nejnovější verzi Vue (doporučeno pro nové projekty).
- **2.x**: Vyber, pokud chceš použít starší verzi Vue (pro projekty, které musí být kompatibilní s Vue 2).

### Krok 4: Další specifické volby
Další otázky budou záviset na funkcích, které jsi vybral. Některé z nich mohou být například:

#### Babel
Žádné další otázky, Babel bude přidán automaticky.

#### TypeScript
```
? Use class-style component syntax?
  Yes
  No
```
- **Yes**: Použití syntaxi tříd (class-style) pro komponenty v TypeScriptu.
- **No**: Nepoužívat tuto syntaxi.

#### Progressive Web App (PWA) Support
Obvykle žádné další otázky, podpora PWA bude přidána.

#### Router
```
? Use history mode for router? (Requires proper server setup for index fallback in production)
  Yes
  No
```
- **Yes**: Použije `history` mód pro router (čistší URL, ale vyžaduje správné nastavení serveru).
- **No**: Použije výchozí hash mód (`#/` v URL).

#### Vuex
Obvykle žádné další otázky, Vuex bude přidán.

#### CSS Pre-processors
```
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default):
  Sass/SCSS (with node-sass)
  Sass/SCSS (with dart-sass)
  Less
  Stylus
```
Vyber si preferovaný CSS pre-procesor.

#### Linter / Formatter
```
? Pick a linter / formatter config:
  ESLint with error prevention only
  ESLint + Airbnb config
  ESLint + Standard config
  ESLint + Prettier
```
Vyber si konfiguraci pro lintování a formátování kódu.

Další otázky se mohou týkat automatického lintování kódu při commitu a podobně.

#### Unit Testing
```
? Pick a unit testing solution:
  Mocha + Chai
  Jest
```
Vyber si preferované řešení pro jednotkové testování.

#### E2E Testing
```
? Pick an E2E testing solution:
  Cypress
  Nightwatch
```
Vyber si preferované řešení pro end-to-end testování.

### Krok 5: Uložení nastavení jako šablonu (preset)
```
? Save this as a preset for future projects?
  Yes
  No
```
- **Yes**: Uloží vybranou konfiguraci jako šablonu pro budoucí projekty.
- **No**: Nepřidá šablonu.

### Shrnutí
Tento průvodce ti umožňuje přizpůsobit nový Vue projekt podle tvých potřeb a preferencí. Každá možnost ti umožňuje rozhodnout se pro konkrétní funkce a nástroje, které chceš mít ve svém projektu. Výběr závisí na konkrétních požadavcích tvého projektu a tvých preferencích v oblasti vývojových nástrojů. Pokud máš nějaké další otázky nebo potřebuješ další vysvětlení, dej vědět!

[*zpět na obsah*](#obsah) 
## Dotaz:

Podle čeho se mám rozhodnout v tomto kroku:

      ? Check the features needed for your project: (Press <space> to select, <a> to toggle all, <i> to
      invert selection, and <enter> to proceed)
      >(*) Babel
       ( ) TypeScript
       ( ) Progressive Web App (PWA) Support
       ( ) Router
       ( ) Vuex
       ( ) CSS Pre-processors
       (*) Linter / Formatter
       ( ) Unit Testing
       ( ) E2E Testing

Jaké možnosti bych mě vybrat? Dej mi i nějaké hypotetické příklady podle kterých bych spíše pochopil, zda je to třeba.

## ChatGPT:

Rozhodnutí, které funkce zvolit při vytváření nového Vue projektu, závisí na konkrétních požadavcích tvého projektu a na tom, jaké nástroje a technologie preferuješ používat. Níže jsou podrobné popisy jednotlivých možností a hypotetické příklady situací, kdy by bylo vhodné je zahrnout.

### 1. Babel
**Význam**: Babel je transpilátor, který umožňuje psát moderní JavaScript (ES6+) a převádět ho do starších verzí JavaScriptu, které podporují všechny prohlížeče.
**Doporučení**: Většinou bys měl Babel zahrnout, protože ti umožňuje používat moderní syntaxi JavaScriptu.

### 2. TypeScript
**Význam**: TypeScript je nadmnožina JavaScriptu, která přidává statické typování a další funkce. Pomáhá předcházet chybám a zlepšuje čitelnost kódu.
**Doporučení**:
- **Vyber TypeScript** pokud preferuješ statické typování a máš zkušenosti s TypeScriptem nebo pokud pracuješ na velkém projektu, kde je důležité předcházet typovým chybám.
- **Nevyber TypeScript** pokud dáváš přednost čistému JavaScriptu nebo pracuješ na menším projektu, kde nejsou typové chyby tak časté.

**Hypotetický příklad**:
- **Velký firemní projekt**: Vyber TypeScript, protože pomáhá udržovat čitelný a bezpečný kód.
- **Malý osobní projekt**: Můžeš zůstat u JavaScriptu, aby byla práce jednodušší a rychlejší.

### 3. Progressive Web App (PWA) Support
**Význam**: PWA umožňuje vytvořit aplikaci, která se chová jako nativní aplikace na mobilních zařízeních, včetně offline režimu a rychlého načítání.
**Doporučení**:
- **Vyber PWA Support** pokud plánuješ, aby tvá aplikace byla použitelná offline a měla funkce podobné nativním mobilním aplikacím.
- **Nevyber PWA Support** pokud je tvůj projekt jednoduchý web a nepotřebuje pokročilé funkce PWA.

**Hypotetický příklad**:
- **Aplikace pro správu úkolů**: Vyber PWA Support, protože by mohla být použita offline a mít rychlý přístup.
- **Jednoduchý blog**: PWA Support není potřeba, protože blog nepotřebuje offline režim.

### 4. Router
**Význam**: Vue Router umožňuje vytvářet jednosměrové aplikace s více stránkami bez nutnosti znovunačítání stránky.
**Doporučení**:
- **Vyber Router** pokud plánuješ více stránek nebo sekcí v aplikaci.
- **Nevyber Router** pokud tvoje aplikace bude mít jen jednu stránku.

**Hypotetický příklad**:
- **E-shop**: Vyber Router, protože bude mít více stránek (produkty, košík, pokladna, atd.).
- **Jednostránková landing page**: Router není potřeba.

### 5. Vuex
**Význam**: Vuex je knihovna pro správu stavu v aplikacích Vue. Umožňuje centrálně spravovat stav a usnadňuje sdílení dat mezi komponentami.
**Doporučení**:
- **Vyber Vuex** pokud tvoje aplikace bude mít komplexní stav, který je potřeba sdílet mezi mnoha komponentami.
- **Nevyber Vuex** pokud tvoje aplikace je jednoduchá a nepotřebuje složitou správu stavu.

**Hypotetický příklad**:
- **Sociální síť**: Vyber Vuex, protože budeš mít hodně sdíleného stavu (uživatelská data, příspěvky, komentáře).
- **Jednoduchý kontaktní formulář**: Vuex není potřeba.

### 6. CSS Pre-processors
**Význam**: CSS pre-processory jako Sass, Less nebo Stylus přidávají do CSS pokročilé funkce jako proměnné, zanořování a mixiny.
**Doporučení**:
- **Vyber CSS Pre-processors** pokud chceš využívat pokročilé funkce pro psaní stylů.
- **Nevyber CSS Pre-processors** pokud preferuješ čisté CSS nebo pracuješ na malém projektu, kde nejsou pokročilé funkce potřeba.

**Hypotetický příklad**:
- **Velká aplikace s komplexním designem**: Vyber CSS Pre-processors, protože ti to usnadní správu a opakované použití stylů.
- **Jednoduchý blog**: CSS Pre-processors nejsou nutné.

### 7. Linter / Formatter
**Význam**: ESLint a další linters pomáhají udržovat konzistentní styl kódu a předcházet chybám. Formátory jako Prettier automaticky formátují kód podle stanovených pravidel.
**Doporučení**:
- **Vyber Linter / Formatter** pokud chceš udržovat konzistentní styl kódu a předcházet chybám.
- **Nevyber Linter / Formatter** pokud preferuješ psát kód bez těchto nástrojů (nedoporučuje se).

**Hypotetický příklad**:
- **Každý projekt**: Vyber Linter / Formatter, protože ti to pomůže udržovat kvalitní a konzistentní kód.

### 8. Unit Testing
**Význam**: Jednotkové testování umožňuje psát testy pro jednotlivé části aplikace, což usnadňuje odhalování chyb a zajišťuje správnou funkčnost.
**Doporučení**:
- **Vyber Unit Testing** pokud chceš psát testy pro svou aplikaci a zajišťovat její správnou funkčnost.
- **Nevyber Unit Testing** pokud nechceš psát testy nebo pracuješ na malém projektu, kde testování není prioritou.

**Hypotetický příklad**:
- **Podniková aplikace**: Vyber Unit Testing, protože je důležité zajišťovat stabilitu a správnost aplikace.
- **Prototyp nebo osobní projekt**: Unit Testing není nutný, pokud nepotřebuješ vysokou míru testování.

### 9. E2E Testing
**Význam**: End-to-end testování simuluje skutečné uživatelské interakce a testuje celou aplikaci od začátku do konce.
**Doporučení**:
- **Vyber E2E Testing** pokud chceš zajistit, že celá aplikace funguje správně z pohledu uživatele.
- **Nevyber E2E Testing** pokud nechceš psát E2E testy nebo pracuješ na malém projektu, kde to není prioritou.

**Hypotetický příklad**:
- **E-commerce aplikace**: Vyber E2E Testing, protože je důležité, aby všechny funkce fungovaly správně.
- **Jednoduchý blog**: E2E Testing není nutný.

### Shrnutí
Rozhodnutí závisí na povaze tvého projektu, velikosti týmu, rozsahu aplikace a tvých preferencích v oblasti nástrojů a technologií. Zde je několik doporučení na základě typických projektů:

- **Malý osobní projekt nebo prototyp**: Vyber jen základní funkce (Babel, Linter / Formatter).
- **Středně velký projekt**: Vyber Babel, Linter / Formatter, Router, Vuex, možná CSS Pre-processors.
- **Velký firemní projekt**: Vyber všechny relevantní funkce (Babel, TypeScript, PWA, Router, Vuex, CSS Pre-processors, Linter / Formatter, Unit Testing, E2E Testing).

Při rozhodování se zaměř na potřeby tvého projektu a na to, co ti pomůže dosáhnout nejlepších výsledků. Pokud máš další otázky nebo potřebuješ další rady, dej vědět!

[*zpět na obsah*](#obsah) 
## Dotaz:

Dají se tyto možnosti později zapnout, když je teď nevyberu?

## ChatGPT:

Ano, většina funkcí a knihoven, které můžeš zvolit při vytváření nového Vue projektu, se dá přidat později. Zde je přehled, jak můžeš tyto funkce přidat do svého projektu, pokud je nyní nevybereš:

### 1. Babel
Babel je obvykle součástí základní konfigurace vytvořeného projektu. Pokud by nebyl, můžeš ho přidat takto:
```sh
npm install @babel/core @babel/preset-env babel-loader --save-dev
```
Pak přidej konfiguraci do `babel.config.js`.

### 2. TypeScript
TypeScript můžeš přidat později pomocí Vue CLI:
```sh
vue add typescript
```
Tento příkaz přidá potřebné závislosti a vytvoří nebo aktualizuje konfigurační soubory.

### 3. Progressive Web App (PWA) Support
Podporu pro PWA můžeš přidat pomocí Vue CLI pluginu:
```sh
vue add pwa
```
Tento příkaz přidá potřebné soubory a konfiguraci pro PWA.

### 4. Router
Vue Router můžeš přidat později pomocí:
```sh
vue add router
```
Tento příkaz vytvoří soubory a nastaví Vue Router v tvém projektu.

### 5. Vuex
Vuex můžeš přidat pomocí Vue CLI:
```sh
vue add vuex
```
Tento příkaz přidá Vuex do projektu a vytvoří základní konfiguraci.

### 6. CSS Pre-processors
CSS pre-processory můžeš přidat ručně. Například pro Sass:
```sh
npm install sass-loader sass --save-dev
```
Pak uprav konfiguraci webpacku v projektu tak, aby používala Sass.

### 7. Linter / Formatter
Linting a formátování můžeš přidat pomocí Vue CLI:
```sh
vue add eslint
```
Tento příkaz přidá ESLint a nastaví jeho konfiguraci.

### 8. Unit Testing
Jednotkové testování můžeš přidat později pomocí Vue CLI:
```sh
vue add @vue/unit-jest
```
Nebo pro Mocha + Chai:
```sh
vue add @vue/unit-mocha
```
Tento příkaz přidá potřebné závislosti a konfigurační soubory.

### 9. E2E Testing
End-to-end testování můžeš přidat pomocí Vue CLI:
```sh
vue add @vue/e2e-cypress
```
Nebo pro Nightwatch:
```sh
vue add @vue/e2e-nightwatch
```
Tento příkaz přidá potřebné závislosti a konfigurační soubory.

### Shrnutí
Vue CLI poskytuje pluginy, které umožňují přidat většinu funkcí později. Použití příkazu `vue add <plugin>` přidá požadované funkce do existujícího projektu. Můžeš tak začít s jednodušším projektem a přidávat další funkce podle toho, jak se projekt vyvíjí a jak rostou jeho požadavky.

Pokud máš konkrétní otázky nebo potřebuješ podrobnější návod pro přidání některých z těchto funkcí, dej vědět!

[*zpět na obsah*](#obsah) 
## Dotaz:

Napiš mi ještě ke každé volbě features, co se jejím vybráním mění pro mě. Například když bych vybral TypeScript, v čem by se mi změnil zápis kódu a uveď mi i nějaké příklady.

## ChatGPT:

Jasně, pojďme si projít, co konkrétně se změní pro tebe, když vybereš jednotlivé volby při vytváření Vue projektu, a jaké budou mít praktické důsledky na zápis a strukturu kódu.

### 1. Babel
**Co se změní:**
- Babel ti umožní psát moderní JavaScript (ES6+) a automaticky transpilovat kód do verze JavaScriptu podporované staršími prohlížeči.
- V `package.json` bude přidána závislost na Babel a vytvoří se konfigurační soubor `babel.config.js`.

**Příklad kódu:**
Můžeš používat moderní syntax:
```javascript
// ES6+ syntax
const greet = () => {
  console.log('Hello, World!');
};
greet();
```
Babel to převede na kompatibilní verzi:
```javascript
var greet = function() {
  console.log('Hello, World!');
};
greet();
```

### 2. TypeScript
**Co se změní:**
- Kód bude psán v TypeScriptu, což znamená přidání typů a statické kontroly typů.
- `tsconfig.json` bude přidán do projektu pro konfiguraci TypeScriptu.
- `.vue` soubory budou obsahovat sekci `<script lang="ts">`.

**Příklad kódu:**
JavaScript:
```javascript
export default {
  data() {
    return {
      message: 'Hello, World!'
    };
  },
  methods: {
    greet() {
      console.log(this.message);
    }
  }
};
```
TypeScript:
```typescript
export default {
  data(): { message: string } {
    return {
      message: 'Hello, World!'
    };
  },
  methods: {
    greet(): void {
      console.log(this.message);
    }
  }
};
```

### 3. Progressive Web App (PWA) Support
**Co se změní:**
- Projekt bude mít konfiguraci a soubory potřebné pro PWA, jako je `manifest.json` a `service-worker.js`.
- Budeš moci využívat offline funkce, rychlejší načítání a další PWA vlastnosti.

**Příklad kódu:**
Service worker (automaticky generován):
```javascript
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request).then(function(response) {
      return response || fetch(event.request);
    })
  );
});
```

### 4. Router
**Co se změní:**
- Přidá se Vue Router, což umožní vytvářet více stránek v jedné aplikaci.
- Vytvoří se soubor `src/router/index.js` s konfigurací routeru.
- V hlavním souboru `main.js` bude importován a použit Vue Router.

**Příklad kódu:**
```javascript
// src/router/index.js
import Vue from 'vue';
import Router from 'vue-router';
import Home from '../views/Home.vue';

Vue.use(Router);

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: () => import('../views/About.vue')
  }
];

export default new Router({
  mode: 'history',
  routes
});
```

### 5. Vuex
**Co se změní:**
- Přidá se Vuex, což umožní centrální správu stavu aplikace.
- Vytvoří se soubor `src/store/index.js` s konfigurací Vuex.

**Příklad kódu:**
```javascript
// src/store/index.js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++;
    }
  },
  actions: {
    increment({ commit }) {
      commit('increment');
    }
  },
  getters: {
    count: state => state.count
  }
});
```

### 6. CSS Pre-processors
**Co se změní:**
- Přidají se závislosti a konfigurace pro vybraný CSS pre-processor (např. Sass, Less, Stylus).
- `.vue` soubory mohou používat sekci `<style lang="scss">` (pro Sass) nebo jiný syntax podle zvoleného pre-processoru.

**Příklad kódu:**
```scss
// Sass (SCSS) syntax
$primary-color: #42b983;

body {
  background-color: $primary-color;
}
```

### 7. Linter / Formatter
**Co se změní:**
- Přidá se ESLint nebo jiný linter a formatter.
- Vytvoří se konfigurační soubory jako `.eslintrc.js`.
- Budeš mít automatickou kontrolu kódu podle zvolených pravidel.

**Příklad kódu:**
```javascript
// Kód před lintováním
let x = 42;
console.log( x );
```
```javascript
// Kód po lintování s ESLint + Prettier
const x = 42;
console.log(x);
```

### 8. Unit Testing
**Co se změní:**
- Přidají se závislosti a konfigurace pro jednotkové testování (např. Jest, Mocha).
- Vytvoří se složka `tests/unit` s příklady testů.

**Příklad kódu:**
```javascript
// Jest unit test example
import { shallowMount } from '@vue/test-utils';
import HelloWorld from '@/components/HelloWorld.vue';

describe('HelloWorld.vue', () => {
  it('renders props.msg when passed', () => {
    const msg = 'new message';
    const wrapper = shallowMount(HelloWorld, {
      propsData: { msg }
    });
    expect(wrapper.text()).toMatch(msg);
  });
});
```

### 9. E2E Testing
**Co se změní:**
- Přidají se závislosti a konfigurace pro end-to-end testování (např. Cypress, Nightwatch).
- Vytvoří se složka `tests/e2e` s příklady testů.

**Příklad kódu:**
```javascript
// Cypress end-to-end test example
describe('My First Test', () => {
  it('Visits the app root url', () => {
    cy.visit('/');
    cy.contains('h1', 'Welcome to Your Vue.js App');
  });
});
```

### Shrnutí
Výběr jednotlivých funkcí při vytváření Vue projektu ovlivní, jak budeš psát a organizovat svůj kód. Pokud některé funkce nevybereš nyní, můžeš je přidat později pomocí Vue CLI nebo ručně. Výběr závisí na potřebách a složitosti tvého projektu.

[*zpět na obsah*](#obsah) 
## Dotaz:

Takže z toho co píšeš jsem si udělal závěr:

 (*) Babel >>> Potřebuji
 ( ) TypeScript >>> Přidává víc práce a  asi není tolik potřeba 
 ( ) Progressive Web App (PWA) Support >>> Projekt je webová aplikace, takže nevím, jestli potřebuji nativní off line ovládání
 (*) Router >>> Potřebuji - aplikace má mnoho stránek
 (?) Vuex >>> Tady tedy vůbec nevím, zda jej pro svůj projekt potřebuji. Řekni mi ještě něco více podle čeho bych se mohl rozhodnout.
 (?) CSS Pre-processors >>> Tady tedy vůbec nevím, zda jej pro svůj projekt potřebuji. Řekni mi ještě něco více podle čeho bych se mohl rozhodnout.
 (*) Linter / Formatter >>> Potřebuji
 ( ) Unit Testing >>> Zatím asi nepotřebuji příjdou na řadu až zvládnu základ.
 ( ) E2E Testing >>> Zatím asi nepotřebuji příjdou na řadu až zvládnu základ.

Co ty na to?

## ChatGPT:

Tvůj závěr dává smysl a je vidět, že jsi dobře zvážil své potřeby. Pojďme si ještě detailněji projít Vuex a CSS Pre-processors, abys měl dostatek informací pro rozhodnutí.

### Vuex
**Vuex** je užitečný, když máš složitější aplikaci, kde je třeba centrálně spravovat stav aplikace (data, která se používají na různých místech aplikace).

**Příklady, kdy bys mohl potřebovat Vuex**:
- **Komplexní data**: Pokud máš složitější datové struktury nebo hodně sdílených dat mezi komponentami (např. uživatelské informace, nastavení, různé seznamy, které se používají na více stránkách).
- **Asynchronní operace**: Pokud často pracuješ s API a potřebuješ spravovat stav při načítání, ukládání nebo aktualizaci dat.
- **Stav aplikace**: Pokud potřebuješ, aby některé části aplikace reagovaly na změny v jiných částech (např. aktualizace košíku v e-shopu, kdykoliv přidáš nebo odebereš produkt).

**Příklady, kdy Vuex nepotřebuješ**:
- **Jednoduché aplikace**: Pokud je aplikace jednoduchá a každá komponenta si může spravovat svůj stav samostatně.
- **Menší projekty**: Pokud nejsou složité datové struktury nebo mnoho sdílených dat mezi komponentami.

**Rozhodnutí**:
- **Potřebuješ Vuex**: Pokud tvá aplikace zahrnuje správu složitějších dat nebo stavů sdílených mezi více komponentami.
- **Nepotřebuješ Vuex**: Pokud je aplikace relativně jednoduchá a komponenty jsou víceméně samostatné.

### CSS Pre-processors
**CSS Pre-processors** (např. Sass, Less) přidávají do CSS pokročilé funkce, které usnadňují správu stylů, zejména ve větších projektech.

**Příklady, kdy bys mohl potřebovat CSS Pre-processors**:
- **Komplexní styly**: Pokud máš hodně opakujícího se kódu, který by se dal zjednodušit pomocí proměnných, mixinů a funkcí.
- **Modulární CSS**: Pokud chceš lépe organizovat CSS pomocí zanořování a importování různých částí stylů.
- **Konzistentní design**: Pokud potřebuješ udržovat konzistentní design napříč celou aplikací pomocí proměnných pro barvy, fonty, velikosti apod.

**Příklady, kdy CSS Pre-processors nepotřebuješ**:
- **Jednoduché styly**: Pokud máš jednoduchou aplikaci s malým množstvím CSS, kde není potřeba pokročilých funkcí.
- **Malé projekty**: Pokud je projekt malý a jednoduchý, klasické CSS může být dostačující.

**Rozhodnutí**:
- **Potřebuješ CSS Pre-processors**: Pokud máš složitější styly a chceš využívat pokročilé funkce pro lepší organizaci a údržbu.
- **Nepotřebuješ CSS Pre-processors**: Pokud jsou styly jednoduché a není potřeba pokročilých funkcí.

### Shrnutí tvého závěru:
- **Babel**: Ano, potřebuješ.
- **TypeScript**: Ne, nemusíš používat.
- **PWA Support**: Ne, zatím nepotřebuješ.
- **Router**: Ano, potřebuješ.
- **Vuex**: Pokud máš složitější správu dat a stavů, doporučuji jej vybrat.
- **CSS Pre-processors**: Pokud máš složitější styly a chceš lepší organizaci, doporučuji jej vybrat.
- **Linter / Formatter**: Ano, potřebuješ.
- **Unit Testing**: Zatím ne, můžeš přidat později.
- **E2E Testing**: Zatím ne, můžeš přidat později.

Pokud stále váháš ohledně Vuex a CSS Pre-processors, můžeš je vyzkoušet v malém měřítku a zjistit, zda ti usnadňují práci. Pokud zjistíš, že je nepotřebuješ, vždy je můžeš přestat používat nebo odstranit.

[*zpět na obsah*](#obsah) 
## Dotaz:

Podle čeho se mám rozhodnout teď?

      ? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): (Use
      arrow keys)
      > Sass/SCSS (with dart-sass)
        Less
        Stylus

## Chat GPT:

Volba CSS pre-processoru závisí na tvých preferencích a potřebách projektu. Zde je přehled jednotlivých pre-processorů a několik vodítek, podle kterých se můžeš rozhodnout.

### Sass/SCSS (with dart-sass)
**Proč si vybrat:**
- **Populární**: Sass je nejpoužívanější pre-processor, má širokou podporu a velkou komunitu.
- **Funkce**: Nabízí pokročilé funkce jako proměnné, zanořování, mixiny, funkce a dědění stylů.
- **Kompatibilita**: SCSS syntaxe je rozšíření standardního CSS, takže je snadné se do ní dostat.
- **Dart-sass**: Moderní implementace Sassu, rychlá a udržovaná.

**Příklad kódu:**
```scss
$primary-color: #42b983;

body {
  background-color: $primary-color;
  h1 {
    color: darken($primary-color, 10%);
  }
}
```

**Kdy použít:**
- Pokud jsi s Sass/SCSS již obeznámen nebo máš zájem o jeho funkcionalitu.
- Pokud chceš využít široké podpory a dokumentace.
- Pokud potřebuješ silné nástroje pro práci se styly ve větších projektech.

### Less
**Proč si vybrat:**
- **Jednoduchý**: Syntaxe je jednoduchá a snadno se učí.
- **Flexibilní**: Nabízí podobné funkce jako Sass, ale s trochu odlišným přístupem.
- **Integrace**: Často používán v některých knihovnách jako Bootstrap (i když Bootstrap nyní podporuje i Sass).

**Příklad kódu:**
```less
@primary-color: #42b983;

body {
  background-color: @primary-color;
  h1 {
    color: darken(@primary-color, 10%);
  }
}
```

**Kdy použít:**
- Pokud preferuješ jednodušší syntax a přístup.
- Pokud pracuješ na projektu, který již používá Less.

### Stylus
**Proč si vybrat:**
- **Minimalistický**: Stylus nabízí velmi flexibilní a minimalistický přístup k psaní CSS.
- **Kompaktní syntax**: Umožňuje psát kód s minimem syntaxe (např. bez středníků nebo složených závorek).
- **Pokročilé funkce**: Má bohatou sadu funkcí a možností, stejně jako Sass a Less.

**Příklad kódu:**
```stylus
primary-color = #42b983

body
  background-color: primary-color
  h1
    color: darken(primary-color, 10%)
```

**Kdy použít:**
- Pokud preferuješ velmi stručný a flexibilní zápis.
- Pokud máš rád minimalistický přístup k psaní stylů.

### Rozhodnutí
- **Sass/SCSS**: Vyber, pokud chceš širokou podporu, bohaté funkce a snadnou integraci do větších projektů. Pokud nejsi rozhodnutý, Sass/SCSS je bezpečná volba díky své popularitě a podpoře.
- **Less**: Vyber, pokud preferuješ jednodušší syntax nebo pracuješ na projektu, který již používá Less.
- **Stylus**: Vyber, pokud chceš stručný zápis a minimalistický přístup k psaní CSS.

### Doporučení
Pokud nemáš silné preference nebo specifické požadavky, doporučuji vybrat **Sass/SCSS** s dart-sass, protože je nejpoužívanější a nabízí skvělou kombinaci funkcionality a podpory.

[*zpět na obsah*](#obsah) 
## Dotaz:

A podle čeho se mám rozhodnout teď:

      ? Pick a linter / formatter config: (Use arrow keys)
      > ESLint with error prevention only
        ESLint + Airbnb config
        ESLint + Standard config
        ESLint + Prettier

## ChatGPT:

Při výběru konfigurace pro linter a formatter je důležité zvážit jaké standardy a praktiky chceš ve svém projektu používat. Zde je přehled jednotlivých možností a doporučení, podle kterých se můžeš rozhodnout:

### 1. ESLint with error prevention only
**Co to je:**
- Základní konfigurace ESLint zaměřená na prevenci chyb bez specifických stylistických pravidel.

**Výhody:**
- **Minimální konfigurace**: Ideální pro malé projekty nebo pro začátečníky, kteří nechtějí hned řešit komplexní nastavení.
- **Flexibilita**: Umožňuje ti nastavit vlastní pravidla podle potřeby.

**Kdy použít:**
- Pokud chceš začít s lintingem, ale nepotřebuješ podrobná stylistická pravidla.
- Pokud plánuješ přidat nebo změnit pravidla podle specifických potřeb projektu.

### 2. ESLint + Airbnb config
**Co to je:**
- Konfigurace ESLint podle standardů Airbnb, které jsou široce používány a velmi detailní.

**Výhody:**
- **Konzistence**: Airbnb konfigurace je známá svou přísností a důrazem na čistý, konzistentní kód.
- **Kompletní sada pravidel**: Obsahuje širokou škálu pravidel pro kódování, což pomáhá udržovat vysokou kvalitu kódu.

**Kdy použít:**
- Pokud chceš dodržovat dobře definovaný a populární styl kódování.
- Pokud pracuješ v týmu nebo chceš, aby tvůj kód byl konzistentní s běžně používanými standardy.

### 3. ESLint + Standard config
**Co to je:**
- Konfigurace ESLint podle JavaScript Standard Style, který se zaměřuje na jednoduchost a konzistenci.

**Výhody:**
- **Jednoduchost**: Standard Style je známý svou jednoduchou a čistou syntaxí.
- **Minimalistický přístup**: Má méně pravidel než Airbnb, což může být výhodné pro menší projekty nebo začátečníky.

**Kdy použít:**
- Pokud chceš jednoduchou a čistou konfiguraci s minimem pravidel.
- Pokud preferuješ standardy, které se zaměřují na čistotu a snadné dodržování.

### 4. ESLint + Prettier
**Co to je:**
- Kombinace ESLint a Prettier, kde ESLint slouží k prevenci chyb a Prettier se zaměřuje na automatické formátování kódu.

**Výhody:**
- **Automatické formátování**: Prettier zajišťuje konzistentní styl formátování kódu napříč projektem.
- **Oddělené starosti**: ESLint se soustředí na prevenci chyb, zatímco Prettier se stará o formátování.

**Kdy použít:**
- Pokud chceš, aby formátování kódu bylo automatické a standardizované bez nutnosti manuálních zásahů.
- Pokud chceš oddělit pravidla pro syntaxi a stylistické konvence (Prettier pro formátování a ESLint pro kontrolu chyb).

### Doporučení
- **Pokud teď začínáš a chceš jednoduché nastavení**: Vyber **ESLint with error prevention only**. Můžeš později přidat další pravidla nebo konfigurace podle potřeby.
- **Pokud hledáš podrobná pravidla a standardy**: **ESLint + Airbnb config** je skvělou volbou pro vysoce standardizovaný a detailní přístup.
- **Pokud preferuješ jednoduchý styl**: **ESLint + Standard config** poskytuje čisté a minimalistické pravidlo bez složité konfigurace.
- **Pokud chceš kombinaci lintingu a automatického formátování**: **ESLint + Prettier** je výborná volba pro udržení formátování a kvality kódu automaticky.

### Moje doporučení
Pro většinu projektů **ESLint + Prettier** poskytuje dobrou rovnováhu mezi prevencí chyb a automatickým formátováním. Pomůže ti to udržet kód čistý a konzistentní s minimem manuální práce.

Pokud se rozhodneš pro jednu z těchto možností, budeš mít dobře nastavený linting a formátování, což ti pomůže udržet kvalitu kódu a zjednodušit vývojový proces.

[*zpět na obsah*](#obsah) 
## Dotaz:

A podle čeho se mám rozhodnout teď:

      ? Pick additional lint features: (Press <space> to select, <a> to toggle all, <i> to invert selection,
       and <enter> to proceed)
      >(*) Lint on save
       ( ) Lint and fix on commit

## ChatGPT:

Při rozhodování mezi **"Lint on save"** a **"Lint and fix on commit"** je dobré zvážit, jaký pracovní postup ti vyhovuje a jak chceš spravovat kvalitu kódu. Obě možnosti mají své výhody a nevýhody. Pojďme se na ně podívat:

### Lint on Save

**Co to je:**
- **Lint on Save** znamená, že linter (např. ESLint) automaticky zkontroluje a označí chyby v kódu, když ukládáš soubor.

**Výhody:**
- **Okamžitá zpětná vazba**: Můžeš ihned vidět chyby a problémy ve svém kódu během psaní, což usnadňuje jejich opravu v reálném čase.
- **Flexibilita**: Umožňuje ti opravit chyby okamžitě, když je detekuješ, což může být efektivní při psaní kódu.
- **Bez dodatečných kroků**: Není třeba provádět další kroky, jako je commitování, abys viděl chyby.

**Kdy použít:**
- Pokud chceš mít okamžitou zpětnou vazbu během psaní kódu.
- Pokud preferuješ mít chyby pod kontrolou a opravit je ihned, jakmile je detekuješ.

### Lint and Fix on Commit

**Co to je:**
- **Lint and Fix on Commit** znamená, že linter provede kontrolu a pokusí se automaticky opravit chyby (pokud to je možné) při provádění commitování změn do verzovacího systému (např. Git).

**Výhody:**
- **Automatické opravy**: Můžeš mít automaticky opravené chyby, které linter dokáže sám vyřešit, což ti ušetří čas.
- **Konzistence**: Pomáhá zajistit, že všechny změny v kódu splňují standardy kódu před tím, než jsou odeslány do repozitáře.
- **Vysoká úroveň kvality kódu**: Udržuje kód v konzistentním a kvalitním stavu tím, že se stará o formátování a chyby před přidáním změn do hlavní větve.

**Kdy použít:**
- Pokud chceš automaticky opravovat chyby a zajistit, že všechny commity splňují určité standardy kódu.
- Pokud preferuješ, aby byl kód čistý a splňoval standardy ještě před tím, než bude přidán do verzovacího systému.

### Doporučení

- **Pokud chceš okamžitou zpětnou vazbu a opravy**: Vyber **"Lint on Save"**. To je užitečné, pokud chceš mít chyby viditelné během psaní kódu a opravit je v reálném čase.
- **Pokud chceš automatické opravy při commitování**: Vyber **"Lint and Fix on Commit"**. To ti pomůže zajistit, že všechny změny, které jsou odesílány do repozitáře, jsou automaticky opravené a splňují standardy.

### Moje doporučení

Pokud chceš mít co nejméně rušení při psaní kódu a zároveň mít zajištěnou kvalitu kódu při odesílání změn, můžeš kombinovat oba přístupy:

- **"Lint on Save"**: Aktivní během vývoje pro okamžité feedback a opravy.
- **"Lint and Fix on Commit"**: Automaticky se postará o to, aby všechny změny v repozitáři splnily kódové standardy a byly opravené.

Pokud máš pouze jednu možnost, **"Lint and Fix on Commit"** může být výhodná, pokud chceš mít zajištěno, že všechny změny, které jsou odeslány do verzovacího systému, jsou správně formátované a bez chyb.

[*zpět na obsah*](#obsah) 
## Dotaz:

A co teď:

      ? Where do you prefer placing config for Babel, ESLint, etc.?
      > In dedicated config files
        In package.json

## ChatGPT:

Při rozhodování o umístění konfiguračních souborů pro Babel, ESLint a další nástroje, je důležité zvážit, jaký způsob integrace do projektu bude pro tebe nejpraktičtější a jaké jsou výhody a nevýhody každé možnosti. Zde jsou klíčové body pro obě možnosti:

### 1. **In Dedicated Config Files**

**Co to je:**
- Konfigurační soubory pro nástroje jako Babel, ESLint, atd. jsou umístěny v samostatných souborech v kořenovém adresáři projektu, např. `.babelrc`, `.eslintrc.js`, `.prettierrc`, atd.

**Výhody:**
- **Jasná struktura**: Každý nástroj má svůj vlastní konfigurační soubor, což může usnadnit správu a údržbu.
- **Jednoduchá údržba**: Snadno se vyhledávají a upravují nastavení pro jednotlivé nástroje, bez nutnosti procházet jeden velký soubor (např. `package.json`).
- **Flexibilita**: Můžeš mít různé konfigurační soubory pro různé prostředí nebo nastavení, což může být užitečné v komplexnějších projektech.

**Nevýhody:**
- **Více souborů**: Může vést k většímu množství souborů v projektu, což může být méně přehledné, pokud je projekt malý.

### 2. **In package.json**

**Co to je:**
- Konfigurační nastavení jsou umístěna přímo v souboru `package.json` pod speciálními klíči (např. `"eslint"`, `"babel"`, `"prettier"`).

**Výhody:**
- **Jednoduchost**: Méně souborů v projektu, všechny konfigurace jsou uloženy na jednom místě, což může být jednodušší při správě malých projektů.
- **Konzistence**: Všechny konfigurace jsou součástí jednoho souboru, což může usnadnit přenos projektu a jeho nastavení.
- **Rychlý přístup**: Snadno vidíš všechna nastavení projektu na jednom místě bez nutnosti otevírat více souborů.

**Nevýhody:**
- **Možná nepřehlednost**: Pokud máš mnoho konfiguračních možností, soubor `package.json` může být přeplněný a méně přehledný.
- **Omezená flexibilita**: Pokud máš složitější nastavení nebo více konfigurací pro různé prostředí, může být obtížné spravovat vše v jednom souboru.

### Doporučení

- **Pro větší projekty nebo projekty s více nástroji**: **"In Dedicated Config Files"**. Tato možnost je obecně preferována, protože umožňuje lepší organizaci a údržbu konfiguračních souborů.
- **Pro menší projekty nebo projekty s minimálním nastavením**: **"In package.json"**. Tento přístup je jednodušší a může být efektivnější pro projekty s menšími konfiguračními potřebami.

### Moje doporučení

Pokud se zaměřuješ na čistotu a udržovatelnost projektu, doporučuji **"In Dedicated Config Files"**. To ti umožní mít samostatné soubory pro každý nástroj a snadněji spravovat jednotlivá nastavení. Tento přístup také usnadňuje práci v týmu, kde každý může snadno upravit pouze konkrétní konfigurační soubor, který se týká jeho oblasti.

[*zpět na obsah*](#obsah) 
## Dotaz:

Tak nyní mám vytvořené základní prostředí pro vue a čím teda pokračovat, co bych měl nyní udělat jako první a co po tom?

## ChaGPT:

Skvělé, že máš základní prostředí pro Vue vytvořené! Nyní se můžeš pustit do dalšího kroku integrace Vue do tvého projektu. Tady je krok za krokem, co bys měl udělat a jak postupovat:

### 1. **Nastavení Projektu**

**1.1.** **Otevři svůj projekt v editoru**:
- Otevři svůj Vue projekt v editoru (např. PyCharm) a zkontroluj, že jsou všechny soubory správně nainstalované.

**1.2.** **Zkontroluj strukturu projektu**:
- Ujisti se, že struktura projektů je taková, jakou očekáváš (např. složky `src`, `public`, a konfigurační soubory jako `vue.config.js` nebo `babel.config.js`).

### 2. **Základní Konfigurace**

**2.1.** **Nastavení Vue Routeru (pokud jsi ho zvolil)**:
- Pokud jsi zvolil Vue Router během vytváření projektu, zkontroluj a nastav základní routy. Otevři soubor `src/router/index.js` a přidej základní routy pro svou aplikaci.

**2.2.** **Nastavení Vuex (pokud jsi ho zvolil)**:
- Pokud jsi zvolil Vuex, nastav základní store. Otevři soubor `src/store/index.js` a nastav základní store, mutace, akce a stavy.

**2.3.** **Nastavení ESLint a Prettier**:
- Pokud jsi zvolil ESLint a Prettier, zkontroluj konfigurační soubory (např. `.eslintrc.js`, `.prettierrc`) a ujisti se, že jsou nastaveny podle tvých preferencí.

### 3. **Vývoj Komponent**

**3.1.** **Vytvoř základní komponenty**:
- Vytvoř základní komponenty Vue. Například `Home.vue`, `About.vue`, atd. v `src/components` nebo `src/views`, pokud používáš Vue Router.

**3.2.** **Vytvoř hlavní App komponentu**:
- Otevři soubor `src/App.vue` a uprav jej podle potřeby. Můžeš přidat základní strukturu, například hlavičku, patičku a hlavní obsah.

**3.3.** **Nastav strukturu složek**:
- Rozhodni se, jak chceš organizovat své komponenty, služby a další soubory (např. složky pro `components`, `views`, `assets`, `store`).

### 4. **Integrace s Backendem (Django)**

**4.1.** **Nastavení API volání**:
- Pokud tvá aplikace potřebuje komunikovat s backendem (např. Django), nastav API volání pomocí knihovny jako Axios nebo Fetch. Můžeš vytvořit základní API službu v `src/services/api.js`.

**4.2.** **Testování API volání**:
- Testuj API volání a ujisti se, že frontend správně komunikuje s backendem. Můžeš použít nástroje jako Postman nebo Insomnia pro testování API.

### 5. **Testování Aplikace**

**5.1.** **Testování komponent**:
- Napiš základní testy pro komponenty, pokud jsi se rozhodl pro Unit Testing. Můžeš použít knihovny jako Jest a Vue Test Utils.

**5.2.** **Testování end-to-end (volitelné)**:
- Pokud chceš, můžeš nastavit testování end-to-end (E2E) pomocí nástrojů jako Cypress nebo Nightwatch. Tato testování ověřují funkčnost celé aplikace.

### 6. **Nasazení Aplikace**

**6.1.** **Build a deploy**:
- Spusť příkaz `npm run build` pro vytvoření produkční verze aplikace. Zkontroluj složku `dist` a ujisti se, že obsahuje všechny potřebné soubory.

**6.2.** **Nasazení**:
- Nasazuj frontend na server, kde běží tvá Django aplikace, nebo na hostingovou platformu (např. Netlify, Vercel, Heroku). 

### 7. **Údržba a Další Vývoj**

**7.1.** **Pravidelná aktualizace**:
- Pravidelně aktualizuj závislosti a prováděj údržbu projektu.

**7.2.** **Přidávání nových funkcí**:
- Jakmile máš základní aplikaci, můžeš přidávat nové funkce a zlepšovat UI/UX.

### Postup kroků

1. **Nastavení projektu**: Zkontroluj a uprav základní konfiguraci.
2. **Vývoj komponent**: Začni s tvorbou komponent a základních stránek.
3. **Integrace s backendem**: Nastav a testuj komunikaci s backendem.
4. **Testování**: Prováděj testování komponent a (volitelně) end-to-end.
5. **Nasazení**: Postav a nasazuj aplikaci.
6. **Údržba**: Udržuj a rozvíjej projekt.

Tento postup ti pomůže začít s vývojem Vue aplikace a integrovat ji s tvým existujícím Django backendem. Ujisti se, že pravidelně testuješ a aktualizuješ svůj projekt, aby byl stabilní a efektivní.


