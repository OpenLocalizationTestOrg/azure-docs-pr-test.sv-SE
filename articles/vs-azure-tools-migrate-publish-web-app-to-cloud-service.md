---
title: "aaaHow tooMigrate och publicera en webbprogrammet tooan Azure Cloud Service från Visual Studio | Microsoft Docs"
description: "Lär dig hur toomigrate och publicera din web application tooan Azure-molntjänst med hjälp av Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9394adfd-a645-4664-9354-dd5df08e8c91
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: a2832c37d2ebdbc1e69a307d16b65b1c87e9070c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-migrate-and-publish-a-web-application-tooan-azure-cloud-service-from-visual-studio"></a>Så här: migrera och publicera en webbprogrammet tooan Azure Cloud Service från Visual Studio
tootake nytta av Hej värdbaserade tjänster och skalbarhet i Azure, du kanske vill toomigrate och publicera programmet tooan Azure-molnet webbtjänsten. Du kan köra ett webbprogram i Azure med minimala ändringar tooyour befintliga program.

> [!NOTE]
> Den här artikeln handlar om hur du distribuerar toocloud tjänster, inte tooweb platser. Information om hur du distribuerar tooweb platser finns i [distribuera en webbapp i Azure App Service](app-service-web/web-sites-deploy.md).
>
>

En lista över specifika mallar som har stöd för både Visual C# och Visual Basic, i avsnittet hello **stöds projektmallar** senare i det här avsnittet.

Du måste först aktivera ditt webbprogram för Azure från Visual Studio. hello följande bild visar hello viktiga steg toopublish ditt befintliga webbprogram genom att lägga till en Azure-projekt toouse för distribution. Den här processen lägger till ett Azure-projekt med hello krävs web rollen tooyour lösning. Baserat på hello typ av webbprojekt som du har uppdateras hello projektegenskaperna för sammansättningar också om hello servicepaket kräver ytterligare paket för distribution.

