---
title: aaaUse hello Azure portal toocreate en IoT-hubb | Microsoft Docs
description: "Hur toocreate, hantera och ta bort Azure IoT Hub via hello Azure-portalen. Innehåller information om prisnivåer, skalning, säkerhet, och messaging konfiguration."
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
ms.openlocfilehash: 383968c90ee7ef3bff85a6c90efbf5f0e8fbb208
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-portal"></a><span data-ttu-id="6f503-104">Skapa en IoT-hubb med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6f503-104">Create an IoT hub using hello Azure portal</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="6f503-105">Den här artikeln beskrivs:</span><span class="sxs-lookup"><span data-stu-id="6f503-105">This article describes:</span></span>

* <span data-ttu-id="6f503-106">Hur hello toofind IoT-hubb tjänst i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6f503-106">How toofind hello IoT Hub service in hello Azure portal.</span></span>
* <span data-ttu-id="6f503-107">Hur toocreate och hantera IoT-hubbar.</span><span class="sxs-lookup"><span data-stu-id="6f503-107">How toocreate and manage IoT hubs.</span></span>

## <a name="where-toofind-hello-iot-hub-service"></a><span data-ttu-id="6f503-108">Där toofind hello IoT-hubb-tjänsten</span><span class="sxs-lookup"><span data-stu-id="6f503-108">Where toofind hello IoT Hub service</span></span>

<span data-ttu-id="6f503-109">Du kan hitta hello IoT-hubb service i hello följande platser i hello portal:</span><span class="sxs-lookup"><span data-stu-id="6f503-109">You can find hello IoT Hub service in hello following locations in hello portal:</span></span>

* <span data-ttu-id="6f503-110">Välj **+ ny**, Välj **Sakernas Internet**.</span><span class="sxs-lookup"><span data-stu-id="6f503-110">Choose **+ New**, then choose **Internet of Things**.</span></span>
* <span data-ttu-id="6f503-111">Välj i hello Marketplace, **Sakernas Internet**.</span><span class="sxs-lookup"><span data-stu-id="6f503-111">In hello Marketplace, choose **Internet of Things**.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="6f503-112">Skapa en IoT Hub</span><span class="sxs-lookup"><span data-stu-id="6f503-112">Create an IoT hub</span></span>

<span data-ttu-id="6f503-113">Du kan skapa en IoT-hubb med hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="6f503-113">You can create an IoT hub using hello following methods:</span></span>

* <span data-ttu-id="6f503-114">Hej **+ ny** hello bladet som visas i följande skärmbild som visar hello öppnas.</span><span class="sxs-lookup"><span data-stu-id="6f503-114">hello **+ New** option opens hello blade shown in hello following screen shot.</span></span> <span data-ttu-id="6f503-115">hello stegen för att skapa hello IoT-hubb via den här metoden och via hello marketplace är identiska.</span><span class="sxs-lookup"><span data-stu-id="6f503-115">hello steps for creating hello IoT hub through this method and through hello marketplace are identical.</span></span>
* <span data-ttu-id="6f503-116">Välj i hello Marketplace, **skapa** tooopen hello bladet som visas i följande skärmbild som visar hello.</span><span class="sxs-lookup"><span data-stu-id="6f503-116">In hello Marketplace, choose **Create** tooopen hello blade shown in hello following screen shot.</span></span>

<span data-ttu-id="6f503-117">hello följande avsnitt beskrivs hello flera steg toocreate en IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="6f503-117">hello following sections describe hello several steps toocreate an IoT hub:</span></span>

### <a name="choose-hello-name-of-hello-iot-hub"></a><span data-ttu-id="6f503-118">Välj hello namnet på hello IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="6f503-118">Choose hello name of hello IoT hub</span></span>

<span data-ttu-id="6f503-119">toocreate en IoT-hubb, måste du namnge hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6f503-119">toocreate an IoT hub, you must name hello IoT hub.</span></span> <span data-ttu-id="6f503-120">Det här namnet måste vara unikt inom alla IoT-hubbar.</span><span class="sxs-lookup"><span data-stu-id="6f503-120">This name must be unique across all IoT hubs.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-hello-pricing-tier"></a><span data-ttu-id="6f503-121">Välj hello prisnivån</span><span class="sxs-lookup"><span data-stu-id="6f503-121">Choose hello pricing tier</span></span>

<span data-ttu-id="6f503-122">Du kan välja mellan fyra nivåer: **lediga**, **Standard 1** och **Standard 2**, och **Standard S3**.</span><span class="sxs-lookup"><span data-stu-id="6f503-122">You can choose from four tiers: **Free**, **Standard 1** and **Standard 2**, and **Standard S3**.</span></span> <span data-ttu-id="6f503-123">hello kostnadsfria nivån kan endast 500 enheter toobe anslutna toohello IoT-hubb och uppåt too8 000 meddelanden per dag.</span><span class="sxs-lookup"><span data-stu-id="6f503-123">hello free tier allows only 500 devices toobe connected toohello IoT hub and up too8,000 messages per day.</span></span>

<span data-ttu-id="6f503-124">**Standard S1**: Använd hello S1 edition för IoT-lösningar med ett stort antal enheter som varje generera små mängder data.</span><span class="sxs-lookup"><span data-stu-id="6f503-124">**Standard S1**: Use hello S1 edition for IoT solutions with a large number of devices that each generate small amounts of data.</span></span> <span data-ttu-id="6f503-125">Varje enhet hello S1 edition kan in too400 000 meddelanden per dag på alla anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="6f503-125">Each unit of hello S1 edition allows up too400,000 messages per day across all connected devices.</span></span>

<span data-ttu-id="6f503-126">**Standard S2**: Använd hello S2 edition för IoT-lösningar som enheter generera stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="6f503-126">**Standard S2**: Use hello S2 edition for IoT solutions in which devices generate large amounts of data.</span></span> <span data-ttu-id="6f503-127">Varje enhet hello S2 edition kan in too6 miljoner meddelanden per dag mellan alla anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="6f503-127">Each unit of hello S2 edition allows up too6 million messages per day between all connected devices.</span></span>

<span data-ttu-id="6f503-128">**Standard S3**: Använd hello S3 edition för IoT-lösningar som genererar stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="6f503-128">**Standard S3**: Use hello S3 edition for IoT solutions that generate large amounts of data.</span></span> <span data-ttu-id="6f503-129">Varje enhet hello S3 edition kan in too300 miljoner meddelanden per dag mellan alla anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="6f503-129">Each unit of hello S3 edition allows up too300 million messages per day between all connected devices.</span></span>

![][4]

> [!NOTE]
> <span data-ttu-id="6f503-130">IoT-hubb kan endast en kostnadsfri hubb per Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6f503-130">IoT Hub only allows one free hub per Azure subscription.</span></span>

### <a name="iot-hub-units"></a><span data-ttu-id="6f503-131">IoT-hubbenheter</span><span class="sxs-lookup"><span data-stu-id="6f503-131">IoT hub units</span></span>

<span data-ttu-id="6f503-132">hello antalet meddelanden som tillåts per enhet per dag beror på din hubb prisnivån.</span><span class="sxs-lookup"><span data-stu-id="6f503-132">hello number of messages allowed per unit per day depends on your hub's pricing tier.</span></span> <span data-ttu-id="6f503-133">Till exempel om du vill att hello IoT-hubb toosupport ingång av 700 000 meddelanden du två S1 nivå enheter.</span><span class="sxs-lookup"><span data-stu-id="6f503-133">For example, if you want hello IoT hub toosupport ingress of 700,000 messages, you choose two S1 tier units.</span></span>

