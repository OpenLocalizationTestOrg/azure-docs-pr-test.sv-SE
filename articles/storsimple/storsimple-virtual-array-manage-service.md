---
title: "aaaDeploy StorSimple enheten Manager-tjänsten | Microsoft Docs"
description: "Förklarar hur toocreate och ta bort hello StorSimple enheten Manager-tjänsten i hello Azure-portalen och beskriver hur toomanage hello nyckel för tjänstregistrering."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 28499494-8c4d-4a7f-9d44-94b2b8a93c93
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: alkohli
ms.openlocfilehash: 9064053addc7b3dfcce08b47e81b38c2e0e1b559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-device-manager-service-for-storsimple-virtual-array"></a><span data-ttu-id="334a9-103">Distribuera hello StorSimple Enhetshanteraren tjänst för virtuell StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="334a9-103">Deploy hello StorSimple Device Manager service for StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="334a9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="334a9-104">Overview</span></span>

<span data-ttu-id="334a9-105">Hej StorSimple enheten Manager-tjänsten körs i Microsoft Azure och ansluter toomultiple StorSimple-enheter.</span><span class="sxs-lookup"><span data-stu-id="334a9-105">hello StorSimple Device Manager service runs in Microsoft Azure and connects toomultiple StorSimple devices.</span></span> <span data-ttu-id="334a9-106">När du har skapat hello service kan du använda den toomanage hello enheter från hello Microsoft Azure portal körs i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="334a9-106">After you create hello service, you can use it toomanage hello devices from hello Microsoft Azure portal running in a browser.</span></span> <span data-ttu-id="334a9-107">Detta gör att du toomonitor alla hello-enheter som är anslutna toohello StorSimple Enhetshanteraren tjänsten från en enda central plats, vilket minimerar administrativa belastningen.</span><span class="sxs-lookup"><span data-stu-id="334a9-107">This allows you toomonitor all hello devices that are connected toohello StorSimple Device Manager service from a single, central location, thereby minimizing administrative burden.</span></span>

<span data-ttu-id="334a9-108">hello vanliga uppgifter relaterade tooa StorSimple enheten Manager-tjänsten är:</span><span class="sxs-lookup"><span data-stu-id="334a9-108">hello common tasks related tooa StorSimple Device Manager service are:</span></span>

* <span data-ttu-id="334a9-109">Skapa en tjänst</span><span class="sxs-lookup"><span data-stu-id="334a9-109">Create a service</span></span>
* <span data-ttu-id="334a9-110">Ta bort en tjänst</span><span class="sxs-lookup"><span data-stu-id="334a9-110">Delete a service</span></span>
* <span data-ttu-id="334a9-111">Hämta hello nyckel för tjänstregistrering</span><span class="sxs-lookup"><span data-stu-id="334a9-111">Get hello service registration key</span></span>
* <span data-ttu-id="334a9-112">Återskapa hello nyckel för tjänstregistrering</span><span class="sxs-lookup"><span data-stu-id="334a9-112">Regenerate hello service registration key</span></span>

