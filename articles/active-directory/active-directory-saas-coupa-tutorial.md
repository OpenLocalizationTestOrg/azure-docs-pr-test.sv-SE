---
title: "Självstudier: Azure Active Directory-integrering med Coupa | Microsoft Docs"
description: "Lär dig hur du använder Coupa med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!"
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
ms.openlocfilehash: c952975919cfd948f1b9ea93ff2ac2641a53f923
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a><span data-ttu-id="ad2d4-103">Självstudier: Azure Active Directory-integrering med Coupa</span><span class="sxs-lookup"><span data-stu-id="ad2d4-103">Tutorial: Azure Active Directory integration with Coupa</span></span>
<span data-ttu-id="ad2d4-104">Syftet med den här kursen är att visa integreringen av Azure och Coupa.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-104">The objective of this tutorial is to show the integration of Azure and Coupa.</span></span>  
<span data-ttu-id="ad2d4-105">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ad2d4-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="ad2d4-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ad2d4-106">A valid Azure subscription</span></span>
* <span data-ttu-id="ad2d4-107">En Coupa enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="ad2d4-107">A Coupa single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="ad2d4-108">Den här kursen Azure AD-användare som du har tilldelat Coupa kommer att kunna enkel inloggning till programmet med hjälp av den [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ad2d4-108">After completing this tutorial, the Azure AD users you have assigned to Coupa will be able to single sign into the application using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="ad2d4-109">Det scenario som beskrivs i den här kursen består av följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ad2d4-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

* <span data-ttu-id="ad2d4-110">Aktivera programintegrationstyp för Coupa</span><span class="sxs-lookup"><span data-stu-id="ad2d4-110">Enabling the application integration for Coupa</span></span>
* <span data-ttu-id="ad2d4-111">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ad2d4-111">Configuring single sign-on</span></span>
* <span data-ttu-id="ad2d4-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="ad2d4-112">Configuring user provisioning</span></span>
* <span data-ttu-id="ad2d4-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="ad2d4-113">Assigning users</span></span>

<span data-ttu-id="ad2d4-114">![Scenariot](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-114">![Scenario](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-coupa"></a><span data-ttu-id="ad2d4-115">Aktivera application-integration för Coupa</span><span class="sxs-lookup"><span data-stu-id="ad2d4-115">Enable the application integration for Coupa</span></span>
<span data-ttu-id="ad2d4-116">Syftet med det här avsnittet är att beskriva hur du aktiverar programintegrationstyp för Coupa.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-116">The objective of this section is to outline how to enable the application integration for Coupa.</span></span>

<span data-ttu-id="ad2d4-117">**Utför följande steg om du vill aktivera programmet-integrering för Coupa:**</span><span class="sxs-lookup"><span data-stu-id="ad2d4-117">**To enable the application integration for Coupa, perform the following steps:**</span></span>

1. <span data-ttu-id="ad2d4-118">I den klassiska Azure-portalen i det vänstra navigeringsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="ad2d4-119">![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-119">![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="ad2d4-120">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="ad2d4-121">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="ad2d4-122">![Program](./media/active-directory-saas-coupa-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-122">![Applications](./media/active-directory-saas-coupa-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="ad2d4-123">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="ad2d4-124">![Lägg till program](./media/active-directory-saas-coupa-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-124">![Add application](./media/active-directory-saas-coupa-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="ad2d4-125">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="ad2d4-126">![Lägga till ett program från gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-126">![Add an application from gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="ad2d4-127">I den **sökrutan**, typen **Coupa**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-127">In the **search box**, type **Coupa**.</span></span>
   
   <span data-ttu-id="ad2d4-128">![Programgalleriet](./media/active-directory-saas-coupa-tutorial/IC791898.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-128">![Application Gallery](./media/active-directory-saas-coupa-tutorial/IC791898.png "Application Gallery")</span></span>
7. <span data-ttu-id="ad2d4-129">I resultatfönstret, Välj **Coupa**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-129">In the results pane, select **Coupa**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="ad2d4-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="ad2d4-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ad2d4-131">Configure single sign-on</span></span>

<span data-ttu-id="ad2d4-132">Syftet med det här avsnittet är att beskriva hur användarna att autentisera till Coupa med sitt konto i Azure AD med hjälp av federation baserat på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-132">The objective of this section is to outline how to enable users to authenticate to Coupa with their account in Azure AD using federation based on the SAML protocol.</span></span>  

<span data-ttu-id="ad2d4-133">Konfigurera enkel inloggning för Coupa måste du hämta ett tumavtrycksvärde från ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-133">Configuring single sign-on for Coupa requires you to retrieve a thumbprint value from a certificate.</span></span> <span data-ttu-id="ad2d4-134">Om du inte är bekant med den här proceduren, se [hur du hämtar ett certifikat tumavtrycksvärde](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="ad2d4-134">If you are not familiar with this procedure, see [How to retrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span>

<span data-ttu-id="ad2d4-135">**Utför följande steg för att konfigurera enkel inloggning:**</span><span class="sxs-lookup"><span data-stu-id="ad2d4-135">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="ad2d4-136">Logga in på webbplatsen Coupa företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-136">Sign on to your Coupa company site as an administrator.</span></span>
2. <span data-ttu-id="ad2d4-137">Gå till **installationsprogrammet \> säkerhetskontroll**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-137">Go to **Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="ad2d4-138">![Säkerhetsåtgärder](./media/active-directory-saas-coupa-tutorial/IC791900.png "säkerhetsåtgärder")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-138">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
3. <span data-ttu-id="ad2d4-139">Om du vill hämta metadatafil Coupa till datorn klickar du på **hämta och importera SP metadata**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-139">To download the Coupa metadata file to your computer, click **Download and import SP metadata**.</span></span>
   
   <span data-ttu-id="ad2d4-140">![Coupa SP metadata](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadata")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-140">![Coupa SP metadata](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadata")</span></span>
4. <span data-ttu-id="ad2d4-141">Logga in på den klassiska Azure-portalen i ett annat webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-141">In a different browser window, sign on to the Azure classic portal.</span></span>
5. <span data-ttu-id="ad2d4-142">På den **Coupa** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-142">On the **Coupa** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="ad2d4-143">![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791902.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-143">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791902.png "Configure Single Sign-On")</span></span>
6. <span data-ttu-id="ad2d4-144">På den **hur vill du att användarna kan logga in på Coupa** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-144">On the **How would you like users to sign on to Coupa** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="ad2d4-145">![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791903.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-145">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791903.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="ad2d4-146">På den **konfigurera App-URL** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ad2d4-146">On the **Configure App URL** page, perform the following steps:</span></span>
   
   <span data-ttu-id="ad2d4-147">![Konfigurera App-URL](./media/active-directory-saas-coupa-tutorial/IC791904.png "konfigurera App-URL")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-147">![Configure App URL](./media/active-directory-saas-coupa-tutorial/IC791904.png "Configure App URL")</span></span>   
   1. <span data-ttu-id="ad2d4-148">I den **logga URL** textruta typen URL som används av dina användare logga in i tillämpningsprogrammet Coupa (t.ex. ”:*http://company.Coupa.com*”).</span><span class="sxs-lookup"><span data-stu-id="ad2d4-148">In the **Sign On URL** textbox, type URL used by your users to sign on to your Coupa application (e.g.: “*http://company.Coupa.com*”).</span></span>
   2. <span data-ttu-id="ad2d4-149">Öppna din hämtade Coupa metadatafilen och kopiera den **AssertionConsumerService index-URL**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-149">Open your downloaded Coupa metadata file, and then copy the **AssertionConsumerService index/URL**.</span></span>
   3. <span data-ttu-id="ad2d4-150">I den **Coupa Reply URL** textruta klistra in den **AssertionConsumerService index-URL** värde.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-150">In the **Coupa Reply URL** textbox, paste the **AssertionConsumerService index/URL** value.</span></span>
   4. <span data-ttu-id="ad2d4-151">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-151">Click **Next**.</span></span>
8. <span data-ttu-id="ad2d4-152">På den **Konfigurera enkel inloggning på Coupa** att hämta metadatafil klickar du på **hämtar metadata**, och sedan spara filen lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-152">On the **Configure single sign-on at Coupa** page, to download your metadata file, click **Download metadata**, and then save the file locally on your computer.</span></span>
   
   <span data-ttu-id="ad2d4-153">![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791905.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-153">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791905.png "Configure Single Sign-On")</span></span>
9. <span data-ttu-id="ad2d4-154">Gå till på webbplatsen Coupa företagets **installationsprogrammet \> säkerhetskontroll**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-154">On the Coupa company site, go to **Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="ad2d4-155">![Säkerhetsåtgärder](./media/active-directory-saas-coupa-tutorial/IC791900.png "säkerhetsåtgärder")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-155">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
10. <span data-ttu-id="ad2d4-156">I den **logga in med autentiseringsuppgifterna för Coupa** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ad2d4-156">In the **Log in using Coupa credentials** section, perform the following steps:</span></span>  

   <span data-ttu-id="ad2d4-157">![Logga in med autentiseringsuppgifterna för Coupa](./media/active-directory-saas-coupa-tutorial/IC791906.png "logga in med autentiseringsuppgifterna för Coupa")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-157">![Log in using Coupa credentials](./media/active-directory-saas-coupa-tutorial/IC791906.png "Log in using Coupa credentials")</span></span> 
   1. <span data-ttu-id="ad2d4-158">Välj **logga in med SAML**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-158">Select **Log in using SAML**.</span></span>
   2. <span data-ttu-id="ad2d4-159">Klicka på **Bläddra** att överföra din hämtade Azure Active metadatafil.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-159">Click **Browse** to upload your downloaded Azure Active metadata file.</span></span>
   3. <span data-ttu-id="ad2d4-160">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-160">Click **Save**.</span></span>
11. <span data-ttu-id="ad2d4-161">Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **Slutför** att stänga den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-161">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
    
   <span data-ttu-id="ad2d4-162">![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791907.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-162">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791907.png "Configure Single Sign-On")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="ad2d4-163">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="ad2d4-163">Configure user provisioning</span></span>

<span data-ttu-id="ad2d4-164">För att aktivera Azure AD-användare att logga in på Coupa etableras de i Coupa.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-164">In order to enable Azure AD users to log into Coupa, they must be provisioned into Coupa.</span></span>  

* <span data-ttu-id="ad2d4-165">När det gäller Coupa är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-165">In the case of Coupa, provisioning is a manual task.</span></span>

<span data-ttu-id="ad2d4-166">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="ad2d4-166">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="ad2d4-167">Logga in på ditt **Coupa** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-167">Log in to your **Coupa** company site as administrator.</span></span>
2. <span data-ttu-id="ad2d4-168">Klicka på menyn högst upp **installationsprogrammet**, och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-168">In the menu on the top, click **Setup**, and then click **Users**.</span></span>
   
   <span data-ttu-id="ad2d4-169">![Användare](./media/active-directory-saas-coupa-tutorial/IC791908.png "användare")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-169">![Users](./media/active-directory-saas-coupa-tutorial/IC791908.png "Users")</span></span>
3. <span data-ttu-id="ad2d4-170">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-170">Click **Create**.</span></span>
   
   <span data-ttu-id="ad2d4-171">![Skapa användare](./media/active-directory-saas-coupa-tutorial/IC791909.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-171">![Create Users](./media/active-directory-saas-coupa-tutorial/IC791909.png "Create Users")</span></span>
4. <span data-ttu-id="ad2d4-172">I den **användare skapa** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ad2d4-172">In the **User Create** section, perform the following steps:</span></span>
   
   <span data-ttu-id="ad2d4-173">![Användarinformation](./media/active-directory-saas-coupa-tutorial/IC791910.png "användarinformation")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-173">![User Details](./media/active-directory-saas-coupa-tutorial/IC791910.png "User Details")</span></span>
   
   1. <span data-ttu-id="ad2d4-174">Typ av **inloggning**, **Förnamn**, **efternamn**, **ID för enkel inloggning**, **e-post** attribut för ett giltigt Azure Active Directory-konto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-174">Type the **Login**, **First name**, **Last Name**, **Single Sign-On ID**, **Email** attributes of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   2. <span data-ttu-id="ad2d4-175">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-175">Click **Create**.</span></span>   
   >[!NOTE]
   ><span data-ttu-id="ad2d4-176">Azure Active Directory kontoinnehavaren får ett e-postmeddelande med en länk till bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-176">The Azure Active Directory account holder will get an email with a link to confirm the account before it becomes active.</span></span> 
   > 

>[!NOTE]
><span data-ttu-id="ad2d4-177">Du kan använda något annat Coupa användarens konto skapas verktyg eller API: er som tillhandahålls av Coupa att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-177">You can use any other Coupa user account creation tools or APIs provided by Coupa to provision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="ad2d4-178">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="ad2d4-178">Assign users</span></span>
<span data-ttu-id="ad2d4-179">Om du vill testa konfigurationen måste du bevilja Azure AD-användare som du vill tillåta med hjälp av ditt programåtkomst till den genom att tilldela dem.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-179">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="ad2d4-180">**Utför följande steg om du vill tilldela Coupa användare:**</span><span class="sxs-lookup"><span data-stu-id="ad2d4-180">**To assign users to Coupa, perform the following steps:**</span></span>

1. <span data-ttu-id="ad2d4-181">Skapa ett testkonto i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-181">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="ad2d4-182">På den ** Coupa ** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-182">On the **Coupa **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="ad2d4-183">![Tilldela användare](./media/active-directory-saas-coupa-tutorial/IC791911.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-183">![Assign Users](./media/active-directory-saas-coupa-tutorial/IC791911.png "Assign Users")</span></span>
3. <span data-ttu-id="ad2d4-184">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** att bekräfta din tilldelning.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-184">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="ad2d4-185">![Ja](./media/active-directory-saas-coupa-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="ad2d4-185">![Yes](./media/active-directory-saas-coupa-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="ad2d4-186">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ad2d4-186">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="ad2d4-187">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ad2d4-187">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

