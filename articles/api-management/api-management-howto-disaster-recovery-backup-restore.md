---
title: "aaaImplement disaster recovery med säkerhetskopiering och återställning i Azure API Management | Microsoft Docs"
description: "Lär dig hur toouse säkerhetskopierar och återställer tooperform katastrofåterställning i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f10be3c-f796-4a6c-bacd-7931b6aa82af
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 058bfb579e3a3f51fb1dac8ea37eb4fdbc83a4ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimplement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Hur tooimplement disaster recovery med tjänsten säkerhetskopiering och återställning på Azure API Management
Genom att välja toopublish och hantera dina API: er via Azure API Management du dra nytta av många fel feltolerans och infrastruktur funktioner som du annars skulle ha toodesign, implementera och hantera. hello Azure-plattformen minskar en stor del av fel på en bråkdel av hello kostnad.

toorecover från tillgänglighetsproblem som påverkar hello region där din API Management-tjänsten är värd för du ska vara klar tooreconstitute tjänsten i en annan region när som helst. Beroende på dina mål för tillgänglighet och mål för återställning kan du vill tooreserve säkerhetskopieringstjänsten i en eller flera regioner och försök toomaintain sina konfigurationer och innehåll som är synkroniserade med hello aktiva tjänsten. hello service säkerhetskopiering och återställning funktionen ger hello nödvändiga byggblock för att implementera din strategi för katastrofåterställning.

Den här guiden visar hur du begär tooauthenticate Azure Resource Manager och hur toobackup och återställa din API Management-tjänstinstanser.

> [!NOTE]
> hello-processen för att säkerhetskopiera och återställa en API Management service-instans för katastrofåterställning kan också användas för att replikera API Management-tjänstinstanser för scenarier, till exempel Förproduktion.
>
> Observera att varje säkerhetskopiering upphör att gälla efter 30 dagar. Om du försöker toorestore en säkerhetskopiering när hello 30-dagars giltighetstid har upphört att gälla, misslyckas hello återställning med en `Cannot restore: backup expired` meddelande.
>
>

