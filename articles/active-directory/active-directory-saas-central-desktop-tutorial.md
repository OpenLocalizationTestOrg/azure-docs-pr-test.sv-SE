---
title: "Självstudier: Azure Active Directory-integrering med Central Desktop | Microsoft Docs"
description: "Lär dig hur du använder centrala skrivbordet med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: fe32c1d68040ceb9d9de2ad6c4a6dc9ea93f5aef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a><span data-ttu-id="2d835-103">Självstudier: Azure Active Directory-integrering med Central Desktop</span><span class="sxs-lookup"><span data-stu-id="2d835-103">Tutorial: Azure Active Directory integration with Central Desktop</span></span>
<span data-ttu-id="2d835-104">Syftet med den här kursen är att visa integreringen av Azure och centrala skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="2d835-104">The objective of this tutorial is to show the integration of Azure and Central Desktop.</span></span> <span data-ttu-id="2d835-105">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2d835-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="2d835-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2d835-106">A valid Azure subscription</span></span>
* <span data-ttu-id="2d835-107">En Central skrivbordet för enkel inloggning aktiverad prenumeration / centrala desktop-klient</span><span class="sxs-lookup"><span data-stu-id="2d835-107">A Central desktop single sign on enabled subscription / Central desktop tenant</span></span>

<span data-ttu-id="2d835-108">Det scenario som beskrivs i den här kursen består av följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2d835-108">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

* <span data-ttu-id="2d835-109">Aktivera programintegrationstyp för Central skrivbord</span><span class="sxs-lookup"><span data-stu-id="2d835-109">Enabling the application integration for Central Desktop</span></span>
* <span data-ttu-id="2d835-110">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="2d835-110">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="2d835-111">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="2d835-111">Configuring user provisioning</span></span>
* <span data-ttu-id="2d835-112">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="2d835-112">Assigning users</span></span>