![Publicera en Web application tooMicrosoft Azure](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

> [!NOTE]
> Hej **konvertera**, **konvertera tooAzure Molntjänstprojekt** kommandot visas bara för hello webbprojekt i din lösning. Till exempel är hello-kommandot inte tillgängligt för en Silverlight-projekt i din lösning.
> När du skapar ett tjänstepaket eller publicera dina program tooAzure inträffa varningar eller fel. Dessa varningar och fel kan hjälpa dig att åtgärda problem innan du distribuerar tooAzure. Exempelvis kan du få en varning om saknas en sammansättning. Mer information om hur tootreat alla varningar som fel, se [konfigurera Azure Cloud Service-projekt i Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Om du skapar ditt program, köra den lokalt med hjälp av hello beräkningsemulatorn eller publicera den tooAzure kan du se följande fel i hello hello **fellista** fönster: **hello anges sökvägen, filnamnet eller båda är för lång** . Detta fel uppstår eftersom hello hello fullständigt kvalificerade Azure projektets namn är för långt. hello kan inte hello projektnamn hello fullständig sökväg innehålla mer än 146 tecken. Detta är till exempel hello fullständig projektnamn inklusive sökväg för en Azure-projekt som har skapats för ett Silverlight-program: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Du kanske toomove lösning tooa olika katalogen som har en kortare sökväg tooreduce hello tidslängd hello fullständigt kvalificerat namn.
>
>

toomigrate och publicera en web application tooAzure från Visual Studio, Följ dessa steg.

## <a name="enable-a-web-application-for-deployment-tooazure"></a>Aktivera ett program för distribution tooAzure
### <a name="tooenable-a-web-application-for-deployment-tooazure"></a>tooenable ett webbprogram för distribution tooAzure
1. tooenable ditt webbprogram för distribution tooAzure, öppna hello snabbmenyn för en webbplats projektet i din lösning och välj Lägg till projekt för Azure-distribution.

    följande åtgärder hello uppstå:

   * Ett Azure-projekt kallas `<name of hello web project>.Azure` läggs toohello lösning för ditt program.
   * En webbroll för hello webbprojekt läggs toothis Azure-projekt.
   * Hej **kopiera lokala** egenskapen tootrue för alla sammansättningar som krävs för MVC 2, MVC 3, 4 MVC och Silverlight affärsprogram. Detta lägger till dessa sammansättningar toohello tjänstpaket som används för distribution.

   > [!IMPORTANT]
   > Om du har andra sammansättningar eller filer som krävs för det här webbprogrammet, måste du manuellt ange hello egenskaper för filerna. Information om hur tooset dessa egenskaper finns hello avsnittet **inkludera filer i hello servicepaket** senare i den här artikeln.
   >
   > [!NOTE]
   > Om en webbroll för en specifik webbprojekt som redan finns i ett Azure-projekt i hello lösning **konvertera**, **konvertera tooAzure Molntjänstprojekt** visas inte på den här webbprojektet hello snabbmeny .
   >
   >

   Om du har flera webbprojekt i ditt webbprogram och du vill toocreate web-roller för varje webbprojekt, utför du hello stegen i den här proceduren för varje webbprojekt. Detta skapar separata Azure projekt för varje webbroll. Varje webbprojekt kan publiceras separat. Du kan också du kan manuellt lägga till en annan tooan befintliga Azure webbrollsprojektet i ditt webbprogram. toodo detta, öppna hello snabbmenyn för hello **roller** väljer du mappen i projektet Azure **Lägg till**, sedan **Webbrollsprojektet i lösningen**, Välj hello projektet tooadd som en webbplats roll, och välj sedan hello **OK** knappen.

## <a name="use-an-azure-sql-database-for-your-application"></a>Använda en Azure SQL-databas för programmet
Om du har en anslutningssträng för ditt webbprogram som använder en SQL Server-databas som är lokalt hello måste du ändra den här anslutningen sträng toouse en instans av SQL-databas som är värd för Azure i stället.

> [!IMPORTANT]
> Din prenumeration måste aktivera toouse SQL-databas. Om du har åtkomst till din prenumeration från hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), kan du bestämma vilka tjänster som ger din prenumeration. hello följande instruktioner gäller toohello publicerat [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885). Om du använder hello [Azure-portalen](http://portal.microsoft.com), hoppa över toohello nästa procedur.
>
>

### <a name="toouse-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>toouse en SQL-databasinstans i din webbroll för anslutningssträngen
1. toocreate en instans av SQL-databas i hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), hello åtgärderna i följande artikel hello: [skapa en SQL-databasserver](http://go.microsoft.com/fwlink/?LinkId=225109).

   > [!NOTE]
   > När du konfigurerar hello brandväggsregler för din instans av SQL-databas måste du välja hello **Tillåt den här servern för andra Azure-tjänster tooaccess** kryssrutan.
   >
   >
2. toocreate en instans av SQL-databas toouse för anslutningssträngen, gör hello i nästa avsnitt om hello i hello följande artikel: [skapa en SQL-databas](http://go.microsoft.com/fwlink/?LinkId=225110).
3. toocopy hello ADO.NET anslutning sträng toouse för anslutningssträngen, utför följande steg i hello hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).  

   1. Välj hello **databasen** knappen och öppna sedan hello nod för hello prenumeration som du använde toocreate din instans av SQL-databas.
   2. toodisplay hello tillgängliga instanser av SQL-databas, Välj hello **SQL-databaser** nod.
   3. toodisplay hello egenskaperna för hello-databasen, Välj hello-databas. Hej **egenskaper** vyn visas.

      > [!NOTE]
      > Om hello **egenskaper** vyn inte visas, måste du kanske tooopen den med hjälp av hello avgränsare.
      >
      >
   4. toodisplay hello anslutningssträngar, Välj hello ellips (...)-knappen Nästa tooView.

      Hej **anslutningssträngar** dialogrutan visas.
   5. toocopy hello ADO.NET-anslutningssträngen, markera hello text och välja hello Ctrl + C nycklar.
   6. tooclose hello dialogrutan Välj hello **Stäng** knappen.
4. tooreplace hello anslutning sträng i hello web.config-filen toouse den här instansen av SQL-databas, öppna hello web.config-filen, markera strängpost hello befintliga anslutningen och välj sedan hello Ctrl + V nycklar. hello ADO.NET-anslutningssträngen för hello instansen av SQL-databas ersätter hello befintliga anslutningssträngen.
5. Du måste också lägga till parametern hello `MultipleActiveResultSets=True` toohello anslutningssträngen. hello anslutningssträngen måste ha hello följande format:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```
6. (Valfritt) En alternativ metod toochanging hello-anslutningssträng är direkt i hello web.config-filen tooadd ett avsnitt i en hello web.config-transformeringsfiler, beroende på hello build-konfiguration som du använder toocreate ditt abonnemang. Öppna antingen hello Web.Debug.Config eller hello Web.Release.Config. Lägg till följande avsnitt i den här filen hello:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```
7. Spara hello-fil som du har ändrat och publicera programmet.

### <a name="toouse-an-instance-of-sql-database-by-using-hello-azure-classic-portal"></a>toouse en instans av SQL-databas med hjälp av hello klassiska Azure-portalen
1. I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), välja hello SQL-databaser nod.

   * Om hello instans av SQL-databas som du vill toouse visas kan du välja tooopen den.
   * Om du inte skapat några instanser, Välj hello lämplig länk och sedan skapa en instans.
2. När du öppnar eller skapar en databasinstans väljer hello **anslutningssträngar** länk.
3. Hello längst ned på sidan hello i Välj hello länken tooconfigure brandväggsinställningar, och accepterar hello standardvärdena eller konfigurera hello-värden som du behöver.
4. Kopiera hello ADO.NET-anslutningssträngen, klistrar in den i web.config-filen över hello gamla anslutningssträngen för hello lokala-databasen och vara säker på att tooadd `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-tooazure"></a>Publicera ett webbprogram tooAzure
### <a name="toopublish-a-web-application-tooazure"></a>toopublish en Web application tooAzure
1. tootest hello programmet i hello lokala utvecklingsmiljö med hello Azure-beräkningsemulatorn, öppna hello snabbmenyn för hello Azure projektet för hello-webbroll och välj **Set as Startup Project**. Välj **felsöka**, **Start Debugging** (tangentbord: **F5**).

    Hej **Start hello Azure felsökning miljö** öppnas och hello programmet startas i hello webbläsare. Mer information om hur toostart varje typ av webbprogram i hello compute emulator, se hello tabellen i det här avsnittet.
2. tooset upp ditt program toopublish tooAzure hello-tjänster som du måste ha ett Microsoft-konto och en Azure-prenumeration. Använd hello stegen i hello efter avsnittet tooset in dina tjänster: [förbereda toopublish eller distribuera ett Azure-program från Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
3. toopublish hello web application tooAzure öppna hello snabbmenyn för hello-projektet och välj **publicera tooAzure**.

    Hej **publicera Azure-programmet** öppnas och Visual Studio startas hello distributionsprocessen. Mer information om hur toopublish hello program i avsnittet hello **publicera ett Azure-program från Visual Studio** i [publicering av en tjänst i molnet med hjälp av hello Azure verktyg](vs-azure-tools-publishing-a-cloud-service.md).

   > [!NOTE]
   > Du kan också publicera hello webbprogrammet från hello Azure-projekt. toodo, öppna hello snabbmenyn för hello Azure-projekt och välj **publicera**.
   >
   >
4. toosee hello fortskrider hello distribution, kan du visa hello **Azure-aktivitetsloggen** fönster. Den här loggen visas automatiskt när hello distributionsprocessen startar. Du kan expandera hello radobjekt i aktivitetsloggen för hello tooshow detaljerad information som visas i följande illustration hello:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)
5. (Valfritt) toocancel hello distributionsprocessen, öppna hello snabbmenyn för hello radobjekt i hello aktivitetsloggen och välj **Avbryt och ta bort**. Detta stoppar hello distributionsprocessen och tar bort hello distributionsmiljön från Azure.

   > [!NOTE]
   > tooremove den här distributionsmiljön efter att det har distribuerats, måste du använda hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).
   >
   >
6. (Valfritt) När dina rollinstanser har startat visar hello distributionsmiljön Visual Studio automatiskt i hello **Azure Compute** nod i **Cloud Explorer** eller **Server Explorer**. Härifrån kan du visa hello statusen för hello enskilda rollinstanser.

    hello följande bild visar hello rollinstanser i **Server Explorer** medan de fortfarande hello initierar tillstånd:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)
7. tooaccess programmet efter distributionen, välja hello pilen Nästa tooyour distributionen när statusen **slutförd** visas i hello **Azure-aktivitetsloggen**. Detta visar hello URL för ditt webbprogram i Azure. Se hello i den följande tabellen hello information om hur toostart en specifik typ av webbprogrammet från Azure.

    hello i den följande tabellen listas hello information om hur toostart webbprogram från Azure eller toorun eller specifika felsöka ett webbprogram lokalt med hjälp av hello Azure Compute Emulator:

   | Web programtyp | Kör/Debug lokalt med hjälp av hello Compute Emulator | körs i Azure |
   | --- | --- | --- |
   | ASP.NET-webbprogram |På menyraden hello väljer **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Välj hello URL: en hyperlänk visas i hello **distribution** för hello **Azure-aktivitetsloggen** tooload hello startsidan i hello webbläsare. |
   | Webbprogram för ASP.NET MVC 2 |På menyraden hello väljer **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Välj hello URL: en hyperlänk visas i hello **distribution** för hello **Azure-aktivitetsloggen** tooload hello startsidan i hello webbläsare. |
   | Webbprogram för ASP.NET MVC 3 |På menyraden hello väljer **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Välj hello URL: en hyperlänk visas i hello **distribution** för hello **Azure-aktivitetsloggen** tooload hello startsidan i hello webbläsare. |
   | Webbprogram för ASP.NET MVC 4 |På menyraden hello väljer **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Välj hello URL: en hyperlänk visas i hello **distribution** för hello **Azure-aktivitetsloggen** tooload hello startsidan i hello webbläsare. |
   | Tom ASP.NET-webbprogram |Du måste lägga till en .aspx-sida i ditt program som du anger som hello startsida för webbprojektet. På menyraden hello Välj **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Om du har en .aspx standardsida i ditt program, Välj hello URL: en hyperlänk visas i hello **distribution** för hello **Azure-aktivitetsloggen** och den här sidan har lästs in i hello webbläsare. Om du har en annan .aspx-sida måste toonavigate toothis viss sida med hjälp av följande format för URL: en hello:`<url for deployment>/<name of page>.aspx` |
   | Silverlight-program |På menyraden hello väljer **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Du behöver toonavigate toohello viss sida för ditt program med hjälp av följande format för URL: en hello:`<url for deployment>/<name of page>.aspx` |
   | Silverlight affärsprogram |På menyraden hello väljer **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Du behöver toonavigate toohello viss sida för ditt program med hjälp av följande format för URL: en hello:`<url for deployment>/<name of page>.aspx` |
   | Navigering Silverlight-program |På menyraden hello väljer **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Du behöver toonavigate toohello viss sida för ditt program med hjälp av följande format för URL: en hello:`<url for deployment>/<name of page>.aspx` |
   | WCF-tjänstprogram |Du måste ange hello .svc-filen som hello startsida för projektet WCF-tjänst. På menyraden hello Välj **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Du behöver toonavigate toohello svc-fil för ditt program med hjälp av följande format för URL: en hello:`<url for deployment>/<name of service file>.svc` |
   | Tjänstprogrammet för WCF-arbetsflöde |Du måste ange hello .svc-filen som hello startsida för projektet WCF-tjänst. På menyraden hello Välj **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Du behöver toonavigate toohello svc-fil för ditt program med hjälp av följande format för URL: en hello:`<url for deployment>/<name of service file>.svc` |
   | ASP.NET dynamiska entiteter |På menyraden hello väljer **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Du måste uppdatera anslutningssträngen hello (se nästa avsnitt). Du måste också toonavigate toohello viss sida för ditt program med hjälp av följande format för URL: en hello:`<url for deployment>/<name of page>.aspx` |
   | ASP.NET dynamiska Data Linq tooSQL |På menyraden hello väljer **felsöka**, **Start Debugging** (tangentbord: Välj hello **F5** nyckel.). |Du måste följa hello stegen i den här proceduren: använder en SQL Azure-databas för ditt program (se tidigare avsnitt i det här avsnittet). Du måste också toonavigate toohello viss sida för ditt program med hjälp av följande format för URL: en hello:`<url for deployment>/<name of page>.aspx` |

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Uppdatera en anslutningssträng för ASP.NET dynamiska entiteter
### <a name="tooupdate-a-connection-string-for-aspnet-dynamic-entities"></a>tooUpdate en anslutningssträng för dynamiska ASP.NET-entiteter
1. toocreate en SQL Azure-databas som kan användas för ett dynamiskt entiteter för ASP.NET-webbprogram följer hello stegen i proceduren hello **använder en SQL Azure-databas för tillämpningsprogrammet** tidigare i det här avsnittet.
2. Lägg till hello tabeller och fält som ska användas för den här databasen från hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).
3. hello anslutningssträngen för den här typen av program har hello följande format i hello web.config-fil:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Uppdatera hello *connectionString* värdet med hello ADO.NET-anslutningssträngen för SQL Azure-databasen på följande sätt:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```
4. toosave hello web.config-filen med hello ändringar som du har gjort toohello anslutningssträngen, hello menyraden väljer **filen**, **spara web.config**.

## <a name="supported-project-templates"></a>Stöds projektmallar
toopublish en web application tooAzure hello programmet måste använda ett hello projektmallar för C# eller Visual Basic som anges i hello nedan.

| Projektgruppen för mallen | Projektmall |
| --- | --- |
| Webb |ASP.NET-webbprogram |
| Webb |Webbprogram för ASP.NET MVC 2 |
| Webb |Webbprogram för ASP.NET MVC 3 |
| Webb |MVC4 för ASP.NET-webbprogram |
| Webb |Tom ASP.NET-webbprogram |
| Webb |Tomt webbprogram för ASP.NET MVC 2 |
| Webb |Webbprogram för ASP.NET dynamiska Data entiteter |
| Webb |ASP.NET dynamiska Data Linq tooSQL webbprogram |
| Silverlight |Silverlight-program |
| Silverlight |Silverlight affärsprogram |
| Silverlight |Navigering Silverlight-program |
| WCF |WCF-tjänstprogram |
| WCF |Tjänstprogrammet för WCF-arbetsflöde |
| Arbetsflöde |Tjänstprogrammet för WCF-arbetsflöde |

## <a name="next-steps"></a>Nästa steg
Mer information om publicering finns [förbereda tooPublish eller distribuera ett Azure-program från Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Se även [inställningen upp med namnet autentiseringsuppgifter](vs-azure-tools-setting-up-named-authentication-credentials.md).
