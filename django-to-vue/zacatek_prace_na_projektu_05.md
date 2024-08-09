# Vytváření opakovaně použitelných komponent
[*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-8-2024)   

## Obsah:
[• *Přiřazení CSS pro `<body>` ve Vue*](#dotaz)  
[• *Alternativní způsoby zápisu kódu ve Vue*](#dotaz-1)  
[• *Výhody a nevýhody inline stylů ve Vue vs. globální CSS soubory*](#dotaz-2)  
[• *Alternativy pro `collapse` a `navbar-collapse` ve Vue*](#dotaz-3)  
[• *Vytváření Vue komponent s oddělením stylů a responzivity*](#dotaz-4)  
[• *Potřeba použití `!important` ve style scoped*](#dotaz-5)  
[• *Náhrada `<a>` za `<router-link>` ve Vue*](#dotaz-6)  
[• *Jednoduchý kód pro zobrazení obrázku ve Vue*](#dotaz-7)  
[• *Použití nástrojů ve Vue pro vytváření komponent podobných Bootstrapu*](#dotaz-8)  
[• *Porovnání používání Vue komponent a Bootstrapu*](#dotaz-9)  

## Dotaz:

Ahojky :-)
Dneska pokračujeme v učení Vue a to stále na přepracování frontendu, který byl celý psaný v Django do Vue.

Dnešní první otázka zní. V mém projektu mám vlastní CSS, které je přepracované z Bootstrap 5 a má kolem 12000 řádků. Počítám, že asi nebude potřeba úplně vše přetahovat, a tak bych rád ponechal jen ta CSS, která jsou skutečně v kódu použita. Napadlo mě, že budu postupně procházet všechny stránky a styly z Bootstrap tříd přepisovat do Vue přímo.

První na co jsem narazil, je že v Django CSS mám definovaný i vzhled pro body:

    body {
      margin: 0;
      font-family: var(--bs-body-font-family);
      font-size: var(--bs-body-font-size);
      font-weight: var(--bs-body-font-weight);
      line-height: var(--bs-body-line-height);
      color: var(--bs-body-color);
      text-align: var(--bs-body-text-align);
      background-color: var(--bs-body-bg);
      -webkit-text-size-adjust: 100%;
      -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
    }

A ve Vue mám body v tomto souboru: frontend\public\index.html

Jak mám ideálně přiřadit CSS pro body ve Vue?

## ChatGPT:

Ahoj! :-) 

Ve Vue projektu je ideální přiřadit globální styly pro tělo stránky (body) do hlavního stylového souboru, který je načítán při spuštění aplikace. Postup je následující:

1. **Vytvoření nebo aktualizace globálního stylového souboru:**
   Ve většině Vue projektů je běžnou praxí mít globální stylový soubor, často pojmenovaný `main.css`, `global.css` nebo podobně. Pokud tento soubor již existuje, můžeme do něj přidat styly pro `body`. Pokud neexistuje, vytvoříme ho.

2. **Přidání globálních stylů do projektu:**
   Předpokládejme, že vytvoříme nebo aktualizujeme soubor `src/assets/styles/global.css`. Přidáme do něj styly pro `body` takto:

```css
/* src/assets/styles/global.css */
body {
  margin: 0;
  font-family: var(--bs-body-font-family);
  font-size: var(--bs-body-font-size);
  font-weight: var(--bs-body-font-weight);
  line-height: var(--bs-body-line-height);
  color: var(--bs-body-color);
  text-align: var(--bs-body-text-align);
  background-color: var(--bs-body-bg);
  -webkit-text-size-adjust: 100%;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
```

3. **Import globálního stylového souboru:**
   Poté musíme zajistit, aby byl tento stylový soubor importován do hlavního souboru aplikace, obvykle `main.js` nebo `main.ts` (pokud používáte TypeScript). Do tohoto souboru přidáme import:

```javascript
// src/main.js nebo src/main.ts
import { createApp } from 'vue'
import App from './App.vue'
import './assets/styles/global.css' // Import globálních stylů

createApp(App).mount('#app')
```

Tímto způsobem zajistíme, že styly pro `body` budou aplikovány globálně v celé aplikaci Vue.

**Poznámka:** Pokud používáte nějaký CSS preprocesor jako SASS, SCSS nebo LESS, můžete postupovat obdobně, jen se přizpůsobit syntaxí daného preprocesoru.

Pokud máte jakékoli další dotazy nebo potřebujete pomoc s dalšími částmi projektu, neváhejte se zeptat!

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-8-2024)  
## Dotaz:

Jaké jsou jiné způsoby zápisu tohoto kódu?

<template>
  <router-link 
    class="navbar-brand text-sm fw-bold text-dark" 
    to="/"
  >
  Bootstrap Blog
  </router-link>
</template>

<style scoped>
@import '@/assets/css/b-class/navbar-brand.css';
@import '@/assets/css/b-class/text-sm.css';
@import '@/assets/css/b-class/fw-bold.css';
@import '@/assets/css/b-class/text-dark.css';
</style>

Tak aby vracel stejný výsledek?
Jde mi o to, že předělávám projekt z Django do Vue a v Django jsem hodně používal upraveného CSS Bootstrap stylu, a nyní přemýšlím, zda by se dalo obejít bez něj a použít to, co Vue nabízí.

Tady jsou ještě i CSS pro dané odkazy:

frontend\src\assets\css\b-class\navbar-brand.css:

      .navbar-brand {
        padding-top: 0.3125rem;
        padding-bottom: 0.3125rem;
        margin-right: 1rem;
        font-size: 1.25rem;
        white-space: nowrap;
      }
      
      .navbar-light .navbar-brand {
        color: #999;
      }
      
      .navbar-light .navbar-brand:hover, .navbar-light .navbar-brand:focus {
        color: #999;
      }
      
      .navbar-dark .navbar-brand {
        color: #fff;
      }
      
      .navbar-dark .navbar-brand:hover, .navbar-dark .navbar-brand:focus {
        color: #fff;
      }

frontend\src\assets\css\b-class\text-sm.css:

      .text-sm {
        font-size: 0.875rem;
      }
      
      @media (min-width: 576px) {
        .text-sm-start {
          text-align: left !important;
        }
        .text-sm-end {
          text-align: right !important;
        }
        .text-sm-center {
          text-align: center !important;
        }
      }

frontend\src\assets\css\b-class\fw-bold.css:

      .fw-bold {
        font-weight: 700 !important;
      }

frontend\src\assets\css\b-class\text-dark.css:

      .text-dark {
        --bs-text-opacity: 1;
        color: rgba(var(--bs-dark-rgb), var(--bs-text-opacity)) !important;
      }

V projektu mám také tento soubor: frontend\src\assets\css\global\variables.css
Kde jsou všechny proměnné a který importuji v main.js.


## ChtaGPT:

Přemýšlet o přechodu na Vue bez závislosti na Bootstrapu je rozumné, zejména pokud hledáte čistší a více přizpůsobené řešení. Ve Vue můžete dosáhnout podobného výsledku pomocí několika různých přístupů, například:

1. **Použití přímo stylů ve Vue komponentě (inline styles):**
   Můžete přímo definovat potřebné styly v rámci komponenty pomocí inline stylů nebo scoped stylů v `<style>` části.

2. **Vytvoření vlastních CSS tříd ve scoped style:**
   Můžete vytvořit vlastní CSS třídy uvnitř scoped stylů vaší Vue komponenty.

3. **Použití CSS Modules:**
   Použití CSS modules pro lokální scoping stylů.

Zde jsou příklady, jak by to mohlo vypadat:

### 1. Použití Inline stylů

```vue
<template>
  <router-link 
    :style="navbarBrandStyles" 
    to="/"
  >
    Bootstrap Blog
  </router-link>
</template>

<script>
export default {
  computed: {
    navbarBrandStyles() {
      return {
        paddingTop: '0.3125rem',
        paddingBottom: '0.3125rem',
        marginRight: '1rem',
        fontSize: '1.25rem',
        whiteSpace: 'nowrap',
        fontSize: '0.875rem',
        fontWeight: '700',
        color: 'rgba(var(--bs-dark-rgb), 1)'
      };
    }
  }
}
</script>
```

### 2. Vytvoření vlastních CSS tříd

```vue
<template>
  <router-link 
    class="navbar-brand-custom text-sm-custom fw-bold-custom text-dark-custom" 
    to="/"
  >
    Bootstrap Blog
  </router-link>
</template>

<style scoped>
.navbar-brand-custom {
  padding-top: 0.3125rem;
  padding-bottom: 0.3125rem;
  margin-right: 1rem;
  font-size: 1.25rem;
  white-space: nowrap;
}

.text-sm-custom {
  font-size: 0.875rem;
}

.fw-bold-custom {
  font-weight: 700 !important;
}

.text-dark-custom {
  color: rgba(var(--bs-dark-rgb), 1) !important;
}
</style>
```

### 3. Použití CSS Modules

Nejprve vytvořte CSS soubor jako `NavbarLink.module.css`:

```css
/* NavbarLink.module.css */
.navbarBrand {
  padding-top: 0.3125rem;
  padding-bottom: 0.3125rem;
  margin-right: 1rem;
  font-size: 1.25rem;
  white-space: nowrap;
}

.textSm {
  font-size: 0.875rem;
}

.fwBold {
  font-weight: 700 !important;
}

.textDark {
  color: rgba(var(--bs-dark-rgb), 1) !important;
}
```

Poté jej importujte a použijte v komponentě:

```vue
<template>
  <router-link 
    :class="[styles.navbarBrand, styles.textSm, styles.fwBold, styles.textDark]" 
    to="/"
  >
    Bootstrap Blog
  </router-link>
</template>

<script>
import styles from './NavbarLink.module.css';

export default {
  computed: {
    styles() {
      return styles;
    }
  }
}
</script>
```

### Další možnost: Použití CSS frameworku

Pokud chcete nadále využívat stylování podobné Bootstrapu, můžete použít CSS frameworky jako Tailwind CSS, který nabízí utility-first přístup a snadno přizpůsobitelné styly.

Přechod od Bootstrapu k vlastnímu řešení ve Vue může být časově náročný, ale nabízí větší flexibilitu a kontrolu nad výsledným vzhledem aplikace.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-8-2024)  
## Dotaz:

Mě jde o to, že projekt, který jsem vytvářel, má vlastní přepracovanou sadu Bootstrap stylů, kde CSS soubor má asi 12000 řádků, takže mě to dovedlo k myšlence, že kdyby byly styly přímo v kódu jako inline style scoped, že by se zlehčilo načítání stylů.
Nemuselo by docházet k tomu, že by se daný styl vyhledával v tomto 12000 řádků dlouhém CSS, ale byl by k dispozici ihned u kódu. Bylo by to tak asi i více přehledné, akorát nevím, zda se nepletu v tom, že načtení stylu definovaném ve style scoped je rychlejší než načtení ze zmíněného 12000 řádkového CSS?

## ChatGPT:

Zvažování výkonu načítání stylů a jejich udržovatelnosti je důležité. Máte pravdu, že mít velmi dlouhý CSS soubor může ovlivnit výkon a přehlednost. Vkládání stylů přímo do kódu pomocí scoped stylů nebo inline stylů může mít několik výhod, ale také několik nevýhod. Podívejme se na oba přístupy a jejich vliv na výkon a udržovatelnost.

### Výhody a nevýhody inline stylů a scoped stylů

**Výhody:**
1. **Lepší přehlednost a udržovatelnost:** Každá komponenta má své styly definované přímo v ní, což usnadňuje čtení a úpravu kódu.
2. **Rychlejší načítání specifických stylů:** Prohlížeč nemusí procházet velký externí CSS soubor, aby našel styly pro danou komponentu.

**Nevýhody:**
1. **Opakování stylů:** Můžete skončit s duplicitními styly v různých komponentách, což může zvýšit celkovou velikost CSS a zpomalit načítání stránky.
2. **Ztráta globálního přehledu:** Správa globálních stylů, jako jsou proměnné nebo mixiny, může být obtížnější.

### Výkon a rychlost načítání

Načítání stylů z jednoho velkého CSS souboru může být rychlejší při prvním načtení, protože prohlížeče mohou tento soubor kešovat. Na druhou stranu, pokud je potřeba neustále hledat specifické styly v tomto velkém souboru, může to mít negativní dopad na výkon.

### Doporučený přístup

1. **Scoped CSS a komponenty:** Používání scoped stylů v rámci komponent Vue je dobrý kompromis. Můžete tím snížit velikost globálních CSS souborů a zároveň udržovat styly organizované.

2. **CSS Modules:** CSS Modules nabízí scoped styly a zároveň umožňuje použití proměnných a mixinů napříč komponentami.

3. **Refaktorování globálních stylů:** Pokud máte mnoho globálních stylů, které se často opakují, může být užitečné je refaktorovat do menších, logických souborů a importovat je jen tam, kde jsou potřeba.

### Konkrétní příklad s použitím scoped stylů

Přeměňme váš příklad do scoped stylů:

```vue
<template>
  <router-link 
    class="navbar-brand text-sm fw-bold text-dark" 
    to="/"
  >
    Bootstrap Blog
  </router-link>
</template>

<style scoped>
.navbar-brand {
  padding-top: 0.3125rem;
  padding-bottom: 0.3125rem;
  margin-right: 1rem;
  font-size: 1.25rem;
  white-space: nowrap;
}

.navbar-light .navbar-brand {
  color: #999;
}

.navbar-light .navbar-brand:hover, .navbar-light .navbar-brand:focus {
  color: #999;
}

.navbar-dark .navbar-brand {
  color: #fff;
}

.navbar-dark .navbar-brand:hover, .navbar-dark .navbar-brand:focus {
  color: #fff;
}

.text-sm {
  font-size: 0.875rem;
}

@media (min-width: 576px) {
  .text-sm-start {
    text-align: left !important;
  }
  .text-sm-end {
    text-align: right !important;
  }
  .text-sm-center {
    text-align: center !important;
  }
}

.fw-bold {
  font-weight: 700 !important;
}

.text-dark {
  --bs-text-opacity: 1;
  color: rgba(var(--bs-dark-rgb), var(--bs-text-opacity)) !important;
}
</style>
```

Tímto způsobem budou styly uloženy přímo v komponentě a budou aplikovány pouze na tuto komponentu.

### Shrnutí

- **Scoped CSS**: Doporučuji využít scoped styly ve Vue komponentách. Pomohou udržet kód přehledný a mohou zlepšit výkon tím, že prohlížeč nemusí prohledávat velký globální CSS soubor.
- **CSS Modules**: Další efektivní způsob, jak mít lokální styly s minimálním dopadem na globální stylování.
- **Refaktorování globálních stylů**: Zmenšení a rozdělení velkého CSS souboru na menší, logicky oddělené soubory může zlepšit výkon a udržovatelnost.

Pokud máte nějaké konkrétní otázky nebo potřebujete další pomoc s refaktoringem, neváhejte se zeptat!

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-8-2024)  
## Dotaz:

Jaké jsou další možnosti, jak bych mohl situaci se styly, které mají na sebe navázanou JS funkcionalitu, řešit?
Pojďme si více rozebrat: collapse a navbar-collapse.

Co dělají, proč je to pro mě dobré a jak jinak bych mohl dosáhnout stejného výsledku, aby to více odpovídalo filozofii práce ve Vue?

## ChatGPT:

### Třídy `collapse` a `navbar-collapse`

#### Co dělají?

- **`collapse`**: Tato třída je součástí Bootstrapu a slouží k vytváření rozbalitelných a sbalitelných oblastí. Používá se v kombinaci s datovými atributy nebo JavaScriptem pro správu viditelnosti těchto oblastí.
- **`navbar-collapse`**: Tato třída se používá k označení oblasti navigačního pruhu (navbar), která bude rozbalitelná. Spolu s třídou `collapse` a specifickými datovými atributy umožňuje skrytí a zobrazení navigačního obsahu.

#### Proč je to pro vás dobré?

Použití těchto tříd v Bootstrapu usnadňuje práci, protože poskytují připravené řešení pro rozbalitelné a sbalitelné navigační prvky. Není třeba psát vlastní JavaScript, abyste dosáhli požadované funkcionality. To zrychluje vývoj a zajišťuje konzistenci a kompatibilitu s různými prohlížeči.

### Jak dosáhnout stejného výsledku ve Vue?

Ve Vue můžete využít jeho komponentový přístup a reaktivitu k vytvoření vlastního řešení pro rozbalitelnou navigaci. Vue umožňuje vytvořit dynamické a interaktivní prvky přímo v komponentách bez nutnosti spoléhat se na externí knihovny jako Bootstrap.

### Možnosti ve Vue

#### 1. Použití BootstrapVue nebo jiného Vue UI frameworku
- **BootstrapVue**: Je to knihovna komponent pro Vue, která přináší Bootstrapové komponenty přímo do Vue aplikace. Můžete tak zachovat známou funkcionalitu Bootstrapu a zároveň pracovat ve Vue.
- **Další UI frameworky**: Quasar, Vuetify, ElementUI, které poskytují podobné komponenty a funkcionalitu.

#### 2. Vytvoření vlastních komponent a funkcionality
- Můžete vytvořit vlastní komponenty, které budou napodobovat chování `collapse` a `navbar-collapse` pomocí reaktivity a stavového managementu ve Vue.

#### Příklad implementace vlastní rozbalitelné navigace ve Vue

```vue
<template>
  <div>
    <button @click="toggleNavbar" class="navbar-toggler" type="button">
      Toggle navigation
    </button>
    <div v-if="isNavbarOpen" class="custom-navbar-collapse" id="navbarSupportedContent">
      <ul class="custom-navbar-nav">
        <NavItem label="Home" to="/" routeName="homeView" />
        <NavItem label="Articles" to="/articles/all" routeName="articlesView" />
        <NavItem label="Post" to="/post" routeName="postView" />
        <NavSearchAndLanguage />
      </ul>
    </div>
  </div>
</template>

<script>
import NavItem from './NavItems/NavItem.vue';
import NavSearchAndLanguage from './NavItems/NavSearchAndLanguage.vue';

export default {
  name: 'NavItems',
  components: {
    NavItem,
    NavSearchAndLanguage,
  },
  data() {
    return {
      isNavbarOpen: false,
    };
  },
  methods: {
    toggleNavbar() {
      this.isNavbarOpen = !this.isNavbarOpen;
    },
  },
};
</script>

<style scoped>
.custom-navbar-collapse {
  padding-right: 5px; /* Ekvivalent třídy my-padding-right-5px */
}

.custom-navbar-nav {
  margin-left: auto; /* Ekvivalent třídy ms-auto */
  margin-bottom: 2rem; /* Ekvivalent třídy mb-2 */
}

@media (min-width: 992px) {
  .custom-navbar-nav {
    margin-bottom: 0; /* Ekvivalent třídy mb-lg-0 */
    align-items: center; /* Ekvivalent třídy align-items-lg-center */
  }
}
</style>
```

### Vysvětlení

- **Button**: Tlačítko s `@click="toggleNavbar"` přepíná stav `isNavbarOpen`.
- **`v-if="isNavbarOpen"`**: Div obsahující navigační položky se zobrazí pouze tehdy, pokud je `isNavbarOpen` true.
- **Data a Methods**: Data obsahují stav `isNavbarOpen`, který je přepínán metodou `toggleNavbar`.

Tímto způsobem můžete snadno vytvořit vlastní rozbalitelnou navigaci ve Vue bez nutnosti používat externí knihovny. Tento přístup je více přizpůsobitelný a integruje se do filozofie Vue, která klade důraz na komponentový a reaktivní přístup k vývoji aplikací.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-8-2024)  
## Dotaz:

OK, rozumím, a právě rád bych si na tomto projektu vyzkoušel Vue přístup s vytvářením vlastních komponent.
Z toho, co píšeš, jsem pochopil, že jsou zde uvedeny jen proto, že při přechodu na menší zařízení se tyto položky schovají do tlačítka s nabídkou.
Řekněme, že tuto funkcionalitu zatím nepotřebuji a momentálně mi jde primárně o funkčnost na PC bez responzivních požadavků - ty počítám půjdou asi doplnit později.
Je tedy dobrý nápad přepsat kód tak, že ve style scoped budou uvedeny jen vzhledové styly a responzivní funkčnost budu dodělávat až následně?

## ChatGPT:

Ano, je to dobrý přístup. Můžete se nejprve zaměřit na funkčnost a vzhled na desktopu a následně přidat responzivní funkcionalitu. To vám umožní postupně zpracovat kód a přidávat složitost, jakmile bude základní verze hotová.

### Přepis kódu pro desktop bez responzivních požadavků

1. **Odstraníme třídy s JavaScriptovou funkcionalitou**: `collapse`, `navbar-collapse`
2. **Ponecháme a převedeme vzhledové styly**: `my-padding-right-5px`, `navbar-nav`, `ms-auto`, `mb-2`, `mb-lg-0`, `align-items-lg-center`

### Upravený kód

```vue
<template>
  <div class="custom-navbar">
    <ul class="custom-navbar-nav">
      <NavItem 
        label="Home" 
        to="/" 
        routeName="homeView" 
      />
      <NavItem 
        label="Articles" 
        to="/articles/all" 
        routeName="articlesView" 
      />
      <NavItem 
        label="Post" 
        to="/post" 
        routeName="postView" 
      />
      <NavSearchAndLanguage />
    </ul>
  </div>
</template>

<script>
import NavItem from './NavItems/NavItem.vue';
import NavSearchAndLanguage from './NavItems/NavSearchAndLanguage.vue';

export default {
  name: 'NavItems',
  components: {
    NavItem,
    NavSearchAndLanguage,
  },
}
</script>

<style scoped>
.custom-navbar {
  padding-right: 5px; /* Ekvivalent třídy my-padding-right-5px */
}

.custom-navbar-nav {
  display: flex;
  justify-content: flex-end; /* Ekvivalent třídy ms-auto */
  margin-bottom: 2rem; /* Ekvivalent třídy mb-2 */
  align-items: center; /* Ekvivalent třídy align-items-lg-center */
}

.custom-navbar-nav > * {
  margin-right: 1rem;
}

.custom-navbar-nav > *:last-child {
  margin-right: 0;
}
</style>
```

### Vysvětlení

- **`custom-navbar`**: Přidává padding napravo.
- **`custom-navbar-nav`**: Vytváří flexbox kontejner, který zarovnává položky doprava a vertikálně centruje.
- **`margin-bottom: 2rem`**: Přidává spodní margin pro všechny položky.
- **`custom-navbar-nav > *`**: Přidává pravý margin mezi jednotlivé položky, poslední položce odstraní pravý margin.

### Další kroky

1. **Testování**: Zkontrolujte, zda kód funguje správně na desktopu.
2. **Přidání responzivní funkcionality**: Jakmile je základní verze funkční, můžete přidat responzivní styly a funkcionalitu, jako je tlačítko pro zobrazení/schování navigačního menu na menších zařízeních.

### Přidání responzivní funkcionality

Například, můžete použít `@media` dotazy k přidání responzivních stylů:

```vue
<style scoped>
.custom-navbar {
  padding-right: 5px; /* Ekvivalent třídy my-padding-right-5px */
}

.custom-navbar-nav {
  display: flex;
  justify-content: flex-end; /* Ekvivalent třídy ms-auto */
  margin-bottom: 2rem; /* Ekvivalent třídy mb-2 */
  align-items: center; /* Ekvivalent třídy align-items-lg-center */
}

.custom-navbar-nav > * {
  margin-right: 1rem;
}

.custom-navbar-nav > *:last-child {
  margin-right: 0;
}

@media (max-width: 992px) {
  .custom-navbar-nav {
    flex-direction: column;
    margin-bottom: 0; /* Ekvivalent třídy mb-lg-0 */
  }
}
</style>
```

### Vysvětlení

- **Flexbox**: Použití flexboxu umožňuje snadnou změnu rozvržení na různých zařízeních.
- **`@media` dotazy**: Přidávají specifické styly pro různá zařízení.

Tímto způsobem můžete postupně budovat svou aplikaci ve Vue, začít s desktopovou verzí a postupně přidávat mobilní a tabletové verze.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-8-2024)  
## Dotaz:

Když píšu styly do style scoped, pak asi už nemusím používat !important, podle mě se toto používá pouze pro přepsání defaultního nastavení, a když vytvářím novou třídu, tak ta přeci žádné defaultní nastavení nemá. Je to tak?

## ChatGPT:

Ano, máte pravdu. Použití `!important` v CSS je obvykle zaměřeno na přepsání již existujících stylů, které mají vyšší specifikaci. Když vytváříte nové třídy a styly v `style scoped`, většinou nemáte žádné konflikty s výchozími styly, které by vyžadovaly použití `!important`. Zde je podrobnější vysvětlení:

### Jak funguje `!important`

- **Specifikace a přepsání**: `!important` zajišťuje, že dané pravidlo bude mít nejvyšší prioritu a přepíše ostatní pravidla, která mohou mít stejnou specifikaci. Používá se, když chcete zajistit, že určitý styl bude aplikován bez ohledu na ostatní pravidla.

- **Použití**: Používá se obvykle v situacích, kdy máte konflikt mezi styly nebo chcete přepsat styly, které jsou definovány v externích knihovnách (např. Bootstrap), které mají vysokou specifikaci nebo jsou definovány jako `!important`.

### `!important` a `style scoped`

- **Specifické styly**: V `style scoped` se styly vztahují pouze na komponentu, což znamená, že vaše nové styly by měly mít výchozí prioritu nad globálními styly. Při použití `scoped` CSS jsou styly izolovány na úrovni komponenty a neměly by kolidovat s ostatními styly.

- **Použití `!important`**: Většinou není potřeba používat `!important`, pokud jste si jisti, že vaše třídy mají správnou specifikaci a neexistují žádné konflikty s jinými styly. Použití `!important` může ztížit ladění, protože zvyšuje prioritu daného pravidla a může maskovat problémy v kaskádování.

### Kdy použít `!important`

- **Globální knihovny**: Pokud pracujete s externími knihovnami (např. Bootstrap) a chcete přepsat jejich styly bez změny samotné knihovny, `!important` může být užitečné.

- **Konflikty specifikace**: Pokud máte více stylů, které se vztahují k stejnému prvku, a stále dochází ke konfliktům, můžete použít `!important` jako poslední možnost.

### Příklady

1. **Bez `!important`**: 
   ```css
   .my-class {
     color: red;
   }
   ```

   V tomto případě, pokud `.my-class` není ovlivněna jinými pravidly s vyšší specifikací, bude mít červenou barvu textu.

2. **S `!important`**:
   ```css
   .my-class {
     color: red !important;
   }
   ```

   Použití `!important` zajistí, že barva textu bude červená bez ohledu na další pravidla.

### Shrnutí

- **V `style scoped`**: Obvykle není třeba používat `!important`, pokud vaše třídy a specifikace jsou správně nastaveny.
- **Pro přepsání knihoven nebo konflikty**: `!important` je užitečné, pokud se snažíte přepsat styly z externích knihoven nebo máte složité konflikty stylů.

Používejte `!important` uvážlivě a pokud je to možné, snažte se dosáhnout požadovaného výsledku pomocí správného nastavení specifikace a hierarchie stylů.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-8-2024)  
## Dotaz:

Je dobré ve Vue nahradit <a> za <router-link>?


# ChatGPT:

Ano, ve Vue.js je doporučeno používat `<router-link>` místo `<a>` pro navigaci v rámci aplikace. `<router-link>` je speciální komponenta poskytovaná Vue Router, která je navržena tak, aby usnadnila navigaci mezi různými stránkami nebo komponentami vaší aplikace bez nutnosti načítání celé stránky znovu.

### Výhody Použití `<router-link>`

1. **Jednoduché Navigační Funkce**:
   - `<router-link>` automaticky používá Vue Router pro navigaci, což znamená, že se vyhneme přímému použití URL a spravování historie navigace ručně.
   - `to` atribut slouží k definování cílové trasy (routy), což činí kód čitelnější a lépe spravovatelný.

2. **Zachování Jednostránkové Aplikace (SPA)**:
   - Použitím `<router-link>` se přechody mezi stránkami dějí bez obnovy celé stránky, což zajišťuje plynulost a rychlost, jakou očekáváte od SPA (Single Page Application).
   - Při použití `<a>` tagu by pro navigaci bylo nutné načítat nové stránky z serveru, což může mít za následek pomalejší přechody a ztrátu stavu aplikace.

3. **Podpora Aktivního Stavu**:
   - `<router-link>` automaticky přidává CSS třídu `router-link-active` (nebo jinou specifikovanou třídu), když je link aktivní (tj. když odpovídá aktuální cestě). To usnadňuje stylování aktivního odkazu.

4. **Podpora Různých Typů Navigace**:
   - `<router-link>` umožňuje definovat, zda se má odkazy otevřít v novém okně (`target="_blank"`) nebo jinak upravit chování odkazu.

### Příklady Použití

1. **Základní Použití**:
   ```vue
   <router-link to="/about">About</router-link>
   ```

   Tento link naviguje na `/about` stránku bez potřeby načítání nové stránky.

2. **S Aktivní Třídou**:
   ```vue
   <router-link :class="{ active: isActive }" to="/services">Services</router-link>
   ```

   Třída `active` bude aplikována, pokud cesta odpovídá aktuální trase.

3. **Navigace s Parametry**:
   ```vue
   <router-link :to="{ name: 'user', params: { userId: 123 } }">User Profile</router-link>
   ```

   Navigace na trasu pojmenovanou `user` s parametrem `userId`.

### Porovnání s `<a>`

- **`<a>` tag**:
  ```html
  <a href="/about">About</a>
  ```

  Tento tag způsobí úplné načtení stránky, což může vést k delšímu času načítání a ztrátě stavu aplikace.

- **`<router-link>`**:
  ```vue
  <router-link to="/about">About</router-link>
  ```

  Používá Vue Router pro navigaci, což je efektivnější a udržuje SPA chování.

### Shrnutí

Použití `<router-link>` místo `<a>` je doporučené v kontextu Vue.js aplikací, protože poskytuje lepší integraci s Vue Router a přispívá k plynulejšímu a rychlejšímu uživatelskému zážitku. Pomocí `<router-link>` můžete efektivně spravovat navigaci bez opětovného načítání stránky, což je klíčové pro výkonnost a uživatelský komfort ve SPA.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-8-2024)  
## Dotaz:

Jak by vypadal jednoduchý kód ve Vue, který by zobrazil obrázek z tohoto umístění: /media/images/articles/main_picture/article-00000001-0440.jpg?


## ChatGPT:

Pokud chcete zobrazit obrázek v aplikaci Vue.js, a to z umístění `/media/images/articles/main_picture/article-00000001-0440.jpg`, budete potřebovat použít správnou cestu k obrázku. Zde je jednoduchý příklad, jak by to mohlo vypadat:

### Příklad Komponenty Vue

#### 1. Vytvoření Komponenty Vue

**`ImageDisplay.vue`**

```vue
<template>
  <div class="image-container">
    <img :src="imageUrl" alt="Article Image" class="article-image" />
  </div>
</template>

<script>
export default {
  name: 'ImageDisplay',
  data() {
    return {
      // Cesta k obrázku
      imageUrl: '/media/images/articles/main_picture/article-00000001-0440.jpg'
    };
  }
}
</script>

<style scoped>
.image-container {
  text-align: center; /* Volitelně center image */
}

.article-image {
  max-width: 100%; /* Umožňuje obrázku přizpůsobit se velikosti kontejneru */
  height: auto; /* Zachování poměru stran obrázku */
}
</style>
```

### Jak Používat Komponentu

1. **Import a Použití Komponenty v Rodičovské Komponentě**

Pokud chcete tento obrázek zobrazit na jiné stránce nebo komponentě, můžete `ImageDisplay` komponentu importovat a použít následovně:

**`App.vue` (nebo jiná komponenta)**

```vue
<template>
  <div id="app">
    <ImageDisplay />
  </div>
</template>

<script>
import ImageDisplay from './components/ImageDisplay.vue'; // Ujistěte se, že cesta je správná

export default {
  name: 'App',
  components: {
    ImageDisplay
  }
}
</script>

<style>
/* Globální styly, pokud jsou potřeba */
</style>
```

### Vysvětlení

1. **`<template>`**:
   - Obsahuje HTML šablonu, kde `<img>` tag je použit pro zobrazení obrázku.
   - `:src="imageUrl"` binduje dynamicky URL obrázku z `data`.

2. **`<script>`**:
   - Definuje data komponenty, kde `imageUrl` obsahuje cestu k obrázku.
   - Můžete zde upravit `imageUrl` na základě potřeby, např. pokud byste chtěli, aby byl dynamický.

3. **`<style scoped>`**:
   - Obsahuje specifické styly pro tuto komponentu.
   - `.article-image` zajišťuje, že obrázek se přizpůsobí velikosti kontejneru a zachovává poměr stran.

### Poznámky

- **Cesta k Obrázku**: V tomto případě je cesta k obrázku přímá, což je v pořádku, pokud používáte statické soubory, které jsou dostupné z webového serveru.
- **Dynamické Cesty**: Pokud by cesta k obrázku byla dynamická, můžete ji upravit pomocí props nebo computed properties.

Tento jednoduchý příklad ukazuje, jak pracovat se statickými obrázky ve Vue.js a jak využít komponenty pro zobrazení obsahu.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-8-2024)  
# Dotazy z předchozího dne z procházky se psem:
## Dotaz:

