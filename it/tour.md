# Go

Guida rapida per imparare il linguaggio Go prevalentemente attraverso pezzetti
di codice e qualche nota.

**Nota:** I seguenti contenuti sono presi dal tour interattivo sul sito
ufficiale e fanno riferimento alla versione 1.11.1.

## Hello world

```go
package main

import "fmt"

func main() {
  fmt.Println("Hello world")
}
```

## Export

I package esportano automaticamente le dichiarazioni che iniziano con la
maiuscola.

## Dichiarazioni di tipo

A differenza di altri linguaggi, il tipo appare a destra, es.:

```go
func add(x int, y, int) int {
  // ...
}
```

Inoltre in presenza di più variabili dello stesso tipo si può indicare il tipo
una volta sola:

```go
func add(x, y int) int {
  // ...
}
```

## `return` di più valori

```go
func swap(x, y int) (int, int) {
  return y, x
}
```

## Named return values

```go
func swap(x, y int) (a, b int) {
  a = y
  b = x
  return
}
```

## Dichiarazione e inizializzazione di variabili

```go
var x, y, z int
var cond bool
// ecc.

var a, b = 1, 2 // type inference
c, d := 1, 2 // sintassi equivalente
```

## Tipi predefiniti

`bool` `string` `int` `int8` `int16` `int32` `int64` `uint` `uint8` `uint16`
`uint32` `uint64` `uintptr` `byte` `rune` `float32` `float64` `complex64`
`complex128`

## Zero values

Sono i valori automaticamente assegnati alle variabili non inizializzate. Alcuni
casi comuni:

* `0` per i tipi numerici
* `false` per i booleani
* `""` per le stringhe

## Type casting

```go
f := float64(2)
```

## Costanti

```go
const foo = "bar"
foo = "baz" // Errore
```

## `for`

Si tratta dell'**unica** sintassi per l'iterazione disponibile in Go.

```go
for i := 0; i < 10; i++ {
  sum += i
}
```

Inizializzazione e aggiornamento sono opzionali:

```go
for sum < 1000 {
  sum += sum
}
```

Si può omettere persino la condizione e iterare indefinitamente:

```go
for {
  // ...
}
```

## `if`

```go
if x >= 0 {
  y := math.Sqrt(x)(x)
} else {
  // ...
}
```

## `switch`

```go
switch foo {
  case "bar":
    // ...
  case baz:
    // ...
  default:
    // ...
}
```

**Nota:** Non richiede `break`, dato che di base esegue soltanto il primo `case`
che matcha.

**Nota:** I valori utilizzati nella parte `case` possono essere espressioni di
qualunque genere (es. variabili, chiamate a funzioni) e non solamente valori
letterali e/o numerici come in altri linguaggi.

Si può utilizzare anche la seguente sintassi per evitare catene di `if`-`else`
anche in assenza di una variabile su cui fare `switch`:

```go
switch {
  case conditionA():
    // ...
  case conditionB():
    // ...
  case conditionC():
    // ...
  default:
    // ...
}
```

**Nota:** Tale sintassi è giustificabile assumendo che l'omissione equivalga ad
uno `switch` sul valore letterale `true`.

## `defer`

Permette di rimandare l'esecuzione di una funzione al ritorno di quella
corrente:

```go
func main() {
  defer fmt.Println("world")

  fmt.Println("hello")
}
```

**Nota:** Gli argomenti della funzione, però, vengono valutati
**immediatamente**.

**Nota:** In caso di più utilizzi di `defer`, al ritorno della funzione corrente
le funzioni differite saranno eseguite in ordine last in first out.

## Puntatori

Stessi operatori del C, ovvero `*` per accedere al valore puntato e `&` per
ottenere un puntatore a variabile:

```go
var x string
var xp *string
x = "hello"
xp = &x;
fmt.Println(*xp) // "hello"
```

**Nota:** Non è supportata alcuna aritmetica dei puntatori.

**Nota:** Un puntatore non inizializzato ha valore `nil`.

## Structs

```go
type Point struct {
  X int
  Y int
}

p := Point{0, 0}
p.X = 1
fmt.Println(p.X) // 1
```

Alcune varianti sintattiche per istanziare struct:

```go
p1 := Point{X: 1, Y: -1}
p2 := Point{X: 1} // Y: 0
p3 := Point{} // X: 0, Y: 0
```

## Array

```go
var message [2]string
message[0] = "Hello"
message[1] = "World"

somePrimes := [6]int{2, 3, 5, 7, 11, 13}
fmt.Println(primes) // [2, 3, 5, 7, 11, 13]
```

## Slices

```go
somePrimes := [6]int{2, 3, 5, 7, 11, 13}
var lowerThanTen []int
lowerThanTen = somePrimes[0:4]
fmt.Println(lowerThanTen) // [2, 3, 5, 7]
```

**Nota:** Primo indice incluso, secondo no.

Una slice non è una copia e funziona come un array di riferimenti ai dati
originali:

