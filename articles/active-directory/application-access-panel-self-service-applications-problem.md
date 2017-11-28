---
title: "aaaProblem med självbetjäning programåtkomst | Microsoft Docs"
description: "Felsöka problem relaterade tooself-tjänstprogrammet åtkomst"
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
ms.reviewer: japere
ms.openlocfilehash: 2487be1df191a4e7fd0bcc0ebbe4ea62fae0fd5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-using-self-service-application-access"></a><span data-ttu-id="c9818-103">Problemet med hjälp av självbetjäning programåtkomst</span><span class="sxs-lookup"><span data-stu-id="c9818-103">Problem using self-service application access</span></span>

<span data-ttu-id="c9818-104">Självbetjäning programåtkomst är ett bra sätt tooallow användare tooself-identifiera program, eventuellt tillåter hello business tooapprove toothose program.</span><span class="sxs-lookup"><span data-stu-id="c9818-104">Self-service application access is a great way tooallow users tooself-discover applications, optionally allow hello business group tooapprove access toothose applications.</span></span> <span data-ttu-id="c9818-105">Du kan tillåta hello business grupp toomanage hello autentiseringsuppgifter tilldelade toothose användare för lösenord enkel inloggning på program direkt från deras åtkomst paneler.</span><span class="sxs-lookup"><span data-stu-id="c9818-105">You can allow hello business group toomanage hello credentials assigned toothose users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="c9818-106">Innan användarna kan själva identifiera program från deras åtkomstpanelen, behöver du tooenable **självbetjäning programåtkomst** tooany program som du vill tooallow användare tooself-identifiera och begära åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="c9818-106">Before your users can self-discover applications from their access panel, you need tooenable **Self-service application access** tooany applications that you wish tooallow users tooself-discover and request access to.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="c9818-107">Allmänna problem toocheck först</span><span class="sxs-lookup"><span data-stu-id="c9818-107">General issues toocheck first</span></span>

-   <span data-ttu-id="c9818-108">Kontrollera att självbetjäning programåtkomst är korrekt konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="c9818-108">Make sure self-service application access is configured correctly.</span></span> <span data-ttu-id="c9818-109">Se ”hur tooconfigure självbetjäning program åt”.</span><span class="sxs-lookup"><span data-stu-id="c9818-109">See “How tooconfigure self-service application access”.</span></span>

-   <span data-ttu-id="c9818-110">Kontrollera att hello användare eller grupp har aktiverat toorequest självbetjäning programåtkomst.</span><span class="sxs-lookup"><span data-stu-id="c9818-110">Make sure hello user or group has been enabled toorequest self-service application access.</span></span>