### <a name="device-toocloud-partitions-and-resource-group"></a><span data-ttu-id="6f503-134">Enheten toocloud partitioner och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="6f503-134">Device toocloud partitions and resource group</span></span>

<span data-ttu-id="6f503-135">Du kan ändra hello antalet partitioner för en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6f503-135">You can change hello number of partitions for an IoT hub.</span></span> <span data-ttu-id="6f503-136">hello standardantalet partitioner är 4 kan du välja ett annat antal hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="6f503-136">hello default number of partitions is 4, you can choose a different number from hello drop-down list.</span></span>

<span data-ttu-id="6f503-137">Du behöver inte tooexplicitly skapar du en tom resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6f503-137">You do not need tooexplicitly create an empty resource group.</span></span> <span data-ttu-id="6f503-138">När du skapar en resurs kan du välja antingen toocreate en ny eller Använd en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6f503-138">When you create a resource, you can choose either toocreate a new, or use an existing resource group.</span></span>

![][5]

### <a name="choose-subscription"></a><span data-ttu-id="6f503-139">Välj prenumeration</span><span class="sxs-lookup"><span data-stu-id="6f503-139">Choose subscription</span></span>

<span data-ttu-id="6f503-140">Azure IoT-hubb automatiskt visar hello Azure-prenumerationer hello-användarkontot är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="6f503-140">Azure IoT Hub automatically lists hello Azure subscriptions hello user account is linked to.</span></span> <span data-ttu-id="6f503-141">Du kan välja hello Azure-prenumeration tooassociate hello IoT-hubb till.</span><span class="sxs-lookup"><span data-stu-id="6f503-141">You can choose hello Azure subscription tooassociate hello IoT hub to.</span></span>

### <a name="choose-hello-location"></a><span data-ttu-id="6f503-142">Välj hello plats</span><span class="sxs-lookup"><span data-stu-id="6f503-142">Choose hello location</span></span>

<span data-ttu-id="6f503-143">Hej platsalternativ innehåller en lista över hello regioner där IoT-hubben är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="6f503-143">hello location option provides a list of hello regions where IoT Hub is available.</span></span>

### <a name="create-hello-iot-hub"></a><span data-ttu-id="6f503-144">Skapa hello IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="6f503-144">Create hello IoT hub</span></span>

<span data-ttu-id="6f503-145">När alla föregående steg har slutförts kan skapa du hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6f503-145">When all previous steps are complete, you can create hello IoT hub.</span></span> <span data-ttu-id="6f503-146">Klicka på **skapa** toostart hello serverdelsprocess toocreate och distribuera hello IoT-hubb med hello-alternativ som du har valt.</span><span class="sxs-lookup"><span data-stu-id="6f503-146">Click **Create** toostart hello back-end process toocreate and deploy hello IoT hub with hello options you chose.</span></span>

<span data-ttu-id="6f503-147">Det kan ta några minuter toocreate hello IoT-hubb som det tar tid för hello backend-distribution toorun på hello lämplig plats servrar.</span><span class="sxs-lookup"><span data-stu-id="6f503-147">It can take a few minutes toocreate hello IoT hub as it takes time for hello back-end deployment toorun on hello appropriate location servers.</span></span>

## <a name="change-hello-settings-of-hello-iot-hub"></a><span data-ttu-id="6f503-148">Ändra hello inställningar för hello IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="6f503-148">Change hello settings of hello IoT hub</span></span>

<span data-ttu-id="6f503-149">Du kan ändra hello inställningarna för en befintlig IoT-hubb när den har skapats från hello IoT-hubb-bladet.</span><span class="sxs-lookup"><span data-stu-id="6f503-149">You can change hello settings of an existing IoT hub after it is created from hello IoT Hub blade.</span></span>

![][8]

