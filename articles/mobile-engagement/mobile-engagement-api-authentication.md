---
title: 'aaaAuthenticate med Mobile Engagement REST API: er'
description: 'Beskriver hur tooauthenticate med Azure Mobile Engagement REST API: er'
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 9b54aa5ec3da4bcf55ffe5b7e8d1759095d0c486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Autentisera med Mobile Engagement REST API: er
## <a name="overview"></a>Översikt
Det här dokumentet beskriver hur tooget en giltig AAD-Oauth token tooauthenticate med hello Mobile Engagement REST API: er. 

Det förutsätts att du har en giltig Azure-prenumeration och du har skapat en Mobile Engagement-app med en av våra [Developer självstudier](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Autentisering
Microsoft Azure Active Directory baserat OAuth-token används för autentisering. 

Ordning tooauthentication en API-begäran, måste ett authorization-huvud läggas tooevery begäran som är av hello följande format:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Azure Active Directory-token upphör gälla inom en timme.
> 
> 

Det finns flera sätt tooget en token. Eftersom hello API: er kallas vanligtvis från en molnbaserad tjänst, vill du toouse en API-nyckel. En API-nyckel i Azure-terminologi kallas Service principal lösenord. hello följande procedur beskriver ett sätt toosetting den upp manuellt.

### <a name="one-time-setup-using-script"></a>Enstaka installationen (med hjälp av skript)
Du bör följa hello uppsättningen anvisningar nedan tooperform hello installationen med hjälp av ett PowerShell-skript som tar hello Minimitiden för installationen, men använder hello mest tillåtna standardvärden. Alternativt kan du kan även följa hello instruktionerna i hello [manuell installation](mobile-engagement-api-authentication-manual.md) för att göra detta från hello Azure-portalen direkt och vill ha bättre konfiguration. 

1. Hämta hello senaste versionen av Azure PowerShell från [här](http://aka.ms/webpi-azps). Mer information om hello Hämtningsinstruktioner kan du se detta [länk](/powershell/azure/overview).  
2. När du har installerat Azure PowerShell Använd hello följande kommandon för att du har hello tooensure **Azure-modulen** installerad:
   
    a. Kontrollera att hello Azure PowerShell-modulen är tillgängliga i hello listan över tillgängliga moduler. 
   
        Get-Module –ListAvailable 
   
    ![Tillgängliga Azure moduler][1]
   
    b. Om du inte hittar hello Azure PowerShell-modulen i hello ovanför listan måste toorun hello följande:
   
        Import-Module Azure 
3. Inloggningen toohello Azure Resource Manager från PowerShell genom att köra hello följande kommando och ange ditt användarnamn och lösenord för kontot: 
   
        Login-AzureRmAccount
4. Om du har flera prenumerationer bör du köra hello följande:
   
    a. Hämta en lista över alla prenumerationer och kopiera hello prenumerations-ID för hello-prenumeration som du vill toouse. Kontrollera att den här prenumerationen är hello samma som har hello Mobile Engagement-App som du kommer toointeract med hello API: er. 
   
        Get-AzureRmSubscription
   
    b. Hello kör följande kommando med hello SubscriptionId tooconfigure hello prenumeration toobe används.
   
        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Kopiera hello text för hello [ny AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) skript tooyour lokala dator och spara den som en PowerShell-cmdlet (t.ex. `APIAuth.ps1`) och kör den `.\APIAuth.ps1`. 
6. hello skript ber dig tooprovide en indata för **huvudkontot**. Ange ett lämpligt namn som du vill toouse toocreate Active Directory-program (t.ex. APIAuth). 
7. När hello skriptet har slutförts visas följande fyra värden som du behöver hello tooauthenticate programmässigt med AD så se till att toocopy dem. 
   
    **TenantId**, **SubscriptionId**, **ApplicationId**, och **hemlighet**.
   
    Du kommer att använda TenantId som `{TENANT_ID}`, ApplicationId som `{CLIENT_ID}` och hemliga som `{CLIENT_SECRET}`.
   
   > [!NOTE]
   > Säkerhetsprinciperna standard kan blockeras från att köra ett PowerShell-skript. I så fall, konfigurera tillfälligt din körning princip tooallow skriptkörningen med hello följande kommando:
   > 
   > Set-ExecutionPolicy RemoteSigned
   > 
   > 
8. Här är hur hello uppsättning PS-cmdlets skulle se ut. 
   
    ![][3]
9. Kontrollera i hello Azure Management portal att ett nytt AD-program har skapats med hello namn du angivna toohello skript kallas **Principalnamnet** under **Visa program som företaget äger**.
   
    ![][4]

#### <a name="steps-tooget-a-valid-token"></a>Steg tooget en giltig token
1. Anropa API för hello med hello följande parametrar och se till att tooreplace hello klient\_, klient-ID\_-ID och klienten\_HEMLIGHET:
   
   * **URL-begäran** som *https://login.microsoftonline.com/ {klient\_ID} / oauth2/token*
   * **HTTP-innhållstyphuvud** som *program/x-www-formuläret-urlencoded*
   * **Brödtext i HTTP-begäran** som *bevilja\_typ = klient\_autentiseringsuppgifter & client_id = {klienten\_ID} & client_secret = {klienten\_HEMLIGHET} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*
     
     hello följande är en exempelbegäran:
     
       POST / {TENANT_ID} / oauth2/token HTTP/1.1 värden: login.microsoftonline.com Content-Type: program/x-www-formuläret-urlencoded grant_type = client_credentials & client_id = {CLIENT_ID} & client_secret = {CLIENT_SECRET} & sammanstä spänningskälla = https % 3A % 2F%2Fmanagement.Core.Windows.NET%2F
     
     Här är ett exempelsvar:
     
       HTTP/1.1 200 OK Content-Type: application/json; charset = utf-8 Content-Length: 1234
     
       {”token_type”: ”ägar”, ”expires_in”: ”3599”, ”expires_on”: ”1445395811”, ”not_before” ”: 144 5391911” ”, resurs”: ”https://management.core.windows.net/”, ”access_token”: {ACCESS_TOKEN}}
     
     Det här exemplet ingår URL-kodning av hello POST-parametrar, `resource` värdet är `https://management.core.windows.net/`. Var noga med att koda tooalso URL `{CLIENT_SECRET}` som den kan innehålla specialtecken.
     
     > [!NOTE]
     > För att testa, kan du använda en HTTP-klientverktyg som [Fiddler](http://www.telerik.com/fiddler) eller [Chrome Postman tillägg](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 
     > 
     > 
2. I varje API-anrop som innehåller nu hello authorization-huvud för begäran:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    Om du får ett 401 returnerade statuskoden, kontrollera svarstexten hello, kan den informera hello token har upphört att gälla. I så fall får en ny token.

## <a name="using-hello-apis"></a>Med hjälp av hello API: er
Nu när du har en giltig token är klar toomake hello API-anrop.

1. I varje API-begäran måste toopass en giltiga token som du fick i hello föregående avsnitt.
2. Du behöver tooplug i vissa parametrar i hello Begärd URI som identifierar ditt program. hello URI-begäran som ser ut som följande hello
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    tooget hello parametrar, klicka på programmets namn och klicka på instrumentpanelen och visas en sida som hello följande med alla hello 3 parametrar.
   
   * **1** `{subscription-id}`
   * **2** `{app-collection}`
   * **3** `{app-resource-name}`
   * **4** din resursgruppens namn kommer toobe **MobileEngagement** om du har skapat en ny. 
     
     ![Mobile Engagement API URI-parametrar][2]

> [!NOTE]
> <br/>
> 
> 1. Ignorera hello API rot-adress som det var för hello tidigare API: er.<br/>
> 2. Om du har skapat hello-app med den klassiska Azure-portalen måste toouse hello programresursen namn som skiljer sig från hello programnamn sig själv. Om du har skapat hello app i hello Azure Portal bör du använda hello Appnamn sig själv (det finns ingen skillnad mellan programmets resursnamn och programnamn för appar som har skapats i hello nya portal).  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



