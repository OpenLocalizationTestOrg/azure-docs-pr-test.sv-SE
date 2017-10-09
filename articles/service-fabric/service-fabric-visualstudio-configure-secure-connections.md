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
# <a name="configure-secure-connections-tooa-service-fabric-cluster-from-visual-studio"></a><span data-ttu-id="46658-103">Konfigurera säkra anslutningar tooa Service Fabric-kluster från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46658-103">Configure secure connections tooa Service Fabric cluster from Visual Studio</span></span>
<span data-ttu-id="46658-104">Lär dig hur toouse Visual Studio toosecurely åt ett Azure Service Fabric-kluster med principer för åtkomstkontroll konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="46658-104">Learn how toouse Visual Studio toosecurely access an Azure Service Fabric cluster with access control policies configured.</span></span>

## <a name="cluster-connection-types"></a><span data-ttu-id="46658-105">Kluster-anslutningstyper</span><span class="sxs-lookup"><span data-stu-id="46658-105">Cluster connection types</span></span>
<span data-ttu-id="46658-106">Två typer av anslutningar som stöds av hello Azure Service Fabric-kluster: **inte är säkra** anslutningar och **x509 certifikatbaserad** säkra anslutningar.</span><span class="sxs-lookup"><span data-stu-id="46658-106">Two types of connections are supported by hello Azure Service Fabric cluster: **non-secure** connections and **x509 certificate-based** secure connections.</span></span> <span data-ttu-id="46658-107">(För Service Fabric-kluster finns lokalt, **Windows** och **dSTS** autentiseringar stöds också.) Du har tooconfigure hello klustret anslutningstypen när hello klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="46658-107">(For Service Fabric clusters hosted on-premises, **Windows** and **dSTS** authentications are also supported.) You have tooconfigure hello cluster connection type when hello cluster is being created.</span></span> <span data-ttu-id="46658-108">Hello anslutningstypen kan inte ändras när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="46658-108">Once it's created, hello connection type can’t be changed.</span></span>

<span data-ttu-id="46658-109">hello verktyg för Visual Studio Service Fabric stöder alla typer av autentisering för den anslutande tooa kluster för publicering.</span><span class="sxs-lookup"><span data-stu-id="46658-109">hello Visual Studio Service Fabric tools support all authentication types for connecting tooa cluster for publishing.</span></span> <span data-ttu-id="46658-110">Se [ställa in ett Service Fabric-kluster från hello Azure-portalen](service-fabric-cluster-creation-via-portal.md) anvisningar för hur tooset upp en säker Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="46658-110">See [Setting up a Service Fabric cluster from hello Azure portal](service-fabric-cluster-creation-via-portal.md) for instructions on how tooset up a secure Service Fabric cluster.</span></span>

## <a name="configure-cluster-connections-in-publish-profiles"></a><span data-ttu-id="46658-111">Konfigurera klustret anslutningar i Publicera profiler</span><span class="sxs-lookup"><span data-stu-id="46658-111">Configure cluster connections in publish profiles</span></span>
<span data-ttu-id="46658-112">Om du publicerar ett Service Fabric-projekt från Visual Studio kan du använda hello **publicera Fabric tjänstprogrammet** dialogrutan rutan toochoose ett Azure Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="46658-112">If you publish a Service Fabric project from Visual Studio, use hello **Publish Service Fabric Application** dialog box toochoose an Azure Service Fabric cluster.</span></span> <span data-ttu-id="46658-113">Under **Anslutningens slutpunkt**, Välj ett befintligt kluster under din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="46658-113">Under **Connection endpoint**, select an existing cluster under your subscription.</span></span>

![hello ** dialogrutan Publicera Service Fabric Application ** är används tooconfigure en Service Fabric-anslutning.][publishdialog]

