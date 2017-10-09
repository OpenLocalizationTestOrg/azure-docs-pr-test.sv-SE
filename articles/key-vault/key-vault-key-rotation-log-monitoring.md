---
title: aaaSet in Azure Key Vault slutpunkt till slutpunkt viktiga rotation och granskning | Microsoft Docs
description: "Använd den här hur tootoohelp du att börja arbeta med viktiga rotation och övervakning nyckelvalv loggar."
services: key-vault
documentationcenter: 
author: swgriffith
manager: mbaldwin
tags: 
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: jodehavi;stgriffi
ms.openlocfilehash: e0c393873077e3b91adc9fa7f39128bc1b6abe26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Konfigurera Azure Key Vault med nyckelgranskning och -rotation från slutpunkt till slutpunkt
## <a name="introduction"></a>Introduktion
När du har skapat nyckelvalvet, kommer du att kunna toostart använder den valvet toostore dina nycklar och hemligheter. Dina program inte längre behöver toopersist dina nycklar och hemligheter, men i stället begär dem från hello nyckelvalv efter behov. Detta ger dig tooupdate nycklar och hemligheter utan att påverka hello beteendet för programmet, vilket öppnar ett brett möjligheter runt din nyckel och hemliga hantering.

Den här artikeln beskriver hur ett exempel på hur Azure Key Vault toostore en hemlighet i det här fallet en nyckel för Azure Storage-konto som används av ett program. Här visas också implementering av en schemalagd rotation av den lagringskontonyckel. Slutligen går den igenom en demonstration av hur toomonitor hello nyckelvalv granskningsloggar och generera aviseringar när oväntat begäranden som görs.

> [!NOTE]
> Den här kursen är inte avsedda tooexplain i detalj hello installationen av nyckelvalvet. Den här informationen finns i [Komma igång med Azure Key Vault](key-vault-get-started.md). Plattformsoberoende kommandoradsgränssnittet instruktioner finns i [hantera Key Vault med hjälp av CLI](key-vault-manage-with-cli2.md).
>
>

## <a name="set-up-key-vault"></a>Konfigurera Key Vault
tooenable ett program tooretrieve en hemlighet från Nyckelvalvet, måste du först skapa hello hemlighet och överföra den tooyour valvet. Detta kan åstadkommas genom att starta en Azure PowerShell-session och logga in tooyour Azure-konto med hello följande kommando:

```powershell
Login-AzureRmAccount
```

Ange ditt användarnamn för Azure-konto och lösenord i hello popup-webbläsarfönstret. PowerShell får alla hello-prenumerationer som är associerade med det här kontot. PowerShell använder hello förstnämnda som standard.

Om du har flera prenumerationer kanske toospecify hello ett som har använt toocreate nyckelvalvet. Ange hello följande toosee hello prenumerationer för ditt konto:

```powershell
Get-AzureRmSubscription
```

toospecify hello prenumeration som är kopplad till hello nyckelvalv som du loggar, ange:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

Eftersom den här artikeln visar lagra en lagringskontonyckel som en hemlighet, måste du hämta den lagringskontonyckel.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Efter hämtning av ditt hemliga (i det här fallet din lagringskontonyckel), måste du konvertera den tooa säker strängen och sedan skapa en hemlighet som har värdet i nyckelvalvet.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Hämta sedan hello URI för hello hemlighet som du skapade. Detta används i ett senare steg när du anropar hello nyckelvalv tooretrieve ditt hemliga. Kör följande PowerShell-kommando hello och anteckna hello ID-värde som är hello hemlighet URI:

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-hello-application"></a>Ställ in hello program
Nu när du har en hemlighet som lagras, kan du använda koden tooretrieve och använda den. Det finns några steg krävs tooachieve detta. hello är första och viktigaste steget registrera ditt program med Azure Active Directory och sedan uppmanar Key Vault information om programmet så att den kan tillåta förfrågningar från ditt program.

> [!NOTE]
> Programmet måste skapas på hello samma Azure Active Directory-klient som ditt nyckelvalv.
>
>

Öppna fliken för hello-program för Azure Active Directory.

![Öppna program i Azure Active Directory](./media/keyvault-keyrotation/AzureAD_Header.png)

Välj **lägga till** tooadd ett program tooyour Azure Active Directory.

