<a id="top"></a>
# Metody a funkce pro 'script setup' ve VUE
[*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

Tento seznam obsahuje některé z nejběžněji používaných metod a funkcí ve script-setup Vue.js komponentách. 
Tyto metody a funkce poskytují silné nástroje pro práci s reaktivními daty, sledování změn, správu životního cyklu komponent a mnoho dalšího.

### 15 nejpoužívanějších:

1. **[ref](#ref)**: Vytváří reaktivní proměnné pro primitivní typy dat (např. čísla, řetězce).
2. **[reactive](#reactive)**: Vytváří reaktivní objekt, který sleduje změny ve svých vlastnostech.
3. **[computed](#computed)**: Vytváří vypočítanou vlastnost, která automaticky reaguje na změny ve svých závislostech.
4. **[watch](#watch)**: Sledování změn specifikovaných reaktivních hodnot nebo funkcí a reagování na ně.
5. **[onMounted](#onmounted)**: Spustí se, když je komponenta připojena do DOM.
6. **[onUpdated](#onupdated)**: Spustí se, když je komponenta aktualizována.
7. **[defineProps](#defineprops)**: Používá se k definování vstupních parametrů komponenty.
8. **[defineEmits](#defineemits)**: Používá se k definování a vyvolávání událostí v komponentě.
9. **[provide and inject](#provide-and-inject)**: provide umožňuje předat hodnotu z rodičovské komponenty do podřízených komponent, inject umožňuje přístup k těmto hodnotám v podřízených komponentách.
10. **[nextTick](##nexttick)**: Spustí kód po dokončení další aktualizace DOM.
11. **[useSlots](#useslots)**: Získá sloty předané do komponenty.
12. **[useAttrs](#useattrs)**: Získá atributy předané do komponenty.
13. **[h](#h)**: Vytváří virtuální DOM node.
14. **[isRef](#isref)**: Kontroluje, zda je hodnota referencí.
15. **[isReactive](#isreactive)**: Kontroluje, zda je hodnota reaktivní objekt.

### Obsah:

**Reaktivita a reaktivní proměnné**:
- **[reactive](#reactive)**: Vytváří reaktivní objekt, který sleduje změny ve svých vlastnostech.
- **[ref](#ref)**: Vytváří reaktivní proměnnou pro primitivní typy dat (např. čísla, řetězce).
- **[computed](#computed)**: Vytváří vypočítanou vlastnost, která automaticky reaguje na změny ve svých závislostech.
- **[shallowReactive](#shallowreactive)**: Vytváří reaktivní objekt, který sleduje pouze první úroveň vlastností.
- **[shallowRef](#shallowref)**: Vytváří reaktivní referenci, která sleduje pouze první úroveň.
- **[readonly](#readonly)**: Vytváří objekt pouze pro čtení.
- **[shallowReadonly](#shallowreadonly)**: Vytváří pouze pro čtení reaktivní objekt, který sleduje pouze první úroveň vlastností.
- **[customRef](#customref)**: Vytváří vlastní referenci s plnou kontrolou nad reaktivitou.
- **[toRefs](#torefs)**: Převádí reaktivní objekt na sadu referencí.
- **[triggerRef](#triggerref)**: Manuálně spouští reaktivní aktualizaci pro shallowRef.

**Kontrola reaktivity**:
- **[isRef](#isref)**: Kontroluje, zda je hodnota referencí.
- **[isReactive](#isreactive)**: Kontroluje, zda je hodnota reaktivní objekt.
- **[isReadonly](#isreadonly)**: Kontroluje, zda je hodnota pouze pro čtení.
- **[markRaw](#markraw)**: Označí objekt jako "surový", takže nebude reaktivní.

**Sledování a efekt**:
- **[watch](#watch)**: Sledování změn specifikovaných reaktivních hodnot nebo funkcí a reagování na ně.
- **[watchEffect](#watcheffect)**: Automaticky sleduje všechny reaktivní závislosti uvnitř předané funkce a spustí ji, když se něco změní.
- **[watchPostEffect](#watchposteffect)**: Podobné jako watchEffect, ale spustí se až po vykreslení změn v DOM.
- **[watchSyncEffect](#watchsynceffect)**: Podobné jako watchEffect, ale spustí se synchronně před aktualizací DOM.

**Životní cyklus komponenty**:
- **[onBeforeMount](#onbeforemount)**: Spustí se těsně před připojením komponenty do DOM.
- **[onMounted](#onmounted)**: Spustí se, když je komponenta připojena do DOM.
- **[onBeforeUpdate](#onbeforeupdate)**: Spustí se těsně před aktualizací komponenty.
- **[onUpdated](#onupdated)**: Spustí se, když je komponenta aktualizována.
- **[onBeforeUnmount](#onbeforeunmount)**: Spustí se těsně před odstraněním komponenty z DOM.
- **[onUnmounted](#onunmounted)**: Spustí se, když je komponenta odstraněna z DOM.

**Komunikace mezi komponentami**:
- **[provide and inject](#provide-and-inject)**: provide umožňuje předat hodnotu z rodičovské komponenty do podřízených komponent, inject umožňuje přístup k těmto hodnotám v podřízených komponentách.
- **[defineProps](#defineprops)**: Používá se k definování vstupních parametrů komponenty.
- **[defineEmits](#defineemits)**: Používá se k definování a vyvolávání událostí v komponentě.

**Práce s DOM a šablonami**:
- **[h](#h)**: Vytváří virtuální DOM node.
- **[resolveComponent](#resolvecomponent)**: Dynamicky řeší komponentu.
- **[resolveDirective](#resolvedirective)**: Dynamicky řeší direktivu.

**Atributy, sloty a CSS moduly**:
- **[useAttrs](#useattrs)**: Získá atributy předané do komponenty.
- **[useSlots](#useslots)**: Získá sloty předané do komponenty.
- **[useCssModule](#usecssmodule)**: Získá CSS moduly připojené ke komponentě.

**Ostatní**:
- **[defineExpose](#defineexpose)**: Umožňuje explicitně určit, které vlastnosti a metody by měly být vystaveny při použití komponenty.
- **[getCurrentInstance](#getcurrentinstance)**: Získá aktuální instanci komponenty.
- **[useSSRContext](#usessrcontext)**: Používá se pro získání kontextu během SSR (Server-Side Rendering).
- **[nextTick](##nexttick)**: Spustí kód po dokončení další aktualizace DOM.

## Příklady:

### reactive
Vytváří reaktivní objekt, který sleduje změny ve svých vlastnostech.

    import { reactive } from 'vue';
    
    const state = reactive({ count: 0 });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### ref
Vytváří reaktivní proměnnou, která může být použita pro primitivní typy dat (např. čísla, řetězce).

    import { ref } from 'vue';
    
    const count = ref(0);

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### computed
Vytváří vypočítanou vlastnost, která automaticky reaguje na změny ve svých závislostech.

    import { computed } from 'vue';
    
    const doubleCount = computed(() => count.value * 2);

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### watch
Sledování změn specifikovaných reaktivních hodnot nebo funkcí a reagování na ně.

    import { watch } from 'vue';
    
    watch(() => state.count, (newVal, oldVal) => {
      console.log(`count changed from ${oldVal} to ${newVal}`);
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### watchEffect
Automaticky sleduje všechny reaktivní závislosti uvnitř předané funkce a spustí ji, když se něco změní.

    import { watchEffect } from 'vue';
    
    watchEffect(() => {
      console.log(`count is now ${count.value}`);
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### onMounted
Spustí se, když je komponenta připojena do DOM.

    import { onMounted } from 'vue';
    
    onMounted(() => {
      console.log('Component has been mounted');
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### onUnmounted
Spustí se, když je komponenta odstraněna z DOM.

    import { onUnmounted } from 'vue';
    
    onUnmounted(() => {
      console.log('Component has been unmounted');
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### onUpdated
Spustí se, když je komponenta aktualizována.

    import { onUpdated } from 'vue';
    
    onUpdated(() => {
      console.log('Component has been updated');
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### onBeforeMount
Spustí se těsně před připojením komponenty do DOM.

    import { onBeforeMount } from 'vue';
    
    onBeforeMount(() => {
      console.log('Component is about to be mounted');
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### onBeforeUnmount
Spustí se těsně před odstraněním komponenty z DOM.

    import { onBeforeUnmount } from 'vue';
    
    onBeforeUnmount(() => {
      console.log('Component is about to be unmounted');
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### onBeforeUpdate
Spustí se těsně před aktualizací komponenty.

    import { onBeforeUpdate } from 'vue';
    
    onBeforeUpdate(() => {
      console.log('Component is about to be updated');
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### provide-and-inject
provide umožňuje předat hodnotu z rodičovské komponenty do podřízených komponent.   
inject umožňuje přístup k těmto hodnotám v podřízených komponentách.  

    import { provide, inject } from 'vue';
    
    provide('key', 'value');

    const injectedValue = inject('key');

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### defineProps
Používá se k definování vstupních parametrů komponenty.

    import { defineProps } from 'vue';
    
    const props = defineProps(['propA', 'propB']);

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### defineEmits
Používá se k definování a vyvolávání událostí v komponentě.

    import { defineEmits } from 'vue';
    
    const emit = defineEmits(['eventA', 'eventB']);

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### useSlots
Získá sloty předané do komponenty.
    
    import { useSlots } from 'vue';
    
    const slots = useSlots();

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### useAttrs
Získá atributy předané do komponenty.

    import { useAttrs } from 'vue';
    
    const attrs = useAttrs();

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### defineExpose
Umožňuje explicitně určit, které vlastnosti a metody by měly být vystaveny při použití komponenty.

    import { defineExpose } from 'vue';
    
    defineExpose({
      someMethod
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### nextTick
Spustí kód po dokončení další aktualizace DOM.

    import { nextTick } from 'vue';
    
    nextTick(() => {
      // Code to be executed after DOM update
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### h
Vytváří virtuální DOM node.

    import { h } from 'vue';
    
    const vnode = h('div', { class: 'foo' }, 'Hello');

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### toRefs
Převádí reaktivní objekt na sadu referencí, což může být užitečné pro destrukturalizaci a předávání reaktivních vlastností.

    import { reactive, toRefs } from 'vue';
    
    const state = reactive({ count: 0, name: 'Vue' });
    const { count, name } = toRefs(state);

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### isRef
Kontroluje, zda je hodnota referencí.

    import { ref, isRef } from 'vue';
    
    const count = ref(0);
    console.log(isRef(count)); // true

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### isReactive
Kontroluje, zda je hodnota reaktivní objekt.

    import { reactive, isReactive } from 'vue';
    
    const state = reactive({ count: 0 });
    console.log(isReactive(state)); // true

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### isReadonly
Kontroluje, zda je hodnota pouze pro čtení.

    import { readonly, isReadonly } from 'vue';
    
    const state = readonly({ count: 0 });
    console.log(isReadonly(state)); // true

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### readonly
Vytváří objekt pouze pro čtení.

    import { readonly } from 'vue';
    
    const state = readonly({ count: 0 });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### shallowReactive
Vytváří reaktivní objekt, který sleduje pouze první úroveň vlastností.

    import { shallowReactive } from 'vue';
    
    const state = shallowReactive({ count: 0 });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### shallowRef
Vytváří reaktivní referenci, která sleduje pouze první úroveň.

    import { shallowRef } from 'vue';
    
    const count = shallowRef(0);

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### triggerRef
Manuálně spouští reaktivní aktualizaci pro shallowRef.
    
    import { shallowRef, triggerRef } from 'vue';
    
    const count = shallowRef(0);
    triggerRef(count);

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### unref
Vrací hodnotu z reference nebo samotnou hodnotu, pokud není reference.

    import { ref, unref } from 'vue';
    
    const count = ref(0);
    console.log(unref(count)); // 0

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### customRef
Vytváří vlastní referenci s plnou kontrolou nad reaktivitou.

    import { customRef } from 'vue';
    
    function useDebouncedRef(value, delay = 200) {
      let timeout;
      return customRef((track, trigger) => {
        return {
          get() {
            track();
            return value;
          },
          set(newValue) {
            clearTimeout(timeout);
            timeout = setTimeout(() => {
              value = newValue;
              trigger();
            }, delay);
          }
        };
      });
    }
    
    const count = useDebouncedRef(0);

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### watchPostEffect
Podobné jako watchEffect, ale spustí se až po vykreslení změn v DOM.

    import { watchPostEffect } from 'vue';
    
    watchPostEffect(() => {
      console.log(`count is now ${count.value}`);
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### watchSyncEffect
Podobné jako watchEffect, ale spustí se synchronně před aktualizací DOM.

    import { watchSyncEffect } from 'vue';
    
    watchSyncEffect(() => {
      console.log(`count is now ${count.value}`);
    });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### useCssModule
Získá CSS moduly připojené ke komponentě.

    import { useCssModule } from 'vue';
    
    const styles = useCssModule();

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### markRaw
Označí objekt jako "surový", takže nebude dělat reaktivní.

    import { markRaw } from 'vue';
    
    const raw = markRaw({ count: 0 });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### shallowReadonly
Vytváří pouze pro čtení reaktivní objekt, který sleduje pouze první úroveň vlastností.
    
    import { shallowReadonly } from 'vue';
    
    const state = shallowReadonly({ count: 0 });

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### useSSRContext
Používá se pro získání kontextu během SSR (Server-Side Rendering).

    import { useSSRContext } from 'vue';
    
    const ssrContext = useSSRContext();

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### getCurrentInstance
Získá aktuální instanci komponenty.

    import { getCurrentInstance } from 'vue';
    
    const instance = getCurrentInstance();

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### resolveComponent
Dynamicky řeší komponentu, často se používá pro renderování komponent pomocí JSX nebo renderovacích funkcí.

    import { resolveComponent, h } from 'vue';
    
    const MyComponent = resolveComponent('MyComponent');
    
    return () => h(MyComponent);

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

### resolveDirective
Dynamicky řeší direktivu, často se používá pro aplikování direktiv pomocí renderovacích funkcí.

    import { resolveDirective, withDirectives, h } from 'vue';
    
    const myDirective = resolveDirective('my-directive');
    
    return () => withDirectives(h('div'), [[myDirective]]);

[*zpět na začátek*](#top) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   
