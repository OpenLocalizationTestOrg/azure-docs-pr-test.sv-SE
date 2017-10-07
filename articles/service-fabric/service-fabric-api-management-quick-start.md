---
title: aaaAzure Service Fabric med API Management Snabbstart | Microsoft Docs
description: "Den här guiden visar hur tooquickly Kom igång med Azure API Management och Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: f76f3f39a92f89892d6a02ecaab1ec3d343fe2a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a>Service Fabric med Azure API Management-Snabbstart

Den här guiden visar hur tooset upp Azure API Management med Service Fabric och konfigurera din första API åtgärden toosend trafik tooback tjänster i Service Fabric. toolearn mer om Azure API Management scenarier med Service Fabric finns hello [översikt](service-fabric-api-management-overview.md) artikel. 

## <a name="deploy-api-management-and-service-fabric-tooazure"></a>Distribuera tooAzure API Management och Service Fabric

hello första steget är toodeploy API-hantering och ett Service Fabric-kluster tooAzure i ett delat virtuella nätverk. Detta tillåter API Management toocommunicate direkt med Service Fabric så att den kan utföra identifiering, service partition upplösning och vidarebefordra trafik direkt tooany backend-tjänsten i Service Fabric.

### <a name="topology"></a>topologi

Den här guiden distribuerar hello följande topologi tooAzure där API-hantering och Service Fabric finns i undernät för hello samma virtuella nätverk:

 ![Bildrubrik][sf-apim-topology-overview]

tooget snabbt igång har Resource Manager-mallar angetts för varje steg i distributionen:

 - Nätverkstopologi:
    - [Network.JSON][network-arm]
    - [Network.parameters.JSON][network-parameters-arm]
 - Service Fabric-kluster:
    - [cluster.JSON][cluster-arm]
    - [cluster.parameters.JSON][cluster-parameters-arm]
 - API-hantering:
    - [APIM.JSON][apim-arm]
    - [APIM.parameters.JSON][apim-parameters-arm]

### <a name="sign-in-tooazure-and-select-your-subscription"></a>Logga in tooAzure och välja din prenumeration

Den här guiden använder [Azure PowerShell][azure-powershell]. När du startar en ny PowerShell-session, logga in tooyour Azure-konto och välja din prenumeration innan du kan köra kommandon för Azure.
 
Logga in tooyour Azure-konto:

```powershell
PS > Login-AzureRmAccount
```

Välj din prenumeration:

```powershell
PS > Get-AzureRmSubscription
PS > Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en ny resursgrupp för din distribution. Ange ett namn och en plats.

```powershell
PS > New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-hello-network-topology"></a>Distribuera hello nätverkstopologi

hello första steget är tooset in hello nätverks topologi toowhich API Management och hello Service Fabric-klustret kommer att distribueras. Hej [network.json] [ network-arm] Resource Manager-mall som är konfigurerade toocreate ett virtuellt nätverk (VNET) med två undernät och två Nätverkssäkerhetsgrupp grupper (NSG). 

Hej [network.parameters.json] [ network-parameters-arm] parameterfilen innehåller hello namnen på hello-undernät och NSG: er som API Management och Service Fabric ska distribueras till. Den här guiden behöver inte hello parametervärden toobe ändras. hello API-hantering och Service Fabric Resource Manager-mallar används dessa värden, så om de ändras här måste du ändra dem i hello andra Resource Manager-mallar i enlighet med detta. 

 1. Hämta hello följande Resource Manager-mall och parametrar:

    - [Network.JSON][network-arm]
    - [Network.parameters.JSON][network-parameters-arm]

 2. Använd följande PowerShell-kommandot toodeploy hello Resource Manager mallen och parametern filer för nätverksinstallation hello hello:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-hello-service-fabric-cluster"></a>Distribuera hello Service Fabric-kluster

