---
title: "Hantera åtkomst till Azure fakturering med roller | Microsoft Docs"
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
ms.openlocfilehash: c70904097f139bc2178feed83f1cf1274f3c738d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-access-to-billing-information-for-azure-using-role-based-access-control"></a><span data-ttu-id="45fc4-102">Hantera åtkomst till faktureringsinformation för Azure med hjälp av rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="45fc4-102">Manage access to billing information for Azure using role-based access control</span></span>

<span data-ttu-id="45fc4-103">Du kan ge åtkomst för Azure faktureringsinformation för medlemmar i gruppen genom att tilldela någon av följande användarroller till din prenumeration: kontoadministratör, administratör, medadministratör, ägare, deltagare, läsare och fakturering läsare.</span><span class="sxs-lookup"><span data-stu-id="45fc4-103">You can grant access for Azure billing information to members of your team by assigning one of the following user roles to your subscription: Account Administrator, Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader.</span></span> <span data-ttu-id="45fc4-104">De skulle ha tillgång till faktureringsinformationen i den [Azure-portalen](https://portal.azure.com/), och de kan använda den [fakturerings-API: er](billing-usage-rate-card-overview.md) få programmässigt fakturor (en gång deltar i) och användningsinformation.</span><span class="sxs-lookup"><span data-stu-id="45fc4-104">They would have access to billing information in the [Azure portal](https://portal.azure.com/), and they can use the [Billing APIs](billing-usage-rate-card-overview.md) to programmatically get invoices (once opted-in) and usage details.</span></span> <span data-ttu-id="45fc4-105">Mer information om vem som kan ge roller, och vilka roller kan göra vad, se [roller i Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="45fc4-105">For more information about who can grant roles, and which roles can do what, see [Roles in Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span></span>

## <span data-ttu-id="45fc4-106"><a name="opt-in"></a>Ytterligare användare att komma åt fakturor</span><span class="sxs-lookup"><span data-stu-id="45fc4-106"><a name="opt-in"></a> Allowing additional users to access invoices</span></span>

<span data-ttu-id="45fc4-107">Kontoadministratören måste välja med hjälp av den [Azure-portalen](https://portal.azure.com/) Tillåt åtkomst till fakturor för andra användare och via API.</span><span class="sxs-lookup"><span data-stu-id="45fc4-107">The Account Administrator must opt in using the [Azure portal](https://portal.azure.com/) allow access to invoices for other users and via API.</span></span>

1. <span data-ttu-id="45fc4-108">Som tjänstadministratör, väljer din prenumeration från den [prenumerationer bladet](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="45fc4-108">As the Account Administrator, select your subscription from the [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="45fc4-109">Välj **fakturor** och sedan **åtkomst till fakturor**.</span><span class="sxs-lookup"><span data-stu-id="45fc4-109">Select **Invoices** and then **Access to invoices**.</span></span>

    ![Skärmbild som visar hur du delegerar åtkomst till fakturor](./media/billing-manage-access/AA-optin.png)

1. <span data-ttu-id="45fc4-111">Aktivera **på** åtkomst följt av sparar ändringarna, så att användarna i prenumerationen omfång roller för att ladda ned faktura.</span><span class="sxs-lookup"><span data-stu-id="45fc4-111">Turn **On** the access followed by saving the changes, to allow users in subscription scoped roles to download invoice.</span></span>

    ![Skärmbild som visar på av till delegera åtkomst till faktura](./media/billing-manage-access/AA-optinAllow.png)

<span data-ttu-id="45fc4-113">Börjar tillåter administratör, medadministratör, ägare, deltagare, läsare och fakturering läsare i prenumeration för att hämta PDF fakturor i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="45fc4-113">Opting in allows Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader on the subscription to download PDF invoices in the Azure portal.</span></span> <span data-ttu-id="45fc4-114">Fakturor som är äldre än December 2016 finns dock att kontoadministratör för tillfället.</span><span class="sxs-lookup"><span data-stu-id="45fc4-114">However, invoices older than December 2016 are available only to the Account Administrator for now.</span></span>

<span data-ttu-id="45fc4-115">Kontoadministratören kan också konfigurera om du vill att fakturor skickas via e-post.</span><span class="sxs-lookup"><span data-stu-id="45fc4-115">The Account Administrator can also configure to have invoices sent via email.</span></span> <span data-ttu-id="45fc4-116">Läs mer i [hämta fakturan i e-post](billing-download-azure-invoice-daily-usage-date.md).</span><span class="sxs-lookup"><span data-stu-id="45fc4-116">To learn more, see [Get your invoice in email](billing-download-azure-invoice-daily-usage-date.md).</span></span>

## <a name="adding-users-to-the-billing-reader-role"></a><span data-ttu-id="45fc4-117">Lägga till användare i rollen Läsare fakturering</span><span class="sxs-lookup"><span data-stu-id="45fc4-117">Adding users to the Billing Reader role</span></span>

<span data-ttu-id="45fc4-118">Rollen fakturering läsare har skrivskyddad åtkomst till faktureringsinformation för prenumerationen i Azure-portalen och ingen åtkomst till tjänster som virtuella datorer och storage-konton.</span><span class="sxs-lookup"><span data-stu-id="45fc4-118">The Billing Reader role has read-only access to subscription billing information in Azure portal, and no access to services such as VMs and storage accounts.</span></span> <span data-ttu-id="45fc4-119">Tilldela rollen fakturering läsare någon som behöver åtkomst till faktureringsinformation för prenumerationen men inte möjligheten att hantera Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="45fc4-119">Assign the Billing Reader role to someone that needs access to the subscription billing information but not the ability to manage Azure services.</span></span> <span data-ttu-id="45fc4-120">Den här rollen är lämplig för användare i en organisation som endast utför ekonomi- och hantering för Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="45fc4-120">This role is appropriate for users in an organization who only perform financial and cost management for Azure subscriptions.</span></span>

1. <span data-ttu-id="45fc4-121">Välj din prenumeration från den [prenumerationer bladet](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="45fc4-121">Select your subscription from the [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="45fc4-122">Välj **åtkomstkontroll (IAM)** och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="45fc4-122">Select **Access control (IAM)** and then click **Add**.</span></span>

    ![Skärmbild som visar IAM i prenumerationsbladet](./media/billing-manage-access/select-iam.PNG)

1. <span data-ttu-id="45fc4-124">Välj **fakturering Reader** i den **Välj en roll** sidan.</span><span class="sxs-lookup"><span data-stu-id="45fc4-124">Choose **Billing Reader** in the **Select a role** page.</span></span>

    ![Skärmbild som visar fakturering läsare i popup-vy](./media/billing-manage-access/select-roles.PNG)

1. <span data-ttu-id="45fc4-126">Skriv e-post för den användare du vill bjuda in och klicka sedan på **OK** att skicka inbjudan.</span><span class="sxs-lookup"><span data-stu-id="45fc4-126">Type the email for the user you want to invite, then click **OK** to send the invitation.</span></span>

    ![Skärmbild som visar för att ange e-postadress för att bjuda in någon](./media/billing-manage-access/add-user.PNG)

1. <span data-ttu-id="45fc4-128">Följ anvisningarna i e-postmeddelandet inbjudan att logga in som en läsare för fakturering.</span><span class="sxs-lookup"><span data-stu-id="45fc4-128">Follow instructions in the invite email to log in as a Billing Reader.</span></span>

    ![Skärmbild som visar vad läsaren fakturering kan se i Azure-portalen](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> <span data-ttu-id="45fc4-130">Fakturering Reader-funktionen är i förhandsvisning och ännu stöder inte företagsprenumerationer (EA) eller icke-globala moln.</span><span class="sxs-lookup"><span data-stu-id="45fc4-130">The Billing Reader feature is in preview, and does not yet support enterprise (EA) subscriptions or non-global clouds.</span></span>

## <a name="adding-users-to-other-roles"></a><span data-ttu-id="45fc4-131">Lägga till användare i andra roller</span><span class="sxs-lookup"><span data-stu-id="45fc4-131">Adding users to other roles</span></span>

<span data-ttu-id="45fc4-132">Användare i andra roller, till exempel ägare eller deltagare, kan komma åt inte bara faktureringsinformation, men Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="45fc4-132">Users in other roles, such as Owner or Contributor, can access not just billing information, but Azure services as well.</span></span> <span data-ttu-id="45fc4-133">Om du vill hantera dessa roller finns [lägga till eller ändra Azure-administratörsroller som hanterar prenumerationen eller tjänster](billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="45fc4-133">To manage these roles, see [Add or change Azure administrator roles that manage the subscription or services](billing-add-change-azure-subscription-administrator.md).</span></span>

## <a name="who-can-access-the-account-centerhttpsaccountwindowsazurecom"></a><span data-ttu-id="45fc4-134">Vem som kan komma åt den [Kontocenter](https://account.windowsazure.com)?</span><span class="sxs-lookup"><span data-stu-id="45fc4-134">Who can access the [Account Center](https://account.windowsazure.com)?</span></span>

<span data-ttu-id="45fc4-135">Enbart administratörskontot kan logga in till mitt konto.</span><span class="sxs-lookup"><span data-stu-id="45fc4-135">Only the Account Administrator can log in to the Account center.</span></span> <span data-ttu-id="45fc4-136">Kontoadministratören är juridiska ägaren av prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="45fc4-136">The Account Administrator is the legal owner of the subscription.</span></span> <span data-ttu-id="45fc4-137">Den person som registrerade sig för eller har köpt Azure-prenumerationen är som standard kontoadministratör, såvida inte den [prenumeration ägarskap överfördes](billing-subscription-transfer.md) till någon annan.</span><span class="sxs-lookup"><span data-stu-id="45fc4-137">By default, the person who signed up for or bought the Azure subscription is the Account Administrator, unless the [subscription ownership was transferred](billing-subscription-transfer.md) to somebody else.</span></span> <span data-ttu-id="45fc4-138">Kontoadministratören kan skapa prenumerationer, avbryta prenumerationer, ändra faktureringsadress för en prenumeration och hantera åtkomstregler för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="45fc4-138">The Account Administrator can create subscriptions, cancel subscriptions, change the billing address for a subscription, and manage access policies for the subscription.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="45fc4-139">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="45fc4-139">Need help?</span></span> <span data-ttu-id="45fc4-140">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="45fc4-140">Contact support.</span></span>

<span data-ttu-id="45fc4-141">Om du fortfarande har fler frågor, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="45fc4-141">If you still have further questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>
