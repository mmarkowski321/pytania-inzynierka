# Programowanie Obiektowe (OOP) — Kompletna Notatka do Obrony Pracy Inżynierskiej

## 1) Co to jest OOP i po co istnieje (sens, nie definicja z Wikipedii)

Programowanie obiektowe (OOP) to sposób budowania programów, w którym system opisuje się jako zbiór obiektów, które:

- **mają stan** (pola),
- **mają zachowanie** (metody),
- **mają odpowiedzialność** (co ta część systemu ma robić i jakie reguły pilnować).

### Najważniejsza idea
Logika ma być blisko danych, które obsługuje. Dzięki temu:
- zmiana w regule dotyka jednego miejsca (mniej błędów),
- kod łatwiej czytać, bo zachowanie wynika ze stanu obiektu,
- system jest bardziej odporny na rozbudowę.

### Cel OOP w praktyce: tanie zmiany
- **niski coupling**: zmieniasz mało miejsc,
- **wysoka cohesion**: klasy robią spójne rzeczy,
- mniej „ifologii” i mniej kopiowania logiki.

**Zdanie na obronę:**
> OOP nie jest po to, żeby były klasy, tylko po to, żeby zmiany były tańsze i bezpieczniejsze.

---

## 2) Klasa, obiekt, instancja — stan vs zachowanie — invariants

### Podstawowe pojęcia
- **Klasa**: przepis/typ (np. `Account`)
- **Obiekt (instancja)**: konkretny egzemplarz w pamięci (np. konto użytkownika)
- **Stan**: wartości pól (np. `balance`)
- **Zachowanie**: metody zmieniające/wykorzystujące stan (np. `deposit()`, `withdraw()`)

### Ważne: obiekt nie jest „workiem na dane”
W dobrym OOP obiekt nie jest „workiem na dane”. Obiekt ma prawo zmieniać swój stan tylko w kontrolowany sposób, a jego metody pilnują reguł.

### Invariant = reguła, która zawsze musi być spełniona
Przykłady:
- saldo nie może spaść poniżej 0,
- email musi być poprawny,
- zamówienie nie może przeskoczyć statusu.

Jeśli invariant jest pilnowany przez obiekt, to trudniej go przypadkiem złamać.

---

## 3) 4 filary OOP — rozwinięcie tak, żebyś umiał mówić i rozumiał

### A) Enkapsulacja (hermetyzacja)

#### Definicja
Obiekt ukrywa swój stan i wystawia tylko takie metody, które mają sens biznesowy/domenowy.
**Nie chodzi o samo `private`, tylko o to, żeby obiekt bronił swoich reguł.**

#### W Javie praktycznie:
- pola są zwykle `private`,
- settery tylko gdy mają sens (często wcale),
- zamiast settera: **metody domenowe**:
  - `withdraw(amount)` zamiast `setBalance(x)`
  - `changePassword(old, new)` zamiast `setPassword(new)`
  - `addItem(item)` zamiast `setItems(list)`

#### Po co:
- żeby obiekt sam pilnował invariants,
- żeby inni nie mogli „ustawić” obiektu w zły stan,
- żeby zmiana implementacji pól nie rozwalała całego kodu.

#### Pułapka #1: mutowalne kolekcje
Jeśli `getItems()` zwraca listę i ktoś zrobi `.clear()`, to stan obiektu zmieniono „z zewnątrz” — bez walidacji.

**Rozwiązania:**
- zwracaj widok niemutowalny (`Collections.unmodifiableList()`)
- zwracaj kopię (defensive copy)
- nie dawaj gettera — daj `addItem/removeItem`

#### Pułapka #2: anemiczny model
Klasa ma tylko gettery/settery, a cała logika jest w serwisach. Wtedy:
- invariants są porozrzucane,
- łatwo je ominąć,
- OOP przestaje działać, bo obiekt nic nie „umie”.

### B) Abstrakcja (tu wchodzi interfejs!)

#### Definicja
Klient ma widzieć **co** obiekt robi, a nie **jak** to robi.
**Abstrakcja = kontrakt.**

