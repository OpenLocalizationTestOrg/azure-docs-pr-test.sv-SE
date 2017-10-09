---
title: aaaAdvanced program uppgradera avsnitt | Microsoft Docs
description: "Den här artikeln beskriver vissa avancerade ämnen gällande tooupgrading ett Service Fabric-program."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar;chackdan
ms.openlocfilehash: bdaf3db6209c574d39f57e0bf9951fad5ad1cbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a><span data-ttu-id="08019-103">Uppgradering av Service Fabric-programmet: avancerade alternativ</span><span class="sxs-lookup"><span data-stu-id="08019-103">Service Fabric application upgrade: advanced topics</span></span>
## <a name="adding-or-removing-services-during-an-application-upgrade"></a><span data-ttu-id="08019-104">Att lägga till eller ta bort tjänster under en uppgradering av programmet</span><span class="sxs-lookup"><span data-stu-id="08019-104">Adding or removing services during an application upgrade</span></span>
<span data-ttu-id="08019-105">Om en ny tjänst läggs tooan program som redan distribuerats och publiceras som en uppgradering, är hello ny tjänst tillagda toohello distribuerade program.</span><span class="sxs-lookup"><span data-stu-id="08019-105">If a new service is added tooan application that is already deployed, and published as an upgrade, hello new service is added toohello deployed application.</span></span>  <span data-ttu-id="08019-106">Sådana uppgradering påverkar inte någon av hello-tjänster som redan fanns hello program.</span><span class="sxs-lookup"><span data-stu-id="08019-106">Such an upgrade does not affect any of hello services that were already part of hello application.</span></span> <span data-ttu-id="08019-107">Men en instans av hello-tjänsten som har lagts till måste startas för hello nya service toobe active (med hjälp av hello `New-ServiceFabricService` cmdlet).</span><span class="sxs-lookup"><span data-stu-id="08019-107">However, an instance of hello service that was added must be started for hello new service toobe active (using hello `New-ServiceFabricService` cmdlet).</span></span>

<span data-ttu-id="08019-108">Tjänster kan också tas bort från ett program som en del av en uppgradering.</span><span class="sxs-lookup"><span data-stu-id="08019-108">Services can also be removed from an application as part of an upgrade.</span></span> <span data-ttu-id="08019-109">Men alla aktuella instanser av hello till ska-bort tjänsten måste stoppas innan du fortsätter med uppgraderingen hello (med hjälp av hello `Remove-ServiceFabricService` cmdlet).</span><span class="sxs-lookup"><span data-stu-id="08019-109">However, all current instances of hello to-be-deleted service must be stopped before proceeding with hello upgrade (using hello `Remove-ServiceFabricService` cmdlet).</span></span>

## <a name="manual-upgrade-mode"></a><span data-ttu-id="08019-110">Manuellt Uppgraderingsläge</span><span class="sxs-lookup"><span data-stu-id="08019-110">Manual upgrade mode</span></span>
> [!NOTE]
> <span data-ttu-id="08019-111">hello oövervakade manuellt läge bör övervägas endast för en misslyckad eller pausat uppgradering.</span><span class="sxs-lookup"><span data-stu-id="08019-111">hello unmonitored manual mode should be considered only for a failed or suspended upgrade.</span></span> <span data-ttu-id="08019-112">hello övervakat läge är hello rekommenderade Uppgraderingsläge för Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="08019-112">hello monitored mode is hello recommended upgrade mode for Service Fabric applications.</span></span>
>
>

<span data-ttu-id="08019-113">Azure Service Fabric innehåller flera uppgradera lägen toosupport kluster för utveckling och produktion.</span><span class="sxs-lookup"><span data-stu-id="08019-113">Azure Service Fabric provides multiple upgrade modes toosupport development and production clusters.</span></span> <span data-ttu-id="08019-114">Distributionsalternativ valt kan vara olika för olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="08019-114">Deployment options chosen may be different for different environments.</span></span>

<span data-ttu-id="08019-115">programmet hello övervakas uppgraderingen är hello vanligaste uppgradera toouse i hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="08019-115">hello monitored rolling application upgrade is hello most typical upgrade toouse in hello production environment.</span></span> <span data-ttu-id="08019-116">När hello uppgraderar princip har angetts, Service Fabric säkerställer att programmet hello är felfri innan hello uppgraderingen fortsätter.</span><span class="sxs-lookup"><span data-stu-id="08019-116">When hello upgrade policy is specified, Service Fabric ensures that hello application is healthy before hello upgrade proceeds.</span></span>

 <span data-ttu-id="08019-117">hello-administratörer kan använda hello manuell rullande program Uppgraderingsläge toohave fullständig kontroll över hello Uppgraderingsförlopp via hello olika uppgraderingsdomäner.</span><span class="sxs-lookup"><span data-stu-id="08019-117">hello application administrator can use hello manual rolling application upgrade mode toohave total control over hello upgrade progress through hello various upgrade domains.</span></span> <span data-ttu-id="08019-118">Det här läget är användbart när en anpassad eller komplexa utvärdering hälsoprincip krävs, eller en icke-konventionella uppgraderingen sker (till exempel hello programmet finns redan i förlust av data).</span><span class="sxs-lookup"><span data-stu-id="08019-118">This mode is useful when a customized or complex health evaluation policy is required, or a non-conventional upgrade is happening (for example, hello application is already in data loss).</span></span>

