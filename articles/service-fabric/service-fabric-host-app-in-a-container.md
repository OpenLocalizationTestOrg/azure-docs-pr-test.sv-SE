---
title: "aaaDeploy en .NET-app i en behållare tooAzure Service Fabric | Microsoft Docs"
description: "Lär dig hur toopackage en .NET-app i Visual Studio i en Docker-behållare. Den här nya ”behållare” app distribueras sedan tooa Service Fabric-klustret."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: mikhegn
ms.openlocfilehash: 094b0e71d76b2e56ffb9b23638dd8154b3aff5fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-net-application-in-a-windows-container-tooazure-service-fabric"></a>Distribuera ett .NET-program i en Windows-behållaren tooAzure Service Fabric

De här självstudierna visar hur toodeploy ett befintligt ASP.NET-program i en Windows-behållare på Azure.

I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Skapa en Docker-projekt i Visual Studio
> * Containerize ett befintligt program
> * Installationsprogrammet kontinuerlig integrering med Visual Studio och VSTS

## <a name="prerequisites"></a>Krav

1. Installera [Docker CE för Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) så att du kan köra behållare på Windows 10.
2. Bekanta dig med hello [Windows 10 behållare quickstart][link-container-quickstart].
3. Hämta hello [Fabrikam Fiber CallCenter] [ link-fabrikam-github] exempelprogrammet.
4. Installera [Azure PowerShell][link-azure-powershell-install]
5. Installera hello [kontinuerlig leveransverktyg tillägget för Visual Studio 2017][link-visualstudio-cd-extension]
6. Skapa en [Azure-prenumeration] [ link-azure-subscription] och en [Visual Studio Team Services-konto][link-vsts-account]. 
7. [Skapa ett kluster i Azure](service-fabric-tutorial-create-cluster-azure-ps.md)

## <a name="containerize-hello-application"></a>Containerize hello program

Nu när du har en [Service Fabric-klustret körs i Azure](service-fabric-tutorial-create-cluster-azure-ps.md) du är redo toocreate och distribuera en av programmet. toostart kör appen i en behållare, behöver vi tooadd **Docker stöd** toohello projekt i Visual Studio. När du lägger till **Docker stöd** toohello programmet två saker. Först en _Dockerfile_ läggs toohello projekt. Den nya filen beskriver hur hello behållaren bilden är toobe som har skapats. Sedan andra, en ny _docker compose_ projektet läggs toohello lösning. hello nytt projekt innehåller några docker compose filer. Docker compose filer kan vara används toodescribe hur hello behållare körs.

Mer information om hur du arbetar med [verktyg för Visual Studio-behållaren][link-visualstudio-container-tools].

>[!NOTE]
>Om den är hello första gången du kör Windows behållaren bilder på datorn, måste Docker CE hämtar hello grundläggande avbildningar för din behållare. hello bilder används i den här kursen är 14 GB. Gå vidare och kör följande terminal kommandot toopull hello grundläggande bilder hello:
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a>Lägga till Docker-stöd

Öppna hello [FabrikamFiber.CallCenter.sln] [ link-fabrikam-github] filen i Visual Studio.

Högerklicka på hello **FabrikamFiber.Web** project > **Lägg till** > **Docker Support**.

### <a name="add-support-for-sql"></a>Lägga till stöd för SQL

Det här programmet används SQL som hello DataProvider och en SQLServer är obligatoriska toorun hello program. Referera till en SQL Server-behållaren avbildning i vår docker-compose.override.yml-filen.

Öppna i Visual Studio **Solution Explorer**, hitta **docker compose**, och öppna hello **docker compose.override.yml**.

Navigera toohello `services:` nod, lägga till en nod med namnet `db:` som definierar hello SQL Server-post för hello behållare.

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
>Du kan använda alla SQL-Server som du föredrar för lokala felsökning så länge den kan nås från värden. Dock **localdb** stöder inte `container -> host` kommunikation.

>[!WARNING]
>Kör SQL Server i en behållare stöder inte bestående data. Dina data raderas när hello behållaren stoppar. Använd inte den här konfigurationen för produktion.

Navigera toohello `fabrikamfiber.web:` nod och Lägg till en underordnad nod med namnet `depends_on:`. Detta säkerställer att hello `db` (hello SQL Server-behållare)-tjänsten startar före våra webbprogram (fabrikamfiber.web).

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-hello-web-config"></a>Uppdatera hello Webbkonfiguration

Tillbaka i hello **FabrikamFiber.Web** projektet update hello anslutningssträngen i hello **web.config** filen toopoint toohello SQL Server i hello behållaren.

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
>Om du vill toouse en annan SQL-Server när du skapar en Versionspost skapa för ditt webbprogram, kan du lägga till en annan sträng tooyour web.release.config anslutningsfilen.

### <a name="test-your-container"></a>Testa din behållare

Tryck på **F5** toorun och felsökningsloggar hello-program i din behållaren.

Edge öppnar programmets definierade startsida med hello IP-adress för hello behållare i hello interna NAT nätverket (vanligtvis 172.x.x.x). toolearn mer information om hur du felsöker program i behållare med hjälp av Visual Studio 2017 finns [i den här artikeln][link-debug-container].

