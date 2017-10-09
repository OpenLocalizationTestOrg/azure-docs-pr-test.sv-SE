---
title: "aaaVisualizing med hjälp av Service Fabric Explorer | Microsoft Docs"
description: "Service Fabric Explorer är ett webbaserat verktyg för att kontrollera och hantera molnprogram och noder i ett Microsoft Azure Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: ryanwi
ms.openlocfilehash: 73adc4fc254cf6b949b4419b02a046cee3f6a83d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Visualisera ditt kluster med Service Fabric Explorer
Service Fabric Explorer är ett webbaserat verktyg för att kontrollera och hantera program och noderna i Azure Service Fabric-klustret. Service Fabric Explorer ligger direkt på hello klustret så att det alltid är tillgängliga, oavsett om klustret körs.

## <a name="video-tutorial"></a>Videosjälvstudie

toolearn hur toouse Service Fabric Explorer titta på hello följande Microsoft Virtual Academy video:

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-tooservice-fabric-explorer"></a>Ansluta tooService Fabric Explorer
Om du har följt anvisningarna hello för[förbereda din utvecklingsmiljö](service-fabric-get-started.md), kan du starta Service Fabric-Utforskaren på din lokala klustret genom att gå toohttp://localhost:19080 / Explorer.

## <a name="understand-hello-service-fabric-explorer-layout"></a>Förstå hello Service Fabric Explorer layout
Du kan bläddra igenom Service Fabric Explorer med hjälp av hello trädet hello vänster. Hello roten i trädet hello innehåller hello klusterinstrumentpanel en översikt över klustret, inklusive en sammanfattning av program och noden hälsa.

![Instrumentpanelen för Service Fabric Explorer-klustret][sfx-cluster-dashboard]

### <a name="view-hello-clusters-layout"></a>Visa hello klustrets layout
Noder i ett Service Fabric-kluster är placerade över en tvådimensionell rutnät för feldomäner och uppgradera domäner. Den här placering garanterar att dina program ska vara tillgängliga i hello förekomsten av maskinvarufel och programuppgraderingar. Du kan visa hur hello aktuella klustret är placerade med hjälp av hello över lediga kluster.

