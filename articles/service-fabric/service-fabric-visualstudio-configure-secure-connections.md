---
title: "Konfigurera säkra anslutningar i Azure Service Fabric-kluster | Microsoft Docs"
description: "Lär dig hur du använder Visual Studio för att konfigurera säkra anslutningar som stöds av Azure Service Fabric-klustret."
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
ms.openlocfilehash: dc07b2f38d6fd2de941ebbe99303f6e63cbf122d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-connections-to-a-service-fabric-cluster-from-visual-studio"></a><span data-ttu-id="7499e-103">Konfigurera säkra anslutningar till ett Service Fabric-kluster från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7499e-103">Configure secure connections to a Service Fabric cluster from Visual Studio</span></span>
<span data-ttu-id="7499e-104">Lär dig hur du använder Visual Studio säker åtkomst till ett Azure Service Fabric-kluster med principer för åtkomstkontroll konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="7499e-104">Learn how to use Visual Studio to securely access an Azure Service Fabric cluster with access control policies configured.</span></span>

## <a name="cluster-connection-types"></a><span data-ttu-id="7499e-105">Kluster-anslutningstyper</span><span class="sxs-lookup"><span data-stu-id="7499e-105">Cluster connection types</span></span>
<span data-ttu-id="7499e-106">Två typer av anslutningar som stöds av Azure Service Fabric-kluster: **inte är säkra** anslutningar och **x509 certifikatbaserad** säkra anslutningar.</span><span class="sxs-lookup"><span data-stu-id="7499e-106">Two types of connections are supported by the Azure Service Fabric cluster: **non-secure** connections and **x509 certificate-based** secure connections.</span></span> <span data-ttu-id="7499e-107">(För Service Fabric-kluster finns lokalt, **Windows** och **dSTS** autentiseringar stöds också.) Du måste konfigurera anslutningstypen klustret när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="7499e-107">(For Service Fabric clusters hosted on-premises, **Windows** and **dSTS** authentications are also supported.) You have to configure the cluster connection type when the cluster is being created.</span></span> <span data-ttu-id="7499e-108">Anslutningstypen kan inte ändras när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="7499e-108">Once it's created, the connection type can’t be changed.</span></span>

<span data-ttu-id="7499e-109">Verktyg för Visual Studio Service Fabric stöder alla typer av autentisering för att ansluta till ett kluster för publicering.</span><span class="sxs-lookup"><span data-stu-id="7499e-109">The Visual Studio Service Fabric tools support all authentication types for connecting to a cluster for publishing.</span></span> <span data-ttu-id="7499e-110">Se [ställa in ett Service Fabric-kluster från Azure portal](service-fabric-cluster-creation-via-portal.md) instruktioner om hur du ställer in en säker Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="7499e-110">See [Setting up a Service Fabric cluster from the Azure portal](service-fabric-cluster-creation-via-portal.md) for instructions on how to set up a secure Service Fabric cluster.</span></span>

## <a name="configure-cluster-connections-in-publish-profiles"></a><span data-ttu-id="7499e-111">Konfigurera klustret anslutningar i Publicera profiler</span><span class="sxs-lookup"><span data-stu-id="7499e-111">Configure cluster connections in publish profiles</span></span>
<span data-ttu-id="7499e-112">Om du publicerar ett Service Fabric-projekt från Visual Studio kan du använda den **publicera Fabric tjänstprogrammet** dialogrutan för att välja ett Azure Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="7499e-112">If you publish a Service Fabric project from Visual Studio, use the **Publish Service Fabric Application** dialog box to choose an Azure Service Fabric cluster.</span></span> <span data-ttu-id="7499e-113">Under **Anslutningens slutpunkt**, Välj ett befintligt kluster under din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7499e-113">Under **Connection endpoint**, select an existing cluster under your subscription.</span></span>

![Den ** dialogrutan Publicera Service Fabric Application ** används för att konfigurera en Service Fabric-anslutning.][publishdialog]