![exempel på fabrikam i en behållare][image-web-preview]

hello-behållaren är nu redo toobe skapats och paketeras i en Service Fabric-programmet. När du har hello behållaren avbildningen skapades på datorn kan du dra nedåt tooany värden toorun push-installera den tooany behållare registret.

## <a name="get-hello-application-ready-for-hello-cloud"></a>Förbereda hello program för hello moln

tooget hello program som är redo för att köra i Service Fabric i Azure, behöver vi toocomplete två steg:

1. Exponera hello port där vi vill toobe kan tooreach våra webbprogram i hello Service Fabric-klustret.
2. Ange en klar SQL-databas för produktion för vårt program.

### <a name="expose-hello-port-for-hello-app"></a>Exponera hello port för hello app
Vi har konfigurerat hello Service Fabric-klustret har port *80* öppen som standard i hello Azure belastningsutjämnare, som balanserar inkommande trafik toohello klustret. Via vårt filen docker-compose.yml kan du använda vår behållaren på den här porten.

Öppna i Visual Studio **Solution Explorer**, hitta **docker compose**, och öppna hello **docker compose.override.yml**.

Ändra hello `fabrikamfiber.web:` nod, lägga till en underordnad nod med namnet `ports:`.

Lägg till en sträng som värde `- "80:80"`.

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a>Använd en SQL-databas för produktion
När du kör i produktion kan vi behöver våra data kvar i vår databas. Det finns för närvarande inga sätt tooguarantee beständiga data i en behållare, därför du lagra inte produktionsdata i SQL Server i en behållare.

Vi rekommenderar att du använder en Azure SQL Database. tooset in och köra en hanterad SQL Server på Azure, gå hello [Azure SQL Database Snabbstart] [ link-azure-sql] artikel.

>[!NOTE]
>Kom ihåg toochange hello anslutning strängar toohello SQLServer i hello **web.release.config** filen i hello **FabrikamFiber.Web** projekt.
>
>Det här programmet misslyckas programinstallation utan problem om inga SQL-databasen inte kan nås. Du kan välja toogo vidare och distribuera hello program med ingen SQLServer.

## <a name="deploy-with-visual-studio-team-services"></a>Distribuera med Visual Studio Team Services

tooset upp distribution med hjälp av Visual Studio Team Services behöver du tooinstall hello [kontinuerlig leveransverktyg tillägget för Visual Studio-2017][link-visualstudio-cd-extension]. Det här tillägget gör det enkelt toodeploy tooAzure genom att konfigurera Visual Studio Team Services och hämta app distribuerat tooyour Service Fabric-klustret.

tooget igång din kod måste finnas i källkontroll. hello resten av det här avsnittet förutsätter **git** används.

### <a name="set-up-a-vsts-repo"></a>Konfigurera en VSTS repo
Hello nedre högra hörnet av Visual Studio klickar du på **lägga till tooSource kontrollen** > **Git** (eller alternativet som du föredrar).

![Klicka på hello källa kontroll för][image-source-control]

I hello _Team Explorer_ rutan, tryck på **publicera Git Repo**.

Välj VSTS databasnamn och tryck på **databasen**.

![Publicera lagringsplatsen tooVSTS][image-publish-repo]

Nu när koden synkroniseras med en VSTS källdatabasen, kan du konfigurera kontinuerlig integrering och kontinuerlig leverans.

### <a name="setup-continuous-delivery"></a>Installationsprogrammet kontinuerlig leverans

I _Solution Explorer_, högerklicka på hello **lösning** > **konfigurera kontinuerlig leverans**.

Välj hello Azure-prenumeration.

Ange **värdtyp** för**Service Fabric-kluster**.

Ange **målvärden** toohello service fabric-kluster som du skapade i föregående avsnitt i hello.

Välj en **behållare registret** toopublish behållare för att.

>[!TIP]
>Använd hello **redigera** knappen toocreate ett register för behållaren.

Tryck på **OK**.

![installationsprogrammet för service fabric kontinuerlig integration][image-setup-ci]
   
   När hello konfigurationen har slutförts är din behållaren distribuerade tooService Fabric. När du trycker på uppdateringar toohello databasen körs en ny version och utgåva.
   
   >[!NOTE]
   >Skapa hello behållaren bilder tar ungefär 15 minuter.
   >hello första distributionen toohello Service Fabric-klustret gör hello grundläggande Windows Server Core behållaren bilder toobe hämtas. hello download tar toocomplete ytterligare 5-10 minuter.

Bläddra toohello Fabrikam Callcenter program med hjälp av hello url för klustret: till exempel *http://mycluster.westeurope.cloudapp.azure.com*

Nu när du har av och distribuerats hello Fabrikam Callcenter lösningen, kan du öppna hello [Azure-portalen] [ link-azure-portal] och se hello-program som körs i Service Fabric. tootry hello program, öppna en webbläsare och gå toohello URL för Service Fabric-klustret.

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Skapa en Docker-projekt i Visual Studio
> * Containerize ett befintligt program
> * Installationsprogrammet kontinuerlig integrering med Visual Studio och VSTS

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance toohello next tutorial toolearn how toobind a custom SSL certificate tooit.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate tooAzure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
