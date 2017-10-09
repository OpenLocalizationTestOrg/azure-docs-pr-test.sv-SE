---
title: aaaService Fabric app uppgradera kursen | Microsoft Docs
description: "Den här artikeln beskriver hur hello erfarenhet av distribution av ett Service Fabric-program, ändra hello kod och distribution av en uppgradering med hjälp av Visual Studio."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a3181a7a-9ab1-4216-b07a-05b79bd826a4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: d069ff0b291018dbac846e65cddff1e9d73d156c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-tutorial-using-visual-studio"></a>Service Fabric uppgradera självstudiekursen använder Visual Studio
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

Azure Service Fabric förenklar hello processen med att uppgradera molnprogram genom att säkerställa att endast ändrade tjänster är uppgraderade och att programmets hälsotillstånd övervakas i hela hello uppgraderingsprocessen. Den återställer också automatiskt hello toohello tidigare programversion ska vidta när problem. Service Fabric programuppgraderingar är *noll avbrottstid*eftersom programmet hello kan uppgraderas utan avbrott. Den här självstudiekursen beskrivs hur toocomplete löpande uppgradering från Visual Studio.

## <a name="step-1-build-and-publish-hello-visual-objects-sample"></a>Steg 1: Skapa och publicera hello visuella objekt exemplet
Hämta först hello [visuella objekt](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Actors/VisualObjects) program från GitHub. Sedan skapa och publicera programmet hello genom att högerklicka på projektet hello, **VisualObjects**, och välja hello **publicera** i hello Service Fabric-menyalternativet.

![Snabbmenyn för ett Service Fabric-program][image1]

Att välja **publicera** öppnas ett popup-fönster, och du kan ange hello **mål profil** för**PublishProfiles\Local.xml**. hello fönstret bör se ut som hello följande innan du klickar på **publicera**.

![Publicera ett Service Fabric-program][image2]

