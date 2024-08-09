### 1) Seznam stručných popisů otázek pro obsah

# Obsah:

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

### 2) Oprava gramatiky a překlepů v otázkách

---

**Dotaz:**

Ahojky :-)
Dneska pokračujeme v učení Vue a to stále na přepracování frontendu, který byl celý psaný v Django do Vue.

Dnešní první otázka zní. V mém projektu mám vlastní CSS, které je přepracované z Bootstrap 5 a má kolem 12000 řádků. Počítám, že asi nebude potřeba úplně vše přetahovat, a tak bych rád ponechal jen ta CSS, která jsou skutečně v kódu použita. Napadlo mě, že budu postupně procházet všechny stránky a styly z Bootstrap tříd přepisovat do Vue přímo.

První na co jsem narazil, je že v Django CSS mám definovaný i vzhled pro body:

```css
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

A ve Vue mám body v tomto souboru: `frontend\public\index.html`

Jak mám ideálně přiřadit CSS pro body ve Vue?

---

**Dotaz:**

Jaké jsou jiné způsoby zápisu tohoto kódu?

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
@import '@/assets/css/b-class/navbar-brand.css';
@import '@/assets/css/b-class/text-sm.css';
@import '@/assets/css/b-class/fw-bold.css';
@import '@/assets/css/b-class/text-dark.css';
</style>
```

Tak aby vracel stejný výsledek?  
Jde mi o to, že předělávám projekt z Django do Vue a v Django jsem hodně používal upraveného CSS Bootstrap stylu, a nyní přemýšlím, zda by se dalo obejít bez něj a použít to, co Vue nabízí.

Tady jsou ještě i CSS pro dané odkazy:

V projektu mám také tento soubor: `frontend\src\assets\css\global\variables.css`  
Kde jsou všechny proměnné a který importuji v `main.js`.

---

**Dotaz:**

Mě jde o to, že projekt, který jsem vytvářel, má vlastní přepracovanou sadu Bootstrap stylů, kde CSS soubor má asi 12000 řádků, takže mě to dovedlo k myšlence, že kdyby byly styly přímo v kódu jako inline style scoped, že by se zlehčilo načítání stylů.  
Nemuselo by docházet k tomu, že by se daný styl vyhledával v tomto 12000 řádků dlouhém CSS, ale byl by k dispozici ihned u kódu. Bylo by to tak asi i více přehledné, akorát nevím, zda se nepletu v tom, že načtení stylu definovaném ve style scoped je rychlejší než načtení ze zmíněného 12000 řádkového CSS?

---

**Dotaz:**

Jaké jsou další možnosti, jak bych mohl situaci se styly, které mají na sebe navázanou JS funkcionalitu, řešit?  
Pojďme si více rozebrat: `collapse` a `navbar-collapse`.

Co dělají, proč je to pro mě dobré a jak jinak bych mohl dosáhnout stejného výsledku, aby to více odpovídalo filozofii práce ve Vue?

---

**Dotaz:**

OK, rozumím, a právě rád bych si na tomto projektu vyzkoušel Vue přístup s vytvářením vlastních komponent.  
Z toho, co píšeš, jsem pochopil, že jsou zde uvedeny jen proto, že při přechodu na menší zařízení se tyto položky schovají do tlačítka s nabídkou.  
Řekněme, že tuto funkcionalitu zatím nepotřebuji a momentálně mi jde primárně o funkčnost na PC bez responzivních požadavků - ty počítám půjdou asi doplnit později.  
Je tedy dobrý nápad přepsat kód tak, že ve style scoped budou uvedeny jen vzhledové styly a responzivní funkčnost budu dodělávat až následně?

---

**Dotaz:**

Když píšu styly do style scoped, pak asi už nemusím používat `!important`, podle mě se toto používá pouze pro přepsání defaultního nastavení, a když vytvářím novou třídu, tak ta přeci žádné defaultní nastavení nemá. Je to tak?

---

**Dotaz:**

Je dobré ve Vue nahradit `<a>` za `<router-link>`?

---

**Dotaz:**

Jak by vypadal jednoduchý kód ve Vue, který by zobrazil obrázek z tohoto umístění: `/media/images/articles/main_picture/article-00000001-0440.jpg`?

---

**Dotaz:**

Je ve Vue nějaký nástroj, který by vytvářel také komponenty, jako mám v Bootstrapu, třeba `container`?

---

**Dotaz:**

Je lepší používat některou z těchto komponent než Bootstrap?