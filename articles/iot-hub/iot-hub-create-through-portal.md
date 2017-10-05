---
title: "Använda Azure portal för att skapa en IoT-hubb | Microsoft Docs"
description: "Så här skapa, hantera och ta bort Azure IoT-hubbar via Azure-portalen. Innehåller information om prisnivåer, skalning, säkerhet, och messaging konfiguration."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0909cd2b-4c1e-49e0-b68a-75532caf0a6a
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: bca7eea5f44bbed3b784b56edaac235161b43e5e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a><span data-ttu-id="84a5f-104">Skapa en IoT-hubb med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="84a5f-104">Create an IoT hub using the Azure portal</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="84a5f-105">Den här artikeln beskrivs:</span><span class="sxs-lookup"><span data-stu-id="84a5f-105">This article describes:</span></span>

* <span data-ttu-id="84a5f-106">Så här hittar du tjänsten IoT-hubb i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="84a5f-106">How to find the IoT Hub service in the Azure portal.</span></span>
* <span data-ttu-id="84a5f-107">Så här skapar och hanterar IoT-hubbar.</span><span class="sxs-lookup"><span data-stu-id="84a5f-107">How to create and manage IoT hubs.</span></span>

## <a name="where-to-find-the-iot-hub-service"></a><span data-ttu-id="84a5f-108">Här hittar du tjänsten IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="84a5f-108">Where to find the IoT Hub service</span></span>

<span data-ttu-id="84a5f-109">Du hittar tjänsten IoT-hubb i portalen på följande platser:</span><span class="sxs-lookup"><span data-stu-id="84a5f-109">You can find the IoT Hub service in the following locations in the portal:</span></span>

* <span data-ttu-id="84a5f-110">Välj **+ ny**, Välj **Sakernas Internet**.</span><span class="sxs-lookup"><span data-stu-id="84a5f-110">Choose **+ New**, then choose **Internet of Things**.</span></span>
* <span data-ttu-id="84a5f-111">Välj i Marketplace, **Sakernas Internet**.</span><span class="sxs-lookup"><span data-stu-id="84a5f-111">In the Marketplace, choose **Internet of Things**.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="84a5f-112">Skapa en IoT Hub</span><span class="sxs-lookup"><span data-stu-id="84a5f-112">Create an IoT hub</span></span>

<span data-ttu-id="84a5f-113">Du kan skapa en IoT-hubb med hjälp av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="84a5f-113">You can create an IoT hub using the following methods:</span></span>

* <span data-ttu-id="84a5f-114">Den **+ ny** alternativet öppnas bladet som visas i följande skärmbild visar.</span><span class="sxs-lookup"><span data-stu-id="84a5f-114">The **+ New** option opens the blade shown in the following screen shot.</span></span> <span data-ttu-id="84a5f-115">Steg för att skapa IoT-hubben via den här metoden och via marketplace är identiska.</span><span class="sxs-lookup"><span data-stu-id="84a5f-115">The steps for creating the IoT hub through this method and through the marketplace are identical.</span></span>
* <span data-ttu-id="84a5f-116">Välj i Marketplace, **skapa** att öppna bladet som visas i följande skärmbild visar.</span><span class="sxs-lookup"><span data-stu-id="84a5f-116">In the Marketplace, choose **Create** to open the blade shown in the following screen shot.</span></span>

<span data-ttu-id="84a5f-117">I följande avsnitt beskrivs stegen för att skapa en IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="84a5f-117">The following sections describe the several steps to create an IoT hub:</span></span>

### <a name="choose-the-name-of-the-iot-hub"></a><span data-ttu-id="84a5f-118">Välj namnet på IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="84a5f-118">Choose the name of the IoT hub</span></span>

<span data-ttu-id="84a5f-119">Om du vill skapa en IoT-hubb, måste du namnge IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="84a5f-119">To create an IoT hub, you must name the IoT hub.</span></span> <span data-ttu-id="84a5f-120">Det här namnet måste vara unikt inom alla IoT-hubbar.</span><span class="sxs-lookup"><span data-stu-id="84a5f-120">This name must be unique across all IoT hubs.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a><span data-ttu-id="84a5f-121">Välj prisnivå</span><span class="sxs-lookup"><span data-stu-id="84a5f-121">Choose the pricing tier</span></span>

