Nu när du har lagt till en utlösare, dess tid toodo något intressant med hello data som genereras av hello-utlösare. Följ dessa steg tooadd hello **SharePoint Online - skapa fil** åtgärd. Den här åtgärden skapar en fil i SharePoint Online varje gång hello nya objekt utlösare utlöses. 

tooconfigure hello den här åtgärden måste du tooprovide hello följande information. När du anger den här informationen ser du att det är enkelt toouse data som genereras av hello utlösare som indata för några av hello egenskaper för ny hello-fil:

| Skapa filen egenskap | Beskrivning |
| --- | --- |
| Webbadress |Detta är hello URL för hello SharePoint Online-webbplats där du vill att toocreate hello ny fil. Välj hello plats hello listan visas. |
| Mappsökväg |Detta är hello mappen (på hello Webbadress som valts i hello föregående steg) där nya hello-filen ska placeras. Bläddra efter och välj hello mapp. |
| Filnamn |Det här är hello hello-filen som skapas. |
| Filinnehåll |hello-innehåll som ska skrivas toohello fil. |

1. Välj **+ nytt steg** tooadd hello åtgärd.  
   ![SharePoint online åtgärd bild 1](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Välj hello **lägga till en åtgärd** länk. Den här öppnas hello-sökrutan där du kan söka efter något du vill tootake. I det här exemplet är SharePoint åtgärder av intresse.    
   ![Bild 2 till SharePoint online åtgärd](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Ange *sharepoint* toosearch för åtgärder relaterade tooSharePoint.
4. Välj **SharePoint Online - skapa filen** som hello åtgärden tootake.   **Obs**: du kommer att tillfrågas tooauthorize din logik app tooaccess din SharePoint-konto om du inte har skapat en anslutning tooSharePoint Online tidigare.    
   ![Bild 3 till SharePoint online åtgärd](./media/connectors-create-api-sharepointonline/action-3.png)    
5. Hej **skapa fil** styra öppnas.   
   ![Bild 4 till SharePoint online åtgärd](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Välj **Webbadress** och bläddra toofind hello plats där du vill ha toocreate hello-filen.     
   ![Bild 5 till SharePoint online åtgärd](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Välj **mappsökväg** och bläddra toofind hello mappen där nya hello-filen ska placeras.  
   ![Bild 6 till SharePoint online åtgärd](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Välj hello **filnamn** styra och ange hello namn hello-fil som du vill toocreate. Här kan du antingen ange hello filnamnet direkt eller använda hello egenskaper från hello utlösare som du skapade tidigare. Det gör du genom att välja Egenskaper hello listan över **utdata från när ett nytt objekt skapas**. Den här listan visas endast när du har valt hello **filnamn** kontroll. I den här walkthough jag valt ID (hello nytt listobjekt hello ID) som hello namnet på hello-filen som skapas av hello **SharePoint Online - skapa fil** åtgärd.    
   ![Bild 7 till SharePoint online åtgärd](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Välj hello **filen innehåll** styr och ange hello-innehåll som ska skrivas toohello-fil som ska skapas. Hello filinnehåll Observera i som du kan använda någon av hello egenskaper från hello utlösare som du skapade tidigare. Välj bara hello egenskaper hello listan visas. Du kan också ange hello **filen innehåll** text direkt i hello kontroll. I det här exemplet jag valt vissa egenskaper och lägga till blanksteg och ett bindestreck mellan varje egenskap.        
   ![SharePoint online åtgärd bild 8](./media/connectors-create-api-sharepointonline/action-8.png)  
10. Spara hello ändringar tooyour arbetsflöde  
11. Grattis, du har nu en fullt fungerande logikappen som utlöses när ett nytt objekt läggs tooa SharePoint Online-lista. hello app skapar sedan en fil med några av hello egenskaper från hello nytt listobjekt.  Nu kan du testa den genom att skapa ett nytt objekt i hello SharePoint-lista. 

