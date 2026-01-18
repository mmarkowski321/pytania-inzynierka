# Różnice między introspekcją i odzwierciedleniem (refleksją) w Javie — Notatka do Obrony

## 0) Podstawy — od zera

### Co to jest refleksja?
**Refleksja (odzwierciedlenie)** to proces, dzięki któremu program może badać i modyfikować swoją własną strukturę w czasie działania programu.

**Analogia:** Refleksja = program może "spojrzeć w lustro" i zobaczyć siebie — swoje klasy, metody, pola.

### Co to jest introspekcja?
**Introspekcja** to analiza ziarenek JavaBean za pomocą metod refleksji, przy założeniu standardowych wzorców nazewnictwa.

**Różnica kluczowa:** 
- **Refleksja** = uniwersalne narzędzie do analizy i modyfikacji dowolnych klas
- **Introspekcja** = specjalizowane narzędzie do analizy ziarenek JavaBean (używa refleksji pod spodem)

---

## 1) Refleksja (Reflection)

### Definicja
Refleksja to mechanizm, dzięki któremu program może:
- **Badać** swoją strukturę (klasy, pola, metody, konstruktory)
- **Modyfikować** zachowanie w czasie wykonania
- **Dynamicznie** zarządzać kodem (tworzyć obiekty, wywoływać metody, dostęp do pól)

### Pakiet
```java
java.lang.reflect
```

### Obiekt Class — klucz do refleksji

JVM dla każdego obiektu utrzymuje obiekt `Class`, który zawiera informacje o klasie:
- **Pola** (fields)
- **Konstruktory** (constructors)
- **Metody** (methods)

### Sposoby otrzymania obiektu Class

| Metoda | Przykład |
|--------|----------|
| `getClass()` | `MyClass obj = new MyClass();`<br>`Class cls = obj.getClass();` |
| `Class.forName()` | `String nazwaKlasy = "test.MyClass";`<br>`Class cls = Class.forName(nazwaKlasy);` |
| Właściwość `.class` | `Class cls = String.class;`<br>`Class cls = int.class;` |

**Przykład:**
```java
// Sposób 1: getClass()
MyClass obj = new MyClass();
Class<?> cls1 = obj.getClass();

// Sposób 2: Class.forName()
Class<?> cls2 = Class.forName("com.example.MyClass");

// Sposób 3: .class
Class<?> cls3 = MyClass.class;
```

### Klasy pomocnicze refleksji

#### Field (dla pól)
Umożliwia dostęp do pól klasy (również prywatnych).

```java
class Person {
    private String name;
    private int age;
}

// Pobranie pola
Class<?> cls = Person.class;
Field nameField = cls.getDeclaredField("name");

// Dostęp do prywatnego pola (wymaga ustawienia accessible)
nameField.setAccessible(true);

// Pobranie wartości
Person person = new Person();
String name = (String) nameField.get(person);

// Ustawienie wartości
nameField.set(person, "Jan");
```

#### Method (dla metod)
Umożliwia wywołanie metod (podobnie jak wskaźniki do funkcji w C++).

```java
class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}

// Pobranie metody
Class<?> cls = Calculator.class;
Method addMethod = cls.getMethod("add", int.class, int.class);

// Wywołanie metody
Calculator calc = new Calculator();
Object result = addMethod.invoke(calc, 5, 3);
int sum = (Integer) result; // wynik: 8
```

#### Constructor (dla konstruktorów)
Umożliwia tworzenie instancji klas dynamicznie.

```java
// Pobranie konstruktora
Class<?> cls = Person.class;
Constructor<?> constructor = cls.getConstructor(String.class, int.class);

// Utworzenie instancji
Object person = constructor.newInstance("Jan", 25);
Person p = (Person) person;
```

### Przykład praktyczny — refleksja

```java
import java.lang.reflect.*;

class Example {
    private String secret = "Tajemnica";
    
    public void publicMethod() {
        System.out.println("Publiczna metoda");
    }
    
    private void privateMethod() {
        System.out.println("Prywatna metoda");
    }
}

public class ReflectionDemo {
    public static void main(String[] args) throws Exception {
        Example obj = new Example();
        Class<?> cls = obj.getClass();
        
        // Pobranie wszystkich metod
        Method[] methods = cls.getDeclaredMethods();
        for (Method m : methods) {
            System.out.println("Metoda: " + m.getName());
            m.setAccessible(true); // Umożliwia dostęp do prywatnych
            m.invoke(obj); // Wywołanie metody
        }
        
        // Dostęp do prywatnego pola
        Field field = cls.getDeclaredField("secret");
        field.setAccessible(true);
        String value = (String) field.get(obj);
        System.out.println("Secret: " + value);
    }
}
```