#### W Javie abstrakcję robisz głównie przez:
- **interfejsy** (`interface`) — najważniejsze,
- czasem klasy abstrakcyjne (`abstract class`).

#### Po co abstrakcja:
- klient zna kontrakt (`PaymentService`), nie implementację (`StripePaymentService`)
- możesz podmienić implementację bez zmiany kodu klienta
- łatwiej testować (w testach podstawiasz fake/mock)

#### Pułapka: interfejs wszędzie „bo ładnie” (YAGNI)
Abstrakcja ma sens, gdy:
- jest zmienność (różne wersje zachowania),
- albo chcesz odciąć logikę od technologii (DB/HTTP).

### C) Dziedziczenie (is-a) + ryzyko

#### Definicja
Podtyp jest rodzajem typu bazowego (`extends`).
Daje wspólny typ i reuse kodu, ale kosztuje:

#### Minusy (ważne do powiedzenia):
- silne sprzężenie z klasą bazową,
- „krucha baza”: zmieniasz klasę bazową i nagle psują się podklasy,
- bardzo łatwo naruszyć LSP.

#### Sygnał, że dziedziczenie jest złe:
- w podklasie pojawia się `UnsupportedOperationException`,
- klient zaczyna robić `instanceof`, bo inaczej „nie działa”.

### D) Polimorfizm (serce OOP)

#### Definicja
Masz jeden typ (interfejs/baza), ale wiele implementacji — i w runtime wybiera się właściwa metoda.

W Javie polimorfizm = overriding + dynamic dispatch.

#### Mega ważne:
- **overriding** (runtime) = polimorfizm
- **overloading** (compile-time) ≠ polimorfizm

#### Po co
Żeby zamiast dopisywać if/switch po typach, dodawać nowe klasy.
To realizuje **OCP**.

---

## 4) Interfejsy w Javie — wytłumaczenie od A do Z

### Co to jest interfejs (najprościej)
Interfejs to kontrakt: mówi jakie metody obiekt musi mieć, ale nie mówi jak (zwykle).

To pozwala pisać kod tak:
```
„pracuję z PaymentService”
```
a nie:
```
„pracuję z StripePaymentService”
```

### Jak się tego używa w praktyce (schemat)

1. Tworzysz interfejs, który opisuje czynność:
   - np. „zapłać”, „zapisz”, „wyślij powiadomienie”

2. Robisz jedną lub więcej implementacji

3. W kodzie klienta używasz tylko interfejsu

To jest **„programowanie do interfejsu"** — i to jest kluczowe w OOP.

### Co daje programowanie do interfejsu (konkrety)
- **wymienność**: zmieniasz implementację bez ruszania kodu klienta,
- **testy**: podstawiasz fake/mock,
- **mniejszy coupling**: klient nie zna szczegółów,
- **OCP**: dodajesz implementację zamiast dopisywać warunki.

### Jak to się łączy z polimorfizmem
Interfejs + różne implementacje = polimorfizm.
W runtime Java wywoła metodę tej implementacji, którą faktycznie masz w obiekcie.

### Kiedy tworzyć interfejs (żeby nie robić YAGNI)
**Twórz interfejs, gdy:**
- spodziewasz się wielu wariantów,
- chcesz odciąć logikę od technologii,
- chcesz łatwo testować.

**Nie twórz interfejsu, gdy:**
- masz jedną prostą klasę i nic nie wskazuje, że będzie inaczej,
- robisz abstrakcję „na przyszłość" bez powodu.

---

## 5) LSP — warunek poprawnego dziedziczenia (tu komisja często dociska)

### LSP (Liskov Substitution Principle)
LSP mówi: **jeśli B extends A, to obiekt B musi działać wszędzie tam, gdzie oczekiwany jest A.**

### Podtyp nie może:
- wymagać więcej (preconditions),
- gwarantować mniej (postconditions),
- łamać invariants bazy,
- wyłączać funkcji (`UnsupportedOperationException`).

### Klasyczny przykład: Square/Rectangle
- logiczne is-a, ale psuje kontrakt
- Square nie może mieć niezależnych szerokości i wysokości jak Rectangle

