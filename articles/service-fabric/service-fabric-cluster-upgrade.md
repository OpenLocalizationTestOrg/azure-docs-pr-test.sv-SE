---
title: Uppgradera en Azure Service Fabric-kluster | Microsoft Docs
description: "Uppgradera Service Fabric-kod och/eller konfiguration som kör ett Service Fabric-kluster, inklusive inställning klustret uppdateringsläge, uppgradera certifikat, lägga till programmet portar, göra operativsystemskorrigeringar, och så vidare. Vad kan du förvänta dig när uppgraderingen utförs?"
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
ms.openlocfilehash: 7ea71ab891583c51b3c07a4d0a9f0b4f54e56669
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a><span data-ttu-id="7ad43-104">Uppgradera en Azure Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="7ad43-104">Upgrade an Azure Service Fabric cluster</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7ad43-105">Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="7ad43-105">Azure Cluster</span></span>](service-fabric-cluster-upgrade.md)
> * [<span data-ttu-id="7ad43-106">Fristående kluster</span><span class="sxs-lookup"><span data-stu-id="7ad43-106">Standalone Cluster</span></span>](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

<span data-ttu-id="7ad43-107">För alla moderna system är designa för möjligheterna avgörande för att uppnå långsiktig framgång för en produkt.</span><span class="sxs-lookup"><span data-stu-id="7ad43-107">For any modern system, designing for upgradability is key to achieving long-term success of your product.</span></span> <span data-ttu-id="7ad43-108">Ett Azure Service Fabric-kluster är en resurs som du äger, men delvis hanteras av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7ad43-108">An Azure Service Fabric cluster is a resource that you own, but is partly managed by Microsoft.</span></span> <span data-ttu-id="7ad43-109">Den här artikeln beskriver vad hanteras automatiskt och vad du kan konfigurera själv.</span><span class="sxs-lookup"><span data-stu-id="7ad43-109">This article describes what is managed automatically and what you can configure yourself.</span></span>

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a><span data-ttu-id="7ad43-110">Kontrollera fabric-versionen som körs på klustret</span><span class="sxs-lookup"><span data-stu-id="7ad43-110">Controlling the fabric version that runs on your Cluster</span></span>
<span data-ttu-id="7ad43-111">Du kan ange att ta emot automatiska fabric uppgraderingar som Microsoft släpper eller så kan du välja en stöds fabric-versionen som du vill att klustret ska finnas på klustret.</span><span class="sxs-lookup"><span data-stu-id="7ad43-111">You can set your cluster to receive automatic fabric upgrades as they are released by Microsoft or you can select a supported fabric version you want your cluster to be on.</span></span>

<span data-ttu-id="7ad43-112">Du kan göra detta genom att ange klusterkonfigurationen ”upgradeMode” på portalen eller med hjälp av hanteraren för filserverresurser vid tidpunkten för skapandet eller senare på ett aktivt kluster</span><span class="sxs-lookup"><span data-stu-id="7ad43-112">You do this by setting the "upgradeMode" cluster configuration on the portal or using Resource Manager at the time of creation or later on a live cluster</span></span> 

> [!NOTE]
> <span data-ttu-id="7ad43-113">Se till att hålla ditt kluster som kör en version stöds fabric alltid.</span><span class="sxs-lookup"><span data-stu-id="7ad43-113">Make sure to keep your cluster running a supported fabric version always.</span></span> <span data-ttu-id="7ad43-114">Och när vi meddela lanseringen av en ny version av service fabric, markeras den tidigare versionen för slutet av stödet efter 60 dagar från det datum då minst.</span><span class="sxs-lookup"><span data-stu-id="7ad43-114">As and when we announce the release of a new version of service fabric, the previous version is marked for end of support after a minimum of 60 days from that date.</span></span> <span data-ttu-id="7ad43-115">den nya versioner tillkännages [på service fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric/).</span><span class="sxs-lookup"><span data-stu-id="7ad43-115">the  The new releases are announced [on the service fabric team blog](https://blogs.msdn.microsoft.com/azureservicefabric/).</span></span> <span data-ttu-id="7ad43-116">Den nya versionen är tillgängligt och välj sedan.</span><span class="sxs-lookup"><span data-stu-id="7ad43-116">The new release is available to choose then.</span></span> 
> 
> 

<span data-ttu-id="7ad43-117">14 dagar före utgången av den versionen klustret körs genereras en hälsohändelse som placerar klustret i ett varningshälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="7ad43-117">14 days prior to the expiry of the release your cluster is running, a health event is generated that puts your cluster into a warning health state.</span></span> <span data-ttu-id="7ad43-118">Klustret förblir i ett varningstillstånd förrän du har uppgraderat till en version som stöds fabric.</span><span class="sxs-lookup"><span data-stu-id="7ad43-118">The cluster remains in a warning state until you upgrade to a supported fabric version.</span></span>

### <a name="setting-the-upgrade-mode-via-portal"></a><span data-ttu-id="7ad43-119">Ange Uppgraderingsläge via portalen</span><span class="sxs-lookup"><span data-stu-id="7ad43-119">Setting the upgrade mode via portal</span></span>
<span data-ttu-id="7ad43-120">Du kan ange klustret och automatisk eller manuell när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="7ad43-120">You can set the cluster to automatic or manual when you are creating the cluster.</span></span>

![Create_Manualmode][Create_Manualmode]

<span data-ttu-id="7ad43-122">Du kan ange klustret och automatisk eller manuell på ett aktivt kluster med hjälp av hantera upplevelse.</span><span class="sxs-lookup"><span data-stu-id="7ad43-122">You can set the cluster to automatic or manual when on a live cluster, using the manage experience.</span></span> 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a><span data-ttu-id="7ad43-123">Uppgradera till en ny version av ett kluster som är inställd på manuell läge via portalen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-123">Upgrading to a new version on a cluster that is set to Manual mode via portal.</span></span>
<span data-ttu-id="7ad43-124">Om du vill uppgradera till en ny version är allt du behöver göra markerar den tillgängliga versionen i listrutan och spara.</span><span class="sxs-lookup"><span data-stu-id="7ad43-124">To upgrade to a new version, all you need to do is select the available version from the dropdown and save.</span></span> <span data-ttu-id="7ad43-125">Fabric-uppgraderingen hämtar inletts automatiskt.</span><span class="sxs-lookup"><span data-stu-id="7ad43-125">The Fabric upgrade gets kicked off automatically.</span></span> <span data-ttu-id="7ad43-126">Kluster-hälsoprinciper (en kombination av noden hälso- och i alla program som körs i klustret) följs under uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-126">The cluster health policies (a combination of node health and the health all the applications running in the cluster) are adhered to during the upgrade.</span></span>

<span data-ttu-id="7ad43-127">Uppgraderingen återställs om hälsoprinciper klustret inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="7ad43-127">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="7ad43-128">Rulla ned det här dokumentet du vill veta mer om hur du anger dessa anpassade hälsoprinciper.</span><span class="sxs-lookup"><span data-stu-id="7ad43-128">Scroll down this document to read more on how to set those custom health policies.</span></span> 

<span data-ttu-id="7ad43-129">När du har åtgärdat problemen som resulterade i återställningen måste du initiera uppgraderingen igen genom att följa samma steg som innan.</span><span class="sxs-lookup"><span data-stu-id="7ad43-129">Once you have fixed the issues that resulted in the rollback, you need to initiate the upgrade again, by following the same steps as before.</span></span>

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a><span data-ttu-id="7ad43-131">Ange Uppgraderingsläge via en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="7ad43-131">Setting the upgrade mode via a Resource Manager template</span></span>
<span data-ttu-id="7ad43-132">Lägg till ”upgradeMode”-konfigurationen i resursdefinitionen Microsoft.ServiceFabric/clusters och ange någon av versionerna som stöds fabric enligt nedan ”clusterCodeVersion” och sedan distribuera mallen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-132">Add the "upgradeMode" configuration to the Microsoft.ServiceFabric/clusters resource definition and set the "clusterCodeVersion" to one of the supported fabric versions as shown below and then deploy the template.</span></span> <span data-ttu-id="7ad43-133">Giltiga värden för ”upgradeMode” är ”manuell” eller ”automatisk”</span><span class="sxs-lookup"><span data-stu-id="7ad43-133">The valid values for "upgradeMode" are "Manual" or "Automatic"</span></span>

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a><span data-ttu-id="7ad43-135">Uppgradera till en ny version av ett kluster som är inställd på manuell läge via en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="7ad43-135">Upgrading to a new version on a cluster that is set to Manual mode via a Resource Manager template.</span></span>
<span data-ttu-id="7ad43-136">När klustret är i manuellt läge, för att uppgradera till en ny version, ändra ”clusterCodeVersion” till en version som stöds och distribuerar den.</span><span class="sxs-lookup"><span data-stu-id="7ad43-136">When the cluster is in Manual mode, to upgrade to a new version, change the "clusterCodeVersion" to a supported version and deploy it.</span></span> <span data-ttu-id="7ad43-137">Distributionen av mallen sparkar av Fabric-uppgraderingen hämtar inletts automatiskt.</span><span class="sxs-lookup"><span data-stu-id="7ad43-137">The deployment of the template, kicks of the Fabric upgrade gets kicked off automatically.</span></span> <span data-ttu-id="7ad43-138">Kluster-hälsoprinciper (en kombination av noden hälso- och i alla program som körs i klustret) följs under uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-138">The cluster health policies (a combination of node health and the health all the applications running in the cluster) are adhered to during the upgrade.</span></span>

<span data-ttu-id="7ad43-139">Uppgraderingen återställs om hälsoprinciper klustret inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="7ad43-139">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="7ad43-140">Rulla ned det här dokumentet du vill veta mer om hur du anger dessa anpassade hälsoprinciper.</span><span class="sxs-lookup"><span data-stu-id="7ad43-140">Scroll down this document to read more on how to set those custom health policies.</span></span> 

<span data-ttu-id="7ad43-141">När du har åtgärdat problemen som resulterade i återställningen måste du initiera uppgraderingen igen genom att följa samma steg som innan.</span><span class="sxs-lookup"><span data-stu-id="7ad43-141">Once you have fixed the issues that resulted in the rollback, you need to initiate the upgrade again, by following the same steps as before.</span></span>

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a><span data-ttu-id="7ad43-142">Hämta en lista över alla tillgängliga versionen för alla miljöer för en viss prenumeration</span><span class="sxs-lookup"><span data-stu-id="7ad43-142">Get list of all available version for all environments for a given subscription</span></span>
<span data-ttu-id="7ad43-143">Kör följande kommando och du bör få ett utgående ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="7ad43-143">Run the following command, and you should get an output similar to this.</span></span>

<span data-ttu-id="7ad43-144">”supportExpiryUtc” talar om när en viss version upphör att gälla eller har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="7ad43-144">"supportExpiryUtc" tells your when a given release is expiring or has expired.</span></span> <span data-ttu-id="7ad43-145">Den senaste versionen har inte ett giltigt datum - har ett värde på ”9999-12-31T23:59:59.9999999”, vilket innebär bara att utgångsdatumet inte ännu har angetts.</span><span class="sxs-lookup"><span data-stu-id="7ad43-145">The latest release does not have a valid date - it has a value of "9999-12-31T23:59:59.9999999", which just means that the expiry date is not yet set.</span></span>

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

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a><span data-ttu-id="7ad43-146">Fabric-uppgraderingar när klustret uppgradera läge är automatisk</span><span class="sxs-lookup"><span data-stu-id="7ad43-146">Fabric upgrade behavior when the cluster Upgrade Mode is Automatic</span></span>
<span data-ttu-id="7ad43-147">Microsoft har fabric koden och konfigurationen som körs i ett Azure-kluster.</span><span class="sxs-lookup"><span data-stu-id="7ad43-147">Microsoft maintains the fabric code and configuration that runs in an Azure cluster.</span></span> <span data-ttu-id="7ad43-148">Vi utföra automatiska övervakade uppgraderingar till programvaran som det behövs.</span><span class="sxs-lookup"><span data-stu-id="7ad43-148">We perform automatic monitored upgrades to the software on an as-needed basis.</span></span> <span data-ttu-id="7ad43-149">Uppgraderingarna kan vara koden, konfiguration, eller båda.</span><span class="sxs-lookup"><span data-stu-id="7ad43-149">These upgrades could be code, configuration, or both.</span></span> <span data-ttu-id="7ad43-150">Kontrollera att ditt program lider ingen påverkan eller minimal påverkan på grund av uppgraderingar, uppgraderar vi i följande faser:</span><span class="sxs-lookup"><span data-stu-id="7ad43-150">To make sure that your application suffers no impact or minimal impact due to these upgrades, we perform the upgrades in the following phases:</span></span>

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a><span data-ttu-id="7ad43-151">Fas 1: En uppgradering utförs med hjälp av alla hälsoprinciper för kluster</span><span class="sxs-lookup"><span data-stu-id="7ad43-151">Phase 1: An upgrade is performed by using all cluster health policies</span></span>
<span data-ttu-id="7ad43-152">Under den här fasen uppgraderingarna fortsätta en domän i taget och program som körs i klustret fortsätter att köras utan driftavbrott.</span><span class="sxs-lookup"><span data-stu-id="7ad43-152">During this phase, the upgrades proceed one upgrade domain at a time, and the applications that were running in the cluster continue to run without any downtime.</span></span> <span data-ttu-id="7ad43-153">Kluster-hälsoprinciper (en kombination av noden hälso- och i alla program som körs i klustret) följs under uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-153">The cluster health policies (a combination of node health and the health all the applications running in the cluster) are adhered to during the upgrade.</span></span>

<span data-ttu-id="7ad43-154">Uppgraderingen återställs om hälsoprinciper klustret inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="7ad43-154">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="7ad43-155">Sedan skickas ett e-postmeddelande till ägaren av prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-155">Then an email is sent to the owner of the subscription.</span></span> <span data-ttu-id="7ad43-156">E-postmeddelandet innehåller följande information:</span><span class="sxs-lookup"><span data-stu-id="7ad43-156">The email contains the following information:</span></span>

* <span data-ttu-id="7ad43-157">Meddelande om att vi var tvungna att återställa en uppgradering av klustret.</span><span class="sxs-lookup"><span data-stu-id="7ad43-157">Notification that we had to roll back a cluster upgrade.</span></span>
* <span data-ttu-id="7ad43-158">Föreslagna vidtar åtgärder, om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="7ad43-158">Suggested remedial actions, if any.</span></span>
* <span data-ttu-id="7ad43-159">Antal dagar (n) förrän vi köra fas 2.</span><span class="sxs-lookup"><span data-stu-id="7ad43-159">The number of days (n) until we execute Phase 2.</span></span>

<span data-ttu-id="7ad43-160">Vi försöker köra samma uppgradering några gånger om eventuella uppgraderingar misslyckades av orsaker som infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="7ad43-160">We try to execute the same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="7ad43-161">Efter n dagar från det datum då e-postmeddelandet skickades kan vi gå vidare till fas 2.</span><span class="sxs-lookup"><span data-stu-id="7ad43-161">After the n days from the date the email was sent, we proceed to Phase 2.</span></span>

<span data-ttu-id="7ad43-162">Om klustret hälsoprinciper uppfylls som genomförd uppgraderingen och markerat som slutförda.</span><span class="sxs-lookup"><span data-stu-id="7ad43-162">If the cluster health policies are met, the upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="7ad43-163">Detta kan inträffa under den första uppgraderingen eller någon av uppgradering repriser i det här steget.</span><span class="sxs-lookup"><span data-stu-id="7ad43-163">This can happen during the initial upgrade or any of the upgrade reruns in this phase.</span></span> <span data-ttu-id="7ad43-164">Det finns ingen e-bekräftelse av en lyckad körning.</span><span class="sxs-lookup"><span data-stu-id="7ad43-164">There is no email confirmation of a successful run.</span></span> <span data-ttu-id="7ad43-165">Detta är att undvika att skicka du för många e-postmeddelanden--en e-postmeddelanden ska ses som ett undantag till normal.</span><span class="sxs-lookup"><span data-stu-id="7ad43-165">This is to avoid sending you too many emails--receiving an email should be seen as an exception to normal.</span></span> <span data-ttu-id="7ad43-166">Planerar vi för de flesta av klusteruppgradering ska lyckas utan att påverka programmets tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="7ad43-166">We expect most of the cluster upgrades to succeed without impacting your application availability.</span></span>

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a><span data-ttu-id="7ad43-167">Fas 2: En uppgradering utförs med hjälp av standard hälsoprinciper endast</span><span class="sxs-lookup"><span data-stu-id="7ad43-167">Phase 2: An upgrade is performed by using default health policies only</span></span>
<span data-ttu-id="7ad43-168">Hälsoprinciper i det här steget är inställda så att antalet program som har felfri i början av uppgraderingen förblir oförändrad under uppgraderingsprocessen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-168">The health policies in this phase are set in such a way that the number of applications that were healthy at the beginning of the upgrade remains the same for the duration of the upgrade process.</span></span> <span data-ttu-id="7ad43-169">Fas 2-uppgraderingar fortsätta en domän samtidigt som fas 1 och program som körs i klustret fortsätter att köras utan driftavbrott.</span><span class="sxs-lookup"><span data-stu-id="7ad43-169">As in Phase 1, the Phase 2 upgrades proceed one upgrade domain at a time, and the applications that were running in the cluster continue to run without any downtime.</span></span> <span data-ttu-id="7ad43-170">Kluster-hälsoprinciper (en kombination av noden hälso- och i alla program som körs i klustret) följs under uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-170">The cluster health policies (a combination of node health and the health all the applications running in the cluster) are adhered to for the duration of the upgrade.</span></span>

<span data-ttu-id="7ad43-171">Om klustret hälsoprinciper gäller inte uppfylls, återställs uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-171">If the cluster health policies in effect are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="7ad43-172">Sedan skickas ett e-postmeddelande till ägaren av prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-172">Then an email is sent to the owner of the subscription.</span></span> <span data-ttu-id="7ad43-173">E-postmeddelandet innehåller följande information:</span><span class="sxs-lookup"><span data-stu-id="7ad43-173">The email contains the following information:</span></span>

* <span data-ttu-id="7ad43-174">Meddelande om att vi var tvungna att återställa en uppgradering av klustret.</span><span class="sxs-lookup"><span data-stu-id="7ad43-174">Notification that we had to roll back a cluster upgrade.</span></span>
* <span data-ttu-id="7ad43-175">Föreslagna vidtar åtgärder, om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="7ad43-175">Suggested remedial actions, if any.</span></span>
* <span data-ttu-id="7ad43-176">Antal dagar (n) förrän vi köra fas 3.</span><span class="sxs-lookup"><span data-stu-id="7ad43-176">The number of days (n) until we execute Phase 3.</span></span>

<span data-ttu-id="7ad43-177">Vi försöker köra samma uppgradering några gånger om eventuella uppgraderingar misslyckades av orsaker som infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="7ad43-177">We try to execute the same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="7ad43-178">En påminnelse om e-postmeddelande skickas ett par dagar innan n dagar är igång.</span><span class="sxs-lookup"><span data-stu-id="7ad43-178">A reminder email is sent a couple of days before n days are up.</span></span> <span data-ttu-id="7ad43-179">Efter n dagar från det datum då e-postmeddelandet skickades kan vi gå vidare till fas 3.</span><span class="sxs-lookup"><span data-stu-id="7ad43-179">After the n days from the date the email was sent, we proceed to Phase 3.</span></span> <span data-ttu-id="7ad43-180">E-postmeddelanden som vi skickar dig i fas 2 måste du vidta allvarligt och vidtar åtgärder vidtas.</span><span class="sxs-lookup"><span data-stu-id="7ad43-180">The emails we send you in Phase 2 must be taken seriously and remedial actions must be taken.</span></span>

<span data-ttu-id="7ad43-181">Om klustret hälsoprinciper uppfylls som genomförd uppgraderingen och markerat som slutförda.</span><span class="sxs-lookup"><span data-stu-id="7ad43-181">If the cluster health policies are met, the upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="7ad43-182">Detta kan inträffa under den första uppgraderingen eller någon av uppgradering repriser i det här steget.</span><span class="sxs-lookup"><span data-stu-id="7ad43-182">This can happen during the initial upgrade or any of the upgrade reruns in this phase.</span></span> <span data-ttu-id="7ad43-183">Det finns ingen e-bekräftelse av en lyckad körning.</span><span class="sxs-lookup"><span data-stu-id="7ad43-183">There is no email confirmation of a successful run.</span></span>

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a><span data-ttu-id="7ad43-184">Fas 3: En uppgradering utförs med hjälp av aggressivt hälsoprinciper</span><span class="sxs-lookup"><span data-stu-id="7ad43-184">Phase 3: An upgrade is performed by using aggressive health policies</span></span>
<span data-ttu-id="7ad43-185">Dessa hälsoprinciper i det här steget är inriktad på uppgraderingen är färdig i stället för hälsotillståndet för programmen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-185">These health policies in this phase are geared towards completion of the upgrade rather than the health of the applications.</span></span> <span data-ttu-id="7ad43-186">Mycket få klusteruppgradering hamna i det här steget.</span><span class="sxs-lookup"><span data-stu-id="7ad43-186">Very few cluster upgrades end up in this phase.</span></span> <span data-ttu-id="7ad43-187">Om klustret kommer till den här fasen, är det troligt att programmet blir ohälsosamt och förlora tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="7ad43-187">If your cluster gets to this phase, there is a good chance that your application becomes unhealthy and/or lose availability.</span></span>

<span data-ttu-id="7ad43-188">Precis som i två faser, fas 3 uppgraderingar fortsätta en domän i taget.</span><span class="sxs-lookup"><span data-stu-id="7ad43-188">Similar to the other two phases, Phase 3 upgrades proceed one upgrade domain at a time.</span></span>

<span data-ttu-id="7ad43-189">Uppgraderingen återställs om hälsoprinciper klustret inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="7ad43-189">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="7ad43-190">Vi försöker köra samma uppgradering några gånger om eventuella uppgraderingar misslyckades av orsaker som infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="7ad43-190">We try to execute the same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="7ad43-191">När är klustret Fäst, så att den får inte längre stöd och/eller uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="7ad43-191">After that, the cluster is pinned, so that it will no longer receive support and/or upgrades.</span></span>

<span data-ttu-id="7ad43-192">Ett e-postmeddelande med informationen skickas till prenumerationsägaren tillsammans med vidtar åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7ad43-192">An email with this information is sent to the subscription owner, along with the remedial actions.</span></span> <span data-ttu-id="7ad43-193">Vi förväntar sig inte några kluster för att hämta i ett tillstånd där fas 3 har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="7ad43-193">We do not expect any clusters to get into a state where Phase 3 has failed.</span></span>

<span data-ttu-id="7ad43-194">Om klustret hälsoprinciper uppfylls som genomförd uppgraderingen och markerat som slutförda.</span><span class="sxs-lookup"><span data-stu-id="7ad43-194">If the cluster health policies are met, the upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="7ad43-195">Detta kan inträffa under den första uppgraderingen eller någon av uppgradering repriser i det här steget.</span><span class="sxs-lookup"><span data-stu-id="7ad43-195">This can happen during the initial upgrade or any of the upgrade reruns in this phase.</span></span> <span data-ttu-id="7ad43-196">Det finns ingen e-bekräftelse av en lyckad körning.</span><span class="sxs-lookup"><span data-stu-id="7ad43-196">There is no email confirmation of a successful run.</span></span>

## <a name="cluster-configurations-that-you-control"></a><span data-ttu-id="7ad43-197">Klusterkonfigurationer som du hanterar</span><span class="sxs-lookup"><span data-stu-id="7ad43-197">Cluster configurations that you control</span></span>
<span data-ttu-id="7ad43-198">Utöver möjligheten att ange kluster Uppgraderingsläge, kan de konfigurationer som du kan ändra på ett aktivt kluster.</span><span class="sxs-lookup"><span data-stu-id="7ad43-198">In addition to the ability to set the cluster upgrade mode, Here are the configurations that you can change on a live cluster.</span></span>

### <a name="certificates"></a><span data-ttu-id="7ad43-199">Certifikat</span><span class="sxs-lookup"><span data-stu-id="7ad43-199">Certificates</span></span>
<span data-ttu-id="7ad43-200">Du kan lägga till nya eller ta bort certifikat för klustret och klienten via portalen enkelt.</span><span class="sxs-lookup"><span data-stu-id="7ad43-200">You can add new or delete certificates for the cluster and client via the portal easily.</span></span> <span data-ttu-id="7ad43-201">Referera till [det här dokumentet för detaljerade anvisningar](service-fabric-cluster-security-update-certs-azure.md)</span><span class="sxs-lookup"><span data-stu-id="7ad43-201">Refer to [this document for detailed instructions](service-fabric-cluster-security-update-certs-azure.md)</span></span>

![Skärmbild som visar certifikattumavtryck i Azure-portalen.][CertificateUpgrade]

### <a name="application-ports"></a><span data-ttu-id="7ad43-203">Programmet portar</span><span class="sxs-lookup"><span data-stu-id="7ad43-203">Application ports</span></span>
<span data-ttu-id="7ad43-204">Du kan ändra program portar genom att ändra belastningsutjämnaren resursegenskaper som är associerade med nodtypen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-204">You can change application ports by changing the Load Balancer resource properties that are associated with the node type.</span></span> <span data-ttu-id="7ad43-205">Du kan använda portalen eller du kan använda Resource Manager PowerShell direkt.</span><span class="sxs-lookup"><span data-stu-id="7ad43-205">You can use the portal, or you can use Resource Manager PowerShell directly.</span></span>

<span data-ttu-id="7ad43-206">Om du vill öppna en ny port på alla virtuella datorer i en nodtyp, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="7ad43-206">To open a new port on all VMs in a node type, do the following:</span></span>

1. <span data-ttu-id="7ad43-207">Lägg till en ny avsökning lämplig belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="7ad43-207">Add a new probe to the appropriate load balancer.</span></span>
   
    <span data-ttu-id="7ad43-208">Om du har distribuerat ditt kluster med hjälp av portalen belastningsutjämnare namnges ”LB-namnet på resursen grupp-NodeTypename”, en för varje nodtyp.</span><span class="sxs-lookup"><span data-stu-id="7ad43-208">If you deployed your cluster by using the portal, the load balancers are named "LB-name of the Resource group-NodeTypename", one for each node type.</span></span> <span data-ttu-id="7ad43-209">Eftersom belastningen belastningsutjämnaren namnen är unika endast inom en resursgrupp, är det bäst om du söker efter dem under en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7ad43-209">Since the load balancer names are unique only within a resource group, it is best if you search for them under a specific resource group.</span></span>
   
    ![Skärmbild som visar hur du lägger till en avsökning till en belastningsutjämnare i portalen.][AddingProbes]
2. <span data-ttu-id="7ad43-211">Lägg till en ny regel till belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="7ad43-211">Add a new rule to the load balancer.</span></span>
   
    <span data-ttu-id="7ad43-212">Lägga till en ny regel till samma belastningsutjämnare med den avsökningen som du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="7ad43-212">Add a new rule to the same load balancer by using the probe that you created in the previous step.</span></span>
   
    ![Lägger till en ny regel till en belastningsutjämnare i portalen.][AddingLBRules]

### <a name="placement-properties"></a><span data-ttu-id="7ad43-214">Placeringsegenskaper</span><span class="sxs-lookup"><span data-stu-id="7ad43-214">Placement properties</span></span>
<span data-ttu-id="7ad43-215">För varje nod, kan du lägga till anpassade placeringsegenskaper som du vill använda i dina program.</span><span class="sxs-lookup"><span data-stu-id="7ad43-215">For each of the node types, you can add custom placement properties that you want to use in your applications.</span></span> <span data-ttu-id="7ad43-216">NodeType är en standardegenskap som du kan använda utan att lägga till det explicit.</span><span class="sxs-lookup"><span data-stu-id="7ad43-216">NodeType is a default property that you can use without adding it explicitly.</span></span>

> [!NOTE]
> <span data-ttu-id="7ad43-217">Mer information om användning av placeringen nodegenskaper och hur du definierar dem finns i avsnittet ”begränsningar och noden placeringsegenskaper” i dokumentet resurshanteraren för Service Fabric-kluster på [som beskriver ditt kluster](service-fabric-cluster-resource-manager-cluster-description.md).</span><span class="sxs-lookup"><span data-stu-id="7ad43-217">For details on the use of placement constraints, node properties, and how to define them, refer to the section "Placement Constraints and Node Properties" in the Service Fabric Cluster Resource Manager Document on [Describing Your Cluster](service-fabric-cluster-resource-manager-cluster-description.md).</span></span>
> 
> 

### <a name="capacity-metrics"></a><span data-ttu-id="7ad43-218">Kapacitet mått</span><span class="sxs-lookup"><span data-stu-id="7ad43-218">Capacity metrics</span></span>
<span data-ttu-id="7ad43-219">Du kan lägga till anpassade kapacitetsmått som du vill använda i dina program att rapportera belastning för varje nodtyper.</span><span class="sxs-lookup"><span data-stu-id="7ad43-219">For each of the node types, you can add custom capacity metrics that you want to use in your applications to report load.</span></span> <span data-ttu-id="7ad43-220">Mer information om användning av kapacitetsmått för att rapportera belastning, referera till Service Fabric klustret Resource Manager-dokument på [som beskriver ditt kluster](service-fabric-cluster-resource-manager-cluster-description.md) och [mått och Läs in](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="7ad43-220">For details on the use of capacity metrics to report load, refer to the Service Fabric Cluster Resource Manager Documents on [Describing Your Cluster](service-fabric-cluster-resource-manager-cluster-description.md) and [Metrics and Load](service-fabric-cluster-resource-manager-metrics.md).</span></span>

### <a name="fabric-upgrade-settings---health-polices"></a><span data-ttu-id="7ad43-221">Inställningar för fabric - hälsoprinciper</span><span class="sxs-lookup"><span data-stu-id="7ad43-221">Fabric upgrade settings - Health polices</span></span>
<span data-ttu-id="7ad43-222">Du kan ange anpassade hälsoprinciper för fabric-uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="7ad43-222">You can specify custom health polices for fabric upgrade.</span></span> <span data-ttu-id="7ad43-223">Om du har angett ditt kluster automatisk fabric uppgraderingar, hämta dessa principer tillämpas på fas 1 av automatisk fabric-uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="7ad43-223">If you have set your cluster to Automatic fabric upgrades, then these policies get applied to the Phase-1 of the automatic fabric upgrades.</span></span>
<span data-ttu-id="7ad43-224">Om du har angett ditt kluster för manuell fabric uppgraderingar, hämta principerna tillämpas varje gång du väljer en ny version utlösa att systemet startar fabric-uppgraderingen i klustret.</span><span class="sxs-lookup"><span data-stu-id="7ad43-224">If you have set your cluster for Manual fabric upgrades, then these policies get applied each time you select a new version triggering the system to kick off the fabric upgrade in your cluster.</span></span> <span data-ttu-id="7ad43-225">Om du inte åsidosätter principerna används standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="7ad43-225">If you do not override the policies, the defaults are used.</span></span>

<span data-ttu-id="7ad43-226">Du kan ange anpassade hälsoprinciper eller granska de aktuella inställningarna under bladet ”fabric-uppgraderingen” genom att välja avancerade inställningar för uppgradering.</span><span class="sxs-lookup"><span data-stu-id="7ad43-226">You can specify the custom health policies or review the current settings under the "fabric upgrade" blade, by selecting the advanced upgrade settings.</span></span> <span data-ttu-id="7ad43-227">Läs om hur du på bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="7ad43-227">Review the following picture on how to.</span></span> 

![Hantera anpassade hälsoprinciper][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a><span data-ttu-id="7ad43-229">Anpassa infrastrukturinställningarna för klustret</span><span class="sxs-lookup"><span data-stu-id="7ad43-229">Customize Fabric settings for your cluster</span></span>
<span data-ttu-id="7ad43-230">Referera till [service fabric-kluster infrastrukturinställningarna](service-fabric-cluster-fabric-settings.md) på vad och hur du kan anpassa dem.</span><span class="sxs-lookup"><span data-stu-id="7ad43-230">Refer to [service fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md) on what and how you can customize them.</span></span>

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a><span data-ttu-id="7ad43-231">Operativsystemskorrigeringar på virtuella datorer som ingår i klustret</span><span class="sxs-lookup"><span data-stu-id="7ad43-231">OS patches on the VMs that make up the cluster</span></span>
<span data-ttu-id="7ad43-232">Referera till [korrigering Orchestration programmet](service-fabric-patch-orchestration-application.md) som kan distribueras på klustret för att installera korrigeringsfiler från Windows Update på ett framförhållning sätt att hålla tjänsterna som är tillgänglig hela tiden.</span><span class="sxs-lookup"><span data-stu-id="7ad43-232">Refer to [Patch Orchestration Application](service-fabric-patch-orchestration-application.md) which can be deployed on your cluster to install patches from Windows Update in an orchestrated manner, keeping the services available all the time.</span></span> 

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a><span data-ttu-id="7ad43-233">OS-uppgraderingar på de virtuella datorerna som utgör klustret</span><span class="sxs-lookup"><span data-stu-id="7ad43-233">OS upgrades on the VMs that make up the cluster</span></span>
<span data-ttu-id="7ad43-234">Om du måste uppgradera den OS-avbildningen på de virtuella datorerna i klustret måste du göra det en virtuell dator i taget.</span><span class="sxs-lookup"><span data-stu-id="7ad43-234">If you must upgrade the OS image on the virtual machines of the cluster, you must do it one VM at a time.</span></span> <span data-ttu-id="7ad43-235">Du ansvarar för den här uppgraderingen – det finns för närvarande ingen automatisering för det här.</span><span class="sxs-lookup"><span data-stu-id="7ad43-235">You are responsible for this upgrade--there is currently no automation for this.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ad43-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ad43-236">Next steps</span></span>
* <span data-ttu-id="7ad43-237">Lär dig hur du anpassar några av de [tjänstinställningar fabric-kluster fabric](service-fabric-cluster-fabric-settings.md)</span><span class="sxs-lookup"><span data-stu-id="7ad43-237">Learn how to customize some of the [service fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md)</span></span>
* <span data-ttu-id="7ad43-238">Lär dig hur du [skala ditt kluster in och ut](service-fabric-cluster-scale-up-down.md)</span><span class="sxs-lookup"><span data-stu-id="7ad43-238">Learn how to [scale your cluster in and out](service-fabric-cluster-scale-up-down.md)</span></span>
* <span data-ttu-id="7ad43-239">Lär dig mer om [programuppgraderingar](service-fabric-application-upgrade.md)</span><span class="sxs-lookup"><span data-stu-id="7ad43-239">Learn about [application upgrades](service-fabric-application-upgrade.md)</span></span>

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
