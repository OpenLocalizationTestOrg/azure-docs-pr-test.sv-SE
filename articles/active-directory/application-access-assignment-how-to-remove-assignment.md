---
title: "Ta bort en användare åtkomst till ett program | Microsoft Docs"
description: "Förstå hur du tar bort en användare åtkomst till ett program"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 497429e7bf62f7e1d67ea429d6b858725f843688
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-remove-a-users-access-to-an-application"></a><span data-ttu-id="5b5fb-103">Ta bort en användare åtkomst till ett program</span><span class="sxs-lookup"><span data-stu-id="5b5fb-103">How to remove a user's access to an application</span></span>

<span data-ttu-id="5b5fb-104">Den här artikeln hjälper dig att förstå hur du tar bort en användare åtkomst till ett program.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-104">This article help you to understand how to remove a user's access to an application.</span></span>

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a><span data-ttu-id="5b5fb-105">Jag vill ta bort en specifik användare eller grupp tilldelning till ett program</span><span class="sxs-lookup"><span data-stu-id="5b5fb-105">I want to remove a specific user’s or group’s assignment to an application</span></span>

<span data-ttu-id="5b5fb-106">Ta bort en användare eller grupptilldelning till ett program genom att följa instruktionerna i den [ta bort en användare eller grupp från en enterprise-app i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) artikel.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-106">To remove a user or group assignment to an application, follow the steps listed in the [Remove a user or group assignment from an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) article.</span></span>

<span data-ttu-id="5b5fb-107">. ## jag vill inaktivera all åtkomst till ett program för varje användare</span><span class="sxs-lookup"><span data-stu-id="5b5fb-107">.## I want to disable all access to an application for every user</span></span>

<span data-ttu-id="5b5fb-108">Om du vill inaktivera alla användarinloggningar till ett program följer du stegen i den [inaktivera användarinloggningar för en enterprise-app i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) artikel.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-108">To disable all user sign-ins to an application, follow the steps listed in the [Disable user sign-ins for an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) article.</span></span>

## <a name="i-want-to-delete-an-application-entirely"></a><span data-ttu-id="5b5fb-109">Jag vill ta bort ett program helt</span><span class="sxs-lookup"><span data-stu-id="5b5fb-109">I want to delete an application entirely</span></span>

<span data-ttu-id="5b5fb-110">Att **ta bort ett program**, följ instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="5b5fb-110">To **delete an application**, follow the instructions below:</span></span>

1.  <span data-ttu-id="5b5fb-111">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="5b5fb-111">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="5b5fb-112">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-112">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5b5fb-113">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-113">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5b5fb-114">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-114">Click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="5b5fb-115">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-115">Click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="5b5fb-116">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="5b5fb-116">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="5b5fb-117">Välj det program som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-117">Select the application you want to delete.</span></span>

7.  <span data-ttu-id="5b5fb-118">När programmet läses in klickar du på **ta bort** ikon från översta programmet **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-118">Once the application loads, click **Delete** icon from the top application’s **Overview** blade.</span></span>

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a><span data-ttu-id="5b5fb-119">Jag vill inaktivera alla framtida användarens medgivande åtgärder för alla program</span><span class="sxs-lookup"><span data-stu-id="5b5fb-119">I want to disable all future user consent operations to any application</span></span>

<span data-ttu-id="5b5fb-120">Inaktiverar användargodkännande för hela katalogen att förhindra att slutanvändare principer för alla program.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-120">Disabling user consent for your entire directory prevent end users from consenting to any application.</span></span> <span data-ttu-id="5b5fb-121">Administratörer kan fortfarande medgivande på användarens behalves.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-121">Administrators can still consent on user’s behalves.</span></span> <span data-ttu-id="5b5fb-122">Lär dig mer om programmet medgivande och varför du kanske eller kanske inte vill göra detta, Läs [förstå användare och admin medgivande](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span><span class="sxs-lookup"><span data-stu-id="5b5fb-122">To learn more about application consent, and why you may or may not wish to do this, read [Understanding user and admin consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span></span>

<span data-ttu-id="5b5fb-123">Att **inaktivera alla framtida användarens medgivande åtgärder i hela katalogen**, följ instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="5b5fb-123">To **disable all future user consent operations in your entire directory**, follow the instructions below:</span></span>

1.  <span data-ttu-id="5b5fb-124">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="5b5fb-124">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5b5fb-125">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-125">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5b5fb-126">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-126">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5b5fb-127">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-127">Click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="5b5fb-128">Klicka på **användarinställningar**.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-128">Click **User settings**.</span></span>

6.  <span data-ttu-id="5b5fb-129">Inaktivera alla framtida användarens medgivande åtgärder genom att ange den **användare kan Tillåt appar att komma åt sina data** växla till **nr** och klicka på den **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5b5fb-129">Disable all future user consent operations by setting the **Users can allow apps to access their data** toggle to **No** and click the **Save** button.</span></span>


# <a name="next-steps"></a><span data-ttu-id="5b5fb-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b5fb-130">Next steps</span></span>
[<span data-ttu-id="5b5fb-131">Hantera åtkomst till appar</span><span class="sxs-lookup"><span data-stu-id="5b5fb-131">Managing access to apps</span></span>](active-directory-managing-access-to-apps.md)