<span data-ttu-id="46658-115">Hej **publicera Fabric tjänstprogrammet** dialogrutan verifieras automatiskt hello klustret anslutning.</span><span class="sxs-lookup"><span data-stu-id="46658-115">hello **Publish Service Fabric Application** dialog box automatically validates hello cluster connection.</span></span> <span data-ttu-id="46658-116">Om du uppmanas logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="46658-116">If prompted, sign in tooyour Azure account.</span></span> <span data-ttu-id="46658-117">Om valideringen lyckas, innebär det att systemet har hello rätt certifikat installerat tooconnect toohello klustret på ett säkert sätt, eller klustret inte är säker.</span><span class="sxs-lookup"><span data-stu-id="46658-117">If validation passes, it means that your system has hello correct certificates installed tooconnect toohello cluster securely, or your cluster is non-secure.</span></span> <span data-ttu-id="46658-118">Verifieringsfel kan orsakas av problem med nätverket eller genom att inte låta systemet korrekt konfigurerad tooconnect tooa säker klustret.</span><span class="sxs-lookup"><span data-stu-id="46658-118">Validation failures can be caused by network issues or by not having your system correctly configured tooconnect tooa secure cluster.</span></span>

![hello ** publicera Service Fabric Application ** dialogrutan verifierar en befintlig konfigurerad anslutning för Service Fabric-klustret.][selectsfcluster]

