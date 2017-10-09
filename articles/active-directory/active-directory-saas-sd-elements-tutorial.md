---
title: "Självstudier: Azure Active Directory-integrering med SD element | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SD-element."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 77949e41beb541c9fe8147b1eb2e7995e05bd753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a><span data-ttu-id="2df37-103">Självstudier: Azure Active Directory-integrering med SD-element</span><span class="sxs-lookup"><span data-stu-id="2df37-103">Tutorial: Azure Active Directory integration with SD Elements</span></span>

<span data-ttu-id="2df37-104">I kursen får du lära dig hur toointegrate SD-element med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2df37-104">In this tutorial, you learn how toointegrate SD Elements with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2df37-105">Integrera SD-element med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2df37-105">Integrating SD Elements with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2df37-106">Du kan styra i Azure AD som har åtkomst tooSD element</span><span class="sxs-lookup"><span data-stu-id="2df37-106">You can control in Azure AD who has access tooSD Elements</span></span>
- <span data-ttu-id="2df37-107">Du kan aktivera din användare tooautomatically get inloggade tooSD element (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2df37-107">You can enable your users tooautomatically get signed-on tooSD Elements (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2df37-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2df37-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2df37-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2df37-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2df37-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2df37-110">Prerequisites</span></span>

<span data-ttu-id="2df37-111">tooconfigure Azure AD-integrering med SD-element måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2df37-111">tooconfigure Azure AD integration with SD Elements, you need hello following items:</span></span>

- <span data-ttu-id="2df37-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2df37-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2df37-113">Ett SD-element enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="2df37-113">A SD Elements single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2df37-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2df37-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2df37-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2df37-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2df37-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2df37-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2df37-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2df37-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2df37-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2df37-118">Scenario description</span></span>
<span data-ttu-id="2df37-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2df37-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2df37-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2df37-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2df37-121">Att lägga till SD-element från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2df37-121">Adding SD Elements from hello gallery</span></span>
2. <span data-ttu-id="2df37-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2df37-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sd-elements-from-hello-gallery"></a><span data-ttu-id="2df37-123">Att lägga till SD-element från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2df37-123">Adding SD Elements from hello gallery</span></span>
<span data-ttu-id="2df37-124">tooconfigure hello integrering av SD-element i Azure AD, behöver du tooadd SD-element från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="2df37-124">tooconfigure hello integration of SD Elements into Azure AD, you need tooadd SD Elements from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2df37-125">**tooadd SD-element från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2df37-125">**tooadd SD Elements from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2df37-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2df37-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2df37-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2df37-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2df37-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="2df37-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2df37-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2df37-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="2df37-133">Skriv i sökrutan hello **SD element**.</span><span class="sxs-lookup"><span data-stu-id="2df37-133">In hello search box, type **SD Elements**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_search.png)

5. <span data-ttu-id="2df37-135">Markera hello resultat på panelen **SD element**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="2df37-135">In hello results panel, select **SD Elements**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2df37-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2df37-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2df37-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med SD-element som baseras på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2df37-138">In this section, you configure and test Azure AD single sign-on with SD Elements based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2df37-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SD-element är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2df37-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SD Elements is tooa user in Azure AD.</span></span> <span data-ttu-id="2df37-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i SD-element toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="2df37-140">In other words, a link relationship between an Azure AD user and hello related user in SD Elements needs toobe established.</span></span>

<span data-ttu-id="2df37-141">I SD-element, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="2df37-141">In SD Elements, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2df37-142">tooconfigure och testa Azure AD enkel inloggning med SD-element måste toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2df37-142">tooconfigure and test Azure AD single sign-on with SD Elements, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2df37-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2df37-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2df37-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2df37-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2df37-145">**[Skapa en testanvändare SD element](#creating-a-sd-elements-test-user)**  -toohave en motsvarighet för Britta Simon i SD-element som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="2df37-145">**[Creating a SD Elements test user](#creating-a-sd-elements-test-user)** - toohave a counterpart of Britta Simon in SD Elements that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2df37-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2df37-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2df37-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2df37-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2df37-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2df37-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2df37-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program för SD-element.</span><span class="sxs-lookup"><span data-stu-id="2df37-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SD Elements application.</span></span>

<span data-ttu-id="2df37-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med SD-element:**</span><span class="sxs-lookup"><span data-stu-id="2df37-150">**tooconfigure Azure AD single sign-on with SD Elements, perform hello following steps:**</span></span>

1. <span data-ttu-id="2df37-151">I hello Azure-portalen på hello **SD element** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2df37-151">In hello Azure portal, on hello **SD Elements** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2df37-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2df37-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_samlbase.png)

3. <span data-ttu-id="2df37-155">På hello **SD element domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2df37-155">On hello **SD Elements Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_url.png)

    <span data-ttu-id="2df37-157">a.</span><span class="sxs-lookup"><span data-stu-id="2df37-157">a.</span></span> <span data-ttu-id="2df37-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.sdelements.com/sso/saml2/metadata`</span><span class="sxs-lookup"><span data-stu-id="2df37-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.sdelements.com/sso/saml2/metadata`</span></span>

    <span data-ttu-id="2df37-159">b.</span><span class="sxs-lookup"><span data-stu-id="2df37-159">b.</span></span> <span data-ttu-id="2df37-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.sdelements.com/sso/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="2df37-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<tenantname>.sdelements.com/sso/saml2/acs/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2df37-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="2df37-161">These values are not real.</span></span> <span data-ttu-id="2df37-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="2df37-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="2df37-163">Kontakta [SD element supportteam](mailto:support@sdelements.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="2df37-163">Contact [SD Elements support team](mailto:support@sdelements.com) tooget these values.</span></span>

4. <span data-ttu-id="2df37-164">Programmet SD-element förväntas hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="2df37-164">SD Elements application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="2df37-165">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="2df37-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="2df37-166">Du kan hantera hello värden för attributen från hello **”användarattribut”** fliken av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="2df37-166">You can manage hello values of these attributes from hello **"User Attribute"** tab of hello application.</span></span> <span data-ttu-id="2df37-167">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="2df37-167">hello following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_attribute.png)

5. <span data-ttu-id="2df37-169">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2df37-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span> 

    | <span data-ttu-id="2df37-170">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="2df37-170">Attribute Name</span></span> | <span data-ttu-id="2df37-171">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="2df37-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="2df37-172">E-post</span><span class="sxs-lookup"><span data-stu-id="2df37-172">email</span></span> |<span data-ttu-id="2df37-173">User.Mail</span><span class="sxs-lookup"><span data-stu-id="2df37-173">user.mail</span></span> |
    | <span data-ttu-id="2df37-174">Förnamn</span><span class="sxs-lookup"><span data-stu-id="2df37-174">firstname</span></span> |<span data-ttu-id="2df37-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="2df37-175">user.givenname</span></span> |
    | <span data-ttu-id="2df37-176">Efternamn</span><span class="sxs-lookup"><span data-stu-id="2df37-176">lastname</span></span> |<span data-ttu-id="2df37-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="2df37-177">user.surname</span></span> |

    <span data-ttu-id="2df37-178">a.</span><span class="sxs-lookup"><span data-stu-id="2df37-178">a.</span></span> <span data-ttu-id="2df37-179">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2df37-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="2df37-182">b.</span><span class="sxs-lookup"><span data-stu-id="2df37-182">b.</span></span> <span data-ttu-id="2df37-183">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="2df37-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="2df37-184">c.</span><span class="sxs-lookup"><span data-stu-id="2df37-184">c.</span></span> <span data-ttu-id="2df37-185">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="2df37-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="2df37-186">d.</span><span class="sxs-lookup"><span data-stu-id="2df37-186">d.</span></span> <span data-ttu-id="2df37-187">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2df37-187">Click **Ok**.</span></span>
 
6. <span data-ttu-id="2df37-188">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="2df37-188">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_certificate.png) 

7. <span data-ttu-id="2df37-190">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2df37-190">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="2df37-192">På hello **SD element Configuration** klickar du på **konfigurera SD-element** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="2df37-192">On hello **SD Elements Configuration** section, click **Configure SD Elements** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2df37-193">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="2df37-193">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_configure.png)

9. <span data-ttu-id="2df37-195">tooget enkel inloggning aktiverad, kontakta din [SD element supportteam](mailto:support@sdelements.com) och ge dem hello hämtade certifikatfil.</span><span class="sxs-lookup"><span data-stu-id="2df37-195">tooget single sign-on enabled, contact your [SD Elements support team](mailto:support@sdelements.com) and provide them with hello downloaded certificate file.</span></span> 

10. <span data-ttu-id="2df37-196">I ett annat fönster i webbläsaren, inloggning tooyour SD element klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="2df37-196">In a different browser window, sign-on tooyour SD Elements tenant as an administrator.</span></span>

11. <span data-ttu-id="2df37-197">Hello-menyn överst hello **System**, och sedan **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2df37-197">In hello menu on hello top, click **System**, and then **Single Sign-on**.</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 

12. <span data-ttu-id="2df37-199">På hello **inställningar för enkel inloggning** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2df37-199">On hello **Single Sign-On Settings** dialog, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    <span data-ttu-id="2df37-201">a.</span><span class="sxs-lookup"><span data-stu-id="2df37-201">a.</span></span> <span data-ttu-id="2df37-202">Som **SSO typen**väljer **SAML**.</span><span class="sxs-lookup"><span data-stu-id="2df37-202">As **SSO Type**, select **SAML**.</span></span>
   
    <span data-ttu-id="2df37-203">b.</span><span class="sxs-lookup"><span data-stu-id="2df37-203">b.</span></span> <span data-ttu-id="2df37-204">I hello **identitet providern enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2df37-204">In hello **Identity Provider Entity ID** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="2df37-205">c.</span><span class="sxs-lookup"><span data-stu-id="2df37-205">c.</span></span> <span data-ttu-id="2df37-206">I hello **identitet providern enkel inloggning** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2df37-206">In hello **Identity Provider Single Sign-On Service** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="2df37-207">d.</span><span class="sxs-lookup"><span data-stu-id="2df37-207">d.</span></span> <span data-ttu-id="2df37-208">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2df37-208">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="2df37-209">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="2df37-209">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2df37-210">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="2df37-210">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2df37-211">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2df37-211">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2df37-212">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2df37-212">Creating an Azure AD test user</span></span>
<span data-ttu-id="2df37-213">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2df37-213">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2df37-215">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2df37-215">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2df37-216">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2df37-216">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2df37-218">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2df37-218">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2df37-220">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2df37-220">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2df37-222">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2df37-222">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2df37-224">a.</span><span class="sxs-lookup"><span data-stu-id="2df37-224">a.</span></span> <span data-ttu-id="2df37-225">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2df37-225">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2df37-226">b.</span><span class="sxs-lookup"><span data-stu-id="2df37-226">b.</span></span> <span data-ttu-id="2df37-227">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2df37-227">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2df37-228">c.</span><span class="sxs-lookup"><span data-stu-id="2df37-228">c.</span></span> <span data-ttu-id="2df37-229">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2df37-229">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2df37-230">d.</span><span class="sxs-lookup"><span data-stu-id="2df37-230">d.</span></span> <span data-ttu-id="2df37-231">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2df37-231">Click **Create**.</span></span>
 
### <a name="creating-a-sd-elements-test-user"></a><span data-ttu-id="2df37-232">Skapa en testanvändare SD-element</span><span class="sxs-lookup"><span data-stu-id="2df37-232">Creating a SD Elements test user</span></span>

<span data-ttu-id="2df37-233">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i SD-element.</span><span class="sxs-lookup"><span data-stu-id="2df37-233">hello objective of this section is toocreate a user called Britta Simon in SD Elements.</span></span> <span data-ttu-id="2df37-234">Skapa element SD-användare är en manuell aktivitet i hello fallet SD-element.</span><span class="sxs-lookup"><span data-stu-id="2df37-234">In hello case of SD Elements, creating SD Elements users is a manual task.</span></span>

<span data-ttu-id="2df37-235">**toocreate Britta Simon i SD-element, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="2df37-235">**toocreate Britta Simon in SD Elements, perform hello following steps:**</span></span>

1. <span data-ttu-id="2df37-236">I ett webbläsarfönster, inloggning tooyour SD element företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="2df37-236">In a web browser window, sign-on tooyour SD Elements company site as an administrator.</span></span>

2. <span data-ttu-id="2df37-237">Hello-menyn överst hello **Användarhantering**, och sedan **användare**.</span><span class="sxs-lookup"><span data-stu-id="2df37-237">In hello menu on hello top, click **User Management**, and then **Users**.</span></span>
   
    ![Skapa en testanvändare SD-element](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 

3. <span data-ttu-id="2df37-239">Klicka på **lägga till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="2df37-239">Click **Add New User**.</span></span>
   
    ![Skapa en testanvändare SD-element](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png)
 
4. <span data-ttu-id="2df37-241">På hello **Lägg till nya användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2df37-241">On hello **Add New User** dialog, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare SD-element](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    <span data-ttu-id="2df37-243">a.</span><span class="sxs-lookup"><span data-stu-id="2df37-243">a.</span></span> <span data-ttu-id="2df37-244">I hello **e-post** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="2df37-244">In hello **E-mail** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="2df37-245">b.</span><span class="sxs-lookup"><span data-stu-id="2df37-245">b.</span></span> <span data-ttu-id="2df37-246">I hello **Förnamn** textruta anger hello först namnet på användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="2df37-246">In hello **First Name** textbox, enter hello first name of user like **Britta**.</span></span>
   
    <span data-ttu-id="2df37-247">c.</span><span class="sxs-lookup"><span data-stu-id="2df37-247">c.</span></span> <span data-ttu-id="2df37-248">I hello **efternamn** textruta ange hello efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="2df37-248">In hello **Last Name** textbox, enter hello last name of user like **Simon**.</span></span>
   
    <span data-ttu-id="2df37-249">d.</span><span class="sxs-lookup"><span data-stu-id="2df37-249">d.</span></span> <span data-ttu-id="2df37-250">Som **rollen**väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="2df37-250">As **Role**, select **User**.</span></span> 
   
    <span data-ttu-id="2df37-251">e.</span><span class="sxs-lookup"><span data-stu-id="2df37-251">e.</span></span> <span data-ttu-id="2df37-252">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="2df37-252">Click **Create User**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2df37-253">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="2df37-253">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2df37-254">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSD element.</span><span class="sxs-lookup"><span data-stu-id="2df37-254">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSD Elements.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2df37-256">**tooassign Britta Simon tooSD element, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="2df37-256">**tooassign Britta Simon tooSD Elements, perform hello following steps:**</span></span>

1. <span data-ttu-id="2df37-257">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2df37-257">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2df37-259">Välj i listan med program hello **SD element**.</span><span class="sxs-lookup"><span data-stu-id="2df37-259">In hello applications list, select **SD Elements**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_app.png) 

3. <span data-ttu-id="2df37-261">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2df37-261">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2df37-263">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2df37-263">Click **Add** button.</span></span> <span data-ttu-id="2df37-264">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2df37-264">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2df37-266">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="2df37-266">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2df37-267">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2df37-267">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2df37-268">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2df37-268">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2df37-269">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2df37-269">Testing single sign-on</span></span>

<span data-ttu-id="2df37-270">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2df37-270">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>
  
<span data-ttu-id="2df37-271">Du bör få automatiskt inloggade tooyour SD element programmet när du klickar på hello SD element panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2df37-271">When you click hello SD Elements tile in hello Access Panel, you should get automatically signed-on tooyour SD Elements application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2df37-272">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2df37-272">Additional resources</span></span>

* [<span data-ttu-id="2df37-273">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2df37-273">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2df37-274">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2df37-274">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png

