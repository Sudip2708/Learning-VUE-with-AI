# Funkce a techniky pro 'style scoped' ve VUE
[*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

## Obsah:
[• *Scopování stylů*](#1-scopování-stylů)  
[• *Dynamic CSS Classes*](#2-dynamic-css-classes)  
[• *Dynamic Inline Styles*](#3-dynamic-inline-styles)  
[• *CSS Variables*](#4-css-variables)  
[• *Scoped CSS Modules*](#5-scoped-css-modules)  
[• *Vue CLI a CSS Pre-processory*](#6-vue-cli-a-css-pre-processory)  
[• *CSS-in-JS*](#7-css-in-js)  
[• *Scoped Slots and Dynamic Styles*](#8-scoped-slots-and-dynamic-styles)  

## Dotaz:
Existují nějaké metody a funkce ve Vue.js, které se používají v rámci 'style scoped', nebo se zde používá pouze čisté CSS?

## ChatGPT
Ve Vue.js se v 'style scoped' používá čistě CSS, ale Vue.js umožňuje využívat některé funkce a techniky, které mohou ovlivnit způsob, jakým se stylování aplikuje. Přímo v 'style scoped' se však Vue.js metody a funkce nepoužívají. Nicméně, zde jsou některé užitečné techniky a možnosti, jak Vue.js může ovlivnit stylování:

### 1. Scopování stylů
Ve Vue.js můžeš použít scoped atribut v 'style' tagu, který zajistí, že styly budou aplikovány pouze na komponenty, kde jsou definovány.

    <template>
      <div class="example">Hello Vue!</div>
    </template>
    
    <style scoped>
    .example {
      color: red;
    }
    </style>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 
### 2. Dynamic CSS Classes
Přidání nebo odebrání CSS tříd na základě reaktivních dat můžeš dosáhnout pomocí v-bind:class:

    <template>
      <div :class="{ active: isActive, 'text-danger': hasError }">Content</div>
    </template>
    
    <style>
    .active {
      color: green;
    }
    .text-danger {
      color: red;
    }
    </style>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 
### 3. Dynamic Inline Styles
Podobně můžeš dynamicky měnit inline styly pomocí v-bind:style:

    <template>
      <div :style="{ color: textColor, fontSize: fontSize + 'px' }">Styled Text</div>
    </template>
    
    <style>
    /* Zde můžeš definovat základní styly */
    </style>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 
### 4. CSS Variables
Vue.js může využívat CSS proměnné pro dynamické změny stylů, což umožňuje změnu stylů na základě reaktivních dat.

    <template>
      <div :style="dynamicStyles">Styled Text</div>
    </template>
    
    <script>
    export default {
      data() {
        return {
          color: 'blue'
        }
      },
      computed: {
        dynamicStyles() {
          return {
            '--main-color': this.color
          }
        }
      }
    }
    </script>
    
    <style scoped>
    div {
      color: var(--main-color);
    }
    </style>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 
### 5. Scoped CSS Modules
Pokud používáš CSS Modules, Vue.js umožňuje použití CSS Modules pro komponenty. Tato technika izoluje CSS a přidává unikátní identifikátory k třídám.

    <template>
      <div :class="$style.example">Hello Vue!</div>
    </template>
    
    <style module>
    .example {
      color: red;
    }
    </style>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 
### 6. Vue CLI a CSS Pre-processory
Pokud používáš Vue CLI, můžeš konfigurovat CSS pre-processory jako Sass, Less nebo Stylus, které umožňují pokročilé možnosti stylování:

    <template>
      <div class="example">Styled with Sass</div>
    </template>
    
    <style lang="scss" scoped>
    .example {
      color: $primary-color;
    }
    </style>
    
    <script>
    export default {
      data() {
        return {
          primaryColor: '#42b983'
        }
      }
    }
    </script>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 
### 7. CSS-in-JS
Pokud používáš knihovny jako styled-components nebo emotion s Vue.js, můžeš psát CSS-in-JS:

    <template>
      <div :style="styledDiv">Styled with CSS-in-JS</div>
    </template>
    
    <script>
    export default {
      computed: {
        styledDiv() {
          return {
            color: 'blue',
            fontSize: '20px'
          }
        }
      }
    }
    </script>

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 
### 8. Scoped Slots and Dynamic Styles
Pokud komponenty mají scoped slots, můžeš dynamicky přizpůsobovat styly podle obsahu slotů:

    <template>
      <my-component>
        <template v-slot:default="{ color }">
          <div :style="{ color }">Slot Content</div>
        </template>
      </my-component>
    </template>
    
    <script>
    export default {
      components: {
        MyComponent
      }
    }
    </script>

V 'style scoped' se používají čistě CSS styly, ale Vue.js nabízí různé techniky a možnosti pro dynamické stylování a integraci stylů s reaktivními daty.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 