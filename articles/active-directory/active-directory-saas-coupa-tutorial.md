---
title: "Självstudier: Azure Active Directory-integrering med Coupa | Microsoft Docs"
description: "Lär dig hur toouse Coupa med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 87e98573718d27d408c886466a374a987f58faa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a><span data-ttu-id="b5621-103">Självstudier: Azure Active Directory-integrering med Coupa</span><span class="sxs-lookup"><span data-stu-id="b5621-103">Tutorial: Azure Active Directory integration with Coupa</span></span>
<span data-ttu-id="b5621-104">hello syftet med den här kursen är tooshow hello integreringen av Azure och Coupa.</span><span class="sxs-lookup"><span data-stu-id="b5621-104">hello objective of this tutorial is tooshow hello integration of Azure and Coupa.</span></span>  
<span data-ttu-id="b5621-105">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b5621-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="b5621-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b5621-106">A valid Azure subscription</span></span>
* <span data-ttu-id="b5621-107">En Coupa enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="b5621-107">A Coupa single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="b5621-108">Den här kursen hello Azure AD-användare som du har tilldelat tooCoupa blir toosingle kan logga in på hello program med hjälp av hello [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b5621-108">After completing this tutorial, hello Azure AD users you have assigned tooCoupa will be able toosingle sign into hello application using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="b5621-109">hello-scenario som beskrivs i den här kursen består av följande byggblock hello:</span><span class="sxs-lookup"><span data-stu-id="b5621-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

* <span data-ttu-id="b5621-110">Aktivera programmet hello-integrering för Coupa</span><span class="sxs-lookup"><span data-stu-id="b5621-110">Enabling hello application integration for Coupa</span></span>
* <span data-ttu-id="b5621-111">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5621-111">Configuring single sign-on</span></span>
* <span data-ttu-id="b5621-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="b5621-112">Configuring user provisioning</span></span>
* <span data-ttu-id="b5621-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="b5621-113">Assigning users</span></span>

<span data-ttu-id="b5621-114">![Scenariot](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="b5621-114">![Scenario](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-coupa"></a><span data-ttu-id="b5621-115">Aktivera hello program-integration för Coupa</span><span class="sxs-lookup"><span data-stu-id="b5621-115">Enable hello application integration for Coupa</span></span>
<span data-ttu-id="b5621-116">hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Coupa.</span><span class="sxs-lookup"><span data-stu-id="b5621-116">hello objective of this section is toooutline how tooenable hello application integration for Coupa.</span></span>

<span data-ttu-id="b5621-117">**tooenable hello programintegrationstyp för Coupa, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b5621-117">**tooenable hello application integration for Coupa, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5621-118">I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b5621-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="b5621-119">![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="b5621-119">![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="b5621-120">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="b5621-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="b5621-121">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="b5621-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="b5621-122">![Program](./media/active-directory-saas-coupa-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="b5621-122">![Applications](./media/active-directory-saas-coupa-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="b5621-123">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="b5621-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="b5621-124">![Lägg till program](./media/active-directory-saas-coupa-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="b5621-124">![Add application](./media/active-directory-saas-coupa-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="b5621-125">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="b5621-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="b5621-126">![Lägga till ett program från gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="b5621-126">![Add an application from gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="b5621-127">I hello **sökrutan**, typen **Coupa**.</span><span class="sxs-lookup"><span data-stu-id="b5621-127">In hello **search box**, type **Coupa**.</span></span>
   
   <span data-ttu-id="b5621-128">![Programgalleriet](./media/active-directory-saas-coupa-tutorial/IC791898.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="b5621-128">![Application Gallery](./media/active-directory-saas-coupa-tutorial/IC791898.png "Application Gallery")</span></span>
7. <span data-ttu-id="b5621-129">I resultatfönstret hello väljer **Coupa**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b5621-129">In hello results pane, select **Coupa**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="b5621-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span><span class="sxs-lookup"><span data-stu-id="b5621-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="b5621-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5621-131">Configure single sign-on</span></span>

<span data-ttu-id="b5621-132">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooCoupa med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="b5621-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooCoupa with their account in Azure AD using federation based on hello SAML protocol.</span></span>  

<span data-ttu-id="b5621-133">Konfigurera enkel inloggning för Coupa måste du tooretrieve en tumavtrycksvärde från ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="b5621-133">Configuring single sign-on for Coupa requires you tooretrieve a thumbprint value from a certificate.</span></span> <span data-ttu-id="b5621-134">Om du inte är bekant med den här proceduren, se [hur tooretrieve en certifikatets tumavtrycksvärde](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="b5621-134">If you are not familiar with this procedure, see [How tooretrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span>

<span data-ttu-id="b5621-135">**tooconfigure enkel inloggning, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b5621-135">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5621-136">Inloggning tooyour Coupa företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b5621-136">Sign on tooyour Coupa company site as an administrator.</span></span>
2. <span data-ttu-id="b5621-137">Gå för**installationsprogrammet \> säkerhetskontroll**.</span><span class="sxs-lookup"><span data-stu-id="b5621-137">Go too**Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="b5621-138">![Säkerhetsåtgärder](./media/active-directory-saas-coupa-tutorial/IC791900.png "säkerhetsåtgärder")</span><span class="sxs-lookup"><span data-stu-id="b5621-138">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
3. <span data-ttu-id="b5621-139">toodownload hello Coupa metadata filen tooyour dator, klicka på **hämta och importera SP metadata**.</span><span class="sxs-lookup"><span data-stu-id="b5621-139">toodownload hello Coupa metadata file tooyour computer, click **Download and import SP metadata**.</span></span>
   
   <span data-ttu-id="b5621-140">![Coupa SP metadata](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadata")</span><span class="sxs-lookup"><span data-stu-id="b5621-140">![Coupa SP metadata](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadata")</span></span>
4. <span data-ttu-id="b5621-141">Inloggning i en annan webbläsarfönster toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b5621-141">In a different browser window, sign on toohello Azure classic portal.</span></span>
5. <span data-ttu-id="b5621-142">På hello **Coupa** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5621-142">On hello **Coupa** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="b5621-143">![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791902.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="b5621-143">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791902.png "Configure Single Sign-On")</span></span>
6. <span data-ttu-id="b5621-144">På hello **hur skulle du som användare toosign på tooCoupa** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b5621-144">On hello **How would you like users toosign on tooCoupa** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="b5621-145">![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791903.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="b5621-145">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791903.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="b5621-146">På hello **konfigurera App-URL** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b5621-146">On hello **Configure App URL** page, perform hello following steps:</span></span>
   
   <span data-ttu-id="b5621-147">![Konfigurera App-URL](./media/active-directory-saas-coupa-tutorial/IC791904.png "konfigurera App-URL")</span><span class="sxs-lookup"><span data-stu-id="b5621-147">![Configure App URL](./media/active-directory-saas-coupa-tutorial/IC791904.png "Configure App URL")</span></span>   
   1. <span data-ttu-id="b5621-148">I hello **logga URL** textruta typen URL som används av dina användare toosign på tooyour Coupa program (t.ex. ”:*http://company.Coupa.com*”).</span><span class="sxs-lookup"><span data-stu-id="b5621-148">In hello **Sign On URL** textbox, type URL used by your users toosign on tooyour Coupa application (e.g.: “*http://company.Coupa.com*”).</span></span>
   2. <span data-ttu-id="b5621-149">Öppna din hämtade Coupa metadatafilen och kopiera hello **AssertionConsumerService index-URL**.</span><span class="sxs-lookup"><span data-stu-id="b5621-149">Open your downloaded Coupa metadata file, and then copy hello **AssertionConsumerService index/URL**.</span></span>
   3. <span data-ttu-id="b5621-150">I hello **Coupa Reply URL** textruta klistra in hello **AssertionConsumerService index-URL** värde.</span><span class="sxs-lookup"><span data-stu-id="b5621-150">In hello **Coupa Reply URL** textbox, paste hello **AssertionConsumerService index/URL** value.</span></span>
   4. <span data-ttu-id="b5621-151">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b5621-151">Click **Next**.</span></span>
8. <span data-ttu-id="b5621-152">På hello **Konfigurera enkel inloggning på Coupa** sida, toodownload metadatafil klickar du på **hämtar metadata**, och sedan spara hello filen lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="b5621-152">On hello **Configure single sign-on at Coupa** page, toodownload your metadata file, click **Download metadata**, and then save hello file locally on your computer.</span></span>
   
   <span data-ttu-id="b5621-153">![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791905.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="b5621-153">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791905.png "Configure Single Sign-On")</span></span>
9. <span data-ttu-id="b5621-154">Hej Coupa företagets webbplats gå för**installationsprogrammet \> säkerhetskontroll**.</span><span class="sxs-lookup"><span data-stu-id="b5621-154">On hello Coupa company site, go too**Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="b5621-155">![Säkerhetsåtgärder](./media/active-directory-saas-coupa-tutorial/IC791900.png "säkerhetsåtgärder")</span><span class="sxs-lookup"><span data-stu-id="b5621-155">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
10. <span data-ttu-id="b5621-156">I hello **logga in med autentiseringsuppgifterna för Coupa** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b5621-156">In hello **Log in using Coupa credentials** section, perform hello following steps:</span></span>  

   <span data-ttu-id="b5621-157">![Logga in med autentiseringsuppgifterna för Coupa](./media/active-directory-saas-coupa-tutorial/IC791906.png "logga in med autentiseringsuppgifterna för Coupa")</span><span class="sxs-lookup"><span data-stu-id="b5621-157">![Log in using Coupa credentials](./media/active-directory-saas-coupa-tutorial/IC791906.png "Log in using Coupa credentials")</span></span> 
   1. <span data-ttu-id="b5621-158">Välj **logga in med SAML**.</span><span class="sxs-lookup"><span data-stu-id="b5621-158">Select **Log in using SAML**.</span></span>
   2. <span data-ttu-id="b5621-159">Klicka på **Bläddra** tooupload din hämtade Azure Active metadatafil.</span><span class="sxs-lookup"><span data-stu-id="b5621-159">Click **Browse** tooupload your downloaded Azure Active metadata file.</span></span>
   3. <span data-ttu-id="b5621-160">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b5621-160">Click **Save**.</span></span>
11. <span data-ttu-id="b5621-161">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5621-161">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
   <span data-ttu-id="b5621-162">![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791907.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="b5621-162">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791907.png "Configure Single Sign-On")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="b5621-163">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="b5621-163">Configure user provisioning</span></span>

<span data-ttu-id="b5621-164">I ordning tooenable Azure AD-användare toolog i Coupa, måste de etableras i Coupa.</span><span class="sxs-lookup"><span data-stu-id="b5621-164">In order tooenable Azure AD users toolog into Coupa, they must be provisioned into Coupa.</span></span>  

* <span data-ttu-id="b5621-165">Hello gäller Coupa är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b5621-165">In hello case of Coupa, provisioning is a manual task.</span></span>

<span data-ttu-id="b5621-166">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="b5621-166">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5621-167">Logga in tooyour **Coupa** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b5621-167">Log in tooyour **Coupa** company site as administrator.</span></span>
2. <span data-ttu-id="b5621-168">Hello-menyn överst hello **installationsprogrammet**, och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="b5621-168">In hello menu on hello top, click **Setup**, and then click **Users**.</span></span>
   
   <span data-ttu-id="b5621-169">![Användare](./media/active-directory-saas-coupa-tutorial/IC791908.png "användare")</span><span class="sxs-lookup"><span data-stu-id="b5621-169">![Users](./media/active-directory-saas-coupa-tutorial/IC791908.png "Users")</span></span>
3. <span data-ttu-id="b5621-170">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b5621-170">Click **Create**.</span></span>
   
   <span data-ttu-id="b5621-171">![Skapa användare](./media/active-directory-saas-coupa-tutorial/IC791909.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="b5621-171">![Create Users](./media/active-directory-saas-coupa-tutorial/IC791909.png "Create Users")</span></span>
4. <span data-ttu-id="b5621-172">I hello **användare skapa** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b5621-172">In hello **User Create** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="b5621-173">![Användarinformation](./media/active-directory-saas-coupa-tutorial/IC791910.png "användarinformation")</span><span class="sxs-lookup"><span data-stu-id="b5621-173">![User Details](./media/active-directory-saas-coupa-tutorial/IC791910.png "User Details")</span></span>
   
   1. <span data-ttu-id="b5621-174">Typen hello **inloggning**, **Förnamn**, **efternamn**, **ID för enkel inloggning**, **e-post** attribut för en giltigt Azure Active Directory-konto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="b5621-174">Type hello **Login**, **First name**, **Last Name**, **Single Sign-On ID**, **Email** attributes of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   2. <span data-ttu-id="b5621-175">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b5621-175">Click **Create**.</span></span>   
   >[!NOTE]
   ><span data-ttu-id="b5621-176">hello Azure Active Directory användare får ett e-postmeddelande med en länk tooconfirm hello-konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="b5621-176">hello Azure Active Directory account holder will get an email with a link tooconfirm hello account before it becomes active.</span></span> 
   > 

>[!NOTE]
><span data-ttu-id="b5621-177">Du kan använda något annat Coupa användarens konto skapas verktyg eller API: er som tillhandahålls av Coupa tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="b5621-177">You can use any other Coupa user account creation tools or APIs provided by Coupa tooprovision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="b5621-178">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="b5621-178">Assign users</span></span>
<span data-ttu-id="b5621-179">tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.</span><span class="sxs-lookup"><span data-stu-id="b5621-179">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="b5621-180">**tooassign användare tooCoupa utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b5621-180">**tooassign users tooCoupa, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5621-181">Skapa ett testkonto i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b5621-181">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="b5621-182">På hello ** Coupa ** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="b5621-182">On hello **Coupa **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="b5621-183">![Tilldela användare](./media/active-directory-saas-coupa-tutorial/IC791911.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="b5621-183">![Assign Users](./media/active-directory-saas-coupa-tutorial/IC791911.png "Assign Users")</span></span>
3. <span data-ttu-id="b5621-184">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="b5621-184">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="b5621-185">![Ja](./media/active-directory-saas-coupa-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="b5621-185">![Yes](./media/active-directory-saas-coupa-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="b5621-186">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b5621-186">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="b5621-187">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b5621-187">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