<span data-ttu-id="08019-119">Slutligen är hello automatiserat program uppgraderingen användbart för utveckling eller tester miljöer tooprovide en snabb iteration växla under utvecklingen av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="08019-119">Finally, hello automated rolling application upgrade is useful for development or testing environments tooprovide a fast iteration cycle during service development.</span></span>

## <a name="change-toomanual-upgrade-mode"></a><span data-ttu-id="08019-120">Ändra toomanual Uppgraderingsläge</span><span class="sxs-lookup"><span data-stu-id="08019-120">Change toomanual upgrade mode</span></span>
<span data-ttu-id="08019-121">**Manuell**--stoppa hello Programuppgradering på hello aktuella UD och ändrar hello uppgradera läge tooUnmonitored manuell.</span><span class="sxs-lookup"><span data-stu-id="08019-121">**Manual**--Stop hello application upgrade at hello current UD and change hello upgrade mode tooUnmonitored Manual.</span></span> <span data-ttu-id="08019-122">Hej administratör måste toomanually anropet **MoveNextApplicationUpgradeDomainAsync** tooproceed med hello uppgradera eller starta en återställning genom att initiera en ny uppgradering.</span><span class="sxs-lookup"><span data-stu-id="08019-122">hello administrator needs toomanually call **MoveNextApplicationUpgradeDomainAsync** tooproceed with hello upgrade or trigger a rollback by initiating a new upgrade.</span></span> <span data-ttu-id="08019-123">När hello uppgraderingen går in i hello manuellt läge, förblir den i hello manuellt läge tills en ny uppgradering initieras.</span><span class="sxs-lookup"><span data-stu-id="08019-123">Once hello upgrade enters into hello Manual mode, it stays in hello Manual mode until a new upgrade is initiated.</span></span> <span data-ttu-id="08019-124">Hej **GetApplicationUpgradeProgressAsync** kommando returnerar FABRIC\_programmet\_uppgradera\_tillstånd\_rullande\_framåt\_väntar.</span><span class="sxs-lookup"><span data-stu-id="08019-124">hello **GetApplicationUpgradeProgressAsync** command returns FABRIC\_APPLICATION\_UPGRADE\_STATE\_ROLLING\_FORWARD\_PENDING.</span></span>

## <a name="upgrade-with-a-diff-package"></a><span data-ttu-id="08019-125">Uppgradera med en diff-paket</span><span class="sxs-lookup"><span data-stu-id="08019-125">Upgrade with a diff package</span></span>
<span data-ttu-id="08019-126">Ett Service Fabric-program kan uppgraderas genom etablering med en fullständig, självständiga programpaket.</span><span class="sxs-lookup"><span data-stu-id="08019-126">A Service Fabric application can be upgraded by provisioning with a full, self-contained application package.</span></span> <span data-ttu-id="08019-127">Ett program kan också uppgraderas med hjälp av en diff-paket som innehåller endast hello uppdateras programfiler, hello uppdateras programmanifestet och hello service manifest-filer.</span><span class="sxs-lookup"><span data-stu-id="08019-127">An application can also be upgraded by using a diff package that contains only hello updated application files, hello updated application manifest, and hello service manifest files.</span></span>

<span data-ttu-id="08019-128">En fullständig programpaketet innehåller alla hello filer nödvändiga toostart och kör ett Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="08019-128">A full application package contains all hello files necessary toostart and run a Service Fabric application.</span></span> <span data-ttu-id="08019-129">En diff-paketet innehåller endast hello-filer som ändrats mellan hello senare bestämmelsen och hello uppgraderingen, plus hello fullständig programmanifestet och hello service manifest filer.</span><span class="sxs-lookup"><span data-stu-id="08019-129">A diff package contains only hello files that changed between hello last provision and hello current upgrade, plus hello full application manifest and hello service manifest files.</span></span> <span data-ttu-id="08019-130">Alla referenser i programmanifestet hello eller service manifest som inte kan hittas i hello build layout sökte efter i hello avbildningsarkivet.</span><span class="sxs-lookup"><span data-stu-id="08019-130">Any reference in hello application manifest or service manifest that can't be found in hello build layout is searched for in hello image store.</span></span>

<span data-ttu-id="08019-131">Fullständig programpaket krävs för hello första installationen av ett program toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="08019-131">Full application packages are required for hello first installation of an application toohello cluster.</span></span> <span data-ttu-id="08019-132">Efterföljande uppdateringar kan vara ett fullständigt programpaket eller en diff-paketet.</span><span class="sxs-lookup"><span data-stu-id="08019-132">Subsequent updates can be either a full application package or a diff package.</span></span>

