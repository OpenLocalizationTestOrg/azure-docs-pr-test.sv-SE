---
title: "aaaPublish en app tooa fjärrkluster med Visual Studio | Microsoft Docs"
description: "Lär dig hur toopublish ett program tooa fjärrtjänsten fabric-kluster med hjälp av Visual Studio."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: faecd892-eb54-4d9c-8023-c67442afb8e8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/29/2016
ms.author: cawa
ms.openlocfilehash: d0f06f120cc7e22f3f8e73ce0970e1da5823e647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-visual-studio"></a>Distribuera och ta bort program med Visual Studio
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [FabricClient-API:er](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

hello Azure Service Fabric-tillägget för Visual Studio tillhandahåller ett enkelt, repeterbara och skriptbara sätt toopublish programmet tooa Service Fabric-klustret.

## <a name="hello-artifacts-required-for-publishing"></a>hello artefakter som krävs för publicering
### <a name="deploy-fabricapplicationps1"></a>Distribuera FabricApplication.ps1
Det här är ett PowerShell-skript som använder en publiceringssökväg som en parameter för publishing Service Fabric-program. Eftersom det här skriptet ingår i ditt program, är du Välkommen toomodify det som krävs för ditt program.

### <a name="publish-profiles"></a>Publicera profiler
En mapp i hello Service Fabric-programprojekt kallas **PublishProfiles** innehåller XML-filer som lagrar viktig information för att publicera ett program, exempelvis:

* Anslutningsparametrar för Service Fabric-kluster
* Sökvägen tooan programmet parameterfilen
* Inställningar för uppgradering

Som standard ditt program innehåller tre publicera profiler: Local.1Node.xml, Local.5Node.xml och Cloud.xml. Du kan lägga till fler profiler genom att kopiera och klistra in en hello filer som standard.

### <a name="application-parameter-files"></a>Parametern programfiler
En mapp i hello Service Fabric-programprojekt kallas **ApplicationParameters** innehåller XML-filer för användardefinierade application manifest parametervärden. Application manifest-filer kan parameteriseras så att du kan använda olika värden för distributionsinställningar. toolearn mer om Parameterisera ditt program, se [hantera flera miljöer i Service Fabric](service-fabric-manage-multiple-environment-app-configuration.md).

> [!NOTE]
> För aktörstjänster, bör du skapa hello projekt först innan du försöker tooedit hello fil i en textredigerare eller via hello publicera dialogrutan. Det beror på att en del av hello manifestfiler genereras under hello konstruktion.

## <a name="toopublish-an-application-using-hello-publish-service-fabric-application-dialog-box"></a>toopublish ett program med hjälp av dialogrutan för hello publicera program för Service Fabric
hello följande steg visar hur ett program som använder toopublish hello **publicera Fabric tjänstprogrammet** dialogruta hello Visual Studio Service Fabric-verktyg.

1. På hello snabbmenyn för hello Service Fabric Application-projekt, väljer du **publicera...** tooview hello **publicera Fabric tjänstprogrammet** dialogrutan.
   
    ![hello ** dialogrutan Publicera Service Fabric Application **][0]
   
    hello-filen som markerats i hello **mål profil** nedrullningsbara listrutan är var alla hello-inställningar, förutom **Manifest versioner**, sparas. Du kan återanvända en befintlig profil eller skapa en ny genom att välja **<... hantera profiler >** i hello **mål profil** nedrullningsbara listrutan. När du väljer en publiceringsprofil visas dess innehåll i hello motsvarande fält hello dialogrutan. toosave dina ändringar när som helst välja hello **spara profil** länk.    
2. I hello **Anslutningens slutpunkt** , anger en lokal eller fjärransluten Service Fabric klustrets publishing slutpunkt. tooadd eller ändra Hej Anslutningens slutpunkt, klicka på hello **Anslutningens slutpunkt** listrutan. hello listan visar hello tillgängliga Service Fabric-kluster anslutning slutpunkter toowhich kan du publicera baserat på dina Azure-abonnemang. Observera att om du inte redan är inloggad i tooVisual Studio du kommer att tillfrågas toodo så.
   
    Använd hello klustret markeringen dialogrutan rutan toochoose från hello uppsättning tillgängliga prenumerationer och -kluster.
   
    ![hello ** dialogrutan Välj Service Fabric klustret **][1]
   
   > [!NOTE]
   > Om du vill att toopublish tooan valfri slutpunkt (t.ex en part kluster), se hello **publicering tooan godtycklig klusterslutpunkten** nedan.
   > 
   > 
   
    När du väljer en slutpunkt verifierar Visual Studio hello anslutning toohello valda Service Fabric-klustret. Om hello klustret inte är säker, ansluta Visual Studio tooit omedelbart. Men om hello klustret är säker, behöver du tooinstall ett certifikat på den lokala datorn innan du fortsätter. Se [hur tooconfigure skydda anslutningar](service-fabric-visualstudio-configure-secure-connections.md) för mer information. När du är klar väljer du hello **OK** knappen. hello valda klustret visas i hello **publicera Fabric tjänstprogrammet** dialogrutan.
3. I hello **programmet parameterfilen** listrutan går tooan programmet parameterfil. Ett program parameterfilen innehåller användardefinierade värden för parametrar i hello programmanifestfilen. Välj hello tooadd eller ändra en parameter **redigera** knappen. Ange eller ändra hello parameterns värde i hello **parametrar** rutnätet. När du är klar väljer du hello **spara** knappen.
   
    ![hello ** dialogrutan Redigera parametrar **][2]
4. Använd hello **uppgradera hello programmet** kryssrutan toospecify om detta publicera åtgärd är en uppgradering. Uppgraderingen publicera åtgärder skiljer sig från normal publicera åtgärder. Se [Service Fabric Application uppgradera](service-fabric-application-upgrade.md) för en lista över skillnader. inställningar för tooconfigure, Välj hello **konfigurera inställningar för uppgradering av** länk. hello uppgradera Parameterredigeraren visas. Se [konfigurera hello uppgradering av ett Service Fabric-program](service-fabric-visualstudio-configure-upgrade.md) toolearn mer om uppgradering parametrar.
5. Välj hello **Manifest versioner...** knappen tooview hello **redigera versioner** dialogrutan. Du behöver tooupdate programmet och service-versioner för en uppgradering tootake plats. Se [Service Fabric uppgradera självstudien](service-fabric-application-upgrade-tutorial.md) toolearn hur programmet och service manifest versioner påverkar en uppgraderingsprocessen.
   
    ![hello ** dialogrutan Redigera versioner **][3]
   
    Om hello programmet och service versioner använder semantiska versionshantering, till exempel 1.0.0 eller numeriska värden i hello-format för 1.0.0.0 markerar hello **automatiskt uppdatera programmet och service versioner** alternativet. När du väljer det här alternativet, hello-tjänst och versionsnummer för programmet uppdateras automatiskt när en kod, config eller Paketversion data uppdateras. Om du föredrar tooedit hello versioner manuellt Rensa hello kryssrutan toodisable den här funktionen.
   
   > [!NOTE]
   > För alla paket poster tooappear för en aktören projekt, först skapa hello projektet toogenerate hello poster i hello Service Manifest-filer.
   > 
   > 
6. När du är klar att ange alla nödvändiga hello-inställningar, väljer hello **publicera** knappen toopublish ditt program toohello valda Service Fabric-klustret. hello-inställningar som du angett tillämpas toohello publiceringsprocessen.

## <a name="publish-tooan-arbitrary-cluster-endpoint-including-party-clusters"></a>Publicera tooan godtycklig klusterslutpunkten (inklusive part kluster)
hello Visual Studio publishing upplevelse är optimerad för att publicera tooremote kluster som är associerade med en av dina Azure-prenumerationer. Det är dock möjligt toopublish tooarbitrary slutpunkter (till exempel Service Fabric part kluster) genom att direkt redigera hello Publicera profil-XML. Enligt beskrivningen ovan, tre publicera profiler som tillhandahålls som standard--**Local.1Node.xml**, **Local.5Node.xml**, och **Cloud.xml**-- men du Välkommen toocreate ytterligare profiler för olika miljöer. Du kan exempelvis vill toocreate en profil för att publicera tooparty kluster med namnet kanske **Party.xml**.

Om du ansluter tooan oskyddad klustret allt som krävs är hello klustret Anslutningens slutpunkt som `partycluster1.eastus.cloudapp.azure.com:19000`. I den fallet hello Anslutningens slutpunkt i hello Publicera profil skulle se ut ungefär så här:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Om du ansluter tooa skyddade kluster behöver också tooprovide hello information om hello klientcertifikat från hello lokalt Arkiv toobe används för autentisering. Mer information finns i [Konfigurera säkra anslutningar tooa Service Fabric-kluster](service-fabric-visualstudio-configure-secure-connections.md).

  När din publiceringsprofil har konfigurerats kan du använda det i hello publicera dialogruta som visas nedan.

  ![Nya publiceringsprofil publicera i dialogrutan][4]

  Observera att nya hello Publicera profil pekar tooone av hello standard parametern programfiler. Detta är lämpligt om du vill toopublish hello samma program configuration tooa antal miljöer. Däremot i fall där du vill att toohave olika konfigurationer för varje miljö som du vill toopublish att blir det meningsfullt toocreate en motsvarande program parameterfil.

## <a name="next-steps"></a>Nästa steg
hur tooautomate hello publiceringsprocessen i en kontinuerlig integration miljö, se toolearn [Ställ in kontinuerlig integration av Service Fabric](service-fabric-set-up-continuous-integration.md).

[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