## <a name="authenticating-azure-resource-manager-requests"></a>Autentiserande Azure Resource Manager-begäranden
> [!IMPORTANT]
> hello REST API för säkerhetskopiering och återställning använder Azure Resource Manager och har en annan autentiseringsmetod än hello REST API: er för att hantera API Management-enheter. hello steg i det här avsnittet beskriver hur tooauthenticate Azure Resource Manager-begäranden. Mer information finns i [autentisera Azure Resource Manager begär](http://msdn.microsoft.com/library/azure/dn790557.aspx).
>
>

Alla hello uppgifter som du gör på resurser med hjälp av hello Azure Resource Manager måste autentiseras med Azure Active Directory med hello följande steg.

* Lägg till ett program toohello Azure Active Directory-klient.
* Ange behörigheter för hello-program som du har lagt till.
* Hämta hello token för att autentisera begäranden tooAzure Resource Manager.

hello första steget är toocreate ett Azure Active Directory-program. Logga in på hello [klassiska Azure-portalen](http://manage.windowsazure.com/) med hello-prenumeration som innehåller din API Management-tjänsten för instansen och navigera toohello **program** för din standard-Azure Active Directory.

> [!NOTE]
> Om hello Azure Active Directory-standardkatalog inte är synliga tooyour konto krävs kontakta Hej administratör hello Azure-prenumeration toogrant hello behörigheter tooyour konto.

![Skapa Azure Active Directory-program][api-management-add-aad-application]

Klicka på **Lägg till**, **Lägg till ett program som min organisation utvecklar**, och välj **Native client-program**. Ange ett beskrivande namn och klicka på nästa hello-pilen. Ange en platshållare-URL som `http://resources` för hello **omdirigerings-URI**eftersom det är ett obligatoriskt fält, men hello värdet används inte senare. Klicka på hello kryssrutan toosave hello program.

När du har sparat hello program klickar du på **konfigurera**, bläddra nedåt toohello **behörigheter tooother program** avsnittet och klicka på **lägga till program**.

![Lägg till behörigheter][api-management-aad-permissions-add]

Välj **Windows** **Azure Service Management API** och på hello kryssrutan tooadd hello program.

![Lägg till behörigheter][api-management-aad-permissions]

Klicka på **delegerade behörigheter** bredvid hello nyligen tillagda **Windows** **Azure Service Management API** programmet hello kryssrutan för **åtkomst till Azure Service Management (förhandsversion)**, och klicka på **spara**.

![Lägg till behörigheter][api-management-aad-delegated-permissions]

Tidigare tooinvoking hello API: er som genererar hello säkerhetskopiering och återställer den, är det nödvändigt tooget en token. hello följande exempel används hello [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget-paketet tooretrieve hello token.

```c#
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

            if (result == null) {
                throw new InvalidOperationException("Failed tooobtain hello JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Ersätt `{tentand id}`, `{application id}`, och `{redirect uri}` med hello följa anvisningar.

Ersätt `{tenant id}` med hello klient-id för hello Azure Active Directory-program som du nyss skapade. Du kan komma åt hello-id genom att klicka på **visa slutpunkter**.

![Slutpunkter][api-management-aad-default-directory]

![Slutpunkter][api-management-endpoint]

Ersätt `{application id}` och `{redirect uri}` med hello **klient-Id** och hello URL från hello **omdirigerings-URI: er** avsnittet från Azure Active Directory-programmets **konfigurera**  fliken.

![Resurser][api-management-aad-resources]

När hello värden har angetts kan ska hello kodexempel returnera en token liknande toohello som följande exempel.

![Token][api-management-arm-token]

Innan du anropar hello säkerhetskopiering och återställning som beskrivs i följande avsnitt hello, ange hello auktorisering begärandehuvudet för REST-anrop.

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <a name="step1"></a>Säkerhetskopiera en API Management-tjänst
tooback upp en API Management-tjänsten problemet hello efter HTTP-begäran:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Där:

* `subscriptionId`-id för hello prenumeration som innehåller hello API Management-tjänsten som du försöker toobackup
* `resourceGroupName`-en sträng i hello form av 'Api - Default-{tjänsten region}' där `service-region` identifierar hello Azure-region där hello API Management-tjänsten som du försöker toobackup finns, t.ex.`North-Central-US`
* `serviceName`-hello namnet på hello API Management-tjänsten som du gör en säkerhetskopia av anges vid hello tidpunkt då skapades
* `api-version`-Ersätt med`2014-02-14`

I hello brödtext hello begäran, ange hello målnamn Azure storage-konto, åtkomstnyckel, blob behållarens namn och namn på säkerhetskopia:

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Värdet för hello hello `Content-Type` förfrågningshuvudet för`application/json`.

Säkerhetskopiering är en tidskrävande process som kan ta flera minuter toocomplete.  Om hello begäran lyckades och hello säkerhetskopieringsprocessen initierades får du en `202 Accepted` Svarets statuskod med en `Location` huvud.  Kontrollera 'GET-begäranden toohello URL: en i hello `Location` huvud toofind ut hello status för hello-åtgärd. Medan hello säkerhetskopiering pågår fortsätter tooreceive statuskod 202 accepteras. En svarskod `200 OK` visar slutförd hello säkerhetskopieringen.

Observera följande begränsningar när du gör en begäran om säkerhetskopiering hello.

* **Behållaren** anges i begärandetexten för hello **måste finnas**.
* När säkerhetskopiering pågår du **bör inte några hanteringsåtgärder för service** , till exempel SKU uppgradering eller nedgradering domän namnbytet osv.
* Återställning av en **säkerhetskopiering garanteras endast för 30 dagar** sedan Hej då skapades.
* **Användningsdata** används för att skapa rapporter för analytics **ingår inte** i hello säkerhetskopiering. Använd [Azure API Management REST API] [ Azure API Management REST API] tooperiodically hämta analytics rapporter förvaras.
* hello frekvens med vilken du säkerhetskopiera tjänsten kommer att påverka dina mål för återställningspunkt. toominimize det rekommenderar vi implementera regelbundna säkerhetskopieringar samt utföra säkerhetskopieringar på begäran när du har gjort viktiga ändringar tooyour API Management-tjänsten.
* **Ändringar** gjorts toohello tjänstkonfiguration (t.ex. API: er, principer, developer portal utseende) vid säkerhetskopiering pågår **kan inte ingå i hello säkerhetskopiering och därför går förlorad**.

## <a name="step2"></a>Återställa en API Management-tjänst
toorestore en API Management-tjänst från en tidigare skapad säkerhetskopia att Hej efter HTTP-begäran:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Där:

* `subscriptionId`-id för hello prenumeration som innehåller hello API Management-tjänsten som du återställer en säkerhetskopia till
* `resourceGroupName`-en sträng i hello form av 'Api - Default-{tjänsten region}' där `service-region` identifierar hello Azure-region där hello du återställer en säkerhetskopia i API Management-tjänsten är värd för, t.ex.`North-Central-US`
* `serviceName`-hello namnet på hello API Management-tjänsten som återställs till anges vid hello tidpunkt då skapades
* `api-version`-Ersätt med`2014-02-14`

Hello brödtexten i begäran hello, ange hello Säkerhetskopians plats, d.v.s. kontonamn för Azure storage, åtkomstnyckel, blob behållarens namn och namn på säkerhetskopia:

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Värdet för hello hello `Content-Type` förfrågningshuvudet för`application/json`.

Återställningen är en långvarig åtgärd kan ta upp too30 eller flera minuter toocomplete.  Om hello begäran lyckades och hello återställningsprocessen initierades får du en `202 Accepted` Svarets statuskod med en `Location` huvud.  Kontrollera 'GET-begäranden toohello URL: en i hello `Location` huvud toofind ut hello status för hello-åtgärd. När hello återställning pågår fortsätter tooreceive '202 godkända-statuskod. En svarskod `200 OK` visar slutförd hello återställningen igen.

> [!IMPORTANT]
> **hello SKU** tjänsternas hello återställs till **måste matcha** hello SKU hello säkerhetskopierade tjänsten som återställs.
>
> **Ändringar** gjorts toohello tjänstkonfiguration (t.ex. API: er, principer, developer portal utseende) när återställningen pågår **kan skrivas över**.
>
>

## <a name="next-steps"></a>Nästa steg
Checka ut hello följande Microsoft bloggar för två olika genomgångar av hello säkerhetskopiering/återställning.

* [Replikera Azure API Management-konton](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * Tack tooGisela för sitt bidrag toothis artikeln.
* [Azure API Management: Säkerhetskopiera och återställa konfiguration](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * hello-metoden anges av Stuart matchar inte hello officiella vägledning men det är mycket intressant.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
