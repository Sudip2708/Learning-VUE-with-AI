### 1) Seznam stručných popisů otázek pro obsah

# Obsah:

[• *Seznámení se stromovou strukturou projektu*](#dotaz)  
[• *Analýza aplikace a struktury templates pro Vue*](#dotaz-1)  
[• *První kroky po analýze projektu*](#dotaz-2)  
[• *První komponenty a routování ve Vue*](#dotaz-3)  
[• *Obsah souboru App.vue*](#dotaz-4)  
[• *Řešení chyb ESLint a Prettier ve Vue*](#dotaz-5)  
[• *Význam export default v App.vue*](#dotaz-6)  

### 2) Oprava gramatiky a překlepů v otázkách

---

**Dotaz:**

OK, tak tady už je asi dobré, aby jsi věděl něco více o mém projektu.

Zde je stromová struktura projektu s vysvětlením jednotlivých složek:

Vem si na prozkoumání těchto dvou struktur klidně i více času a rád bych potom nejprve slyšel slovní rozbor obou struktur, abych věděl, že jsi je pochopil, a poté se budu ptát dál.

---

**Dotaz:**

Nejprve mi prosím popiš mou aplikaci dle toho, co dokážeš ze stromové struktury vyčíst.
A poté mi zhodnoť i strukturu pro templates a zda pro Vue bude struktura podobná.

---

**Dotaz:**

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

---