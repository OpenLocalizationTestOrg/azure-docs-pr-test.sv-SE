---
title: "Självstudier: Azure Active Directory-integrering med Central Desktop | Microsoft Docs"
description: "Lär dig hur toouse centrala skrivbordet med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
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
ms.openlocfilehash: 93036ae801c446ce476288c00579931ba10a843b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a><span data-ttu-id="affad-103">Självstudier: Azure Active Directory-integrering med Central Desktop</span><span class="sxs-lookup"><span data-stu-id="affad-103">Tutorial: Azure Active Directory integration with Central Desktop</span></span>
<span data-ttu-id="affad-104">hello syftet med den här kursen är tooshow hello integreringen av Azure och centrala skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="affad-104">hello objective of this tutorial is tooshow hello integration of Azure and Central Desktop.</span></span> <span data-ttu-id="affad-105">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="affad-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="affad-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="affad-106">A valid Azure subscription</span></span>
* <span data-ttu-id="affad-107">En Central skrivbordet för enkel inloggning aktiverad prenumeration / centrala desktop-klient</span><span class="sxs-lookup"><span data-stu-id="affad-107">A Central desktop single sign on enabled subscription / Central desktop tenant</span></span>

<span data-ttu-id="affad-108">hello-scenario som beskrivs i den här kursen består av följande byggblock hello:</span><span class="sxs-lookup"><span data-stu-id="affad-108">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

* <span data-ttu-id="affad-109">Aktivera hello programintegrationstyp för Central skrivbord</span><span class="sxs-lookup"><span data-stu-id="affad-109">Enabling hello application integration for Central Desktop</span></span>
* <span data-ttu-id="affad-110">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="affad-110">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="affad-111">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="affad-111">Configuring user provisioning</span></span>
* <span data-ttu-id="affad-112">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="affad-112">Assigning users</span></span>