-   <span data-ttu-id="c9818-111">Kontrollera att hello användare besöker hello rätt plats för självbetjäning programåtkomst.</span><span class="sxs-lookup"><span data-stu-id="c9818-111">Make sure hello user is visiting hello correct place for self-service application access.</span></span> <span data-ttu-id="c9818-112">användare kan navigera tootheir [programmet åtkomstpanelen](https://myapps.microsoft.com/) och klicka på hello **+ Lägg till** knappen toofind hello appar toowhich du har aktiverat självbetjäning åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c9818-112">users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled self-service access.</span></span>

-   <span data-ttu-id="c9818-113">Om självbetjäning programåtkomst konfigurerades har nyligen, försök toosign in och ut i hello användarens åtkomstpanelen efter några minuter toosee om hello självbetjäning ändringar har visats.</span><span class="sxs-lookup"><span data-stu-id="c9818-113">If self-service application access was just recently configured, try toosign in and out again into hello user’s Access Panel after a few minutes toosee if hello self-service access changes have appeared.</span></span>

## <a name="how-tooconfigure-self-service-application-access"></a><span data-ttu-id="c9818-114">Hur tooconfigure självbetjäning program åt</span><span class="sxs-lookup"><span data-stu-id="c9818-114">How tooconfigure self-service application access</span></span>

<span data-ttu-id="c9818-115">tooenable självbetjäning åtkomst tooan program, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="c9818-115">tooenable self-service application access tooan application, follow hello steps below:</span></span>

1.  <span data-ttu-id="c9818-116">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="c9818-116">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="c9818-117">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c9818-117">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c9818-118">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="c9818-118">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c9818-119">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c9818-119">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="c9818-120">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="c9818-120">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="c9818-121">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="c9818-121">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="c9818-122">Välj hello-program som du vill tooenable självbetjäning toofrom hello åtkomstlista.</span><span class="sxs-lookup"><span data-stu-id="c9818-122">Select hello application you want tooenable Self-service access toofrom hello list.</span></span>

7.  <span data-ttu-id="c9818-123">När programmet hello läses in klickar du på **självbetjäning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c9818-123">Once hello application loads, click **Self-service** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="c9818-124">tooenable självbetjäning programåtkomst för det här programmet stänga hello **toorequest åtkomst toothis program för användarna?** växla för**Ja.**</span><span class="sxs-lookup"><span data-stu-id="c9818-124">tooenable Self-service application access for this application, turn hello **Allow users toorequest access toothis application?** toggle too**Yes.**</span></span>

9.  <span data-ttu-id="c9818-125">Klicka sedan tooselect hello grupp toowhich användare som begär åtkomst toothis program bör läggas på hello selector nästa toohello etikett **toowhich grupp ska tilldelas användare läggas?** och välja en grupp.</span><span class="sxs-lookup"><span data-stu-id="c9818-125">Next, tooselect hello group toowhich users who request access toothis application should be added, click hello selector next toohello label **toowhich group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="c9818-126">**Valfritt:** om du vill toorequire ett företag godkännande innan användarna får åtkomst måste du ange hello **kräver godkännande innan du beviljar åtkomst toothis program?** växla för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="c9818-126">**Optional:** If you wish toorequire a business approval before users are allowed access, set hello **Require approval before granting access toothis application?** toggle too**Yes**.</span></span>

11. <span data-ttu-id="c9818-127">**Valfritt: för program som använder enkel inloggning för lösenord på endast** om du vill tooallow dessa företag godkännare toospecify hello lösenord som skickas toothis program för godkända användare måste ange hello **Tillåt godkännare tooset användarens lösenord för det här programmet?**  växla för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="c9818-127">**Optional: For applications using password single-sign on only,** if you wish tooallow those business approvers toospecify hello passwords that are sent toothis application for approved users, set hello **Allow approvers tooset user’s passwords for this application?** toggle too**Yes**.</span></span>

12. <span data-ttu-id="c9818-128">**Valfritt:** toospecify hello business godkännare som tooapprove åtkomst toothis program tillåts på hello selector nästa toohello etikett **vem som får tooapprove åtkomst toothis program?** tooselect in too10 enskilda företag godkännare.</span><span class="sxs-lookup"><span data-stu-id="c9818-128">**Optional:** toospecify hello business approvers who are allowed tooapprove access toothis application, click hello selector next toohello label **Who is allowed tooapprove access toothis application?** tooselect up too10 individual business approvers.</span></span>

 >[!NOTE]
 > <span data-ttu-id="c9818-129">Grupper stöds inte.</span><span class="sxs-lookup"><span data-stu-id="c9818-129">Groups are not supported.</span></span>
 >
 >

13. <span data-ttu-id="c9818-130">**Valfritt:** **för program som exponera roller**, om du inte vill tooassign godkända Självbetjäningsanvändare tooa roll, klicka på nästa hello selector-toohello **toowhich roll ska tilldelas användare i den här programmet?**  tooselect hello rollen toowhich dessa användare ska tilldelas.</span><span class="sxs-lookup"><span data-stu-id="c9818-130">**Optional:** **For applications which expose roles**, if you wish tooassign self-service approved users tooa role, click hello selector next toohello **toowhich role should users be assigned in this application?** tooselect hello role toowhich these users should be assigned.</span></span>

14. <span data-ttu-id="c9818-131">Klicka på hello **spara** knappen hello överst i hello bladet toofinish.</span><span class="sxs-lookup"><span data-stu-id="c9818-131">Click hello **Save** button at hello top of hello blade toofinish.</span></span>

<span data-ttu-id="c9818-132">När du har slutfört självbetjäning programkonfigurationen användare kan navigera tootheir [programmet åtkomstpanelen](https://myapps.microsoft.com/) och klicka på hello **+ Lägg till** knappen toofind hello appar toowhich du har aktiverat Självbetjäning åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c9818-132">Once you complete Self-service application configuration, users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled Self-service access.</span></span> <span data-ttu-id="c9818-133">Företag godkännare också se ett meddelande i sina [programmet åtkomstpanelen](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="c9818-133">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="c9818-134">Du kan aktivera ett e-postmeddelande till dem när en användare har begärt åtkomst tooan program som kräver godkännande.</span><span class="sxs-lookup"><span data-stu-id="c9818-134">You can enable an email notifying them when a user has requested access tooan application that requires their approval.</span></span> 

<span data-ttu-id="c9818-135">Dessa godkännanden stöd för godkännande endast arbetsflöden, vilket innebär att alla godkännare kan godkänna åtkomst toohello program om du anger flera godkännare.</span><span class="sxs-lookup"><span data-stu-id="c9818-135">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approve access toohello application.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a><span data-ttu-id="c9818-136">Om felsökningen inte löser problemet hello</span><span class="sxs-lookup"><span data-stu-id="c9818-136">If these troubleshooting steps do not resolve hello issue</span></span> 

<span data-ttu-id="c9818-137">Öppna ett supportärende med hello efter information om tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="c9818-137">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="c9818-138">Fel-ID för korrelation</span><span class="sxs-lookup"><span data-stu-id="c9818-138">Correlation error ID</span></span>

-   <span data-ttu-id="c9818-139">UPN (användarens e-postadress)</span><span class="sxs-lookup"><span data-stu-id="c9818-139">UPN (user email address)</span></span>

-   <span data-ttu-id="c9818-140">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="c9818-140">TenantID</span></span>

-   <span data-ttu-id="c9818-141">Typ av webbläsare</span><span class="sxs-lookup"><span data-stu-id="c9818-141">Browser type</span></span>

-   <span data-ttu-id="c9818-142">Tidszon och tid/tidsperioden under fel inträffar</span><span class="sxs-lookup"><span data-stu-id="c9818-142">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="c9818-143">Fiddler spårningar</span><span class="sxs-lookup"><span data-stu-id="c9818-143">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9818-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c9818-144">Next steps</span></span>
[<span data-ttu-id="c9818-145">Konfigurera Azure Active Directory för grupphantering via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="c9818-145">Setting up Azure Active Directory for self-service group management</span></span>](active-directory-accessmanagement-self-service-group-management.md)
