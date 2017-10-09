---
title: "aaaAzure IoT Suite vanliga frågor och svar | Microsoft Docs"
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
ms.openlocfilehash: cb39e24af6d1ce2afea554285512d05b2d7c721e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a><span data-ttu-id="86286-103">Vanliga frågor och svar om IoT Suite</span><span class="sxs-lookup"><span data-stu-id="86286-103">Frequently asked questions for IoT Suite</span></span>

<span data-ttu-id="86286-104">Se även hello anslutna factory specifika [vanliga frågor och svar](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="86286-104">See also, hello connected factory specific [FAQ](iot-suite-faq-cf.md).</span></span>

### <a name="where-can-i-find-hello-source-code-for-hello-preconfigured-solutions"></a><span data-ttu-id="86286-105">Var hittar jag hello källkoden för hello förkonfigurerade lösningar?</span><span class="sxs-lookup"><span data-stu-id="86286-105">Where can I find hello source code for hello preconfigured solutions?</span></span>

<span data-ttu-id="86286-106">hello källkoden lagras i följande GitHub-lagringsplatser hello:</span><span class="sxs-lookup"><span data-stu-id="86286-106">hello source code is stored in hello following GitHub repositories:</span></span>
* <span data-ttu-id="86286-107">[Fjärrövervaknings förkonfigurerade lösningen][lnk-remote-monitoring-github]</span><span class="sxs-lookup"><span data-stu-id="86286-107">[Remote monitoring preconfigured solution][lnk-remote-monitoring-github]</span></span>
* <span data-ttu-id="86286-108">[Förutsägande Underhåll förkonfigurerade lösningen][lnk-predictive-maintenance-github]</span><span class="sxs-lookup"><span data-stu-id="86286-108">[Predictive maintenance preconfigured solution][lnk-predictive-maintenance-github]</span></span>
* [<span data-ttu-id="86286-109">Anslutna factory förkonfigurerade lösningen</span><span class="sxs-lookup"><span data-stu-id="86286-109">Connected factory preconfigured solution</span></span>](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-toohello-latest-version-of-hello-remote-monitoring-preconfigured-solution-that-uses-hello-iot-hub-device-management-features"></a><span data-ttu-id="86286-110">Hur uppdaterar toohello senaste versionen av hello remote förkonfigurerade övervakningslösning som använder hello IoT-hubb funktioner för enhetshantering?</span><span class="sxs-lookup"><span data-stu-id="86286-110">How do I update toohello latest version of hello remote monitoring preconfigured solution that uses hello IoT Hub device management features?</span></span>

* <span data-ttu-id="86286-111">Om du distribuerar en förkonfigurerade lösning från hello https://www.azureiotsuite.com/ plats distribuerar alltid en ny instans av hello senaste versionen av hello lösning.</span><span class="sxs-lookup"><span data-stu-id="86286-111">If you deploy a preconfigured solution from hello https://www.azureiotsuite.com/ site, it always deploys a new instance of hello latest version of hello solution.</span></span>
* <span data-ttu-id="86286-112">Om du distribuerar en förkonfigurerade lösning hello kommandoraden måste uppdatera du en befintlig distribution med ny kod.</span><span class="sxs-lookup"><span data-stu-id="86286-112">If you deploy a preconfigured solution using hello command line, you can update an existing deployment with new code.</span></span> <span data-ttu-id="86286-113">Se [Cloud deployment] [ lnk-cloud-deployment] i hello GitHub [databasen][lnk-remote-monitoring-github].</span><span class="sxs-lookup"><span data-stu-id="86286-113">See [Cloud deployment][lnk-cloud-deployment] in hello GitHub [repository][lnk-remote-monitoring-github].</span></span>

### <a name="how-can-i-add-support-for-a-new-device-method-toohello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="86286-114">Hur kan jag lägga till stöd för en ny enhet metoden toohello remote förkonfigurerade övervakningslösning?</span><span class="sxs-lookup"><span data-stu-id="86286-114">How can I add support for a new device method toohello remote monitoring preconfigured solution?</span></span>

<span data-ttu-id="86286-115">Avsnittet hello [lägga till stöd för en ny metod toohello simulator] [ lnk-add-method] i hello [anpassa förkonfigurerade lösning] [ lnk-customize] artikel.</span><span class="sxs-lookup"><span data-stu-id="86286-115">See hello section [Add support for a new method toohello simulator][lnk-add-method] in hello [Customize a preconfigured solution][lnk-customize] article.</span></span>

### <a name="hello-simulated-device-is-ignoring-my-desired-property-changes-why"></a><span data-ttu-id="86286-116">hello simulerade enheten ignorerar min önskade egenskapsändringar varför?</span><span class="sxs-lookup"><span data-stu-id="86286-116">hello simulated device is ignoring my desired property changes, why?</span></span>
<span data-ttu-id="86286-117">Hej fjärrövervaknings förkonfigurerade lösningen, hello simulerade enheten koden använder endast hello **Desired.Config.TemperatureMeanValue** och **Desired.Config.TelemetryInterval** önskade egenskaper tooupdate hello rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="86286-117">In hello remote monitoring preconfigured solution, hello simulated device code only uses hello **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties tooupdate hello reported properties.</span></span> <span data-ttu-id="86286-118">Alla andra ändringsbegäranden för önskad egenskapen ignoreras.</span><span class="sxs-lookup"><span data-stu-id="86286-118">All other desired property change requests are ignored.</span></span>

### <a name="my-device-does-not-appear-in-hello-list-of-devices-in-hello-solution-dashboard-why"></a><span data-ttu-id="86286-119">Min enhet visas inte i hello lista över enheter i instrumentpanelen för hello lösning varför?</span><span class="sxs-lookup"><span data-stu-id="86286-119">My device does not appear in hello list of devices in hello solution dashboard, why?</span></span>

<span data-ttu-id="86286-120">hello lista över enheter i instrumentpanelen för hello lösningen använder en fråga tooreturn hello lista över enheter.</span><span class="sxs-lookup"><span data-stu-id="86286-120">hello list of devices in hello solution dashboard uses a query tooreturn hello list of devices.</span></span> <span data-ttu-id="86286-121">För närvarande kan inte en fråga returnera fler än 10K-enheter.</span><span class="sxs-lookup"><span data-stu-id="86286-121">Currently, a query cannot return more than 10K devices.</span></span> <span data-ttu-id="86286-122">Försök att göra hello sökvillkor för frågan mer restriktiva.</span><span class="sxs-lookup"><span data-stu-id="86286-122">Try making hello search criteria for your query more restrictive.</span></span>

### <a name="whats-hello-difference-between-deleting-a-resource-group-in-hello-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a><span data-ttu-id="86286-123">Vad är hello skillnaden mellan att ta bort en resursgrupp i Azure portal och klicka på Ta bort på en förkonfigurerade lösning i azureiotsuite.com hello?</span><span class="sxs-lookup"><span data-stu-id="86286-123">What's hello difference between deleting a resource group in hello Azure portal and clicking delete on a preconfigured solution in azureiotsuite.com?</span></span>

* <span data-ttu-id="86286-124">Om du tar bort hello förkonfigurerade lösning i [azureiotsuite.com][lnk-azureiotsuite], du ta bort alla hello-resurser som etablerades när du skapade hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="86286-124">If you delete hello preconfigured solution in [azureiotsuite.com][lnk-azureiotsuite], you delete all hello resources that were provisioned when you created hello preconfigured solution.</span></span> <span data-ttu-id="86286-125">Om du har lagt till fler resurser toohello resursgruppen raderas även dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="86286-125">If you added additional resources toohello resource group, these resources are also deleted.</span></span> 
* <span data-ttu-id="86286-126">Om du tar bort hello resursgrupp i hello [Azure-portalen][lnk-azure-portal], bara ta bort hello resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="86286-126">If you delete hello resource group in hello [Azure portal][lnk-azure-portal], you only delete hello resources in that resource group.</span></span> <span data-ttu-id="86286-127">Du måste också toodelete hello Azure Active Directory-program som är associerade med hello förkonfigurerade lösning i hello [klassiska Azure-portalen][lnk-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="86286-127">You also need toodelete hello Azure Active Directory application associated with hello preconfigured solution in hello [Azure classic portal][lnk-classic-portal].</span></span>

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a><span data-ttu-id="86286-128">Hur många IoT Hub-instanser kan etablera i en prenumeration?</span><span class="sxs-lookup"><span data-stu-id="86286-128">How many IoT Hub instances can I provision in a subscription?</span></span>

<span data-ttu-id="86286-129">Som standard kan du etablera [10 IoT-hubbar per prenumeration][link-azuresublimits].</span><span class="sxs-lookup"><span data-stu-id="86286-129">By default you can provision [10 IoT hubs per subscription][link-azuresublimits].</span></span> <span data-ttu-id="86286-130">Du kan skapa en [Azure supportärende] [ link-azuresupportticket] tooraise den här gränsen.</span><span class="sxs-lookup"><span data-stu-id="86286-130">You can create an [Azure support ticket][link-azuresupportticket] tooraise this limit.</span></span> <span data-ttu-id="86286-131">Därför kan kan sedan varje förkonfigurerade lösningen etablerar en ny IoT-hubb endast etablera in too10 förkonfigurerade lösningar i en viss prenumeration.</span><span class="sxs-lookup"><span data-stu-id="86286-131">As a result, since every preconfigured solution provisions a new IoT Hub, you can only provision up too10 preconfigured solutions in a given subscription.</span></span> 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a><span data-ttu-id="86286-132">Hur många Azure DB som Cosmos-instanser kan etablera i en prenumeration?</span><span class="sxs-lookup"><span data-stu-id="86286-132">How many Azure Cosmos DB instances can I provision in a subscription?</span></span>

<span data-ttu-id="86286-133">Femtio.</span><span class="sxs-lookup"><span data-stu-id="86286-133">Fifty.</span></span> <span data-ttu-id="86286-134">Du kan skapa en [Azure supportärende] [ link-azuresupportticket] tooraise detta begränsa, men som standard kan du bara etablera 50 Cosmos DB instanser per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="86286-134">You can create an [Azure support ticket][link-azuresupportticket] tooraise this limit, but by default, you can only provision 50 Cosmos DB instances per subscription.</span></span> 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a><span data-ttu-id="86286-135">Hur många kostnadsfria Bing Maps-API:er kan jag etablera i en prenumeration?</span><span class="sxs-lookup"><span data-stu-id="86286-135">How many Free Bing Maps APIs can I provision in a subscription?</span></span>

<span data-ttu-id="86286-136">Två.</span><span class="sxs-lookup"><span data-stu-id="86286-136">Two.</span></span> <span data-ttu-id="86286-137">Du kan skapa två interna transaktioner nivå 1 Bing Maps för Enterprise planer i en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="86286-137">You can create only two Internal Transactions Level 1 Bing Maps for Enterprise plans in an Azure subscription.</span></span> <span data-ttu-id="86286-138">hello remote övervakningslösning etableras som standard med hello interna transaktioner nivå 1-plan.</span><span class="sxs-lookup"><span data-stu-id="86286-138">hello remote monitoring solution is provisioned by default with hello Internal Transactions Level 1 plan.</span></span> <span data-ttu-id="86286-139">Därför kan du bara etablera tootwo fjärråtkomst övervakning lösningar i en prenumeration utan ändringar.</span><span class="sxs-lookup"><span data-stu-id="86286-139">As a result, you can only provision up tootwo remote monitoring solutions in a subscription with no modifications.</span></span>

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a><span data-ttu-id="86286-140">Jag har en distribution av en fjärrövervakningslösning med en statisk karta. Hur lägger jag till en interaktiv Bing-karta?</span><span class="sxs-lookup"><span data-stu-id="86286-140">I have a remote monitoring solution deployment with a static map, how do I add an interactive Bing map?</span></span>

1. <span data-ttu-id="86286-141">Hämta Bing Maps API för Enterprise QueryKey från [Azure-portalen][lnk-azure-portal]:</span><span class="sxs-lookup"><span data-stu-id="86286-141">Get your Bing Maps API for Enterprise QueryKey from [Azure portal][lnk-azure-portal]:</span></span> 
   
   1. <span data-ttu-id="86286-142">Navigera toohello resursgruppen där dina Bing Maps API för företag är i hello [Azure-portalen][lnk-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="86286-142">Navigate toohello Resource Group where your Bing Maps API for Enterprise is in hello [Azure portal][lnk-azure-portal].</span></span>
   2. <span data-ttu-id="86286-143">Klicka på **alla inställningar**, sedan **nyckelhantering**.</span><span class="sxs-lookup"><span data-stu-id="86286-143">Click **All Settings**, then **Key Management**.</span></span> 
   3. <span data-ttu-id="86286-144">Du kan se två nycklar: **MasterKey** och **QueryKey**.</span><span class="sxs-lookup"><span data-stu-id="86286-144">You can see two keys: **MasterKey** and **QueryKey**.</span></span> <span data-ttu-id="86286-145">Kopiera hello värde för **QueryKey**.</span><span class="sxs-lookup"><span data-stu-id="86286-145">Copy hello value for **QueryKey**.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="86286-146">Har du inget Bing Maps-API för Enterprise-konto?</span><span class="sxs-lookup"><span data-stu-id="86286-146">Don't have a Bing Maps API for Enterprise account?</span></span> <span data-ttu-id="86286-147">Skapa en i hello [Azure-portalen] [ lnk-azure-portal] genom att klicka på + ny söker efter Bing Maps API för företag och följ uppmanar toocreate.</span><span class="sxs-lookup"><span data-stu-id="86286-147">Create one in hello [Azure portal][lnk-azure-portal] by clicking + New, searching for Bing Maps API for Enterprise and follow prompts toocreate.</span></span>
      > 
      > 
2. <span data-ttu-id="86286-148">Hämtar hello senaste kod från hello [Azure IoT-fjärr-övervakning][lnk-remote-monitoring-github].</span><span class="sxs-lookup"><span data-stu-id="86286-148">Pull down hello latest code from hello [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].</span></span>
3. <span data-ttu-id="86286-149">Kör en lokal eller cloud deployment följande hello kommandoradsverktyget distributionsanvisningar i hello /docs/ mappen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="86286-149">Run a local or cloud deployment following hello command-line deployment guidance in hello /docs/ folder in hello repository.</span></span> 
4. <span data-ttu-id="86286-150">När du har kört en lokal eller molnet distribution, titta på din rotmapp för hello *. user.config-filen som skapades under distributionen.</span><span class="sxs-lookup"><span data-stu-id="86286-150">After you've run a local or cloud deployment, look in your root folder for hello *.user.config file created during deployment.</span></span> <span data-ttu-id="86286-151">Öppna den här filen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="86286-151">Open this file in a text editor.</span></span> 
5. <span data-ttu-id="86286-152">Ändra hello följande rad tooinclude hello-värde som du kopierade från din **QueryKey**:</span><span class="sxs-lookup"><span data-stu-id="86286-152">Change hello following line tooinclude hello value you copied from your **QueryKey**:</span></span> 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a><span data-ttu-id="86286-153">Kan jag skapa en förkonfigurerad lösning om jag har Microsoft Azure för DreamSpark?</span><span class="sxs-lookup"><span data-stu-id="86286-153">Can I create a preconfigured solution if I have Microsoft Azure for DreamSpark?</span></span>

<span data-ttu-id="86286-154">För närvarande kan du inte skapa en förkonfigurerade lösning med en [Microsoft Azure för DreamSpark] [ lnk-dreamspark] konto.</span><span class="sxs-lookup"><span data-stu-id="86286-154">Currently, you cannot create a preconfigured solution with a [Microsoft Azure for DreamSpark][lnk-dreamspark] account.</span></span> <span data-ttu-id="86286-155">Du kan dock skapa en [kostnadsfri utvärderingsversion konto för Azure] [ lnk-30daytrial] på ett par minuter som du kan skapa en förkonfigurerade lösning.</span><span class="sxs-lookup"><span data-stu-id="86286-155">However, you can create a [free trial account for Azure][lnk-30daytrial] in just a couple of minutes that enables you create a preconfigured solution.</span></span>

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a><span data-ttu-id="86286-156">Kan jag skapa en förkonfigurerade lösning om jag har Cloud Solution Providers (CSP)-prenumeration?</span><span class="sxs-lookup"><span data-stu-id="86286-156">Can I create a preconfigured solution if I have Cloud Solution Provider (CSP) subscription?</span></span>

<span data-ttu-id="86286-157">För närvarande kan skapa du inte en förkonfigurerade lösning med en prenumeration Cloud Solution Providers (CSP).</span><span class="sxs-lookup"><span data-stu-id="86286-157">Currently, you cannot create a preconfigured solution with a Cloud Solution Provider (CSP) subscription.</span></span> <span data-ttu-id="86286-158">Du kan dock skapa en [kostnadsfri utvärderingsversion konto för Azure] [ lnk-30daytrial] på ett par minuter som du kan skapa en förkonfigurerade lösning.</span><span class="sxs-lookup"><span data-stu-id="86286-158">However, you can create a [free trial account for Azure][lnk-30daytrial] in just a couple of minutes that enables you create a preconfigured solution.</span></span>

### <a name="how-do-i-delete-an-aad-tenant"></a><span data-ttu-id="86286-159">Hur tar jag bort en AAD-klient?</span><span class="sxs-lookup"><span data-stu-id="86286-159">How do I delete an AAD tenant?</span></span>

<span data-ttu-id="86286-160">Finns i blogginlägget Eric Golpe [genomgång av du tar bort en Azure AD-klient][lnk-delete-aad-tennant].</span><span class="sxs-lookup"><span data-stu-id="86286-160">See Eric Golpe's blog post [Walkthrough of Deleting an Azure AD Tenant][lnk-delete-aad-tennant].</span></span>

### <a name="next-steps"></a><span data-ttu-id="86286-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="86286-161">Next steps</span></span>

<span data-ttu-id="86286-162">Du kan även utforska några av hello andra funktioner och förmågor i hello IoT Suite förkonfigurerade lösningar:</span><span class="sxs-lookup"><span data-stu-id="86286-162">You can also explore some of hello other features and capabilities of hello IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="86286-163">[Förutsägande Underhåll förkonfigurerade lösning: översikt][lnk-predictive-overview]</span><span class="sxs-lookup"><span data-stu-id="86286-163">[Predictive maintenance preconfigured solution overview][lnk-predictive-overview]</span></span>
* [<span data-ttu-id="86286-164">Anslutna factory förkonfigurerade lösning: översikt</span><span class="sxs-lookup"><span data-stu-id="86286-164">Connected factory preconfigured solution overview</span></span>](iot-suite-connected-factory-overview.md)
* <span data-ttu-id="86286-165">[IoT-säkerhet från hello bakgrund][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="86286-165">[IoT security from hello ground up][lnk-security-groundup]</span></span>

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
