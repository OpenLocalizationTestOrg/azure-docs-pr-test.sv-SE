---
title: "aaaHow tooconfigure självbetjäning programmet tilldelning | Microsoft Docs"
description: "Aktivera självbetjäning programmet åtkomst tooallow användare toofind sina egna program"
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
ms.openlocfilehash: d25a0146c4c8cebf9c2ae8c516f094a8eccb4570
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-self-service-application-assignment"></a><span data-ttu-id="08f6f-103">Hur tooconfigure självbetjäning programmet tilldelning</span><span class="sxs-lookup"><span data-stu-id="08f6f-103">How tooconfigure self-service application assignment</span></span>

<span data-ttu-id="08f6f-104">Innan användarna kan själva identifiera program från deras åtkomstpanelen, behöver du tooenable **självbetjäning programåtkomst** tooany program som du vill tooallow användare tooself-identifiera och begära åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="08f6f-104">Before your users can self-discover applications from their access panel, you need tooenable **Self-service application access** tooany applications that you wish tooallow users tooself-discover and request access to.</span></span>

<span data-ttu-id="08f6f-105">Den här funktionen är ett bra sätt du toosave tid och pengar som IT-grupp och rekommenderas som en del av en distribution för moderna program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="08f6f-105">This feature is a great way for you toosave time and money as an IT group, and is highly recommended as part of a modern applications deployment with Azure Active Directory.</span></span>

<span data-ttu-id="08f6f-106">Den här funktionen kan du:</span><span class="sxs-lookup"><span data-stu-id="08f6f-106">Using this feature, you can:</span></span>

