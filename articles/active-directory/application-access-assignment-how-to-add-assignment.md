---
title: "aaaHow tooassign användare och grupper tooan program | Microsoft Docs"
description: "Tilldela användare toohello toogrant programåtkomst"
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
ms.openlocfilehash: e039a26e4b8f88ad747354859f1071b8f74b6789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooassign-users-and-groups-tooan-application"></a><span data-ttu-id="11106-103">Hur tooassign användare och grupper tooan program</span><span class="sxs-lookup"><span data-stu-id="11106-103">How tooassign users and groups tooan application</span></span>

<span data-ttu-id="11106-104">Innan användarna kan göra något av hello nedan för ett visst program, behöver du toofirst **tilldela dem toohello program** toogrant dem åtkomst till:</span><span class="sxs-lookup"><span data-stu-id="11106-104">Before your users can do any of hello below for a specific application, you need toofirst **assign them toohello application** toogrant them access:</span></span>

-   <span data-ttu-id="11106-105">Åtkomst till ett program genom **navigera toohello programmets URL direkt** (även kallat SP-initierad inloggning).</span><span class="sxs-lookup"><span data-stu-id="11106-105">Access an application by **navigating toohello application’s URL directly** (also known as SP-initiated sign-on).</span></span>

-   <span data-ttu-id="11106-106">Åtkomst till ett program med hjälp av hello **användaren åtkomst-URL** på ett program **egenskaper** sida (även kallat IDP-initierad inloggning).</span><span class="sxs-lookup"><span data-stu-id="11106-106">Access an application by using hello **User Access URL** on an application’s **Properties** page (also known as IDP-initiated sign on).</span></span>

