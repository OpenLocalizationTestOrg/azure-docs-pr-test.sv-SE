---
title: aaaQuickly distribuera ett befintligt app tooan Azure Service Fabric-kluster
description: "Använda en Azure Service Fabric-kluster toohost ett befintligt Node.js-program med Visual Studio."
services: service-fabric
documentationcenter: nodejs
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: adegeo
ms.openlocfilehash: 20a3eb4a9206ba465acf96d0976ba241b07158bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="host-a-nodejs-application-on-azure-service-fabric"></a>Skapa ett Node.js-program i Azure med Node.js

Den här snabbstarten hjälper dig att distribuera ett befintligt program (Node.js i det här exemplet) tooa Service Fabric-kluster körs på Azure.

## <a name="prerequisites"></a>Krav

Du måste [konfigurera utvecklingsmiljön](service-fabric-get-started.md) innan du börjar. Vilket innefattar installerar hello Service Fabric-SDK och Visual Studio 2017 eller 2015.

Du måste också toohave ett befintligt Node.js-program för distribution. Denna snabbstart använder en enkel Node.js-webbplats som du kan hämta [här][download-sample]. Extrahera den här filen tooyour `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` mapp när du skapar hello-projekt i hello nästa steg.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto][create-account] innan du börjar.

## <a name="create-hello-service"></a>Skapa hello-tjänst

Starta Visual Studio som **administratör**.

Skapa ett projekt med `CTRL`+`SHIFT`+`N`

I hello **nytt projekt** dialogrutan Välj **molntjänster > Fabric tjänstprogrammet**.

Namnge programmet hello **MyGuestApp** och tryck på **OK**.

>[!IMPORTANT]
>Node.js kan enkelt dela hello 260 tecken för sökvägar som windows har. Använd en kort sökväg för själva hello-projektet som **c:\code\svc1**.
   
![Dialogrutan Nytt projekt i Visual Studio][new-project]

Du kan skapa alla typer av Service Fabric-tjänsten från hello nästa dialogruta. För den här snabbstarten, välj **kan köras av gäst**.

Namnge hello service **MyGuestService** och ange hello alternativ på hello rätt toohello följande värden:

| Inställning                   | Värde |
| ------------------------- | ------ |
| Kodpaketmapp       | _&lt;hello mappen med din Node.js-app&gt;_ |
| Kodpaketbeteende     | Kopiera mappen innehållet tooproject |
| Program                   | node.exe |
| Argument                 | server.js |
| Arbetsmapp            | Kodpaket |

Tryck på **OK**.

![Dialogrutan Ny tjänst i Visual Studio][new-service]

Visual Studio skapar hello projektet och hello aktören service-projekt och visar dem i Solution Explorer.

hello projektet (**MyGuestApp**) innehåller inte någon kod direkt. I stället refererar det till en uppsättning tjänstprojekt. Dessutom innehåller det tre andra typer av innehåll:

* **Publicera profiler**  
Verktygsinställningar för olika miljöer.

* **Skript**  
PowerShell-skript för distribution/uppgradering av program.

* **Programdefinition**  
Innehåller hello programmanifestet under *ApplicationPackageRoot*. Associerade programfiler för parametern är *ApplicationParameters*, som definierar hello program och Tillåt tooconfigure den specifikt för en given miljö.
    
En översikt över hello innehållet i hello service-projekt, se [komma igång med Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="set-up-networking"></a>Konfigurera nätverk

hello exempel Node.js-appen som vi distribuerar använder port **80** och vi behöver tootell Service Fabric som vi behöver för att öppna port.

Öppna hello **ServiceManifest.xml** fil i hello-projekt. Längst ned hello hello manifestet, det finns en `<Resources> \ <Endpoints>` med en transaktion som redan har definierats. Ändra den post tooadd `Port`, `Protocol`, och `Type`. 

```xml
  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyGuestAppServiceTypeEndpoint" Port="80" Protocol="http" Type="Input" />
    </Endpoints>
  </Resources>
```

## <a name="deploy-tooazure"></a>Distribuera tooAzure

Om du trycker på **F5** och kör hello-projekt är det distribuerade toohello lokala klustret. Men vi distribuera tooAzure i stället.

Högerklicka på hello projektet och välj **publicera...**  som öppnar en dialogruta toopublish tooAzure.

![Publicera tooazure dialogrutan för en tjänst för service fabric][publish]

Välj hello **PublishProfiles\Cloud.xml** mål profil.

Välj en Azure-konto toodeploy till om du inte gjort tidigare. Om du inte har en ännu, kan du [registrera dig för en][create-account].

Under **Anslutningens slutpunkt**, Välj hello Service Fabric-kluster toodeploy till. Om du inte har något väljer  **&lt;Skapa nytt kluster... &gt;**  som öppnas web webbläsare fönstret toohello Azure-portalen. Mer information finns i [skapar ett kluster i hello portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal). 

När du skapar hello Service Fabric-kluster, se till att tooset hello **anpassade slutpunkter** inställning för**80**.

![Service Fabric-nod-typkonfiguration med anpassad slutpunkt][custom-endpoint]

Skapa ett nytt Service Fabric-kluster tar några tid toocomplete. När det har skapats och gå tillbaka toohello dialogrutan Publicera och välj  **&lt;uppdatera&gt;**. hello nya klustret visas i hello listrutan; Markera den.

Tryck på **publicera** och vänta tills hello distribution toofinish.

Det kan ta några minuter. När den är klar, kan det ta några minuter för hello programmet toobe helt tillgängligt.

## <a name="test-hello-website"></a>Testa hello webbplats

När tjänsten har publicerats kan du testa den i en webbläsare. 

Först öppna hello Azure-portalen och Sök efter Service Fabric-tjänsten.

Kontrollera hello översikt bladet för hello-adress. Använd hello domännamn från hello _klienten Anslutningens slutpunkt_ egenskapen. Till exempel `http://mysvcfab1.westus2.cloudapp.azure.com`.

![Service fabric översikt bladet på hello Azure-portalen][overview]

Navigera toothis adress där du kan se hello `HELLO WORLD` svar.

## <a name="delete-hello-cluster"></a>Ta bort hello kluster

Glöm inte toodelete alla hello-resurser som du har skapat för den här snabbstarten, som du debiteras för dessa resurser.

## <a name="next-steps"></a>Nästa steg
Läs mer om [körbara filer för gäst](service-fabric-deploy-existing-app.md).

<!-- Image References -->

[new-project]: ./media/quickstart-guest-app/new-project.png
[new-service]: ./media/quickstart-guest-app/template.png
[solution-exp]: ./media/quickstart-guest-app/solution-explorer.png
[publish]: ./media/quickstart-guest-app/publish.png
[overview]: ./media/quickstart-guest-app/overview.png
[custom-endpoint]: ./media/quickstart-guest-app/custom-endpoint.png

[download-sample]: https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/service-fabric-node-website.zip
[create-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F