Nu kan du klicka på **publicera** hello i dialogrutan. Du kan använda [Service Fabric Explorer tooview hello klustret och hello programmet](service-fabric-visualizing-your-cluster.md). hello visuella objekt programmet har en webbtjänst som du kan gå att skriva tooby [http://localhost:8081/visualobjects/](http://localhost:8081/visualobjects/) i hello adressfältet i webbläsaren.  Du bör se 10 flytande visuella objekt flytta på hello-skärmen.

**Obs:** om distribuerar för`Cloud.xml` profil (Azure Service Fabric) hello programmet sedan ska vara tillgängligt vid **http://{ServiceFabricName}. { Region}.cloudapp.Azure.com:8081/visualobjects/**. Kontrollera att du har `8081/TCP` konfigurerats i hello belastningsutjämnare (hitta hello belastningsutjämnare i hello samma resursgrupp som hello Service Fabric-instans).

## <a name="step-2-update-hello-visual-objects-sample"></a>Steg 2: Uppdatera hello visuella objekt exempel
Du kan se att med hello-version som har distribuerats i steg 1 hello visuella objekt inte rotera. Vi uppgradera det här programmet tooone där hello visuella objekt också rotera.

Välj hello VisualObjects.ActorService projekt i hello VisualObjects lösningen och öppna hello **VisualObjectActor.cs** fil. Gå toohello metod i denna fil `MoveObject`, kommentera ut `visualObject.Move(false)`, och Avkommentera `visualObject.Move(true)`. Den här koden ändringen roterar hello objekt när hello-tjänsten har uppgraderats.  **Nu kan du skapa (inte återskapa) hello lösningen**, som bygger hello ändrade projekt. Om du väljer *återskapa alla*, du har tooupdate hello versioner för alla hello projekt.

Vi behöver också tooversion vårt program. toomake hello version ändras när du högerklickar på hello **VisualObjects** -projekt kan du använda hello Visual Studio **redigera Manifest versioner** alternativet. Det här alternativet öppnar hello dialogrutan för edition versioner på följande sätt:

![Dialogrutan för versionshantering][image3]

Hello Uppdateringsversioner för hello ändrade projekt och deras kod paket, tillsammans med hello programmet tooversion 2.0.0. När hello ändringar har gjorts hello manifestet bör se ut som följande hello (fetstil delar visa hello ändringar):

![Uppdatera versioner][image4]

hello Visual Studio tools kan göra automatiska uppdateringar av versioner när du väljer **automatiskt uppdatera programmet och service versioner**. Om du använder [SemVer](http://www.semver.org), behöver du tooupdate hello koden och/eller konfiguration paketet version enbart om alternativet är markerat.

Spara hello ändringar och nu kontrollera hello **uppgradera hello programmet** rutan.

## <a name="step-3--upgrade-your-application"></a>Steg 3: Uppgradera ditt program
Bekanta dig med hello [uppgradera applikationsparametrarna](service-fabric-application-upgrade-parameters.md) och hello [uppgraderingsprocessen](service-fabric-application-upgrade.md) tooget en god förståelse av hello olika uppgradera parametrar, timeout och hälsotillstånd villkor som kan tillämpas. Den här genomgången anges hello service hälsa utvärdering kriterium toohello standard (oövervakade läge). Du kan konfigurera dessa inställningar genom att välja **konfigurera inställningar för uppgradering av** och sedan ändra hello parametrarna efter behov.

Nu är alla set toostart hello programmet uppgraderingen genom att välja **publicera**. Det här alternativet om du uppgraderar ditt program tooversion 2.0.0, där hello objekt rotera. Service Fabric uppgraderas en uppdateringsdomän i taget (vissa objekt, uppdateras först, följt av andra) och hello tjänsten förblir tillgängligt under hello uppgraderingen. Tjänsten för dataåtkomst toohello kan kontrolleras via din klient (webbläsare).  

Nu som hello programmet uppgraderingen fortsätter du kan övervaka den med Service Fabric Explorer med hjälp av hello **uppgraderingar pågår** fliken under hello program.

Alla domäner som uppdateringen ska uppgraderas (slutförd) om några minuter och hello Visual Studio utdatafönstret bör också ange att hello uppgraderingen har slutförts. Och du bör hitta som *alla* hello visuella objekt i webbläsarfönstret nu rotera!

Om du vill tootry ändrar hello och flytta från version 2.0.0 tooversion 3.0.0 som Övning eller även från version 2.0.0 tillbaka tooversion 1.0.0. Spela upp med tidsgränser och hälsotillstånd principer toomake själv bekant med dem. När du distribuerar tooan Azure klustret i motsats tooa lokala klustret, kan hello parametrar som används ha toodiffer. Vi rekommenderar att du har angett hello timeout hänsyn.

## <a name="next-steps"></a>Nästa steg
[Uppgradera ditt program med hjälp av PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) vägleder dig genom en uppgradering av programmet med hjälp av PowerShell.

Styra hur programmet uppgraderas med hjälp av [Uppgraderingsparametrar](service-fabric-application-upgrade-parameters.md).

Gör din programuppgraderingar kompatibla genom att lära dig hur toouse [dataserialisering](service-fabric-application-upgrade-data-serialization.md).

Lär dig hur toouse avancerade funktioner när du uppgraderar ditt program genom att referera för[avancerade alternativ](service-fabric-application-upgrade-advanced.md).

Lösa vanliga problem i programuppgraderingar genom att referera toohello stegen i [felsökning programuppgraderingar](service-fabric-application-upgrade-troubleshooting.md).

[image1]: media/service-fabric-application-upgrade-tutorial/upgrade7.png
[image2]: media/service-fabric-application-upgrade-tutorial/upgrade1.png
[image3]: media/service-fabric-application-upgrade-tutorial/upgrade5.png
[image4]: media/service-fabric-application-upgrade-tutorial/upgrade6.png
