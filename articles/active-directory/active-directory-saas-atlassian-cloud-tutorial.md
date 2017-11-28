---
title: "Självstudier: Azure Active Directory-integrering med molntjänster Atlassian | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Atlassian moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: f679e8b3306bf0efb9373d8baa0cfe095b760aaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a><span data-ttu-id="d6d9c-103">Självstudier: Azure Active Directory-integrering med Atlassian moln</span><span class="sxs-lookup"><span data-stu-id="d6d9c-103">Tutorial: Azure Active Directory integration with Atlassian Cloud</span></span>

<span data-ttu-id="d6d9c-104">I kursen får du lära dig hur toointegrate Atlassian moln med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d6d9c-104">In this tutorial, you learn how toointegrate Atlassian Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d6d9c-105">Integrera Atlassian moln med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d6d9c-105">Integrating Atlassian Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d6d9c-106">Du kan styra i Azure AD som har åtkomst tooAtlassian moln</span><span class="sxs-lookup"><span data-stu-id="d6d9c-106">You can control in Azure AD who has access tooAtlassian Cloud</span></span>
- <span data-ttu-id="d6d9c-107">Du kan aktivera din användare tooautomatically get inloggade tooAtlassian molnet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d6d9c-107">You can enable your users tooautomatically get signed-on tooAtlassian Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d6d9c-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d6d9c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d6d9c-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d6d9c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6d9c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d6d9c-110">Prerequisites</span></span>

<span data-ttu-id="d6d9c-111">tooconfigure Azure AD-integrering med Atlassian molnet, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d6d9c-111">tooconfigure Azure AD integration with Atlassian Cloud, you need hello following items:</span></span>

- <span data-ttu-id="d6d9c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d6d9c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d6d9c-113">En Atlassian enkel inloggning aktiverad molnprenumeration</span><span class="sxs-lookup"><span data-stu-id="d6d9c-113">An Atlassian Cloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d6d9c-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d6d9c-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d6d9c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d6d9c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d6d9c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d6d9c-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d6d9c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d6d9c-118">Scenario description</span></span>
<span data-ttu-id="d6d9c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d6d9c-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d6d9c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d6d9c-121">Att lägga till Atlassian moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d6d9c-121">Adding Atlassian Cloud from hello gallery</span></span>
2. <span data-ttu-id="d6d9c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d6d9c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-atlassian-cloud-from-hello-gallery"></a><span data-ttu-id="d6d9c-123">Att lägga till Atlassian moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d6d9c-123">Adding Atlassian Cloud from hello gallery</span></span>
<span data-ttu-id="d6d9c-124">tooconfigure hello integrering av Atlassian moln i Azure AD, behöver du tooadd Atlassian moln hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-124">tooconfigure hello integration of Atlassian Cloud into Azure AD, you need tooadd Atlassian Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d6d9c-125">**tooadd Atlassian moln från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d6d9c-125">**tooadd Atlassian Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6d9c-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d6d9c-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d6d9c-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d6d9c-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d6d9c-133">Skriv i sökrutan hello **Atlassian moln**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-133">In hello search box, type **Atlassian Cloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_search.png)

5. <span data-ttu-id="d6d9c-135">Markera hello resultat på panelen **Atlassian moln**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-135">In hello results panel, select **Atlassian Cloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d6d9c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d6d9c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d6d9c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Atlassian molnet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-138">In this section, you configure and test Azure AD single sign-on with Atlassian Cloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d6d9c-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Atlassian molnet är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Atlassian Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="d6d9c-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Atlassian molnet toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-140">In other words, a link relationship between an Azure AD user and hello related user in Atlassian Cloud needs toobe established.</span></span>

<span data-ttu-id="d6d9c-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Atlassian molnet.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Atlassian Cloud.</span></span>