```go
a := [3]int{1, 2, 3}
b := a[1:3]
b[1] = b[1] * 4
fmt.Println(a) // [1, 2, 12]
```

Le sintassi per le slice prevede dei ragionevoli default:

```go
a := [3]int{1, 2, 3}
b := a[:2] // default 0
fmt.Println(b) // [1, 2]
b = a[1:] // default 3
fmt.Println(b) // [2, 3]
b = a[:] // default 0:3
fmt.Println(b) // [1, 2, 3]
```

**Nota:** Una slice non inizializzata vale `nil`, ha capacità zero e non è
associata ad alcun array.

### Lunghezza e capacità di una slice

* le slice hanno una lunghezza e una capacità
* quest'ultima esprime il numero di elementi nell'array sottostante, partendo
però dal primo elemento della slice
* le funzioni globali `len` e `cap` permettono di recuperare lunghezza e
capacità di una slice
* una slice `s` può essere allargata fin tanto che `len(s) <= cap(s)`

```go
s := []int{2, 3, 5, 7, 11, 13}
fmt.Println(s) // [2, 3, 5, 7, 11, 13]
fmt.Println(len(s)) // 6
fmt.Println(cap(s)) // 6

s = s[:0] // slice vuota
fmt.Println(s) // []
fmt.Println(len(s)) // 0
fmt.Println(cap(s)) // 6

s = s[:4] // allargo a 4 elementi
fmt.Println(s) // [2, 3, 5, 7]
fmt.Println(len(s)) // 4
fmt.Println(cap(s)) // 6

s = s[2:] // elimino i primi 2 elementi
fmt.Println(s) // [5, 7]
fmt.Println(len(s)) // 2
fmt.Println(cap(s)) // 6
```

**Nota:** Meccanismo non molto intuitivo...

La funzione `make` permette di creare slice di lunghezza e (opzionalmente)
capacità definite in maniera dinamica:

```go
n := 5
s := make([]int, n)
fmt.Println(s) // [0, 0, 0, 0, 0]
fmt.Println(len(s)) // 5
fmt.Println(cap(s)) // 5

s = make([]int, 2, 5)
fmt.Println(s) // [0, 0]
fmt.Println(len(s)) // 2
fmt.Println(cap(s)) // 5
```

## `range`

Permette di iterare sugli indici e/o i valori di una slice:

```go
s := []string{"a", "b", "c"}
for i, v := range s {
  fmt.Println(i) // 0, 1, 2
  fmt.Println(v) // "a", "b", "c"
}
```

Si interessa solo l'indice, si può omettere il valore (e viceversa) utilizzando
il simbolo `_`:

```go
s := []string{"a", "b", "c"}
for i, _ := range s {
  // ...
}
for _, v := range s {
  // ...
}
```

## Mappe

```go
var m map[string]int
m = make(map[string]int)
m["a"] = 1
m["b"] = 2
fmt.Println(m) // map[a:1 b:2]
```

Nell'esempio sopra, la mappa è inizializzata con `make`, ma in alternativa è
possibile utilizzare la sintassi apposita per i letterali `map`:

```go
m := map[string]int{
  "a": 1,
  "b": 2,
}
fmt.Println(m) // map[a:1 b:2]
```

**Nota:** Attenzione alla subdola, **obbligatoria** virgola in corrispondenza
anche dell'ultimo elemento della mappa.

**Nota:** Una mappa non inizializzata vale `nil` e non le si possono aggiungere
elementi.

Come cancellare elementi da una mappa:

```go
m := map[string]int{
  "a": 1,
  "b": 2,
}
delete(m, "a")
fmt.Println(m) // map[b:2]
```

Come testare la presenza di una chiave in una mappa:

```go
m := map[string]int{
  "a": 1,
  "b": 2,
}

v, p := m["a"]
fmt.Println(v) // 1
fmt.Println(p) // true

v, p = m["c"]
fmt.Println(v) // 0
fmt.Println(p) // false
```

## Funzioni

Sono utilizzabili come valori:

```go
printThis := func(fn func() string) {
  fmt.Println(fn())
}
getMessage := func() string {
  return "hello"
}
printThis(getMessage) // hello
```

Possono essere utilizzate come closure:

```go
createAdder := func() func(int) int {
  sum := 0
  return func(x int) int {
    sum += x
    return sum
  }
}
adder := createAdder()
count := adder(1)
count = adder(2)
count = adder(3)
fmt.Println(count) // 6
```

## Metodi

```go
type Rectangle struct {
  Width, Length int
}

func (r Rectangle) Area() int {
  return r.Width * r.Length
}

func main() {
  r := Rectangle{3, 4}
  fmt.Println(r.Area()) // 12
}
```

**Nota:** Si possono definire metodi solo per i tipi dichiarati all'interno del
package corrente.

Occorre tenere presente che i metodi agiscono su una copia del ricevitore, a
meno che il ricevitore non sia dichiarato come puntatore:

```go
func (r Rectangle) Scale1(factor int) {
  r.Width = r.Width * factor
  r.Length = r.Length * factor
}

func (r *Rectangle) Scale2(factor int) {
  r.Width = r.Width * factor
  r.Length = r.Length * factor
}

func main() {
  r := Rectangle{3, 4}
  r.Scale1(10)
  fmt.Println(r.Width) // 3
  fmt.Println(r.Length) // 4
  r.Scale2(10)
  fmt.Println(r.Width) // 30
  fmt.Println(r.Length) // 40
}
```

## Interfacce

In Go sono concepite come tipi definiti da un insieme di firme di metodi:

```go
type Shape interface {
  Area() int
}

func main() {
  var s Shape = Rectangle{3, 4}
}
```

L'interfaccia vuota `interface{}` può essere utilizzata per lavorare con
qualunque tipo:

```go
var anything interface{}
anything = 7
anything = "foo"
// ecc.
```

## Type assertions

Permettono, al contempo, di fare un downcasting e di mandare in panic
l'esecuzione qualora non sia effettuabile:

```go
var anything interface{}
anything = 7
i := anything.(int) // Ok
s := anything.(string) // Panic!
```

Qualora si voglia evitare un comportamento così estremo, è possibile ricorrere
ad una doppia assegnazione in cui la seconda variabile memorizzerà l'esito del
downcasting, che comunque non determinerà errori:

```go
var anything interface{}
anything = 7
s, ok := anything.(string)
fmt.Println(s) // ""
fmt.Println(ok) // false
```

Nel caso si vogliano effettuare più test sullo stesso valore, è possibile
utilizzare questa sintassi alternativa di `switch`:

```go
swicth y := x.(type) {
  case int:
    // ...
  case string:
    // ...
  // ecc.
  default:
    // ...
}
```

## Errori

Esiste un'interfaccia predefinita `error`, utilizzata da diverse funzioni come
tipo per un secondo valore di ritorno, allo scopo di segnalare errori, es.:

```go
x, err := doSomething()
if err != nil {
  fmt.Println("Ops")
}
```

## Goroutine

Sono thread leggeri, avviabili tramite parola chiave `go`:

```go
import (
  "fmt"
  "time"
)

func repeat(what string) {
  for {
    time.Sleep(100 * time.Millisecond)
    fmt.Println(what)
  }
}

func main() {
  go repeat("ping")
  repeat("pong")
}
```

**Nota:** Le due scritte dovrebbero alternarsi più o meno equamente.

## Channel

Permettono di scambiare valori di un tipo prefissato fra due goroutine tramite
operatore `<-`:

```go
c <- v // Invia v attraverso il channel c
```

```go
v := <- c  // Salva in v il valore ricevuto da c
```

La creazione di channel può essere effettuata tramite funzione `make`:

```go
c := make(chan int)
```

L'accesso ai channel viene sincronizzato automaticamente, ad esempio due
goroutine possono scrivere sullo stesso channel senza timore di interferenza.
Inoltre lettura e scrittura sono bloccanti fin quando l'altra parte non è
pronta. In alternativa è possibile specificare un buffer così da avere scritture
bloccanti solo a buffer saturo:

```go
c := make(chan int, 100)
```

Quando non ci sono più dati da inviare è possibile chiudere un channel:

```go
close(c)
```

In lettura è possibile testare la chiusura di un channel:

```go
v, stillOpen := <- c
```

Analogamente, è possibile iterare su un channel fino alla sua chiusura:

```go
for v := range c {
  // ...
}
```

## `select`

La parola chiave `select` permette di fermare la goroutine corrente in attesa
di una delle sue condizioni `case` possa essere eseguita:

```go
func marco(c chan string) {
  time.Sleep(15 * time.Millisecond)
  c <- "finish!"
}

func bolt(c chan string) {
  time.Sleep(10 * time.Millisecond)
  c <- "finish!"
}

func main() {
  cm := make(chan string)
  cb := make(chan string)
  go marco(cm)
  go bolt(cb)
  select {
    case <- cm:
      fmt.Println("Marco wins!")
    case <- cb:
      fmt.Println("Bolt wins!")
  }
}
```

**Note:**

* qualora più `case` siano pronti, ne sarà comunque eseguito uno solo, scelto
in maniera casuale (o meglio: secondo criteri non specificati)
* è possibile usare anche `default` per specificare cosa fare se nessun `case` è
pronto

## `sync.Mutex`

Nel package `sync` è presente un tipo `Mutex` con dei metodi `Lock` e `Unlock`
per prevenire l'interferenza tra più goroutine:

```go
func increment(i *int, m sync.Mutex) {
  m.Lock()
  *i = *i + 1
  m.Unlock()
}

func main() {
  i := 0
  m := sync.Mutex{}
  for j := 0; j < 100; j++ {
    go increment(&i, m)
  }
  time.Sleep(5000 * time.Millisecond)
  fmt.Println(i) // 100 (a meno che tu non abbia una pessima macchina!)
}
```