### W praktyce:
jak masz dużo `instanceof` w kodzie → coś jest nie tak z typami.

---

## 6) Kompozycja vs dziedziczenie (mega ważne i często pytane)

### Czym jest kompozycja w ogóle?

**Kompozycja** to relacja "has-a" (ma-a), w której obiekt składa się z innych obiektów lub używa ich do realizacji swoich funkcji.

**Przykład kompozycji:**
```java
class Car {
    private Engine engine;  // Car HAS-A Engine
    private Wheel[] wheels; // Car HAS-A wheels
    
    public void start() {
        engine.start();  // delegacja do składowego obiektu
    }
}
```

### Kompozycja vs Dziedziczenie — porównanie

| Aspekt | Dziedziczenie (is-a) | Kompozycja (has-a) |
|--------|---------------------|-------------------|
| Relacja | "Jest rodzajem" | "Ma/może użyć" |
| Coupling | Silne sprzężenie | Słabe sprzężenie |
| Elastyczność | Niska (sztywna hierarchia) | Wysoka (dynamiczne) |
| Reużywalność | Kod z klasy bazowej | Komponenty |
| LSP | Może naruszać | Nie dotyczy |

### Dziedziczenie: kiedy

- stabilne is-a,
- podtyp w 100% zastępowalny (LSP),
- hierarchia nie będzie często zmieniana.

**Przykład dobrego dziedziczenia:**
```java
abstract class Animal {
    abstract void makeSound();
}

class Dog extends Animal {
    void makeSound() { System.out.println("Woof"); }
}

class Cat extends Animal {
    void makeSound() { System.out.println("Meow"); }
}
```

### Kompozycja: kiedy

- warianty zachowania (Strategy),
- chcesz elastyczność bez kruchych hierarchii,
- boisz się LSP (bo podtyp nie jest idealnym przypadkiem bazy),
- chcesz składać funkcje jak klocki.

**Przykład kompozycji zamiast dziedziczenia:**
```java
// ZLE - dziedziczenie:
class Stack extends ArrayList { }  // Stack IS-A ArrayList? Nie!

// DOBRZE - kompozycja:
class Stack {
    private List list = new ArrayList();  // Stack HAS-A List
    
    public void push(Object item) {
        list.add(item);
    }
    
    public Object pop() {
        return list.remove(list.size() - 1);
    }
}
```

### Kluczowe zdanie:
> **Dziedziczenie zwiększa coupling, kompozycja go zmniejsza.**

**Zasada "Favor composition over inheritance":**
- Kompozycja jest bardziej elastyczna
- Kompozycja pozwala na runtime zmianę zachowania
- Kompozycja unika problemów z LSP

---

## 7) Strategy i State (czyli jak praktycznie ogarniać zmienność)

### Strategy Pattern

#### Co to jest?
Wzorzec Strategy pozwala na **definiowanie rodziny algorytmów**, enkapsulację każdego z nich i umożliwienie ich wzajemnej zamienności. Strategy pozwala algorytmowi zmieniać się niezależnie od klientów, którzy go używają.

#### Problem, który rozwiązuje
Zamiast wielu `if-else` po typach/wariantach, tworzysz interfejs i różne implementacje.

#### Przykład - system rabatów:
```java
// PRZED (złe - dużo ifów):
class Order {
    void applyDiscount(String type) {
        if (type.equals("STUDENT")) {
            price *= 0.9;
        } else if (type.equals("SENIOR")) {
            price *= 0.85;
        } else if (type.equals("VIP")) {
            price *= 0.8;
        }
    }
}

// PO (Strategy):
interface DiscountStrategy {
    double apply(double price);
}

class StudentDiscount implements DiscountStrategy {
    public double apply(double price) {
        return price * 0.9;
    }
}

class SeniorDiscount implements DiscountStrategy {
    public double apply(double price) {
        return price * 0.85;
    }
}

class Order {
    private DiscountStrategy discount;
    
    void setDiscount(DiscountStrategy discount) {
        this.discount = discount;
    }
    
    double getFinalPrice() {
        return discount.apply(price);
    }
}
```