<span data-ttu-id="334a9-113">Den här självstudiekursen beskrivs hur tooperform av hello föregående aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="334a9-113">This tutorial describes how tooperform each of hello preceding tasks.</span></span> <span data-ttu-id="334a9-114">hello informationen i den här artikeln är tillämpliga endast tooStorSimple virtuella matriser.</span><span class="sxs-lookup"><span data-stu-id="334a9-114">hello information contained in this article is applicable only tooStorSimple Virtual Arrays.</span></span> <span data-ttu-id="334a9-115">Mer information om StorSimple 8000-serien finns för[distribuera StorSimple Manager-tjänsten](storsimple-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="334a9-115">For more information on StorSimple 8000 series, go too[deploy a StorSimple Manager service](storsimple-manage-service.md).</span></span>

## <a name="create-a-service"></a><span data-ttu-id="334a9-116">Skapa en tjänst</span><span class="sxs-lookup"><span data-stu-id="334a9-116">Create a service</span></span>

<span data-ttu-id="334a9-117">toocreate en tjänst behöver du toohave:</span><span class="sxs-lookup"><span data-stu-id="334a9-117">toocreate a service, you need toohave:</span></span>

* <span data-ttu-id="334a9-118">En prenumeration med ett Enterprise-avtal</span><span class="sxs-lookup"><span data-stu-id="334a9-118">A subscription with an Enterprise Agreement</span></span>
* <span data-ttu-id="334a9-119">Ett aktivt Microsoft Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="334a9-119">An active Microsoft Azure storage account</span></span>
* <span data-ttu-id="334a9-120">Hej faktureringsinformation som används för hantering</span><span class="sxs-lookup"><span data-stu-id="334a9-120">hello billing information that is used for access management</span></span>

<span data-ttu-id="334a9-121">Du kan också välja toogenerate storage-konto när du skapar hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="334a9-121">You can also choose toogenerate a storage account when you create hello service.</span></span>

<span data-ttu-id="334a9-122">En enskild tjänst kan hantera flera enheter.</span><span class="sxs-lookup"><span data-stu-id="334a9-122">A single service can manage multiple devices.</span></span> <span data-ttu-id="334a9-123">Men en enhet kan sträcka sig över flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="334a9-123">However, a device cannot span multiple services.</span></span> <span data-ttu-id="334a9-124">Stora företag kan ha flera instanser av tjänsten toowork med olika prenumerationer, organisationer eller även distribution platser.</span><span class="sxs-lookup"><span data-stu-id="334a9-124">A large enterprise can have multiple service instances toowork with different subscriptions, organizations, or even deployment locations.</span></span>

> [!NOTE]
> <span data-ttu-id="334a9-125">Du behöver separata instanser av StorSimple Enhetshanteraren service toomanage StorSimple 8000-serien enheter och virtuella StorSimple-matriser.</span><span class="sxs-lookup"><span data-stu-id="334a9-125">You need separate instances of StorSimple Device Manager service toomanage StorSimple 8000 series devices and StorSimple Virtual Arrays.</span></span>


<span data-ttu-id="334a9-126">Utföra hello följande steg toocreate en tjänst.</span><span class="sxs-lookup"><span data-stu-id="334a9-126">Perform hello following steps toocreate a service.</span></span>

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a><span data-ttu-id="334a9-127">Ta bort en tjänst</span><span class="sxs-lookup"><span data-stu-id="334a9-127">Delete a service</span></span>

<span data-ttu-id="334a9-128">Innan du tar bort en tjänst måste du kontrollera att inga anslutna enheter använder den.</span><span class="sxs-lookup"><span data-stu-id="334a9-128">Before you delete a service, make sure that no connected devices are using it.</span></span> <span data-ttu-id="334a9-129">Om tjänsten hello används, inaktivera hello anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="334a9-129">If hello service is in use, deactivate hello connected devices.</span></span> <span data-ttu-id="334a9-130">hello inaktivera åtgärden hello anslutning mellan hello enhets- och hello-servern, men bevara hello enhetsdata i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="334a9-130">hello deactivate operation will sever hello connection between hello device and hello service, but preserve hello device data in hello cloud.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="334a9-131">När en tjänst har tagits bort, kan hello-åtgärden inte ångras.</span><span class="sxs-lookup"><span data-stu-id="334a9-131">After a service is deleted, hello operation cannot be reversed.</span></span> <span data-ttu-id="334a9-132">Alla enheter som använder hello tjänsten behöver toobe fabriksåterställning innan den kan användas med en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="334a9-132">Any device that was using hello service will need toobe factory reset before it can be used with another service.</span></span> <span data-ttu-id="334a9-133">I det här scenariot hello lokala data på hello enhet samt hello konfiguration kommer att förloras.</span><span class="sxs-lookup"><span data-stu-id="334a9-133">In this scenario, hello local data on hello device, as well as hello configuration, will be lost.</span></span>
 

<span data-ttu-id="334a9-134">Utföra hello följande steg toodelete en tjänst.</span><span class="sxs-lookup"><span data-stu-id="334a9-134">Perform hello following steps toodelete a service.</span></span>

#### <a name="toodelete-a-service"></a><span data-ttu-id="334a9-135">toodelete en tjänst</span><span class="sxs-lookup"><span data-stu-id="334a9-135">toodelete a service</span></span>

1. <span data-ttu-id="334a9-136">Gå för**alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="334a9-136">Go too**All resources**.</span></span> <span data-ttu-id="334a9-137">Sök efter StorSimple Device Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="334a9-137">Search for your StorSimple Device Manager service.</span></span> <span data-ttu-id="334a9-138">Välj hello-tjänsten som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="334a9-138">Select hello service that you wish toodelete.</span></span>
   
    ![Välj tjänsten toodelete](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. <span data-ttu-id="334a9-140">Gå tooyour service instrumentpanelen tooensure det finns inga enheter anslutna toohello service.</span><span class="sxs-lookup"><span data-stu-id="334a9-140">Go tooyour service dashboard tooensure there are no devices connected toohello service.</span></span> <span data-ttu-id="334a9-141">Om det finns inga enheter som registrerats med den här tjänsten kan visas du dessutom en banderoll visas toohello effekt.</span><span class="sxs-lookup"><span data-stu-id="334a9-141">If there are no devices registered with this service, you will also see a banner message toohello effect.</span></span> <span data-ttu-id="334a9-142">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="334a9-142">Click **Delete**.</span></span>
   
    ![Ta bort tjänsten](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. <span data-ttu-id="334a9-144">När du uppmanas att bekräfta, klickar du på **Ja** i hello meddelande om bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="334a9-144">When prompted for confirmation, click **Yes** in hello confirmation notification.</span></span> 
   
    ![Bekräfta borttagning av tjänsten](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. <span data-ttu-id="334a9-146">Det kan ta några minuter för hello service toobe tas bort.</span><span class="sxs-lookup"><span data-stu-id="334a9-146">It may take a few minutes for hello service toobe deleted.</span></span> <span data-ttu-id="334a9-147">Efter hello service är har tagits bort, du får ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="334a9-147">After hello service is successfully deleted, you will be notified.</span></span>
   
    ![Lyckad tjänsten tas bort](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

<span data-ttu-id="334a9-149">hello lista över tjänster kommer att uppdateras.</span><span class="sxs-lookup"><span data-stu-id="334a9-149">hello list of services will be refreshed.</span></span>

 ![Uppdaterade listan över tjänster](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-hello-service-registration-key"></a><span data-ttu-id="334a9-151">Hämta hello nyckel för tjänstregistrering</span><span class="sxs-lookup"><span data-stu-id="334a9-151">Get hello service registration key</span></span>
<span data-ttu-id="334a9-152">När du har skapat en tjänst måste tooregister din StorSimple-enhet med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="334a9-152">After you have successfully created a service, you will need tooregister your StorSimple device with hello service.</span></span> <span data-ttu-id="334a9-153">tooregister din första StorSimple-enhet du behöver hello nyckel för tjänstregistrering.</span><span class="sxs-lookup"><span data-stu-id="334a9-153">tooregister your first StorSimple device, you will need hello service registration key.</span></span> <span data-ttu-id="334a9-154">tooregister ytterligare enheter med en befintlig StorSimple-tjänst, måste både hello registreringsnyckel och hello krypteringsnyckel för tjänstdata (som genereras på hello första enheten under registreringen).</span><span class="sxs-lookup"><span data-stu-id="334a9-154">tooregister additional devices with an existing StorSimple service, you will need both hello registration key and hello service data encryption key (which is generated on hello first device during registration).</span></span> <span data-ttu-id="334a9-155">Läs mer om hello krypteringsnyckel för tjänstdata [StorSimple-säkerhet](storsimple-security.md).</span><span class="sxs-lookup"><span data-stu-id="334a9-155">For more information about hello service data encryption key, see [StorSimple security](storsimple-security.md).</span></span> <span data-ttu-id="334a9-156">Du kan hämta hello registreringsnyckel genom att öppna hello **nycklar** bladet för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="334a9-156">You can get hello registration key by accessing hello **Keys** blade for your service.</span></span>

<span data-ttu-id="334a9-157">Utför följande steg tooget hello tjänstens registreringsnyckel hello.</span><span class="sxs-lookup"><span data-stu-id="334a9-157">Perform hello following steps tooget hello service registration key.</span></span>

#### <a name="tooget-hello-service-registration-key"></a><span data-ttu-id="334a9-158">tooget hello nyckel för tjänstregistrering</span><span class="sxs-lookup"><span data-stu-id="334a9-158">tooget hello service registration key</span></span>
1. <span data-ttu-id="334a9-159">I hello **StorSimple Enhetshanteraren** bladet går för**Management &gt;**  **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="334a9-159">In hello **StorSimple Device Manager** blade, go too**Management &gt;** **Keys**.</span></span>
   
   ![Bladet Nycklar](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. <span data-ttu-id="334a9-161">I hello **nycklar** bladet en nyckel för tjänstregistrering visas.</span><span class="sxs-lookup"><span data-stu-id="334a9-161">In hello **Keys** blade, a service registration key appears.</span></span> <span data-ttu-id="334a9-162">Kopiera hello registreringsnyckel med hello kopiera-ikonen.</span><span class="sxs-lookup"><span data-stu-id="334a9-162">Copy hello registration key using hello copy icon.</span></span> 

<span data-ttu-id="334a9-163">Behåll hello nyckel för tjänstregistrering på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="334a9-163">Keep hello service registration key in a safe location.</span></span> <span data-ttu-id="334a9-164">Du behöver den här nyckeln, samt hello krypteringsnyckel för tjänstdata, tooregister ytterligare enheter med den här tjänsten.</span><span class="sxs-lookup"><span data-stu-id="334a9-164">You will need this key, as well as hello service data encryption key, tooregister additional devices with this service.</span></span> <span data-ttu-id="334a9-165">När du har fått hello nyckel för tjänstregistrering måste tooconfigure enheten via hello Windows PowerShell för StorSimple-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="334a9-165">After obtaining hello service registration key, you will need tooconfigure your device through hello Windows PowerShell for StorSimple interface.</span></span>

## <a name="regenerate-hello-service-registration-key"></a><span data-ttu-id="334a9-166">Återskapa hello nyckel för tjänstregistrering</span><span class="sxs-lookup"><span data-stu-id="334a9-166">Regenerate hello service registration key</span></span>
<span data-ttu-id="334a9-167">Du behöver tooregenerate en nyckel för tjänstregistrering om du är obligatoriska tooperform viktiga rotation eller om hello listan över tjänstadministratörer har ändrats.</span><span class="sxs-lookup"><span data-stu-id="334a9-167">You will need tooregenerate a service registration key if you are required tooperform key rotation or if hello list of service administrators has changed.</span></span> <span data-ttu-id="334a9-168">När du återskapar nyckeln hello används hello nya nyckeln bara för att registrera följande enheter.</span><span class="sxs-lookup"><span data-stu-id="334a9-168">When you regenerate hello key, hello new key is used only for registering subsequent devices.</span></span> <span data-ttu-id="334a9-169">hello-enheter som redan har registrerat påverkas inte av den här processen.</span><span class="sxs-lookup"><span data-stu-id="334a9-169">hello devices that were already registered are unaffected by this process.</span></span>

<span data-ttu-id="334a9-170">Utför följande steg tooregenerate en nyckel för tjänstregistrering hello.</span><span class="sxs-lookup"><span data-stu-id="334a9-170">Perform hello following steps tooregenerate a service registration key.</span></span>

#### <a name="tooregenerate-hello-service-registration-key"></a><span data-ttu-id="334a9-171">tooregenerate hello nyckel för tjänstregistrering</span><span class="sxs-lookup"><span data-stu-id="334a9-171">tooregenerate hello service registration key</span></span>
1. <span data-ttu-id="334a9-172">I hello **StorSimple Enhetshanteraren** bladet går för**Management &gt;**  **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="334a9-172">In hello **StorSimple Device Manager** blade, go too**Management &gt;** **Keys**.</span></span>
   
   ![Bladet Nycklar](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. <span data-ttu-id="334a9-174">I hello **nycklar** bladet, klickar du på **återskapa**.</span><span class="sxs-lookup"><span data-stu-id="334a9-174">In hello **Keys** blade, click **Regenerate**.</span></span>
   
   ![Klicka på Återskapa](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. <span data-ttu-id="334a9-176">I hello **Generera nyckel för tjänstregistrering** bladet granska hello åtgärd krävs när hello nycklar genereras om.</span><span class="sxs-lookup"><span data-stu-id="334a9-176">In hello **Regenerate service registration key** blade, review hello action required when hello keys are regenerated.</span></span> <span data-ttu-id="334a9-177">Alla efterföljande hello-enheter som registreras med tjänsten ska använda hello ny registreringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="334a9-177">All hello subsequent devices that are registered with this service will use hello new registration key.</span></span> <span data-ttu-id="334a9-178">Klicka på **återskapa** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="334a9-178">Click **Regenerate** tooconfirm.</span></span> <span data-ttu-id="334a9-179">Du meddelas när hello registreringen är slutförd.</span><span class="sxs-lookup"><span data-stu-id="334a9-179">You will be notified after hello registration is complete.</span></span>
   
   ![Bekräfta Generera nyckel](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. <span data-ttu-id="334a9-181">En ny nyckel för tjänstregistrering visas.</span><span class="sxs-lookup"><span data-stu-id="334a9-181">A new service registration key will appear.</span></span>
   
    ![Bekräfta Generera nyckel](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   <span data-ttu-id="334a9-183">Kopiera den här nyckeln och spara den för att registrera nya enheter med den här tjänsten.</span><span class="sxs-lookup"><span data-stu-id="334a9-183">Copy this key and save it for registering any new devices with this service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="334a9-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="334a9-184">Next steps</span></span>
* <span data-ttu-id="334a9-185">Lär dig hur för[Kom igång](storsimple-virtual-array-deploy1-portal-prep.md) med en virtuell StorSimple-matris.</span><span class="sxs-lookup"><span data-stu-id="334a9-185">Learn how too[get started](storsimple-virtual-array-deploy1-portal-prep.md) with a StorSimple Virtual Array.</span></span>
* <span data-ttu-id="334a9-186">Lär dig hur för[administrera din StorSimple-enhet](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="334a9-186">Learn how too[administer your StorSimple device](storsimple-ova-web-ui-admin.md).</span></span>

