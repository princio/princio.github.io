---
layout: page
title: Codifiche testuali
permalink: /dispense/terza/ascii/
---

Le codifiche testuali si effettuano codificando ogni singolo simbolo di un testo, detto _carattere_. Il testo verrà infine codificato come una sequenza di caratteri.

Ogni carattere, a seconda della codifica, viene rappresentato da un numero corrispondente alla sua posizioni nella tabella di conversione. Tale numero, che equivale al numero della riga, è detto `codepoint`.

Vedremo due codifiche:
- La codifica ASCII → ogni carattere codificato in una parola di codice di lunghezza pari a 7 bit.
- La codifica UTF-8 → ogni carattere codificato in una parola di codice di lunghezza variabile (1 byte, 2 byte, 3 byte, 4 byte).

## La tabella di conversione

Le codifiche di caratteri testuali fanno uso di delle tabelle dove ad ogni riga corrisponde un _simbolo_ testuale.

Ad esempio, la codifica ASCII utilizza una tabella di 127 righe, dove ad ogni riga è associato un simbolo.

Esempio:
- Alla riga _0_, è associato il simbolo `NULL`.
- Alla riga _48_ è associato il carattere `0`.
- Alla riga _65_ è associato il carattere `A`.
- Alla riga _97_ è associato il carattere `a`.

Dalla riga _0_ alla riga _31_ (inclusa) troviamo caratteri **non stampabili**, ovvero caratteri non visibili ma utili per definire inizio e fine di un testo, o di una linea.
Dalla riga _32_ alla riga _127_ (inclusa) troviamo i caratteri **stampabili**.

> **IMPORTANTE**: Il numero di riga di un simbolo nella tabella viene chiamato `CODEPOINT`.


## ASCII

La codifica ASCII è semplicemente la codifica del numero di riga (detto codepoint) del carattere/simbolo in 7 bit.
Naturalmente, il numero di riga è senza segno.

- Alfabeto sorgente: caratteri testuali stampabili e non-stampabili.
- Alfabeto codice: ${0, 1}$.
- Lunghezza parola di codice: **fissa**, 7 bit.
- Tabella: tabella ASCII di 127 codepoint (righe).


### Codifica di un carattere/simbolo:


Procedimento:
1. Dato un simbolo, si cerca il codepoint corrispondente.
2. Il codepoint è un numero decimale tra 0 e 127 compresi. Lo si converte in una parola di codice di 7 bit:
  - Simbolo `NULL` →  codepoint `0` → `000 0000`.
  - Simbolo _0_ →  codepoint `48` →  `011 0000`.
  - Simbolo _new-line_ →  codepoint `13` →  `000 1101`.

### Decodifica di un carattere/simbolo:

Procedimento:
1. Dato un codepoint, rappresentato dalla parola di codice a 7 bit, lo si converte in numero decimale.
2. Si cerca nella tabella il simbolo corrispondente alla riga numero uguale al codepoint.

## UTF-8

La codifica UTF-8 (Unicode Transformation Format) funziona sempre con una tabella codepoint↔simbolo, ma tale tabella è lunga 1,112,064 righe.
In altre parole, il codepoint può assumere un valore compreso tra 0 e 1112064 (inclusi), per cui 7 bit non permettono di salvare tutti i caratteri.

La codifica UTF-8 è una codifica a lunghezza variabile, ovvero la parola di codice può assumere tre diverse lunghezze:
- 1 byte.
- 2 byte.
- 3 byte.
- 4 byte.

Lo scopo della codifica è sempre quello di rappresentare in binario il codepoint.
I codepoint di valore ≤127 della tabella UTF-8 sono gli stessi della tabella ASCII, ma con un preceduti da un bit 0.

|    |   ascii        |      utf-8          |
|----|----------------|---------------------|
|`C` | 100 0011       | **0**110 0011       | 

**IMPORTANTE**: i caratteri ASCII vengono rappresentati in UTF-8 con un solo byte, o meglio, la codifica UTF-8 ha scelto di rappresentare i caratteri ASCII con un solo byte perché i caratteri ASCII sono quelli più **frequenti**, ed è intelligente associare ai caratteri più frequenti una parola di codice di lunghezza minore.

I codepoint >127 vengono salvati in un modo particolare, ovvero non vengono codificati direttamente in un numero binario di 16,24,32 bit:
- ❌ `È` →  200 →  `1100 1000` →  `0000 0000 1100 1000`.

