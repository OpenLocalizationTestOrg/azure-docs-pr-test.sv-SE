---
title: aaaConfigure meddelanden och e-mallar i Azure API Management | Microsoft Docs
description: "Lär dig hur tooconfigure meddelanden och e-mallar i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: ee25f26d-4752-433b-af9c-3817db38aed5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: dc23289c25a1641992b73cb955099b3f207b6968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-notifications-and-email-templates-in-azure-api-management"></a><span data-ttu-id="25fdc-103">Hur tooconfigure meddelanden och e-mallar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="25fdc-103">How tooconfigure notifications and email templates in Azure API Management</span></span>
<span data-ttu-id="25fdc-104">API Management ger hello möjlighet tooconfigure meddelanden för specifika händelser och tooconfigure hello e-mallar som används toocommunicate med hello administratörer och utvecklare av en API Management-instans.</span><span class="sxs-lookup"><span data-stu-id="25fdc-104">API Management provides hello ability tooconfigure notifications for specific events, and tooconfigure hello email templates that are used toocommunicate with hello administrators and developers of an API Management instance.</span></span> <span data-ttu-id="25fdc-105">Det här avsnittet visar hur tooconfigure meddelanden för hello tillgängliga händelser, och ger en översikt över hur du konfigurerar hello e-mallar som används för dessa händelser.</span><span class="sxs-lookup"><span data-stu-id="25fdc-105">This topic shows how tooconfigure notifications for hello available events, and provides an overview of configuring hello email templates used for these events.</span></span>

## <span data-ttu-id="25fdc-106"><a name="publisher-notifications"></a>Konfigurera publisher meddelanden</span><span class="sxs-lookup"><span data-stu-id="25fdc-106"><a name="publisher-notifications"> </a>Configure publisher notifications</span></span>
<span data-ttu-id="25fdc-107">tooconfigure meddelanden, klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="25fdc-107">tooconfigure notifications, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="25fdc-108">Då kommer du toohello API Management publisher portal.</span><span class="sxs-lookup"><span data-stu-id="25fdc-108">This takes you toohello API Management publisher portal.</span></span>

![Utgivarportalen][api-management-management-console]

> [!NOTE] 
> <span data-ttu-id="25fdc-110">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="25fdc-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

<span data-ttu-id="25fdc-111">Klicka på **meddelanden** från hello **API Management** menyn på hello lämnas tooview hello tillgängliga meddelanden.</span><span class="sxs-lookup"><span data-stu-id="25fdc-111">Click **Notifications** from hello **API Management** menu on hello left tooview hello available notifications.</span></span>

![Publisher meddelanden][api-management-publisher-notifications]

<span data-ttu-id="25fdc-113">hello kan följande lista över händelser som konfigureras för meddelanden.</span><span class="sxs-lookup"><span data-stu-id="25fdc-113">hello following list of events can be configured for notifications.</span></span>

* <span data-ttu-id="25fdc-114">**Prenumerationsbegäranden (som kräver godkännande)** – hello angivna e-postmottagare och användarna ska få e-postaviseringar om prenumerationen förfrågningar för API-produkter som kräver godkännande.</span><span class="sxs-lookup"><span data-stu-id="25fdc-114">**Subscription requests (requiring approval)** - hello specified email recipients and users will receive email notifications about subscription requests for API products requiring approval.</span></span>
* <span data-ttu-id="25fdc-115">**Nya prenumerationer** – hello angivna e-postmottagare och användarna ska få e-postmeddelanden om nya prenumerationer för API-produkten.</span><span class="sxs-lookup"><span data-stu-id="25fdc-115">**New subscriptions** - hello specified email recipients and users will receive email notifications about new API product subscriptions.</span></span>
* <span data-ttu-id="25fdc-116">**Galleriet programförfrågningar** – hello angivna e-postmottagare och användarna får e-postmeddelanden när nya program som skickade toohello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="25fdc-116">**Application gallery requests** - hello specified email recipients and users will receive email notifications when new applications are submitted toohello application gallery.</span></span>
* <span data-ttu-id="25fdc-117">**Hemlig kopia** – hello angivna e-postmottagare och användarna ska få e-hemliga kopior av alla e-postmeddelanden skickas toodevelopers.</span><span class="sxs-lookup"><span data-stu-id="25fdc-117">**BCC** - hello specified email recipients and users will receive email blind carbon copies of all emails sent toodevelopers.</span></span>
* <span data-ttu-id="25fdc-118">**Nytt ärende eller kommentar** – hello angivna e-postmottagare och användarna ska få e-postaviseringar när ett nytt ärende eller kommentar skickas på hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="25fdc-118">**New issue or comment** - hello specified email recipients and users will receive email notifications when a new issue or comment is submitted on hello developer portal.</span></span>
* <span data-ttu-id="25fdc-119">**Avsluta konto meddelandet** – hello angivna e-postmottagare och får användarna e-postmeddelanden när ett konto har stängts.</span><span class="sxs-lookup"><span data-stu-id="25fdc-119">**Close account message** - hello specified email recipients and users will receive email notifications when an account is closed.</span></span>
* <span data-ttu-id="25fdc-120">**Närmar sig kvotgränsen för prenumerationen** – hello följande e-postmottagare och får användarna e-postaviseringar när prenumerationsanvändning hämtar Stäng toousage kvoten.</span><span class="sxs-lookup"><span data-stu-id="25fdc-120">**Approaching subscription quota limit** - hello following email recipients and users will receive email notifications when subscription usage gets close toousage quota.</span></span>

