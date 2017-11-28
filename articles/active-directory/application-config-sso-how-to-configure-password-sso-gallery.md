---
title: "aaaHow tooconfigure lösenord enkel inloggning för ett program för Azure AD-galleriet | Microsoft Docs"
description: "Hur tooconfigure ett program för säkra lösenordsbaserade enkel inloggning när det finns redan i hello Azure AD Application Gallery"
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
ms.openlocfilehash: 7a93bff119b477d946368686fc2d9006ca2722a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="f3aff-103">Hur tooconfigure lösenord enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="f3aff-103">How tooconfigure password single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="f3aff-104">När du lägger till ett program från hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), du har hello väljer du hur dina användare toosign i toothat program.</span><span class="sxs-lookup"><span data-stu-id="f3aff-104">When you add an application from hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), you have hello choice of how you want your users toosign in toothat application.</span></span> <span data-ttu-id="f3aff-105">Du kan konfigurera detta val när som helst genom att välja hello **enkel inloggning** navigeringsobjektet på ett affärsprogram i hello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f3aff-105">You can configure this choice at any time by selecting hello **Single Sign-on** navigation item on an Enterprise Application in hello [Azure Portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="f3aff-106">En av hello enkel inloggning metoder tillgängliga tooyou är hello [lösenordsbaserade enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) alternativet.</span><span class="sxs-lookup"><span data-stu-id="f3aff-106">One of hello single sign-on methods available tooyou is hello [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) option.</span></span> <span data-ttu-id="f3aff-107">Detta är ett bra sätt tooget igång snabbt integrera program i Azure AD, och du kan:</span><span class="sxs-lookup"><span data-stu-id="f3aff-107">This is a great way tooget started integrating applications into Azure AD quickly, and allows you to:</span></span>

-   <span data-ttu-id="f3aff-108">Aktivera **enkel inloggning för användarnas** genom att lagra och spela upp användarnamn och lösenord för hello programmet på ett säkert sätt som du har integrerat med Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3aff-108">Enable **Single Sign-on for your users** by securely storing and replaying usernames and passwords for hello application you’ve integrated with Azure AD</span></span>

-   <span data-ttu-id="f3aff-109">**Stöd för program som kräver flera inloggning fält** för program som kräver mer än bara användarnamn och lösenord fält toosign i</span><span class="sxs-lookup"><span data-stu-id="f3aff-109">**Support applications that require multiple sign-in fields** for applications that require more than just username and password fields toosign in</span></span>

-   <span data-ttu-id="f3aff-110">**Anpassa hello etiketter** av hello användarnamn och lösenord indatafält användarna ser på hello [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) när de anger sina autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="f3aff-110">**Customize hello labels** of hello username and password input fields your users see on hello [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) when they enter their credentials</span></span>

