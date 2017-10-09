---
title: "aaaInstall programpaket på datornoderna - Azure Batch | Microsoft Docs"
description: "Använd hello tillämpningsfunktion paket för Azure Batch tooeasily hantera flera program och versioner för installation på Batch-beräkningsnoder."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 683be7b7f1bd5db7835332016f6dccb72f45c3b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-applications-toocompute-nodes-with-batch-application-packages"></a>Distribuera program toocompute noder med Batch-programpaket

hello programmet paket funktion i Azure Batch tillhandahåller enkel hantering av aktiviteten program och deras distribution toohello compute-noder i din pool. Du kan använda programpaket, för att ladda upp och hantera flera versioner av hello program kör aktiviteter, inklusive tillhörande stödfiler. Du kan sedan distribuera automatiskt en eller flera av dessa program toohello compute-noder i din pool.

I den här artikeln får du lära dig hur tooupload och hantera programpaket i hello Azure-portalen. Sedan får du lära dig hur tooinstall dem på en pool compute-noder med hello [Batch .NET] [ api_net] bibliotek.

> [!NOTE]
> 
> Programpaket kan användas för alla Batch-pooler som skapats efter 5 juli 2017. De stöds i Batch-adresspooler skapa mellan 10 mars 2016 och 5 juli 2017 om hello poolen har skapats med en tjänst i molnet. Batch-pooler som skapats tidigare too10 mars 2016 stöder inte programpaket.
>
> hello API: er för att skapa och hantera programpaket är en del av hello [Batch Management .NET] [[api_net_mgmt]] bibliotek. hello API: er för att installera paket med program på en beräkningsnod är en del av hello [Batch .NET] [ api_net] bibliotek.  
>
> hello paket tillämpningsfunktion beskrivs här ersätter hello Batch-appar funktion som är tillgänglig i tidigare versioner av hello-tjänsten.
> 
> 