<span data-ttu-id="25fdc-121">Du kan ange e-postmottagare med hjälp av hello e-postadress textruta för varje händelse eller kan du välja användare från en lista.</span><span class="sxs-lookup"><span data-stu-id="25fdc-121">For each event, you can specify email recipients using hello email address text box or you can select users from a list.</span></span>

<span data-ttu-id="25fdc-122">toospecify hello e-postadresser toobe meddelande, ange dem i textrutan för hello e-postadress.</span><span class="sxs-lookup"><span data-stu-id="25fdc-122">toospecify hello email addresses toobe notified, enter them in hello email address text box.</span></span> <span data-ttu-id="25fdc-123">Om du har flera e-postadresser avgränsade med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="25fdc-123">If you have multiple email addresses, separate them using commas.</span></span>

![Meddelandemottagare][api-management-email-addresses]

<span data-ttu-id="25fdc-125">toospecify hello användare toobe meddelande, klicka på **Lägg till mottagare**hello kryssrutan bredvid hello användare toobe meddelande och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="25fdc-125">toospecify hello users toobe notified, click **add recipient**, check hello box beside hello users toobe notified, and click **OK**.</span></span>

> [!NOTE] 
> <span data-ttu-id="25fdc-126">Endast administratörer visas i hello.</span><span class="sxs-lookup"><span data-stu-id="25fdc-126">Only administrators are displayed in hello list.</span></span>


<span data-ttu-id="25fdc-127">När du har konfigurerat hello meddelandemottagare klickar du på **spara** tooapply hello uppdatera meddelandemottagare.</span><span class="sxs-lookup"><span data-stu-id="25fdc-127">After configuring hello notification recipients, click **Save** tooapply hello updated notification recipients.</span></span>

> [!NOTE] 
> <span data-ttu-id="25fdc-128">Om du navigerar bort från hello **Publisher meddelanden** fliken hello publisher portal varnar dig om det finns osparade ändringar.</span><span class="sxs-lookup"><span data-stu-id="25fdc-128">If you navigate away from hello **Publisher Notifications** tab hello publisher portal alerts you if there are unsaved changes.</span></span>


## <span data-ttu-id="25fdc-129"><a name="email-templates"></a>Konfigurera e-mallar</span><span class="sxs-lookup"><span data-stu-id="25fdc-129"><a name="email-templates"> </a>Configure email templates</span></span>
<span data-ttu-id="25fdc-130">API Management innehåller postmallar för e-för hello e-postmeddelanden som skickas i hello loppet av administrera och hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="25fdc-130">API Management provides email templates for hello email messages that are sent in hello course of administering and using hello service.</span></span> <span data-ttu-id="25fdc-131">följande e-postmallar hello tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="25fdc-131">hello following email templates are provided.</span></span>