Je ve Vue nějaký nástroj, který by vytvářel také komponenty, jako mám v Bootstrapu, třeba container?

## ChatGPT:

Ano, ve Vue.js je možné používat různé UI knihovny, které nabízejí předpřipravené komponenty podobné těm v Bootstrapu. Některé z těchto knihoven jsou speciálně navrženy pro použití s Vue a poskytují komponenty jako kontejnery, tlačítka, modaly a další. Zde jsou některé z nejpopulárnějších:

1. **Vuetify**:
   - Vuetify je kompletní UI framework založený na Material Designu od Googlu. Nabízí širokou škálu komponent a je dobře dokumentovaný.
   - [Vuetify](https://vuetifyjs.com/)

2. **BootstrapVue**:
   - BootstrapVue přináší plnou moc Bootstrapu 4 do Vue.js. Poskytuje komponenty Bootstrapu jako `<b-container>`, `<b-row>`, `<b-col>` a další, přímo použitelné ve Vue.
   - [BootstrapVue](https://bootstrap-vue.org/)

3. **Quasar Framework**:
   - Quasar je další výkonný framework, který nabízí množství UI komponent a utilit, a zároveň umožňuje vytvářet aplikace pro různé platformy (web, mobil, desktop).
   - [Quasar](https://quasar.dev/)

4. **Element**:
   - Element je další populární UI knihovna, která poskytuje komponenty jako kontejnery, tlačítka, formuláře a mnoho dalších. Je známá svou čistou a profesionální estetikou.
   - [Element](https://element.eleme.io/)

Všechny tyto knihovny poskytují obsáhlou dokumentaci a příklady, jak začít a používat jejich komponenty ve vašich Vue projektech.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-8-2024)  
## Dotaz:

Je lepší používat některou z těchto komponent než Bootstrap?

## ChatGPT:

Zda je lepší používat některou z výše zmíněných knihoven místo Bootstrapu, závisí na vašich konkrétních potřebách a preferencích. Zde jsou některé aspekty, které můžete zvážit:

### Vuetify
**Výhody:**
- Kompletní implementace Material Designu.
- Bohatá sada komponent.
- Skvělá integrace s Vue.js a dobrá dokumentace.

**Nevýhody:**
- Může být těžší přizpůsobit, pokud nepotřebujete nebo nechcete Material Design.
- Větší balík, což může ovlivnit výkon.

### BootstrapVue
**Výhody:**
- Pokud už znáte Bootstrap, přechod bude velmi snadný.
- Předpřipravené Bootstrap komponenty přímo pro Vue.js.
- Velká komunita a mnoho zdrojů.

**Nevýhody:**
- Design je omezen na Bootstrap styl, který může být méně moderní ve srovnání s jinými frameworky.
- Méně zaměřeno na Vue specifické vlastnosti než některé jiné knihovny.

### Quasar Framework
**Výhody:**
- Podpora pro různé platformy (web, mobil, desktop).
- Bohatá sada komponent a utilit.
- Flexibilita a možnost přizpůsobení.

**Nevýhody:**
- Může mít strmější křivku učení kvůli množství funkcí a možností.
- Větší velikost balíčku.

### Element
**Výhody:**
- Profesionální a čistý design.
- Široká škála komponent.
- Dobrá dokumentace a podpora pro Vue.js.

**Nevýhody:**
- Může být méně známý a používaný ve srovnání s Bootstrap.
- Omezenější možnosti přizpůsobení vzhledu než u Quasaru nebo Vuetify.

### Bootstrap
**Výhody:**
- Velká popularita a široká podpora.
- Snadné použití a integrace s jakýmkoliv frontend frameworkem.
- Obrovská komunita a mnoho existujících šablon a příkladů.

**Nevýhody:**
- Styl může být považován za méně moderní.
- Omezenější komponenty specifické pro Vue.js oproti knihovnám navrženým přímo pro Vue.

### Závěr
- **Pokud chcete moderní a esteticky příjemné UI**: Vuetify nebo Element.
- **Pokud máte zkušenosti s Bootstrapem a hledáte snadný přechod na Vue**: BootstrapVue.
- **Pokud potřebujete víceplatforemní podporu a flexibilitu**: Quasar Framework.

Volba nejlepší knihovny závisí na vašem konkrétním projektu, preferencích v designu a funkcionalitě, a zkušenostech s jednotlivými frameworky.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-8-2024)  