<span data-ttu-id="84a5f-122">Du kan välja mellan fyra nivåer: **lediga**, **Standard 1** och **Standard 2**, och **Standard S3**.</span><span class="sxs-lookup"><span data-stu-id="84a5f-122">You can choose from four tiers: **Free**, **Standard 1** and **Standard 2**, and **Standard S3**.</span></span> <span data-ttu-id="84a5f-123">Den kostnadsfria nivån kan endast 500 enheter kan anslutas till IoT-hubb och upp till 8 000 meddelanden per dag.</span><span class="sxs-lookup"><span data-stu-id="84a5f-123">The free tier allows only 500 devices to be connected to the IoT hub and up to 8,000 messages per day.</span></span>

<span data-ttu-id="84a5f-124">**Standard S1**: använda S1 edition för IoT-lösningar med ett stort antal enheter som varje generera små mängder data.</span><span class="sxs-lookup"><span data-stu-id="84a5f-124">**Standard S1**: Use the S1 edition for IoT solutions with a large number of devices that each generate small amounts of data.</span></span> <span data-ttu-id="84a5f-125">Varje enhet av S1-versionen tillåter upp till 400 000 meddelanden per dag på alla anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="84a5f-125">Each unit of the S1 edition allows up to 400,000 messages per day across all connected devices.</span></span>

<span data-ttu-id="84a5f-126">**Standard S2**: Använd S2 edition för IoT-lösningar som enheter generera stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="84a5f-126">**Standard S2**: Use the S2 edition for IoT solutions in which devices generate large amounts of data.</span></span> <span data-ttu-id="84a5f-127">Varje enhet S2 edition kan upp till 6 miljoner meddelanden per dag mellan alla anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="84a5f-127">Each unit of the S2 edition allows up to 6 million messages per day between all connected devices.</span></span>

<span data-ttu-id="84a5f-128">**Standard S3**: Använd S3 edition för IoT-lösningar som genererar stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="84a5f-128">**Standard S3**: Use the S3 edition for IoT solutions that generate large amounts of data.</span></span> <span data-ttu-id="84a5f-129">Varje enhet S3 edition kan upp till 300 miljoner meddelanden per dag mellan alla anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="84a5f-129">Each unit of the S3 edition allows up to 300 million messages per day between all connected devices.</span></span>

![][4]

> [!NOTE]
> <span data-ttu-id="84a5f-130">IoT-hubb kan endast en kostnadsfri hubb per Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="84a5f-130">IoT Hub only allows one free hub per Azure subscription.</span></span>

### <a name="iot-hub-units"></a><span data-ttu-id="84a5f-131">IoT-hubbenheter</span><span class="sxs-lookup"><span data-stu-id="84a5f-131">IoT hub units</span></span>

<span data-ttu-id="84a5f-132">Antal meddelanden som tillåts per enhet per dag beror på din hubb prisnivå.</span><span class="sxs-lookup"><span data-stu-id="84a5f-132">The number of messages allowed per unit per day depends on your hub's pricing tier.</span></span> <span data-ttu-id="84a5f-133">Om du vill IoT-hubb som stöd för ingång av 700 000 meddelanden, Välj till exempel två S1 nivå enheter.</span><span class="sxs-lookup"><span data-stu-id="84a5f-133">For example, if you want the IoT hub to support ingress of 700,000 messages, you choose two S1 tier units.</span></span>

### <a name="device-to-cloud-partitions-and-resource-group"></a><span data-ttu-id="84a5f-134">Enheten till molnet partitioner och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="84a5f-134">Device to cloud partitions and resource group</span></span>

<span data-ttu-id="84a5f-135">Du kan ändra antalet partitioner för en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="84a5f-135">You can change the number of partitions for an IoT hub.</span></span> <span data-ttu-id="84a5f-136">Standardvärdet för antalet partitioner är 4 kan du välja ett annat antal från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="84a5f-136">The default number of partitions is 4, you can choose a different number from the drop-down list.</span></span>

<span data-ttu-id="84a5f-137">Du behöver inte skapa en tom resursgrupp explicit.</span><span class="sxs-lookup"><span data-stu-id="84a5f-137">You do not need to explicitly create an empty resource group.</span></span> <span data-ttu-id="84a5f-138">När du skapar en resurs, kan du välja att skapa en ny eller använda en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="84a5f-138">When you create a resource, you can choose either to create a new, or use an existing resource group.</span></span>

![][5]

### <a name="choose-subscription"></a><span data-ttu-id="84a5f-139">Välj prenumeration</span><span class="sxs-lookup"><span data-stu-id="84a5f-139">Choose subscription</span></span>