<span data-ttu-id="d6d9c-142">tooconfigure och testa Azure AD enkel inloggning med Atlassian molnet, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d6d9c-142">tooconfigure and test Azure AD single sign-on with Atlassian Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d6d9c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d6d9c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d6d9c-145">**[Skapa en testanvändare Atlassian moln](#creating-an-atlassian-cloud-test-user)**  -toohave en motsvarighet för Britta Simon i Atlassian moln som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-145">**[Creating an Atlassian Cloud test user](#creating-an-atlassian-cloud-test-user)** - toohave a counterpart of Britta Simon in Atlassian Cloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d6d9c-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d6d9c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d6d9c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d6d9c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d6d9c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Atlassian moln.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Atlassian Cloud application.</span></span>

<span data-ttu-id="d6d9c-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med Atlassian molnet:**</span><span class="sxs-lookup"><span data-stu-id="d6d9c-150">**tooconfigure Azure AD single sign-on with Atlassian Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6d9c-151">I hello Azure-portalen på hello **Atlassian moln** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-151">In hello Azure portal, on hello **Atlassian Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d6d9c-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. <span data-ttu-id="d6d9c-155">På hello **Atlassian moln domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="d6d9c-155">On hello **Atlassian Cloud Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)

    <span data-ttu-id="d6d9c-157">a.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-157">a.</span></span> <span data-ttu-id="d6d9c-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.atlassian.net/admin/saml/edit`</span><span class="sxs-lookup"><span data-stu-id="d6d9c-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.atlassian.net/admin/saml/edit`</span></span>

    <span data-ttu-id="d6d9c-159">b.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-159">b.</span></span> <span data-ttu-id="d6d9c-160">I hello **Reply URL** textruta Skriv en URL som:`https://id.atlassian.com/login/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="d6d9c-160">In hello **Reply URL** textbox, type a URL as: `https://id.atlassian.com/login/saml/acs`</span></span>

4. <span data-ttu-id="d6d9c-161">Kontrollera **visa avancerade inställningar för URL: en** och utför följande steg om du inte vill tooconfigure hello programmet hello **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="d6d9c-161">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    <span data-ttu-id="d6d9c-163">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.atlassian.net`</span><span class="sxs-lookup"><span data-stu-id="d6d9c-163">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instancename>.atlassian.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d6d9c-164">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-164">These values are not real.</span></span> <span data-ttu-id="d6d9c-165">Uppdatera dessa värden med hello faktiska identifierare och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-165">Update these values with hello actual Identifier and Sign-On URL.</span></span> <span data-ttu-id="d6d9c-166">Du kan hämta hello exakta värden från Atlassian Molnkonfigurationen för SAML-skärmen.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-166">You can get hello exact values from Atlassian Cloud SAML Configuration screen.</span></span>
 
5. <span data-ttu-id="d6d9c-167">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-167">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. <span data-ttu-id="d6d9c-169">På hello **Atlassian Molnkonfigurationen** klickar du på **konfigurera Atlassian moln** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-169">On hello **Atlassian Cloud Configuration** section, click **Configure Atlassian Cloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d6d9c-170">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d6d9c-170">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

7. <span data-ttu-id="d6d9c-172">tooget SSO konfigurerats för tillämpningsprogrammet inloggningen toohello Atlassian Portal med hello administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-172">tooget SSO configured for your application, login toohello Atlassian Portal using hello administrator rights.</span></span>

8. <span data-ttu-id="d6d9c-173">Klicka på under hello autentisering i hello vänster navigeringsfält **domäner**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-173">In hello Authentication section of hello left navigation click **Domains**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

    <span data-ttu-id="d6d9c-175">a.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-175">a.</span></span> <span data-ttu-id="d6d9c-176">Skriv ditt domännamn i hello textrutan och klicka på **Lägg till domän**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-176">In hello textbox, type your domain name, and then click **Add domain**.</span></span>
        
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_07.png)

    <span data-ttu-id="d6d9c-178">b.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-178">b.</span></span> <span data-ttu-id="d6d9c-179">tooverify hello domän, klickar du på **Kontrollera**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-179">tooverify hello domain, click **Verify**.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_08.png)

    <span data-ttu-id="d6d9c-181">c.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-181">c.</span></span> <span data-ttu-id="d6d9c-182">Hämta hello verifiering HTML-fil, ladda upp den toohello rotmappen för din domän webbplatsen och klicka sedan på **verifiera domän**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-182">Download hello domain verification html file, upload it toohello root folder of your domain's website, and then click **Verify domain**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_09.png)

    <span data-ttu-id="d6d9c-184">d.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-184">d.</span></span> <span data-ttu-id="d6d9c-185">När hello domän har verifierats hello värdet för hello **Status** fältet är **verifierad**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-185">Once hello domain is verified, hello value of hello **Status** field is **Verified**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_10.png)

9. <span data-ttu-id="d6d9c-187">I hello vänstra navigeringsfältet klickar du på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-187">In hello left navigation bar, click **SAML**.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

10. <span data-ttu-id="d6d9c-189">Skapa en SAML-konfiguration och Lägg till hello identitet Providerkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-189">Create a SAML Configuration and add hello Identity provider configuration.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    <span data-ttu-id="d6d9c-191">a.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-191">a.</span></span> <span data-ttu-id="d6d9c-192">I hello **identitetsleverantör enhets-ID** klistra in hello värdet för textrutan **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-192">In hello **Identity provider Entity ID** text box, paste hello value of  **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d6d9c-193">b.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-193">b.</span></span> <span data-ttu-id="d6d9c-194">I hello **identitetsleverantör SSO URL** klistra in hello värdet för textrutan **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-194">In hello **Identity provider SSO URL** text box, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d6d9c-195">c.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-195">c.</span></span> <span data-ttu-id="d6d9c-196">Öppna hello hämtat certifikat från Azure portal och kopiera hello värden utan hello Begin och avsluta rader och klistra in den i hello **offentliga X509 certifikat** rutan.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-196">Open hello downloaded certificate from Azure portal and copy hello values without hello Begin and End lines and paste it in hello **Public X509 certificate** box.</span></span>
    
    <span data-ttu-id="d6d9c-197">d.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-197">d.</span></span> <span data-ttu-id="d6d9c-198">Klicka på **spara konfigurationen** tooSave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-198">Click **Save Configuration**  tooSave hello settings.</span></span>
     
11. <span data-ttu-id="d6d9c-199">Uppdatera hello Azure AD-inställningar toomake till att du har installationen hello korrigera Identifierare.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-199">Update hello Azure AD settings toomake sure that you have setup hello correct Identifier URL.</span></span>
  
    <span data-ttu-id="d6d9c-200">a.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-200">a.</span></span> <span data-ttu-id="d6d9c-201">Kopiera hello **SP identitet ID** från hello SAML skärmen och klistra in den i Azure AD som hello **identifierare** värde.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-201">Copy hello **SP Identity ID** from hello SAML screen and paste it in Azure AD as hello **Identifier** value.</span></span>

    <span data-ttu-id="d6d9c-202">b.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-202">b.</span></span> <span data-ttu-id="d6d9c-203">Inloggning på URL: en är hello klient-URL för ditt Atlassian moln.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-203">Sign On URL is hello tenant URL of your Atlassian Cloud.</span></span>     

     ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)
    
12. <span data-ttu-id="d6d9c-205">I hello Azure-portalen klickar du på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-205">In hello Azure portal, Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> <span data-ttu-id="d6d9c-207">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="d6d9c-207">You can now read a concise version of these instructions inside hello [Azure  portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d6d9c-208">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-208">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d6d9c-209">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d6d9c-209">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d6d9c-210">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d6d9c-210">Creating an Azure AD test user</span></span>
<span data-ttu-id="d6d9c-211">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-211">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d6d9c-213">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d6d9c-213">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6d9c-214">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-214">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d6d9c-216">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-216">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d6d9c-218">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-218">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d6d9c-220">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d6d9c-220">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d6d9c-222">a.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-222">a.</span></span> <span data-ttu-id="d6d9c-223">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-223">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d6d9c-224">b.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-224">b.</span></span> <span data-ttu-id="d6d9c-225">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-225">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d6d9c-226">c.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-226">c.</span></span> <span data-ttu-id="d6d9c-227">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-227">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d6d9c-228">d.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-228">d.</span></span> <span data-ttu-id="d6d9c-229">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-229">Click **Create**.</span></span>
 
### <a name="creating-an-atlassian-cloud-test-user"></a><span data-ttu-id="d6d9c-230">Skapa en testanvändare Atlassian moln</span><span class="sxs-lookup"><span data-stu-id="d6d9c-230">Creating an Atlassian Cloud test user</span></span>

<span data-ttu-id="d6d9c-231">tooenable Azure AD-användare toolog i tooAtlassian moln, måste de etableras i Atlassian moln.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-231">tooenable Azure AD users toolog in tooAtlassian Cloud, they must be provisioned into Atlassian Cloud.</span></span>  
<span data-ttu-id="d6d9c-232">Vid Atlassian moln är att etablera en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-232">In case of Atlassian Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="d6d9c-233">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="d6d9c-233">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6d9c-234">I hello administrationsavsnittet för platsen, klickar du på hello **användare** knappen</span><span class="sxs-lookup"><span data-stu-id="d6d9c-234">In hello Site administration section, click hello **Users** button</span></span>

    ![Skapa Atlassian moln användare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. <span data-ttu-id="d6d9c-236">Klicka på hello **skapa användare** knappen toocreate en användare i hello Atlassian moln</span><span class="sxs-lookup"><span data-stu-id="d6d9c-236">Click hello **Create User** button toocreate a user in hello Atlassian Cloud</span></span>

    ![Skapa Atlassian moln användare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. <span data-ttu-id="d6d9c-238">Ange hello användare **e-postadress**, **användarnamn**, och **fullständiga namn** och tilldela hello programåtkomst.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-238">Enter hello user's **Email address**, **Username**, and **Full Name** and assign hello application access.</span></span> 

    ![Skapa Atlassian moln användare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. <span data-ttu-id="d6d9c-240">Klicka på **skapa användare** knappen, skickas hello inbjudan toohello användare och efter accepterar hello inbjudan hello användaren kommer att vara aktiv i hello system.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-240">Click **Create user** button, it will send hello email invitation toohello user and after accepting hello invitation hello user will be active in hello system.</span></span> 

>[!NOTE] 
><span data-ttu-id="d6d9c-241">Du kan också skapa grupp användare för hello genom att klicka på hello **Bulk skapa** knapp i hello avsnittet användare.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-241">You can also create hello bulk users by clicking hello **Bulk Create** button in hello Users section.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d6d9c-242">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="d6d9c-242">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d6d9c-243">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAtlassian moln.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-243">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAtlassian Cloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d6d9c-245">**tooassign Britta Simon tooAtlassian moln, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="d6d9c-245">**tooassign Britta Simon tooAtlassian Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6d9c-246">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-246">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d6d9c-248">Välj i listan med program hello **Atlassian moln**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-248">In hello applications list, select **Atlassian Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png) 

3. <span data-ttu-id="d6d9c-250">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-250">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d6d9c-252">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-252">Click **Add** button.</span></span> <span data-ttu-id="d6d9c-253">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-253">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d6d9c-255">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-255">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d6d9c-256">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-256">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d6d9c-257">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-257">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d6d9c-258">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d6d9c-258">Testing single sign-on</span></span>

<span data-ttu-id="d6d9c-259">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-259">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="d6d9c-260">När du klickar på hello Atlassian moln panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Atlassian molnapp.</span><span class="sxs-lookup"><span data-stu-id="d6d9c-260">When you click hello Atlassian Cloud tile in hello Access Panel, you should get automatically signed-on tooyour Atlassian Cloud application.</span></span> <span data-ttu-id="d6d9c-261">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d6d9c-261">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d6d9c-262">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d6d9c-262">Additional resources</span></span>

* [<span data-ttu-id="d6d9c-263">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d6d9c-263">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d6d9c-264">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d6d9c-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