#### Korzyści Strategy:
- **OCP**: dodajesz nowy rabat = nowa klasa, nie modyfikujesz istniejącego kodu
- **Usuwa ify**: kod jest czystszy
- **Testowalność**: każdy algorytm testujesz osobno
- **Niski coupling**: Order nie zna szczegółów rabatów

### State Pattern

#### Co to jest?
Wzorzec State pozwala obiektowi **zmieniać swoje zachowanie**, gdy zmienia się jego stan wewnętrzny. Wygląda to tak, jakby obiekt zmienił swoją klasę.

#### Problem, który rozwiązuje
Zamiast `switch` po statusach, każdy status jest osobną klasą.

#### Przykład - status zamówienia:
```java
// PRZED (złe - switch w każdym miejscu):
class Order {
    private String status;
    
    void cancel() {
        switch(status) {
            case "NEW": status = "CANCELLED"; break;
            case "PAID": // trzeba zwrócić pieniądze
            case "SHIPPED": throw new Exception("Nie można anulować");
        }
    }
    
    void pay() {
        switch(status) {
            case "NEW": status = "PAID"; break;
            case "PAID": throw new Exception("Już opłacone");
        }
    }
}

// PO (State):
interface OrderState {
    void cancel(Order order);
    void pay(Order order);
    void ship(Order order);
}

class NewOrderState implements OrderState {
    public void cancel(Order order) {
        order.setState(new CancelledState());
    }
    
    public void pay(Order order) {
        order.setState(new PaidState());
    }
    
    public void ship(Order order) {
        throw new IllegalStateException("Nie można wysłać przed opłaceniem");
    }
}

class PaidState implements OrderState {
    public void cancel(Order order) {
        refund(order);
        order.setState(new CancelledState());
    }
    
    public void ship(Order order) {
        order.setState(new ShippedState());
    }
}

class Order {
    private OrderState state = new NewOrderState();
    
    void cancel() {
        state.cancel(this);
    }
    
    void setState(OrderState state) {
        this.state = state;
    }
}
```

#### Korzyści State:
- **Usuwa switchy**: każdy status ma swoją logikę
- **OCP**: nowy status = nowa klasa
- **Czytelność**: zachowanie jest w odpowiednim stanie
- **Bezpieczeństwo**: niemożliwe przejścia są niemożliwe w kodzie

---

## 8) Coupling i Cohesion — czyli „co jest dobre w projekcie"

### Coupling (Sprzężenie)

#### Definicja
**Coupling** to miara zależności między modułami/klasami. Mówi, jak bardzo jedna klasa zależy od drugiej.

#### Rodzaje coupling (od najlepszego do najgorszego):

1. **No coupling** - klasy niezależne
2. **Data coupling** - przekazywanie prostych danych (parametry metod)
3. **Stamp coupling** - przekazywanie złożonych struktur (ale tylko potrzebnych pól)
4. **Control coupling** - jedna klasa kontroluje zachowanie drugiej (flagi, enumy)
5. **External coupling** - zależność od zewnętrznego standardu/interfejsu
6. **Common coupling** - wiele klas używa globalnych danych
7. **Content coupling** - jedna klasa bezpośrednio modyfikuje wewnętrzny stan drugiej

#### Dlaczego niski coupling jest ważny?
- **Łatwość zmian**: zmieniasz jedną klasę, nie wpływa na inne
- **Testowalność**: łatwiej testować izolowane komponenty
- **Zrozumiałość**: mniej zależności = łatwiej zrozumieć kod
- **Reużywalność**: klasa z niskim coupling jest bardziej niezależna

#### Jak osiągnąć niski coupling?
- **Programowanie do interfejsu** - zależność od abstrakcji, nie konkretnych klas
- **Enkapsulacja** - ukrywanie szczegółów implementacji
- **Dependency Injection** - przekazywanie zależności z zewnątrz
- **Law of Demeter** - obiekt komunikuje się tylko z bezpośrednimi znajomymi

