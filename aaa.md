### 1) Seznam stručných popisů otázek pro obsah

# Obsah:

[• *Přenos základní šablony z Django do Vue*](#dotaz)  
[• *Význam chybových hlášení*](#dotaz-1)  
[• *Další význam chybových hlášení*](#dotaz-2)  
[• *Doporučený postup při oddělení frontendu z Django do Vue*](#dotaz-3)  
[• *Výběr umístění statických souborů ve Vue*](#dotaz-4)  
[• *Návrh umístění souborů z Django static a media ve Vue*](#dotaz-5)  
[• *Přenesení nastavení z Django HEAD do Vue*](#dotaz-6)  
[• *Ponechání noscript tagu v public/index.html*](#dotaz-7)  
[• *Kontrola struktury souboru pro rozčlenění*](#dotaz-8)  
[• *Zajištění integrity odkazů pro externí zdroje*](#dotaz-9)  
[• *Přenos hlavičky a struktury navigace z Django do Vue*](#dotaz-10)  
[• *Převod šablon do Vue a oddělení komponent*](#dotaz-11)  
[• *Dočasné vypnutí sémantických chyb od Prettier*](#dotaz-12)  
[• *Dočasné vypnutí chyb od ESLint*](#dotaz-13)  

### 2) Oprava gramatiky a překlepů v otázkách

---

**Dotaz:**

OK, takže jsem zatím přenesl jen statiku s tím, že soubory z media budou přístupné přes Django, takže jsem je zatím neřešil. Teď tedy mám přetaženou statiku a počítám, že jako první bych měl asi zapracovat na kořenu stránky, čímž myslím nastavení v HEAD:

Co z tohoto bych měl přenést a kam?

---

**Dotaz:**

Paráda, tak toto bychom měli, a nyní se můžeme pustit do hlavičky. Zde je můj aktuální kód z Django:

S tím, že tady je ještě pro doplnění stromová struktura souborů pro navigaci a vyklápěcí tlačítko:

---

**Dotaz:**

Jak mohu dočasně vypnout ukazování sémantických chyb od Prettier?

---