---

## 2) Zalety i wady refleksji

| Zalety | Wady |
|--------|------|
| Ułatwienie procesu debugowania (dostęp do wszystkich danych) | Wysoki poziom skomplikowania kodu |
| Możliwość tworzenia dynamicznych programów (dane nie muszą być znane na etapie kompilacji) | Brak sprawdzenia poprawności na etapie kompilacji |
| Elastyczność — możliwość tworzenia frameworków, ORM, serializacji | Obniżona wydajność programów |
| Uniwersalność — działa z dowolnymi klasami | Możliwe naruszenie zasad bezpieczeństwa (dostęp do prywatnych pól/metod) |
| Umożliwia tworzenie narzędzi typu DI containers, JPA/Hibernate | Błędy występują w czasie wykonania, nie kompilacji |

**Uwaga:** Refleksja jest wolniejsza niż zwykłe wywołania, ponieważ sprawdzanie odbywa się w runtime.

---

## 3) Ziarenka JavaBeans

### Co to jest JavaBean?
**JavaBean** to mechanizm komponentów wielokrotnego użytku dla platformy Java. Ziarenko to fragment kodu, który napisany raz, może być używany wielokrotnie.

**Przykłady:** Komponenty GUI w pakiecie SWING (przyciski, pola tekstowe itp.), klasy danych w frameworkach.

### Struktura ziarenka JavaBean

Każde ziarno składa się z:
- **Właściwości** (properties):
  - **Proste** — odwołanie poprzez metody get/set
  - **Złożone** — mają wpływ na inne ziarna
- **Metod**
- **Zdarzeń** (events)

### Konwencja nazewnictwa (wzorce)

Aby ziarenko było rozpoznawane przez mechanizmy introspekcji, musi stosować konwencję:

| Typ właściwości | Metoda pobierania | Metoda ustawiania |
|-----------------|-------------------|-------------------|
| Normalna | `getWlasciwosc()` | `setWlasciwosc(T wartosc)` |
| Boolean | `isWlasciwosc()` lub `getWlasciwosc()` | `setWlasciwosc(boolean wartosc)` |

**Przykład ziarenka:**
```java
public class PersonBean {
    private String name;
    private int age;
    private boolean active;
    
    // Właściwość "name"
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    // Właściwość "age"
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        this.age = age;
    }
    
    // Właściwość boolean "active"
    public boolean isActive() {
        return active;
    }
    
    public void setActive(boolean active) {
        this.active = active;
    }
}
```

**Kluczowe zasady:**
- Metody muszą być `public`
- Konstruktor bezparametrowy (domyślny lub jawny)
- Metody get/set muszą być zgodne z konwencją nazewnictwa

### Tworzenie ziarenka

1. **Określić przeznaczenie i metody**
2. **Określić właściwości**
3. **Wybrać metodę dostępu:**
   - **Bezpośredni dostęp** — zastosowanie konwencji get/set
   - **Klasa informacyjna BeanInfo** — pozwala tworzyć deskryptory metod/właściwości/zdarzeń

---

## 4) Introspekcja (Introspection)

### Definicja
**Introspekcja** to analiza ziarenek JavaBean za pomocą metod refleksji, przy założeniu standardowych wzorców nazewnictwa.

**Cel:** Określenie właściwości i ich typu, metod pobierania/ustawiania, rodzajów zdarzeń, metod.

**Ważne:** Introspekcja opiera się na refleksji, ale jej celem jest **jedynie analiza** struktury (bez modyfikacji).

### Pakiet
```java
java.beans.Introspector
```

### Proces analizy ziarenka

Przy użyciu klasy `Introspector` proces przebiega następująco:

1. **Sprawdzenie klasy informacyjnej ziarenka (BeanInfo)** — jeśli istnieje
2. **Przeszukanie lokalizacji domyślnej** (`sun.beans.infos`)
3. **Analiza niskopoziomowa ziarna** (przy użyciu refleksji) — użycie domyślnej konwencji nazewnictwa get/set/is