<span data-ttu-id="08019-133">Tillfällen när du använder en diff-paketet är ett bra alternativ:</span><span class="sxs-lookup"><span data-stu-id="08019-133">Occasions when using a diff package would be a good choice:</span></span>

* <span data-ttu-id="08019-134">En diff-paketet rekommenderas när du har ett stort programpaket som refererar till flera service manifest-filer och/eller flera kod paket, config paket eller data paket.</span><span class="sxs-lookup"><span data-stu-id="08019-134">A diff package is preferred when you have a large application package that references several service manifest files and/or several code packages, config packages, or data packages.</span></span>
* <span data-ttu-id="08019-135">En diff-paketet rekommenderas när du har en distribution som genererar hello build layout direkt från din Byggprocessen.</span><span class="sxs-lookup"><span data-stu-id="08019-135">A diff package is preferred when you have a deployment system that generates hello build layout directly from your application build process.</span></span> <span data-ttu-id="08019-136">Även om hello koden inte har ändrats hämta nyligen inbyggd sammansättningar i det här fallet en annan kontrollsumma.</span><span class="sxs-lookup"><span data-stu-id="08019-136">In this case, even though hello code hasn't changed, newly built assemblies get a different checksum.</span></span> <span data-ttu-id="08019-137">Med hjälp av en fullständig programpaket skulle kräva du tooupdate hello-versionen på alla kod paket.</span><span class="sxs-lookup"><span data-stu-id="08019-137">Using a full application package would require you tooupdate hello version on all code packages.</span></span> <span data-ttu-id="08019-138">Använder en diff-paketet ange du bara hello-filer som ändras och hello manifestfiler där hello version har ändrats.</span><span class="sxs-lookup"><span data-stu-id="08019-138">Using a diff package, you only provide hello files that changed and hello manifest files where hello version has changed.</span></span>

<span data-ttu-id="08019-139">När ett program uppgraderas med Visual Studio, publiceras hello diff-paketet automatiskt.</span><span class="sxs-lookup"><span data-stu-id="08019-139">When an application is upgraded using Visual Studio, hello diff package is published automatically.</span></span> <span data-ttu-id="08019-140">toocreate en diff-paketet hello manuellt programmanifestet, och hello service manifest måste uppdateras, men endast hello ändras paket ska tas med i hello slutliga programpaket.</span><span class="sxs-lookup"><span data-stu-id="08019-140">toocreate a diff package manually, hello application manifest, and hello service manifests must be updated, but only hello changed packages should be included in hello final application package.</span></span>

<span data-ttu-id="08019-141">Till exempel börja med hello följande program (versionsnummer för enkel förstå):</span><span class="sxs-lookup"><span data-stu-id="08019-141">For example, let's start with hello following application (version numbers provided for ease of understanding):</span></span>

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

<span data-ttu-id="08019-142">Nu antar vi att du vill ha tooupdate endast hello kodpaketet av service1 använder en diff-paketet med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08019-142">Now, let's assume you wanted tooupdate only hello code package of service1 using a diff package using PowerShell.</span></span> <span data-ttu-id="08019-143">Nu har uppdaterade programmet hello följande mappstrukturen:</span><span class="sxs-lookup"><span data-stu-id="08019-143">Now, your updated application has hello following folder structure:</span></span>

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

<span data-ttu-id="08019-144">I så fall måste uppdatera du hello application manifest too2.0.0 och hello service manifest för uppdatering av service1 tooreflect hello paketet.</span><span class="sxs-lookup"><span data-stu-id="08019-144">In this case, you update hello application manifest too2.0.0, and hello service manifest for service1 tooreflect hello code package update.</span></span> <span data-ttu-id="08019-145">hello mapp för ditt programpaket skulle ha hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="08019-145">hello folder for your application package would have hello following structure:</span></span>

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a><span data-ttu-id="08019-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="08019-146">Next steps</span></span>
<span data-ttu-id="08019-147">[Uppgradera ditt program med hjälp av Visual Studio](service-fabric-application-upgrade-tutorial.md) vägleder dig genom en uppgradering av programmet med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="08019-147">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>

<span data-ttu-id="08019-148">[Uppgradera ditt program med hjälp av Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vägleder dig genom en uppgradering av programmet med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08019-148">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>

<span data-ttu-id="08019-149">Styra hur programmet ska uppgraderas med hjälp av [uppgradera parametrar](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="08019-149">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>

<span data-ttu-id="08019-150">Gör din programuppgraderingar kompatibla genom att lära dig hur toouse [dataserialisering](service-fabric-application-upgrade-data-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="08019-150">Make your application upgrades compatible by learning how toouse [Data Serialization](service-fabric-application-upgrade-data-serialization.md).</span></span>

<span data-ttu-id="08019-151">Lösa vanliga problem i programuppgraderingar genom att referera toohello stegen i [felsökning programuppgraderingar](service-fabric-application-upgrade-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="08019-151">Fix common problems in application upgrades by referring toohello steps in [Troubleshooting Application Upgrades](service-fabric-application-upgrade-troubleshooting.md).</span></span>