![Service Fabric Explorer över lediga kluster][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Visa program och tjänster
hello-kluster som innehåller två underträd: en för program och en annan för noder.

Du kan använda hello program visa toonavigate via Service Fabric logiska hierarkin: program, tjänster, partitioner och repliker.

Hej program i hello exemplet nedan **MyApp** består av två tjänster **MyStatefulService** och **WebService**. Eftersom **MyStatefulService** är tillståndskänslig så innehåller en partition med en primär och två sekundära repliker. Däremot WebSvcService är tillståndslösa och innehåller en enda instans.

![Service Fabric Explorer programvy][sfx-application-tree]

På varje nivå i trädet hello visar hello huvudfönstret relevant information om hello-objektet. Du kan till exempel se hello hälsostatus och version för en viss tjänst.

![Service Fabric Explorer essentials fönstret][sfx-service-essentials]

### <a name="view-hello-clusters-nodes"></a>Visa hello klusternoder
hello nod vyn visar hello fysiska struktur hello klustret. För en viss nod kan du inspektera vilka program som har kod distribuerad på noden. Mer specifikt kan du se vilka repliker körs det.

## <a name="actions"></a>Åtgärder
Service Fabric Explorer erbjuder ett snabbt sätt tooinvoke åtgärder på noder, program och tjänster inom klustret.

Till exempel toodelete en programinstans Välj hello programmet hello trädet hello vänster och välj **åtgärder** > **ta bort programmet**.

![Om du tar bort ett program i Service Fabric Explorer][sfx-delete-application]

> [!TIP]
> Du kan utföra samma åtgärder hello genom att klicka på hello knappen Nästa tooeach element.
>
>

hello visas följande tabell hello-åtgärder som är tillgängliga för varje entitet:

| **Entitet** | **Åtgärd** | **Beskrivning** |
| --- | --- | --- |
| Programtyp |Avetablera typ |Tar bort hello programpaketet från avbildningsarkivet hello klustret. Kräver att typen toobe först bort alla applikationer. |
| Program |Ta bort program |Ta bort hello program, inklusive alla tjänster och deras tillstånd (eventuella). |
| Tjänst |Ta bort tjänsten |Ta bort hello service och dess tillstånd (eventuella). |
| Node |Aktivera |Aktivera hello-nod. |
| Node | Inaktivera (paus) | Pausa hello nod i det aktuella tillståndet. Tjänster fortsätter toorun men Service Fabric flyttas proaktivt inte något på eller inaktivera den om den inte är nödvändiga tooprevent ett avbrott eller inkonsekventa data. Den här åtgärden är brukar användas tooenable felsökning tjänster på en viss nod-tooensure inte flytta under kontroll. | |
| Node | Inaktivera (omstart) | Flytta alla InMemory-tjänster av en nod och Stäng beständiga tjänster på ett säkert sätt. Används vanligtvis när hello värdprocesser eller toobe för datorn måste startas om. | |
| Node | Inaktivera (ta bort data) | Stäng alla tjänster som körs på hello nod när du har skapat tillräckligt ledig repliker på ett säkert sätt. Används vanligtvis när en nod (eller åtminstone dess lagring) som permanent tas utanför kommissionen. | |
| Node | Ta bort nodens tillstånd | Ta bort kunskap om en nod repliker från hello kluster. Används vanligtvis när en redan felaktiga noden bedöms oåterkalleligt. | |
| Node | Starta om | Simulera ett nodfel genom att starta om hello-nod. Mer information [här](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps) | |

Eftersom många åtgärder är skadliga du tillfrågas tooconfirm din avsikt innan hello åtgärd har slutförts.

> [!TIP]
> Varje åtgärd som kan utföras via Service Fabric Explorer kan också utföras via PowerShell eller REST-API, tooenable automation.
>
>

Du kan också använda Service Fabric Explorer toocreate programinstanser för en given programtypen och versionen. Välj hello programtyp i hello trädvyn och klicka sedan på hello **skapa app-instansen** länken nästa toohello version som du vill i hello till höger.

![Skapa en programinstans i Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> Programinstanser som skapats via Service Fabric Explorer kan för närvarande parameteriseras. De skapas med hjälp av standardparametervärden.
>
>

## <a name="connect-tooa-remote-service-fabric-cluster"></a>Ansluta tooa remote Service Fabric-kluster
Om du känner hello klusterslutpunkten och har behörighet kan du komma åt Service Fabric Explorer från en webbläsare. Detta beror på att Service Fabric Explorer är en tjänst som körs i hello kluster.

### <a name="discover-hello-service-fabric-explorer-endpoint-for-a-remote-cluster"></a>Identifiera hello Service Fabric Explorer-slutpunkt för ett kluster
tooreach Service Fabric Explorer för ett kluster, peka webbläsaren:

http://&lt;din klusterslutpunkten&gt;: 19080/Explorer

För Azure-kluster finns också hello fullständiga URL: en i hello klustret essentials rutan hello Azure-portalen.

### <a name="connect-tooa-secure-cluster"></a>Ansluta tooa säker kluster
Du kan styra klienten åtkomst tooyour Service Fabric-kluster med certifikat eller med hjälp av Azure Active Directory (AAD).

Om du försöker tooconnect tooService Fabric-Utforskaren på en säker klustret sedan beroende på hello klusterkonfigurationen ska du vara nödvändiga toopresent ett klientcertifikat eller logga in med AAD.

## <a name="next-steps"></a>Nästa steg
* [Möjlighet att testa översikt](service-fabric-testability-overview.md)
* [Hantera dina Service Fabric-program i Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Service Fabric-programdistribution med hjälp av PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
