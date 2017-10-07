---
title: "aaaHow hanterar användarkonton i Azure API Management | Microsoft Docs"
description: "Lär dig hur toocreate eller bjuda in användare i Azure API Management"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 078abfa5-1e4f-4c9d-b9c7-a172bd19c1a2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 3966f4454e29621d7c615beefee352ec91b48b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-user-accounts-in-azure-api-management"></a><span data-ttu-id="fe227-103">Hur toomanage användarkonton i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="fe227-103">How toomanage user accounts in Azure API Management</span></span>
<span data-ttu-id="fe227-104">Utvecklare finns i API Management hello användare av hello API: er som du exponera med hjälp av API-hantering.</span><span class="sxs-lookup"><span data-stu-id="fe227-104">In API Management, developers are hello users of hello APIs that you expose using API Management.</span></span> <span data-ttu-id="fe227-105">Den här guiden visar toohow toocreate och inbjudan utvecklare toouse hello API: er och produkter som du gör tillgängliga toothem med din API Management-instans.</span><span class="sxs-lookup"><span data-stu-id="fe227-105">This guide shows toohow toocreate and invite developers toouse hello APIs and products that you make available toothem with your API Management instance.</span></span> <span data-ttu-id="fe227-106">Information om hur du hanterar användarkonton programmässigt finns hello [användarentiteten](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentation i hello [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) referens.</span><span class="sxs-lookup"><span data-stu-id="fe227-106">For information on managing user accounts programmatically, see hello [User entity](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentation in hello [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) reference.</span></span>

## <span data-ttu-id="fe227-107"><a name="create-developer"></a>Skapa en ny utvecklare</span><span class="sxs-lookup"><span data-stu-id="fe227-107"><a name="create-developer"> </a>Create a new developer</span></span>
<span data-ttu-id="fe227-108">toocreate nya utvecklare, klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="fe227-108">toocreate a new developer, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="fe227-109">Då kommer du toohello API Management publisher portal.</span><span class="sxs-lookup"><span data-stu-id="fe227-109">This takes you toohello API Management publisher portal.</span></span> <span data-ttu-id="fe227-110">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="fe227-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

![Utgivarportalen][api-management-management-console]

<span data-ttu-id="fe227-112">Klicka på **användare** från hello **API Management** menyn på hello vänster och klicka sedan på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="fe227-112">Click **Users** from hello **API Management** menu on hello left, and then click **add user**.</span></span>

![Skapa utvecklare][api-management-create-developer]

<span data-ttu-id="fe227-114">Ange hello **e-post**, **lösenord**, och **namn** för nya hello-utvecklare och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="fe227-114">Enter hello **Email**, **Password**, and **Name** for hello new developer and click **Save**.</span></span>

![Skapa utvecklare][api-management-add-new-user]

<span data-ttu-id="fe227-116">Nyligen skapade developer konton är som standard **Active**, och som är associerade med hello **utvecklare** grupp.</span><span class="sxs-lookup"><span data-stu-id="fe227-116">By default, newly created developer accounts are **Active**, and associated with hello **Developers** group.</span></span>

![Nya utvecklare][api-management-new-developer]

<span data-ttu-id="fe227-118">Utvecklare konton som är i ett **active** tillstånd kan vara används tooaccess alla hello-API: er som de har prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="fe227-118">Developer accounts that are in an **active** state can be used tooaccess all of hello APIs for which they have subscriptions.</span></span> <span data-ttu-id="fe227-119">tooassociate hello nyskapad utvecklare med ytterligare grupper, se [hur tooassociate grupper med utvecklare][How tooassociate groups with developers].</span><span class="sxs-lookup"><span data-stu-id="fe227-119">tooassociate hello newly created developer with additional groups, see [How tooassociate groups with developers][How tooassociate groups with developers].</span></span>

## <span data-ttu-id="fe227-120"><a name="invite-developer"></a>Bjuda in en utvecklare</span><span class="sxs-lookup"><span data-stu-id="fe227-120"><a name="invite-developer"> </a>Invite a developer</span></span>
<span data-ttu-id="fe227-121">tooinvite utvecklare, klickar du på **användare** från hello **API Management** menyn på hello vänster och klicka sedan på **bjuda in användare**.</span><span class="sxs-lookup"><span data-stu-id="fe227-121">tooinvite a developer, click **Users** from hello **API Management** menu on hello left, and then click **Invite User**.</span></span>

![Bjud in utvecklare][api-management-invite-developer]

<span data-ttu-id="fe227-123">Ange hello namn och e-postadress hello utvecklare och klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="fe227-123">Enter hello name and email address of hello developer, and click **Invite**.</span></span>

![Bjud in utvecklare][api-management-invite-developer-window]

<span data-ttu-id="fe227-125">Ett meddelande visas, men hello nyligen inbjuden developer visas inte i hello listan förrän efter att de accepterar hello inbjudan.</span><span class="sxs-lookup"><span data-stu-id="fe227-125">A confirmation message is displayed, but hello newly invited developer does not appear in hello list until after they accept hello invitation.</span></span> 

![Bjud in bekräftelse][api-management-invite-developer-confirmation]

<span data-ttu-id="fe227-127">När du uppmanas att utvecklare skickas ett e-postmeddelande toohello utvecklare.</span><span class="sxs-lookup"><span data-stu-id="fe227-127">When a developer is invited, an email is sent toohello developer.</span></span> <span data-ttu-id="fe227-128">Den här e-post skapas med en mall och anpassas.</span><span class="sxs-lookup"><span data-stu-id="fe227-128">This email is generated using a template and is customizable.</span></span> <span data-ttu-id="fe227-129">Mer information finns i [Konfigurera e-postmallar][Configure email templates].</span><span class="sxs-lookup"><span data-stu-id="fe227-129">For more information, see [Configure email templates][Configure email templates].</span></span>

<span data-ttu-id="fe227-130">Hello konto blir aktiv när hello inbjudan har accepterats.</span><span class="sxs-lookup"><span data-stu-id="fe227-130">Once hello invitation is accepted, hello account becomes active.</span></span>

## <span data-ttu-id="fe227-131"><a name="block-developer"></a> Inaktivera eller återaktivera ett utvecklarkonto</span><span class="sxs-lookup"><span data-stu-id="fe227-131"><a name="block-developer"> </a> Deactivate or reactivate a developer account</span></span>
<span data-ttu-id="fe227-132">Som standard nyskapade eller inbjudna developer konton är **Active**.</span><span class="sxs-lookup"><span data-stu-id="fe227-132">By default, newly created or invited developer accounts are **Active**.</span></span> <span data-ttu-id="fe227-133">toodeactivate ett utvecklarkonto klickar du på **Block**.</span><span class="sxs-lookup"><span data-stu-id="fe227-133">toodeactivate a developer account, click **Block**.</span></span> <span data-ttu-id="fe227-134">tooreactivate blockerade utvecklarkonto klickar du på **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="fe227-134">tooreactivate a blocked developer account, click **Activate**.</span></span> <span data-ttu-id="fe227-135">Blockerade utvecklarkonto kan inte komma åt hello developer-portalen eller anropa alla API: er.</span><span class="sxs-lookup"><span data-stu-id="fe227-135">A blocked developer account can not access hello developer portal or call any APIs.</span></span> <span data-ttu-id="fe227-136">toodelete ett användarkonto, klickar du på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="fe227-136">toodelete a user account, click **Delete**.</span></span>

![Block-utvecklare][api-management-new-developer]

## <a name="reset-a-user-password"></a><span data-ttu-id="fe227-138">Återställa en användarlösenord</span><span class="sxs-lookup"><span data-stu-id="fe227-138">Reset a user password</span></span>
<span data-ttu-id="fe227-139">tooreset hello lösenordet för ett användarkonto, klickar du på hello hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="fe227-139">tooreset hello password for a user account, click hello name of hello account.</span></span>

![Återställa lösenord][api-management-view-developer]

<span data-ttu-id="fe227-141">Klicka på **Återställ lösenord** toosend en länk toohello användaren tooreset sitt lösenord.</span><span class="sxs-lookup"><span data-stu-id="fe227-141">Click **Reset password** toosend a link toohello user tooreset their password.</span></span>

![Återställa lösenord][api-management-reset-password]

<span data-ttu-id="fe227-143">tooprogrammatically fungerar med användarkonton, se hello [användarentiteten](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentation i hello [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) referens.</span><span class="sxs-lookup"><span data-stu-id="fe227-143">tooprogrammatically work with user accounts, see hello [User entity](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentation in hello [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) reference.</span></span> <span data-ttu-id="fe227-144">tooreset ett användarens konto lösenord tooa specifikt värde, kan du använda hello [uppdatera en användare](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) igen och ange önskade hello-lösenord.</span><span class="sxs-lookup"><span data-stu-id="fe227-144">tooreset a user account password tooa specific value, you can use hello [Update a user](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) operation and specify hello desired password.</span></span>

## <a name="pending-verification"></a><span data-ttu-id="fe227-145">Väntande verifiering</span><span class="sxs-lookup"><span data-stu-id="fe227-145">Pending verification</span></span>
![Väntande verifiering][api-management-pending-verification]

## <span data-ttu-id="fe227-147"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe227-147"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="fe227-148">När ett utvecklarkonto har skapats kan du koppla den till roller och prenumerera på den tooproducts och API: er.</span><span class="sxs-lookup"><span data-stu-id="fe227-148">Once a developer account is created, you can associate it with roles and subscribe it tooproducts and APIs.</span></span> <span data-ttu-id="fe227-149">Mer information finns i [hur toocreate och Använd][How toocreate and use groups].</span><span class="sxs-lookup"><span data-stu-id="fe227-149">For more information, see [How toocreate and use groups][How toocreate and use groups].</span></span>

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
