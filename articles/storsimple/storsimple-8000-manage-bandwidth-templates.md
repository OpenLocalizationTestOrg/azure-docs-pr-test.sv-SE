---
title: "aaaManage bandbredd mallar för StorSimple 8000-serien | Microsoft Docs"
description: "Beskriver hur toomanage StorSimple bandbredd mallar, vilket gör att du toocontrol bandbreddsanvändning."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: ebcd1824d7bb9e4c235194c04edbfe8001a3e794
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-storsimple-bandwidth-templates"></a><span data-ttu-id="2e3fd-103">Använd hello StorSimple Enhetshanteraren service toomanage StorSimple bandbredd mallar</span><span class="sxs-lookup"><span data-stu-id="2e3fd-103">Use hello StorSimple Device Manager service toomanage StorSimple bandwidth templates</span></span>

## <a name="overview"></a><span data-ttu-id="2e3fd-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2e3fd-104">Overview</span></span>

<span data-ttu-id="2e3fd-105">Bandbredd-mallar kan du tooconfigure nätverkets bandbredd över flera tid på dagen scheman tootier hello data från hello StorSimple enhet toohello moln.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-105">Bandwidth templates allow you tooconfigure network bandwidth usage across multiple time-of-day schedules tootier hello data from hello StorSimple device toohello cloud.</span></span>

<span data-ttu-id="2e3fd-106">Med bandbreddsbegränsning scheman kan du:</span><span class="sxs-lookup"><span data-stu-id="2e3fd-106">With bandwidth throttling schedules you can:</span></span>

* <span data-ttu-id="2e3fd-107">Ange anpassade bandbredd scheman beroende på hello arbetsbelastning nätverket användningsområden.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-107">Specify customized bandwidth schedules depending on hello workload network usages.</span></span>
* <span data-ttu-id="2e3fd-108">Centralisera hanteringen och återanvända hello scheman över flera enheter på ett enkelt och smidigt sätt.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-108">Centralize management and reuse hello schedules across multiple devices in an easy and seamless manner.</span></span>

> [!NOTE]
> <span data-ttu-id="2e3fd-109">Den här funktionen är tillgänglig endast för fysiska StorSimple-enheter (modeller 8100 och 8600) och inte för StorSimple moln installationer (modeller 8010 och 8020).</span><span class="sxs-lookup"><span data-stu-id="2e3fd-109">This feature is available only for StorSimple physical devices (models 8100 and 8600) and not for StorSimple Cloud Appliances (models 8010 and 8020).</span></span>


## <a name="hello-bandwidth-templates-blade"></a><span data-ttu-id="2e3fd-110">hello bandbredd mallar bladet</span><span class="sxs-lookup"><span data-stu-id="2e3fd-110">hello Bandwidth templates blade</span></span>

<span data-ttu-id="2e3fd-111">Hej **bandbredd mallar** bladet har alla hello bandbredd mallar för tjänsten i tabellformat och innehåller hello följande information:</span><span class="sxs-lookup"><span data-stu-id="2e3fd-111">hello **Bandwidth templates** blade has all hello bandwidth templates for your service in a tabular format, and contains hello following information:</span></span>

* <span data-ttu-id="2e3fd-112">**Namnet** – ett unikt namn som tilldelats toohello bandbredd mallen när den skapades.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-112">**Name** – A unique name assigned toohello bandwidth template when it was created.</span></span>
* <span data-ttu-id="2e3fd-113">**Schemat** – hello antalet scheman som finns i en viss bandbreddsmall.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-113">**Schedule** – hello number of schedules contained in a given bandwidth template.</span></span>
* <span data-ttu-id="2e3fd-114">**Används av** – hello antalet volymer med hello bandbredd mallar.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-114">**Used by** – hello number of volumes using hello bandwidth templates.</span></span>

<span data-ttu-id="2e3fd-115">Du kan också hitta ytterligare information toohelp konfigurera bandbredd mallar i:</span><span class="sxs-lookup"><span data-stu-id="2e3fd-115">You can also find additional information toohelp configure bandwidth templates in:</span></span>

