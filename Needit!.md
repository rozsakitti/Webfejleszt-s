### Java Bean: 
A JavaBean egy olyan Java osztály, amely bizonyos tervezési konvenciókat követ, 
és ezáltal egyszerűen integrálható más Java alapú rendszerekbe. 
A JavaBeanek egyfajta komponensmodellt valósítanak meg, amely lehetővé teszi az osztályok 
**újrafelhasználhatóságát** és **könnyű integrálhatóságát**.

__Jellemzők:__
- Publikus, paraméter nélküli konstruktor:
  
  A JavaBeannek szüksége van egy publikus konstruktorra, amely paraméter nélküli.
  Ez lehetővé teszi a JavaBean példányosítását a Reflection API segítségével
  
       public class Example {
          private String message;

          public Example() {
            this.message = "Hello, World!";
        }


- Getter és Setter metódusok:

  A JavaBeaneknek getter és setter metódusokkal kell rendelkezniük a tulajdonságok eléréséhez és beállításához.
  Például, ha egy JavaBean egy "name" tulajdonságot tartalmaz, akkor a következő típusú metódusok szükségesek:

      public String getName() {
        return this.name;
      }

      public void setName(String name) {
        this.name = name;
      }

- Serializable interfész implementációja:
    
    Ha a JavaBean példányokat át szeretnénk adni a hálózaton vagy példányosítani szeretnénk őket
    a szerializáció révén, akkor implementálni kell a java.io.Serializable interfészt.

- Példa kód: 

      public AppleBean implements Serializable {
        private String colour;
        private String type; 
        private String state;
      
        /** paraméter nélküli konstruktor */
        public AppleBean() {
        }
      
        public String getColour(){
          return this.colour;
        }
        public String getType(){
          return this.type;
        }
        public String getState(){
          return this.state;
        }
      
        public void setColour(String colour){
          this.colour = colour;
        }
        public void setType(String type){
          this.type = type;
        }
        public void setState(String state){
          this.state = state;
        }
      }

##### Managed Bean:
  
A Managed Bean egy, a JSF-hez regisztrált Java-osztály (faces-config.xml-ben), ami a felhasználói felület és az üzleti logika közötti interakciót lehetővé teszi. A @ManagedBean annotációval szokták ezeket az osztályokat megjelölni.

A @ManagedBean annotációval jelezzük, hogy ez az osztály egy Managed Bean
lesz. A name=”helloWorld”-del pedig nevet is adhatunk a bean-nek. Általában
érdemes valamilyen találó névvel ellátni a managed bean-eket. 

Request scope-pal, session scope-pal vagy application scope-pal lehet őket definiálni. Automatikusan kerülnek létrehozásra, amikor szükség van rájuk. A Managed Bean-ek igazából Java Bean osztályok. A Java beaneknek nevez minden olyan osztályt, amely tulajdonságokat és eseményeket tesz elérhetővé külső környezet számára.

##### Backing Bean:

Egy JSF alkalmazásnak lehet egy vagy több backing bean-je is, amelyek egyfajta managed bean-ek, amik hozzá társíthatók az oldalon használt komponensekhez. Request scope-pal lehet őket definiálni. 

Az oldal komponenseihez tartozó objektumokat adattagokként tartalmazzák.

A JSF alkalmazásoknak van egy életciklusa, amin végigmennek:

A JSF esetében alapvetően kétfajta kérés létezik: az **initial** és a **postback**. Az initial
request (HTTP GET) hatására a szerver oldalon létrejön a kért oldalhoz tartozó komponensfa, melynek a csomópontjai az oldalon található elemek lesznek (UIForm, UIOutput, UIInput, stb...). 

Az initial kérések során csak a Restore View és az Render Response fázis fog lefutni, azaz felépül a komponensfa (+ eseménykezelők, validátorok, konverterek jóváhagyásra kerülnek, majd a view elmentésre kerül a FacesContext-be) ami alapján lerenderelődik a HTML oldal. Postback kérések esetén tipikusan egy form mezői kerülnek átküldésre a szerver oldalra (HTTP POST), ahol létezik már az oldalhoz tartozó komponensfa, egy korábbi initial kérés következtében.

Alapesetben a postback kérések elküldésekor mind a 6 JSF fázis lefut, ahol is a komponensfa aktualizálása mellett a megfelelő lépések után a managed bean-ekbe beíródnak az értékek.
 
- #### Restore View =>	Nézet visszaállítása :
  
•	A komponensfa visszaállítása vagy létrehozása.

•	A már korábban megjelenített oldal esetén a komponensek a legutóbbi állapotukba visszatöltődnek.
  
- #### Apply Request Values => Kérésben szereplő értékek érvényesítése: 

•	A komponensfán való iteráció, ahol a komponensek kiválaszthatják, melyik lekérési adatok tartoznak hozzájuk.

•	A kiválasztott adatok eltárolása.

- #### Process validations => Validációk:

•	Az érvényességi ellenőrzések, például konvertálások és validációk végrehajtása.

•	Hiba esetén átugrás a Render Response fázisba.

- #### Update model values => A modell értékeinek frissítése:

•	Az érvényesített adatok alapján a Managed Beanek (model) értékeinek frissítése.

- #### Invoke Application => Az alkalmazás meghívása: 

•	Az űrlap elküldését aktiváló gomb vagy link action attribútum által indukált metódusok végrehajtása.

•	Az esetlegesen generált eredmény String használata a navigáció meghatározásához.

- #### Render response => A válasz generálása:

•	A válasz generálása, kódolása, és elküldése a kliensnek.

### Scope-ok:

•	**@RequestScoped:**

o	A lekérdezés élettartama alatt él, tehát az adatok csak a lekérdezés ideje alatt maradnak érvényesek.

o	Minden lekérdezéskor új példány jön létre, és ez a példány a válasz elküldésekor törlődik.

**•	@SessionScoped:**

o	Az élettartama a kliens által kezdeményezett sorozatos csatlakozásokon alapul.

o	Az objektumok a session kezdetétől a session megszűnéséig élnek.

o	Egy klienshez egy adott osztályból csak egy példány tartozik.

**•	@ApplicationScoped:**

o	A legtágabb élettartamú scope, azaz a webalkalmazás teljes élettartama alatt él.

o	Az objektumok az alkalmazás első lekérdezésekor jönnek létre, és az alkalmazás eltávolításakor szűnnek meg.

o	Egy adott osztályból csak egy példány létezik a teljes alkalmazásban.

**•	@ViewScoped:**

o	Az élettartama egy adott nézet (view) életciklusához van kötve.

o	Az adatok érvényesek mindaddig, amíg a felhasználó az adott nézeten belül operál.

o	A példány létrejön, amikor egy JSF View egy GET-kéréssel kezdődik, és addig tart, amíg a felhasználó beküld egy POST form-ot az action method-höz, ami null-t vagy void-t ad vissza.



### MVC architektúra + rajz: 

A Model-View-Controller (MVC) architektúra egy olyan tervezési minta, amely elkülöníti az alkalmazás logikáját (model), a felhasználói felületet (view) és a felhasználói interakciókat (controller). Ezt a mintát használva könnyebbé válik az alkalmazások fejlesztése és karbantartása, mivel a különböző részeket egymástól függetlenül lehet fejleszteni és tesztelni.

- Model (Modell):

A modell felelős az alkalmazás üzleti logikájának és adatmanipulációjának kezeléséért.
Tartalmazza az adatokat és a hozzájuk tartozó műveleteket.
Nem függ a felhasználói felülettől vagy a vezérlőtől, így könnyen tesztelhető és újrahasznosítható.

- View (Nézet):

A nézet felelős az információ megjelenítéséért és a felhasználói felület kialakításáért.
A nézet nem tartalmazza az alkalmazás üzleti logikáját, csak az adatok megjelenítésére szolgáló eszközöket.
Értesítéseket fogad a modeltől, amikor az adatok változnak, és frissíti magát ennek megfelelően.

- Controller (Vezérlő):

A vezérlő feladata az felhasználói interakciók kezelése, azok feldolgozása és továbbítása a model és a nézet között.
Fogadja a bejövő kéréseket a felhasználótól, elvégzi a szükséges műveleteket, majd frissíti a modelt vagy a nézetet.
A vezérlő biztosítja a kapcsolatot a model és a nézet között, de maga nem tartalmazza a teljes üzleti logikát.


### Mi az @autowired?

Az @Autowired egy Spring keretrendszerben használt annotáció, amelyet a függőség befecskendezés (dependency injection) támogatására használnak. A függőség-befecskendezés egy olyan tervezési minta, amelynek célja, hogy a szükséges objektumokat (függőségeket) külsőleg injektálja egy komponensbe, ahelyett, hogy a komponensnek kellene létrehoznia vagy keresnie azokat.

      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Component;

      @Component
      public class MyComponent {
      
          private SomeService someService;
      
          @Autowired
          public MyComponent(SomeService someService) {
              this.someService = someService;
          }
      
      }

Ebben az példában a MyComponent osztály egy Spring komponens (bean), amelynek egy SomeService típusú függősége van. Az @Autowired annotáció segítségével a Spring konténer automatikusan injektálja a megfelelő SomeService példányt a MyComponent konstruktorán keresztül. Ez azt jelenti, hogy a fejlesztőnek nem kell manuálisan létrehoznia vagy keresnie a SomeService példányt; a Spring konténer gondoskodik róla, hogy az szükség esetén rendelkezésre álljon.

//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

### Mi az @inject?

Az @Inject annotáció egy Java Dependency Injection (DI) annotáció, amelyet a Java Platform, Enterprise Edition (Java EE) és a Java Platform, Micro Edition (Java ME) alkalmazásokban használnak. Ezzel az annotációval jelöljük meg azokat a helyeket a kódban, ahol függőségeket injektálunk egy objektumba vagy komponensbe.

Fontos megjegyezni, hogy az @Inject annotáció is használható Java SE (Standard Edition) alkalmazásokban is, ha az alkalmazás támogatja a Dependency Injection-t. A Spring keretrendszer például használható Java SE alkalmazásokban is, és az @Autowired annotáció helyettesíti az @Inject-et.

        import javax.inject.Inject;
        import javax.servlet.annotation.WebServlet;
        import javax.servlet.http.HttpServlet;
        import javax.servlet.http.HttpServletRequest;
        import javax.servlet.http.HttpServletResponse;
        
        @WebServlet("/myServlet")
        public class MyServlet extends HttpServlet {
        
            @Inject
            private MyService myService;
        
            @Override
            protected void doGet(HttpServletRequest request, HttpServletResponse response) {
                // Használjuk a myService-t...
            }
        }

Ebben a példában a MyServlet osztály injektálja a MyService típusú függőségét az @Inject annotáció segítségével. A Java EE konténer gondoskodik arról, hogy a megfelelő MyService példány rendelkezésre álljon a servlet számára.

Az @Inject annotációval történő függőség-befecskendezés lehetőséget nyújt a gyenge kapcsolatú és könnyen tesztelhető komponensek létrehozására, mivel a függőségeket az objektum kívülről kapja meg, és nem maga hozza létre őket.

<center> ![image](https://github.com/rozsakitti/Webfejleszt-s/assets/90957539/c36b5325-d17e-48e0-b39e-b54c13b5150c) </center>

- ### Különbségek: @autowired és @injection

#### 1.) Keretrendszer támogatás:

**@Autowired:** A Spring keretrendszer része, és főként a Spring Dependency Injection rendszerrel használják.

**@Inject:** A Java Contexts and Dependency Injection (CDI) specifikáció része, és elsősorban Java EE (Enterprise Edition) alkalmazásokban használják, de használható Java SE (Standard Edition) alkalmazásokban is, ha a megfelelő CDI konténer rendelkezésre áll.

#### 2.) Importálás:

**@Autowired:** Az org.springframework.beans.factory.annotation.Autowired csomagból származik.

**@Inject:** Az javax.inject.Inject csomagból származik.

#### 3.) Beállítási lehetőségek:

**@Autowired:** Többféle módon is konfigurálható, például az attribútumok (fields), konstruktorok és setter metódusok segítségével. Támogat továbbá opcionális és kötelező függőségek kezelését is.

**@Inject:** Általában a konstruktorok és setter metódusok használatával konfigurálható, és a CDI specifikációban kevesebb beállítási lehetőséget definiál, mint a Spring.

#### 4.) Támogatott típusok:

Mindkét annotáció általában támogatja a konstruktorokat, setter metódusokat és mezőket (fields) a függőségek injektálására.
Kiterjesztési lehetőségek:

**@Autowired:** A Spring keretrendszer számos egyéb annotációt és lehetőséget biztosít az @Autowired kiterjesztésére és finomhangolására.
**@Inject:** A CDI specifikáció kevésbé rugalmas és kevesebb kiterjesztési lehetőséget biztosít.

#### 5.) Kompatibilitás:

**@Autowired:** Jellemzően csak Spring alkalmazásokban használják.

**@Inject:** Javasolt a Java EE alkalmazásokban, de használható Java SE alkalmazásokban is, ha a CDI konténer elérhető.
Végső soron mindkét annotáció fő célja a függőség-befecskendezés elősegítése, de a választás a projekt követelményeitől, a keretrendszer preferenciáitól és az alkalmazás kontextusától is függ.




### Autentikáció: 

### Webszolgáltatások architektúrája: 

### Rest fogalma: 

### JWT: 

### Angular szintaxis: 