-   <span data-ttu-id="11106-107">Se ett program som visas på sina [programmet åtkomstpanelen](https://myapps.microsoft.com/) eller mobila program.</span><span class="sxs-lookup"><span data-stu-id="11106-107">See an application appear on their [Application Access Panel](https://myapps.microsoft.com/) or mobile application.</span></span>

-   <span data-ttu-id="11106-108">Se ett program som visas på sina [startprogrammet för Office 365](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span><span class="sxs-lookup"><span data-stu-id="11106-108">See an application appear on their [Office 365 Application Launcher](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span></span>

## <a name="methods-tooassign-applications-with-azure-active-directory"></a><span data-ttu-id="11106-109">Metoder tooassign program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="11106-109">Methods tooassign applications with Azure Active Directory</span></span> 

<span data-ttu-id="11106-110">Det finns 3 sätt som du kan tilldela program med Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="11106-110">There are 3 ways you can assign applications with Azure Active Directory:</span></span>

-   [<span data-ttu-id="11106-111">Tilldela en användare direkt tooan programmet som administratör</span><span class="sxs-lookup"><span data-stu-id="11106-111">Assign a user directly tooan application as an administrator</span></span>](#assign-a-user-directly-as-an-administrator)

-   [<span data-ttu-id="11106-112">Tilldela en grupp direkt tooan programmet som administratör</span><span class="sxs-lookup"><span data-stu-id="11106-112">Assign a group directly tooan application as an administrator</span></span>](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [<span data-ttu-id="11106-113">Aktivera självbetjäning programmet åtkomst tooallow användare toofind sina egna program</span><span class="sxs-lookup"><span data-stu-id="11106-113">Enable self-service application access tooallow users toofind their own applications</span></span>](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## <a name="assign-a-user-directly-as-an-administrator"></a><span data-ttu-id="11106-114">Tilldela en användare direkt som en administratör</span><span class="sxs-lookup"><span data-stu-id="11106-114">Assign a user directly as an administrator</span></span>

<span data-ttu-id="11106-115">tooassign en eller flera användare tooan programmet direkt, gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="11106-115">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="11106-116">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="11106-116">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="11106-117">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="11106-117">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="11106-118">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="11106-118">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="11106-119">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="11106-119">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="11106-120">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="11106-120">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="11106-121">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="11106-121">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="11106-122">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="11106-122">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="11106-123">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="11106-123">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="11106-124">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="11106-124">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="11106-125">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="11106-125">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="11106-126">Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="11106-126">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="11106-127">Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="11106-127">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="11106-128">Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="11106-128">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="11106-129">**Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="11106-129">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="11106-130">När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="11106-130">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="11106-131">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="11106-131">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="11106-132">Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.</span><span class="sxs-lookup"><span data-stu-id="11106-132">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="11106-133">Efter en kort tidsperiod att hello användare som du har valt kan toolaunch dessa program med hjälp av hello metoder som beskrivs under hello lösning beskrivning.</span><span class="sxs-lookup"><span data-stu-id="11106-133">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="assign-a-group-directly-tooan-application-as-an-administrator"></a><span data-ttu-id="11106-134">Tilldela en grupp direkt tooan programmet som administratör</span><span class="sxs-lookup"><span data-stu-id="11106-134">Assign a group directly tooan application as an administrator</span></span>

<span data-ttu-id="11106-135">tooassign en eller flera grupper tooan programmet direkt, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="11106-135">tooassign one or more groups tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="11106-136">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="11106-136">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="11106-137">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="11106-137">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="11106-138">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="11106-138">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="11106-139">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="11106-139">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="11106-140">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="11106-140">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="11106-141">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="11106-141">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="11106-142">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="11106-142">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="11106-143">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="11106-143">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="11106-144">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="11106-144">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="11106-145">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="11106-145">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="11106-146">Typen i hello **fullständig gruppnamn** av hello-grupp som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="11106-146">Type in hello **full group name** of hello group you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="11106-147">Hovra över hello **grupp** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="11106-147">Hover over hello **group** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="11106-148">Klicka på hello kryssrutan nästa toohello gruppens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="11106-148">Click hello checkbox next toohello group’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="11106-149">**Valfritt:** om du vill ha för**lägga till fler än en grupp**, typ i en annan **fullständig gruppnamn** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här gruppen toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="11106-149">**Optional:** If you would like too**add more than one group**, type in another **full group name** into hello **Search by name or email address** search box, and click hello checkbox tooadd this group toohello **Selected** list.</span></span>

13. <span data-ttu-id="11106-150">När du har valt grupper klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="11106-150">When you are finished selecting groups, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="11106-151">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello grupper som du har valt.</span><span class="sxs-lookup"><span data-stu-id="11106-151">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello groups you have selected.</span></span>

15. <span data-ttu-id="11106-152">Klicka på hello **tilldela** knappen tooassign hello programmet toohello valda grupper.</span><span class="sxs-lookup"><span data-stu-id="11106-152">Click hello **Assign** button tooassign hello application toohello selected groups.</span></span>

<span data-ttu-id="11106-153">Efter en kort tidsperiod att hello användare inom hello grupper som du har valt kan toolaunch dessa program med hjälp av hello metoder som beskrivs under hello lösning beskrivning.</span><span class="sxs-lookup"><span data-stu-id="11106-153">After a short period of time, hello users within hello groups you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span> <span data-ttu-id="11106-154">Om dessa är dynamiska grupper, kan det finnas vissa ytterligare bearbetning fördröjning i dessa tilldelningar visas för användare i de tilldelade grupper.</span><span class="sxs-lookup"><span data-stu-id="11106-154">If these are dynamic groups, there may be some additional processing delay in these assignments appearing for users within these assigned groups.</span></span>

## <a name="enable-self-service-application-access-tooallow-users-toofind-their-own-applications"></a><span data-ttu-id="11106-155">Aktivera självbetjäning programmet åtkomst tooallow användare toofind sina egna program</span><span class="sxs-lookup"><span data-stu-id="11106-155">Enable self-service application access tooallow users toofind their own applications</span></span>

<span data-ttu-id="11106-156">Självbetjäning programåtkomst är ett bra sätt tooallow användare tooself-identifiera program, eventuellt tillåter hello business tooapprove toothose program.</span><span class="sxs-lookup"><span data-stu-id="11106-156">Self-service application access is a great way tooallow users tooself-discover applications, optionally allow hello business group tooapprove access toothose applications.</span></span> <span data-ttu-id="11106-157">Du kan tillåta hello business grupp toomanage hello autentiseringsuppgifter tilldelade toothose användare för lösenord enkel inloggning på program direkt från deras åtkomst paneler.</span><span class="sxs-lookup"><span data-stu-id="11106-157">You can allow hello business group toomanage hello credentials assigned toothose users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="11106-158">tooenable självbetjäning åtkomst tooan program, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="11106-158">tooenable self-service application access tooan application, follow hello steps below:</span></span>

1.  <span data-ttu-id="11106-159">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="11106-159">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="11106-160">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="11106-160">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="11106-161">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="11106-161">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="11106-162">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="11106-162">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="11106-163">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="11106-163">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="11106-164">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="11106-164">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="11106-165">Välj hello-program som du vill tooenable självbetjäning toofrom hello åtkomstlista.</span><span class="sxs-lookup"><span data-stu-id="11106-165">Select hello application you want tooenable Self-service access toofrom hello list.</span></span>

7.  <span data-ttu-id="11106-166">När programmet hello läses in klickar du på **självbetjäning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="11106-166">Once hello application loads, click **Self-service** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="11106-167">tooenable självbetjäning programåtkomst för det här programmet stänga hello **toorequest åtkomst toothis program för användarna?** växla för**Ja.**</span><span class="sxs-lookup"><span data-stu-id="11106-167">tooenable Self-service application access for this application, turn hello **Allow users toorequest access toothis application?** toggle too**Yes.**</span></span>

9.  <span data-ttu-id="11106-168">Klicka sedan tooselect hello grupp toowhich användare som begär åtkomst toothis program bör läggas på hello selector nästa toohello etikett **toowhich grupp ska tilldelas användare läggas?** och välja en grupp.</span><span class="sxs-lookup"><span data-stu-id="11106-168">Next, tooselect hello group toowhich users who request access toothis application should be added, click hello selector next toohello label **toowhich group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="11106-169">**Valfritt:** om du vill toorequire ett företag godkännande innan användarna får åtkomst måste du ange hello **kräver godkännande innan du beviljar åtkomst toothis program?** växla för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="11106-169">**Optional:** If you wish toorequire a business approval before users are allowed access, set hello **Require approval before granting access toothis application?** toggle too**Yes**.</span></span>

11. <span data-ttu-id="11106-170">**Valfritt: för program som använder enkel inloggning för lösenord på endast** om du vill tooallow dessa företag godkännare toospecify hello lösenord som skickas toothis program för godkända användare måste ange hello **Tillåt godkännare tooset användarens lösenord för det här programmet?**  växla för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="11106-170">**Optional: For applications using password single-sign on only,** if you wish tooallow those business approvers toospecify hello passwords that are sent toothis application for approved users, set hello **Allow approvers tooset user’s passwords for this application?** toggle too**Yes**.</span></span>

12. <span data-ttu-id="11106-171">**Valfritt:** toospecify hello business godkännare som tooapprove åtkomst toothis program tillåts på hello selector nästa toohello etikett **vem som får tooapprove åtkomst toothis program?** tooselect in too10 enskilda företag godkännare.</span><span class="sxs-lookup"><span data-stu-id="11106-171">**Optional:** toospecify hello business approvers who are allowed tooapprove access toothis application, click hello selector next toohello label **Who is allowed tooapprove access toothis application?** tooselect up too10 individual business approvers.</span></span>

  >[!NOTE]
  ><span data-ttu-id="11106-172">Grupper stöds inte.</span><span class="sxs-lookup"><span data-stu-id="11106-172">Groups are not supported.</span></span>
  >
  >

13. <span data-ttu-id="11106-173">**Valfritt:** **för program som exponera roller**, om du inte vill tooassign godkända Självbetjäningsanvändare tooa roll, klicka på nästa hello selector-toohello **toowhich roll ska tilldelas användare i den här programmet?**  tooselect hello rollen toowhich dessa användare ska tilldelas.</span><span class="sxs-lookup"><span data-stu-id="11106-173">**Optional:** **For applications which expose roles**, if you wish tooassign self-service approved users tooa role, click hello selector next toohello **toowhich role should users be assigned in this application?** tooselect hello role toowhich these users should be assigned.</span></span>

14. <span data-ttu-id="11106-174">Klicka på hello **spara** knappen hello överst i hello bladet toofinish.</span><span class="sxs-lookup"><span data-stu-id="11106-174">Click hello **Save** button at hello top of hello blade toofinish.</span></span>

<span data-ttu-id="11106-175">När du har slutfört självbetjäning programkonfigurationen användare kan navigera tootheir [programmet åtkomstpanelen](https://myapps.microsoft.com/) och klicka på hello **+ Lägg till** knappen toofind hello appar toowhich du har aktiverat Självbetjäning åtkomst.</span><span class="sxs-lookup"><span data-stu-id="11106-175">Once you complete Self-service application configuration, users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled Self-service access.</span></span> <span data-ttu-id="11106-176">Företag godkännare också se ett meddelande i sina [programmet åtkomstpanelen](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="11106-176">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="11106-177">Du kan aktivera ett e-postmeddelande till dem när en användare har begärt åtkomst tooan program som kräver godkännande.</span><span class="sxs-lookup"><span data-stu-id="11106-177">You can enable an email notifying them when a user has requested access tooan application that requires their approval.</span></span> 

<span data-ttu-id="11106-178">Dessa godkännanden stöder godkännandearbetsflöden, vilket innebär att om du anger flera godkännare alla godkännare kan godkännare åtkomst toohello program.</span><span class="sxs-lookup"><span data-stu-id="11106-178">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access toohello application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="11106-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="11106-179">Next steps</span></span>
[<span data-ttu-id="11106-180">Tillhandahålla enkel inloggning tooyour appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="11106-180">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