<span data-ttu-id="84a5f-140">Azure IoT-hubb visas automatiskt i Azure-prenumerationer användarkontot är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="84a5f-140">Azure IoT Hub automatically lists the Azure subscriptions the user account is linked to.</span></span> <span data-ttu-id="84a5f-141">Du kan välja att associera IoT-hubben till Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="84a5f-141">You can choose the Azure subscription to associate the IoT hub to.</span></span>

### <a name="choose-the-location"></a><span data-ttu-id="84a5f-142">Välj plats</span><span class="sxs-lookup"><span data-stu-id="84a5f-142">Choose the location</span></span>

<span data-ttu-id="84a5f-143">Alternativet plats innehåller en lista över de regioner där IoT-hubben är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="84a5f-143">The location option provides a list of the regions where IoT Hub is available.</span></span>

### <a name="create-the-iot-hub"></a><span data-ttu-id="84a5f-144">Skapa IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="84a5f-144">Create the IoT hub</span></span>

<span data-ttu-id="84a5f-145">När alla föregående steg har slutförts kan skapa du IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="84a5f-145">When all previous steps are complete, you can create the IoT hub.</span></span> <span data-ttu-id="84a5f-146">Klicka på **skapa** att starta backend-processen för att skapa och distribuera IoT-hubb med alternativen som du har valt.</span><span class="sxs-lookup"><span data-stu-id="84a5f-146">Click **Create** to start the back-end process to create and deploy the IoT hub with the options you chose.</span></span>

<span data-ttu-id="84a5f-147">Det kan ta några minuter att skapa IoT-hubb som det tar tid för backend-distributionen för att köras på rätt plats-servrar.</span><span class="sxs-lookup"><span data-stu-id="84a5f-147">It can take a few minutes to create the IoT hub as it takes time for the back-end deployment to run on the appropriate location servers.</span></span>

## <a name="change-the-settings-of-the-iot-hub"></a><span data-ttu-id="84a5f-148">Ändra inställningarna för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="84a5f-148">Change the settings of the IoT hub</span></span>

<span data-ttu-id="84a5f-149">Du kan ändra inställningarna för en befintlig IoT-hubb när den har skapats från IoT-hubb-bladet.</span><span class="sxs-lookup"><span data-stu-id="84a5f-149">You can change the settings of an existing IoT hub after it is created from the IoT Hub blade.</span></span>

![][8]

<span data-ttu-id="84a5f-150">**Delade åtkomstprinciper**: dessa principer ange behörigheter för enheter och tjänster för att ansluta till IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="84a5f-150">**Shared access policies**: These policies define the permissions for devices and services to connect to IoT Hub.</span></span> <span data-ttu-id="84a5f-151">Du kan komma åt dessa principer genom att klicka på **principer för delad åtkomst** under **allmänna**.</span><span class="sxs-lookup"><span data-stu-id="84a5f-151">You can access these policies by clicking **Shared access policies** under **General**.</span></span> <span data-ttu-id="84a5f-152">I det här bladet kan du ändra befintliga principer eller lägga till en ny princip.</span><span class="sxs-lookup"><span data-stu-id="84a5f-152">In this blade, you can either modify existing policies or add a new policy.</span></span>

### <a name="create-a-policy"></a><span data-ttu-id="84a5f-153">Skapa en princip</span><span class="sxs-lookup"><span data-stu-id="84a5f-153">Create a policy</span></span>

* <span data-ttu-id="84a5f-154">Klicka på **Lägg till** så öppnas ett blad.</span><span class="sxs-lookup"><span data-stu-id="84a5f-154">Click **Add** to open a blade.</span></span> <span data-ttu-id="84a5f-155">Här kan du ange det nya principnamnet och de behörigheter som du vill associera med den här principen som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="84a5f-155">Here you can enter the new policy name and the permissions that you want to associate with this policy, as shown in the following figure:</span></span>

    <span data-ttu-id="84a5f-156">Det finns flera behörigheter som kan associeras med dessa principer för delad.</span><span class="sxs-lookup"><span data-stu-id="84a5f-156">There are several permissions that can be associated with these shared policies.</span></span> <span data-ttu-id="84a5f-157">Den **läsa registret** och **registret skrivåtgärder** principer bevilja rättigheter för Läs- och skrivåtkomst till identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="84a5f-157">The **Registry read** and **Registry write** policies grant read and write access rights to the identity registry.</span></span> <span data-ttu-id="84a5f-158">Om du väljer alternativet skrivåtgärder väljer skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="84a5f-158">Choosing the write option automatically chooses the read option.</span></span>

    <span data-ttu-id="84a5f-159">Den **tjänsten ansluta** princip ger behörighet att komma åt slutpunkter som **får enhet till moln**.</span><span class="sxs-lookup"><span data-stu-id="84a5f-159">The **Service connect** policy grants permission to access service endpoints such as **Receive device-to-cloud**.</span></span> <span data-ttu-id="84a5f-160">Den **enhet ansluta** princip ger behörighet för att skicka och ta emot meddelanden med IoT-hubb enhetssidan.</span><span class="sxs-lookup"><span data-stu-id="84a5f-160">The **Device connect** policy grants permissions for sending and receiving messages using the IoT Hub device-side endpoints.</span></span>

