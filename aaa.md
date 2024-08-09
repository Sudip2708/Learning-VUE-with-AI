### 1) Seznam stručných popisů otázek pro obsah

# Obsah:

[• *Přehled použití vzorových komponent ve Vue*](#dotaz)  
[• *Stylizace HTML pomocí vlastních tříd vs. Bootstrap*](#dotaz-1)  
[• *Použití `:root` a proměnných z CSS ve Vue*](#dotaz-2)  
[• *Kontrola CSS pro třídu `container` a umístění specifických stylů*](#dotaz-3)  
[• *Import stylů přímo do komponent*](#dotaz-4)  
[• *Vytváření specifických stylů, které obsahují více importů*](#dotaz-5)  
[• *Význam stylů pro `*`, `*::before`, `*::after` a `scroll-behavior`*](#dotaz-6)  
[• *Rozhodování mezi umístěním importu do `main.js` nebo `App.vue`*](#dotaz-7)  
[• *Určení, zda styly pro `<body>` patří do globálního stylu*](#dotaz-8)  
[• *Význam stylu pro `<abbr>` s atributem `title`*](#dotaz-9)  
[• *Vazba stylu na tag `<abbr>` a jeho potřeba v projektu*](#dotaz-10)  


### 2) Oprava gramatiky a překlepů v otázkách

---

**Dotaz:**

Ahojky :-)
Učím se Vue a teď by mě zajímalo toto:
Takto mám definovaný komponent, který zobrazuje některé položky v horní navigační liště `frontend\src\components\RootHeader\NavItems.vue`:

A takto mám pak definované tyto položky:

1) `frontend\src\components\RootHeader\NavItems\NavHome.vue`:

2) `frontend\src\components\RootHeader\NavItems\NavArticles.vue`:

3) `frontend\src\components\RootHeader\NavItems\NavPost.vue`

A teď k mému dotazu:
Tyto všechny tři kódy mají identický základ a jenom se mění proměnné hodnoty. Napadlo mě tedy, jestli by nebylo lepší udělat jeden vzor pro všeobecnou položku navigace a pak tento vzor volat s předáním příslušných proměnných?
Zajímalo by mě k tomu zejména, jestli se to dělá a je to běžnou praktikou, a pak taky jestli to není tak, že je lepší definovat každý kód zvlášť, tak jak to mám v projektu teď, z důvodu rychlejšího načítání. Přeci jenom dosazení proměnných do nějaké šablony je asi delší než nahrání šablony už s proměnnými. Jak to tedy je?

---

**Dotaz:**

Nyní bych se chtěl zeptat k stylizování HTML pomocí tříd. Ve svém projektu jsem používal Bootstrap a nyní mi přijde, že v rámci Vue by možná bylo přehlednější vytvořit vlastní CSS pro každý prvek, aby nedocházelo k tomu, že se mi v template za každým prvkem bude vytvářet řada Bootstrap tříd namísto jedné.

Mám k tomu ale pár dotazů:
1) Je to vůbec dobrý nápad předělávat Bootstrap třídy na vlastní styl ve `style scoped`?
2) Pokud je to dobrý nápad, neměl bych pak například ve `frontend\src\assets\css` vytvořit soubory pro sdílené třídy, které se vyskytují v projektu vícekrát, jako je třeba třída `container`?
3) Pokud je dobré vytvořit sdílené třídy, pak mě zajímá, zda ve Vue se používá formát jako v Django, kde se udělá jeden soubor pro CSS a kde jsou všechny kódy uvedené, nebo je ve Vue preferovaný způsob, že se vytvoří samostatné soubory (např. `container.css`) a ty se pak budou importovat jen do těch souborů, kde jsou potřeba. Teoreticky si totiž říkám, že mít jeden dlouhý soubor a v něm vyhledávat nastavení pro danou třídu musí být náročnější úkon, než načíst soubor jen s nastavením pro danou třídu - ale nevím, jestli to zase nezpomalí otevírání souboru.
4) Je kromě složky: `frontend\src\assets\css` ještě někde v projektu další nějaký základní soubor s CSS, který by ovlivňoval vzhled aplikace?

---

**Dotaz:**

Tak jsem začal vypreparovávat kód ze souboru `static/css/style.default.css`, kde je uložený Bootstrap CSS. Všiml jsem si, že hned na začátku tohoto souboru je položka `:root`, kde je hromada definovaných proměnných:

Dá se tohoto přístupu využít i nějak ve Vue, nebo mám z tohoto souboru hodnoty proměnných vypreparovat a vložit do CSS souborů, kde jsou tyto proměnné použity?

---

**Dotaz:**

Dále se mi podařilo vytáhnout všechny styly pro `container`:

Kouknul by ses prosím na ně, zda jsou takto zapsané v pořádku a zda je budu všechny potřebovat (nejsem si jistý, zda tu má být uvedená položka `.navbar > .container`, zda nemá být uvedena až u kódu, kde budu řešit `navbar`)?

---

**Dotaz:**

A nebylo by lepší tyto styly importovat až v komponentách, kde jsou použity? 

---

**Dotaz:**

Dá se vytvořit jeden specifický styl pro určitý prvek, který by v sobě obsahoval více importovaných stylů? Myslím tím, že by je byl schopen sdružit tak, aby v samotné třídě elementu mohl být namísto všech globálních stylů týkajících se daného prvku uveden jen jeden, který by tyto globální prvky sdružoval a přidával k nim vlastní styly?

---

**Dotaz:**

Co znamenají tyto dva styly a kde bych je měl mít umístěné?

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}

@media (prefers-reduced-motion: no-preference) {
  :root {
    scroll-behavior: smooth;
  }
}
```

---

**Dotaz:**

Podle čeho se mám rozhodnout, zda umístit import do `main.js` nebo do `App.vue`?

---

**Dotaz:**

Patří toto také do globálního stylu?

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

---

**Dotaz:**

Co znamená tento styl?

```css
abbr[title],
abbr[data-bs-original-title] {
  -webkit-text-decoration: underline dotted;
  text-decoration: underline dotted;
  cursor: help;
  -webkit-text-decoration-skip-ink: none;
  text-decoration-skip-ink: none;
}
```

---

**Dotaz:**

Takže je vázán na tagy `<abbr>`? Ptám se, protože jsem si v templates v Django nechal vyhledat `abbr` a nenašlo mi to nic, takže nevím, jestli ho vůbec potřebuji.