När hello nätverksresurser är klar med att distribuera, hello nästa steg är toodeploy ett Service Fabric-kluster toohello VNET i undernät hello och NSG utsetts för hello Service Fabric-klustret. Den här självstudien är hello Service Fabric Resource Manager-mall förkonfigurerade toouse hello namnen på hello VNET och undernät NSG som du skapade i föregående steg i hello. 

Resource Manager-mall för hello Service Fabric-klustret är konfigurerade toocreate säker kluster med Certifikatsäkerhet. hello certifikatet är används toosecure nod till nod kommunikation för klustret och toomanage användaren åtkomst tooyour Service Fabric-klustret. API Management använder det här certifikatet tooaccess hello Fabric namngivningstjänst för identifiering av tjänst.

Det här steget kräver att ett certifikat i Key Vault för Klustersäkerhet. Mer information om hur du skapar en säker kluster med Key Vault finns [den här guiden om hur du skapar ett kluster i Azure med hjälp av hanteraren för filserverresurser](service-fabric-cluster-creation-via-arm.md)

> [!NOTE]
> Du kan lägga till Azure Active Directory-autentisering i tillägg toohello certifikatet som används för åtkomst till klustret. Azure Active Directory är hello rekommenderat sätt toomanage användaren åtkomst tooyour Service Fabric-kluster, men är inte nödvändigt toocomplete den här kursen. Ett certifikat krävs oavsett hur för klustret nod till nod säkerhet och Azure API Management-autentisering, vilket inte stöder för närvarande autentisera med Azure Active Directory för en Service Fabric-serverdel.

 1. Hämta hello följande Resource Manager-mall och parametrar:
 
    - [cluster.JSON][cluster-arm]
    - [cluster.parameters.JSON][cluster-parameters-arm]

 2. Fyll i hello tom parametrar i hello `cluster.parameters.json` -filen för din distribution, inklusive hello [Key Vault information](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) för kluster-certifikatet.

 3. Använd följande PowerShell-kommandot toodeploy hello Resource Manager mallen och parametern filer toocreate hello Service Fabric-klustret hello:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a>Distribuera API Management

Slutligen kan distribuera API Management toohello VNET i undernät hello och NSG utsetts för API-hantering. Du behöver inte toowait för hello Service Fabric-kluster distribution toofinish innan du distribuerar API-hantering. 

Den här självstudien är hello API Management Resource Manager-mall förkonfigurerade toouse hello namnen på hello VNET och undernät NSG som du skapade i föregående steg i hello. 

 1. Hämta hello följande Resource Manager-mall och parametrar:
 
    - [APIM.JSON][apim-arm]
    - [APIM.parameters.JSON][apim-parameters-arm]

 2. Fyll i hello tom parametrar i hello `apim.parameters.json` för din distribution.

 3. Använd följande PowerShell-kommandot toodeploy hello Resource Manager mallen och parametern filer för API Management hello:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a>Konfigurera API-hantering

När din API-hantering och Service Fabric-klustret har distribuerats, kan du konfigurera en Service Fabric-serverdel i API-hantering. Detta gör att du toocreate för en backend-tjänsten som skickar trafik tooyour Service Fabric-klustret.

### <a name="api-management-security"></a>API Management-säkerhet

tooconfigure hello Service Fabric serverdelen måste du först tooconfigure API Management säkerhetsinställningar. tooconfigure säkerhetsinställningar, gå tooyour API Management-tjänsten i hello Azure-portalen.

#### <a name="enable-hello-api-management-rest-api"></a>Aktivera hello API Management REST API

hello API Management REST API är för närvarande hello endast sätt tooconfigure en serverdelstjänst. hello första steget är tooenable hello API Management REST-API och skydda den.

 1. Välj i hello API Management-tjänsten, **Management API - PREVIEW** under **säkerhet**.
 2. Kontrollera hello **aktivera API Management REST API** kryssrutan.
 3. Observera hello URL för API Management – det här är hello-URL som vi använder senare tooset in hello Service Fabric-serverdelen
 4. Generera en **åtkomsttoken** genom att välja ett förfallodatum och en nyckel, och klicka sedan på hello **generera** knappen hello nedre delen av hello-sidan.
 5. Kopiera hello **åtkomsttoken** och spara den – vi använder informationen i följande steg hello. Observera att detta skiljer sig från hello primärnyckel och sekundärnyckel.