* [<span data-ttu-id="2e3fd-116">Frågor och svar om bandbredd mallar</span><span class="sxs-lookup"><span data-stu-id="2e3fd-116">Questions and answers about bandwidth templates</span></span>](#questions-and-answers-about-bandwidth-templates)
* [<span data-ttu-id="2e3fd-117">Metodtips för bandbredd mallar</span><span class="sxs-lookup"><span data-stu-id="2e3fd-117">Best practices for bandwidth templates</span></span>](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a><span data-ttu-id="2e3fd-118">Lägg till en bandbreddsmall</span><span class="sxs-lookup"><span data-stu-id="2e3fd-118">Add a bandwidth template</span></span>

<span data-ttu-id="2e3fd-119">Utför följande steg toocreate en ny bandbreddsmall för hello.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-119">Perform hello following steps toocreate a new bandwidth template.</span></span>

#### <a name="tooadd-a-bandwidth-template"></a><span data-ttu-id="2e3fd-120">tooadd en bandbreddsmall</span><span class="sxs-lookup"><span data-stu-id="2e3fd-120">tooadd a bandwidth template</span></span>

1. <span data-ttu-id="2e3fd-121">Gå tooyour StorSimple Enhetshanteraren service **bandbredd mallar** och klicka sedan på **+ Lägg till bandbreddsmall**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-121">Go tooyour StorSimple Device Manager service, click **Bandwidth templates** and then click **+ Add Bandwidth template**.</span></span>

    ![Klicka på + Lägg till bandbreddsmall](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. <span data-ttu-id="2e3fd-123">I hello **Lägg till bandbreddsmall** bladet hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2e3fd-123">In hello **Add bandwidth template** blade, do hello following steps:</span></span>
   
    1. <span data-ttu-id="2e3fd-124">Ange ett unikt namn för mallen för bandbredd.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-124">Specify a unique name for your bandwidth template.</span></span>
    2. <span data-ttu-id="2e3fd-125">Definiera ett schema för bandbredd.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-125">Define a bandwidth schedule.</span></span> <span data-ttu-id="2e3fd-126">toocreate ett schema:</span><span class="sxs-lookup"><span data-stu-id="2e3fd-126">toocreate a schedule:</span></span>
   
        1. <span data-ttu-id="2e3fd-127">Hello nedrullningsbara listan och väljer hello **dagar** av hello vecka hello schema har konfigurerats för.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-127">From hello drop-down list, choose hello **Days** of hello week hello schedule is configured for.</span></span> <span data-ttu-id="2e3fd-128">Du kan ange flera dagar.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-128">You can select multiple days.</span></span>        
        
        2. <span data-ttu-id="2e3fd-129">Ange en **starttid** i _hh: mm_ format.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-129">Enter a **Start Time** in _hh:mm_ format.</span></span> <span data-ttu-id="2e3fd-130">Detta är när hello schema börjar.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-130">This is when hello schedule will begin.</span></span>

        3. <span data-ttu-id="2e3fd-131">Ange en **sluttid** i _hh: mm_ format.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-131">Enter an **End Time** in _hh:mm_ format.</span></span> <span data-ttu-id="2e3fd-132">Detta är när hello schema stoppas.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-132">This is when hello schedule will stop.</span></span>
      
           > [!NOTE]
           > <span data-ttu-id="2e3fd-133">Överlappande scheman tillåts inte.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-133">Overlapping schedules are not allowed.</span></span> <span data-ttu-id="2e3fd-134">Om hello start- och sluttider resulterar i ett överlappande schema, visas en fel meddelande toothat effekt.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-134">If hello start and end times will result in an overlapping schedule, you will see an error message toothat effect.</span></span>

        4. <span data-ttu-id="2e3fd-135">Ange hello **bandbredd hastighet**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-135">Specify hello **Bandwidth Rate**.</span></span> <span data-ttu-id="2e3fd-136">Detta är hello bandbredd i megabit per sekund (Mbps) som används av din StorSimple-enhet i åtgärder som rör hello molnet (överföringar och hämtningsbara filer).</span><span class="sxs-lookup"><span data-stu-id="2e3fd-136">This is hello bandwidth in Megabits per second (Mbps) used by your StorSimple device in operations involving hello cloud (both uploads and downloads).</span></span> <span data-ttu-id="2e3fd-137">Ange ett tal mellan 1 och 1000 för det här fältet.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-137">Supply a number between 1 and 1,000 for this field.</span></span>

            ![Definiera bandbredd schema](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            <span data-ttu-id="2e3fd-139">Upprepa hello senare steg toodefine flera scheman för mallen tills du är klar.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-139">Repeat hello above steps toodefine multiple schedules for your template until you are done.</span></span>

        5. <span data-ttu-id="2e3fd-140">Klicka på **Lägg till** toostart skapar en bandbreddsmall.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-140">Click **Add** toostart creating a bandwidth template.</span></span> <span data-ttu-id="2e3fd-141">hello skapat mall läggs toohello lista över mallar för bandbredd.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-141">hello created template is added toohello list of bandwidth templates.</span></span>
      

## <a name="edit-a-bandwidth-template"></a><span data-ttu-id="2e3fd-142">Redigera en bandbreddsmall</span><span class="sxs-lookup"><span data-stu-id="2e3fd-142">Edit a bandwidth template</span></span>

<span data-ttu-id="2e3fd-143">Utför följande steg tooedit en bandbreddsmall hello.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-143">Perform hello following steps tooedit a bandwidth template.</span></span>

### <a name="tooedit-a-bandwidth-template"></a><span data-ttu-id="2e3fd-144">tooedit en bandbreddsmall</span><span class="sxs-lookup"><span data-stu-id="2e3fd-144">tooedit a bandwidth template</span></span>

1. <span data-ttu-id="2e3fd-145">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **bandbredd mallar**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-145">Go tooyour StorSimple Device Manager service and click **Bandwidth templates**.</span></span>
2. <span data-ttu-id="2e3fd-146">Välj hello-mallen som du vill toodelete i hello lista bandbredd mallar.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-146">In hello list of bandwidth templates, select hello template you wish toodelete.</span></span> <span data-ttu-id="2e3fd-147">Högerklicka på och hello snabbmenyn, Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-147">Right-click and from hello context menu, select **Delete**.</span></span>
3. <span data-ttu-id="2e3fd-148">När du uppmanas att bekräfta, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-148">When prompted for confirmation, click **OK**.</span></span> <span data-ttu-id="2e3fd-149">Det bör ta bort hello bandbreddsmall.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-149">This should delete hello bandwidth template.</span></span> 
4. <span data-ttu-id="2e3fd-150">hello lista över bandbredd mallar uppdateras tooreflect hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-150">hello list of bandwidth templates updates tooreflect hello deletion.</span></span>

> [!NOTE]
> <span data-ttu-id="2e3fd-151">Du kan inte spara ändringarna om hello redigerade schemat överlappar ett befintligt schema i hello bandbreddsmall som du ändrar.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-151">You cannot save your changes if hello edited schedule overlaps with an existing schedule in hello bandwidth template that you are modifying.</span></span>

## <a name="delete-a-bandwidth-template"></a><span data-ttu-id="2e3fd-152">Ta bort en bandbreddsmall</span><span class="sxs-lookup"><span data-stu-id="2e3fd-152">Delete a bandwidth template</span></span>

<span data-ttu-id="2e3fd-153">Utför följande steg toodelete en bandbreddsmall hello.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-153">Perform hello following steps toodelete a bandwidth template.</span></span>

#### <a name="toodelete-a-bandwidth-template"></a><span data-ttu-id="2e3fd-154">toodelete en bandbreddsmall</span><span class="sxs-lookup"><span data-stu-id="2e3fd-154">toodelete a bandwidth template</span></span>

1. <span data-ttu-id="2e3fd-155">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **bandbredd mallar**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-155">Go tooyour StorSimple Device Manager service and click **Bandwidth templates**.</span></span>
2. <span data-ttu-id="2e3fd-156">Välj hello-mallen som du vill toodelete i hello lista bandbredd mallar.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-156">In hello list of bandwidth templates, select hello template you wish toodelete.</span></span> <span data-ttu-id="2e3fd-157">Högerklicka och välj Ta bort hello snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-157">Right-click and from hello context menu, select Delete.</span></span>
3. <span data-ttu-id="2e3fd-158">När du uppmanas att bekräfta, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-158">When prompted for confirmation, click **OK**.</span></span> <span data-ttu-id="2e3fd-159">Det bör ta bort hello bandbreddsmall.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-159">This should delete hello bandwidth template.</span></span>
4. <span data-ttu-id="2e3fd-160">hello lista över bandbredd mallar uppdateras tooreflect hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-160">hello list of bandwidth templates updates tooreflect hello deletion.</span></span>

<span data-ttu-id="2e3fd-161">Om hello mall är av någon volymerna, du kommer inte toodelete den.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-161">If hello template is in use by any volume(s), you will not be allowed toodelete it.</span></span> <span data-ttu-id="2e3fd-162">Du ser ett felmeddelande om att hello mallen används.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-162">You will see an error message indicating that hello template is in use.</span></span> <span data-ttu-id="2e3fd-163">En dialogrutan med felmeddelandet visas som talar om att alla hello referenser toohello mallen bör tas bort.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-163">An error message dialog box will appear advising you that all hello references toohello template should be removed.</span></span>

<span data-ttu-id="2e3fd-164">Du kan ta bort alla hello referenser toohello mallen genom att öppna hello **Volymbehållare** sidan och ändra hello volymbehållare som använder den här mallen så att de använder en annan mall eller använda en anpassad eller obegränsad bandbredd inställningen.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-164">You can delete all hello references toohello template by accessing hello **Volume Containers** page and modifying hello volume containers that use this template so that they use another template or use a custom or unlimited bandwidth setting.</span></span> <span data-ttu-id="2e3fd-165">När alla hello referenser har tagits bort kan du ta bort hello mallen.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-165">When all hello references have been removed, you can delete hello template.</span></span>

## <a name="use-a-default-bandwidth-template"></a><span data-ttu-id="2e3fd-166">Använda en standardmall för bandbredd</span><span class="sxs-lookup"><span data-stu-id="2e3fd-166">Use a default bandwidth template</span></span>

<span data-ttu-id="2e3fd-167">En standardmall för bandbredd tillhandahålls och används av volymbehållare av standardkontrollerna tooenforce bandbredd vid åtkomst till hello molnet.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-167">A default bandwidth template is provided and is used by volume containers by default tooenforce bandwidth controls when accessing hello cloud.</span></span> <span data-ttu-id="2e3fd-168">hello standardmallen fungerar också som en referens som är redo för användare skapar egna mallar.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-168">hello default template also serves as a ready reference for users who create their own templates.</span></span> <span data-ttu-id="2e3fd-169">hello information om den här standardmallen är:</span><span class="sxs-lookup"><span data-stu-id="2e3fd-169">hello details of this default template are:</span></span>

* <span data-ttu-id="2e3fd-170">**Namnet** – obegränsade kvällar och helger</span><span class="sxs-lookup"><span data-stu-id="2e3fd-170">**Name** – Unlimited nights and weekends</span></span>
* <span data-ttu-id="2e3fd-171">**Schemat** – ett enda schema från måndag tooFriday som gäller en bandbredd andel 1 Mbit/s mellan 8: 00 och 17: 00 tid.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-171">**Schedule** – A single schedule from Monday tooFriday that applies a bandwidth rate of 1 Mbps between 8 AM and 5 PM device time.</span></span> <span data-ttu-id="2e3fd-172">hello bandbredd är inställd tooUnlimited för hello resten av hello vecka.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-172">hello bandwidth is set tooUnlimited for hello remainder of hello week.</span></span>

<span data-ttu-id="2e3fd-173">hello standardmallen kan redigeras.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-173">hello default template can be edited.</span></span> <span data-ttu-id="2e3fd-174">hello användning av den här mallen (inklusive redigerade versioner) spåras.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-174">hello usage of this template (including edited versions) is tracked.</span></span>

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a><span data-ttu-id="2e3fd-175">Skapa en varar bandbreddsmall som börjar vid en angiven tid</span><span class="sxs-lookup"><span data-stu-id="2e3fd-175">Create an all-day bandwidth template that starts at a specified time</span></span>

<span data-ttu-id="2e3fd-176">Följ den här proceduren toocreate ett schema som startar vid en viss tidpunkt och kör hela dagen.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-176">Follow this procedure toocreate a schedule that starts at a specified time and runs all day.</span></span> <span data-ttu-id="2e3fd-177">I exemplet hello hello schema börjar vid 9: 00 hello morgonen och körs förrän 9: 00 hello nästa morgon.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-177">In hello example, hello schedule starts at 9 AM in hello morning and runs until 9 AM hello next morning.</span></span> <span data-ttu-id="2e3fd-178">Det är viktigt toonote som hello start och sluttider för ett visst schema måste båda finnas på hello samma 24-timmarsformat schema och kan sträcka sig över flera dagar.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-178">It's important toonote that hello start and end times for a given schedule must both be contained on hello same 24 hour schedule and cannot span multiple days.</span></span> <span data-ttu-id="2e3fd-179">Om du behöver tooset bandbredd mallar som sträcker sig över flera dagar, måste toouse flera scheman (som visas i exemplet hello).</span><span class="sxs-lookup"><span data-stu-id="2e3fd-179">If you need tooset up bandwidth templates that span multiple days, you will need toouse multiple schedules (as shown in hello example).</span></span>

#### <a name="toocreate-an-all-day-bandwidth-template"></a><span data-ttu-id="2e3fd-180">toocreate en varar bandbreddsmall</span><span class="sxs-lookup"><span data-stu-id="2e3fd-180">toocreate an all-day bandwidth template</span></span>

1. <span data-ttu-id="2e3fd-181">Skapa ett schema som börjar vid 9: 00 hello morgonen och kör till midnatt.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-181">Create a schedule that starts at 9 AM in hello morning and runs until midnight.</span></span>
2. <span data-ttu-id="2e3fd-182">Lägg till ett annat schema.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-182">Add another schedule.</span></span> <span data-ttu-id="2e3fd-183">Konfigurera hello andra schema toorun från midnatt tills 9: 00 hello morgonen.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-183">Configure hello second schedule toorun from midnight until 9 AM in hello morning.</span></span>
3. <span data-ttu-id="2e3fd-184">Spara hello bandbreddsmall.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-184">Save hello bandwidth template.</span></span>

<span data-ttu-id="2e3fd-185">hello sammansatt schema kommer sedan att starta i taget väljer och kör varar.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-185">hello composite schedule will then start at a time of your choosing and run all-day.</span></span>

## <a name="questions-and-answers-about-bandwidth-templates"></a><span data-ttu-id="2e3fd-186">Frågor och svar om bandbredd mallar</span><span class="sxs-lookup"><span data-stu-id="2e3fd-186">Questions and answers about bandwidth templates</span></span>

<span data-ttu-id="2e3fd-187">**Q**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-187">**Q**.</span></span> <span data-ttu-id="2e3fd-188">Vad händer toobandwidth kontroller när du är between hello scheman?</span><span class="sxs-lookup"><span data-stu-id="2e3fd-188">What happens toobandwidth controls when you are in between hello schedules?</span></span> <span data-ttu-id="2e3fd-189">(Ett schema har avslutats och en annan har inte startat ännu.)</span><span class="sxs-lookup"><span data-stu-id="2e3fd-189">(A schedule has ended and another one has not started yet.)</span></span>

<span data-ttu-id="2e3fd-190">**EN**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-190">**A**.</span></span> <span data-ttu-id="2e3fd-191">I sådana fall kommer inga bandbreddskontroller användas.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-191">In such cases, no bandwidth controls will be employed.</span></span> <span data-ttu-id="2e3fd-192">Detta innebär att hello-enheten kan använda obegränsad bandbredd när skiktning data toohello moln.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-192">This means that hello device can use unlimited bandwidth when tiering data toohello cloud.</span></span>

<span data-ttu-id="2e3fd-193">**Q**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-193">**Q**.</span></span> <span data-ttu-id="2e3fd-194">Kan du ändra bandbredd mallar på en offline-enhet?</span><span class="sxs-lookup"><span data-stu-id="2e3fd-194">Can you modify bandwidth templates on an offline device?</span></span>

<span data-ttu-id="2e3fd-195">**EN**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-195">**A**.</span></span> <span data-ttu-id="2e3fd-196">Du kommer inte att kunna toomodify bandbredd mallar på volymer behållare om motsvarande hello-enheten är offline.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-196">You will not be able toomodify bandwidth templates on volumes containers if hello corresponding device is offline.</span></span>

<span data-ttu-id="2e3fd-197">**Q**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-197">**Q**.</span></span> <span data-ttu-id="2e3fd-198">Kan du redigera en bandbreddsmall som är associerade med en volymbehållare när hello associerade volymer är offline?</span><span class="sxs-lookup"><span data-stu-id="2e3fd-198">Can you edit a bandwidth template associated with a volume container when hello associated volumes are offline?</span></span>

<span data-ttu-id="2e3fd-199">**EN**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-199">**A**.</span></span> <span data-ttu-id="2e3fd-200">Du kan ändra en bandbreddsmall som är associerade med en volymbehållare vars volymer som är offline.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-200">You can modify a bandwidth template associated with a volume container whose volumes are offline.</span></span> <span data-ttu-id="2e3fd-201">Observera att när volymer är offline, inga data inte kommer nivåindelas från hello enhet toohello moln.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-201">Note that when volumes are offline, no data will be tiered from hello device toohello cloud.</span></span>

<span data-ttu-id="2e3fd-202">**Q**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-202">**Q**.</span></span> <span data-ttu-id="2e3fd-203">Kan du ta bort en standardmall?</span><span class="sxs-lookup"><span data-stu-id="2e3fd-203">Can you delete a default template?</span></span>

<span data-ttu-id="2e3fd-204">**EN**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-204">**A**.</span></span> <span data-ttu-id="2e3fd-205">Du kan ta bort en standardmall, är det inte en bra idé toodo så.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-205">Although you can delete a default template, it is not a good idea toodo so.</span></span> <span data-ttu-id="2e3fd-206">hello användning av en standardmall, inklusive redigerade versionerna spåras.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-206">hello usage of a default template, including edited versions, is tracked.</span></span> <span data-ttu-id="2e3fd-207">hello spårningsdata analyseras och över hello förstås tid är används tooimprove hello standardmallen.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-207">hello tracking data is analyzed and over hello course of time, is used tooimprove hello default template.</span></span>

<span data-ttu-id="2e3fd-208">**Q**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-208">**Q**.</span></span> <span data-ttu-id="2e3fd-209">Hur vet du att mallarna bandbredd måste toobe ändras?</span><span class="sxs-lookup"><span data-stu-id="2e3fd-209">How do you determine that your bandwidth templates need toobe modified?</span></span>

<span data-ttu-id="2e3fd-210">**EN**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-210">**A**.</span></span> <span data-ttu-id="2e3fd-211">En av hello tecken som du behöver toomodify hello bandbredd mallar är när du startar ser hello nätverket långsammare eller strypning flera gånger under en dag.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-211">One of hello signs that you need toomodify hello bandwidth templates is when you start seeing hello network slow down or choke multiple times in a day.</span></span> <span data-ttu-id="2e3fd-212">Om det händer kan du övervaka hello lagrings- och nätverk genom att titta på hello diagram för i/o-prestanda och dataflödet i nätverket.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-212">If this happens, monitor hello storage and usage network by looking at hello I/O Performance and Network Throughput charts.</span></span>

<span data-ttu-id="2e3fd-213">Identifiera hello tid på dagen från hello för genomströmning nätverksdata och hello volymbehållare i vilken hello inträffar flaskhalsar i nätverket.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-213">From hello network throughput data, identify hello time of day and hello volume containers in which hello network bottleneck occurs.</span></span> <span data-ttu-id="2e3fd-214">Om detta händer när data håller på att nivåindelade toohello molnet (hämta informationen från i/o-prestanda för alla volymbehållare för enheten toocloud), behöver du toomodify hello bandbredd mallar som är associerade med din volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-214">If this happens when data is being tiered toohello cloud (get this information from I/O performance for all volume containers for device toocloud), then you will need toomodify hello bandwidth templates associated with your volume containers.</span></span>

<span data-ttu-id="2e3fd-215">När hello ändrats mallar används så behöver du toomonitor hello nätverket igen för betydande fördröjningar.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-215">After hello modified templates are in use, you will need toomonitor hello network again for significant latencies.</span></span> <span data-ttu-id="2e3fd-216">Om de fortfarande finns måste toorevisit bandbredd mallarna.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-216">If these still exist, then you will need toorevisit your bandwidth templates.</span></span>

<span data-ttu-id="2e3fd-217">**Q**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-217">**Q**.</span></span> <span data-ttu-id="2e3fd-218">Vad händer om flera volymbehållare på enheten har schemalägger som överlappar varandra men olika begränsningar gäller tooeach?</span><span class="sxs-lookup"><span data-stu-id="2e3fd-218">What happens if multiple volume containers on my device have schedules that overlap but different limits apply tooeach?</span></span>

<span data-ttu-id="2e3fd-219">**EN**.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-219">**A**.</span></span> <span data-ttu-id="2e3fd-220">Anta att du har en enhet med 3 volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-220">Let's assume that you have a device with 3 volume containers.</span></span> <span data-ttu-id="2e3fd-221">hello scheman som är associerade med de här behållarna helt överlappar varandra.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-221">hello schedules associated with these containers completely overlap.</span></span> <span data-ttu-id="2e3fd-222">För var och en av de här behållarna är hello bandbreddsgränser som används 5, 10, och 15 Mbit/s.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-222">For each of these containers, hello bandwidth limits used are 5, 10, and 15 Mbps respectively.</span></span> <span data-ttu-id="2e3fd-223">När i/o uppstår på alla dessa behållare på hello tillämpas samtidigt, hello minst hello 3 bandbreddsgränser: i det här fallet 5 Mbit/s som dessa utgående i/o-begäranden resursen hello samma kö.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-223">When I/O are occurring on all of these containers at hello same time, hello minimum of hello 3 bandwidth limits may be applied: in this case, 5 Mbps as these outgoing I/O requests share hello same queue.</span></span>

## <a name="best-practices-for-bandwidth-templates"></a><span data-ttu-id="2e3fd-224">Metodtips för bandbredd mallar</span><span class="sxs-lookup"><span data-stu-id="2e3fd-224">Best practices for bandwidth templates</span></span>

<span data-ttu-id="2e3fd-225">Följ dessa rekommenderade säkerhetsmetoderna för din StorSimple-enhet:</span><span class="sxs-lookup"><span data-stu-id="2e3fd-225">Follow these best practices for your StorSimple device:</span></span>

* <span data-ttu-id="2e3fd-226">Konfigurera bandbredd mallar på din enhet tooenable variabeln begränsning av hello dataflödet i nätverket av hello enheten vid olika tidpunkter på dagen hello.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-226">Configure bandwidth templates on your device tooenable variable throttling of hello network throughput by hello device at different times of hello day.</span></span> <span data-ttu-id="2e3fd-227">Mallarna bandbredd när det används med scheman för säkerhetskopiering kan effektivt utnyttja nätverkets bandbredd för molnet åtgärder vid låg belastning.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-227">These bandwidth templates when used with backup schedules can effectively leverage additional network bandwidth for cloud operations during off-peak hours.</span></span>
* <span data-ttu-id="2e3fd-228">Beräkna hello faktiska bandbredd som krävs för en viss distribution som baseras på hello storleken på hello distribution och hello krävs recovery tid mål för Återställningstid.</span><span class="sxs-lookup"><span data-stu-id="2e3fd-228">Calculate hello actual bandwidth required for a particular deployment based on hello size of hello deployment and hello required recovery time objective (RTO).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e3fd-229">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2e3fd-229">Next steps</span></span>

<span data-ttu-id="2e3fd-230">Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="2e3fd-230">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