Questo perché quando si codifica, non basta _salvare_ il valore del numero, ma anche rendere facile la ricostruzione in fase di decodifica.
Per ricostruirlo, dato che ogni carattere farà parte di una stringa di bit, sarà **essenziale** capire facilmente e **senza ambiguità** dove ogni parola inizia e finisce.
Per cui la codifica in realtà sarà la seguente:
- ✅ `È` → 200 → `1100 1000` →  `1100 0011 1000 1000`.
Per capire:
```
1100 0011 1000 1000
###^ ^^^^ ##^^ ^^^^
```
Dove:
- `#` specificano l'inizio del primo byte se `110`, del secondo byte se `10`.
- `^` specificano i bit del numero 200.

I più attenti noteranno come con due byte, dato che si perdono 5 bit per specificare l'inizio dei singoli byte, non sarà possibile rappresentare $2^{16}$ codepoint, ma solamente $2^{16-5}$.

Per le parole lunghe 3 e 4 byte il concetto è lo stesso.

> **IMPORTANTE**: in una codifica si deve tenere conto anche della fase di decodifica, ovvero di permettere al decodificatore di comprendere facilmente l'inizio e la fine di ogni parola di codice, soprattutto quando la lunghezza è variabile.




Una stringa è una sequenza di caratteri:
```
ciao
Ciro!
```

Perciò una stringa di caratteri codificati in ASCII è una stringa di parola di codice lunghe 7 bit.

* 1000011 → `C`
* 1101001 → `i`
* 1100001 → `a`
* 1101111 → `o`
* 0001010 → `\n` ovvero carattere _a capo_.
* 1000011 → `C`
* 1101001 → `i`
* 1110010 → `r`
* 1101111 → `o`
* 0100000 → ` `
* 1100100 → `d`
* 1100001 → `a`
* 0100000 → ` `
* 0101110 → `.`
* 0101110 → `.`
* 0101110 → `.`

In binario diventa:
```
1000011 1101001 1100001 1101111 0001010 1000011 1101001 
C       i       a       o       \n      C       i       
```
Continua:
```
1110010 1101111 0100000 1100100 1100001 0100000 0101110 0101110 0101110
r       o               d       a               .       .       .
```

# UTF

Lunghezza variabile.

Se la parola inizia — da sinistra — per `0`    → 1Byte → 0XXX XXXX (ASCII).
Se la parola inizia — da sinistra — per `110`  → 2Byte → 110X XXXX  10XX XXXX.
Se la parola inizia — da sinistra — per `1110` → 3Byte → 1111 0XXX  10XX XXXX  10XX XXXX  10XX XXXX.

## Codepoint

> A code point, codepoint or code position is a particular position in a table, where the position has been assigned a meaning.
da [https://en.wikipedia.org/wiki/Code_point](https://en.wikipedia.org/wiki/Code_point),

Quindi il codepoint è il numero che indica, nel caso della tabella ascii o UTF-8, il **numero di riga**:

|       Codepoint         |         |                          |
| Decimale  | Esadecimale | Simbolo | Descrizione              |
|-----------|-------------|---------|--------------------------|
| 114       | 72          | r       | Lowercase r              |
| 115       | 73          | s       | Lowercase s              |
| 116       | 74          | t       | Lowercase t              |
| 117       | 75          | u       | Lowercase u              |
| 118       | 76          | v       | Lowercase v              |
| 119       | 77          | w       | Lowercase w              |
| 120       | 78          | x       | Lowercase x              |
| 121       | 79          | y       | Lowercase y              |
| 122       | 7A          | z       | Lowercase z              |
| 123       | 7B          | {       | Opening brace            |
| 124       | 7C          | \|      | Vertical bar             |
| 125       | 7D          | }       | Closing brace            |
| 126       | 7E          | ~       | Equivalency sign - tilde |
| 127       | 7F          | DEL     | Delete                   |


## Codifica ASCII

La codifica ASCII è molto semplice dato che **ogni parola ha la stessa lunghezza**.



# File

## File testuale

Sequenza di caratteri.

string a = "ciao!"; // ASCII
string a = "è ciao!"; // UTF-8

### Salvare un numero con codifica testuale o con codifica numerica

Il numero → 2018209281029 (13 cifre).

Tale numero richiede:
- con codifica ASCII: 13 × 7 = 91 bit.
- con codifica numerica senza segno: 2018209281029 → ~40bit.




## Immagini

### Immagini: codifica bitmap

uint8 immagine[R][C];    // bianco nero
uint8 immagine[R][C][3]; // colori

### Immagini: codifica jpeg

Grandezza immagini bitmap:
> 10MP -> ×23 bit a colore → 230 Mbit → ~40MByte.

Necessaria codifica.

# I file sono sempre una sequenza di bit lineare

