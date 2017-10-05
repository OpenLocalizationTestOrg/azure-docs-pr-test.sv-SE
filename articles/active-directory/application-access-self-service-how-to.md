---
title: "Konfigurera självbetjäning programmet tilldelning | Microsoft Docs"
description: "Aktivera självbetjäning programåtkomst så att användarna kan hitta sina egna program"
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
ms.openlocfilehash: 7991dc19d41c5eb8e149c3ee08069e1a162929cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-self-service-application-assignment"></a><span data-ttu-id="5b328-103">Konfigurera självbetjäning programmet tilldelning</span><span class="sxs-lookup"><span data-stu-id="5b328-103">How to configure self-service application assignment</span></span>

<span data-ttu-id="5b328-104">Innan användarna kan själva identifiera program från deras åtkomstpanelen, måste du aktivera **självbetjäning programåtkomst** för program som du vill tillåta användare att identifiera själv och begära åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="5b328-104">Before your users can self-discover applications from their access panel, you need to enable **Self-service application access** to any applications that you wish to allow users to self-discover and request access to.</span></span>

<span data-ttu-id="5b328-105">Den här funktionen är ett bra sätt att spara tid och pengar som IT-grupp och rekommenderas som en del av en distribution för moderna program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5b328-105">This feature is a great way for you to save time and money as an IT group, and is highly recommended as part of a modern applications deployment with Azure Active Directory.</span></span>

<span data-ttu-id="5b328-106">Den här funktionen kan du:</span><span class="sxs-lookup"><span data-stu-id="5b328-106">Using this feature, you can:</span></span>