<span data-ttu-id="7499e-115">Den **publicera Fabric tjänstprogrammet** dialogrutan verifieras automatiskt kluster-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="7499e-115">The **Publish Service Fabric Application** dialog box automatically validates the cluster connection.</span></span> <span data-ttu-id="7499e-116">Om du uppmanas logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="7499e-116">If prompted, sign in to your Azure account.</span></span> <span data-ttu-id="7499e-117">Om valideringen lyckas, innebär det att systemet har rätt certifikat installerat för att ansluta till klustret på ett säkert sätt eller klustret är inte säker.</span><span class="sxs-lookup"><span data-stu-id="7499e-117">If validation passes, it means that your system has the correct certificates installed to connect to the cluster securely, or your cluster is non-secure.</span></span> <span data-ttu-id="7499e-118">Verifieringsfel kan orsakas av problem med nätverket eller genom att inte låta systemet korrekt konfigurerade för att ansluta till en säker kluster.</span><span class="sxs-lookup"><span data-stu-id="7499e-118">Validation failures can be caused by network issues or by not having your system correctly configured to connect to a secure cluster.</span></span>

![Den ** publicera Service Fabric Application ** dialogrutan verifierar en befintlig konfigurerad anslutning för Service Fabric-klustret.][selectsfcluster]