* <span data-ttu-id="84a5f-161">Klicka på **skapa** att lägga till den nya principen i den befintliga listan.</span><span class="sxs-lookup"><span data-stu-id="84a5f-161">Click **Create** to add this newly created policy to the existing list.</span></span>

![][10]

## <a name="endpoints"></a><span data-ttu-id="84a5f-162">Slutpunkter</span><span class="sxs-lookup"><span data-stu-id="84a5f-162">Endpoints</span></span>

<span data-ttu-id="84a5f-163">Klicka på **slutpunkter** att visa en lista över slutpunkter för IoT-hubb som du ändrar.</span><span class="sxs-lookup"><span data-stu-id="84a5f-163">Click **Endpoints** to display a list of endpoints for the IoT hub that you are modifying.</span></span> <span data-ttu-id="84a5f-164">Det finns två typer av slutpunkter: slutpunkter som är inbyggda i IoT-hubb och slutpunkter som du lägger till IoT-hubben efter skapades.</span><span class="sxs-lookup"><span data-stu-id="84a5f-164">There are two types of endpoints: endpoints that are built into the IoT hub, and endpoints that you add to the IoT hub after its creation.</span></span>

![][11]

### <a name="built-in-endpoints"></a><span data-ttu-id="84a5f-165">Inbyggda slutpunkter</span><span class="sxs-lookup"><span data-stu-id="84a5f-165">Built-in endpoints</span></span>

<span data-ttu-id="84a5f-166">Det finns två inbyggda slutpunkter: **moln till enhet feedback** och **händelser**.</span><span class="sxs-lookup"><span data-stu-id="84a5f-166">There are two built-in endpoints: **Cloud to device feedback** and **Events**.</span></span>

* <span data-ttu-id="84a5f-167">**Moln till enhet feedback** inställningar: den här inställningen har två subsettings: **moln till enhet TTL** (time-to-live) och **kvarhållningstiden** (i timmar) för meddelanden.</span><span class="sxs-lookup"><span data-stu-id="84a5f-167">**Cloud to device feedback** settings: This setting has two subsettings: **Cloud to Device TTL** (time-to-live) and **Retention time** (in hours) for the messages.</span></span> <span data-ttu-id="84a5f-168">När först skapar en IoT-hubb kan ha båda de här inställningarna standardvärdet för en timme.</span><span class="sxs-lookup"><span data-stu-id="84a5f-168">When your first create an IoT hub, both these settings have the default value of one hour.</span></span> <span data-ttu-id="84a5f-169">Använd skjutreglagen för att justera inställningarna eller ange värdena.</span><span class="sxs-lookup"><span data-stu-id="84a5f-169">To adjust these settings, use the sliders or type the values.</span></span>
* <span data-ttu-id="84a5f-170">**Händelser** inställningar: den här inställningen har flera subsettings, vilket är skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="84a5f-170">**Events** settings: This setting has several subsettings, some of which are read-only.</span></span> <span data-ttu-id="84a5f-171">I följande lista beskrivs de här inställningarna:</span><span class="sxs-lookup"><span data-stu-id="84a5f-171">The following list describes these settings:</span></span>

  * <span data-ttu-id="84a5f-172">**Partitioner**: en är standardvärdet när IoT-hubben har skapats.</span><span class="sxs-lookup"><span data-stu-id="84a5f-172">**Partitions**: A default value is set when the IoT hub is created.</span></span> <span data-ttu-id="84a5f-173">Du kan ändra antalet partitioner via den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="84a5f-173">You can change the number of partitions through this setting.</span></span>

  * <span data-ttu-id="84a5f-174">**Händelsenamn hubb-kompatibel och slutpunkten**: när det IoT-hubben har skapats, en Händelsehubb har skapats internt att behöva åtkomst till under vissa omständigheter.</span><span class="sxs-lookup"><span data-stu-id="84a5f-174">**Event Hub-compatible name and endpoint**: When the IoT hub is created, an Event Hub is created internally that you may need access to under certain circumstances.</span></span> <span data-ttu-id="84a5f-175">Du kan anpassa Event Hub-kompatibelt namn och en slutpunkt värden men du kan kopiera dem genom att klicka på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="84a5f-175">You cannot customize the Event Hub-compatible name and endpoint values but you can copy them by clicking **Copy**.</span></span>

  * <span data-ttu-id="84a5f-176">**Kvarhållningstiden**: inställd på en dag som standard men du kan ändra den med hjälp av den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="84a5f-176">**Retention Time**: Set to one day by default but you can change it using the drop-down list.</span></span> <span data-ttu-id="84a5f-177">Det här värdet är i dagar för inställningen enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="84a5f-177">This value is in days for the device-to-cloud setting.</span></span>

  * <span data-ttu-id="84a5f-178">**Konsumentgrupper**: konsumentgrupper aktivera flera läsare att oberoende läsa meddelanden från IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="84a5f-178">**Consumer Groups**: Consumer groups enable multiple readers to read messages independently from the IoT hub.</span></span> <span data-ttu-id="84a5f-179">Alla IoT-hubben har skapats med en förinställd konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="84a5f-179">Every IoT hub is created with a default consumer group.</span></span> <span data-ttu-id="84a5f-180">Du kan lägga till eller ta bort konsumentgrupper till din IoT-hubbar som använder den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="84a5f-180">However, you can add or delete consumer groups to your IoT hubs using this setting.</span></span>

  > [!NOTE]
  > <span data-ttu-id="84a5f-181">Standard-konsumentgrupp inte redigeras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="84a5f-181">The default consumer group cannot be edited or deleted.</span></span>