* <span data-ttu-id="25fdc-132">Application gallery skicka godkända</span><span class="sxs-lookup"><span data-stu-id="25fdc-132">Application gallery submission approved</span></span>
* <span data-ttu-id="25fdc-133">Utvecklare farewell bokstav</span><span class="sxs-lookup"><span data-stu-id="25fdc-133">Developer farewell letter</span></span>
* <span data-ttu-id="25fdc-134">Kvot för utvecklare som närmar sig meddelande</span><span class="sxs-lookup"><span data-stu-id="25fdc-134">Developer quota limit approaching notification</span></span>
* <span data-ttu-id="25fdc-135">Bjud in användare</span><span class="sxs-lookup"><span data-stu-id="25fdc-135">Invite user</span></span>
* <span data-ttu-id="25fdc-136">Nya kommentaren till tooan problemet</span><span class="sxs-lookup"><span data-stu-id="25fdc-136">New comment added tooan issue</span></span>
* <span data-ttu-id="25fdc-137">Nya problemet som tagits emot</span><span class="sxs-lookup"><span data-stu-id="25fdc-137">New issue received</span></span>
* <span data-ttu-id="25fdc-138">Ny prenumeration som aktiverats</span><span class="sxs-lookup"><span data-stu-id="25fdc-138">New subscription activated</span></span>
* <span data-ttu-id="25fdc-139">Prenumerationen förnyas bekräftelse</span><span class="sxs-lookup"><span data-stu-id="25fdc-139">Subscription renewed confirmation</span></span>
* <span data-ttu-id="25fdc-140">Prenumerationsbegäran avböjer</span><span class="sxs-lookup"><span data-stu-id="25fdc-140">Subscription request declines</span></span>
* <span data-ttu-id="25fdc-141">Prenumerationsbegäran mottagen</span><span class="sxs-lookup"><span data-stu-id="25fdc-141">Subscription request received</span></span>

<span data-ttu-id="25fdc-142">Dessa mallar kan ändras enligt önskemål.</span><span class="sxs-lookup"><span data-stu-id="25fdc-142">These templates can be modified as desired.</span></span>

<span data-ttu-id="25fdc-143">tooview och konfigurerar hello e-postmallar för API Management-instans, klickar du på **meddelanden** från hello **API Management** menyn på hello vänster och välj hello **mallar för e-post**  fliken.</span><span class="sxs-lookup"><span data-stu-id="25fdc-143">tooview and configure hello email templates for your API Management instance, click **Notifications** from hello **API Management** menu on hello left, and select hello **Email Templates** tab.</span></span>

![E-postmallar][api-management-email-templates]

<span data-ttu-id="25fdc-145">tooview eller ändra en mall, väljer du den hello **mallar** listrutan.</span><span class="sxs-lookup"><span data-stu-id="25fdc-145">tooview or modify a specific template, select it from hello **Templates** drop-down list.</span></span>

![Lista över mallar för e-post][api-management-email-templates-list]

<span data-ttu-id="25fdc-147">Varje e-postmall har ett ämne med oformaterad text och en brödtext definition i HTML-format.</span><span class="sxs-lookup"><span data-stu-id="25fdc-147">Each email template has a subject in plain text, and a body definition in HTML format.</span></span> <span data-ttu-id="25fdc-148">Varje objekt kan anpassas enligt önskemål.</span><span class="sxs-lookup"><span data-stu-id="25fdc-148">Each item can be customized as desired.</span></span>

![Redigeraren för mallen för e-post][api-management-email-template]

<span data-ttu-id="25fdc-150">Hej **parametrar** listan innehåller en lista av parametrar, som under infogas i hello ämne eller brödtext blir ersatta hello avsett värde när hello e-postmeddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="25fdc-150">hello **Parameters** list contains a list of parameters, which when inserted into hello subject or body, will be replaced hello designated value when hello email is sent.</span></span> <span data-ttu-id="25fdc-151">tooinsert parameter, placerar hello markören där du vill hello parametern toogo, och klicka på hello pilen toohello vänster i hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="25fdc-151">tooinsert a parameter, place hello cursor where you wish hello parameter toogo, and click hello arrow toohello left of hello parameter name.</span></span>

<span data-ttu-id="25fdc-152">Klicka på **Preview** eller **skicka ett test** toosee hur hello e-post ska se ut eller skicka ett test-e-post.</span><span class="sxs-lookup"><span data-stu-id="25fdc-152">Click **Preview** or **Send a test** toosee how hello email will look or send a test email.</span></span>

> [!NOTE] 
> <span data-ttu-id="25fdc-153">hello-parametrar är inte ersättas med verkliga värden när Förhandsgranska eller skickar ett test.</span><span class="sxs-lookup"><span data-stu-id="25fdc-153">hello parameters are not replaced with actual values when previewing or sending a test.</span></span>

<span data-ttu-id="25fdc-154">toosave hello ändringar toohello e-postmall klickar du på **spara**, eller klicka på toocancel hello ändringar **Avbryt**.</span><span class="sxs-lookup"><span data-stu-id="25fdc-154">toosave hello changes toohello email template, click **Save**, or toocancel hello changes click **Cancel**.</span></span>
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
