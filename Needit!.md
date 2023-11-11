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

### Mi az @autowired?

### Mi az @inject?

### Autentikáció: 

### Webszolgáltatások architektúrája: 

### Rest fogalma: 

### JWT: 

### Angular szintaxis: 