### <a name="tooconnect-tooa-secure-cluster"></a><span data-ttu-id="46658-120">tooconnect tooa säker kluster</span><span class="sxs-lookup"><span data-stu-id="46658-120">tooconnect tooa secure cluster</span></span>
1. <span data-ttu-id="46658-121">Kontrollera att du kan komma åt en hello klientcertifikat som hello mål klustret förtroenden.</span><span class="sxs-lookup"><span data-stu-id="46658-121">Make sure you can access one of hello client certificates that hello destination cluster trusts.</span></span> <span data-ttu-id="46658-122">hello certifikatet delas vanligtvis som en Personal Information Exchange (.pfx)-fil.</span><span class="sxs-lookup"><span data-stu-id="46658-122">hello certificate is usually shared as a Personal Information Exchange (.pfx) file.</span></span> <span data-ttu-id="46658-123">Se [installera ett Service Fabric-kluster från hello Azure-portalen](service-fabric-cluster-creation-via-portal.md) för hur tooconfigure hello server toogrant åt tooa klienten.</span><span class="sxs-lookup"><span data-stu-id="46658-123">See [Setting up a Service Fabric cluster from hello Azure portal](service-fabric-cluster-creation-via-portal.md) for how tooconfigure hello server toogrant access tooa client.</span></span>
2. <span data-ttu-id="46658-124">Installera hello betrodda certifikat.</span><span class="sxs-lookup"><span data-stu-id="46658-124">Install hello trusted certificate.</span></span> <span data-ttu-id="46658-125">toodo, dubbelklicka på hello .pfx-fil eller använder hello PowerShell script importera PfxCertificate tooimport hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="46658-125">toodo this, double-click hello .pfx file, or use hello PowerShell script Import-PfxCertificate tooimport hello certificates.</span></span> <span data-ttu-id="46658-126">Installera hello certifikat för**Cert: \LocalMachine\My**.</span><span class="sxs-lookup"><span data-stu-id="46658-126">Install hello certificate too**Cert:\LocalMachine\My**.</span></span> <span data-ttu-id="46658-127">Det är OK tooaccept alla standardinställningar när du importerar hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="46658-127">It's OK tooaccept all default settings while importing hello certificate.</span></span>
3. <span data-ttu-id="46658-128">Välj hello **publicera...**  på hello snabbmenyn för hello projektet tooopen hello **publicera Azure-programmet** dialogrutan och välj sedan hello målkluster.</span><span class="sxs-lookup"><span data-stu-id="46658-128">Choose hello **Publish...** command on hello shortcut menu of hello project tooopen hello **Publish Azure Application** dialog box and then select hello target cluster.</span></span> <span data-ttu-id="46658-129">hello verktyget matchar hello anslutningen automatiskt och sparar hello säker anslutning parametrar i hello Publicera profil.</span><span class="sxs-lookup"><span data-stu-id="46658-129">hello tool automatically resolves hello connection and saves hello secure connection parameters in hello publish profile.</span></span>
4. <span data-ttu-id="46658-130">Valfritt: Du kan redigera hello Publicera profil toospecify en säker kluster-anslutning.</span><span class="sxs-lookup"><span data-stu-id="46658-130">Optional: You can edit hello publish profile toospecify a secure cluster connection.</span></span>
   
   <span data-ttu-id="46658-131">Eftersom du redigerar manuellt hello Publicera profil-XML-filen toospecify hello certifikatinformationen kan vara säker på att toonote hello Certifikatarkivets namn måste lagra plats och tumavtrycket för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="46658-131">Since you're manually editing hello Publish Profile XML file toospecify hello certificate information, be sure toonote hello certificate store name, store location, and certificate thumbprint.</span></span> <span data-ttu-id="46658-132">Du behöver tooprovide värdena för hello certifikatarkivet namn och plats.</span><span class="sxs-lookup"><span data-stu-id="46658-132">You'll need tooprovide these values for hello certificate's store name and store location.</span></span> <span data-ttu-id="46658-133">Se [så här: hämta hello tumavtrycket för ett certifikat](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="46658-133">See [How to: Retrieve hello Thumbprint of a Certificate](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) for more information.</span></span>
   
   <span data-ttu-id="46658-134">Du kan använda hello *ClusterConnectionParameters* parametrar toospecify hello PowerShell parametrar toouse när du ansluter toohello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="46658-134">You can use hello *ClusterConnectionParameters* parameters toospecify hello PowerShell parameters toouse when connecting toohello Service Fabric cluster.</span></span> <span data-ttu-id="46658-135">Parametrar som är giltiga som accepteras av hello Connect-ServiceFabricCluster cmdlet.</span><span class="sxs-lookup"><span data-stu-id="46658-135">Valid parameters are any that are accepted by hello Connect-ServiceFabricCluster cmdlet.</span></span> <span data-ttu-id="46658-136">Se [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) en lista över tillgängliga parametrar.</span><span class="sxs-lookup"><span data-stu-id="46658-136">See [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) for a list of available parameters.</span></span>
   
   <span data-ttu-id="46658-137">Om du ska publicera tooa fjärrkluster, måste toospecify hello lämpliga parametrar för specifika klustret.</span><span class="sxs-lookup"><span data-stu-id="46658-137">If you’re publishing tooa remote cluster, you need toospecify hello appropriate parameters for that specific cluster.</span></span> <span data-ttu-id="46658-138">hello följande är ett exempel på anslutande tooa icke-säker klustret:</span><span class="sxs-lookup"><span data-stu-id="46658-138">hello following is an example of connecting tooa non-secure cluster:</span></span>
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   <span data-ttu-id="46658-139">Här är ett exempel för anslutande tooan x509 certifikatbaserad säker klustret:</span><span class="sxs-lookup"><span data-stu-id="46658-139">Here’s an example for connecting tooan x509 certificate-based secure cluster:</span></span>
   
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
5. <span data-ttu-id="46658-140">Redigera andra nödvändiga inställningar, till exempel uppgradera parametrar och programmet parametern plats, och publicera programmet från hello **publicera Fabric tjänstprogrammet** dialogrutan i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46658-140">Edit any other necessary settings, such as upgrade parameters and Application Parameter file location, and then publish your application from hello **Publish Service Fabric Application** dialog box in Visual Studio.</span></span>

## <a name="next-steps"></a><span data-ttu-id="46658-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46658-141">Next steps</span></span>
<span data-ttu-id="46658-142">Mer information om åtkomst till Service Fabric-kluster finns [visualisera ditt kluster med hjälp av Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="46658-142">For more information about accessing Service Fabric clusters, see [Visualizing your cluster by using Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