#### Przykład wysokiego coupling (złe):
```java
class Order {
    void process() {
        PaymentService ps = new StripePaymentService();  // coupling do konkretnej klasy
        Database db = Database.getInstance();  // coupling do globalnej instancji
        db.connect();
        ps.charge(amount);
    }
}
```

#### Przykład niskiego coupling (dobre):
```java
class Order {
    private PaymentService paymentService;  // coupling do interfejsu
    
    Order(PaymentService paymentService) {  // Dependency Injection
        this.paymentService = paymentService;
    }
    
    void process() {
        paymentService.charge(amount);  // nie zna implementacji
    }
}
```

### Cohesion (Spójność)

#### Definicja
**Cohesion** to miara tego, jak bardzo elementy klasy (pola, metody) są ze sobą powiązane i współpracują w realizacji jednego, spójnego celu.

#### Rodzaje cohesion (od najgorszej do najlepszej):

1. **Coincidental cohesion** - elementy razem przypadkowo (złe!)
2. **Logical cohesion** - elementy robią podobne rzeczy (np. wszystkie metody operujące na stringach)
3. **Temporal cohesion** - elementy używane w tym samym czasie (np. `initialize()`, `setup()`, `start()`)
4. **Procedural cohesion** - elementy wykonują kroki algorytmu w kolejności
5. **Communicational cohesion** - elementy używają tych samych danych
6. **Sequential cohesion** - output jednej metody jest inputem drugiej
7. **Functional cohesion** - wszystkie elementy realizują jedną, dobrze zdefiniowaną funkcję (idealne!)

#### Dlaczego wysoka cohesion jest ważna?
- **Zrozumiałość**: klasa robi jedną rzecz, łatwo zrozumieć
- **Utrzymywalność**: zmiany są lokalne
- **Reużywalność**: klasa z wysoką cohesion jest bardziej użyteczna
- **Testowalność**: łatwiej testować jedną odpowiedzialność

#### Przykład niskiej cohesion (złe):
```java
class Utils {
    void calculateTax(double amount) { }
    void sendEmail(String to) { }
    void parseXml(String xml) { }
    void formatDate(Date date) { }
    // Metody nie mają ze sobą nic wspólnego!
}
```

#### Przykład wysokiej cohesion (dobre):
```java
class Account {
    private double balance;
    
    void deposit(double amount) {  // operacja na balance
        balance += amount;
    }
    
    void withdraw(double amount) {  // operacja na balance
        if (balance >= amount) {
            balance -= amount;
        }
    }
    
    double getBalance() {  // operacja na balance
        return balance;
    }
    
    // Wszystkie metody operują na tym samym stanie (balance)
    // i realizują spójny cel: zarządzanie kontem
}
```

### Coupling vs Cohesion — razem

**Idealny kod:**
- **Wysoka cohesion** - klasy robią spójne rzeczy
- **Niski coupling** - klasy są mało zależne od siebie

**Zasada:**
> Staraj się o wysoką cohesion wewnątrz klas i niski coupling między klasami.

#### Jak OOP pomaga?
- **Enkapsulacja + cohesion** → reguły lokalne w obiekcie
- **Abstrakcja + polimorfizm** → mniejsze coupling (klient zależy od kontraktu)

---

## 9) equals/hashCode/== (Java)

### Operator ==
- Porównuje **referencje** (adresy w pamięci)
- Dla typów prymitywnych: porównuje wartości
- Dla obiektów: sprawdza, czy to ten sam obiekt w pamięci

```java
String s1 = new String("test");
String s2 = new String("test");
s1 == s2;  // false! (różne obiekty)

Integer a = 127;
Integer b = 127;
a == b;  // true (cache dla małych liczb)

Integer c = 128;
Integer d = 128;
c == d;  // false! (nie ma cache)
```

### Metoda equals()
- Porównuje **równość logiczną** (czy obiekty reprezentują tę samą wartość)
- Domyślnie: `==` (porównanie referencji)
- **Należy nadpisać** dla własnych klas

```java
String s1 = new String("test");
String s2 = new String("test");
s1.equals(s2);  // true (równość logiczna)
```