<span data-ttu-id="affad-113">![Scenariot](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="affad-113">![Scenario](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-central-desktop"></a><span data-ttu-id="affad-114">Aktivera hello programintegrationstyp för Central skrivbord</span><span class="sxs-lookup"><span data-stu-id="affad-114">Enable hello application integration for Central Desktop</span></span>
<span data-ttu-id="affad-115">hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Central skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="affad-115">hello objective of this section is toooutline how tooenable hello application integration for Central Desktop.</span></span>

<span data-ttu-id="affad-116">**tooenable hello programintegrationstyp för Central skrivbordet utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="affad-116">**tooenable hello application integration for Central Desktop, perform hello following steps:**</span></span>

1. <span data-ttu-id="affad-117">I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="affad-117">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="affad-118">![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="affad-118">![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="affad-119">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="affad-119">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="affad-120">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="affad-120">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="affad-121">![Program](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="affad-121">![Applications](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="affad-122">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="affad-122">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="affad-123">![Lägg till program](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="affad-123">![Add application](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="affad-124">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="affad-124">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="affad-125">![Lägga till ett program från gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="affad-125">![Add an application from gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="affad-126">I hello **sökrutan**, typen **centrala Desktop**.</span><span class="sxs-lookup"><span data-stu-id="affad-126">In hello **search box**, type **Central Desktop**.</span></span>
   
   <span data-ttu-id="affad-127">![Programgalleriet](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="affad-127">![Application gallery](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Application gallery")</span></span>
7. <span data-ttu-id="affad-128">I resultatfönstret hello väljer **centrala Desktop**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="affad-128">In hello results pane, select **Central Desktop**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="affad-129">![Central Desktop](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "centrala Desktop")</span><span class="sxs-lookup"><span data-stu-id="affad-129">![Central Desktop](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Central Desktop")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="affad-130">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="affad-130">Configure single sign-on</span></span>

<span data-ttu-id="affad-131">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooCentral skrivbordet med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="affad-131">hello objective of this section is toooutline how tooenable users tooauthenticate tooCentral Desktop with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="affad-132">Som en del av den här proceduren är obligatoriska tooupload en Base64-kodade certifikatet tooyour centrala Desktop-klient.</span><span class="sxs-lookup"><span data-stu-id="affad-132">As part of this procedure, you are required tooupload a base-64 encoded certificate tooyour Central Desktop tenant.</span></span>  
<span data-ttu-id="affad-133">Om du inte är bekant med den här proceduren, se [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="affad-133">If you are not familiar with this procedure, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>

<span data-ttu-id="affad-134">**tooconfigure enkel inloggning, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="affad-134">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="affad-135">I hello klassiska Azure-portalen på hello **centrala Desktop** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello ** Konfigurera enkel inloggning ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="affad-135">In hello Azure classic portal, on hello **Central Desktop** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On ** dialog.</span></span>
   
   <span data-ttu-id="affad-136">![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="affad-136">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="affad-137">På hello **hur skulle du som användare toosign på tooCentral Desktop** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="affad-137">On hello **How would you like users toosign on tooCentral Desktop** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="affad-138">![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="affad-138">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="affad-139">På hello **konfigurera App-URL** , utför följande steg hello, och klickar sedan på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="affad-139">On hello **Configure App URL** page, perform hello following steps, and then click **Next**:</span></span> 
   
   1. <span data-ttu-id="affad-140">I hello **Desktop tecken i URL: en Central** textruta typen hello URL för din centrala Desktop-klient (t.ex.: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="affad-140">In hello **Central Desktop Sign In URL** textbox, type hello URL of your Central Desktop tenant (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   2. <span data-ttu-id="affad-141">Ange din centrala Desktop AssertionConsumerService URL i hello centrala Desktop Reply URL textruta (t.ex.: https://contoso.centraldesktop.com/saml2-assertion.php).</span><span class="sxs-lookup"><span data-stu-id="affad-141">In hello Central  Desktop Reply URL textbox, type your Central Desktop AssertionConsumerService URL (e.g.:  https://contoso.centraldesktop.com/saml2-assertion.php).</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="affad-142">Du kan hämta hello värdet från hello centrala skrivbord metadata (t.ex.: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="affad-142">You can get hello value from hello central desktop metadata (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   >  
   
   <span data-ttu-id="affad-143">![Konfigurera app-URL](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="affad-143">![Configure app URL](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Configure app URL")</span></span>
4. <span data-ttu-id="affad-144">På hello **Konfigurera enkel inloggning på centrala Desktop** sida, toodownload ditt certifikat klickar du på **hämta certifikat**, och sedan spara hello certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="affad-144">On hello **Configure single sign-on at Central Desktop** page, toodownload your certificate, click **Download certificate**, and then save hello certificate file on your computer.</span></span>
   
  <span data-ttu-id="affad-145">![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="affad-145">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="affad-146">Logga in tooyour **centrala Desktop** klient.</span><span class="sxs-lookup"><span data-stu-id="affad-146">Log in tooyour **Central Desktop** tenant.</span></span>
6. <span data-ttu-id="affad-147">Gå för**inställningar**, klickar du på **Avancerat**, och klicka sedan på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="affad-147">Go too**Settings**, click **Advanced**, and then click **Single Sign On**.</span></span>
   
  <span data-ttu-id="affad-148">![-Installation - avancerade](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "-installation - avancerade")</span><span class="sxs-lookup"><span data-stu-id="affad-148">![Setup - Advanced](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - Advanced")</span></span>
7. <span data-ttu-id="affad-149">På hello **enkel inloggning på inställningar** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="affad-149">On hello **Single Sign On Settings** page, perform hello following steps:</span></span>
   
  <span data-ttu-id="affad-150">![Enkel inloggning på inställningar](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "enkel inloggning på inställningar")</span><span class="sxs-lookup"><span data-stu-id="affad-150">![Single Sign On Settings](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Single Sign On Settings")</span></span>
   
  1. <span data-ttu-id="affad-151">Välj **aktivera SAML v2 för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="affad-151">Select **Enable SAML v2 Single Sign On**.</span></span>
  2. <span data-ttu-id="affad-152">I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på centrala Desktop** sidan, kopiera hello **utfärdar-URL** värdet och klistrar in den i hello **SSO URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="affad-152">In hello Azure classic portal, on hello **Configure single sign-on at Central Desktop** page, copy hello **Issuer URL** value, and then paste it into hello **SSO URL** textbox.</span></span>
  3. <span data-ttu-id="affad-153">I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på centrala Desktop** sidan, kopiera hello **Remote inloggnings-URL** värdet och klistrar in den i hello **SSO inloggnings-URL**textruta.</span><span class="sxs-lookup"><span data-stu-id="affad-153">In hello Azure classic portal, on hello **Configure single sign-on at Central Desktop** page, copy hello **Remote Login URL** value, and then paste it into hello **SSO Login URL** textbox.</span></span>
  4. <span data-ttu-id="affad-154">I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på centrala Desktop** sidan, kopiera hello **tjänst-URL för enkel Sign-Out** värdet och klistrar in den i hello **SSO logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="affad-154">In hello Azure classic portal, on hello **Configure single sign-on at Central Desktop** page, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **SSO Logout URL** textbox.</span></span>
8. <span data-ttu-id="affad-155">I hello **meddelandet signatur verifieringsmetod** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="affad-155">In hello **Message Signature Verification Method** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="affad-156">![Meddelandet signatur verifieringsmetod](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "meddelande signatur verifieringsmetod")</span><span class="sxs-lookup"><span data-stu-id="affad-156">![Message Signature Verification Method](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Message Signature Verification Method")</span></span>
   
  1. <span data-ttu-id="affad-157">Välj **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="affad-157">Select **Certificate**.</span></span>
  2. <span data-ttu-id="affad-158">Från hello **SSO certifikat** väljer **RSH SHA256**.</span><span class="sxs-lookup"><span data-stu-id="affad-158">From hello **SSO Certificate** list, select **RSH SHA256**.</span></span>
  3. <span data-ttu-id="affad-159">Skapa en textfil från hello hämtat certifikat, kopiera hello innehåll på hello textfil och klistra in den i hello **SSO certifikat** fältet.</span><span class="sxs-lookup"><span data-stu-id="affad-159">Create a text file from hello downloaded certificate, copy hello content of hello text file, and then paste it into hello **SSO Certificate** field.</span></span>  
     >[!TIP]
     ><span data-ttu-id="affad-160">Mer information finns i [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="affad-160">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
      >  
   4. <span data-ttu-id="affad-161">Välj **visas en länk tooyour SAMLv2 inloggningssida**.</span><span class="sxs-lookup"><span data-stu-id="affad-161">Select **Display a link tooyour SAMLv2 login page**.</span></span>
9. <span data-ttu-id="affad-162">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="affad-162">Click **Update**.</span></span>
10. <span data-ttu-id="affad-163">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="affad-163">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="affad-164">![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="affad-164">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Configure single sign-on")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="affad-165">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="affad-165">Configure user provisioning</span></span>

<span data-ttu-id="affad-166">De måste vara etablerade toohello centrala Desktop-programmet för AAD användare toobe kan toosign i.</span><span class="sxs-lookup"><span data-stu-id="affad-166">For AAD users toobe able toosign in, they must be provisioned toohello Central Desktop application.</span></span> <span data-ttu-id="affad-167">Det här avsnittet beskrivs hur toocreate AAD användarkonton i centrala Desktop.</span><span class="sxs-lookup"><span data-stu-id="affad-167">This section describes how toocreate AAD user accounts in Central Desktop.</span></span>

<span data-ttu-id="affad-168">**tooprovision användaren konton tooCentral skrivbordet:**</span><span class="sxs-lookup"><span data-stu-id="affad-168">**tooprovision user accounts tooCentral Desktop:**</span></span>
1. <span data-ttu-id="affad-169">Logga in tooyour centrala Desktop-klient.</span><span class="sxs-lookup"><span data-stu-id="affad-169">Log in tooyour Central Desktop tenant.</span></span>
2. <span data-ttu-id="affad-170">Gå för**personer \> inre medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="affad-170">Go too**People \> Internal Members**.</span></span>
3. <span data-ttu-id="affad-171">Klicka på **lägga till inre medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="affad-171">Click **Add Internal Members**.</span></span>
   
  <span data-ttu-id="affad-172">![Personer](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "personer")</span><span class="sxs-lookup"><span data-stu-id="affad-172">![People](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "People")</span></span>
4. <span data-ttu-id="affad-173">I hello **e-postadressen för nya medlemmar** textruta skriver ett AAD-konto du vill tooprovision och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="affad-173">In hello **Email Address of New Members** textbox, type an AAD account you want tooprovision, and then click **Next**.</span></span>
   
  <span data-ttu-id="affad-174">![E-postadresser för nya medlemmar](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "e-postadresser för nya medlemmar")</span><span class="sxs-lookup"><span data-stu-id="affad-174">![Email Addresses of New Members](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Email Addresses of New Members")</span></span>
5. <span data-ttu-id="affad-175">Klicka på **lägga till interna medlem(mar)**.</span><span class="sxs-lookup"><span data-stu-id="affad-175">Click **Add Internal member(s)**.</span></span>
   
  <span data-ttu-id="affad-176">![Lägga till inre medlem](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "lägga till inre medlem")</span><span class="sxs-lookup"><span data-stu-id="affad-176">![Add Internal Member](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Add Internal Member")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="affad-177">hello-användare som du har lagt till får ett e-postmeddelande som innehåller en bekräftelse-länk som de behöver tooclick tooactivate hello-konto.</span><span class="sxs-lookup"><span data-stu-id="affad-177">hello users you have added will receive an email that includes a confirmation link they need tooclick tooactivate hello account.</span></span>
   > 

>[!NOTE]
><span data-ttu-id="affad-178">Du kan använda något annat centrala Desktop användarens konto skapas verktyg eller API: er som tillhandahålls av centrala Desktop tooprovision AAD-användarkonton</span><span class="sxs-lookup"><span data-stu-id="affad-178">You can use any other Central Desktop user account creation tools or APIs provided by Central Desktop tooprovision AAD user accounts</span></span>
>  

## <a name="assign-users"></a><span data-ttu-id="affad-179">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="affad-179">Assign users</span></span>
<span data-ttu-id="affad-180">tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.</span><span class="sxs-lookup"><span data-stu-id="affad-180">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="affad-181">**tooassign användare tooCentral skrivbordet utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="affad-181">**tooassign users tooCentral Desktop, perform hello following steps:**</span></span>

1. <span data-ttu-id="affad-182">Skapa ett testkonto i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="affad-182">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="affad-183">På hello **centrala Desktop** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="affad-183">On hello **Central Desktop** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="affad-184">![Tilldela användare](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="affad-184">![Assign users](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Assign users")</span></span>
3. <span data-ttu-id="affad-185">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="affad-185">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="affad-186">![Ja](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="affad-186">![Yes](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="affad-187">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="affad-187">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="affad-188">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="affad-188">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

