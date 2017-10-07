---
title: aaaDeploy en part kluster med Azure Service Fabric application tooa | Microsoft Docs
description: "Lär dig hur toodeploy ett program tooa part kluster."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.openlocfilehash: db16b6418fa2533ed915c8b6e612b7a8e7311bed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-party-cluster-in-azure"></a>Distribuera ett program tooa part kluster i Azure
Den här kursen ingår två av en serie och visar hur toodeploy ett Azure Service Fabric application tooa part kluster i Azure.

Del två av hello självstudiekursen serie du lära dig hur du:
> [!div class="checklist"]
> * Distribuera ett program tooa remote kluster med hjälp av Visual Studio
> * Ta bort ett program från ett kluster med Service Fabric Explorer

I den här självstudiekursen serien lär du dig hur du:
> [!div class="checklist"]
> * [Skapa ett .NET Service Fabric-program](service-fabric-tutorial-create-dotnet-app.md)
> * Distribuera programmet hello tooa fjärrkluster
> * [Konfigurera CI/CD: N med hjälp av Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a>Krav
Innan du börjar den här kursen:
- Om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Installera Visual Studio 2017](https://www.visualstudio.com/) och installera hello **Azure-utveckling** och **ASP.NET och web development** arbetsbelastningar.
- [Installera hello Service Fabric-SDK](service-fabric-get-started.md)

## <a name="download-hello-voting-sample-application"></a>Hämta hello röst exempelprogrammet
Om du inte att skapa hello röst exempelprogrammet [ingår i den här självstudiekursen serie](service-fabric-tutorial-create-dotnet-app.md), du kan ladda ned den. Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett kommandofönster.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-a-party-cluster"></a>Konfigurera en part-kluster
Part kluster är ledigt, tidsbegränsade Service Fabric-kluster i Azure och kör hello Service Fabric-grupp där alla kan distribuera program och lär dig mer om hello-plattformen. Kostnadsfritt!

tooget åtkomst tooa part kluster, bläddra toothis plats: http://aka.ms/tryservicefabric och följ hello instruktioner tooget åtkomst tooa klustret. Du behöver en Facebook eller GitHub-konto tooget åtkomst tooa part klustret.

> [!NOTE]
> Part kluster skyddas inte så att dina program och alla data som du lägger till i dem vara synliga tooothers. Distribuerar inte något du inte vill att andra toosee. Vara att tooread via vårt användningsvillkoren för alla hello information.

## <a name="configure-hello-listening-port"></a>Konfigurera hello lyssningsport
När hello VotingWeb frontend-tjänst skapas väljer slumpmässigt en port för hello service toolisten på Visual Studio.  Hej VotingWeb tjänsten fungerar som frontend för det här programmet hello och godkänner externa trafiken, så vi binda den tjänsten tooa fast och vet port korrekt. I Solution Explorer öppnar *VotingWeb/PackageRoot/ServiceManifest.xml*.  Hitta hello **Endpoint** resurs i hello **resurser** avsnittet och ändra hello **Port** värdet too80.

```xml
<Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="80" />
    </Endpoints>
  </Resources>
```

Uppdatera också hello programmets URL egenskapsvärdet i hello röst-projektet så att en webbläsare öppnas toohello rätt port när du felsöker med 'F5'.  Välj hello i Solution Explorer **Röstningsdatabasen** projekt och uppdatera hello **programmets URL** egenskapen.

![Programmets URL](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-url.png)

## <a name="deploy-hello-app-toohello-azure"></a>Distribuera hello app toohello Azure
Nu när programmet hello är klar, kan du distribuera den toohello part klustret direkt från Visual Studio.

1. Högerklicka på **Röstningsdatabasen** hello i Solution Explorer och välj **publicera**.

    ![Dialogrutan Publicera](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

2. Ange hello Anslutningens slutpunkt för hello part klustret i hello **Anslutningens slutpunkt** fältet och klickar på **publicera**.

    När hello publicera har slutförts, bör du kunna toosend en begäran toohello program via en webbläsare.

3. Öppna din webbläsare och ange hello klusteradress (hello Anslutningens slutpunkt utan portinformation hello - exempelvis win1kw5649s.westus.cloudapp.azure.com).

    Du bör nu se hello samma resultat som du såg när du kör programmet hello lokalt.

    ![API-svar från kluster](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="remove-hello-application-from-a-cluster-using-service-fabric-explorer"></a>Ta bort programmet hello från ett kluster med Service Fabric Explorer
Service Fabric Explorer är ett grafiskt gränssnitt tooexplore och hantera program i ett Service Fabric-kluster.

tooremove hello program från hello part klustret:

1. Bläddra toohello Service Fabric Explorer med hello-länk som tillhandahålls av hello part klustret registreringssidan. Till exempel http://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.

2. Navigera i Service Fabric Explorer toohello **fabric://Voting** noden i treeview hello hello vänster.

3. Klicka på hello **åtgärd** knapp i hello högra **Essentials** fönstret och välj **ta bort programmet**. Bekräfta borttagning hello programinstansen, vilket tar bort hello instans av vårt program som körs i hello klustret.

![Ta bort program i Service Fabric Explorer](./media/service-fabric-tutorial-deploy-app-to-party-cluster/delete-application.png)

## <a name="remove-hello-application-type-from-a-cluster-using-service-fabric-explorer"></a>Ta bort hello programtyp från ett kluster med Service Fabric Explorer
Program distribueras som programtyperna i ett Service Fabric-kluster, vilket gör du toohave flera instanser och versioner av hello-program som körs inom hello kluster. När du har tagit bort hello kör instans av vårt program, kan vi också ta bort hello typ, toocomplete hello rensning av hello distribution.

Mer information om hello programmodell i Service Fabric finns [modellen är ett program i Service Fabric](service-fabric-application-model.md).

1. Navigera toohello **VotingType** nod i hello treeview.

2. Klicka på hello **åtgärd** knapp i hello högra **Essentials** fönstret och välj **avetablera typen**. Bekräfta avetableras hello programtyp.

![Avetablera programtyp i Service Fabric Explorer](./media/service-fabric-tutorial-deploy-app-to-party-cluster/unprovision-type.png)

Detta avslutar hello kursen.

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Distribuera ett program tooa remote kluster med hjälp av Visual Studio
> * Ta bort ett program från ett kluster med Service Fabric Explorer

Avancerade toohello nästa kurs:
> [!div class="nextstepaction"]
> [Ställ in kontinuerlig integration med hjälp av Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)