-   <span data-ttu-id="08f6f-107">Användarna själva identifiera program från hello [programmet åtkomstpanelen](https://myapps.microsoft.com/) utan stör hello IT-gruppen.</span><span class="sxs-lookup"><span data-stu-id="08f6f-107">Let users self-discover applications from hello [Application Access Panel](https://myapps.microsoft.com/) without bothering hello IT group.</span></span>

-   <span data-ttu-id="08f6f-108">Lägg till dessa tooa förkonfigurerade användargruppen så att du kan se vem som har begärt åtkomst, ta bort åtkomst och hantera hello toothem för roller.</span><span class="sxs-lookup"><span data-stu-id="08f6f-108">Add those users tooa pre-configured group so you can see who has requested access, remove access, and manage hello roles assigned toothem.</span></span>

-   <span data-ttu-id="08f6f-109">Om du vill tillåta en business godkännare tooapprove programmet åtkomstbegäranden så hello IT-grupp inte behöver.</span><span class="sxs-lookup"><span data-stu-id="08f6f-109">Optionally allow a business approver tooapprove application access requests so hello IT group doesn’t have to.</span></span>

-   <span data-ttu-id="08f6f-110">Du kan också konfigurera in too10 personer som kan godkänna åtkomst toothis program.</span><span class="sxs-lookup"><span data-stu-id="08f6f-110">Optionally configure up too10 individuals who may approve access toothis application.</span></span>

-   <span data-ttu-id="08f6f-111">Om du vill att en business godkännare tooset hello lösenord dessa användare kan använda toosign i toohello program direkt från hello business godkännare [programmet åtkomstpanelen](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="08f6f-111">Optionally allow a business approver tooset hello passwords those users can use toosign in toohello application, right from hello business approver’s [Application Access Panel](https://myapps.microsoft.com/).</span></span>

-   <span data-ttu-id="08f6f-112">Du kan också automatiskt tilldela tilldelade Självbetjäningsanvändare tooan programrollen direkt.</span><span class="sxs-lookup"><span data-stu-id="08f6f-112">Optionally automatically assign self-service assigned users tooan application role directly.</span></span>

## <a name="enable-self-service-application-access-tooallow-users-toofind-their-own-applications"></a><span data-ttu-id="08f6f-113">Aktivera självbetjäning programmet åtkomst tooallow användare toofind sina egna program</span><span class="sxs-lookup"><span data-stu-id="08f6f-113">Enable self-service application access tooallow users toofind their own applications</span></span>

<span data-ttu-id="08f6f-114">Självbetjäning programåtkomst är ett bra sätt tooallow användare tooself-identifiera program, eventuellt tillåter hello business tooapprove toothose program.</span><span class="sxs-lookup"><span data-stu-id="08f6f-114">Self-service application access is a great way tooallow users tooself-discover applications, optionally allow hello business group tooapprove access toothose applications.</span></span> <span data-ttu-id="08f6f-115">Du kan tillåta hello business grupp toomanage hello autentiseringsuppgifter tilldelade toothose användare för lösenord enkel inloggning på program direkt från deras åtkomst paneler.</span><span class="sxs-lookup"><span data-stu-id="08f6f-115">You can allow hello business group toomanage hello credentials assigned toothose users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="08f6f-116">tooenable självbetjäning åtkomst tooan program, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="08f6f-116">tooenable self-service application access tooan application, follow hello steps below:</span></span>

1.  <span data-ttu-id="08f6f-117">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="08f6f-117">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="08f6f-118">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="08f6f-118">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="08f6f-119">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="08f6f-119">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="08f6f-120">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="08f6f-120">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="08f6f-121">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="08f6f-121">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="08f6f-122">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="08f6f-122">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="08f6f-123">Välj hello-program som du vill tooenable självbetjäning toofrom hello åtkomstlista.</span><span class="sxs-lookup"><span data-stu-id="08f6f-123">Select hello application you want tooenable Self-service access toofrom hello list.</span></span>

7.  <span data-ttu-id="08f6f-124">När programmet hello läses in klickar du på **självbetjäning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="08f6f-124">Once hello application loads, click **Self-service** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="08f6f-125">tooenable självbetjäning programåtkomst för det här programmet stänga hello **toorequest åtkomst toothis program för användarna?** växla för**Ja.**</span><span class="sxs-lookup"><span data-stu-id="08f6f-125">tooenable Self-service application access for this application, turn hello **Allow users toorequest access toothis application?** toggle too**Yes.**</span></span>

9.  <span data-ttu-id="08f6f-126">Klicka sedan tooselect hello grupp toowhich användare som begär åtkomst toothis program bör läggas på hello selector nästa toohello etikett **toowhich grupp ska tilldelas användare läggas?** och välja en grupp.</span><span class="sxs-lookup"><span data-stu-id="08f6f-126">Next, tooselect hello group toowhich users who request access toothis application should be added, click hello selector next toohello label **toowhich group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="08f6f-127">**Valfritt:** om du vill toorequire ett företag godkännande innan användarna får åtkomst måste du ange hello **kräver godkännande innan du beviljar åtkomst toothis program?** växla för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="08f6f-127">**Optional:** If you wish toorequire a business approval before users are allowed access, set hello **Require approval before granting access toothis application?** toggle too**Yes**.</span></span>

11. <span data-ttu-id="08f6f-128">**Valfritt: för program som använder enkel inloggning för lösenord på endast** om du vill tooallow dessa företag godkännare toospecify hello lösenord som skickas toothis program för godkända användare måste ange hello **Tillåt godkännare tooset användarens lösenord för det här programmet?**  växla för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="08f6f-128">**Optional: For applications using password single-sign on only,** if you wish tooallow those business approvers toospecify hello passwords that are sent toothis application for approved users, set hello **Allow approvers tooset user’s passwords for this application?** toggle too**Yes**.</span></span>

12. <span data-ttu-id="08f6f-129">**Valfritt:** toospecify hello business godkännare som tooapprove åtkomst toothis program tillåts på hello selector nästa toohello etikett **vem som får tooapprove åtkomst toothis program?** tooselect in too10 enskilda företag godkännare.</span><span class="sxs-lookup"><span data-stu-id="08f6f-129">**Optional:** toospecify hello business approvers who are allowed tooapprove access toothis application, click hello selector next toohello label **Who is allowed tooapprove access toothis application?** tooselect up too10 individual business approvers.</span></span>

   >[!NOTE]
   ><span data-ttu-id="08f6f-130">Grupper stöds inte.</span><span class="sxs-lookup"><span data-stu-id="08f6f-130">Groups are not supported.</span></span>
   >
   >

13. <span data-ttu-id="08f6f-131">**Valfritt:** **för program som exponera roller**, om du inte vill tooassign godkända Självbetjäningsanvändare tooa roll, klicka på nästa hello selector-toohello **toowhich roll ska tilldelas användare i den här programmet?**  tooselect hello rollen toowhich dessa användare ska tilldelas.</span><span class="sxs-lookup"><span data-stu-id="08f6f-131">**Optional:** **For applications which expose roles**, if you wish tooassign self-service approved users tooa role, click hello selector next toohello **toowhich role should users be assigned in this application?** tooselect hello role toowhich these users should be assigned.</span></span>

14. <span data-ttu-id="08f6f-132">Klicka på hello **spara** knappen hello överst i hello bladet toofinish.</span><span class="sxs-lookup"><span data-stu-id="08f6f-132">Click hello **Save** button at hello top of hello blade toofinish.</span></span>

<span data-ttu-id="08f6f-133">När du har slutfört självbetjäning programkonfigurationen användare kan navigera tootheir [programmet åtkomstpanelen](https://myapps.microsoft.com/) och klicka på hello **+ Lägg till** knappen toofind hello appar toowhich du har aktiverat Självbetjäning åtkomst.</span><span class="sxs-lookup"><span data-stu-id="08f6f-133">Once you complete Self-service application configuration, users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled Self-service access.</span></span> <span data-ttu-id="08f6f-134">Företag godkännare också se ett meddelande i sina [programmet åtkomstpanelen](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="08f6f-134">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="08f6f-135">Du kan aktivera ett e-postmeddelande till dem när en användare har begärt åtkomst tooan program som kräver godkännande.</span><span class="sxs-lookup"><span data-stu-id="08f6f-135">You can enable an email notifying them when a user has requested access tooan application that requires their approval.</span></span> 

<span data-ttu-id="08f6f-136">Dessa godkännanden stöder godkännandearbetsflöden, vilket innebär att om du anger flera godkännare alla godkännare kan godkännare åtkomst toohello program.</span><span class="sxs-lookup"><span data-stu-id="08f6f-136">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access toohello application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="08f6f-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="08f6f-137">Next steps</span></span>
[<span data-ttu-id="08f6f-138">Konfigurera Azure Active Directory för grupphantering via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="08f6f-138">Setting up Azure Active Directory for self-service group management</span></span>](active-directory-accessmanagement-self-service-group-management.md)
