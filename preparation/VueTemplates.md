<a id="top"></a>
# Direktivy a funkce v 'templates' ve VUE
[*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

Tento seznam obsahuje základní a nejběžněji používané direktivy a funkce v templates Vue.js.   
Můžete je použít k řízení zobrazení, manipulaci s daty a interakci s uživatelským rozhraním ve vašich Vue.js aplikacích.

### Obsah:

**Dynamické vázání**:
- **[v-bind](#v-bind)**: Dynamicky binduje atributy a vlastnosti k elementům.
- **[v-bind](#v-bind)**: Dynamicky binduje CSS třídy.
- **[v-bind](#v-bind)**: Dynamicky binduje inline styly.
- **[v-bind](#v-bind)**: Dynamicky binduje třídy CSS ke elementům. Můžeš použít objekt nebo pole pro definování tříd.
- **[v-bind](#v-bind)**: Dynamicky binduje inline styly k elementům. Můžeš použít objekt pro definování stylů.
- **[v-bind](#v-bind)**: Dynamicky binduje hodnotu atributu href pro <a> tagy.
- **[v-bind](#v-bind)**: Dynamicky binduje hodnotu atributu src pro <img> tagy.

**Dvoustranné vázání**
- **[v-model](#v-model)**: Dvoustranné vázání dat na formulářové prvky.
- **[v-model.lazy](#v-model.lazy)**: Modifikátor pro v-model, který aktualizuje data pouze při události change, nikoli při každé události input.
- **[v-model.number](#v-model.number)**: Modifikátor pro v-model, který konvertuje hodnotu z formulářového pole na číslo.
- **[v-model.trim](#v-model.trim)**: Modifikátor pro v-model, který odstraňuje bílé znaky z začátku a konce vstupního textu.

**Podmínkové vykreslování**
- **[v-if](#v-if)**: Podmínkově vykresluje elementy na základě podmínky.
- **[v-else-if](#v-else-if)**: Používá se spolu s v-if pro další podmínky.
- **[v-else](#v-else)**: Používá se spolu s v-if pro vykreslení, když není splněna žádná předchozí podmínka.

**Iterace**
- **[v-for](#v-for)**: Iteruje přes seznam a vykresluje elementy na základě iterovaných dat.
- **[: ](#:)**: Pomocná direktiva pro poskytování unikátního klíče v v-for cyklu.

**Eventy**
- **[v-on](#v-on)**: Připojuje event listenery k elementům.
- **[v-on](#v-on)**: Specifické použití v-on pro přidání klikacího event listeneru (i když je to zástupné pro v-on
, které už je zmíněno).

**Reaktivita a životní cyklus**
- **[v-show](#v-show)**: Podmínkově zobrazuje nebo skrývá elementy, ale zachovává je v DOM.
- **[v-once](#v-once)**: Vykresluje element pouze jednou a nikdy neaktualizuje.

**Sloty**
- **[v-slot](#v-slot)**: Používá se pro definování slotů v komponentách.

**Speciální direktivy**
- **[v-pre](#v-pre)**: Přeskakuje překlad a reaktivní zpracování pro elementy a jejich děti.
- **[v-cloak](#v-cloak)**: Používá se pro skrytí obsahu šablony, dokud Vue.js nedokončí její připojení.
- **[v-text](#v-text)**: Nastavuje text obsah elementu, přepíše existující text.
- **[v-html](#v-html)**: Vkládá HTML obsah do elementu.


## Příklady:

### v-bind
Dynamicky binduje atributy a vlastnosti k elementům:

    <img v-bind:src="imageUrl" alt="Image">

Dynamicky binduje CSS třídy:

    <div v-bind:class="{ active: isActive }"></div>

Dynamicky binduje inline styly:

    <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

Dynamicky binduje třídy CSS ke elementům:   
)Můžeš použít objekt nebo pole pro definování tříd.)

    <div v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
    <!-- nebo -->
    <div v-bind:class="[activeClass, errorClass]"></div>

Dynamicky binduje inline styly k elementům:  
(Můžeš použít objekt pro definování stylů.)

    <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

Dynamicky binduje hodnotu atributu href pro 'a' tagy:

    <a v-bind:href="url">Link</a>

Dynamicky binduje hodnotu atributu src pro 'img' tagy:

    <img v-bind:src="imageSrc" alt="Image">

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-model
Dvoustranné vázání dat na formulářové prvky.
    
    <input v-model="message" />

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-model.lazy
Modifikátor pro v-model, který aktualizuje data pouze při události change, nikoli při každé události input.

    <input v-model.lazy="message" type="text">

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-model.number
Modifikátor pro v-model, který konvertuje hodnotu z formulářového pole na číslo.

    <input v-model.number="age" type="number">

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-model.trim
Modifikátor pro v-model, který odstraňuje bílé znaky z začátku a konce vstupního textu.

    <input v-model.trim="text" type="text">

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-if
Podmínkově vykresluje elementy na základě podmínky.

    <p v-if="isVisible">Visible content</p>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-else-if
Používá se spolu s v-if pro další podmínky.

    <p v-else-if="isAnotherCondition">Another condition</p>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-else
Používá se spolu s v-if pro vykreslení, když není splněna žádná předchozí podmínka.

    <p v-else>No content available</p>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-for
Iteruje přes seznam a vykresluje elementy na základě iterovaných dat.

    <ul>
      <li v-for="item in items" :key="item.id">{{ item.name }}</li>
    </ul>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-on
Připojuje event listenery k elementům.

    <button v-on:click="handleClick">Click me</button>

Specifické použití v-on pro přidání klikacího event listeneru.

    <button v-on:click="handleClick">Click me</button>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-show
Podmínkově zobrazuje nebo skrývá elementy, ale zachovává je v DOM.

    <p v-show="isVisible">This will be shown or hidden</p>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-slot
Používá se pro definování slotů v komponentách.

    <template v-slot:header>
      <h1>Header Content</h1>
    </template>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-pre
Přeskakuje překlad a reaktivní zpracování pro elementy a jejich děti.

    <span v-pre>{{ rawText }}</span>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-cloak: Používá se pro skrytí obsahu šablony, dokud Vue.js nedokončí její připojení.

    <div v-cloak>Hidden until Vue is ready</div>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-once
Vykresluje element pouze jednou a nikdy neaktualizuje.

    <p v-once>This will not change</p>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### : 
Pomocná direktiva pro poskytování unikátního klíče v v-for cyklu.

    <li v-for="item in items" :key="item.id">{{ item.name }}</li>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-text
Nastavuje text obsah elementu, přepíše existující text.

    <span v-text="textContent"></span>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 

### v-html
Vkládá HTML obsah do elementu.

    <div v-html="htmlContent"></div>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 



