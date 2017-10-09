---
title: "Självstudier: Azure Active Directory-integrering med TINFOIL SECURITY | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TINFOIL SECURITY."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 1a3fa9880d9e026c2d6d6548188df2269ff69139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a><span data-ttu-id="33c86-103">Självstudier: Azure Active Directory-integrering med TINFOIL SECURITY</span><span class="sxs-lookup"><span data-stu-id="33c86-103">Tutorial: Azure Active Directory integration with TINFOIL SECURITY</span></span>

<span data-ttu-id="33c86-104">I kursen får du lära dig hur toointegrate TINFOIL SECURITY med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="33c86-104">In this tutorial, you learn how toointegrate TINFOIL SECURITY with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="33c86-105">Integrera TINFOIL SECURITY med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="33c86-105">Integrating TINFOIL SECURITY with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="33c86-106">Du kan styra i Azure AD som har åtkomst tooTINFOIL säkerhet</span><span class="sxs-lookup"><span data-stu-id="33c86-106">You can control in Azure AD who has access tooTINFOIL SECURITY</span></span>
- <span data-ttu-id="33c86-107">Du kan aktivera din användare tooautomatically get inloggade tooTINFOIL säkerhet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="33c86-107">You can enable your users tooautomatically get signed-on tooTINFOIL SECURITY (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="33c86-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="33c86-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="33c86-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="33c86-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33c86-110">Krav</span><span class="sxs-lookup"><span data-stu-id="33c86-110">Prerequisites</span></span>

<span data-ttu-id="33c86-111">tooconfigure Azure AD-integrering med TINFOIL SECURITY måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="33c86-111">tooconfigure Azure AD integration with TINFOIL SECURITY, you need hello following items:</span></span>

- <span data-ttu-id="33c86-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="33c86-112">An Azure AD subscription</span></span>
- <span data-ttu-id="33c86-113">En TINFOIL SECURITY enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="33c86-113">A TINFOIL SECURITY single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="33c86-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="33c86-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="33c86-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="33c86-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="33c86-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="33c86-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="33c86-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="33c86-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="33c86-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="33c86-118">Scenario description</span></span>
<span data-ttu-id="33c86-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="33c86-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="33c86-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="33c86-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="33c86-121">Lägg till TINFOIL SECURITY från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="33c86-121">Add TINFOIL SECURITY from hello gallery</span></span>
2. <span data-ttu-id="33c86-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33c86-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tinfoil-security-from-hello-gallery"></a><span data-ttu-id="33c86-123">Lägg till TINFOIL SECURITY från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="33c86-123">Add TINFOIL SECURITY from hello gallery</span></span>
<span data-ttu-id="33c86-124">tooconfigure hello integrering av TINFOIL SECURITY i Azure AD, behöver du tooadd TINFOIL SECURITY hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="33c86-124">tooconfigure hello integration of TINFOIL SECURITY into Azure AD, you need tooadd TINFOIL SECURITY from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="33c86-125">**tooadd TINFOIL SECURITY från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="33c86-125">**tooadd TINFOIL SECURITY from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="33c86-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="33c86-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="33c86-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="33c86-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="33c86-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="33c86-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="33c86-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33c86-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="33c86-133">Skriv i sökrutan hello **TINFOIL SECURITY**väljer **TINFOIL SECURITY** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="33c86-133">In hello search box, type **TINFOIL SECURITY**, select  **TINFOIL SECURITY** from result panel then click **Add** button tooadd hello application.</span></span>

    ![TINFOIL SECURITY från galleriet](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="33c86-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33c86-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="33c86-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TINFOIL SECURITY baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="33c86-136">In this section, you configure and test Azure AD single sign-on with TINFOIL SECURITY based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="33c86-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i TINFOIL SECURITY är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="33c86-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TINFOIL SECURITY is tooa user in Azure AD.</span></span> <span data-ttu-id="33c86-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i TINFOIL SECURITY toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="33c86-138">In other words, a link relationship between an Azure AD user and hello related user in TINFOIL SECURITY needs toobe established.</span></span>

<span data-ttu-id="33c86-139">Tilldela hello värdet för hello i TINFOIL SECURITY **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="33c86-139">In TINFOIL SECURITY, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="33c86-140">tooconfigure och testa Azure AD enkel inloggning med TINFOIL SECURITY, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="33c86-140">tooconfigure and test Azure AD single sign-on with TINFOIL SECURITY, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="33c86-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="33c86-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="33c86-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="33c86-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="33c86-143">**[Skapa en testanvändare TINFOIL SECURITY](#create-a-tinfoil-security-test-user)**  -toohave en motsvarighet för Britta Simon i TINFOIL SECURITY som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="33c86-143">**[Create a TINFOIL SECURITY test user](#create-a-tinfoil-security-test-user)** - toohave a counterpart of Britta Simon in TINFOIL SECURITY that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="33c86-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="33c86-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="33c86-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="33c86-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="33c86-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33c86-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="33c86-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt TINFOIL SECURITY-program.</span><span class="sxs-lookup"><span data-stu-id="33c86-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TINFOIL SECURITY application.</span></span>

<span data-ttu-id="33c86-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med TINFOIL SECURITY:**</span><span class="sxs-lookup"><span data-stu-id="33c86-148">**tooconfigure Azure AD single sign-on with TINFOIL SECURITY, perform hello following steps:**</span></span>

1. <span data-ttu-id="33c86-149">I hello Azure-portalen på hello **TINFOIL SECURITY** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="33c86-149">In hello Azure portal, on hello **TINFOIL SECURITY** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="33c86-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="33c86-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![SAML-baserade inloggning](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

3. <span data-ttu-id="33c86-153">På hello **TINFOIL SECURITY domän och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.</span><span class="sxs-lookup"><span data-stu-id="33c86-153">On hello **TINFOIL SECURITY Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


4. <span data-ttu-id="33c86-155">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värde.</span><span class="sxs-lookup"><span data-stu-id="33c86-155">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value.</span></span>

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

5. <span data-ttu-id="33c86-157">mappningar av tooadd hello krävs, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="33c86-157">tooadd hello required attribute mappings, perform hello following steps:</span></span>
    
    <span data-ttu-id="33c86-158">![Attribut](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="33c86-158">![Attributes](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "Attributes")</span></span>
    
    | <span data-ttu-id="33c86-159">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="33c86-159">Attribute Name</span></span>    |   <span data-ttu-id="33c86-160">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="33c86-160">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="33c86-161">accountid</span><span class="sxs-lookup"><span data-stu-id="33c86-161">accountid</span></span> | <span data-ttu-id="33c86-162">UXXXXXXXXXXXXX</span><span class="sxs-lookup"><span data-stu-id="33c86-162">UXXXXXXXXXXXXX</span></span> |
    
    <span data-ttu-id="33c86-163">a.</span><span class="sxs-lookup"><span data-stu-id="33c86-163">a.</span></span> <span data-ttu-id="33c86-164">Klicka på **lägga till användarattribut**.</span><span class="sxs-lookup"><span data-stu-id="33c86-164">Click **add user attribute**.</span></span>
    
    <span data-ttu-id="33c86-165">![Lägg till attributet](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="33c86-165">![ADD Attribute](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "Attributes")</span></span>
    
    <span data-ttu-id="33c86-166">![Lägg till attributet](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="33c86-166">![ADD Attribute](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "Attributes")</span></span>
    
    <span data-ttu-id="33c86-167">b.</span><span class="sxs-lookup"><span data-stu-id="33c86-167">b.</span></span> <span data-ttu-id="33c86-168">I hello **attributnamn** textruta typen **accountid**.</span><span class="sxs-lookup"><span data-stu-id="33c86-168">In hello **Attribute Name** textbox, type **accountid**.</span></span>
    
    <span data-ttu-id="33c86-169">c.</span><span class="sxs-lookup"><span data-stu-id="33c86-169">c.</span></span> <span data-ttu-id="33c86-170">I hello **attributvärdet** textruta klistra in hello konto-ID-värde som visas vid ett senare tillfälle hello kursen.</span><span class="sxs-lookup"><span data-stu-id="33c86-170">In hello **Attribute Value** textbox, paste hello account ID value which you will get later on hello tutorial.</span></span>
    
    <span data-ttu-id="33c86-171">d.</span><span class="sxs-lookup"><span data-stu-id="33c86-171">d.</span></span> <span data-ttu-id="33c86-172">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="33c86-172">Click **Ok**.</span></span>    

6. <span data-ttu-id="33c86-173">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="33c86-173">Click **Save** button.</span></span>

    ![Knappen Spara](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="33c86-175">På hello **TINFOIL SECURITY Configuration** klickar du på **konfigurera TINFOIL SECURITY** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="33c86-175">On hello **TINFOIL SECURITY Configuration** section, click **Configure TINFOIL SECURITY** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="33c86-176">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="33c86-176">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![TINFOIL SECURITY Configuration](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

8. <span data-ttu-id="33c86-178">Logga in på webbplatsen TINFOIL SECURITY företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="33c86-178">In a different web browser window, log into your TINFOIL SECURITY company site as an administrator.</span></span>

9. <span data-ttu-id="33c86-179">Klicka i hello verktygsfältet hello längst upp **mitt konto**.</span><span class="sxs-lookup"><span data-stu-id="33c86-179">In hello toolbar on hello top, click **My Account**.</span></span>
   
    <span data-ttu-id="33c86-180">![Instrumentpanelen](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "instrumentpanelen")</span><span class="sxs-lookup"><span data-stu-id="33c86-180">![Dashboard](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "Dashboard")</span></span>

10. <span data-ttu-id="33c86-181">Klicka på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="33c86-181">Click **Security**.</span></span>
   
    <span data-ttu-id="33c86-182">![Säkerhet](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="33c86-182">![Security](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "Security")</span></span>

11. <span data-ttu-id="33c86-183">På hello **enkel inloggning** konfigurationen utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="33c86-183">On hello **Single Sign-On** configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="33c86-184">![Enkel inloggning](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="33c86-184">![Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "Single Sign-On")</span></span>
   
    <span data-ttu-id="33c86-185">a.</span><span class="sxs-lookup"><span data-stu-id="33c86-185">a.</span></span> <span data-ttu-id="33c86-186">Välj **aktivera SAML**.</span><span class="sxs-lookup"><span data-stu-id="33c86-186">Select **Enable SAML**.</span></span>
   
    <span data-ttu-id="33c86-187">b.</span><span class="sxs-lookup"><span data-stu-id="33c86-187">b.</span></span> <span data-ttu-id="33c86-188">Klicka på **manuell konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="33c86-188">Click **Manual Configuration**.</span></span>
   
    <span data-ttu-id="33c86-189">c.</span><span class="sxs-lookup"><span data-stu-id="33c86-189">c.</span></span> <span data-ttu-id="33c86-190">I **SAML Post URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="33c86-190">In **SAML Post URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal</span></span>
   
    <span data-ttu-id="33c86-191">d.</span><span class="sxs-lookup"><span data-stu-id="33c86-191">d.</span></span> <span data-ttu-id="33c86-192">I **SAML certifikat fingeravtryck** textruta klistra in hello värdet för **tumavtrycket** som du har kopierat från **SAML-signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="33c86-192">In **SAML Certificate Fingerprint** textbox, paste hello value of **Thumbprint** which you have copied from **SAML Signing Certificate** section.</span></span>
  
    <span data-ttu-id="33c86-193">e.</span><span class="sxs-lookup"><span data-stu-id="33c86-193">e.</span></span> <span data-ttu-id="33c86-194">Kopiera **ditt konto-ID** värdet och klistrar in hello värdet i **attributvärdet** textruta under **lägga till attributet** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="33c86-194">Copy **Your Account ID** value and paste hello value in **Attribute Value** textbox under **Add Attribute** section in Azure portal.</span></span>
   
    <span data-ttu-id="33c86-195">f.</span><span class="sxs-lookup"><span data-stu-id="33c86-195">f.</span></span> <span data-ttu-id="33c86-196">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="33c86-196">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="33c86-197">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="33c86-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="33c86-198">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="33c86-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="33c86-199">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="33c86-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="33c86-200">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="33c86-200">Create an Azure AD test user</span></span>
<span data-ttu-id="33c86-201">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="33c86-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="33c86-203">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="33c86-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="33c86-204">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="33c86-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="33c86-206">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="33c86-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![<span data-ttu-id="33c86-207">Användare och grupper -> alla användare</span><span class="sxs-lookup"><span data-stu-id="33c86-207">Users and groups -> All users</span></span> ](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="33c86-208">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33c86-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Användare](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="33c86-210">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="33c86-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="33c86-212">a.</span><span class="sxs-lookup"><span data-stu-id="33c86-212">a.</span></span> <span data-ttu-id="33c86-213">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="33c86-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="33c86-214">b.</span><span class="sxs-lookup"><span data-stu-id="33c86-214">b.</span></span> <span data-ttu-id="33c86-215">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="33c86-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="33c86-216">c.</span><span class="sxs-lookup"><span data-stu-id="33c86-216">c.</span></span> <span data-ttu-id="33c86-217">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="33c86-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="33c86-218">d.</span><span class="sxs-lookup"><span data-stu-id="33c86-218">d.</span></span> <span data-ttu-id="33c86-219">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="33c86-219">Click **Create**.</span></span>
 
### <a name="create-a-tinfoil-security-test-user"></a><span data-ttu-id="33c86-220">Skapa en testanvändare TINFOIL SECURITY</span><span class="sxs-lookup"><span data-stu-id="33c86-220">Create a TINFOIL SECURITY test user</span></span>

<span data-ttu-id="33c86-221">I ordning tooenable Azure AD-användare toolog till TINFOIL SECURITY, måste de etableras i TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="33c86-221">In order tooenable Azure AD users toolog into TINFOIL SECURITY, they must be provisioned into TINFOIL SECURITY.</span></span> <span data-ttu-id="33c86-222">TINFOIL SECURITY hello gäller är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="33c86-222">In hello case of TINFOIL SECURITY, provisioning is a manual task.</span></span>

<span data-ttu-id="33c86-223">**tooget en användare som har etablerats, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="33c86-223">**tooget a user provisioned, perform hello following steps:**</span></span>

1. <span data-ttu-id="33c86-224">Om hello användaren är en del av ett Enterprise-konto, behöver du för[Kontakta supportteamet för hello TINFOIL SECURITY](https://www.tinfoilsecurity.com/contact) tooget hello användarkonton som skapas.</span><span class="sxs-lookup"><span data-stu-id="33c86-224">If hello user is a part of an Enterprise account, you need too[contact hello TINFOIL SECURITY support team](https://www.tinfoilsecurity.com/contact) tooget hello user account created.</span></span>

2. <span data-ttu-id="33c86-225">Om hello användare är en vanlig TINFOIL SECURITY SaaS-användare, sedan hello användare kan lägga till en deltagare tooany hello användarens webbplatser.</span><span class="sxs-lookup"><span data-stu-id="33c86-225">If hello user is a regular TINFOIL SECURITY SaaS user, then hello user can add a collaborator tooany of hello user’s sites.</span></span> <span data-ttu-id="33c86-226">Den här utlösare en process toosend en inbjudan toohello angivna e-toocreate ett nytt användarkonto TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="33c86-226">This triggers a process toosend an invitation toohello specified email toocreate a new TINFOIL SECURITY user account.</span></span>

> [!NOTE]
> <span data-ttu-id="33c86-227">Du kan använda något annat TINFOIL SECURITY användare konto skapas verktyg eller API: er som tillhandahålls av TINFOIL SECURITY tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="33c86-227">You can use any other TINFOIL SECURITY user account creation tools or APIs provided by TINFOIL SECURITY tooprovision Azure AD user accounts.</span></span>
> 
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="33c86-228">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="33c86-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="33c86-229">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTINFOIL säkerhet.</span><span class="sxs-lookup"><span data-stu-id="33c86-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTINFOIL SECURITY.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="33c86-231">**tooassign Britta Simon tooTINFOIL säkerhet, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="33c86-231">**tooassign Britta Simon tooTINFOIL SECURITY, perform hello following steps:**</span></span>

1. <span data-ttu-id="33c86-232">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="33c86-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="33c86-234">Välj i listan med program hello **TINFOIL SECURITY**.</span><span class="sxs-lookup"><span data-stu-id="33c86-234">In hello applications list, select **TINFOIL SECURITY**.</span></span>

    ![Välj TINFOIL SECURITY](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

3. <span data-ttu-id="33c86-236">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="33c86-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="33c86-238">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="33c86-238">Click **Add** button.</span></span> <span data-ttu-id="33c86-239">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33c86-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="33c86-241">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="33c86-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="33c86-242">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33c86-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="33c86-243">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33c86-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="33c86-244">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33c86-244">Test single sign-on</span></span>

<span data-ttu-id="33c86-245">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="33c86-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="33c86-246">Du bör få automatiskt inloggade tooyour TINFOIL SECURITY programmet när du klickar på hello TINFOIL SECURITY-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="33c86-246">When you click hello TINFOIL SECURITY tile in hello Access Panel, you should get automatically signed-on tooyour TINFOIL SECURITY application.</span></span> <span data-ttu-id="33c86-247">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="33c86-247">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="33c86-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="33c86-248">Additional resources</span></span>

* [<span data-ttu-id="33c86-249">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="33c86-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="33c86-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="33c86-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_203.png