-   <span data-ttu-id="5b328-107">Användarna själva identifiera program från den [programmet åtkomstpanelen](https://myapps.microsoft.com/) utan stör IT-grupp.</span><span class="sxs-lookup"><span data-stu-id="5b328-107">Let users self-discover applications from the [Application Access Panel](https://myapps.microsoft.com/) without bothering the IT group.</span></span>

-   <span data-ttu-id="5b328-108">Lägga till användare i en förkonfigurerad så att du kan se vem som har begärt åtkomst, ta bort åtkomst och hantera roller som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="5b328-108">Add those users to a pre-configured group so you can see who has requested access, remove access, and manage the roles assigned to them.</span></span>

-   <span data-ttu-id="5b328-109">Alternativt kan en business godkännare godkänna programförfrågningar så att IT-grupp inte behöver.</span><span class="sxs-lookup"><span data-stu-id="5b328-109">Optionally allow a business approver to approve application access requests so the IT group doesn’t have to.</span></span>

-   <span data-ttu-id="5b328-110">Du kan också konfigurera upp till 10 personer som kan godkänna åtkomst till det här programmet.</span><span class="sxs-lookup"><span data-stu-id="5b328-110">Optionally configure up to 10 individuals who may approve access to this application.</span></span>

-   <span data-ttu-id="5b328-111">Om du vill tillåta ett företag godkännare och ange sedan lösenorden dessa användare kan använda för att logga in på programmet, direkt från godkännaren företag [programmet åtkomstpanelen](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="5b328-111">Optionally allow a business approver to set the passwords those users can use to sign in to the application, right from the business approver’s [Application Access Panel](https://myapps.microsoft.com/).</span></span>

-   <span data-ttu-id="5b328-112">Du kan också automatiskt tilldela självbetjäning tilldelade användare till en roll för programmet direkt.</span><span class="sxs-lookup"><span data-stu-id="5b328-112">Optionally automatically assign self-service assigned users to an application role directly.</span></span>

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a><span data-ttu-id="5b328-113">Aktivera självbetjäning programåtkomst så att användarna kan hitta sina egna program</span><span class="sxs-lookup"><span data-stu-id="5b328-113">Enable self-service application access to allow users to find their own applications</span></span>

<span data-ttu-id="5b328-114">Självbetjäning programåtkomst är ett bra sätt att tillåta användarna att identifiera program, automatisk låta affärsgruppen att godkänna åtkomst till dessa program.</span><span class="sxs-lookup"><span data-stu-id="5b328-114">Self-service application access is a great way to allow users to self-discover applications, optionally allow the business group to approve access to those applications.</span></span> <span data-ttu-id="5b328-115">Du kan tillåta affärsgruppen att hantera de autentiseringsuppgifter som tilldelats till användare för höger lösenord enkel inloggning på program från deras åtkomst paneler.</span><span class="sxs-lookup"><span data-stu-id="5b328-115">You can allow the business group to manage the credentials assigned to those users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="5b328-116">Följ stegen nedan om du vill aktivera självbetjäning programmet åtkomst till ett program:</span><span class="sxs-lookup"><span data-stu-id="5b328-116">To enable self-service application access to an application, follow the steps below:</span></span>

1.  <span data-ttu-id="5b328-117">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="5b328-117">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5b328-118">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="5b328-118">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5b328-119">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="5b328-119">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5b328-120">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="5b328-120">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="5b328-121">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="5b328-121">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="5b328-122">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="5b328-122">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="5b328-123">Välj det program som du vill aktivera självbetjäning åtkomst till i listan.</span><span class="sxs-lookup"><span data-stu-id="5b328-123">Select the application you want to enable Self-service access to from the list.</span></span>

7.  <span data-ttu-id="5b328-124">När programmet läses in klickar du på **självbetjäning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="5b328-124">Once the application loads, click **Self-service** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="5b328-125">Om du vill aktivera självbetjäning programåtkomst för det här programmet, aktivera den **Tillåt användare att begära åtkomst till det här programmet?** växla till **Ja.**</span><span class="sxs-lookup"><span data-stu-id="5b328-125">To enable Self-service application access for this application, turn the **Allow users to request access to this application?** toggle to **Yes.**</span></span>

9.  <span data-ttu-id="5b328-126">Klicka sedan på selector bredvid etiketten för att välja gruppen till vilken användare som begär åtkomst till det här programmet ska läggas till, **vilken grupp ska tilldelade användare läggas?** och välja en grupp.</span><span class="sxs-lookup"><span data-stu-id="5b328-126">Next, to select the group to which users who request access to this application should be added, click the selector next to the label **To which group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="5b328-127">**Valfritt:** om du vill kräva en business godkännande innan användare tillåts åtkomst genom att ange den **kräver godkännande innan åtkomst beviljas till det här programmet?** växla till **Ja**.</span><span class="sxs-lookup"><span data-stu-id="5b328-127">**Optional:** If you wish to require a business approval before users are allowed access, set the **Require approval before granting access to this application?** toggle to **Yes**.</span></span>

11. <span data-ttu-id="5b328-128">**Valfritt: för program som använder enkel inloggning för lösenord på endast** om du vill att dessa företag godkännare att ange de lösenord som skickas till det här programmet för godkända användare måste ange den **Tillåt godkännare att ange användarens lösenord för det här programmet?** växla till **Ja**.</span><span class="sxs-lookup"><span data-stu-id="5b328-128">**Optional: For applications using password single-sign on only,** if you wish to allow those business approvers to specify the passwords that are sent to this application for approved users, set the **Allow approvers to set user’s passwords for this application?** toggle to **Yes**.</span></span>

12. <span data-ttu-id="5b328-129">**Valfritt:** om du vill ange godkännare för företag som har behörighet att godkänna åtkomst till det här programmet, klickar du på väljaren bredvid etiketten **som har tillåtelse att godkänna åtkomst till det här programmet?** att välja upp till 10 enskilda företag godkännare.</span><span class="sxs-lookup"><span data-stu-id="5b328-129">**Optional:** To specify the business approvers who are allowed to approve access to this application, click the selector next to the label **Who is allowed to approve access to this application?** to select up to 10 individual business approvers.</span></span>

   >[!NOTE]
   ><span data-ttu-id="5b328-130">Grupper stöds inte.</span><span class="sxs-lookup"><span data-stu-id="5b328-130">Groups are not supported.</span></span>
   >
   >

13. <span data-ttu-id="5b328-131">**Valfritt:** **för program som exponera roller**, om du vill tilldela en roll godkända Självbetjäningsanvändare, klickar du på väljaren bredvid den **vilken roll ska användare tilldelas i det här programmet?** att välja rollen som användarna ska tilldelas.</span><span class="sxs-lookup"><span data-stu-id="5b328-131">**Optional:** **For applications which expose roles**, if you wish to assign self-service approved users to a role, click the selector next to the **To which role should users be assigned in this application?** to select the role to which these users should be assigned.</span></span>

14. <span data-ttu-id="5b328-132">Klicka på den **spara** längst upp på bladet för att avsluta.</span><span class="sxs-lookup"><span data-stu-id="5b328-132">Click the **Save** button at the top of the blade to finish.</span></span>

<span data-ttu-id="5b328-133">När du har slutfört självbetjäning programkonfigurationen användare kan navigera till deras [programmet åtkomstpanelen](https://myapps.microsoft.com/) och klicka på den **+ Lägg till** för att hitta de appar som du har aktiverat självbetjäning åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5b328-133">Once you complete Self-service application configuration, users can navigate to their [Application Access Panel](https://myapps.microsoft.com/) and click the **+Add** button to find the apps to which you have enabled Self-service access.</span></span> <span data-ttu-id="5b328-134">Företag godkännare också se ett meddelande i sina [programmet åtkomstpanelen](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="5b328-134">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="5b328-135">Du kan aktivera ett e-postmeddelande till dem när en användare har begärt åtkomst till ett program som kräver godkännande.</span><span class="sxs-lookup"><span data-stu-id="5b328-135">You can enable an email notifying them when a user has requested access to an application that requires their approval.</span></span> 

<span data-ttu-id="5b328-136">Dessa godkännanden stöder godkännandearbetsflöden, vilket innebär att om du anger flera godkännare alla godkännare kan godkännare åtkomst till programmet.</span><span class="sxs-lookup"><span data-stu-id="5b328-136">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access to the application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b328-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b328-137">Next steps</span></span>
[<span data-ttu-id="5b328-138">Konfigurera Azure Active Directory för grupphantering via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="5b328-138">Setting up Azure Active Directory for self-service group management</span></span>](active-directory-accessmanagement-self-service-group-management.md)