-   <span data-ttu-id="f3aff-111">Tillåt din **användare** tooprovide användarnamn och lösenord för alla program-konton som de skriver manuellt på hello [åtkomstpanelen för programmet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span><span class="sxs-lookup"><span data-stu-id="f3aff-111">Allow your **users** tooprovide their own usernames and passwords for any existing application accounts they are typing in manually on hello [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span></span>

-   <span data-ttu-id="f3aff-112">Tillåt en **tillhör hello affärsgrupp** toospecify hello användarnamn och lösenord tilldelad tooa användare med hjälp av hello [Self-Service programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) funktion</span><span class="sxs-lookup"><span data-stu-id="f3aff-112">Allow a **member of hello business group** toospecify hello usernames and passwords assigned tooa user by using hello [Self-Service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) feature</span></span>

-   <span data-ttu-id="f3aff-113">Tillåt en **administratör** toospecify hello användarnamn och lösenord som tilldelats tooa användare med hjälp av referenser för uppdatering av hello funktionen när [tilldela en tooan-användarprogram](#assign-a-user-to-an-application-directly)</span><span class="sxs-lookup"><span data-stu-id="f3aff-113">Allow an **administrator** toospecify hello usernames and passwords assigned tooa user by using hello Update Credentials feature when [assigning a user tooan application](#assign-a-user-to-an-application-directly)</span></span>

-   <span data-ttu-id="f3aff-114">Tillåt en **administratör** toospecify hello delade användarnamnet eller lösenordet som används av en grupp människor med hjälp av autentiseringsuppgifter för uppdatering av hello funktionen när [tilldela en grupp tooan program](#assign-an-application-to-a-group-directly)</span><span class="sxs-lookup"><span data-stu-id="f3aff-114">Allow an **administrator** toospecify hello shared username or password used by a group of people by using hello Update Credentials feature when [assigning a group tooan application](#assign-an-application-to-a-group-directly)</span></span>

<span data-ttu-id="f3aff-115">Nedan beskrivs hur du kan aktivera [lösenordsbaserade enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) tooan program som redan hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="f3aff-115">Below describes how you can enable [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) tooan application that is already in hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="f3aff-116">Översikt över steg som krävs</span><span class="sxs-lookup"><span data-stu-id="f3aff-116">Overview of steps required</span></span>
<span data-ttu-id="f3aff-117">tooconfigure ett program från hello Azure AD-galleriet måste du:</span><span class="sxs-lookup"><span data-stu-id="f3aff-117">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="f3aff-118">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="f3aff-118">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="f3aff-119">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3aff-119">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="f3aff-120">Tilldela hello programmet tooa användare eller grupp</span><span class="sxs-lookup"><span data-stu-id="f3aff-120">Assign hello application tooa user or a group</span></span>](#assign-the-application-to-a-user-or-a-group)

    -   [<span data-ttu-id="f3aff-121">Tilldela en användare tooan program direkt</span><span class="sxs-lookup"><span data-stu-id="f3aff-121">Assign a user tooan application directly</span></span>](#assign-a-user-to-an-application-directly)

    -   [<span data-ttu-id="f3aff-122">Tilldela en programgrupp tooa direkt</span><span class="sxs-lookup"><span data-stu-id="f3aff-122">Assign an application tooa group directly</span></span>](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="f3aff-123">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="f3aff-123">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="f3aff-124">tooadd ett program från hello Azure AD-galleriet så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="f3aff-124">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="f3aff-125">Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="f3aff-125">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="f3aff-126">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f3aff-126">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f3aff-127">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="f3aff-127">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f3aff-128">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f3aff-128">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f3aff-129">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet</span><span class="sxs-lookup"><span data-stu-id="f3aff-129">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="f3aff-130">I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program</span><span class="sxs-lookup"><span data-stu-id="f3aff-130">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application</span></span>

7.  <span data-ttu-id="f3aff-131">Välj hello-program som du vill använda tooconfigure för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3aff-131">Select hello application you want tooconfigure for single sign-on</span></span>

8.  <span data-ttu-id="f3aff-132">Innan du lägger till programmet hello kan du ändra dess namn från hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="f3aff-132">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="f3aff-133">Klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="f3aff-133">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="f3aff-134">Efter en kort period vara kan toosee hello programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="f3aff-134">After a short period, you be able toosee hello application’s configuration blade.</span></span>

## <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="f3aff-135">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3aff-135">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="f3aff-136">tooconfigure enkel inloggning för ett program gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="f3aff-136">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="f3aff-137">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="f3aff-137">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="f3aff-138">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f3aff-138">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f3aff-139">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="f3aff-139">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f3aff-140">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f3aff-140">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f3aff-141">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="f3aff-141">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="f3aff-142">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="f3aff-142">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="f3aff-143">Välj hello-program som du vill tooconfigure enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3aff-143">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="f3aff-144">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f3aff-144">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="f3aff-145">Välj hello läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="f3aff-145">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="f3aff-146">[Tilldela användare toohello program](#assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="f3aff-146">[Assign users toohello application](#assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="f3aff-147">Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare.</span><span class="sxs-lookup"><span data-stu-id="f3aff-147">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="f3aff-148">Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.</span><span class="sxs-lookup"><span data-stu-id="f3aff-148">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="assign-a-user-tooan-application-directly"></a><span data-ttu-id="f3aff-149">Tilldela en användare tooan program direkt</span><span class="sxs-lookup"><span data-stu-id="f3aff-149">Assign a user tooan application directly</span></span>

<span data-ttu-id="f3aff-150">tooassign en eller flera användare tooan programmet direkt, gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="f3aff-150">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="f3aff-151">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="f3aff-151">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f3aff-152">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f3aff-152">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f3aff-153">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="f3aff-153">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f3aff-154">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f3aff-154">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f3aff-155">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="f3aff-155">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="f3aff-156">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="f3aff-156">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="f3aff-157">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f3aff-157">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="f3aff-158">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f3aff-158">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="f3aff-159">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="f3aff-159">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="f3aff-160">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="f3aff-160">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="f3aff-161">Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="f3aff-161">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="f3aff-162">Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="f3aff-162">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="f3aff-163">Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="f3aff-163">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="f3aff-164">**Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="f3aff-164">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="f3aff-165">När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="f3aff-165">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="f3aff-166">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="f3aff-166">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="f3aff-167">Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.</span><span class="sxs-lookup"><span data-stu-id="f3aff-167">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

## <a name="assign-an-application-tooa-group-directly"></a><span data-ttu-id="f3aff-168">Tilldela en programgrupp tooa direkt</span><span class="sxs-lookup"><span data-stu-id="f3aff-168">Assign an application tooa group directly</span></span>

<span data-ttu-id="f3aff-169">tooassign en eller flera grupper tooan programmet direkt, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="f3aff-169">tooassign one or more groups tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="f3aff-170">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="f3aff-170">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f3aff-171">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f3aff-171">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f3aff-172">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="f3aff-172">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f3aff-173">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f3aff-173">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f3aff-174">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="f3aff-174">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="f3aff-175">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="f3aff-175">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="f3aff-176">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f3aff-176">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="f3aff-177">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f3aff-177">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="f3aff-178">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="f3aff-178">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="f3aff-179">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="f3aff-179">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="f3aff-180">Typen i hello **fullständig gruppnamn** av hello-grupp som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="f3aff-180">Type in hello **full group name** of hello group you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="f3aff-181">Hovra över hello **grupp** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="f3aff-181">Hover over hello **group** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="f3aff-182">Klicka på hello kryssrutan nästa toohello gruppens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="f3aff-182">Click hello checkbox next toohello group’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="f3aff-183">**Valfritt:** om du vill ha för**lägga till fler än en grupp**, typ i en annan **fullständig gruppnamn** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här gruppen toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="f3aff-183">**Optional:** If you would like too**add more than one group**, type in another **full group name** into hello **Search by name or email address** search box, and click hello checkbox tooadd this group toohello **Selected** list.</span></span>

13. <span data-ttu-id="f3aff-184">När du har valt grupper klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="f3aff-184">When you are finished selecting groups, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="f3aff-185">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello grupper som du har valt.</span><span class="sxs-lookup"><span data-stu-id="f3aff-185">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello groups you have selected.</span></span>

15. <span data-ttu-id="f3aff-186">Klicka på hello **tilldela** knappen tooassign hello programmet toohello valda grupper.</span><span class="sxs-lookup"><span data-stu-id="f3aff-186">Click hello **Assign** button tooassign hello application toohello selected groups.</span></span>

<span data-ttu-id="f3aff-187">Efter en kort period hello-användare som du har valt att kan toolaunch programmen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f3aff-187">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3aff-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f3aff-188">Next steps</span></span>
[<span data-ttu-id="f3aff-189">Tillhandahålla enkel inloggning tooyour appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="f3aff-189">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