### Kontrakt equals():
1. **Refleksywność**: `a.equals(a)` → true
2. **Symetryczność**: `a.equals(b)` ↔ `b.equals(a)`
3. **Przechodniość**: `a.equals(b)` ∧ `b.equals(c)` → `a.equals(c)`
4. **Spójność**: wielokrotne wywołania dają ten sam wynik
5. **Null-safety**: `a.equals(null)` → false

### Metoda hashCode()
- Zwraca liczbę całkowitą reprezentującą obiekt
- Używana w `HashMap`, `HashSet`

### Kontrakt hashCode():
1. **Spójność z equals()**: jeśli `a.equals(b)`, to `a.hashCode() == b.hashCode()`
2. **Spójność**: wielokrotne wywołania dają ten sam wynik (dla niezmiennych obiektów)

### Pułapka: mutowalny klucz w HashMap
```java
Map<Point, String> map = new HashMap<>();
Point p = new Point(1, 2);
map.put(p, "test");

p.setX(3);  // Zmieniamy obiekt!

map.get(p);  // null! Obiekt "zniknął", bo hashCode się zmienił
```

**Rozwiązanie**: używaj niemutowalnych kluczy lub nie modyfikuj kluczy po wstawieniu.

---

## 10) DRY/KISS/YAGNI + SOLID (w teorii i praktyce)

### DRY (Don't Repeat Yourself)

#### Definicja
**Nie duplikuj reguł/logiki** — każda wiedza powinna mieć jedną, jednoznaczną reprezentację w systemie.

#### Przykład naruszenia DRY:
```java
// ZLE - duplikacja logiki walidacji:
class UserService {
    void createUser(String email) {
        if (!email.contains("@")) {
            throw new IllegalArgumentException("Błędny email");
        }
        // ...
    }
}

class OrderService {
    void createOrder(String email) {
        if (!email.contains("@")) {  // DUPLIKACJA!
            throw new IllegalArgumentException("Błędny email");
        }
        // ...
    }
}
```

#### Rozwiązanie (DRY):
```java
class EmailValidator {
    static boolean isValid(String email) {
        return email != null && email.contains("@");
    }
}

// Użycie w wielu miejscach, ale reguła w jednym
```

#### Kiedy NIE stosować DRY?
- **Zbyt wcześnie** - nie duplikuj zanim nie masz 3+ kopii (YAGNI)
- **Różne powody zmian** - jeśli logika może się różnić w przyszłości

### KISS (Keep It Simple, Stupid)

#### Definicja
**Prostota** — rozwiązania powinny być jak najprostsze, ale nie prostsze.

#### Zasady:
- Wybieraj prostsze rozwiązanie, jeśli spełnia wymagania
- Nie komplikuj "na przyszłość"
- Kod ma być czytelny dla innych programistów

#### Przykład naruszenia KISS:
```java
// ZLE - overengineering:
class SimpleCalculator {
    private CalculationStrategyFactory factory;
    private ValidationService validator;
    private LoggingService logger;
    
    public int add(int a, int b) {
        logger.log("Starting addition");
        validator.validate(a, b);
        CalculationStrategy strategy = factory.create(Operation.ADD);
        return strategy.calculate(a, b);
    }
}
```

#### Prostsze rozwiązanie (KISS):
```java
// DOBRZE - proste i czytelne:
class SimpleCalculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

### YAGNI (You Aren't Gonna Need It)

#### Definicja
**Nie buduj funkcji "na przyszłość"** — implementuj tylko to, czego potrzebujesz teraz.

#### Przykład naruszenia YAGNI:
```java
// ZLE - abstrakcja "na przyszłość":
interface PaymentService { }  // Może kiedyś będziemy mieli więcej?