<span data-ttu-id="6f503-150">**Delade åtkomstprinciper**: dessa principer definiera hello behörigheter för enheter och tjänster tooconnect tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="6f503-150">**Shared access policies**: These policies define hello permissions for devices and services tooconnect tooIoT Hub.</span></span> <span data-ttu-id="6f503-151">Du kan komma åt dessa principer genom att klicka på **principer för delad åtkomst** under **allmänna**.</span><span class="sxs-lookup"><span data-stu-id="6f503-151">You can access these policies by clicking **Shared access policies** under **General**.</span></span> <span data-ttu-id="6f503-152">I det här bladet kan du ändra befintliga principer eller lägga till en ny princip.</span><span class="sxs-lookup"><span data-stu-id="6f503-152">In this blade, you can either modify existing policies or add a new policy.</span></span>

### <a name="create-a-policy"></a><span data-ttu-id="6f503-153">Skapa en princip</span><span class="sxs-lookup"><span data-stu-id="6f503-153">Create a policy</span></span>

* <span data-ttu-id="6f503-154">Klicka på **Lägg till** tooopen ett blad.</span><span class="sxs-lookup"><span data-stu-id="6f503-154">Click **Add** tooopen a blade.</span></span> <span data-ttu-id="6f503-155">Här kan du ange hello nya principnamn och hello behörigheter som du vill tooassociate med den här principen enligt hello följande bild:</span><span class="sxs-lookup"><span data-stu-id="6f503-155">Here you can enter hello new policy name and hello permissions that you want tooassociate with this policy, as shown in hello following figure:</span></span>

    <span data-ttu-id="6f503-156">Det finns flera behörigheter som kan associeras med dessa principer för delad.</span><span class="sxs-lookup"><span data-stu-id="6f503-156">There are several permissions that can be associated with these shared policies.</span></span> <span data-ttu-id="6f503-157">Hej **läsa registret** och **registret skrivåtgärder** principer ger Läs- och skrivbehörighet rättigheter toohello identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="6f503-157">hello **Registry read** and **Registry write** policies grant read and write access rights toohello identity registry.</span></span> <span data-ttu-id="6f503-158">Att välja alternativ för skrivning av hello automatiskt väljer hello läsa alternativet.</span><span class="sxs-lookup"><span data-stu-id="6f503-158">Choosing hello write option automatically chooses hello read option.</span></span>

    <span data-ttu-id="6f503-159">Hej **tjänsten ansluta** princip ger behörighet tooaccess slutpunkter som **får enhet till moln**.</span><span class="sxs-lookup"><span data-stu-id="6f503-159">hello **Service connect** policy grants permission tooaccess service endpoints such as **Receive device-to-cloud**.</span></span> <span data-ttu-id="6f503-160">Hej **enhet ansluta** princip ger behörighet för att skicka och ta emot meddelanden med hello IoT-hubb enheten sida slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="6f503-160">hello **Device connect** policy grants permissions for sending and receiving messages using hello IoT Hub device-side endpoints.</span></span>

* <span data-ttu-id="6f503-161">Klicka på **skapa** tooadd detta nyskapad princip toohello befintlig lista.</span><span class="sxs-lookup"><span data-stu-id="6f503-161">Click **Create** tooadd this newly created policy toohello existing list.</span></span>

![][10]

## <a name="endpoints"></a><span data-ttu-id="6f503-162">Slutpunkter</span><span class="sxs-lookup"><span data-stu-id="6f503-162">Endpoints</span></span>

<span data-ttu-id="6f503-163">Klicka på **slutpunkter** toodisplay en lista över slutpunkter för hello IoT-hubb som du ändrar.</span><span class="sxs-lookup"><span data-stu-id="6f503-163">Click **Endpoints** toodisplay a list of endpoints for hello IoT hub that you are modifying.</span></span> <span data-ttu-id="6f503-164">Det finns två typer av slutpunkter: slutpunkter som är inbyggda i hello IoT-hubb och slutpunkter som du lägger till toohello IoT-hubb när skapandet.</span><span class="sxs-lookup"><span data-stu-id="6f503-164">There are two types of endpoints: endpoints that are built into hello IoT hub, and endpoints that you add toohello IoT hub after its creation.</span></span>

