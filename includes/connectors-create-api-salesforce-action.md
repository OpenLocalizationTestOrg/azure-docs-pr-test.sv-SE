Nu när du har lagt till ett villkor, dess tid toodo något intressant med hello data som genereras av hello-utlösare. Följ dessa steg tooadd hello **Salesforce - Get-objektet** åtgärd. Den här åtgärden hämta hello data varje gång en ny lead skapas. Du kommer också lägga till en andra åtgärd som ska använda hello data från hello Salesforce - Get ett objekt åtgärden toosend ett e-post med hello Office 365-kopplingen.  

tooconfigure hello den här åtgärden måste du tooprovide hello följande information. Du kommer att märka att det är enkelt toouse data som genereras av hello utlösare som indata för några av hello egenskaper för ny hello-fil:

| Skapa filen egenskap | Beskrivning |
| --- | --- |
| Objekttyp |Detta är hello Salesforce-objekt som du är intresserad av. Exempel är Lead, konto, osv. |
| Objekt-ID |Detta representerar en identifierare för hello-objektet. |

1. Välj **lägga till en åtgärd** länk. Den här öppnas hello-sökrutan där du kan söka efter något du vill tootake. I det här exemplet är Salesforce åtgärder av intresse.      
   ![Bild av åtgärder för Salesforce 1](./media/connectors-create-api-salesforce/action-1.png)  
2. Ange *salesforce* toosearch för åtgärder relaterade toosalesforce.
3. Välj **Salesforce - Get-objektet** som hello åtgärden tootake.   **Obs**: du kommer att tillfrågas tooauthorize din logik app tooaccess ditt Salesforce-konto om du inte har gjort det tidigare.    
   ![Bild av åtgärder för Salesforce 2](./media/connectors-create-api-salesforce/action-2.png)    
4. Hej **Get objektet** styra öppnas.  
5. Välj *leda* som hello objekttyp.
6. Välj hello **objekt-ID** kontroll.
7. Välj **...**  tooexpand hello lista över token som kan användas som indata för åtgärder.       
   ![Bild av åtgärder för Salesforce 3](./media/connectors-create-api-salesforce/action-3.png)    
8. Välj **leda ID** styra öppnas.   
   ![Bild 4 till Salesforce åtgärd](./media/connectors-create-api-salesforce/action-4.png)     
9. Observera att hello leda ID token är nu i hello kontroll för objekt-ID, som anger att hello Get händelse för objektet söker efter ett lead med ett ID som är lika toohello lead-ID för lead som utlöses den här logikapp.  
   ![Bild 5 till Salesforce åtgärd](./media/connectors-create-api-salesforce/action-5.png)  
10. Spara ditt arbete. Det kan du har lagt till hello Get objektet åtgärd tooyour logikapp. Get-objektet kontrollen ska se ut så här:    
    ![Bild av åtgärder för Salesforce 6](./media/connectors-create-api-salesforce/action-6.png)  

Nu när du har lagt till en åtgärd tooget ett lead kan kanske du vill toodo något intressant med hello nyskapad lead. Du kanske vill toosend en e-toonotify en distributionslista som en ny lead har skapats i ett företag. Vi använder hello Office 365 connector toosend ett e-postmeddelande med en del av hello relevant information från hello nytt lead objekt i Salesforce.  

1. Välj **lägga till en åtgärd** ange *e-post* i hello sökkontrollen. Detta filtrerar hello åtgärder toothose som är relaterade toosending och e-postmeddelanden.  
2. Välj hello **Office 365 Outlook - skicka ett e-post** listobjekt. Om du inte redan har skapat en *anslutning* tooyour Office 365-konto som du kommer att ange tooenter din Office 365 autentiseringsuppgifter toocreate it nu. När du är klar hello **skickar ett e-** styra öppnas.        
   ![Bild 7 till Salesforce åtgärd](./media/connectors-create-api-salesforce/action-7.png)  
3. Ange hello e-postadress som du vill att toosend e-tooin hello **till** kontroll.
4. I hello **ämne** styra, ange *nya leda skapade* – Välj hello *företagets* token. Detta visar hello *företagets* från hello ny lead som skapats i Salesforce.  
5. I hello **brödtext** kontroll, kan du markera någon av hello-token från hello nytt lead objekt och du kan också ange texten som du vill att toodisplay i hello brödtext hello e-post. Här är ett exempel:  
   ![Bild av åtgärder för Salesforce 8](./media/connectors-create-api-salesforce/action-8.png)   
6. Spara ditt arbetsflöde.  

Det var allt. Din logikapp är slutförd.  

Nu kan du testa logikappen: skapa en ny lead som uppfyller hello-villkor som du skapade i Salesforce,.  Om du har följt den här genomgången fullständigt bara skapa ett lead med en e-postadress som innehåller *amazon.com* i den. Efter några sekunder logikappen ska utlösas och hello resultat kan se ut ungefär toothis:  
![Bild av åtgärder för Salesforce 9](./media/connectors-create-api-salesforce/action-9.png)  

