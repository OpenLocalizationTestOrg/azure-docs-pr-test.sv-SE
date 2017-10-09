1. Logga in på hello [Azure Portal].
2. Klicka på **+NY** > **Webb + Mobil** > **Mobilapp** och ange sedan ett namn för din serverdel för mobilappen.
3. För hello **resursgruppen**, Välj en befintlig resursgrupp eller skapa en ny (med hjälp av hello samma namn som din app.) 
   
    Du kan antingen välja en annan App Service-plan eller skapa en ny. Mer information om App Service-planer och hur toocreate en ny plan i en annan prisnivå på datanivå och på din önskade plats finns [Azure App Service-planer djupgående översikt över](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. För hello **programtjänstplanen**, hello standardplanen (i hello [standardnivån](https://azure.microsoft.com/pricing/details/app-service/)) är markerad. Du kan också välja en annan plan eller [skapa en ny](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). hello App Service-planens inställningar avgör hello [plats, funktioner, kostnad och beräkningsresurser](https://azure.microsoft.com/pricing/details/app-service/) som är associerade med din app. 
   
    När du bestämmer dig för hello planen klickar du på **skapa**. Detta skapar hello mobilappsserverdel. 
5. I hello **inställningar** bladet för hello ny mobilappsserverdel och klicka på **Snabbstart** > din klientapplattform > **ansluta en databas**. 
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
6. I hello **Lägg till dataanslutning** bladet, klickar du på **SQL-databas** > **skapa en ny databas**, typen hello databasen **namnet**, Välj prisnivå och klicka sedan på **Server**.  Du kan återanvända den nya databasen. Om du redan har en databas i hello samma plats kan du istället välja **Använd en befintlig databas**. hello användning av en databas på en annan plats rekommenderas inte på grund av toobandwidth kostnader och högre latens.
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
7. I hello **ny server** bladet, Skriv ett unikt servernamn i hello **servernamn** anger ett inloggningsnamn och lösenord, kontrollera **Tillåt azure-tjänster tooaccess server**, och klicka på **OK**. Detta skapar nya hello-databasen.
8. Tillbaka i hello **Lägg till dataanslutning** bladet klickar du på **anslutningssträngen**, ange värden för hello inloggningsnamn och lösenord för din databas och på **OK**. Vänta några minuter tills hello databasen toobe distribuerats innan du fortsätter.

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/
