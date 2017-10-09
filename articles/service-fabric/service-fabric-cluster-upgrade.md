---
title: aaaUpgrade ett Azure Service Fabric-kluster | Microsoft Docs
description: "Uppgradera hello Service Fabric-kod och/eller konfiguration som kör ett Service Fabric-kluster, inklusive inställning klustret uppdateringsläge, uppgradera certifikat, lägga till programmet portar, göra operativsystemskorrigeringar, och så vidare. Vad kan du förvänta dig när hello uppgraderingar utförs?"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/10/2017
ms.author: chackdan
ms.openlocfilehash: 94ac3833ec0810f79de06ecb50f254028fa90408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a><span data-ttu-id="57106-104">Uppgradera en Azure Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="57106-104">Upgrade an Azure Service Fabric cluster</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="57106-105">Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="57106-105">Azure Cluster</span></span>](service-fabric-cluster-upgrade.md)
> * [<span data-ttu-id="57106-106">Fristående kluster</span><span class="sxs-lookup"><span data-stu-id="57106-106">Standalone Cluster</span></span>](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

<span data-ttu-id="57106-107">För alla moderna system är designa för möjligheterna viktiga tooachieving långsiktig framgång för en produkt.</span><span class="sxs-lookup"><span data-stu-id="57106-107">For any modern system, designing for upgradability is key tooachieving long-term success of your product.</span></span> <span data-ttu-id="57106-108">Ett Azure Service Fabric-kluster är en resurs som du äger, men delvis hanteras av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="57106-108">An Azure Service Fabric cluster is a resource that you own, but is partly managed by Microsoft.</span></span> <span data-ttu-id="57106-109">Den här artikeln beskriver vad hanteras automatiskt och vad du kan konfigurera själv.</span><span class="sxs-lookup"><span data-stu-id="57106-109">This article describes what is managed automatically and what you can configure yourself.</span></span>

## <a name="controlling-hello-fabric-version-that-runs-on-your-cluster"></a><span data-ttu-id="57106-110">Kontrollera hello fabric-versionen som körs på klustret</span><span class="sxs-lookup"><span data-stu-id="57106-110">Controlling hello fabric version that runs on your Cluster</span></span>
<span data-ttu-id="57106-111">Du kan ange ditt kluster tooreceive automatisk fabric uppgraderingar som Microsoft släpper eller så kan du välja en stöds fabric-versionen som du vill använda ditt kluster toobe på.</span><span class="sxs-lookup"><span data-stu-id="57106-111">You can set your cluster tooreceive automatic fabric upgrades as they are released by Microsoft or you can select a supported fabric version you want your cluster toobe on.</span></span>

<span data-ttu-id="57106-112">Det gör du genom inställningen hello ”upgradeMode” klusterkonfigurationen på hello-portalen eller med hjälp av Resource Manager för närvarande hello skapas eller senare på ett aktivt kluster</span><span class="sxs-lookup"><span data-stu-id="57106-112">You do this by setting hello "upgradeMode" cluster configuration on hello portal or using Resource Manager at hello time of creation or later on a live cluster</span></span> 

> [!NOTE]
> <span data-ttu-id="57106-113">Se till att tookeep ditt kluster som kör en version stöds fabric alltid.</span><span class="sxs-lookup"><span data-stu-id="57106-113">Make sure tookeep your cluster running a supported fabric version always.</span></span> <span data-ttu-id="57106-114">Och när vi meddela hello-versionen av en ny version av service fabric, har hello tidigare version markerats för slutet av stödet efter minst 60 dagar från det datumet.</span><span class="sxs-lookup"><span data-stu-id="57106-114">As and when we announce hello release of a new version of service fabric, hello previous version is marked for end of support after a minimum of 60 days from that date.</span></span> <span data-ttu-id="57106-115">hello hello nya versioner tillkännages [på hello service fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric/).</span><span class="sxs-lookup"><span data-stu-id="57106-115">hello  hello new releases are announced [on hello service fabric team blog](https://blogs.msdn.microsoft.com/azureservicefabric/).</span></span> <span data-ttu-id="57106-116">hello nya versionen är sedan tillgängliga toochoose.</span><span class="sxs-lookup"><span data-stu-id="57106-116">hello new release is available toochoose then.</span></span> 
> 
> 

<span data-ttu-id="57106-117">14 dagar tidigare toohello utgången av hello versionen klustret körs, skapas en hälsohändelse som placerar klustret i ett varningshälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="57106-117">14 days prior toohello expiry of hello release your cluster is running, a health event is generated that puts your cluster into a warning health state.</span></span> <span data-ttu-id="57106-118">hello klustret förblir i ett varningstillstånd förrän du har uppgraderat tooa stöds fabric-versionen.</span><span class="sxs-lookup"><span data-stu-id="57106-118">hello cluster remains in a warning state until you upgrade tooa supported fabric version.</span></span>

### <a name="setting-hello-upgrade-mode-via-portal"></a><span data-ttu-id="57106-119">Inställningen hello Uppgraderingsläge via portalen</span><span class="sxs-lookup"><span data-stu-id="57106-119">Setting hello upgrade mode via portal</span></span>
<span data-ttu-id="57106-120">Du kan ange hello klustret tooautomatic eller manuellt när du skapar hello klustret.</span><span class="sxs-lookup"><span data-stu-id="57106-120">You can set hello cluster tooautomatic or manual when you are creating hello cluster.</span></span>

![Create_Manualmode][Create_Manualmode]

<span data-ttu-id="57106-122">Du kan ange hello klustret tooautomatic eller manuell på ett aktivt kluster, med hjälp av hello hantera upplevelse.</span><span class="sxs-lookup"><span data-stu-id="57106-122">You can set hello cluster tooautomatic or manual when on a live cluster, using hello manage experience.</span></span> 

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-portal"></a><span data-ttu-id="57106-123">Uppgradera tooa nya versionen på ett kluster som har angetts tooManual läge via portalen.</span><span class="sxs-lookup"><span data-stu-id="57106-123">Upgrading tooa new version on a cluster that is set tooManual mode via portal.</span></span>
<span data-ttu-id="57106-124">tooupgrade tooa ny version, allt du behöver toodo är Välj hello tillgängliga versionen hello listrutan och spara.</span><span class="sxs-lookup"><span data-stu-id="57106-124">tooupgrade tooa new version, all you need toodo is select hello available version from hello dropdown and save.</span></span> <span data-ttu-id="57106-125">hello Fabric-uppgraderingen hämtar inletts automatiskt.</span><span class="sxs-lookup"><span data-stu-id="57106-125">hello Fabric upgrade gets kicked off automatically.</span></span> <span data-ttu-id="57106-126">hello klustret hälsoprinciper (en kombination av noden hälso- och hello alla hello program som körs i klustret hello) följs tooduring hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="57106-126">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered tooduring hello upgrade.</span></span>

<span data-ttu-id="57106-127">Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="57106-127">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="57106-128">Rulla ned det här dokumentet tooread mer om hur tooset dessa anpassade hälsoprinciper.</span><span class="sxs-lookup"><span data-stu-id="57106-128">Scroll down this document tooread more on how tooset those custom health policies.</span></span> 

<span data-ttu-id="57106-129">När du har åtgärdat hello problem som resulterade i hello återställning måste tooinitiate hello uppgraderingen igen, med följande hello samma steg som tidigare.</span><span class="sxs-lookup"><span data-stu-id="57106-129">Once you have fixed hello issues that resulted in hello rollback, you need tooinitiate hello upgrade again, by following hello same steps as before.</span></span>

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-hello-upgrade-mode-via-a-resource-manager-template"></a><span data-ttu-id="57106-131">Inställningen hello Uppgraderingsläge via en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="57106-131">Setting hello upgrade mode via a Resource Manager template</span></span>
<span data-ttu-id="57106-132">Lägg till hello ”upgradeMode” configuration toohello Microsoft.ServiceFabric/clusters resursdefinitionen och ange hello ”clusterCodeVersion” tooone av hello fabric-versioner som stöds som visas nedan och sedan distribuera hello mallen.</span><span class="sxs-lookup"><span data-stu-id="57106-132">Add hello "upgradeMode" configuration toohello Microsoft.ServiceFabric/clusters resource definition and set hello "clusterCodeVersion" tooone of hello supported fabric versions as shown below and then deploy hello template.</span></span> <span data-ttu-id="57106-133">hello giltiga värden för ”upgradeMode” är ”manuell” eller ”automatisk”</span><span class="sxs-lookup"><span data-stu-id="57106-133">hello valid values for "upgradeMode" are "Manual" or "Automatic"</span></span>

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-a-resource-manager-template"></a><span data-ttu-id="57106-135">Uppgradera tooa nya versionen på ett kluster som har angetts tooManual läge via en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="57106-135">Upgrading tooa new version on a cluster that is set tooManual mode via a Resource Manager template.</span></span>
<span data-ttu-id="57106-136">När hello klustret är i manuellt läge måste tooupgrade tooa ny version, ändra hello ”clusterCodeVersion” tooa-version som stöds och distribuerar den.</span><span class="sxs-lookup"><span data-stu-id="57106-136">When hello cluster is in Manual mode, tooupgrade tooa new version, change hello "clusterCodeVersion" tooa supported version and deploy it.</span></span> <span data-ttu-id="57106-137">hello distribution av hello mallen, sparkar av hello Fabric-uppgraderingen hämtar inletts automatiskt.</span><span class="sxs-lookup"><span data-stu-id="57106-137">hello deployment of hello template, kicks of hello Fabric upgrade gets kicked off automatically.</span></span> <span data-ttu-id="57106-138">hello klustret hälsoprinciper (en kombination av noden hälso- och hello alla hello program som körs i klustret hello) följs tooduring hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="57106-138">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered tooduring hello upgrade.</span></span>

<span data-ttu-id="57106-139">Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="57106-139">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="57106-140">Rulla ned det här dokumentet tooread mer om hur tooset dessa anpassade hälsoprinciper.</span><span class="sxs-lookup"><span data-stu-id="57106-140">Scroll down this document tooread more on how tooset those custom health policies.</span></span> 

<span data-ttu-id="57106-141">När du har åtgärdat hello problem som resulterade i hello återställning måste tooinitiate hello uppgraderingen igen, med följande hello samma steg som tidigare.</span><span class="sxs-lookup"><span data-stu-id="57106-141">Once you have fixed hello issues that resulted in hello rollback, you need tooinitiate hello upgrade again, by following hello same steps as before.</span></span>

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a><span data-ttu-id="57106-142">Hämta en lista över alla tillgängliga versionen för alla miljöer för en viss prenumeration</span><span class="sxs-lookup"><span data-stu-id="57106-142">Get list of all available version for all environments for a given subscription</span></span>
<span data-ttu-id="57106-143">Kör hello följande kommando och du bör få en liknande toothis utdata.</span><span class="sxs-lookup"><span data-stu-id="57106-143">Run hello following command, and you should get an output similar toothis.</span></span>

<span data-ttu-id="57106-144">”supportExpiryUtc” talar om när en viss version upphör att gälla eller har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="57106-144">"supportExpiryUtc" tells your when a given release is expiring or has expired.</span></span> <span data-ttu-id="57106-145">hello senaste versionen har inte ett giltigt datum - har ett värde på ”9999-12-31T23:59:59.9999999”, vilket innebär bara att hello förfallodatum inte ännu har angetts.</span><span class="sxs-lookup"><span data-stu-id="57106-145">hello latest release does not have a valid date - it has a value of "9999-12-31T23:59:59.9999999", which just means that hello expiry date is not yet set.</span></span>

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/locations/{{location}}/clusterVersions?api-version=2016-09-01

Example: https://management.azure.com/subscriptions/1857f442-3bce-4b96-ad95-627f76437a67/providers/Microsoft.ServiceFabric/locations/eastus/clusterVersions?api-version=2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-hello-cluster-upgrade-mode-is-automatic"></a><span data-ttu-id="57106-146">Fabric-uppgraderingar när hello klustret uppgradera läge är automatisk</span><span class="sxs-lookup"><span data-stu-id="57106-146">Fabric upgrade behavior when hello cluster Upgrade Mode is Automatic</span></span>
<span data-ttu-id="57106-147">Microsoft har hello fabric koden och konfigurationen som körs i ett Azure-kluster.</span><span class="sxs-lookup"><span data-stu-id="57106-147">Microsoft maintains hello fabric code and configuration that runs in an Azure cluster.</span></span> <span data-ttu-id="57106-148">Vi utföra automatiska övervakade uppgraderingar toohello programvara som det behövs.</span><span class="sxs-lookup"><span data-stu-id="57106-148">We perform automatic monitored upgrades toohello software on an as-needed basis.</span></span> <span data-ttu-id="57106-149">Uppgraderingarna kan vara koden, konfiguration, eller båda.</span><span class="sxs-lookup"><span data-stu-id="57106-149">These upgrades could be code, configuration, or both.</span></span> <span data-ttu-id="57106-150">toomake till att ditt program lider ingen påverkan eller minimal påverkan på grund av toothese uppgraderingar, vi utföra hello uppgraderingar i hello följande faser:</span><span class="sxs-lookup"><span data-stu-id="57106-150">toomake sure that your application suffers no impact or minimal impact due toothese upgrades, we perform hello upgrades in hello following phases:</span></span>

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a><span data-ttu-id="57106-151">Fas 1: En uppgradering utförs med hjälp av alla hälsoprinciper för kluster</span><span class="sxs-lookup"><span data-stu-id="57106-151">Phase 1: An upgrade is performed by using all cluster health policies</span></span>
<span data-ttu-id="57106-152">Under den här fasen hello uppgraderingar fortsätter en domän i taget och hello-program som kördes i hello kluster fortsätter toorun utan driftavbrott.</span><span class="sxs-lookup"><span data-stu-id="57106-152">During this phase, hello upgrades proceed one upgrade domain at a time, and hello applications that were running in hello cluster continue toorun without any downtime.</span></span> <span data-ttu-id="57106-153">hello klustret hälsoprinciper (en kombination av noden hälso- och hello alla hello program som körs i klustret hello) följs tooduring hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="57106-153">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered tooduring hello upgrade.</span></span>

<span data-ttu-id="57106-154">Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="57106-154">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="57106-155">Ett e-postmeddelande skickas sedan toohello ägare hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="57106-155">Then an email is sent toohello owner of hello subscription.</span></span> <span data-ttu-id="57106-156">hello e-post innehåller hello följande information:</span><span class="sxs-lookup"><span data-stu-id="57106-156">hello email contains hello following information:</span></span>

* <span data-ttu-id="57106-157">Meddelande om att vi hade tooroll tillbaka en uppgradering av klustret.</span><span class="sxs-lookup"><span data-stu-id="57106-157">Notification that we had tooroll back a cluster upgrade.</span></span>
* <span data-ttu-id="57106-158">Föreslagna vidtar åtgärder, om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="57106-158">Suggested remedial actions, if any.</span></span>
* <span data-ttu-id="57106-159">hello antal dagar (n) förrän vi köra fas 2.</span><span class="sxs-lookup"><span data-stu-id="57106-159">hello number of days (n) until we execute Phase 2.</span></span>

<span data-ttu-id="57106-160">Vi försök tooexecute hello samma uppgradera några gånger om eventuella uppgraderingar misslyckades av orsaker som infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="57106-160">We try tooexecute hello same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="57106-161">Efter hello n dagar från hello datum hello e-postmeddelande har skickats, vi fortsätta tooPhase 2.</span><span class="sxs-lookup"><span data-stu-id="57106-161">After hello n days from hello date hello email was sent, we proceed tooPhase 2.</span></span>

<span data-ttu-id="57106-162">Om hello klustret hälsoprinciper uppfylls som genomförd hello uppgradering och markerat som slutförda.</span><span class="sxs-lookup"><span data-stu-id="57106-162">If hello cluster health policies are met, hello upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="57106-163">Detta kan inträffa under hello inledande uppgradering eller någon av hello uppgradera repriser i det här steget.</span><span class="sxs-lookup"><span data-stu-id="57106-163">This can happen during hello initial upgrade or any of hello upgrade reruns in this phase.</span></span> <span data-ttu-id="57106-164">Det finns ingen e-bekräftelse av en lyckad körning.</span><span class="sxs-lookup"><span data-stu-id="57106-164">There is no email confirmation of a successful run.</span></span> <span data-ttu-id="57106-165">Detta är tooavoid skickar du för många e-postmeddelanden--en e-postmeddelanden ska ses som ett undantag toonormal.</span><span class="sxs-lookup"><span data-stu-id="57106-165">This is tooavoid sending you too many emails--receiving an email should be seen as an exception toonormal.</span></span> <span data-ttu-id="57106-166">De flesta av hello klustret uppgraderingar toosucceed planerar vi för utan att påverka programmets tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="57106-166">We expect most of hello cluster upgrades toosucceed without impacting your application availability.</span></span>

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a><span data-ttu-id="57106-167">Fas 2: En uppgradering utförs med hjälp av standard hälsoprinciper endast</span><span class="sxs-lookup"><span data-stu-id="57106-167">Phase 2: An upgrade is performed by using default health policies only</span></span>
<span data-ttu-id="57106-168">hello hälsoprinciper i det här steget är inställda så att hello antalet program som har felfri hello början av hello uppgraderingen förblir hello samma för hello varaktighet hello uppgraderingsprocessen.</span><span class="sxs-lookup"><span data-stu-id="57106-168">hello health policies in this phase are set in such a way that hello number of applications that were healthy at hello beginning of hello upgrade remains hello same for hello duration of hello upgrade process.</span></span> <span data-ttu-id="57106-169">Som i fas 1 hello fas 2 uppgraderingar fortsätter en domän i taget och hello program som kördes i hello kluster fortsätter toorun utan driftavbrott.</span><span class="sxs-lookup"><span data-stu-id="57106-169">As in Phase 1, hello Phase 2 upgrades proceed one upgrade domain at a time, and hello applications that were running in hello cluster continue toorun without any downtime.</span></span> <span data-ttu-id="57106-170">hello klustret hälsoprinciper (en kombination av noden hälso- och hello alla hello program som körs i klustret hello) är följt toofor hello varaktighet hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="57106-170">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered toofor hello duration of hello upgrade.</span></span>

<span data-ttu-id="57106-171">Hello uppgraderingen återställs om hello klustret hälsoprinciper gäller inte uppfylls.</span><span class="sxs-lookup"><span data-stu-id="57106-171">If hello cluster health policies in effect are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="57106-172">Ett e-postmeddelande skickas sedan toohello ägare hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="57106-172">Then an email is sent toohello owner of hello subscription.</span></span> <span data-ttu-id="57106-173">hello e-post innehåller hello följande information:</span><span class="sxs-lookup"><span data-stu-id="57106-173">hello email contains hello following information:</span></span>

* <span data-ttu-id="57106-174">Meddelande om att vi hade tooroll tillbaka en uppgradering av klustret.</span><span class="sxs-lookup"><span data-stu-id="57106-174">Notification that we had tooroll back a cluster upgrade.</span></span>
* <span data-ttu-id="57106-175">Föreslagna vidtar åtgärder, om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="57106-175">Suggested remedial actions, if any.</span></span>
* <span data-ttu-id="57106-176">hello antal dagar (n) förrän vi köra fas 3.</span><span class="sxs-lookup"><span data-stu-id="57106-176">hello number of days (n) until we execute Phase 3.</span></span>

<span data-ttu-id="57106-177">Vi försök tooexecute hello samma uppgradera några gånger om eventuella uppgraderingar misslyckades av orsaker som infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="57106-177">We try tooexecute hello same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="57106-178">En påminnelse om e-postmeddelande skickas ett par dagar innan n dagar är igång.</span><span class="sxs-lookup"><span data-stu-id="57106-178">A reminder email is sent a couple of days before n days are up.</span></span> <span data-ttu-id="57106-179">Efter hello n dagar från hello datum hello e-postmeddelande har skickats, vi fortsätta tooPhase 3.</span><span class="sxs-lookup"><span data-stu-id="57106-179">After hello n days from hello date hello email was sent, we proceed tooPhase 3.</span></span> <span data-ttu-id="57106-180">hello e-postmeddelanden som vi skickar dig i fas 2 måste du vidta allvarligt och vidtar åtgärder vidtas.</span><span class="sxs-lookup"><span data-stu-id="57106-180">hello emails we send you in Phase 2 must be taken seriously and remedial actions must be taken.</span></span>

<span data-ttu-id="57106-181">Om hello klustret hälsoprinciper uppfylls som genomförd hello uppgradering och markerat som slutförda.</span><span class="sxs-lookup"><span data-stu-id="57106-181">If hello cluster health policies are met, hello upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="57106-182">Detta kan inträffa under hello inledande uppgradering eller någon av hello uppgradera repriser i det här steget.</span><span class="sxs-lookup"><span data-stu-id="57106-182">This can happen during hello initial upgrade or any of hello upgrade reruns in this phase.</span></span> <span data-ttu-id="57106-183">Det finns ingen e-bekräftelse av en lyckad körning.</span><span class="sxs-lookup"><span data-stu-id="57106-183">There is no email confirmation of a successful run.</span></span>

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a><span data-ttu-id="57106-184">Fas 3: En uppgradering utförs med hjälp av aggressivt hälsoprinciper</span><span class="sxs-lookup"><span data-stu-id="57106-184">Phase 3: An upgrade is performed by using aggressive health policies</span></span>
<span data-ttu-id="57106-185">Dessa hälsoprinciper i det här steget är inriktad på hello uppgraderingen har slutförts i stället för hello hälsotillståndet hos hello-program.</span><span class="sxs-lookup"><span data-stu-id="57106-185">These health policies in this phase are geared towards completion of hello upgrade rather than hello health of hello applications.</span></span> <span data-ttu-id="57106-186">Mycket få klusteruppgradering hamna i det här steget.</span><span class="sxs-lookup"><span data-stu-id="57106-186">Very few cluster upgrades end up in this phase.</span></span> <span data-ttu-id="57106-187">Om klustret hämtar toothis fasen, är det troligt att programmet blir ohälsosamt och förlora tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="57106-187">If your cluster gets toothis phase, there is a good chance that your application becomes unhealthy and/or lose availability.</span></span>

<span data-ttu-id="57106-188">Liknande toohello andra två faser, fas 3 uppgraderingar fortsätta en domän i taget.</span><span class="sxs-lookup"><span data-stu-id="57106-188">Similar toohello other two phases, Phase 3 upgrades proceed one upgrade domain at a time.</span></span>

<span data-ttu-id="57106-189">Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="57106-189">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="57106-190">Vi försök tooexecute hello samma uppgradera några gånger om eventuella uppgraderingar misslyckades av orsaker som infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="57106-190">We try tooexecute hello same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="57106-191">Efter det fästs hello klustret, så att den får inte längre stöd och/eller uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="57106-191">After that, hello cluster is pinned, so that it will no longer receive support and/or upgrades.</span></span>

<span data-ttu-id="57106-192">Ett e-postmeddelande med informationen skickas toohello prenumerationsägaren tillsammans med hello vidtar åtgärder.</span><span class="sxs-lookup"><span data-stu-id="57106-192">An email with this information is sent toohello subscription owner, along with hello remedial actions.</span></span> <span data-ttu-id="57106-193">Vi förväntar sig inte några kluster tooget i ett tillstånd där fas 3 har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="57106-193">We do not expect any clusters tooget into a state where Phase 3 has failed.</span></span>

<span data-ttu-id="57106-194">Om hello klustret hälsoprinciper uppfylls som genomförd hello uppgradering och markerat som slutförda.</span><span class="sxs-lookup"><span data-stu-id="57106-194">If hello cluster health policies are met, hello upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="57106-195">Detta kan inträffa under hello inledande uppgradering eller någon av hello uppgradera repriser i det här steget.</span><span class="sxs-lookup"><span data-stu-id="57106-195">This can happen during hello initial upgrade or any of hello upgrade reruns in this phase.</span></span> <span data-ttu-id="57106-196">Det finns ingen e-bekräftelse av en lyckad körning.</span><span class="sxs-lookup"><span data-stu-id="57106-196">There is no email confirmation of a successful run.</span></span>

## <a name="cluster-configurations-that-you-control"></a><span data-ttu-id="57106-197">Klusterkonfigurationer som du hanterar</span><span class="sxs-lookup"><span data-stu-id="57106-197">Cluster configurations that you control</span></span>
<span data-ttu-id="57106-198">Dessutom toohello möjlighet tooset hello klustret Uppgraderingsläge, här hello-konfigurationer som du kan ändra på ett aktivt kluster.</span><span class="sxs-lookup"><span data-stu-id="57106-198">In addition toohello ability tooset hello cluster upgrade mode, Here are hello configurations that you can change on a live cluster.</span></span>

### <a name="certificates"></a><span data-ttu-id="57106-199">Certifikat</span><span class="sxs-lookup"><span data-stu-id="57106-199">Certificates</span></span>
<span data-ttu-id="57106-200">Du kan lägga till nya eller ta bort certifikat för hello kluster och klienten via hello portal enkelt.</span><span class="sxs-lookup"><span data-stu-id="57106-200">You can add new or delete certificates for hello cluster and client via hello portal easily.</span></span> <span data-ttu-id="57106-201">Se för[det här dokumentet för detaljerade anvisningar](service-fabric-cluster-security-update-certs-azure.md)</span><span class="sxs-lookup"><span data-stu-id="57106-201">Refer too[this document for detailed instructions](service-fabric-cluster-security-update-certs-azure.md)</span></span>

![Skärmbild som visar certifikattumavtryck i hello Azure-portalen.][CertificateUpgrade]

### <a name="application-ports"></a><span data-ttu-id="57106-203">Programmet portar</span><span class="sxs-lookup"><span data-stu-id="57106-203">Application ports</span></span>
<span data-ttu-id="57106-204">Du kan ändra program portar genom att ändra hello belastningsutjämnaren resursegenskaper som är associerade med hello nodtypen.</span><span class="sxs-lookup"><span data-stu-id="57106-204">You can change application ports by changing hello Load Balancer resource properties that are associated with hello node type.</span></span> <span data-ttu-id="57106-205">Du kan använda hello portalen eller använda Resource Manager PowerShell direkt.</span><span class="sxs-lookup"><span data-stu-id="57106-205">You can use hello portal, or you can use Resource Manager PowerShell directly.</span></span>

<span data-ttu-id="57106-206">tooopen en ny port på alla virtuella datorer i en nodtyp hello följande:</span><span class="sxs-lookup"><span data-stu-id="57106-206">tooopen a new port on all VMs in a node type, do hello following:</span></span>

1. <span data-ttu-id="57106-207">Lägg till en ny avsökning toohello lämplig belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="57106-207">Add a new probe toohello appropriate load balancer.</span></span>
   
    <span data-ttu-id="57106-208">Om du har distribuerat ditt kluster med hjälp av hello portal hello belastningsutjämnare namnges ”LB-namnet på hello Resource group-NodeTypename”, en för varje nodtyp.</span><span class="sxs-lookup"><span data-stu-id="57106-208">If you deployed your cluster by using hello portal, hello load balancers are named "LB-name of hello Resource group-NodeTypename", one for each node type.</span></span> <span data-ttu-id="57106-209">Eftersom hello belastningen belastningsutjämnaren namnen är unika endast inom en resursgrupp, är det bäst om du söker efter dem under en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="57106-209">Since hello load balancer names are unique only within a resource group, it is best if you search for them under a specific resource group.</span></span>
   
    ![Skärmbild som visar hur du lägger till en avsökning tooa belastningsutjämnare i hello-portalen.][AddingProbes]
2. <span data-ttu-id="57106-211">Lägga till en ny regel toohello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="57106-211">Add a new rule toohello load balancer.</span></span>
   
    <span data-ttu-id="57106-212">Lägg till en ny regel toohello samma belastningsutjämning med hjälp av hello-avsökningen som du skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="57106-212">Add a new rule toohello same load balancer by using hello probe that you created in hello previous step.</span></span>
   
    ![Lägger till en ny regel tooa belastningsutjämnare i hello-portalen.][AddingLBRules]

### <a name="placement-properties"></a><span data-ttu-id="57106-214">Placeringsegenskaper</span><span class="sxs-lookup"><span data-stu-id="57106-214">Placement properties</span></span>
<span data-ttu-id="57106-215">För varje hello nodtyper, kan du lägga till anpassade placeringsegenskaper som du vill toouse i dina program.</span><span class="sxs-lookup"><span data-stu-id="57106-215">For each of hello node types, you can add custom placement properties that you want toouse in your applications.</span></span> <span data-ttu-id="57106-216">NodeType är en standardegenskap som du kan använda utan att lägga till det explicit.</span><span class="sxs-lookup"><span data-stu-id="57106-216">NodeType is a default property that you can use without adding it explicitly.</span></span>

> [!NOTE]
> <span data-ttu-id="57106-217">Mer information om hello användning av placeringen nodegenskaper, och hur toodefine dem, finns toohello ”begränsningar och noden placeringsegenskaper” i avsnittet hello resurshanteraren för Service Fabric-klustret dokumentet på [som beskriver ditt kluster ](service-fabric-cluster-resource-manager-cluster-description.md).</span><span class="sxs-lookup"><span data-stu-id="57106-217">For details on hello use of placement constraints, node properties, and how toodefine them, refer toohello section "Placement Constraints and Node Properties" in hello Service Fabric Cluster Resource Manager Document on [Describing Your Cluster](service-fabric-cluster-resource-manager-cluster-description.md).</span></span>
> 
> 

### <a name="capacity-metrics"></a><span data-ttu-id="57106-218">Kapacitet mått</span><span class="sxs-lookup"><span data-stu-id="57106-218">Capacity metrics</span></span>
<span data-ttu-id="57106-219">Du kan lägga till anpassade kapacitetsmått som du vill toouse i ditt program tooreport belastning för varje hello nodtyper.</span><span class="sxs-lookup"><span data-stu-id="57106-219">For each of hello node types, you can add custom capacity metrics that you want toouse in your applications tooreport load.</span></span> <span data-ttu-id="57106-220">Mer information om hello användning av kapacitet mått tooreport finns toohello resurshanteraren för Service Fabric-klustret dokument på [som beskriver ditt kluster](service-fabric-cluster-resource-manager-cluster-description.md) och [mått och Läs in](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="57106-220">For details on hello use of capacity metrics tooreport load, refer toohello Service Fabric Cluster Resource Manager Documents on [Describing Your Cluster](service-fabric-cluster-resource-manager-cluster-description.md) and [Metrics and Load](service-fabric-cluster-resource-manager-metrics.md).</span></span>

### <a name="fabric-upgrade-settings---health-polices"></a><span data-ttu-id="57106-221">Inställningar för fabric - hälsoprinciper</span><span class="sxs-lookup"><span data-stu-id="57106-221">Fabric upgrade settings - Health polices</span></span>
<span data-ttu-id="57106-222">Du kan ange anpassade hälsoprinciper för fabric-uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="57106-222">You can specify custom health polices for fabric upgrade.</span></span> <span data-ttu-id="57106-223">Om du har angett ditt kluster tooAutomatic fabric uppgraderingar, hämta tillämpade toohello fas 1 hello automatisk fabric uppgraderas med dessa principer.</span><span class="sxs-lookup"><span data-stu-id="57106-223">If you have set your cluster tooAutomatic fabric upgrades, then these policies get applied toohello Phase-1 of hello automatic fabric upgrades.</span></span>
<span data-ttu-id="57106-224">Om du har angett ditt kluster för manuell fabric uppgraderingar, och dessa principer tillämpas varje gång väljer du en ny version utlösa hello system tookick av hello fabric-uppgraderingen i klustret.</span><span class="sxs-lookup"><span data-stu-id="57106-224">If you have set your cluster for Manual fabric upgrades, then these policies get applied each time you select a new version triggering hello system tookick off hello fabric upgrade in your cluster.</span></span> <span data-ttu-id="57106-225">Om du inte åsidosätter hello principer används hello standardinställningar.</span><span class="sxs-lookup"><span data-stu-id="57106-225">If you do not override hello policies, hello defaults are used.</span></span>

<span data-ttu-id="57106-226">Du kan ange hello anpassade hälsoprinciper eller granska hello aktuella inställningar under hello ”fabric-uppgraderingen” bladet genom att välja hello avancerade inställningar för uppgradering.</span><span class="sxs-lookup"><span data-stu-id="57106-226">You can specify hello custom health policies or review hello current settings under hello "fabric upgrade" blade, by selecting hello advanced upgrade settings.</span></span> <span data-ttu-id="57106-227">Granska hello följande bild på hur du.</span><span class="sxs-lookup"><span data-stu-id="57106-227">Review hello following picture on how to.</span></span> 

![Hantera anpassade hälsoprinciper][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a><span data-ttu-id="57106-229">Anpassa infrastrukturinställningarna för klustret</span><span class="sxs-lookup"><span data-stu-id="57106-229">Customize Fabric settings for your cluster</span></span>
<span data-ttu-id="57106-230">Se för[service fabric-kluster infrastrukturinställningarna](service-fabric-cluster-fabric-settings.md) på vad och hur du kan anpassa dem.</span><span class="sxs-lookup"><span data-stu-id="57106-230">Refer too[service fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md) on what and how you can customize them.</span></span>

### <a name="os-patches-on-hello-vms-that-make-up-hello-cluster"></a><span data-ttu-id="57106-231">Operativsystemskorrigeringar på hello virtuella datorer som utgör hello kluster</span><span class="sxs-lookup"><span data-stu-id="57106-231">OS patches on hello VMs that make up hello cluster</span></span>
<span data-ttu-id="57106-232">Se för[korrigering Orchestration programmet](service-fabric-patch-orchestration-application.md) som kan distribueras på ditt kluster tooinstall korrigeringsfiler från Windows Update på ett framförhållning sätt att hålla hello-tjänster som är tillgängliga alla hello tid.</span><span class="sxs-lookup"><span data-stu-id="57106-232">Refer too[Patch Orchestration Application](service-fabric-patch-orchestration-application.md) which can be deployed on your cluster tooinstall patches from Windows Update in an orchestrated manner, keeping hello services available all hello time.</span></span> 

### <a name="os-upgrades-on-hello-vms-that-make-up-hello-cluster"></a><span data-ttu-id="57106-233">OS-uppgraderingar på hello virtuella datorer som utgör hello kluster</span><span class="sxs-lookup"><span data-stu-id="57106-233">OS upgrades on hello VMs that make up hello cluster</span></span>
<span data-ttu-id="57106-234">Om du måste uppgradera hello OS-avbildningen på hello virtuella datorer i hello kluster, måste du göra det en virtuell dator i taget.</span><span class="sxs-lookup"><span data-stu-id="57106-234">If you must upgrade hello OS image on hello virtual machines of hello cluster, you must do it one VM at a time.</span></span> <span data-ttu-id="57106-235">Du ansvarar för den här uppgraderingen – det finns för närvarande ingen automatisering för det här.</span><span class="sxs-lookup"><span data-stu-id="57106-235">You are responsible for this upgrade--there is currently no automation for this.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57106-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57106-236">Next steps</span></span>
* <span data-ttu-id="57106-237">Lär dig hur toocustomize vissa hello [tjänstinställningar fabric-kluster fabric](service-fabric-cluster-fabric-settings.md)</span><span class="sxs-lookup"><span data-stu-id="57106-237">Learn how toocustomize some of hello [service fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md)</span></span>
* <span data-ttu-id="57106-238">Lär dig hur för[skala ditt kluster in och ut](service-fabric-cluster-scale-up-down.md)</span><span class="sxs-lookup"><span data-stu-id="57106-238">Learn how too[scale your cluster in and out](service-fabric-cluster-scale-up-down.md)</span></span>
* <span data-ttu-id="57106-239">Lär dig mer om [programuppgraderingar](service-fabric-application-upgrade.md)</span><span class="sxs-lookup"><span data-stu-id="57106-239">Learn about [application upgrades](service-fabric-application-upgrade.md)</span></span>

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