#### <a name="upload-a-service-fabric-client-certificate"></a>Överföra ett certifikat för Service Fabric

API Management måste autentisera med Service Fabric-klustret för identifiering av tjänst använder ett klientcertifikat som har åtkomst tooyour klustret. För enkelhetens skull hello den här självstudiekursen använder samma certifikat som anges när du skapar hello Service Fabric-kluster, vilket som standard kan vara används tooaccess klustret.

 1. Välj i hello API Management-tjänsten, **klientcertifikat - PREVIEW** under **säkerhet**.
 2. Klicka på hello **+ Lägg till** knappen
 2. Välj hello fil för privat nyckel (.pfx) för hello klustret certifikat som du angav när du skapar Service Fabric-kluster, ge det ett namn och ange lösenordet för hello privata nyckeln.

> [!NOTE]
> Den här kursen använder hello samma certifikat för autentisering och kluster nod till nod klientsäkerhet. Du kan använda ett separat klientcertifikat om du har en konfigurerad tooaccess Service Fabric-klustret.

### <a name="configure-hello-backend"></a>Konfigurera hello backend

Nu när API Management-säkerheten är konfigurerad, kan du konfigurera hello Service Fabric-serverdel. För Service Fabric serverdelar är hello Service Fabric-kluster hello backend, i stället för en specifik tjänst för Service Fabric. På så sätt kan en enda princip tooroute toomore än en tjänst i hello kluster.

Det här steget kräver hello åtkomst-token som du tidigare genererade och hello tumavtrycket för ditt kluster certifikat du laddade upp tooAPI Management i hello föregående steg.

> [!NOTE]
> Om du använder ett separat klientcertifikat i hello föregående steg för API-hantering, behöver du hello tumavtrycket för hello klientcertifikat i tillägg toohello klustret tumavtryck för certifikatet i det här steget.

Skicka hello följande HTTP PUT-begäran toohello API Management API-URL som du antecknade tidigare när du aktiverar hello API Management REST API tooconfigure hello Service Fabric backend-tjänst. Du bör se en `HTTP 201 Created` svar när hello kommandot slutförs. Mer information om varje fält finns hello API Management [backend API-referensdokumentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).

HTTP-kommando och URL:
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

Huvuden för begäran:
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

Begärandetexten:
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

Hej **url** här parametern är ett fullständigt tjänstnamn för en tjänst i klustret som alla begäranden dirigeras tooby standard om inget namn anges i en backend-princip. Du kan använda falska tjänstnamn, till exempel ”fabric: / falska/service” om du inte avser toohave en återställningsplats tjänst.

Se toohello API Management [backend API-referensdokumentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) för mer information om varje fält.

#### <a name="example"></a>Exempel