![][11]

### <a name="built-in-endpoints"></a><span data-ttu-id="6f503-165">Inbyggda slutpunkter</span><span class="sxs-lookup"><span data-stu-id="6f503-165">Built-in endpoints</span></span>

<span data-ttu-id="6f503-166">Det finns två inbyggda slutpunkter: **molnet toodevice feedback** och **händelser**.</span><span class="sxs-lookup"><span data-stu-id="6f503-166">There are two built-in endpoints: **Cloud toodevice feedback** and **Events**.</span></span>

* <span data-ttu-id="6f503-167">**Molnet toodevice feedback** inställningar: den här inställningen har två subsettings: **molnet tooDevice TTL** (time-to-live) och **kvarhållningstiden** (i timmar) för hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="6f503-167">**Cloud toodevice feedback** settings: This setting has two subsettings: **Cloud tooDevice TTL** (time-to-live) and **Retention time** (in hours) for hello messages.</span></span> <span data-ttu-id="6f503-168">När först skapar en IoT-hubb kan ha båda de här inställningarna hello standardvärdet 1 timme.</span><span class="sxs-lookup"><span data-stu-id="6f503-168">When your first create an IoT hub, both these settings have hello default value of one hour.</span></span> <span data-ttu-id="6f503-169">tooadjust inställningarna, använda Hej reglage eller Skriv hello-värden.</span><span class="sxs-lookup"><span data-stu-id="6f503-169">tooadjust these settings, use hello sliders or type hello values.</span></span>
* <span data-ttu-id="6f503-170">**Händelser** inställningar: den här inställningen har flera subsettings, vilket är skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="6f503-170">**Events** settings: This setting has several subsettings, some of which are read-only.</span></span> <span data-ttu-id="6f503-171">hello följande lista beskrivs de här inställningarna:</span><span class="sxs-lookup"><span data-stu-id="6f503-171">hello following list describes these settings:</span></span>

  * <span data-ttu-id="6f503-172">**Partitioner**: en är standardvärdet när hello IoT-hubben har skapats.</span><span class="sxs-lookup"><span data-stu-id="6f503-172">**Partitions**: A default value is set when hello IoT hub is created.</span></span> <span data-ttu-id="6f503-173">Du kan ändra hello antalet partitioner via den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="6f503-173">You can change hello number of partitions through this setting.</span></span>

  * <span data-ttu-id="6f503-174">**Händelsenamn hubb-kompatibel och slutpunkten**: när hello IoT-hubb skapas en Händelsehubb har skapats internt att du kanske behöver åtkomst till toounder vissa omständigheter.</span><span class="sxs-lookup"><span data-stu-id="6f503-174">**Event Hub-compatible name and endpoint**: When hello IoT hub is created, an Event Hub is created internally that you may need access toounder certain circumstances.</span></span> <span data-ttu-id="6f503-175">Du kan anpassa hello Event Hub-kompatibelt namn och en slutpunkt värden men du kan kopiera dem genom att klicka på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="6f503-175">You cannot customize hello Event Hub-compatible name and endpoint values but you can copy them by clicking **Copy**.</span></span>

  * <span data-ttu-id="6f503-176">**Kvarhållningstiden**: Ange tooone dagen som standard men du kan ändra den med hjälp av hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="6f503-176">**Retention Time**: Set tooone day by default but you can change it using hello drop-down list.</span></span> <span data-ttu-id="6f503-177">Det här värdet är i dagar för inställningen för hello-enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="6f503-177">This value is in days for hello device-to-cloud setting.</span></span>

  * <span data-ttu-id="6f503-178">**Konsumentgrupper**: konsumentgrupper aktivera flera läsare tooread meddelanden oberoende av hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6f503-178">**Consumer Groups**: Consumer groups enable multiple readers tooread messages independently from hello IoT hub.</span></span> <span data-ttu-id="6f503-179">Alla IoT-hubben har skapats med en förinställd konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="6f503-179">Every IoT hub is created with a default consumer group.</span></span> <span data-ttu-id="6f503-180">Du kan lägga till eller ta bort konsumenten grupper tooyour IoT Hub genom att använda den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="6f503-180">However, you can add or delete consumer groups tooyour IoT hubs using this setting.</span></span>

  > [!NOTE]
  > <span data-ttu-id="6f503-181">hello förinställd konsumentgrupp inte redigeras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="6f503-181">hello default consumer group cannot be edited or deleted.</span></span>