### Przykład użycia introspekcji

```java
import java.beans.*;

public class IntrospectionDemo {
    public static void main(String[] args) throws IntrospectionException {
        MyBean bean = new MyBean();
        
        // Pobranie informacji o ziarenku
        BeanInfo info = Introspector.getBeanInfo(bean.getClass());
        
        // Pobranie deskryptorów właściwości
        PropertyDescriptor[] properties = info.getPropertyDescriptors();
        for (PropertyDescriptor pd : properties) {
            System.out.println("Właściwość: " + pd.getName());
            System.out.println("  Typ: " + pd.getPropertyType());
            System.out.println("  Getter: " + pd.getReadMethod());
            System.out.println("  Setter: " + pd.getWriteMethod());
        }
        
        // Pobranie deskryptorów zdarzeń
        EventSetDescriptor[] events = info.getEventSetDescriptors();
        for (EventSetDescriptor esd : events) {
            System.out.println("Zdarzenie: " + esd.getName());
        }
        
        // Pobranie deskryptorów metod
        MethodDescriptor[] methods = info.getMethodDescriptors();
        for (MethodDescriptor md : methods) {
            System.out.println("Metoda: " + md.getName());
        }
    }
}
```

### Deskryptory

**PropertyDescriptor** — opisuje właściwość:
- Nazwa właściwości
- Typ właściwości
- Metoda getter
- Metoda setter

**MethodDescriptor** — opisuje metodę:
- Nazwa metody
- Parametry metody

**EventSetDescriptor** — opisuje zdarzenie:
- Nazwa zdarzenia
- Metody dodawania/usuwania listenerów

---

## 5) Zalety i wady introspekcji

| Zalety | Wady |
|--------|------|
| Dostęp do informacji na temat ziarenka | Zawiły sposób implementacji |
| Wykorzystywana w graficznych środowiskach tworzenia GUI (np. NetBeans, Eclipse) | Brak sprawdzenia poprawności na etapie kompilacji |
| Umożliwia dynamiczne tworzenie formularzy i edytorów | Obniżona wydajność programów (używa refleksji) |
| Prostsza w użyciu niż bezpośrednia refleksja (wysoki poziom abstrakcji) | Ograniczona do ziarenek JavaBean |

**Uwaga:** Introspekcja jest wolniejsza niż bezpośrednie wywołania, bo używa refleksji pod spodem.

---

## 6) Refleksja vs Introspekcja — porównanie

| Cecha | Refleksja | Introspekcja |
|-------|-----------|--------------|
| **Pakiet** | `java.lang.reflect` | `java.beans.Introspector` |
| **Zakres** | Analiza dowolnej klasy | Analiza ziarenek JavaBean |
| **Modyfikacja** | Tak — może modyfikować strukturę i zachowanie | Nie — tylko analiza struktury |
| **Użycie** | Uniwersalne narzędzie | Specjalizowane dla JavaBeans |
| **Relacja** | Rozbudowane narzędzie oferujące wiele możliwości | Można traktować jako podzbiór refleksji |
| **Dynamiczne zarządzanie** | Tak — może tworzyć obiekty, wywoływać metody, zmieniać pola | Nie — tylko odczyt informacji |
| **Cel** | Analiza i modyfikacja struktur oraz zachowań obiektów | Analiza struktur obiektów (właściwości, zdarzenia, metody) |
| **Wymagania** | Dowolna klasa | Klasa musi być ziarenkiem JavaBean (konwencja get/set) |
| **Poziom abstrakcji** | Niski — bezpośredni dostęp do pól/metod | Wysoki — używa konwencji i deskryptorów |

### Wizualne porównanie

```
┌─────────────────────────────────┐
│         REFLEKSJA               │
│  (java.lang.reflect)            │
│                                 │
│  ┌───────────────────────────┐  │
│  │   INTROSPEKCJA            │  │
│  │   (java.beans)            │  │
│  │                           │  │
│  │   (używa refleksji        │  │
│  │    pod spodem)            │  │
│  └───────────────────────────┘  │
│                                 │
│  • Wszystkie klasy              │
│  • Analiza + modyfikacja        │
│  • Field, Method, Constructor   │
└─────────────────────────────────┘
```