### <a name="to-connect-to-a-secure-cluster"></a><span data-ttu-id="7499e-120">Att ansluta till en säker kluster</span><span class="sxs-lookup"><span data-stu-id="7499e-120">To connect to a secure cluster</span></span>
1. <span data-ttu-id="7499e-121">Kontrollera att du kan komma åt ett klientcertifikat som litar på målklustret.</span><span class="sxs-lookup"><span data-stu-id="7499e-121">Make sure you can access one of the client certificates that the destination cluster trusts.</span></span> <span data-ttu-id="7499e-122">Certifikatet delas vanligtvis som en Personal Information Exchange (.pfx)-fil.</span><span class="sxs-lookup"><span data-stu-id="7499e-122">The certificate is usually shared as a Personal Information Exchange (.pfx) file.</span></span> <span data-ttu-id="7499e-123">Se [ställa in ett Service Fabric-kluster från Azure portal](service-fabric-cluster-creation-via-portal.md) att konfigurera servern för att bevilja åtkomst till en klient.</span><span class="sxs-lookup"><span data-stu-id="7499e-123">See [Setting up a Service Fabric cluster from the Azure portal](service-fabric-cluster-creation-via-portal.md) for how to configure the server to grant access to a client.</span></span>
2. <span data-ttu-id="7499e-124">Installera det betrodda certifikatet.</span><span class="sxs-lookup"><span data-stu-id="7499e-124">Install the trusted certificate.</span></span> <span data-ttu-id="7499e-125">Dubbelklicka på PFX-filen för att göra detta, eller använda PowerShell-skript importera PfxCertificate för att importera certifikaten.</span><span class="sxs-lookup"><span data-stu-id="7499e-125">To do this, double-click the .pfx file, or use the PowerShell script Import-PfxCertificate to import the certificates.</span></span> <span data-ttu-id="7499e-126">Installera certifikatet till **Cert: \LocalMachine\My**.</span><span class="sxs-lookup"><span data-stu-id="7499e-126">Install the certificate to **Cert:\LocalMachine\My**.</span></span> <span data-ttu-id="7499e-127">Det är OK att acceptera alla standardinställningar vid import av certifikatet.</span><span class="sxs-lookup"><span data-stu-id="7499e-127">It's OK to accept all default settings while importing the certificate.</span></span>
3. <span data-ttu-id="7499e-128">Välj den **publicera...**  på snabbmenyn för projektet för att öppna den **publicera Azure-programmet** dialogrutan och välj sedan målklustret.</span><span class="sxs-lookup"><span data-stu-id="7499e-128">Choose the **Publish...** command on the shortcut menu of the project to open the **Publish Azure Application** dialog box and then select the target cluster.</span></span> <span data-ttu-id="7499e-129">Verktyget matchar anslutningen automatiskt och sparar säker anslutningsparametrar i profilen.</span><span class="sxs-lookup"><span data-stu-id="7499e-129">The tool automatically resolves the connection and saves the secure connection parameters in the publish profile.</span></span>
4. <span data-ttu-id="7499e-130">Valfritt: Du kan redigera profilen som om du vill ange en säker kluster-anslutning.</span><span class="sxs-lookup"><span data-stu-id="7499e-130">Optional: You can edit the publish profile to specify a secure cluster connection.</span></span>
   
   <span data-ttu-id="7499e-131">Eftersom du redigerar manuellt publicera profil-XML-filen om du vill ange certifikatinformationen anteckna Certifikatarkivets namn kan lagra plats och tumavtrycket för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="7499e-131">Since you're manually editing the Publish Profile XML file to specify the certificate information, be sure to note the certificate store name, store location, and certificate thumbprint.</span></span> <span data-ttu-id="7499e-132">Du måste ange dessa värden för certifikatets store namn och plats.</span><span class="sxs-lookup"><span data-stu-id="7499e-132">You'll need to provide these values for the certificate's store name and store location.</span></span> <span data-ttu-id="7499e-133">Se [så här: hämta en certifikatets tumavtryck](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="7499e-133">See [How to: Retrieve the Thumbprint of a Certificate](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) for more information.</span></span>
   
   <span data-ttu-id="7499e-134">Du kan använda den *ClusterConnectionParameters* parametrar för att ange PowerShell-parametrar som ska användas vid anslutning till Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="7499e-134">You can use the *ClusterConnectionParameters* parameters to specify the PowerShell parameters to use when connecting to the Service Fabric cluster.</span></span> <span data-ttu-id="7499e-135">Parametrar som är giltiga som accepteras av Connect-ServiceFabricCluster cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7499e-135">Valid parameters are any that are accepted by the Connect-ServiceFabricCluster cmdlet.</span></span> <span data-ttu-id="7499e-136">Se [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) en lista över tillgängliga parametrar.</span><span class="sxs-lookup"><span data-stu-id="7499e-136">See [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) for a list of available parameters.</span></span>
   
   <span data-ttu-id="7499e-137">Om du ska publicera till ett kluster, måste du ange lämpliga parametrar för specifika klustret.</span><span class="sxs-lookup"><span data-stu-id="7499e-137">If you’re publishing to a remote cluster, you need to specify the appropriate parameters for that specific cluster.</span></span> <span data-ttu-id="7499e-138">Följande är ett exempel på ansluter till en icke-säker kluster:</span><span class="sxs-lookup"><span data-stu-id="7499e-138">The following is an example of connecting to a non-secure cluster:</span></span>
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   <span data-ttu-id="7499e-139">Här är ett exempel för att ansluta till x509 certifikatbaserad säker klustret:</span><span class="sxs-lookup"><span data-stu-id="7499e-139">Here’s an example for connecting to an x509 certificate-based secure cluster:</span></span>
   
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
5. <span data-ttu-id="7499e-140">Redigera andra nödvändiga inställningar, till exempel uppgradera parametrar och programmet parametern plats, och publicera programmet från den **publicera Fabric tjänstprogrammet** dialogrutan i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7499e-140">Edit any other necessary settings, such as upgrade parameters and Application Parameter file location, and then publish your application from the **Publish Service Fabric Application** dialog box in Visual Studio.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7499e-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7499e-141">Next steps</span></span>
<span data-ttu-id="7499e-142">Mer information om åtkomst till Service Fabric-kluster finns [visualisera ditt kluster med hjälp av Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="7499e-142">For more information about accessing Service Fabric clusters, see [Visualizing your cluster by using Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
