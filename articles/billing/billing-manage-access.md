---
title: "aaaManage åtkomst tooAzure fakturering med roller | Microsoft Docs"
description: 
services: 
documentationcenter: 
author: vikramdesai01
manager: vikdesai
editor: 
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: vikdesai
ms.openlocfilehash: 5937fac5ffa5ca204eb03a1dcbc5e800b3d5eb74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-access-toobilling-information-for-azure-using-role-based-access-control"></a><span data-ttu-id="1f622-102">Hantera åtkomst toobilling information för Azure med hjälp av rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="1f622-102">Manage access toobilling information for Azure using role-based access control</span></span>

<span data-ttu-id="1f622-103">Du kan ge åtkomst för Azure fakturering information toomembers gruppmedlemmarna genom att tilldela en hello efter användarens roller tooyour prenumeration: kontoadministratör, administratör, medadministratör, ägare, deltagare, läsare och fakturering läsare.</span><span class="sxs-lookup"><span data-stu-id="1f622-103">You can grant access for Azure billing information toomembers of your team by assigning one of hello following user roles tooyour subscription: Account Administrator, Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader.</span></span> <span data-ttu-id="1f622-104">De skulle ha åtkomst till toobilling information i hello [Azure-portalen](https://portal.azure.com/), och de kan använda hello [fakturering API: er](billing-usage-rate-card-overview.md) tooprogrammatically hämta fakturor (en gång deltar i) och användningsinformation.</span><span class="sxs-lookup"><span data-stu-id="1f622-104">They would have access toobilling information in hello [Azure portal](https://portal.azure.com/), and they can use hello [Billing APIs](billing-usage-rate-card-overview.md) tooprogrammatically get invoices (once opted-in) and usage details.</span></span> <span data-ttu-id="1f622-105">Mer information om vem som kan ge roller, och vilka roller kan göra vad, se [roller i Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="1f622-105">For more information about who can grant roles, and which roles can do what, see [Roles in Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span></span>

## <span data-ttu-id="1f622-106"><a name="opt-in"></a>Att tillåta ytterligare användare tooaccess fakturor</span><span class="sxs-lookup"><span data-stu-id="1f622-106"><a name="opt-in"></a> Allowing additional users tooaccess invoices</span></span>

<span data-ttu-id="1f622-107">hello kontoadministratör måste välja med hello [Azure-portalen](https://portal.azure.com/) Tillåt åtkomst tooinvoices för andra användare och via API.</span><span class="sxs-lookup"><span data-stu-id="1f622-107">hello Account Administrator must opt in using hello [Azure portal](https://portal.azure.com/) allow access tooinvoices for other users and via API.</span></span>

1. <span data-ttu-id="1f622-108">Eftersom hello kontoadministratör, väljer din prenumeration från hello [prenumerationer bladet](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1f622-108">As hello Account Administrator, select your subscription from hello [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="1f622-109">Välj **fakturor** och sedan **åtkomst till tooinvoices**.</span><span class="sxs-lookup"><span data-stu-id="1f622-109">Select **Invoices** and then **Access tooinvoices**.</span></span>

    ![Skärmbild som visar hur toodelegate åt tooinvoices](./media/billing-manage-access/AA-optin.png)

1. <span data-ttu-id="1f622-111">Aktivera **på** hello åtkomst följt av hello ändringar, tooallow användare i prenumerationen begränsade roller toodownload faktura.</span><span class="sxs-lookup"><span data-stu-id="1f622-111">Turn **On** hello access followed by saving hello changes, tooallow users in subscription scoped roles toodownload invoice.</span></span>

    ![Skärmbild som visar-off toodelegate åtkomst tooinvoice](./media/billing-manage-access/AA-optinAllow.png)

<span data-ttu-id="1f622-113">Börjar tillåter administratör, medadministratör, ägare, deltagare, läsare och fakturering Reader på hello prenumeration toodownload PDF fakturor i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1f622-113">Opting in allows Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader on hello subscription toodownload PDF invoices in hello Azure portal.</span></span> <span data-ttu-id="1f622-114">Fakturor som är äldre än December 2016 är dock tillgängligt endast toohello kontoadministratör för tillfället.</span><span class="sxs-lookup"><span data-stu-id="1f622-114">However, invoices older than December 2016 are available only toohello Account Administrator for now.</span></span>

<span data-ttu-id="1f622-115">hello kontoadministratör kan också konfigurera toohave fakturor skickas via e-post.</span><span class="sxs-lookup"><span data-stu-id="1f622-115">hello Account Administrator can also configure toohave invoices sent via email.</span></span> <span data-ttu-id="1f622-116">Det finns fler toolearn [hämta fakturan i e-post](billing-download-azure-invoice-daily-usage-date.md).</span><span class="sxs-lookup"><span data-stu-id="1f622-116">toolearn more, see [Get your invoice in email](billing-download-azure-invoice-daily-usage-date.md).</span></span>

## <a name="adding-users-toohello-billing-reader-role"></a><span data-ttu-id="1f622-117">Att lägga till rollen för användare toohello fakturering läsare</span><span class="sxs-lookup"><span data-stu-id="1f622-117">Adding users toohello Billing Reader role</span></span>

<span data-ttu-id="1f622-118">rollen för hello fakturering läsare har skrivskyddad åtkomst toosubscription faktureringsinformation i Azure-portalen och ingen åtkomst tooservices, till exempel virtuella datorer och storage-konton.</span><span class="sxs-lookup"><span data-stu-id="1f622-118">hello Billing Reader role has read-only access toosubscription billing information in Azure portal, and no access tooservices such as VMs and storage accounts.</span></span> <span data-ttu-id="1f622-119">Tilldela hello fakturering Reader rollen toosomeone som behöver komma åt toohello faktureringsinformation för prenumerationen men inte hello möjlighet toomanage Azure services.</span><span class="sxs-lookup"><span data-stu-id="1f622-119">Assign hello Billing Reader role toosomeone that needs access toohello subscription billing information but not hello ability toomanage Azure services.</span></span> <span data-ttu-id="1f622-120">Den här rollen är lämplig för användare i en organisation som endast utför ekonomi- och hantering för Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="1f622-120">This role is appropriate for users in an organization who only perform financial and cost management for Azure subscriptions.</span></span>

1. <span data-ttu-id="1f622-121">Välj din prenumeration från hello [prenumerationer bladet](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1f622-121">Select your subscription from hello [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="1f622-122">Välj **åtkomstkontroll (IAM)** och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1f622-122">Select **Access control (IAM)** and then click **Add**.</span></span>

    ![Skärmbild som visar IAM i hello prenumerationsbladet](./media/billing-manage-access/select-iam.PNG)

1. <span data-ttu-id="1f622-124">Välj **fakturering Reader** i hello **Välj en roll** sidan.</span><span class="sxs-lookup"><span data-stu-id="1f622-124">Choose **Billing Reader** in hello **Select a role** page.</span></span>

    ![Skärmbild som visar fakturering läsare i hello popup-vy](./media/billing-manage-access/select-roles.PNG)

1. <span data-ttu-id="1f622-126">Skriv hello e-post för hello användare du vill tooinvite och klickar sedan **OK** toosend hello inbjudan.</span><span class="sxs-lookup"><span data-stu-id="1f622-126">Type hello email for hello user you want tooinvite, then click **OK** toosend hello invitation.</span></span>

    ![Skärmbild som visar tooenter e-tooinvite någon](./media/billing-manage-access/add-user.PNG)

1. <span data-ttu-id="1f622-128">Följ anvisningarna i hello inbjudan e-toolog i som en läsare för fakturering.</span><span class="sxs-lookup"><span data-stu-id="1f622-128">Follow instructions in hello invite email toolog in as a Billing Reader.</span></span>

    ![Skärmbild som visar vad hello fakturering läsaren kan se i Azure-portalen](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> <span data-ttu-id="1f622-130">hello fakturering Reader funktionen är i förhandsvisning och ännu stöder inte företagsprenumerationer (EA) eller icke-globala moln.</span><span class="sxs-lookup"><span data-stu-id="1f622-130">hello Billing Reader feature is in preview, and does not yet support enterprise (EA) subscriptions or non-global clouds.</span></span>

## <a name="adding-users-tooother-roles"></a><span data-ttu-id="1f622-131">Lägga till användare tooother roller</span><span class="sxs-lookup"><span data-stu-id="1f622-131">Adding users tooother roles</span></span>

<span data-ttu-id="1f622-132">Användare i andra roller, till exempel ägare eller deltagare, kan komma åt inte bara faktureringsinformation, men Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="1f622-132">Users in other roles, such as Owner or Contributor, can access not just billing information, but Azure services as well.</span></span> <span data-ttu-id="1f622-133">dessa roller finns i toomanage [lägga till eller ändra Azure-administratörsroller som hanterar hello prenumeration eller tjänster](billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="1f622-133">toomanage these roles, see [Add or change Azure administrator roles that manage hello subscription or services](billing-add-change-azure-subscription-administrator.md).</span></span>

## <a name="who-can-access-hello-account-centerhttpsaccountwindowsazurecom"></a><span data-ttu-id="1f622-134">Vem som kan komma åt hello [Kontocenter](https://account.windowsazure.com)?</span><span class="sxs-lookup"><span data-stu-id="1f622-134">Who can access hello [Account Center](https://account.windowsazure.com)?</span></span>

<span data-ttu-id="1f622-135">Endast hello kontoadministratör kan logga in toohello kontocenter.</span><span class="sxs-lookup"><span data-stu-id="1f622-135">Only hello Account Administrator can log in toohello Account center.</span></span> <span data-ttu-id="1f622-136">hello kontoadministratör äger hello juridiska hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1f622-136">hello Account Administrator is hello legal owner of hello subscription.</span></span> <span data-ttu-id="1f622-137">Hello person som registrerade sig för eller köpt hello Azure-prenumeration är som standard hello kontoadministratör, såvida inte hello [prenumeration ägarskap överfördes](billing-subscription-transfer.md) toosomebody annan.</span><span class="sxs-lookup"><span data-stu-id="1f622-137">By default, hello person who signed up for or bought hello Azure subscription is hello Account Administrator, unless hello [subscription ownership was transferred](billing-subscription-transfer.md) toosomebody else.</span></span> <span data-ttu-id="1f622-138">hello kontoadministratör kan skapa prenumerationer, avbryta prenumerationer, ändra hello faktureringsadress för en prenumeration och hantera åtkomstregler för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1f622-138">hello Account Administrator can create subscriptions, cancel subscriptions, change hello billing address for a subscription, and manage access policies for hello subscription.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="1f622-139">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="1f622-139">Need help?</span></span> <span data-ttu-id="1f622-140">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="1f622-140">Contact support.</span></span>

<span data-ttu-id="1f622-141">Om du fortfarande har fler frågor, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.</span><span class="sxs-lookup"><span data-stu-id="1f622-141">If you still have further questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>