class StripePaymentService implements PaymentService {
    // Jedna implementacja, interfejs nie jest potrzebny
}
```

#### Rozwiązanie (YAGNI):
```java
// DOBRZE - prosty kod, interfejs dodamy gdy będzie potrzebny:
class StripePaymentService {
    // Prosty kod, bez przedwczesnej abstrakcji
}
```

#### Kiedy łamać YAGNI?
- Gdy refaktoryzacja będzie bardzo kosztowna
- Gdy wymagania są pewne (nie spekulacyjne)

---

## SOLID — 5 zasad programowania obiektowego

### S - Single Responsibility Principle (SRP)

#### Definicja
**Klasa powinna mieć tylko jeden powód do zmiany** — tylko jedną odpowiedzialność.

#### Dlaczego?
- Łatwiejsze zrozumienie
- Łatwiejsze testowanie
- Łatwiejsze utrzymanie
- Mniejsze ryzyko wprowadzenia błędów

#### Przykład naruszenia SRP:
```java
// ZLE - klasa robi za dużo:
class User {
    private String name;
    private String email;
    
    // Odpowiedzialność #1: dane użytkownika
    String getName() { return name; }
    
    // Odpowiedzialność #2: walidacja
    boolean isValid() {
        return email.contains("@");
    }
    
    // Odpowiedzialność #3: zapis do bazy
    void save() {
        Database.save(this);
    }
    
    // Odpowiedzialność #4: wysyłka emaila
    void sendEmail(String message) {
        EmailService.send(email, message);
    }
}
```

#### Rozwiązanie (SRP):
```java
// DOBRZE - podział odpowiedzialności:
class User {
    private String name;
    private String email;
    
    String getName() { return name; }
    String getEmail() { return email; }
}

class UserValidator {
    boolean isValid(User user) {
        return user.getEmail().contains("@");
    }
}

class UserRepository {
    void save(User user) {
        Database.save(user);
    }
}

class EmailService {
    void sendEmail(User user, String message) {
        send(user.getEmail(), message);
    }
}
```

### O - Open/Closed Principle (OCP)

#### Definicja
**Klasy powinny być otwarte na rozszerzenia, ale zamknięte na modyfikacje.**

#### Jak to osiągnąć?
- Używaj **abstrakcji** (interfejsy, klasy abstrakcyjne)
- Używaj **polimorfizmu**
- Dodawaj nowe funkcje przez nowe klasy, nie modyfikując starych

#### Przykład naruszenia OCP:
```java
// ZLE - modyfikacja przy dodawaniu nowego typu:
class AreaCalculator {
    double calculateArea(Shape shape) {
        if (shape instanceof Rectangle) {
            Rectangle r = (Rectangle) shape;
            return r.width * r.height;
        } else if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            return Math.PI * c.radius * c.radius;
        }
        // Dodanie nowego typu wymaga modyfikacji!
        throw new IllegalArgumentException();
    }
}
```

#### Rozwiązanie (OCP):
```java
// DOBRZE - otwarte na rozszerzenia:
interface Shape {
    double calculateArea();
}

class Rectangle implements Shape {
    private double width, height;
    
    public double calculateArea() {
        return width * height;
    }
}

class Circle implements Shape {
    private double radius;
    
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class AreaCalculator {
    double calculateArea(Shape shape) {
        return shape.calculateArea();  // Nie trzeba modyfikować przy nowych typach!
    }
}
```

### L - Liskov Substitution Principle (LSP)

#### Definicja
**Obiekty podtypu powinny być zastępowalne obiektami typu bazowego bez zmiany poprawności programu.**

#### Innymi słowy:
Jeśli `B extends A`, to obiekt typu `B` musi działać wszędzie, gdzie używany jest typ `A`.

#### Zasady:
- **Preconditions** nie mogą być silniejsze w podklasie
- **Postconditions** nie mogą być słabsze w podklasie
- **Invariants** klasy bazowej muszą być zachowane
- Metody podklasy nie mogą rzucać nowych wyjątków (chyba że są podtypami wyjątków z bazy)

#### Przykład naruszenia LSP:
```java
// ZLE - Square narusza kontrakt Rectangle:
class Rectangle {
    protected int width, height;
    
    void setWidth(int w) { width = w; }
    void setHeight(int h) { height = h; }
    int getArea() { return width * height; }
}

class Square extends Rectangle {
    void setWidth(int w) {
        width = w;
        height = w;  // Naruszenie kontraktu Rectangle!
    }
    