## <a name="application-package-requirements"></a>Paketet programkrav
toouse programpaket du behöver för[länka ett Azure Storage-konto](#link-a-storage-account) tooyour Batch-kontot.

Den här funktionen introducerades i [Batch REST API] [ api_rest] version 2015-12-01.2.2 och hello motsvarande [Batch .NET] [ api_net] version library 3.1.0. Vi rekommenderar att du alltid använder hello senaste API-versionen när du arbetar med Batch.

> [!NOTE]
> Programpaket kan användas för alla Batch-pooler som skapats efter 5 juli 2017. De stöds i Batch-adresspooler skapa mellan 10 mars 2016 och 5 juli 2017 om hello poolen har skapats med en tjänst i molnet. Batch-pooler som skapats tidigare too10 mars 2016 stöder inte programpaket.
>
>

## <a name="about-applications-and-application-packages"></a>Om program och programpaket
I Azure Batch en *programmet* refererar tooa uppsättning version binärfiler som kan vara automatiskt hämtade toohello beräkningsnoder i din pool. En *programpaket* refererar tooa *specifika* av dessa binärfiler och representerar en viss *version* av programmet hello.

![Översiktsdiagram över program och programpaket][1]

### <a name="applications"></a>Program
Ett program i batchen innehåller en eller flera program paket och anger konfigurationsalternativ för hello program. Ett program kan exempelvis ange hello standard programmet paketet version tooinstall på datornoder och om dess paket kan uppdateras eller tas bort.

### <a name="application-packages"></a>Programpaket
Ett programpaket är en .zip-fil som innehåller hello binärfiler och stödfiler som krävs för uppgifter toorun hello programmet. Varje programpaket representerar en viss version av programmet hello.

Du kan ange programpaket på hello poolen och aktivitet. Du kan ange en eller flera av dessa paket och (valfritt) en version när du skapar en pool eller aktiviteten.

* **Poolen programpaket** distribueras för*varje* nod i hello pool. Program distribueras när en nod ansluter till en pool och när den startas om eller avbildade.
  
    Poolen programpaket är lämplig när alla noder i en pool köra ett jobb aktiviteter. Du kan ange en eller flera programpaket när du skapar en pool och du kan lägga till eller uppdatera en befintlig adresspool paket. Om du uppdaterar en befintlig adresspool programpaket måste du starta om dess noder tooinstall hello nya paket.
* **Uppgift programpaket** distribueras endast tooa beräkningsnod schemalagd toorun en aktivitet innan du kör kommandoraden hello aktivitet. Om hello anges programpaket och versionen finns redan på hello nod, det är inte omdistribueras och hello befintliga paketet används.
  
    Uppgiften programpaket är användbara i miljöer med delad pool, där olika jobben körs på en pool och hello poolen tas inte bort när ett jobb har slutförts. Om jobbet har färre uppgifter än noder i hello pool, kan aktivitet programpaket minimera dataöverföring eftersom programmet är distribuerade endast toohello noder som kör aktiviteter.
  
    Andra scenarier kan dra nytta av aktiviteten programpaket finns jobb som kör ett stort program, men endast för några aktiviteter. Till exempel kan ett före bearbetning steg eller en merge-aktivitet, där hello föregående bearbetning eller merge-program är tunga, dra nytta av programpaket för aktiviteten.

> [!IMPORTANT]
> Det finns begränsningar på hello antal program och programpaket i ett batchkonto och hello största storlek för paketet. Se [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md) mer information om dessa begränsningar.
> 
> 

### <a name="benefits-of-application-packages"></a>Fördelarna med programpaket
Programpaket kan förenkla hello koden i Batch-lösningen och lägre hello administration krävs toomanage hello program som körs i dina uppgifter.

Med programpaket saknar din pool startuppgift toospecify en lång lista med en enskild resurs filer tooinstall på hello noder. Du har inte toomanually hantera flera versioner av dina filer i Azure Storage, eller på noderna. Och du inte behöver tooworry om hur du genererar [SAS-URL: er](../storage/common/storage-dotnet-shared-access-signature-part-1.md) tooprovide åtkomst toohello filer i ditt lagringskonto. Batch-fungerar på hello bakgrund med Azure Storage toostore programpaket och distribuerar dem toocompute noder.

> [!NOTE] 
> hello total storlek på Startuppgiften måste vara mindre än eller lika med too32768 tecken, inklusive resursfiler och miljövariabler. Om din startuppgift överskrider denna gräns, är med programpaket ett annat alternativ. Du kan också skapa ett komprimerade Arkiv som innehåller din resursfiler, ladda upp den som en blob-tooAzure lagring och packa upp den från hello kommandorad för start-aktivitet. 
>
>

## <a name="upload-and-manage-applications"></a>Ladda upp och hantera program
Du kan använda hello [Azure-portalen] [ portal] eller hello [Batch Management .NET](batch-management-dotnet.md) biblioteket toomanage hello programpaket Batch-kontot. Nästa Hej i avsnitten, först visar vi hur toolink ett lagringskonto diskutera sedan att lägga till program och paket- och hantera dem med hello portal.

### <a name="link-a-storage-account"></a>Länka ett lagringskonto
toouse programpaket måste du först koppla ett Azure Storage-konto tooyour Batch-kontot. Om du ännu inte har konfigurerat ett lagringskonto hello Azure-portalen visas en varning hello första gången du klickar på hello **program** panelen i hello **Batch-kontot** bladet.

> [!IMPORTANT]
> Batch för närvarande stöder *endast* hello **allmänna** lagringskontotypen enligt beskrivningen i steg 5 [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account)i [om Azure Storage-konton](../storage/common/storage-create-storage-account.md). När du länkar ett Azure Storage-konto tooyour Batch-kontot, koppla *endast* en **allmänna** storage-konto.
> 
> 

!['Inget lagringskonto har konfigurerats, varning i Azure-portalen][9]

hello associerade Batch-tjänsten använder hello Storage-konto toostore ditt programpaket. När du har länkat hello två konton kan distribuera Batch automatiskt hello-paket som lagras i hello länkade Storage-konto tooyour compute-noder. toolink en Storage-konto tooyour Batch-kontot, klickar du på **lagringsutrymmet kontoinställningar** på hello **varning** bladet och klicka sedan på **Lagringskonto** på hello **Lagringskonto** bladet.

![Välj lagring konto-bladet i Azure-portalen][10]

Vi rekommenderar att du skapar ett lagringskonto *specifikt* för användning med Batch-kontot och markera den här. Mer information om hur toocreate ett lagringskonto i avsnittet ”Skapa ett lagringskonto” i [om Azure Storage-konton](../storage/common/storage-create-storage-account.md). När du har skapat ett lagringskonto kan du sedan koppla den tooyour Batch-kontot med hjälp av hello **Lagringskonto** bladet.

> [!WARNING]
> hello Batch-tjänsten använder Azure Storage toostore ditt programpaket som blockblobar. Du är [debiteras som vanligt] [ storage_pricing] för hello block blob-data. Vara säker på att tooconsider hello storlek och antal programpaket och tar regelbundet bort inaktuella paket toominimize kostnader.
> 
> 

### <a name="view-current-applications"></a>Visa aktuella program
tooview hello program i Batch-kontot, klickar du på hello **program** menyalternativet i hello vänstra menyn när du visar hello **Batch-kontot** bladet.

![Program sida vid sida][2]

Om du markerar alternativet öppnas hello **program** bladet:

![Lista över program][3]

Hej **program** bladet visar hello ID för varje program i ditt konto och hello följande egenskaper:

* **Paket**: hello antal versioner som är associerade med det här programmet.
* **Standardversionen**: hello programversion installeras om du inte anger en version när du anger hello program för en pool. Den här inställningen är valfri.
* **Tillåt uppdateringar**: hello-värde som anger om paketet uppdateringar, borttagningar och tillägg är tillåtna. Om värdet för**nr**, paketet uppdateringar och borttagningar är inaktiverat för hello program. Endast nya programpaketversioner kan läggas till. hello standardvärdet är **Ja**.

### <a name="view-application-details"></a>Visa programinformation
tooopen hello blad som innehåller hello information för ett program, Välj hello-program i hello **program** bladet.

![Programinformation][4]

Du kan konfigurera följande inställningar för programmet hello informationsbladet för hello program.

* **Tillåt uppdateringar**: Ange om dess programpaket kan uppdateras eller tas bort. Se ”uppdatera eller ta bort ett programpaket” senare i den här artikeln.
* **Standardversionen**: Ange en standard programmet paketet toodeploy toocompute noder.
* **Visningsnamn**: Ange ett eget namn som din lösning kan använda när den visas information om programmet hello, till exempel i hello Användargränssnittet för en tjänst som du anger tooyour kunder via Batch Batch.

### <a name="add-a-new-application"></a>Lägg till ett nytt program
toocreate nya program, lägga till ett programpaket och ange ett nytt, unikt-ID. hello första programpaketet som du lägger till med hello nya program-ID skapas också hello nytt program.

Klicka på **Lägg till** på hello **program** bladet tooopen hello **nytt program** bladet.

![Nya program bladet i Azure-portalen][5]

Hej **nytt program** bladet innehåller hello följande fält toospecify hello inställningarna för ditt nya program och programpaket.

**Program-id**

Det här fältet anger hello-ID för det nya programmet som är ämne toohello standard Azure Batch-ID valideringsregler. hello regler för att tillhandahålla ett program-ID är följande:

* På ett Windows kan hello ID innehålla en kombination av alfanumeriska tecken, bindestreck och understreck. På Linux-noder tillåts endast alfanumeriska tecken och understreck.
* Får inte innehålla fler än 64 tecken.
* Måste vara unika inom hello Batch-kontot.
* Är bevara och skiftlägesoberoende.

**Version**

Det här fältet anger hello-versionen av hello programpaketet som du överför. Version strängar är ämne toohello följande valideringsregler:

* På ett Windows kan hello versionssträng innehålla en kombination av alfanumeriska tecken, bindestreck, understreck och punkter. För Linux-noder, får hello versionssträng bara innehålla alfanumeriska tecken och understreck.
* Får inte innehålla fler än 64 tecken.
* Måste vara unika inom hello program.
* Är bevara och skiftlägesoberoende.

**Programpaket**

Det här fältet anger hello .zip-fil som innehåller hello binärfiler och stödfiler som krävs tooexecute hello program. Klicka på hello **Välj en fil** rutan eller hello mappen ikonen toobrowse tooand väljer en .zip-fil som innehåller filer som ditt program.

När du har valt en fil, klickar du på **OK** toobegin hello överför tooAzure lagring. När hello överföringen är klar, visas ett meddelande hello portal och stänger hello-bladet. Den här åtgärden kan ta lite tid beroende på hello storleken på hello-filen som du är överföring och hello hastigheten för nätverksanslutningen.

> [!WARNING]
> Stäng inte hello **nytt program** bladet innan hello överföringen är klar. Detta stoppar hello överföringen.
> 
> 

### <a name="add-a-new-application-package"></a>Lägg till ett nytt programpaket
tooadd en ny Paketversion för programmet för ett befintligt program väljer du en programmet hello **program** bladet, klickar du på **paket**, klicka på **Lägg till** tooopen Hej **Lägg till paketet** bladet.

![Lägg till program paketet bladet i Azure-portalen][8]

Som du ser hello fält matchar de hello **nytt program** bladet, men hello **program-id** kryssrutan är inaktiverad. Som du gjorde för hello nytt program ange hello **Version** för din nya paket, bläddra tooyour **programpaket** ZIP-filen och klicka sedan på **OK** tooupload hello paketet.

### <a name="update-or-delete-an-application-package"></a>Uppdatera eller ta bort ett programpaket
tooupdate eller ta bort ett befintligt programpaket, öppna hello informationsbladet för hello program klickar du på **paket** tooopen hello **paket** bladet, klickar du på hello **tre punkter**i hello raden i hello programpaketet som du vill toomodify och väljer hello åtgärd som du vill tooperform.

![Uppdatera eller ta bort paketet i Azure-portalen][7]

**Uppdatering**

När du klickar på **uppdatering**, hello *uppdateringspaketet* bladet visas. Det här bladet är liknande toohello *nytt programpaket* bladet, men endast hello paketet markeringen fältet är aktiverat, så att du toospecify tooupload för en ny ZIP-filen.

![Uppdatera paketet bladet i Azure-portalen][11]

**Ta bort**

När du klickar på **ta bort**, du uppmanas tooconfirm hello borttagning av hello Paketversion och Batch tar bort hello-paket från Azure Storage. Om du tar bort hello standardversionen av ett program hello **standardversionen** inställningen tas bort för hello program.

![Ta bort programmet][12]

## <a name="install-applications-on-compute-nodes"></a>Installera program på datornoderna
Nu när du har lärt dig hur toomanage programmet paket med hello Azure-portalen, diskuterar hur toodeploy dem toocompute noder och köra dem med Batch-uppgifter.

### <a name="install-pool-application-packages"></a>Installera poolen programpaket
tooinstall ett programpaket på alla compute-noder i en pool, ange en eller flera programpaket *referenser* för hello poolen. hello-programpaket som du anger för en pool är installerade på varje beräkningsnod när noden ansluter hello poolen och hello nod startas om eller avbildade.

Ange en eller flera i Batch .NET [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] när du skapar en ny pool eller för en befintlig adresspool. Hej [ApplicationPackageReference] [ net_pkgref] klassen anger en program-ID och version tooinstall på en pool compute-noder.

```csharp
// Create hello unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify hello application and version tooinstall on hello compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit hello pool so that it's created in hello Batch service. As hello nodes join
// hello pool, hello specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> Om en distribution av paketet misslyckas av någon anledning hello Batch-tjänstemärken hello nod [oanvändbar][net_nodestate], och inga aktiviteter som är schemalagda för körning på noden. I det här fallet bör du **starta om** hello nod tooreinitiate hello paketdistributionen. Starta om hello nod kan också schemaläggning igen på hello-nod.
> 
> 

### <a name="install-task-application-packages"></a>Installera aktivitet programpaket
Liknande tooa poolen, ange programpaket *referenser* för en aktivitet. När en aktivitet är schemalagd toorun på en nod, hello paketet hämtas och extraheras precis innan hello aktivitetens kommandoraden körs. Om ett angivet paket och version redan är installerad på noden hello hello paketet inte har hämtats och hello befintliga paketet används.

tooinstall ett programpaket för aktivitet, konfigurera hello aktivitet [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] egenskapen:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-hello-installed-applications"></a>Köra hello installerade program
hello paket som du har angett för en pool eller aktivitet har hämtat och extraherat tooa med namnet katalog i hello `AZ_BATCH_ROOT_DIR` för hello-nod. Batch skapar även en miljövariabel som innehåller hello sökvägen toohello heter katalogen. Din aktivitet kommandorader använda den här miljövariabeln referera till programmet hello på hello-nod. 

På ett Windows är hello variabeln i hello följande format:

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

Hello-formatet är något annorlunda på Linux-noder. Punkter (.), bindestreck (-) och nummertecken (#) är sammanslagna toounderscores i hello miljövariabeln. Exempel:

```
Linux:
AZ_BATCH_APP_PACKAGE_APPLICATIONID_version
```

`APPLICATIONID`och `version` är värden som motsvarar toohello program och Paketversion du har angett för distribution. Om du har angett att version 2.7 av programmet till exempel *blandaren* ska installeras på Windows-noder din aktivitet kommandorader använder den här variabeln tooaccess miljö dess filer:

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

Ange hello-miljövariabeln på Linux-noder i det här formatet:

```
Linux:
AZ_BATCH_APP_PACKAGE_BLENDER_2_7
``` 

När du överför ett programpaket kan du ange en standard version toodeploy tooyour compute-noder. Om du har angett en standardversion för ett program, kan du utelämna hello versionssuffix när du refererar till hello program. Du kan ange hello standardversionen för programmet i hello Azure-portalen på hello program bladet som visas i [ladda upp och hantera program](#upload-and-manage-applications).

Till exempel om du anger ”2.7” som hello standardversionen för programmet *blandaren*, och dina aktiviteter referera hello följande miljövariabeln och sedan Windows-noder som ska köra version 2.7:

`AZ_BATCH_APP_PACKAGE_BLENDER`

hello följande kodavsnitt visar ett exempel uppgiften-kommandoraden som startar hello standardversionen av hello *blandaren* program:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> Se [miljöinställningar för uppgifter](batch-api-basics.md#environment-settings-for-tasks) i hello [Batch funktionsöversikt](batch-api-basics.md) mer information om beräkning nod miljöinställningar.
> 
> 

## <a name="update-a-pools-application-packages"></a>Uppdatera programpaket för en pool
Om en befintlig pool har redan konfigurerats med ett programpaket, kan du ange ett nytt paket för hello poolen. Om du anger en ny paketet referens för en pool gäller hello följande:

* hello Batch-tjänsten installeras hello nyligen angivet paket på alla nya noder som hello pool för att ansluta till och alla befintliga nod som startats om eller avbildade.
* Compute-noder som redan finns i hello pool när du uppdaterar hello paketet refererar till inte installerar automatiskt hello nytt programpaket. Dessa compute-noder måste startas om eller avbildade tooreceive hello nytt paket.
* När ett nytt paket distribueras avspeglar hello skapade miljövariabler hello nya program paketet refererar till.

I det här exemplet hello befintlig pool har version 2.7 av hello *blandaren* program som har konfigurerats som en av dess [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref]. tooupdate hello poolen noder med version 2.76b, ange ett nytt [ApplicationPackageReference] [ net_pkgref] med hello ny version och commit hello ändrar.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Nu när hello nya versionen har konfigurerats, hello Batch-tjänsten installerar version 2.76b tooany *nya* nod som ansluter till hello poolen. tooinstall 2.76b på hello-noder som är *redan* i hello poolen, starta om eller återavbilda dem. Observera att behålla omstartad noder hello filer från tidigare distributioner för paketet.

## <a name="list-hello-applications-in-a-batch-account"></a>Lista hello program i ett Batch-konto
Du kan ange hello program och deras paket i ett Batch-konto med hjälp av hello [ApplicationOperations][net_appops].[ ListApplicationSummaries] [ net_appops_listappsummaries] metod.

```csharp
// List hello applications and their application packages in hello Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Sammanfattning
Med programpaket, kan du hjälpa kunderna Välj hello program för sitt arbete och ange hello exakt vilken version toouse vid bearbetning av jobb med aktiverad Batch-tjänsten. Du kan också ange hello möjligheten för kunder-tooupload och spåra sina egna program i din tjänst.

## <a name="next-steps"></a>Nästa steg
* Hej [Batch REST API] [ api_rest] ger support toowork programpaket. Se exempelvis hello [applicationPackageReferences] [ rest_add_pool_with_packages] element i [Lägg till ett poolen tooan] [ rest_add_pool] information om hur toospecify paket tooinstall med hjälp av hello REST API. Se [program] [ rest_applications] mer information om hur tooobtain programinformation med hjälp av hello Batch REST API.
* Lär dig hur tooprogrammatically [hantera Azure Batch-konton och kvoter med Batch Management .NET](batch-management-dotnet.md). Hej [Batch Management .NET][api_net_mgmt] biblioteket kan aktivera konto skapas eller tas bort för Batch-programmet eller tjänsten.

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/en-us/rest/api/batchservice/
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Översiktsdiagram för programmets paket"
[2]: ./media/batch-application-packages/app_pkg_02.png "Program-panelen i Azure-portalen"
[3]: ./media/batch-application-packages/app_pkg_03.png "Program-bladet i Azure-portalen"
[4]: ./media/batch-application-packages/app_pkg_04.png "Programmet informationsbladet i Azure-portalen"
[5]: ./media/batch-application-packages/app_pkg_05.png "Nya program bladet i Azure-portalen"
[7]: ./media/batch-application-packages/app_pkg_07.png "Uppdatera eller ta bort paket listrutan i Azure-portalen"
[8]: ./media/batch-application-packages/app_pkg_08.png "Nya bladet med paketet i Azure-portalen"
[9]: ./media/batch-application-packages/app_pkg_09.png "Ingen varning har länkade Storage-konto"
[10]: ./media/batch-application-packages/app_pkg_10.png "Välj lagring konto-bladet i Azure-portalen"
[11]: ./media/batch-application-packages/app_pkg_11.png "Uppdatera paketet bladet i Azure-portalen"
[12]: ./media/batch-application-packages/app_pkg_12.png "Ta bort paketet bekräftelsedialogruta i Azure-portalen"
