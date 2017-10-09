---
title: "aaaConfigure säker Azure Service Fabric klusteranslutningar | Microsoft Docs"
description: "Lär dig hur toouse Visual Studio tooconfigure skydda anslutningar som stöds av hello Azure Service Fabric-klustret."
services: service-fabric
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: tglee
ms.assetid: 80501867-dd7a-4648-8bd6-d4f26b68402d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/04/2017
ms.author: cawa
ms.openlocfilehash: ed3e5043264cf026f74e24ca09b40ccc70086cf2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-connections-tooa-service-fabric-cluster-from-visual-studio"></a>Konfigurera säkra anslutningar tooa Service Fabric-kluster från Visual Studio
Lär dig hur toouse Visual Studio toosecurely åt ett Azure Service Fabric-kluster med principer för åtkomstkontroll konfigurerats.

## <a name="cluster-connection-types"></a>Kluster-anslutningstyper
Två typer av anslutningar som stöds av hello Azure Service Fabric-kluster: **inte är säkra** anslutningar och **x509 certifikatbaserad** säkra anslutningar. (För Service Fabric-kluster finns lokalt, **Windows** och **dSTS** autentiseringar stöds också.) Du har tooconfigure hello klustret anslutningstypen när hello klustret skapas. Hello anslutningstypen kan inte ändras när den har skapats.

hello verktyg för Visual Studio Service Fabric stöder alla typer av autentisering för den anslutande tooa kluster för publicering. Se [ställa in ett Service Fabric-kluster från hello Azure-portalen](service-fabric-cluster-creation-via-portal.md) anvisningar för hur tooset upp en säker Service Fabric-klustret.

## <a name="configure-cluster-connections-in-publish-profiles"></a>Konfigurera klustret anslutningar i Publicera profiler
Om du publicerar ett Service Fabric-projekt från Visual Studio kan du använda hello **publicera Fabric tjänstprogrammet** dialogrutan rutan toochoose ett Azure Service Fabric-kluster. Under **Anslutningens slutpunkt**, Välj ett befintligt kluster under din prenumeration.

![hello ** dialogrutan Publicera Service Fabric Application ** är används tooconfigure en Service Fabric-anslutning.][publishdialog]

Hej **publicera Fabric tjänstprogrammet** dialogrutan verifieras automatiskt hello klustret anslutning. Om du uppmanas logga in tooyour Azure-konto. Om valideringen lyckas, innebär det att systemet har hello rätt certifikat installerat tooconnect toohello klustret på ett säkert sätt, eller klustret inte är säker. Verifieringsfel kan orsakas av problem med nätverket eller genom att inte låta systemet korrekt konfigurerad tooconnect tooa säker klustret.

![hello ** publicera Service Fabric Application ** dialogrutan verifierar en befintlig konfigurerad anslutning för Service Fabric-klustret.][selectsfcluster]

### <a name="tooconnect-tooa-secure-cluster"></a>tooconnect tooa säker kluster
1. Kontrollera att du kan komma åt en hello klientcertifikat som hello mål klustret förtroenden. hello certifikatet delas vanligtvis som en Personal Information Exchange (.pfx)-fil. Se [installera ett Service Fabric-kluster från hello Azure-portalen](service-fabric-cluster-creation-via-portal.md) för hur tooconfigure hello server toogrant åt tooa klienten.
2. Installera hello betrodda certifikat. toodo, dubbelklicka på hello .pfx-fil eller använder hello PowerShell script importera PfxCertificate tooimport hello certifikat. Installera hello certifikat för**Cert: \LocalMachine\My**. Det är OK tooaccept alla standardinställningar när du importerar hello certifikat.
3. Välj hello **publicera...**  på hello snabbmenyn för hello projektet tooopen hello **publicera Azure-programmet** dialogrutan och välj sedan hello målkluster. hello verktyget matchar hello anslutningen automatiskt och sparar hello säker anslutning parametrar i hello Publicera profil.
4. Valfritt: Du kan redigera hello Publicera profil toospecify en säker kluster-anslutning.
   
   Eftersom du redigerar manuellt hello Publicera profil-XML-filen toospecify hello certifikatinformationen kan vara säker på att toonote hello Certifikatarkivets namn måste lagra plats och tumavtrycket för certifikatet. Du behöver tooprovide värdena för hello certifikatarkivet namn och plats. Se [så här: hämta hello tumavtrycket för ett certifikat](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) för mer information.
   
   Du kan använda hello *ClusterConnectionParameters* parametrar toospecify hello PowerShell parametrar toouse när du ansluter toohello Service Fabric-klustret. Parametrar som är giltiga som accepteras av hello Connect-ServiceFabricCluster cmdlet. Se [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) en lista över tillgängliga parametrar.
   
   Om du ska publicera tooa fjärrkluster, måste toospecify hello lämpliga parametrar för specifika klustret. hello följande är ett exempel på anslutande tooa icke-säker klustret:
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   Här är ett exempel för anslutande tooan x509 certifikatbaserad säker klustret:
   
   ```xml
   <ClusterConnectionParameters
   ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
   X509Credential="true"
   ServerCertThumbprint="0123456789012345678901234567890123456789"
   FindType="FindByThumbprint"
   FindValue="9876543210987654321098765432109876543210"
   StoreLocation="CurrentUser"
   StoreName="My" />
   ```
5. Redigera andra nödvändiga inställningar, till exempel uppgradera parametrar och programmet parametern plats, och publicera programmet från hello **publicera Fabric tjänstprogrammet** dialogrutan i Visual Studio.

## <a name="next-steps"></a>Nästa steg
Mer information om åtkomst till Service Fabric-kluster finns [visualisera ditt kluster med hjälp av Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