```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
Authorization: SharedAccessSignature 230948023984&Ld93cRGcNU6KZ4uVz7JlfTec4eX43Q9Nu8ndatOgBzs6+f559Pkf3iHX2cSge+r42pn35qGY3TitjrIl13hwcQ==
Content-Type: application/json

{
    "description": "My Service Fabric backend",
    "url": "fabric:/myapp/myservice",
    "protocol": "http",
    "resourceId": "https://your-cluster.westus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": ["https://your-cluster.westus.cloudapp.azure.com:19080"],
            "clientCertificateThumbprint": "57bc463aba3aea3a12a18f36f44154f819f0fe32",
            "serverCertificateThumbprints": ["57bc463aba3aea3a12a18f36f44154f819f0fe32"],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Distribuera en backend-tjänst för Service Fabric

Nu när du har hello Service Fabric konfigurerad som en backend-tooAPI hantering av skapar du backend-principer för dina API: er som skickar trafik tooyour Service Fabric-tjänster. Men du måste först en tjänst som körs i Service Fabric tooaccept begäranden.

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a>Skapa ett Service Fabric-tjänsten med en HTTP-slutpunkt

Den här självstudiekursen skapar vi en grundläggande tillståndslös ASP.NET Core tillförlitlig tjänst med hello standardmallen Web API-projekt. Detta skapar en HTTP-slutpunkt för din tjänst som du ska visa genom Azure API Management:

```
/api/values
```

Börja med [ställa in din utvecklingsmiljö för utveckling av ASP.NET Core](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).

När din utvecklingsmiljö har ställts in, starta Visual Studio som administratör och skapa en ASP.NET Core-tjänst:

 1. Välj Arkiv -> Nytt projekt i Visual Studio.
 2. Välj hello Fabric tjänstprogrammet mall under molntjänster och ger den namnet **”ApiApplication”**.
 3. Välj namn hello projekt och hello ASP.NET Core-tjänstmall **”WebApiService”**.
 4. Välj hello Web API ASP.NET Core 1.1 projektmall.
 5. När du har skapat hello projektet öppnar du `PackageRoot\ServiceManifest.xml` och ta bort hello `Port` attribut från hello slutpunktskonfigurationen för resursen:
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    Detta gör att Service Fabric toospecify en port dynamiskt från hello programmet portintervall, som vi öppnas via hello Nätverkssäkerhetsgruppen i hello klustret Resource Manager-mall, att tillåta trafik tooflow tooit från API-hantering.
 
 6. Tryck på F5 i Visual Studio tooverify hello webb-API finns lokalt. 

    Öppna Service Fabric Explorer detaljnivån tooa specifika instansen av hello ASP.NET Core toosee hello basadress hello-tjänsten lyssnar på. Lägg till `/api/values` toohello basadressen och öppna den i en webbläsare. Detta startar hello Get-metoden på hello ValuesController i hello Web API-mallen. Den returnerar hello Standardsvar som tillhandahålls av hello mall, en JSON-matris som innehåller två strängar:

    ```json
    ["value1", "value2"]`
    ```

    Detta är hello-slutpunkt som du ska visa genom API Management i Azure.

 7. Slutligen distribuera hello programmet tooyour kluster i Azure. [Med Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), högerklicka på projektet hello och välj **publicera**. Ange din klusterslutpunkten (till exempel `mycluster.westus.cloudapp.azure.com:19000`) toodeploy hello programmet tooyour Service Fabric-klustret i Azure.

En ASP.NET Core tillståndslösa tjänsten med namnet `fabric:/ApiApplication/WebApiService` bör körs i Service Fabric-kluster i Azure.

## <a name="create-an-api-operation"></a>Skapa en API-åtgärd

Nu är klar toocreate en åtgärd i API Management hello det externa klienter Använd toocommunicate med ASP.NET Core tillståndslösa tjänsten som körs i hello Service Fabric-klustret.

 1. Logga in toohello Azure-portalen och navigera tooyour API Management service-distributionen.
 2. I hello API Management service-bladet välj **API: er – förhandsgranskning**
 3. Lägg till ett nytt API genom att klicka på hello **tomt API** rutan och fyller i dialogrutan för hello:

     - **Webbtjänstens URL**: för Service Fabric serverdelar används inte den här URL-värdet. Du kan ange ett värde här. Den här kursen använder: `http://servicefabric`.
     - **Namnet**: Ange ett namn för din API. Den här kursen använder `Service Fabric App`.
     - **URL-schema**: Välj HTTP, HTTPS eller båda. Den här kursen använder `both`.
     - **API-URL-Suffix**: Ange ett suffix för vårt API. Den här kursen använder `myapp`.
 
 4. När hello API har skapats klickar du på **+ Lägg till åtgärden** tooadd en frontend-API-åtgärden. Fyll ut hello värden:
    
     - **URL: en**: Välj `GET` och ange en URL-sökväg för hello API. Den här kursen använder `/api/values`.
     
       Hello URL-sökväg anges här skickas hello URL-sökvägen toohello serverdelstjänst Service Fabric. Om du använder hello samma URL-sökvägen här som tjänsten använder i det här fallet `/api/values`, sedan hello åtgärden fungerar utan ytterligare ändringar. Du kan också ange en URL-sökväg här som skiljer sig från hello URL-sökvägen som används av din serverdel Service Fabric-tjänsten, då du kommer också att behovet av toospecify omarbetning av en sökväg i din princip för åtgärden senare.
     - **Visningsnamn**: Ange ett namn för hello API. Den här kursen använder `Values`.

