### 1) Seznam stručných popisů otázek pro obsah

# Obsah:

[• *Vliv více spuštěných terminálů na běh systému*](#dotaz)  
[• *Přehled kroků k propojení Vue a Django pro výpis článků*](#dotaz-1)  
[• *Vysvětlení procesu komunikace mezi Django a Vue*](#dotaz-2)  
[• *Jak se Vue dozví o adrese serveru Django*](#dotaz-3)  
[• *Shrnutí postupu pro propojení Django a Vue*](#dotaz-4)  
[• *Detaily pro nastavení django-cors-headers*](#dotaz-5)  
[• *Kontrola nastavení a umístění konfigurace v settings.py*](#dotaz-6)  
[• *Kde umístit nastavení URL pro spolupráci s Vue*](#dotaz-7)  
[• *Význam URL endpointů v Django a jejich použití*](#dotaz-8)  
[• *Návrh na úpravu Django view pro vracení JSON dat*](#dotaz-9)  
[• *Konfigurace URL API endpointů ve Vue.js*](#dotaz-10)  
[• *Kroky pro přidání nové stránky ve Vue*](#dotaz-11)  
[• *Umístění souborů ve Vue do src/views vs. src/components*](#dotaz-12)  
[• *Analýza chyby při načítání dat z API ve Vue*](#dotaz-13)  
[• *Problém s lomítkem na konci URL ve Vue*](#dotaz-14)  
[• *Shrnutí propojení Django s Vue*](#dotaz-15)  

### 2) Oprava gramatiky a překlepů v otázkách

---

**Dotaz:**

Takže Vue s Django komunikuje tím, že odesílá požadavek na adresu serveru Django http://localhost:8000/api/articles/
Ale jak se Vue o této adrese dozví? Jak ví, že má posílat požadavek na http://localhost:8000/api/articles/?

---

**Dotaz:**

Tak se mi chybu povedlo dohledat. Díky našemu společnému rozboru jsem zjistil, že v definici pohledu pro stránku se všemi články ve Vue jsem v tomto řádku:
`const response = await api.get('articles/all');`
měl na konci adresy skutečně ode mě zapsané lomítko a to bylo to, co způsobilo problém.
Nyní se mi stránka už načetla a dokonce i s názvy článků z databáze, takže Vue a Django jsou propojené a pracují dohromady.
Což je tedy paráda, a děkuji za pomoc. Pro dnešek máme splněno.
A na závěr bych chtěl od tebe jen takové poslední shrnutí k tématu propojení Django s Vue.
Podle toho, co jsem za dnešek pochopil, tak v Django si normálně rozjedu server, který hlídá na localhost portu 8000, zda nebude zadána nějaká adresa, či požadavek, který umí zpracovat – to pozná podle URL adresy, že se shoduje s URL adresou definovanou v jeho kódu.
Vue pak do toho vstupuje tak, že je spuštěné na vlastním serveru localhost 8080 a když kliknu na odkaz na stránku se všemi články, axios ve Vue nejprve předělá tento požadavek na adresu localhost portu 8000 a tam ho odešle a čeká na odpověď v podobě JSON souboru. Poté co odpověď přijde a obdrží data, předá je dál pro vytváření stránky se všemi články na localhost 8080 dle kódu uvedeného ve Vue.
Je to tak?

---

**Dotaz:**

Tak se můžeme pustit do dalšího kroku:
Přidat URL endpoint do `urls.py`, který bude mapovat na toto view.

První, co by mě zajímalo, je, zda je lepší pro tuto chvíli umístit nastavení pro URL, která bude spolupracovat s Vue, v souboru `main/urls.py`:

Nebo v souboru `articles/urls/articles.py`:

A jak by tato definice měla vypadat?

---

**Dotaz:**

Takže když jsi mi vytvořil tento bod:

Vytvořit URL pro toto view:

Přidej URL endpoint do `urls.py`, který bude mapovat na toto view.

Kde k definici URL používáš formulaci: `'api/articles/'`

To neznamená to, co jsem si původně myslel, že definice URL adresy musí začínat v Django slovem `'api/'`, ale prakticky své nastavení mohu nechat tak, jak je, a slovo `'api'` zde bylo jen pro ilustrační charakter?