### <a name="custom-endpoints"></a><span data-ttu-id="6f503-182">Anpassade slutpunkter</span><span class="sxs-lookup"><span data-stu-id="6f503-182">Custom endpoints</span></span>

<span data-ttu-id="6f503-183">Du kan lägga till anpassade slutpunkter på din IoT-hubb med hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="6f503-183">You can add custom endpoints on your IoT hub using hello portal.</span></span> <span data-ttu-id="6f503-184">Från hello **slutpunkter** bladet, klickar du på **Lägg till** på hello översta tooopen hello **lägga till slutpunkten** bladet.</span><span class="sxs-lookup"><span data-stu-id="6f503-184">From hello **Endpoints** blade, click **Add** at hello top tooopen hello **Add endpoint** blade.</span></span> <span data-ttu-id="6f503-185">Ange information om hello som krävs och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f503-185">Enter hello required information, then click **OK**.</span></span> <span data-ttu-id="6f503-186">Din anpassade slutpunkt visas nu i hello huvudsakliga **slutpunkter** bladet.</span><span class="sxs-lookup"><span data-stu-id="6f503-186">Your custom endpoint is now listed in hello main **Endpoints** blade.</span></span>

![][13]

<span data-ttu-id="6f503-187">Du kan läsa mer om anpassade slutpunkter i [referens - IoT-hubbslutpunkter][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="6f503-187">You can read more about custom endpoints in [Reference - IoT hub endpoints][lnk-devguide-endpoints].</span></span>

## <a name="routes"></a><span data-ttu-id="6f503-188">Vägar</span><span class="sxs-lookup"><span data-stu-id="6f503-188">Routes</span></span>

<span data-ttu-id="6f503-189">Klicka på **vägar** toomanage hur IoT-hubb skickar meddelanden från din enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="6f503-189">Click **Routes** toomanage how IoT Hub dispatches your device-to-cloud messages.</span></span>

![][14]

<span data-ttu-id="6f503-190">Du kan lägga till vägar tooyour IoT-hubb genom att klicka på **Lägg till** hello överst i hello **vägar*** bladet att ange hello krävs information och klicka **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f503-190">You can add routes tooyour IoT hub by clicking **Add** at hello top of hello **Routes*** blade, entering hello required information, and clicking **OK**.</span></span> <span data-ttu-id="6f503-191">Rutten visas sedan i hello huvudsakliga **vägar** bladet.</span><span class="sxs-lookup"><span data-stu-id="6f503-191">Your route is then listed in hello main **Routes** blade.</span></span> <span data-ttu-id="6f503-192">Du kan redigera en väg genom att klicka på hello listan över vägar.</span><span class="sxs-lookup"><span data-stu-id="6f503-192">You can edit a route by clicking it in hello list of routes.</span></span> <span data-ttu-id="6f503-193">tooenable en väg klickar du på hello lista över vägar och ange hello **aktiverad** växla för**av**.</span><span class="sxs-lookup"><span data-stu-id="6f503-193">tooenable a route, click it in hello list of routes and set hello **Enabled** toggle too**Off**.</span></span> <span data-ttu-id="6f503-194">toosave hello ändras, klickar du på **OK** längst hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="6f503-194">toosave hello change, click **OK** at hello bottom of hello blade.</span></span>

![][15]

## <a name="pricing-and-scale"></a><span data-ttu-id="6f503-195">Priser och skalning</span><span class="sxs-lookup"><span data-stu-id="6f503-195">Pricing and scale</span></span>

<span data-ttu-id="6f503-196">hello priser för en befintlig IoT-hubb kan ändras via hello **priser** inställningar med hello följande undantag:</span><span class="sxs-lookup"><span data-stu-id="6f503-196">hello pricing of an existing IoT hub can be changed through hello **Pricing** settings, with hello following exceptions:</span></span>

* <span data-ttu-id="6f503-197">I aktuella hello-implementering måste en IoT-hubb med en kostnadsfri SKU kan inte ändra nivåerna tooone av hello betald SKU: er, och vice versa.</span><span class="sxs-lookup"><span data-stu-id="6f503-197">In hello current implementation, an IoT hub with a free SKU cannot change tiers tooone of hello paid SKUs, or vice versa.</span></span>
* <span data-ttu-id="6f503-198">Det kan bara finnas en kostnadsfria nivån IoT-hubb i hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6f503-198">There can only be one free tier IoT hub in hello Azure subscription.</span></span>

![][12]

<span data-ttu-id="6f503-199">Du kan flytta från en högre nivå toolower endast när hello antal meddelanden skickade den dagen överstiger hello kvot för hello lägre nivå.</span><span class="sxs-lookup"><span data-stu-id="6f503-199">You can move from a higher toolower tier only when hello number of messages sent that day do exceed hello quota for hello lower tier.</span></span> <span data-ttu-id="6f503-200">Till exempel om hello antalet meddelanden per dag överskrider 400 000, sedan hello-nivå för hello IoT-hubb kan ändras.</span><span class="sxs-lookup"><span data-stu-id="6f503-200">For example, if hello number of messages per day exceeds 400,000, then hello tier for hello IoT hub can be changed.</span></span> <span data-ttu-id="6f503-201">Men om du ändrar toohello S1 nivå begränsas sedan hello IoT-hubb för dagen.</span><span class="sxs-lookup"><span data-stu-id="6f503-201">However, if you change toohello S1 tier then hello IoT hub is throttled for that day.</span></span>

## <a name="delete-hello-iot-hub"></a><span data-ttu-id="6f503-202">Ta bort hello IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="6f503-202">Delete hello IoT hub</span></span>

<span data-ttu-id="6f503-203">Du kan bläddra toohello IoT-hubb som du vill toodelete genom att klicka på **Bläddra**, och sedan välja hello lämpliga hubb toodelete.</span><span class="sxs-lookup"><span data-stu-id="6f503-203">You can browse toohello IoT hub you want toodelete by clicking **Browse**, and then choosing hello appropriate hub toodelete.</span></span> <span data-ttu-id="6f503-204">toodelete Hej IoT-hubben, klickar du på hello **ta bort** nedan hello IoT-hubbnamnet.</span><span class="sxs-lookup"><span data-stu-id="6f503-204">toodelete hello IoT hub, click hello **Delete** button below hello IoT hub name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f503-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6f503-205">Next steps</span></span>

<span data-ttu-id="6f503-206">Följ dessa länkar toolearn mer om hur du hanterar Azure IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="6f503-206">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="6f503-207">[Massinläsning hantera IoT-enheter][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="6f503-207">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="6f503-208">[IoT-hubb mått][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="6f503-208">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="6f503-209">[Åtgärder som övervakning][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="6f503-209">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="6f503-210">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="6f503-210">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="6f503-211">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="6f503-211">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="6f503-212">[Simulera en enhet med IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="6f503-212">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="6f503-213">[Skydda din IoT-lösning från hello bakgrund][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="6f503-213">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

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
