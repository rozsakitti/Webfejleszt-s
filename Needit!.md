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

        
### Scope-ok:

### MVC architektúra + rajz: 

### Mi az @autowired?

### Mi az @inject?

### Autentikáció: 

### Webszolgáltatások architektúrája: 

### Rest fogalma: 

### JWT: 

### Angular szintaxis: 
