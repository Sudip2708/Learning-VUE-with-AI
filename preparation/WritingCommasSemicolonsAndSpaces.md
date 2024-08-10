# Psaní čárek, středníků a mezer v JavaScriptu a Vue.js
[*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue)   

Psaní čárek, středníků a mezer v JavaScriptu a Vue.js (a v obecném kódu) je důležité pro udržení čitelnosti a správného fungování kódu. Zde je přehled, jak a kde se tyto znaky používají.

## Obsah:
[• Čárky (,)](#1-čárky-)  
[• Středníky (;)](#2-středníky-)  
[• Mezery](#3-mezery)  
[• Shrnutí](#shrnutí)


## 1. Čárky (,)

### Použití:

**Oddělení prvků v seznamech**:
Čárky se používají k oddělení jednotlivých prvků v seznamech (arrays) a objektech (objects).

    const fruits = ['apple', 'banana', 'cherry'];
    const person = { name: 'John', age: 30, city: 'New York' };

**Oddělení parametrů ve funkcích**:
Čárky oddělují jednotlivé parametry funkcí.

    function greet(name, age) {
      console.log(`Hello ${name}, you are ${age} years old.`);
    }

**Oddělení jednotlivých položek v seznamu importů**:
Čárky oddělují importy z modulů.

    import { ref, computed } from 'vue';

### Pozor na:

**Poslední čárku**:
V některých jazykových variantách (např. v JavaScriptu ES5) by čárka na konci seznamu (tzv. trailing comma) mohla způsobit problémy v určitých prohlížečích nebo prostředích. Od ES2017 (ES8) je ale trailing comma povolená a běžně používaná v moderních JavaScriptových projektech.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 
## 2. Středníky (;)

### Použití:

**Oddělení příkazů**:
Středníky oddělují jednotlivé příkazy v JavaScriptu. V moderním JavaScriptu je to často volitelné, protože JavaScript má automatické semikolonové zakončení (ASI).

    let x = 10;
    let y = 20;
    let sum = x + y;

### Pozor na:

**Když ASI selže**:
I když většina středníků může být vynechána kvůli ASI, je dobré používat středníky na konci příkazů, aby se předešlo nejednoznačnostem a chybám. Například, ASI může selhat při použití return statement a dalšího výrazu na stejné řádce.

    function getValue() {
      return
      { value: 42 }; // Tohle se nebude chovat, jak byste čekali
    }

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 
## 3. Mezery

### Použití:

**Mezery kolem operátorů**:
Používejte mezery kolem operátorů (+, -, *, /, =) pro zlepšení čitelnosti.

    let sum = x + y;

**Mezery za čárkami**:
Používejte mezery za čárkami v seznamech a parametrech pro zlepšení čitelnosti.

    const person = { name: 'John', age: 30, city: 'New York' };

**Mezery po klíčových slovech**:
Používejte mezery po klíčových slovech (např. if, for, function).

    if (x > 10) {
      console.log('x is greater than 10');
    }

### Pozor na:

**Nadbytečné mezery**:
Vyhněte se nadbytečným mezerám, které mohou zhoršit čitelnost kódu. Například mezera před závorkou není obvyklá.

    // Špatně
    function greet (name) {
      console.log('Hello ' + name);
    }
    
    // Správně
    function greet(name) {
      console.log('Hello ' + name);
    }

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 
## Shrnutí
- **Čárky**: Používejte pro oddělení prvků, parametrů a položek importů. Trailing commas jsou v moderním JavaScriptu povoleny, ale buďte si vědomi jejich použití v některých prostředích.
- **Středníky**: Používejte je pro oddělení příkazů, i když moderní JavaScript podporuje automatické semikolonové zakončení (ASI). Vždy je lepší je používat, aby se předešlo nejednoznačnostem.
- **Mezery**: Používejte je pro zlepšení čitelnosti kolem operátorů, za čárkami a po klíčových slovech. Vyhněte se nadbytečným mezerám.

Používání těchto konvencí přispívá k lepší čitelnosti, údržbě a konzistenci kódu.

[*zpět na obsah*](#obsah) / [*zpět na readme*](https://github.com/Sudip2708/learning-VUE-with-the-help-of-AI#1-krok---sezn%C3%A1men%C3%AD-se-s-vue) 