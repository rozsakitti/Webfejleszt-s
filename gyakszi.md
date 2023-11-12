**Mi a különbség az Inversion of Control (IoC) és a Dependency Injection (DI) között?**

Az Inversion of Control (IoC) egy szoftvertervezési elv, amely a vezérlési folyamatot fordítja meg a hagyományos módon.=>
a komponensek kezelését a keretrendszerre bízzuk.

A Dependency Injection (DI) a IoC egy konkrét megvalósítása, amikor egy objektum függőségeit (pl.: más objektumokat vagy szolgáltatásokat) 
külső forrás injektálja, és nem a függőséget igénylő objektum hozza létre magának. Fontos a lazán csatolt kód és a könnyű tesztelhetőség szempontjából.

**Mi az a DI konténer, és mi a feladata a Dependency Injection (DI) szempontjából?**

A DI konténer olyan programkönyvtár vagy eszköz, amely biztosítja a függőség-befecskendezés (DI) funkcióit. A konténer példányosítja, konfigurálja, és injektálja a bean-eket a szükséges helyeken a kódban.

**Miért előnyös a lazán csatolt kód (loose coupling) a szoftvertervezésben, és hogyan segíti ezt elő a DI?**

A lazán csatolt kód rugalmasabb, könnyebben karbantartható és könnyebben kiterjeszthető. A DI segít a lazán csatolt kód elérésében, mivel a függőségeket külső forrásból kapjuk meg, és nem a kódunk hozza létre azokat.

**Mi a Bean fogalma a Spring keretrendszerben?**

A Spring-ben minden Java objektumot bean-nek hívunk, amit az alkalmazás elindításakor a Spring keretrendszer hoz létre.

- #### A Spring-ben a függőség-befecskendezés történhet konstruktorokon, setter-eken vagy mezőkön keresztül is.

**1.) Konstruktor alapú DI:** A konténer meg fogja hívni azt a konstruktort, amely argumentumokkal rendelkezik, megfelelve a megadott függőségeknek. 

Példa:

        public class Example {
            private Dependency dependency;
        
            // Konstruktor alapú DI
            public Example(Dependency dependency) {
                this.dependency = dependency;
            }
        }

**2.) Setter alapú DI:** Setter alapú DI esetén a konténer először a bean példányosításához meghív egy argumentum nélküli konstruktort, 
majd ezután meghívja az osztályunk setter metódusait a függőség befecskendezéséhez. 

Példa:

        public class Example {
            private Dependency dependency;
        
            // Setter alapú DI
            public void setDependency(Dependency dependency) {
                this.dependency = dependency;
            }
        }

**3.) Mező alapú DI:** Mező alapú DI-nél a függőségeket az @Autowired annotációval ellátott mezőkön keresztül lehet befecskendezni. 

Példa:

        public class Example {
            @Autowired
            private Dependency dependency;
        }

**Ez azonban nem az ajánlott módja a DI-nek, hiszen:**

a) Reflexiókon keresztül történik a függőség befecskendezése, ami költségesebb, mint egy konstruktor vagy setter használata,

b) Final mezőkkel nem lehet használni,

c) Ezzel a módszerrel könnyűnek tűnhet több függőséget is megadni, de így egy osztálynak több felelőssége is lehet – több függőség, több felelősség - , ami megsérti az egyszeres felelősség elvét.

**Bean-ek:** Ahogy arról már szó esett fentebb, a Spring-ben azokat az objektumokat nevezzük bean-eknek, amik az alkalmazásunk gerincét alkotják, és a Spring IoC konténer kezeli őket.

### Annotációk:

- **@Bean:** Ahhoz, hogy egy bean-t deklaráljunk, elég egy metódust ellátnunk a @Bean annotációval. Ezzel szétválasztható a bean létrehozása az osztálydeklarációtól. Ez az annotáció azt jelzi, hogy az ezzel megjelölt metódus egy olyan objektumot ad vissza, amit bean-ként kell regisztrálni a Spring application context-ben.

- **@Configuration:** Az ezzel az annotációval ellátott osztályokat a Spring IoC konténer bean-definíciók forrásaként használhatja.

- **@ComponentScan:** A Spring képes egy csomagot automatikusan átkutatni, és megkeresni benne a bean-eket. A @ComponentScan annotációt a @Configuration annotációval együtt szokták használni, és megmondja, hogy melyik package-ben keressen a Spring bean-eket.

- **@Component:** Ez egy osztályszintű annotáció. A komponensszkennelés alatt a Spring keretrendszer automatikusan észreveszi a @Component annotációval ellátott osztályokat,

- **@Repository:** A DAO vagy Repository osztályok általában az adatbázis hozzáférési réteget képviselik egy alkalmazásban, és ezeket el kell látni a @Repository annotációval:






