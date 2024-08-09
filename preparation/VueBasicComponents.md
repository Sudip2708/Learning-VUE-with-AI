<a id="top"></a>
[*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   


## Dotaz:
Dokázal bys mi popsat základní stromovou strukturu VUE?

## ChatGPT
Samozřejmě! Ve Vue.js je základní stromová struktura tvořena komponentami, které se hierarchicky skládají. Tady je přehled základní struktury:

### Obsah:
1. [Kořenová komponenta](#1-kořenová-komponenta)
2. [Podkomponenty](#2-podkomponenty)
3. [Komponentní strom](#3-komponentní-strom)
4. [Props a Events](#4-props-a-events)
5. [Routing a State Management](#5-routing-a-state-management)
6. [Lifecycle Hooks](#6-lifecycle-hooks)

Tato základní struktura poskytuje přehled o tom, jak jsou komponenty ve Vue.js organizovány a jak mohou interagovat mezi sebou.   
Každý prvek má své vlastní místo v této hierarchii, což usnadňuje správu a rozšiřování aplikace.

### 1. Kořenová komponenta
**Kořenová komponenta (Root Component)**: Je to hlavní komponenta, ze které se celý Vue aplikace odvíjí. Obvykle je to komponenta, která je připojena k HTML elementu na stránce. Je definována v souboru App.vue nebo podobně pojmenovaném souboru.

    <template>
      <div id="app">
        <header-component></header-component>
        <main-content></main-content>
        <footer-component></footer-component>
      </div>
    </template>
    
    <script>
    import HeaderComponent from './components/HeaderComponent.vue'
    import MainContent from './components/MainContent.vue'
    import FooterComponent from './components/FooterComponent.vue'
    
    export default {
      components: {
        HeaderComponent,
        MainContent,
        FooterComponent
      }
    }
    </script>
    
    <style>
    /* Styles for the root component */
    </style>

[*Zpět na začátek*](#top)

### 2. Podkomponenty
**Podkomponenty (Child Components)**: Jsou komponenty, které jsou použity v rámci kořenové komponenty nebo jiných podkomponent. Můžou být znovu použitelné a mají vlastní šablony, skripty a styly.

**HeaderComponent.vue**

    <template>
      <header>
        <h1>My Application</h1>
      </header>
    </template>
    
    <script>
    export default {
      name: 'HeaderComponent'
    }
    </script>
    
    <style scoped>
    header {
      background-color: #f4f4f4;
      padding: 10px;
    }
    </style>

**MainContent.vue**

    <template>
      <main>
        <p>Welcome to the main content area.</p>
      </main>
    </template>
    
    <script>
    export default {
      name: 'MainContent'
    }
    </script>
    
    <style scoped>
    main {
      padding: 20px;
    }
    </style>

**FooterComponent.vue**

    <template>
      <footer>
        <p>&copy; 2024 My Company</p>
      </footer>
    </template>
    
    <script>
    export default {
      name: 'FooterComponent'
    }
    </script>
    
    <style scoped>
    footer {
      background-color: #333;
      color: white;
      text-align: center;
      padding: 10px;
    }
    </style>

[*Zpět na začátek*](#top)

### 3. Komponentní strom
**Komponentní strom (Component Tree)**: Je hierarchická struktura, která ukazuje vztahy mezi komponentami. Kořenová komponenta je vrchol stromu, a její podkomponenty jsou uzly pod tímto vrcholem. Komponenty mohou mít své vlastní podkomponenty, což vytváří víceúrovňový strom.

    App
    ├── HeaderComponent
    ├── MainContent
    └── FooterComponent

[*Zpět na začátek*](#top)

### 4. Props a Events
**Props**: Jsou datové atributy, které komponenty mohou přijímat od jejich rodičovských komponent. Pomocí props se mohou předávat data mezi komponentami.

    <!-- Parent Component -->
    <template>
      <child-component :message="parentMessage"></child-component>
    </template>
    
    <script>
    export default {
      data() {
        return {
          parentMessage: 'Hello from parent!'
        }
      }
    }
    </script>
    
    <!-- Child Component -->
    <template>
      <div>{{ message }}</div>
    </template>
    
    <script>
    export default {
      props: ['message']
    }
    </script>

[*Zpět na začátek*](#top)

**Events**: Komponenty mohou emitovat vlastní události, které mohou být zachyceny rodičovskými komponentami pro zpracování nebo změnu stavu.

    <!-- Parent Component -->
    <template>
      <child-component @custom-event="handleCustomEvent"></child-component>
    </template>
    
    <script>
    export default {
      methods: {
        handleCustomEvent(payload) {
          console.log('Event received with payload:', payload);
        }
      }
    }
    </script>
    
    <!-- Child Component -->
    <template>
      <button @click="emitEvent">Emit Event</button>
    </template>
    
    <script>
    export default {
      methods: {
        emitEvent() {
          this.$emit('custom-event', 'Event Payload');
        }
      }
    }
    </script>

[*Zpět na začátek*](#top)

### 5. Routing a State Management
**Routing (Vue Router)**: Pokud aplikace používá více stránek, Vue Router je nástroj pro správu navigace mezi komponentami a jejich zobrazení na základě URL.

    import Vue from 'vue'
    import Router from 'vue-router'
    import Home from './components/Home.vue'
    import About from './components/About.vue'
    
    Vue.use(Router)
    
    export default new Router({
      routes: [
        { path: '/', component: Home },
        { path: '/about', component: About }
      ]
    })

**State Management (Vuex)**: Pro správu globálního stavu aplikace, Vuex poskytuje centrální úložiště pro všechny komponenty.

    import Vue from 'vue'
    import Vuex from 'vuex'
    
    Vue.use(Vuex)
    
    export default new Vuex.Store({
      state: {
        count: 0
      },
      mutations: {
        increment(state) {
          state.count++
        }
      },
      actions: {
        increment({ commit }) {
          commit('increment')
        }
      }
    })

[*Zpět na začátek*](#top)

### 6. Lifecycle Hooks
**Lifecycle Hooks**: Vue komponenty mají několik životních cyklů, jako jsou created, mounted, updated, a destroyed, které můžeš využít pro inicializaci, aktualizaci nebo čištění komponent.

    export default {
      created() {
        console.log('Component has been created');
      },
      mounted() {
        console.log('Component has been mounted');
      },
      beforeDestroy() {
        console.log('Component will be destroyed');
      }
    }

[*Zpět na začátek*](#top)