## <a name="configure-a-backend-policy"></a>Konfigurera en backend-princip

hello backend principen kopplar samman allt tillsammans. Det är där du konfigurerar hello backend Service Fabric toowhich tjänstbegäranden dirigeras. Du kan använda den här principen tooany API-åtgärd. Hej [backend-konfiguration för Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) innehåller hello följande begär routning kontroller: 
 - Instansurval av-tjänsten genom att ange ett Service Fabric-tjänstnamn, antingen hårdkodad (till exempel `"fabric:/myapp/myservice"`) eller genereras från hello HTTP-begäran (till exempel `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Partitionen lösning genom att generera en partitionsnyckel med alla Service Fabric-partitioneringsschema.
 - Val av replik för tillståndskänsliga tjänster.
 - Lösning försök villkor som gör att du toospecify hello villkor för återlösa på en plats och skicka en begäran.

För den här självstudiekursen skapar du en backend-princip att vägar direkt begär toohello ASP.NET Core tillståndslösa tjänsten har distribuerats tidigare:

 1. Markera och redigera hello **inkommande principer** för hello `Values` igen genom att klicka på redigeringsikonen hello och sedan välja **kodvy**.
 2. I Redigeraren för hello kod lägger du till en `set-backend-service` princip under inkommande principer som visas här och på hello **spara** knappen:
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
           sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
      </inbound>
      <backend>
        <base/>
      </backend>
      <outbound>
        <base/>
      </outbound>
    </policies>
    ```

En fullständig uppsättning attribut för Service Fabric-backend-principen, finns i toohello [API Management backend-dokumentation](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)

### <a name="add-hello-api-tooa-product"></a>Lägga till hello API tooa produkt. 

Innan du kan anropa hello API, måste den läggas tooa produkten där kan du bevilja åtkomst toousers. 

 1. Välj i hello API Management-tjänsten, **produkter - PREVIEW**.
 2. Som standard API Management providers två produkter: Start och obegränsad. Välj hello obegränsade produkten.
 3. Välj API: er.
 4. Klicka på hello **+ Lägg till** knappen.
 5. Välj hello `Service Fabric App` API du skapade i föregående steg i hello och klicka på hello **Välj** knappen.

### <a name="test-it"></a>testa den

Nu kan du försöka skicka en begäran tooyour backend-tjänst i Service Fabric via API Management direkt från hello Azure-portalen.

 1. Välj i hello API Management-tjänsten, **API - PREVIEW**.
 2. I hello `Service Fabric App` API som du skapade i föregående steg för hello, Välj hello **Test** fliken.
 3. Klicka på hello **skicka** knappen toosend en test begäran toohello backend-tjänst.

## <a name="next-steps"></a>Nästa steg

Du bör nu ha en grundläggande installation med Service Fabric och API-hantering.

Den här kursen använder grundläggande certifikatbaserad användarautentisering för din tooget för Service Fabric-klustret som du komma igång snabbt. Mer avancerade användarautentisering för Service Fabric-kluster är bättre med [Azure Active Directory-autentisering](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication). 

Nästa [skapa och konfigurera avancerad produktinställningarna i Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) tooprepare ditt program för verkliga världen trafik.

<!-- links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json


<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-api-management-quickstart/sf-apim-topology-overview.png