**Kluczowa różnica:**
- **Refleksja** = można **czytać i pisać** (analiza + modyfikacja)
- **Introspekcja** = można tylko **czytać** (analiza struktury ziarenka)

### Kiedy używać czego?

**Używaj refleksji, gdy:**
- Chcesz analizować lub modyfikować dowolne klasy (nie tylko JavaBeans)
- Potrzebujesz dynamicznego tworzenia obiektów, wywoływania metod
- Budujesz frameworki (ORM, DI containers, serializacja)
- Potrzebujesz dostępu do prywatnych pól/metod

**Używaj introspekcji, gdy:**
- Pracujesz z ziarenkami JavaBean
- Potrzebujesz tylko **analizy** właściwości/zdarzeń/metod
- Budujesz narzędzia GUI (np. edytory właściwości)
- Chcesz wyższy poziom abstrakcji (nie musisz ręcznie przeszukiwać metod get/set)

---

## Przykłady praktyczne

### Przykład 1: Refleksja — dynamiczne wywołanie metody

```java
import java.lang.reflect.Method;

class Calculator {
    public int multiply(int a, int b) {
        return a * b;
    }
}

public class ReflectionExample {
    public static void main(String[] args) throws Exception {
        Calculator calc = new Calculator();
        
        // Pobranie metody przez refleksję
        Method multiplyMethod = Calculator.class.getMethod("multiply", int.class, int.class);
        
        // Dynamiczne wywołanie
        Object result = multiplyMethod.invoke(calc, 5, 7);
        System.out.println("Wynik: " + result); // Wynik: 35
    }
}
```

### Przykład 2: Introspekcja — analiza właściwości ziarenka

```java
import java.beans.*;

class UserBean {
    private String username;
    private String email;
    
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }
    
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}

public class IntrospectionExample {
    public static void main(String[] args) throws IntrospectionException {
        BeanInfo info = Introspector.getBeanInfo(UserBean.class);
        
        PropertyDescriptor[] properties = info.getPropertyDescriptors();
        
        System.out.println("Właściwości ziarenka:");
        for (PropertyDescriptor pd : properties) {
            // Pomijamy właściwość "class" (dziedziczona z Object)
            if (!pd.getName().equals("class")) {
                System.out.println("- " + pd.getName() + " (" + pd.getPropertyType() + ")");
            }
        }
    }
}
```

**Wynik:**
```
Właściwości ziarenka:
- email (class java.lang.String)
- username (class java.lang.String)
```

---

## Kluczowe zdania na obronę

1. **Refleksja** to mechanizm umożliwiający programowi analizę i modyfikację własnej struktury w czasie wykonania (pakiet `java.lang.reflect`).

2. **Introspekcja** to specjalizowane narzędzie do analizy ziarenek JavaBean przy użyciu refleksji i standardowych wzorców nazewnictwa (pakiet `java.beans.Introspector`).

3. **Obiekt Class** to klucz do refleksji — można go uzyskać przez `getClass()`, `Class.forName()` lub `.class`.

4. **Refleksja oferuje:** `Field` (pola), `Method` (metody), `Constructor` (konstruktory) — umożliwia dostęp nawet do prywatnych elementów.

5. **JavaBean** to komponent wielokrotnego użytku zgodny z konwencją nazewnictwa: `getWlasciwosc()`, `setWlasciwosc()`, `isWlasciwosc()` dla boolean.

6. **Główna różnica:** Refleksja może **modyfikować** strukturę i zachowanie, introspekcja tylko **analizuje** strukturę ziarenka.

7. **Refleksja działa z dowolnymi klasami**, introspekcja wymaga ziarenek JavaBean (konwencja get/set).

8. **Introspekcja używa refleksji pod spodem**, ale oferuje wyższy poziom abstrakcji poprzez deskryptory (`PropertyDescriptor`, `MethodDescriptor`, `EventSetDescriptor`).

9. **Zalety refleksji:** elastyczność, dynamiczne programy, możliwość budowania frameworków. **Wady:** niższa wydajność, brak sprawdzania w kompilacji, możliwe naruszenia bezpieczeństwa.

10. **Zastosowania:** Refleksja — ORM (Hibernate), DI containers (Spring), serializacja. Introspekcja — edytory GUI, narzędzia do tworzenia formularzy dynamicznie.