![Välj Lägg till](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Lämna hello programtyp som **WEB APPLICATION och/eller webb-API** och namnge ditt program.

![Namnet hello program](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Ge programmet en **SIGN-ON-URL** och en **APP-ID URI**. Dessa kan vara vad du vill använda för den här demon och de kan ändras senare om det behövs.

![Ange nödvändiga URL: er](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

När programmet hello läggs tooAzure Active Directory, kommer du till sidan för hello-programmet. Klicka på hello **konfigurera** fliken och leta sedan reda på och kopiera hello **klient-ID** värde. Anteckna hello klient-ID för senare steg.

Generera en nyckel för tillämpningsprogrammet bredvid, så den kan interagera med Azure Active Directory. Du kan skapa det under hello **nycklar** avsnitt i hello **Configuration** fliken. Anteckna hello nya nyckeln från Azure Active Directory-program för användning i ett senare steg.

![Azure Active Directory App nycklar](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Innan du upprättar varje anrop från ditt program i hello nyckelvalv måste du se hello nyckelvalv om programmet och dess behörighet. hello följande kommando tar hello valvnamnet och hello klient-ID från din app i Azure Active Directory och ger **hämta** åtkomst tooyour nyckelvalv för hello program.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Nu är du redo toostart bygga program-anrop. Du måste installera nödvändiga toointeract för hello NuGet-paket med Azure Key Vault och Azure Active Directory i ditt program. Ange hello följande kommandon från hello Visual Studio Package Manager-konsolen. Hello aktuella versionen av hello Azure Active Directory-paketet är 3.10.305231913, så att du kanske vill tooconfirm hello senaste versionen och uppdateras vid hello skrivning i den här artikeln.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Skapa en klass toohold hello metod för din Azure Active Directory-autentisering i din programkod. I det här exemplet är klassen kallas **verktyg för webbplatsuppgradering**. Lägg till hello följande med instruktionen:

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Lägg till följande metod tooretrieve hello JWT-token från Azure Active Directory hello. Du kanske vill toomove hello hårdkodade strängvärden till webb- eller konfigurationen för underhålla.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed tooobtain hello JWT token");

    return result.AccessToken;
}
```

Lägg till hello nödvändig kod toocall Key Vault och hämta det hemliga värdet. Först måste du lägga till hello följande med instruktionen:

```csharp
using Microsoft.Azure.KeyVault;
```

Lägg till hello metoden anrop tooinvoke Key Vault och hämta ditt hemliga. I den här metoden kan ange du hello hemlighet URI som du sparade i föregående steg. Observera hello användning av hello **GetToken** metod från hello **verktyg för webbplatsuppgradering** klassen som skapade tidigare.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

När du kör programmet bör du nu autentisera tooAzure Active Directory och sedan hämta din hemligt värde från Azure Key Vault.

## <a name="key-rotation-using-azure-automation"></a>Viktiga rotation med hjälp av Azure Automation
Det finns olika alternativ för att implementera en rotation strategi för värden som du lagrar som Azure Key Vault hemligheter. Hemligheter kan roteras som en del av en manuell process, de kan roteras programmässigt med hjälp av API-anrop eller kan roteras med ett Automation-skript. Hello enligt den här artikeln, du kan använda Azure PowerShell tillsammans med Azure Automation toochange snabbtangent Azure Storage-konto. Sedan uppdaterar du en hemlighet i nyckelvalvet med den nya nyckeln.

tooallow Azure Automation tooset hemliga värden i nyckelvalvet, måste du hämta hello klient-ID för hello anslutningen med namnet AzureRunAsConnection som skapades när du har etablerat Azure Automation-instans. Du hittar det här ID genom att välja **tillgångar** från Azure Automation-instans. Därifrån kan du välja **anslutningar** och välj sedan hello **AzureRunAsConnection** tjänsten principen. Anteckna hello **program-ID**.

![Azure Automation-klient-ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

I **tillgångar**, Välj **moduler**. Från **moduler**väljer **galleriet**, och sök sedan efter och **importera** uppdaterade versioner av vart och ett av följande moduler hello:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> Vid hello skrivning av den här artikeln, hello tidigare noterats moduler som krävs för toobe uppdateras bara efter Hej följande skript. Om du tycker att ditt automation-jobb inte fungerar kan du bekräfta att du har importerat alla moduler som krävs och deras beroenden.
>
>

När du har hämtat hello program-ID för Azure Automation-anslutning, måste du se nyckelvalvet som det här programmet har åtkomst tooupdate hemligheter i ditt valv. Detta kan åstadkommas med hello följande PowerShell-kommando:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Välj därefter **Runbooks** under din Azure Automation-instans och välj sedan **lägga till en Runbook**. Välj **Snabbregistrering**. Din runbook och välj **PowerShell** som hello runbooktyp. Du har hello alternativet tooadd en beskrivning. Klicka slutligen på **skapa**.

![Skapa runbook](./media/keyvault-keyrotation/Create_Runbook.png)

Klistra in följande PowerShell-skript i hello editor-fönstret för din nya runbook hello:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get hello connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in tooAzure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set hello following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for hello storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Hello editor-fönstret, Välj **Test fönstret** tootest skriptet. När hello skriptet körs utan fel, kan du välja **publicera**, och du kan koppla ett schema för hello runbook tillbaka i hello runbook configuration rutan.

## <a name="key-vault-auditing-pipeline"></a>Key Vault granskning pipeline
När du ställer in ett nyckelvalv som du kan aktivera granskningsloggar toocollect på toohello nyckelvalv för av åtkomstbegäranden. Dessa loggar lagras i ett avsedda Azure Storage-konto och kan hämtas, övervakas och analyseras. hello använder följande scenario Azure functions, Azure logikappar och nyckelvalv granska loggarna toocreate pipeline-toosend ett e-postmeddelande när en app som matchar hello app-ID för hello webbapp hämtar hemligheter från hello-valvet.

Först måste du aktivera loggning på nyckelvalvet. Detta kan göras via hello följande PowerShell-kommandon (fullständig information kan visas vid [key vault loggning](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

När den är aktiverad, granskningsloggar start samla i hello avses storage-konto. Dessa loggar innehålla händelser om hur och när ditt nyckelvalv kan nås och av vem.

> [!NOTE]
> Du kan komma åt din loggningsinformation 10 minuter efter hello nyckelvalv igen. Det kommer vanligtvis vara snabbare än så.
>
>

hello nästa steg är för[skapa en Azure Service Bus-kö](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Detta är där nyckelvalv granskningsloggar push. När hello granskningsloggmeddelanden hello kön, hello logikapp hämtar dem och fungerar på dem.. Skapa en service bus med hello följande steg:

1. Skapa ett namnområde för Service Bus (om du redan har en som du vill toouse för det här hoppa över tooStep 2).
2. Bläddra toohello service bus i hello Azure-portalen och väljer hello namnområde som du vill toocreate hello kön i.
3. Välj **ny** och välj **Service Bus > kön** och ange information om hello krävs.
4. Markerar hello anslutningsinformationen för Service Bus genom att välja hello namnområde **anslutningsinformationen**. Du behöver den här informationen för hello nästa avsnitt.

Nästa [skapa en Azure-funktion](../azure-functions/functions-create-first-azure-function.md) toopoll nyckelvalvet i hello storage-konto och hämta nya händelser. Det här är en funktion som utlöses enligt ett schema.

Välj toocreate en Azure-funktion **New > Funktionsapp** i hello Azure-portalen. Du kan använda en befintlig värd plan eller skapa en ny vid skapandet. Du kan också välja dynamisk värd. Mer information om funktionen som värd för alternativ finns på [hur tooscale Azure Functions](../azure-functions/functions-scale.md).

När hello Azure-funktion skapas navigerar tooit och väljer en timer funktionen och C\#. Klicka på **skapa den här funktionen**.

![Azure Functions starta bladet](./media/keyvault-keyrotation/Azure_Functions_Start.png)

På hello **utveckla** fliken ersätter hello run.csx kod med hello följande:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging;
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log)
{
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting toonow.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required tooorder by time as they may not be in hello file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending tooServiceBus, use hello payloadStream and set keeporiginal tootrue
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate hello shared access signature on hello container, setting hello constraints directly on hello signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```


> [!NOTE]
> Se till att tooreplace hello variabler i föregående kod toopoint tooyour storage-konto där hello nyckelvalv loggarna skrivs hello hello service bus som du skapade tidigare, och hello angiven sökväg toohello nyckelvalv lagring loggar.
>
>

hello funktionen hämtar hello senaste loggfilen från hello lagringskonto där hello nyckelvalv loggarna skrivs, grabs hello senaste händelser från filen, och skickar dem tooa Service Bus-kö. Eftersom en enda fil kan ha flera händelser, bör du skapa en sync.txt-fil som hello funktionen kontrollerar också toodetermine hello tidsstämpel hello senaste händelse som hämtades. Detta säkerställer att du inte push hello flera gånger för samma händelse. Sync.txt filen innehåller en tidsstämpel för senaste påträffade hello-händelse. hello loggar när har lästs in, har toobe sorteras utifrån hello tidsstämpel tooensure de sorteras på rätt sätt.

För den här funktionen referera vi några ytterligare bibliotek som inte är tillgängliga för out of box hello i Azure Functions. tooinclude, behöver vi Azure Functions toopull dem med hjälp av NuGet. Välj hello **visa filer** alternativet.

![Visa filer](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

Och Lägg till en fil som heter project.json med följande innehåll:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Vid **spara**, Azure Functions hämtar hello krävs binärfiler.

Växla toohello **integrera** fliken och ge hello timer parametern ett beskrivande namn toouse i hello-funktion. Hello föregående kod, den förväntar sig hello timer toobe kallas *myTimer*. Ange en [CRON-uttryck](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) enligt följande: 0 \* \* \* \* \* för hello timer som gör att hello funktionen toorun en gång i minuten.

Hej på samma **integrera** lägger du till indata av typen hello **Azure Blob Storage**. Detta kommer att peka toohello sync.txt-fil som innehåller hello tidsstämpel för hello sista händelsen granskats av hello-funktionen. Det här är tillgängliga i hello-funktion av hello parameternamnet. Hello föregående kod, hello Azure Blob Storage indata förväntar sig hello parametern name toobe *inputBlob*. Välj hello storage-konto där hello sync.txt filen ska placeras (det kan vara hello samma eller ett annat lagringskonto). Ange i hello sökvägsfältet hello sökväg där filen hello bor i hello formatet {container-name}/path/to/sync.txt.

Lägga till utdata av hello typen *Azure Blob Storage* utdata. Detta kommer att peka toohello sync.txt filen som du har definierat i hello indata. Detta används av hello funktionen toowrite hello tidsstämpel för hello sista händelsen tittar på. hello föregående kod förväntar sig den här parametern toobe kallas *outputBlob*.

Hello funktion är nu klar. Se till att tooswitch tillbaka toohello **utveckla** fliken och spara hello kod. Kontrollera hello utdatafönstret för några kompileringsfel och korrigera dem i enlighet med detta. Om hello kod kompileras hello koden ska nu kontrollera hello nyckelvalv loggar varje minut och trycka på alla nya händelser till hello definierats Service Bus-kö. Du bör se loggningsinformation skriva ut toohello fönstret varje gång hello funktionen utlöses.

### <a name="azure-logic-app"></a>Azure logikapp
Nu måste du skapa ett Azure logikappen som hämtar hello händelser att hello funktion gör att toohello Service Bus-kö Parsar hello innehåll och skickar ett e-post baserat på ett villkor som matchas.

[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md) genom att gå för**New > Logikapp**.

När du har skapat hello logikapp navigerar tooit och väljer **redigera**. Hello logik app Editor väljer **Service Bus-kö** och ange dina autentiseringsuppgifter för Service Bus-tooconnect den toohello kön.

![Logik för Azure App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Välj nästa **Lägg till ett villkor**. Växla toohello Avancerad redigerare i hello villkor och ange hello följande kod, ersätta APP_ID med hello faktiska APP_ID av ditt webbprogram:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Det här uttrycket returnerar i stort sett **FALSKT** om hello *appid* från hello inkommande händelse (vilket är hello hello Service Bus meddelandets text) är inte hello *appid* av hello App.

Nu ska du skapa en åtgärd under **om Nej, gör ingenting**.

![Azure Logikapp välja åtgärd](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Hello-åtgärd, Välj **Office 365 – skicka e-post**. Fyll i hello fält toocreate toosend en e-post när hello definierade villkoret returnerar **FALSKT**. Om du inte har Office 365 kan du titta på alternativ tooachieve hello samma resultat.

Du har nu en end tooend pipeline som söker efter nya nyckelvalv granskningsloggar en gång i minuten. Den skickar nya loggar påträffas tooa service bus-kö. Hej logikapp utlöses när ett nytt meddelande hamnar i hello kön. Om hello *appid* inom hello matchar inte händelse hello app-ID för hello anropar programmet, skickas ett e-postmeddelande.