    void setHeight(int h) {
        width = h;
        height = h;  // Naruszenie kontraktu Rectangle!
    }
}

// Kod klienta załamie się:
void test(Rectangle r) {
    r.setWidth(5);
    r.setHeight(4);
    assert r.getArea() == 20;  // Dla Square będzie 16! BŁĄD!
}
```

#### Rozwiązanie (LSP):
```java
// DOBRZE - kompozycja zamiast dziedziczenia:
interface Shape {
    int getArea();
}

class Rectangle implements Shape {
    private int width, height;
    
    void setWidth(int w) { width = w; }
    void setHeight(int h) { height = h; }
    
    public int getArea() { return width * height; }
}

class Square implements Shape {
    private int size;
    
    void setSize(int s) { size = s; }
    
    public int getArea() { return size * size; }
}
```

### I - Interface Segregation Principle (ISP)

#### Definicja
**Klient nie powinien być zmuszany do zależności od interfejsów, których nie używa.**

#### Innymi słowy:
Twórz **małe, spójne interfejsy** zamiast dużych, "grubych".

#### Przykład naruszenia ISP:
```java
// ZLE - gruby interfejs:
interface Worker {
    void work();
    void eat();
    void sleep();
}

class Human implements Worker {
    public void work() { }
    public void eat() { }
    public void sleep() { }
}

class Robot implements Worker {
    public void work() { }
    public void eat() { throw new UnsupportedOperationException(); }  // Robot nie je!
    public void sleep() { throw new UnsupportedOperationException(); }  // Robot nie śpi!
}
```

#### Rozwiązanie (ISP):
```java
// DOBRZE - segregacja interfejsów:
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

class Human implements Workable, Eatable, Sleepable {
    public void work() { }
    public void eat() { }
    public void sleep() { }
}

class Robot implements Workable {
    public void work() { }  // Tylko to, czego potrzebuje
}
```

### D - Dependency Inversion Principle (DIP)

#### Definicja
**Moduły wysokiego poziomu nie powinny zależeć od modułów niskiego poziomu. Oba powinny zależeć od abstrakcji.**

#### Innymi słowy:
- Zależność od **interfejsów**, nie konkretnych klas
- Używaj **Dependency Injection**

#### Przykład naruszenia DIP:
```java
// ZLE - zależność od konkretnej klasy:
class OrderService {
    private MySQLDatabase database = new MySQLDatabase();  // Zależność od konkretu!
    
    void saveOrder(Order order) {
        database.save(order);
    }
}
```

#### Rozwiązanie (DIP):
```java
// DOBRZE - zależność od abstrakcji:
interface Database {
    void save(Object obj);
}

class MySQLDatabase implements Database {
    public void save(Object obj) { /* implementacja */ }
}

class OrderService {
    private Database database;  // Zależność od abstrakcji
    
    OrderService(Database database) {  // Dependency Injection
        this.database = database;
    }
    
    void saveOrder(Order order) {
        database.save(order);
    }
}
```

#### Korzyści DIP:
- **Testowalność**: łatwo podstawić mock w testach
- **Elastyczność**: zmiana implementacji bez zmiany kodu klienta
- **Niski coupling**: zależność od abstrakcji, nie konkretu

---

## Podsumowanie — kluczowe zdania na obronę

1. **OOP nie jest po to, żeby były klasy, tylko po to, żeby zmiany były tańsze i bezpieczniejsze.**

2. **Enkapsulacja to nie samo `private`, tylko bronienie invariants przez obiekt.**

3. **Programowanie do interfejsu = klucz do elastyczności i testowalności.**

4. **Dziedziczenie zwiększa coupling, kompozycja go zmniejsza.**

5. **Strategy i State eliminują ify/switchy i realizują OCP.**

6. **Wysoka cohesion + niski coupling = dobry kod OOP.**

7. **SOLID to nie teoretyczne zasady, tylko praktyczne wskazówki jak pisać kod, który się łatwo zmienia.**

8. **DRY/KISS/YAGNI — równowaga między duplikacją a przedwczesną optymalizacją.**

