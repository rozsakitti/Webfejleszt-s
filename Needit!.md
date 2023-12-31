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

![image](https://github.com/rozsakitti/Webfejleszt-s/assets/90957539/744fb534-78c4-4d5a-acde-f905f99cd6cc)

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


### Autentikáció = > felhasználók hitelesítése:

Alapból három eset van a Spring Security-ben a felhasználók hitelesítésénél:

1. Hozzáférsz a felhasználó – hash-elt – jelszavához, mert pl. az adatait egy adatbázisban tárolod – ez az alap
   
2. Kevésbé gyakori: Nem tudsz hozzáférni a felhasználó – hash-elt – jelszavához. Ez akkor áll fenn, ha a felhasználók adatait valahol máshol tároljuk, például egy harmadik féltől származó identity management product-ban, ami a felhasználók hitelesítéséhez REST-szolgáltatásokat nyújt. Ilyen például az Atlassian Crowd.
   
3. Szintén népszerű: OAuth2-t, vagy OpenID-t (Google-lel/Twitter-rel, stb.-vel történő belépés) akarunk használni, általában JWT-vel – JSON Web Tokens - együtt.

### AAA protokoll, autentikációs mechanizmusok => authentication,  authorization, accounting:

- **authentication**: felhasználó azonosítása

- **authorization**: azonosítást követően ellenőrzi, hogy a felhasználó jogosult-e a kért erőforrásokhoz.

- **accounting**: felhasználói tevékenységek rögzítésével és nyomon követésével foglalkozik. Nyomon követi, hogy a felhasználó milyen erőforrásokhoz fér hozzá, mikor és hogyan használja azokat.

### Webszolgáltatások architektúrája: 

**- Webszolgáltatások:** alkalmazások közötti adatcserére szolgáló protokollok és szabványok gyűjteménye. Különböző programnyelveken írt és különböző platformokon futó szoftveralkalmazások számítógép-hálózatokon (mint az internet) keresztül történő adatcserére használják.

**- SOAP (Simple Object Access Protocol):**
A SOAP egy protokoll, amelyet XML formátumban használnak a különböző rendszerek közötti kommunikációhoz.
A SOAP webszolgáltatások általában XML-en alapuló üzeneteket használnak, és támogatják a kliens-szerver kommunikációt.

**- REST (Representational State Transfer):**
A REST egy könnyű, állapottól mentes (stateless) architektúrális stílus, amelyet webszolgáltatások kialakításához használnak.
A RESTful webszolgáltatásoknak nincs szükségük állapotfenntartásra, minden kérés a kliens által tartalmazott információkból áll.
Az erőforrásokat URL-eken keresztül azonosítják, és különböző HTTP módszereket használnak a műveletek végrehajtásához (GET, POST, PUT, DELETE stb.).

**-WSDL (Web Services Description Language):**
A WSDL egy XML alapú nyelv, amely a webszolgáltatások interfészét írja le.
Részletesen meghatározza, hogy egy webszolgáltatás milyen funkciókat nyújt, és milyen formátumban kell az üzeneteket küldeni és fogadni.

**A webszolgáltatások architektúrája a következő kulcsfontosságú elveken alapul:**

- Interoperabilitás: A webszolgáltatásoknak különböző platformokon és programozási nyelveken kell működniük, és képeseknek kell lenniük az adatok megosztására.

- Kényszerítés (Loose Coupling): A kliens és a szerver közötti kapcsolatnak lazának kell lennie, vagyis a változtatásoknak az egyik oldalon ne kelljen befolyásolniuk a másik oldalt.

- Állapottól mentesség (Statelessness): A webszolgáltatásoknak nem kell tárolniuk a kliensek állapotát a kérések között.

- Egységes interfész: A webszolgáltatásoknak egységes interfészt kell használniuk, például HTTP protokollt, REST vagy SOAP formátumot.

- Biztonság: A webszolgáltatásoknak biztonságosnak kell lenniük az adatok védelme érdekében. Ez magában foglalhatja az SSL használatát, az azonosítást és az engedélyezést.


### JWT => JSON Web Token: 
A JWT-ket általában az azonosítási mechanizmusokban használják. Amikor egy felhasználó bejelentkezik, a szerver előállíthat egy olyan JWT-t, amely tartalmazza a felhasználóval kapcsolatos információkat, majd elküldi azt a kliensnek. A kliens ezt a tokent később beleillesztheti a kéréseinek fejlécébe azonosítás céljából. A szerver pedig ellenőrzi a tokent, hogy megbizonyosodjon arról, hogy valódi, és nem lett megváltoztatva.

- fejléc:
alkalmazott aláírási algoritmus + token típusa

      {
        "alg": "HS256",
        "typ": "JWT"
      }

- adattartalom (Payload): 
A token második része az adattartalom, amely tartalmazza az állításokat. Az állítások olyan kijelentések, amelyeket egy entitásról szólnak (általában a felhasználóról) és további adatokat. Gyakori állítások a iss (kibocsátó), exp (lejárati idő), sub (tárgy), aud (közönség) stb.

      {
        "sub": "1234567890",
        "name": "John Doe",
        "iat": 1516239022
      }

- Aláírás (Signature):
Az aláírás rész létrehozásához szükséges az előzőleg kódolt fejléc, az előzőleg kódolt adattartalom, egy titok, a fejléc által meghatározott algoritmus és az aláírás.

      HMACSHA256(
        base64UrlEncode(header) + "." +
        base64UrlEncode(payload),
        secret)

**A végső JWT így néz ki:**

xxxxx.yyyyy.zzzzz

**xxxxx** a base64Url-kódolt fejléc.

**yyyyy** a base64Url-kódolt adattartalom.

**zzzzz** a base64Url-kódolt aláírás.

### Angular szintaxis: 

- String interpoláció:

  <p>Welcome, {{ username }}!</p>

- Eseménykötés:

  <button (click)="onClick()">Click me</button>

- Tulajdonságkötés:

  <input [value]="myProperty">


### Milyen http kérések léteznek? => get, head, post, put, delete, connect, options, trace, patch

**GET**: Az erőforrás lekérése. A GET kérés nem módosítja az erőforrást, csak lekéri az azt reprezentáló adatokat.

        GET /index.html HTTP/1.1
        Host: www.example.com

**HEAD**: A HEAD kérés hasonló a GET kéréshez, de csak a fejléceket kéri le, anélkül, hogy a tényleges tartalmat elküldené.

        HEAD /info HTTP/1.1
        Host: www.example.com

**POST** => create: Arra szolgál, hogy új erőforrást hozzon létre a szerveren, például új felhasználót regisztrálni. A POST nem idempotens, azaz ugyanazt a kérést többször elküldve más és más eredményt hozhat létre.

        POST /submit-form HTTP/1.1
        Host: www.example.com
        Content-Type: application/x-www-form-urlencoded
        
        username=johndoe&password=secretpass

**PUT**: => update: Az adatok küldése a szervernek a megadott erőforrás módosítása céljából. A szerver a kért erőforrást módosítja, vagy ha nem létezik, létrehozza azt. A PUT idempotens, azaz ugyanazt a kérést többször elküldve ugyanazt az eredményt hozza létre.
  
        PUT /update-resource HTTP/1.1
        Host: www.example.com
        Content-Type: application/json
        
        {
          "id": 123,
          "data": "updated content"
        }

----

        PUT /users/1
        {
          "name": "John Doe",
          "age": 30,
          "email": "john.doe@example.com"
        }

**DELETE**: Az erőforrás törlése a szerveren.

        DELETE /delete-resource/123 HTTP/1.1
        Host: www.example.com

**CONNECT**: A célja egy TCP-tunnel kiépítése a kliens és a szerver között.

        CONNECT www.example.com:443 HTTP/1.1
        Host: www.example.com:443

**OPTIONS**: A szerverrel való kommunikáció paramétereinek lekérése. Például, hogy milyen HTTP módszereket támogat a szerver.

        OPTIONS /supported-methods HTTP/1.1
        Host: www.example.com

**TRACE**: A kliensnek lehetősége van kérni a szervertől, hogy küldje vissza az eredeti kérési fejléceket. Általában a hibakeresésre használják. 
A TRACE módszer arra szolgál, hogy az ügyfél képes legyen kinyomozni, hogy a kérés milyen formában érkezik meg a szerverhez, és hogy a szerver hogyan dolgozza fel azt. A szerver minden egyes proxy-n, gateway-en, vagy más eszközön, amelyen áthalad, hozzáad egy Via fejlécet a válaszához.

        TRACE /path/to/resource HTTP/1.1
        Host: www.example.com

  szerver válasza:
  
        HTTP/1.1 200 OK
        Date: Thu, 11 Nov 2023 12:00:00 GMT
        Content-Type: message/http
        
        TRACE /path/to/resource HTTP/1.1
        Host: www.example.com
        Via: 1.1 proxy.example.com (Proxy/1.0)
        Via: 1.1 gateway.example.com (Gateway/2.0)


**PATCH**:A PATCH kérés célja egy meglévő erőforrás részleges módosítása. Ez hasznos lehet, ha csak néhány adatot kell frissíteni, és nem akarjuk teljesen lecserélni az erőforrást.


        PATCH /users/1
        {
          "age": 31
        }
