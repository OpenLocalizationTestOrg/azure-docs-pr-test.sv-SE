
Som standard anropas API: er i en Mobile Apps-serverdel anonymt. Sedan måste toorestrict tooonly autentiseras klienterna.  

* **Node.js tillbaka avslutas (via hello Azure-portalen)** :  

    I inställningarna för Mobilappar klickar du på **enkelt tabeller** och välj din tabell. Klicka på **ändra behörigheter**väljer **autentiserad åtkomst bara** för alla behörigheter och klicka sedan på **spara**.
* **.NET serverdel (C#)**:  

    Hello serverprojekt navigera för**domänkontrollanter** > **TodoItemController.cs**. Lägg till hello `[Authorize]` attributet toohello **TodoItemController** class, enligt följande. toorestrict endast toospecific åtkomstmetoder, du kan också använda det här attributet bara toothose metoder i stället för hello-klassen. Publicera hello serverprojekt.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **Node.js-serverdel (via Node.js-kod)** :  

    toorequire autentisering för fältåtkomst Lägg till hello följande rad toohello Node.js server-skript:

        table.access = 'authenticated';

    Mer information finns i [så här: kräver autentisering för åtkomst tootables](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). toolearn hur toodownload hello snabbstartsprojekt koden från din webbplats, se [så här: hämta hello Node.js-serverdel kod snabbstartsprojekt med Git](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).