### <a name="custom-endpoints"></a><span data-ttu-id="84a5f-182">Anpassade slutpunkter</span><span class="sxs-lookup"><span data-stu-id="84a5f-182">Custom endpoints</span></span>

<span data-ttu-id="84a5f-183">Du kan lägga till anpassade slutpunkter på din IoT-hubb i portalen.</span><span class="sxs-lookup"><span data-stu-id="84a5f-183">You can add custom endpoints on your IoT hub using the portal.</span></span> <span data-ttu-id="84a5f-184">Från den **slutpunkter** bladet, klickar du på **Lägg till** längst upp för att öppna den **lägga till slutpunkten** bladet.</span><span class="sxs-lookup"><span data-stu-id="84a5f-184">From the **Endpoints** blade, click **Add** at the top to open the **Add endpoint** blade.</span></span> <span data-ttu-id="84a5f-185">Ange informationen som krävs och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="84a5f-185">Enter the required information, then click **OK**.</span></span> <span data-ttu-id="84a5f-186">Din anpassade slutpunkt visas nu i huvudsakliga **slutpunkter** bladet.</span><span class="sxs-lookup"><span data-stu-id="84a5f-186">Your custom endpoint is now listed in the main **Endpoints** blade.</span></span>

![][13]

<span data-ttu-id="84a5f-187">Du kan läsa mer om anpassade slutpunkter i [referens - IoT-hubbslutpunkter][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="84a5f-187">You can read more about custom endpoints in [Reference - IoT hub endpoints][lnk-devguide-endpoints].</span></span>

## <a name="routes"></a><span data-ttu-id="84a5f-188">Vägar</span><span class="sxs-lookup"><span data-stu-id="84a5f-188">Routes</span></span>

<span data-ttu-id="84a5f-189">Klicka på **vägar** att hantera hur IoT-hubb skickar meddelanden från din enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="84a5f-189">Click **Routes** to manage how IoT Hub dispatches your device-to-cloud messages.</span></span>

![][14]

<span data-ttu-id="84a5f-190">Du kan lägga till vägar för din IoT-hubb genom att klicka på **Lägg till** överst i den **vägar*** bladet för att ange informationen som krävs och klicka **OK**.</span><span class="sxs-lookup"><span data-stu-id="84a5f-190">You can add routes to your IoT hub by clicking **Add** at the top of the **Routes*** blade, entering the required information, and clicking **OK**.</span></span> <span data-ttu-id="84a5f-191">Rutten visas sedan i huvudsakliga **vägar** bladet.</span><span class="sxs-lookup"><span data-stu-id="84a5f-191">Your route is then listed in the main **Routes** blade.</span></span> <span data-ttu-id="84a5f-192">Du kan redigera en väg genom att klicka på den i listan över vägar.</span><span class="sxs-lookup"><span data-stu-id="84a5f-192">You can edit a route by clicking it in the list of routes.</span></span> <span data-ttu-id="84a5f-193">Om du vill aktivera en väg, klickar du på den i listan över vägar och ange den **aktiverad** växla till **av**.</span><span class="sxs-lookup"><span data-stu-id="84a5f-193">To enable a route, click it in the list of routes and set the **Enabled** toggle to **Off**.</span></span> <span data-ttu-id="84a5f-194">Spara ändringen genom att klicka på **OK** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="84a5f-194">To save the change, click **OK** at the bottom of the blade.</span></span>

![][15]

## <a name="pricing-and-scale"></a><span data-ttu-id="84a5f-195">Priser och skalning</span><span class="sxs-lookup"><span data-stu-id="84a5f-195">Pricing and scale</span></span>

<span data-ttu-id="84a5f-196">Priser för en befintlig IoT-hubb kan ändras via den **priser** inställningar, med följande undantag:</span><span class="sxs-lookup"><span data-stu-id="84a5f-196">The pricing of an existing IoT hub can be changed through the **Pricing** settings, with the following exceptions:</span></span>

* <span data-ttu-id="84a5f-197">I den aktuella implementeringen en IoT-hubb med en kostnadsfri SKU kan inte ändra nivåerna till en betald SKU: er och vice versa.</span><span class="sxs-lookup"><span data-stu-id="84a5f-197">In the current implementation, an IoT hub with a free SKU cannot change tiers to one of the paid SKUs, or vice versa.</span></span>
* <span data-ttu-id="84a5f-198">Det kan bara finnas en kostnadsfria nivån IoT-hubb i Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="84a5f-198">There can only be one free tier IoT hub in the Azure subscription.</span></span>

![][12]

<span data-ttu-id="84a5f-199">Du kan flytta från en högre lägre nivå endast när antalet meddelanden som skickas den dagen du överskrida kvoten för lägre nivå.</span><span class="sxs-lookup"><span data-stu-id="84a5f-199">You can move from a higher to lower tier only when the number of messages sent that day do exceed the quota for the lower tier.</span></span> <span data-ttu-id="84a5f-200">Till exempel om antalet meddelanden per dag överskrider 400 000, kan sedan nivån för IoT-hubb ändras.</span><span class="sxs-lookup"><span data-stu-id="84a5f-200">For example, if the number of messages per day exceeds 400,000, then the tier for the IoT hub can be changed.</span></span> <span data-ttu-id="84a5f-201">Men om du ändrar till nivån S1 begränsas sedan IoT-hubben för dagen.</span><span class="sxs-lookup"><span data-stu-id="84a5f-201">However, if you change to the S1 tier then the IoT hub is throttled for that day.</span></span>

## <a name="delete-the-iot-hub"></a><span data-ttu-id="84a5f-202">Ta bort IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="84a5f-202">Delete the IoT hub</span></span>

<span data-ttu-id="84a5f-203">Du kan bläddra till IoT-hubb som du vill ta bort genom att klicka på **Bläddra**, och sedan välja lämpliga hubben att ta bort.</span><span class="sxs-lookup"><span data-stu-id="84a5f-203">You can browse to the IoT hub you want to delete by clicking **Browse**, and then choosing the appropriate hub to delete.</span></span> <span data-ttu-id="84a5f-204">Om du vill ta bort IoT-hubben, klickar du på den **ta bort** nedan IoT-hubbnamnet.</span><span class="sxs-lookup"><span data-stu-id="84a5f-204">To delete the IoT hub, click the **Delete** button below the IoT hub name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84a5f-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="84a5f-205">Next steps</span></span>

<span data-ttu-id="84a5f-206">Du kan följa dessa länkar om du vill veta mer om hur du hanterar Azure IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="84a5f-206">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="84a5f-207">[Massinläsning hantera IoT-enheter][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="84a5f-207">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="84a5f-208">[IoT-hubb mått][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="84a5f-208">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="84a5f-209">[Åtgärder som övervakning][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="84a5f-209">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="84a5f-210">Om du vill utforska ytterligare funktionerna i IoT-hubb, se:</span><span class="sxs-lookup"><span data-stu-id="84a5f-210">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="84a5f-211">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="84a5f-211">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="84a5f-212">[Simulera en enhet med IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="84a5f-212">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="84a5f-213">[Skydda din IoT-lösning från grunden upp][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="84a5f-213">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
