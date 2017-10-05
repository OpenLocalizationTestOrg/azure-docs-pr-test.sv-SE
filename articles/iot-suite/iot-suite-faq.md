---
title: "Azure IoT Suite vanliga frågor och svar | Microsoft Docs"
description: "Vanliga frågor och svar om IoT Suite"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: 85867fb8d18377637b3aa848555831a8d9b53512
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a><span data-ttu-id="5522d-103">Vanliga frågor och svar om IoT Suite</span><span class="sxs-lookup"><span data-stu-id="5522d-103">Frequently asked questions for IoT Suite</span></span>

<span data-ttu-id="5522d-104">Se även anslutna factory specifika [vanliga frågor och svar](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="5522d-104">See also, the connected factory specific [FAQ](iot-suite-faq-cf.md).</span></span>

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a><span data-ttu-id="5522d-105">Var hittar källkoden för de förkonfigurerade lösningarna?</span><span class="sxs-lookup"><span data-stu-id="5522d-105">Where can I find the source code for the preconfigured solutions?</span></span>

<span data-ttu-id="5522d-106">Källkoden lagras i följande GitHub-databaser:</span><span class="sxs-lookup"><span data-stu-id="5522d-106">The source code is stored in the following GitHub repositories:</span></span>
* <span data-ttu-id="5522d-107">[Fjärrövervaknings förkonfigurerade lösningen][lnk-remote-monitoring-github]</span><span class="sxs-lookup"><span data-stu-id="5522d-107">[Remote monitoring preconfigured solution][lnk-remote-monitoring-github]</span></span>
* <span data-ttu-id="5522d-108">[Förutsägande Underhåll förkonfigurerade lösningen][lnk-predictive-maintenance-github]</span><span class="sxs-lookup"><span data-stu-id="5522d-108">[Predictive maintenance preconfigured solution][lnk-predictive-maintenance-github]</span></span>
* [<span data-ttu-id="5522d-109">Anslutna factory förkonfigurerade lösningen</span><span class="sxs-lookup"><span data-stu-id="5522d-109">Connected factory preconfigured solution</span></span>](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-to-the-latest-version-of-the-remote-monitoring-preconfigured-solution-that-uses-the-iot-hub-device-management-features"></a><span data-ttu-id="5522d-110">Hur uppdatera till den senaste versionen av fjärråtkomst övervakning förkonfigurerade lösning som använder hanteringsfunktionerna för IoT Hub-enheter?</span><span class="sxs-lookup"><span data-stu-id="5522d-110">How do I update to the latest version of the remote monitoring preconfigured solution that uses the IoT Hub device management features?</span></span>

* <span data-ttu-id="5522d-111">Om du distribuerar en förkonfigurerade lösning från webbplatsen https://www.azureiotsuite.com/ distribuerar alltid en ny instans av den senaste versionen av lösningen.</span><span class="sxs-lookup"><span data-stu-id="5522d-111">If you deploy a preconfigured solution from the https://www.azureiotsuite.com/ site, it always deploys a new instance of the latest version of the solution.</span></span>
* <span data-ttu-id="5522d-112">Om du distribuerar en förkonfigurerade lösning med hjälp av kommandoraden, kan du uppdatera en befintlig distribution med ny kod.</span><span class="sxs-lookup"><span data-stu-id="5522d-112">If you deploy a preconfigured solution using the command line, you can update an existing deployment with new code.</span></span> <span data-ttu-id="5522d-113">Se [Cloud deployment] [ lnk-cloud-deployment] i GitHub [databasen][lnk-remote-monitoring-github].</span><span class="sxs-lookup"><span data-stu-id="5522d-113">See [Cloud deployment][lnk-cloud-deployment] in the GitHub [repository][lnk-remote-monitoring-github].</span></span>

### <a name="how-can-i-add-support-for-a-new-device-method-to-the-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="5522d-114">Hur kan jag lägga till stöd för en ny enhet metod fjärråtkomst övervakning förkonfigurerade lösningen?</span><span class="sxs-lookup"><span data-stu-id="5522d-114">How can I add support for a new device method to the remote monitoring preconfigured solution?</span></span>

<span data-ttu-id="5522d-115">I avsnittet [lägga till stöd för en ny metod i simulatorn] [ lnk-add-method] i den [anpassa förkonfigurerade lösning] [ lnk-customize] artikel.</span><span class="sxs-lookup"><span data-stu-id="5522d-115">See the section [Add support for a new method to the simulator][lnk-add-method] in the [Customize a preconfigured solution][lnk-customize] article.</span></span>

### <a name="the-simulated-device-is-ignoring-my-desired-property-changes-why"></a><span data-ttu-id="5522d-116">Den simulerade enheten ignorerar min önskade egenskapsändringar varför?</span><span class="sxs-lookup"><span data-stu-id="5522d-116">The simulated device is ignoring my desired property changes, why?</span></span>
<span data-ttu-id="5522d-117">I fjärråtkomst övervakning förkonfigurerade lösningen simulerade enheten kod endast används den **Desired.Config.TemperatureMeanValue** och **Desired.Config.TelemetryInterval** önskade egenskaper Uppdatera den rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="5522d-117">In the remote monitoring preconfigured solution, the simulated device code only uses the **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties to update the reported properties.</span></span> <span data-ttu-id="5522d-118">Alla andra ändringsbegäranden för önskad egenskapen ignoreras.</span><span class="sxs-lookup"><span data-stu-id="5522d-118">All other desired property change requests are ignored.</span></span>

### <a name="my-device-does-not-appear-in-the-list-of-devices-in-the-solution-dashboard-why"></a><span data-ttu-id="5522d-119">Min enhet visas inte i listan över enheter i instrumentpanelen för lösningen varför?</span><span class="sxs-lookup"><span data-stu-id="5522d-119">My device does not appear in the list of devices in the solution dashboard, why?</span></span>

<span data-ttu-id="5522d-120">Listan över enheter i instrumentpanelen för lösningen används en fråga för att returnera en lista med enheter.</span><span class="sxs-lookup"><span data-stu-id="5522d-120">The list of devices in the solution dashboard uses a query to return the list of devices.</span></span> <span data-ttu-id="5522d-121">För närvarande kan inte en fråga returnera fler än 10K-enheter.</span><span class="sxs-lookup"><span data-stu-id="5522d-121">Currently, a query cannot return more than 10K devices.</span></span> <span data-ttu-id="5522d-122">Försök att göra sökvillkor för frågan mer restriktiva.</span><span class="sxs-lookup"><span data-stu-id="5522d-122">Try making the search criteria for your query more restrictive.</span></span>

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a><span data-ttu-id="5522d-123">Vad är skillnaden mellan att ta bort en resursgrupp på Azure-portalen och att klicka på Ta bort för en förkonfigurerad lösning på azureiotsuite.com?</span><span class="sxs-lookup"><span data-stu-id="5522d-123">What's the difference between deleting a resource group in the Azure portal and clicking delete on a preconfigured solution in azureiotsuite.com?</span></span>

* <span data-ttu-id="5522d-124">Om du tar bort förkonfigurerade lösningen i [azureiotsuite.com][lnk-azureiotsuite], du ta bort alla resurser som etablerades när du skapade den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="5522d-124">If you delete the preconfigured solution in [azureiotsuite.com][lnk-azureiotsuite], you delete all the resources that were provisioned when you created the preconfigured solution.</span></span> <span data-ttu-id="5522d-125">Om du har lagt till fler resurser till resursgruppen raderas även dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="5522d-125">If you added additional resources to the resource group, these resources are also deleted.</span></span> 
* <span data-ttu-id="5522d-126">Om du tar bort resursgrupp i den [Azure-portalen][lnk-azure-portal], bara ta bort resurserna i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="5522d-126">If you delete the resource group in the [Azure portal][lnk-azure-portal], you only delete the resources in that resource group.</span></span> <span data-ttu-id="5522d-127">Du måste också ta bort Azure Active Directory-programmet som är associerade med förkonfigurerade lösningen i den [klassiska Azure-portalen][lnk-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="5522d-127">You also need to delete the Azure Active Directory application associated with the preconfigured solution in the [Azure classic portal][lnk-classic-portal].</span></span>

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a><span data-ttu-id="5522d-128">Hur många IoT Hub-instanser kan etablera i en prenumeration?</span><span class="sxs-lookup"><span data-stu-id="5522d-128">How many IoT Hub instances can I provision in a subscription?</span></span>

<span data-ttu-id="5522d-129">Som standard kan du etablera [10 IoT-hubbar per prenumeration][link-azuresublimits].</span><span class="sxs-lookup"><span data-stu-id="5522d-129">By default you can provision [10 IoT hubs per subscription][link-azuresublimits].</span></span> <span data-ttu-id="5522d-130">Du kan skapa en [Azure supportärende] [ link-azuresupportticket] att höja gränsen.</span><span class="sxs-lookup"><span data-stu-id="5522d-130">You can create an [Azure support ticket][link-azuresupportticket] to raise this limit.</span></span> <span data-ttu-id="5522d-131">Eftersom varje förkonfigurerade lösning etablerar en ny IoT-hubb, kan du därför bara etablera upp till 10 förkonfigurerade lösningar i en viss prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5522d-131">As a result, since every preconfigured solution provisions a new IoT Hub, you can only provision up to 10 preconfigured solutions in a given subscription.</span></span> 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a><span data-ttu-id="5522d-132">Hur många Azure DB som Cosmos-instanser kan etablera i en prenumeration?</span><span class="sxs-lookup"><span data-stu-id="5522d-132">How many Azure Cosmos DB instances can I provision in a subscription?</span></span>

<span data-ttu-id="5522d-133">Femtio.</span><span class="sxs-lookup"><span data-stu-id="5522d-133">Fifty.</span></span> <span data-ttu-id="5522d-134">Du kan skapa en [Azure supportärende] [ link-azuresupportticket] att höja gränsen, men som standard kan du bara etablera 50 Cosmos DB instanser per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5522d-134">You can create an [Azure support ticket][link-azuresupportticket] to raise this limit, but by default, you can only provision 50 Cosmos DB instances per subscription.</span></span> 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a><span data-ttu-id="5522d-135">Hur många kostnadsfria Bing Maps-API:er kan jag etablera i en prenumeration?</span><span class="sxs-lookup"><span data-stu-id="5522d-135">How many Free Bing Maps APIs can I provision in a subscription?</span></span>

<span data-ttu-id="5522d-136">Två.</span><span class="sxs-lookup"><span data-stu-id="5522d-136">Two.</span></span> <span data-ttu-id="5522d-137">Du kan skapa två interna transaktioner nivå 1 Bing Maps för Enterprise planer i en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5522d-137">You can create only two Internal Transactions Level 1 Bing Maps for Enterprise plans in an Azure subscription.</span></span> <span data-ttu-id="5522d-138">Fjärråtkomst övervakningslösning etableras som standard med interna transaktioner nivå 1-plan.</span><span class="sxs-lookup"><span data-stu-id="5522d-138">The remote monitoring solution is provisioned by default with the Internal Transactions Level 1 plan.</span></span> <span data-ttu-id="5522d-139">Detta betyder att du bara kan etablera upp till två fjärrövervakningslösningar i en prenumeration utan några ändringar.</span><span class="sxs-lookup"><span data-stu-id="5522d-139">As a result, you can only provision up to two remote monitoring solutions in a subscription with no modifications.</span></span>

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a><span data-ttu-id="5522d-140">Jag har en distribution av en fjärrövervakningslösning med en statisk karta. Hur lägger jag till en interaktiv Bing-karta?</span><span class="sxs-lookup"><span data-stu-id="5522d-140">I have a remote monitoring solution deployment with a static map, how do I add an interactive Bing map?</span></span>

1. <span data-ttu-id="5522d-141">Hämta Bing Maps API för Enterprise QueryKey från [Azure-portalen][lnk-azure-portal]:</span><span class="sxs-lookup"><span data-stu-id="5522d-141">Get your Bing Maps API for Enterprise QueryKey from [Azure portal][lnk-azure-portal]:</span></span> 
   
   1. <span data-ttu-id="5522d-142">Gå till resursgruppen där dina Bing Maps API för företag finns i den [Azure-portalen][lnk-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="5522d-142">Navigate to the Resource Group where your Bing Maps API for Enterprise is in the [Azure portal][lnk-azure-portal].</span></span>
   2. <span data-ttu-id="5522d-143">Klicka på **alla inställningar**, sedan **nyckelhantering**.</span><span class="sxs-lookup"><span data-stu-id="5522d-143">Click **All Settings**, then **Key Management**.</span></span> 
   3. <span data-ttu-id="5522d-144">Du kan se två nycklar: **MasterKey** och **QueryKey**.</span><span class="sxs-lookup"><span data-stu-id="5522d-144">You can see two keys: **MasterKey** and **QueryKey**.</span></span> <span data-ttu-id="5522d-145">Kopiera värdet för **QueryKey**.</span><span class="sxs-lookup"><span data-stu-id="5522d-145">Copy the value for **QueryKey**.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="5522d-146">Har du inget Bing Maps-API för Enterprise-konto?</span><span class="sxs-lookup"><span data-stu-id="5522d-146">Don't have a Bing Maps API for Enterprise account?</span></span> <span data-ttu-id="5522d-147">Skapa en i den [Azure-portalen] [ lnk-azure-portal] genom att klicka på + nya, söker efter Bing Maps API för företag och följ anvisningarna för att skapa.</span><span class="sxs-lookup"><span data-stu-id="5522d-147">Create one in the [Azure portal][lnk-azure-portal] by clicking + New, searching for Bing Maps API for Enterprise and follow prompts to create.</span></span>
      > 
      > 
2. <span data-ttu-id="5522d-148">Hämtar det senaste kod från de [Azure IoT-fjärr-övervakning][lnk-remote-monitoring-github].</span><span class="sxs-lookup"><span data-stu-id="5522d-148">Pull down the latest code from the [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].</span></span>
3. <span data-ttu-id="5522d-149">Kör en lokal eller cloud deployment följande vägledning för distribution i mappen /docs/ i databasen.</span><span class="sxs-lookup"><span data-stu-id="5522d-149">Run a local or cloud deployment following the command-line deployment guidance in the /docs/ folder in the repository.</span></span> 
4. <span data-ttu-id="5522d-150">När du har kört en lokal eller molnbaserad distribution tittar du i rotmappen efter *.user.config-filen som skapades under distributionen.</span><span class="sxs-lookup"><span data-stu-id="5522d-150">After you've run a local or cloud deployment, look in your root folder for the *.user.config file created during deployment.</span></span> <span data-ttu-id="5522d-151">Öppna den här filen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="5522d-151">Open this file in a text editor.</span></span> 
5. <span data-ttu-id="5522d-152">Ändra följande rad att inkludera det värde som du kopierade från din **QueryKey**:</span><span class="sxs-lookup"><span data-stu-id="5522d-152">Change the following line to include the value you copied from your **QueryKey**:</span></span> 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a><span data-ttu-id="5522d-153">Kan jag skapa en förkonfigurerad lösning om jag har Microsoft Azure för DreamSpark?</span><span class="sxs-lookup"><span data-stu-id="5522d-153">Can I create a preconfigured solution if I have Microsoft Azure for DreamSpark?</span></span>

<span data-ttu-id="5522d-154">För närvarande kan du inte skapa en förkonfigurerade lösning med en [Microsoft Azure för DreamSpark] [ lnk-dreamspark] konto.</span><span class="sxs-lookup"><span data-stu-id="5522d-154">Currently, you cannot create a preconfigured solution with a [Microsoft Azure for DreamSpark][lnk-dreamspark] account.</span></span> <span data-ttu-id="5522d-155">Du kan dock skapa en [kostnadsfri utvärderingsversion konto för Azure] [ lnk-30daytrial] på ett par minuter som du kan skapa en förkonfigurerade lösning.</span><span class="sxs-lookup"><span data-stu-id="5522d-155">However, you can create a [free trial account for Azure][lnk-30daytrial] in just a couple of minutes that enables you create a preconfigured solution.</span></span>

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a><span data-ttu-id="5522d-156">Kan jag skapa en förkonfigurerade lösning om jag har Cloud Solution Providers (CSP)-prenumeration?</span><span class="sxs-lookup"><span data-stu-id="5522d-156">Can I create a preconfigured solution if I have Cloud Solution Provider (CSP) subscription?</span></span>

<span data-ttu-id="5522d-157">För närvarande kan skapa du inte en förkonfigurerade lösning med en prenumeration Cloud Solution Providers (CSP).</span><span class="sxs-lookup"><span data-stu-id="5522d-157">Currently, you cannot create a preconfigured solution with a Cloud Solution Provider (CSP) subscription.</span></span> <span data-ttu-id="5522d-158">Du kan dock skapa en [kostnadsfri utvärderingsversion konto för Azure] [ lnk-30daytrial] på ett par minuter som du kan skapa en förkonfigurerade lösning.</span><span class="sxs-lookup"><span data-stu-id="5522d-158">However, you can create a [free trial account for Azure][lnk-30daytrial] in just a couple of minutes that enables you create a preconfigured solution.</span></span>

### <a name="how-do-i-delete-an-aad-tenant"></a><span data-ttu-id="5522d-159">Hur tar jag bort en AAD-klient?</span><span class="sxs-lookup"><span data-stu-id="5522d-159">How do I delete an AAD tenant?</span></span>

<span data-ttu-id="5522d-160">Finns i blogginlägget Eric Golpe [genomgång av du tar bort en Azure AD-klient][lnk-delete-aad-tennant].</span><span class="sxs-lookup"><span data-stu-id="5522d-160">See Eric Golpe's blog post [Walkthrough of Deleting an Azure AD Tenant][lnk-delete-aad-tennant].</span></span>

### <a name="next-steps"></a><span data-ttu-id="5522d-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5522d-161">Next steps</span></span>

<span data-ttu-id="5522d-162">Du kan även utforska några andra funktioner och möjligheter i de förkonfigurerade lösningarna i IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="5522d-162">You can also explore some of the other features and capabilities of the IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="5522d-163">[Förutsägande Underhåll förkonfigurerade lösning: översikt][lnk-predictive-overview]</span><span class="sxs-lookup"><span data-stu-id="5522d-163">[Predictive maintenance preconfigured solution overview][lnk-predictive-overview]</span></span>
* [<span data-ttu-id="5522d-164">Anslutna factory förkonfigurerade lösning: översikt</span><span class="sxs-lookup"><span data-stu-id="5522d-164">Connected factory preconfigured solution overview</span></span>](iot-suite-connected-factory-overview.md)
* <span data-ttu-id="5522d-165">[IoT-säkerhet från grunden][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="5522d-165">[IoT security from the ground up][lnk-security-groundup]</span></span>

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
[lnk-cloud-deployment]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
[lnk-add-method]: iot-suite-guidance-on-customizing-preconfigured-solutions.md#add-support-for-a-new-method-to-the-simulator
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-predictive-maintenance-github]: https://github.com/Azure/azure-iot-predictive-maintenance