<span data-ttu-id="2d835-113">![Scenariot](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="2d835-113">![Scenario](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-central-desktop"></a><span data-ttu-id="2d835-114">Aktivera application-integrering för centrala skrivbord</span><span class="sxs-lookup"><span data-stu-id="2d835-114">Enable the application integration for Central Desktop</span></span>
<span data-ttu-id="2d835-115">Syftet med det här avsnittet är att beskriva hur du aktiverar programintegrationstyp för Central skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="2d835-115">The objective of this section is to outline how to enable the application integration for Central Desktop.</span></span>

<span data-ttu-id="2d835-116">**Utför följande steg om du vill aktivera programintegrationstyp för Central skrivbordet:**</span><span class="sxs-lookup"><span data-stu-id="2d835-116">**To enable the application integration for Central Desktop, perform the following steps:**</span></span>

1. <span data-ttu-id="2d835-117">I den klassiska Azure-portalen i det vänstra navigeringsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2d835-117">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="2d835-118">![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="2d835-118">![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="2d835-119">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="2d835-119">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="2d835-120">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d835-120">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="2d835-121">![Program](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="2d835-121">![Applications](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="2d835-122">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="2d835-122">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="2d835-123">![Lägg till program](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="2d835-123">![Add application](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="2d835-124">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="2d835-124">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="2d835-125">![Lägga till ett program från gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="2d835-125">![Add an application from gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="2d835-126">I den **sökrutan**, typen **centrala Desktop**.</span><span class="sxs-lookup"><span data-stu-id="2d835-126">In the **search box**, type **Central Desktop**.</span></span>
   
   <span data-ttu-id="2d835-127">![Programgalleriet](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="2d835-127">![Application gallery](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Application gallery")</span></span>
7. <span data-ttu-id="2d835-128">I resultatfönstret, Välj **centrala Desktop**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="2d835-128">In the results pane, select **Central Desktop**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="2d835-129">![Central Desktop](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "centrala Desktop")</span><span class="sxs-lookup"><span data-stu-id="2d835-129">![Central Desktop](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Central Desktop")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="2d835-130">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2d835-130">Configure single sign-on</span></span>

<span data-ttu-id="2d835-131">Syftet med det här avsnittet är att beskriva hur användarna att autentisera till centrala skrivbordet med sitt konto i Azure AD med hjälp av federation baserat på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="2d835-131">The objective of this section is to outline how to enable users to authenticate to Central Desktop with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="2d835-132">Som en del av den här proceduren måste krävs du för att överföra en Base64-kodade certifikat till din centrala Desktop-klient.</span><span class="sxs-lookup"><span data-stu-id="2d835-132">As part of this procedure, you are required to upload a base-64 encoded certificate to your Central Desktop tenant.</span></span>  
<span data-ttu-id="2d835-133">Om du inte är bekant med den här proceduren, se [hur du konverterar ett binärt certifikat till en textfil](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="2d835-133">If you are not familiar with this procedure, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>

<span data-ttu-id="2d835-134">**Utför följande steg för att konfigurera enkel inloggning:**</span><span class="sxs-lookup"><span data-stu-id="2d835-134">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="2d835-135">I den klassiska Azure-portalen på den **centrala Desktop** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den ** Konfigurera enkel inloggning ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2d835-135">In the Azure classic portal, on the **Central Desktop** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On ** dialog.</span></span>
   
   <span data-ttu-id="2d835-136">![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="2d835-136">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="2d835-137">På den **hur vill du att användarna kan logga in på centrala Desktop** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2d835-137">On the **How would you like users to sign on to Central Desktop** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="2d835-138">![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="2d835-138">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="2d835-139">På den **konfigurera App-URL** , utför följande steg, och klickar sedan på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="2d835-139">On the **Configure App URL** page, perform the following steps, and then click **Next**:</span></span> 
   
   1. <span data-ttu-id="2d835-140">I den **Desktop tecken i URL: en Central** textruta anger du URL för din centrala Desktop-klient (t.ex.: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="2d835-140">In the **Central Desktop Sign In URL** textbox, type the URL of your Central Desktop tenant (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   2. <span data-ttu-id="2d835-141">Ange din centrala Desktop AssertionConsumerService URL i textrutan centrala Desktop Reply URL (t.ex.: https://contoso.centraldesktop.com/saml2-assertion.php).</span><span class="sxs-lookup"><span data-stu-id="2d835-141">In the Central  Desktop Reply URL textbox, type your Central Desktop AssertionConsumerService URL (e.g.:  https://contoso.centraldesktop.com/saml2-assertion.php).</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="2d835-142">Du kan hämta värdet från central skrivbord metadata (t.ex.: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="2d835-142">You can get the value from the central desktop metadata (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   >  
   
   <span data-ttu-id="2d835-143">![Konfigurera app-URL](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="2d835-143">![Configure app URL](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Configure app URL")</span></span>
4. <span data-ttu-id="2d835-144">På den **Konfigurera enkel inloggning på centrala Desktop** att ladda ned ditt certifikat klickar du på **hämta certifikat**, och sedan spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="2d835-144">On the **Configure single sign-on at Central Desktop** page, to download your certificate, click **Download certificate**, and then save the certificate file on your computer.</span></span>
   
  <span data-ttu-id="2d835-145">![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="2d835-145">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="2d835-146">Logga in på ditt **centrala Desktop** klient.</span><span class="sxs-lookup"><span data-stu-id="2d835-146">Log in to your **Central Desktop** tenant.</span></span>
6. <span data-ttu-id="2d835-147">Gå till **inställningar**, klickar du på **Avancerat**, och klicka sedan på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2d835-147">Go to **Settings**, click **Advanced**, and then click **Single Sign On**.</span></span>
   
  <span data-ttu-id="2d835-148">![-Installation - avancerade](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "-installation - avancerade")</span><span class="sxs-lookup"><span data-stu-id="2d835-148">![Setup - Advanced](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - Advanced")</span></span>
7. <span data-ttu-id="2d835-149">På den **enkel inloggning på inställningar** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2d835-149">On the **Single Sign On Settings** page, perform the following steps:</span></span>
   
  <span data-ttu-id="2d835-150">![Enkel inloggning på inställningar](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "enkel inloggning på inställningar")</span><span class="sxs-lookup"><span data-stu-id="2d835-150">![Single Sign On Settings](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Single Sign On Settings")</span></span>
   
  1. <span data-ttu-id="2d835-151">Välj **aktivera SAML v2 för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2d835-151">Select **Enable SAML v2 Single Sign On**.</span></span>
  2. <span data-ttu-id="2d835-152">I den klassiska Azure-portalen på den **Konfigurera enkel inloggning på centrala Desktop** sidan, kopiera den **utfärdar-URL** värdet och klistrar in det i den **SSO URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="2d835-152">In the Azure classic portal, on the **Configure single sign-on at Central Desktop** page, copy the **Issuer URL** value, and then paste it into the **SSO URL** textbox.</span></span>
  3. <span data-ttu-id="2d835-153">I den klassiska Azure-portalen på den **Konfigurera enkel inloggning på centrala Desktop** sidan, kopiera den **Remote inloggnings-URL** värdet och klistrar in det i den **inloggnings-URL för SSO** textruta .</span><span class="sxs-lookup"><span data-stu-id="2d835-153">In the Azure classic portal, on the **Configure single sign-on at Central Desktop** page, copy the **Remote Login URL** value, and then paste it into the **SSO Login URL** textbox.</span></span>
  4. <span data-ttu-id="2d835-154">I den klassiska Azure-portalen på den **Konfigurera enkel inloggning på centrala Desktop** sidan, kopiera den **tjänst-URL för enkel Sign-Out** värdet och klistrar in det i den **SSO logga ut URL**textruta.</span><span class="sxs-lookup"><span data-stu-id="2d835-154">In the Azure classic portal, on the **Configure single sign-on at Central Desktop** page, copy the **Single Sign-Out Service URL** value, and then paste it into the **SSO Logout URL** textbox.</span></span>
8. <span data-ttu-id="2d835-155">I den **meddelandet signatur verifieringsmetod** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2d835-155">In the **Message Signature Verification Method** section, perform the following steps:</span></span>
   
   <span data-ttu-id="2d835-156">![Meddelandet signatur verifieringsmetod](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "meddelande signatur verifieringsmetod")</span><span class="sxs-lookup"><span data-stu-id="2d835-156">![Message Signature Verification Method](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Message Signature Verification Method")</span></span>
   
  1. <span data-ttu-id="2d835-157">Välj **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="2d835-157">Select **Certificate**.</span></span>
  2. <span data-ttu-id="2d835-158">Från den **SSO certifikat** väljer **RSH SHA256**.</span><span class="sxs-lookup"><span data-stu-id="2d835-158">From the **SSO Certificate** list, select **RSH SHA256**.</span></span>
  3. <span data-ttu-id="2d835-159">Skapa en textfil från hämtade certifikatet, kopiera innehållet i filen och klistrar in det i den **SSO certifikat** fältet.</span><span class="sxs-lookup"><span data-stu-id="2d835-159">Create a text file from the downloaded certificate, copy the content of the text file, and then paste it into the **SSO Certificate** field.</span></span>  
     >[!TIP]
     ><span data-ttu-id="2d835-160">Mer information finns i [hur du konverterar ett binärt certifikat till en textfil](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="2d835-160">For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
      >  
   4. <span data-ttu-id="2d835-161">Välj **visas en länk till inloggningssidan för SAMLv2**.</span><span class="sxs-lookup"><span data-stu-id="2d835-161">Select **Display a link to your SAMLv2 login page**.</span></span>
9. <span data-ttu-id="2d835-162">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="2d835-162">Click **Update**.</span></span>
10. <span data-ttu-id="2d835-163">Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **Slutför** att stänga den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2d835-163">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="2d835-164">![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="2d835-164">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Configure single sign-on")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="2d835-165">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="2d835-165">Configure user provisioning</span></span>

<span data-ttu-id="2d835-166">För AAD-användare för att kunna logga in, måste de etableras till centrala Desktop-programmet.</span><span class="sxs-lookup"><span data-stu-id="2d835-166">For AAD users to be able to sign in, they must be provisioned to the Central Desktop application.</span></span> <span data-ttu-id="2d835-167">Det här avsnittet beskriver hur du skapar AAD användarkonton i centrala Desktop.</span><span class="sxs-lookup"><span data-stu-id="2d835-167">This section describes how to create AAD user accounts in Central Desktop.</span></span>

<span data-ttu-id="2d835-168">**Att etablera användarkonton till centrala skrivbordet:**</span><span class="sxs-lookup"><span data-stu-id="2d835-168">**To provision user accounts to Central Desktop:**</span></span>
1. <span data-ttu-id="2d835-169">Logga in på din centrala Desktop-klient.</span><span class="sxs-lookup"><span data-stu-id="2d835-169">Log in to your Central Desktop tenant.</span></span>
2. <span data-ttu-id="2d835-170">Gå till **personer \> inre medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="2d835-170">Go to **People \> Internal Members**.</span></span>
3. <span data-ttu-id="2d835-171">Klicka på **lägga till inre medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="2d835-171">Click **Add Internal Members**.</span></span>
   
  <span data-ttu-id="2d835-172">![Personer](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "personer")</span><span class="sxs-lookup"><span data-stu-id="2d835-172">![People](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "People")</span></span>
4. <span data-ttu-id="2d835-173">I den **e-postadressen för nya medlemmar** textruta skriver ett AAD-konto du vill etablera och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2d835-173">In the **Email Address of New Members** textbox, type an AAD account you want to provision, and then click **Next**.</span></span>
   
  <span data-ttu-id="2d835-174">![E-postadresser för nya medlemmar](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "e-postadresser för nya medlemmar")</span><span class="sxs-lookup"><span data-stu-id="2d835-174">![Email Addresses of New Members](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Email Addresses of New Members")</span></span>
5. <span data-ttu-id="2d835-175">Klicka på **lägga till interna medlem(mar)**.</span><span class="sxs-lookup"><span data-stu-id="2d835-175">Click **Add Internal member(s)**.</span></span>
   
  <span data-ttu-id="2d835-176">![Lägga till inre medlem](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "lägga till inre medlem")</span><span class="sxs-lookup"><span data-stu-id="2d835-176">![Add Internal Member](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Add Internal Member")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="2d835-177">Du har lagt till användare får ett e-postmeddelande som innehåller en bekräftelse-länk som de behöver klicka om du vill aktivera kontot.</span><span class="sxs-lookup"><span data-stu-id="2d835-177">The users you have added will receive an email that includes a confirmation link they need to click to activate the account.</span></span>
   > 

>[!NOTE]
><span data-ttu-id="2d835-178">Du kan använda något annat centrala Desktop användarens konto skapas verktyg eller API: er som tillhandahålls av centrala skrivbordet till etablera AAD-användarkonton</span><span class="sxs-lookup"><span data-stu-id="2d835-178">You can use any other Central Desktop user account creation tools or APIs provided by Central Desktop to provision AAD user accounts</span></span>
>  

## <a name="assign-users"></a><span data-ttu-id="2d835-179">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="2d835-179">Assign users</span></span>
<span data-ttu-id="2d835-180">Om du vill testa konfigurationen måste du bevilja Azure AD-användare som du vill tillåta med hjälp av ditt programåtkomst till den genom att tilldela dem.</span><span class="sxs-lookup"><span data-stu-id="2d835-180">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="2d835-181">**Utför följande steg för att tilldela användare till centrala skrivbordet:**</span><span class="sxs-lookup"><span data-stu-id="2d835-181">**To assign users to Central Desktop, perform the following steps:**</span></span>

1. <span data-ttu-id="2d835-182">Skapa ett testkonto i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2d835-182">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="2d835-183">På den **centrala Desktop** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="2d835-183">On the **Central Desktop** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="2d835-184">![Tilldela användare](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="2d835-184">![Assign users](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Assign users")</span></span>
3. <span data-ttu-id="2d835-185">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** att bekräfta din tilldelning.</span><span class="sxs-lookup"><span data-stu-id="2d835-185">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="2d835-186">![Ja](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="2d835-186">![Yes](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="2d835-187">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2d835-187">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="2d835-188">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2d835-188">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

