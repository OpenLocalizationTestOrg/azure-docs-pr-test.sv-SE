---
title: "Självstudier: Azure Active Directory-integrering med DigiCert | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och DigiCert."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 646f3129-aa67-4875-9073-1d0b6a3173d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 861039d00533b3aeb361d04e45c4460c6fc8cef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-digicert"></a><span data-ttu-id="bd272-103">Självstudier: Azure Active Directory-integrering med DigiCert</span><span class="sxs-lookup"><span data-stu-id="bd272-103">Tutorial: Azure Active Directory integration with DigiCert</span></span>

<span data-ttu-id="bd272-104">I kursen får du lära dig hur toointegrate DigiCert med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="bd272-104">In this tutorial, you learn how toointegrate DigiCert with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bd272-105">Integrera DigiCert med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="bd272-105">Integrating DigiCert with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bd272-106">Du kan styra i Azure AD som har åtkomst till tooDigiCert</span><span class="sxs-lookup"><span data-stu-id="bd272-106">You can control in Azure AD who has access tooDigiCert</span></span>
- <span data-ttu-id="bd272-107">Du kan aktivera din användare tooautomatically get inloggade tooDigiCert (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="bd272-107">You can enable your users tooautomatically get signed-on tooDigiCert (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bd272-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bd272-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="bd272-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bd272-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd272-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bd272-110">Prerequisites</span></span>

<span data-ttu-id="bd272-111">tooconfigure Azure AD-integrering med DigiCert, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="bd272-111">tooconfigure Azure AD integration with DigiCert, you need hello following items:</span></span>

- <span data-ttu-id="bd272-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="bd272-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bd272-113">En DigiCert enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="bd272-113">A DigiCert single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bd272-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="bd272-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bd272-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="bd272-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bd272-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="bd272-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bd272-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bd272-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bd272-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="bd272-118">Scenario description</span></span>
<span data-ttu-id="bd272-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="bd272-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bd272-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="bd272-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bd272-121">Att lägga till DigiCert från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="bd272-121">Adding DigiCert from hello gallery</span></span>
2. <span data-ttu-id="bd272-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd272-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-digicert-from-hello-gallery"></a><span data-ttu-id="bd272-123">Att lägga till DigiCert från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="bd272-123">Adding DigiCert from hello gallery</span></span>
<span data-ttu-id="bd272-124">tooconfigure hello integrering av DigiCert i Azure AD, behöver du tooadd DigiCert hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="bd272-124">tooconfigure hello integration of DigiCert into Azure AD, you need tooadd DigiCert from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bd272-125">**tooadd DigiCert från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bd272-125">**tooadd DigiCert from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bd272-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bd272-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bd272-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="bd272-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bd272-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="bd272-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="bd272-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd272-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="bd272-133">Skriv i sökrutan hello **DigiCert**.</span><span class="sxs-lookup"><span data-stu-id="bd272-133">In hello search box, type **DigiCert**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_search.png)

5. <span data-ttu-id="bd272-135">Markera hello resultat på panelen **DigiCert**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="bd272-135">In hello results panel, select **DigiCert**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bd272-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd272-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bd272-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med DigiCert baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="bd272-138">In this section, you configure and test Azure AD single sign-on with DigiCert based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bd272-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i DigiCert är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bd272-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in DigiCert is tooa user in Azure AD.</span></span> <span data-ttu-id="bd272-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i DigiCert toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="bd272-140">In other words, a link relationship between an Azure AD user and hello related user in DigiCert needs toobe established.</span></span>

<span data-ttu-id="bd272-141">I DigiCert, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="bd272-141">In DigiCert, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bd272-142">tooconfigure och testa Azure AD enkel inloggning med DigiCert, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="bd272-142">tooconfigure and test Azure AD single sign-on with DigiCert, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bd272-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="bd272-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bd272-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bd272-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bd272-145">**[Skapa en testanvändare DigiCert](#creating-a-digicert-test-user)**  -toohave en motsvarighet för Britta Simon i DigiCert som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="bd272-145">**[Creating a DigiCert test user](#creating-a-digicert-test-user)** - toohave a counterpart of Britta Simon in DigiCert that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bd272-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bd272-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bd272-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="bd272-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bd272-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd272-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bd272-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt DigiCert-program.</span><span class="sxs-lookup"><span data-stu-id="bd272-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your DigiCert application.</span></span>

<span data-ttu-id="bd272-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med DigiCert:**</span><span class="sxs-lookup"><span data-stu-id="bd272-150">**tooconfigure Azure AD single sign-on with DigiCert, perform hello following steps:**</span></span>

1. <span data-ttu-id="bd272-151">I hello Azure-portalen på hello **DigiCert** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="bd272-151">In hello Azure portal, on hello **DigiCert** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="bd272-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bd272-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_samlbase.png)

3. <span data-ttu-id="bd272-155">På hello **DigiCert domän och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.</span><span class="sxs-lookup"><span data-stu-id="bd272-155">On hello **DigiCert Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_url.png)

4. <span data-ttu-id="bd272-157">DigiCert program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="bd272-157">DigiCert application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="bd272-158">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="bd272-158">Configure hello following claims for this application.</span></span> <span data-ttu-id="bd272-159">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="bd272-159">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="bd272-160">hello följande skärmbild visar ett exempel på den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="bd272-160">hello following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_attributes.png)
    
5. <span data-ttu-id="bd272-162">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd272-162">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="bd272-163">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="bd272-163">Attribute Name</span></span> | <span data-ttu-id="bd272-164">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="bd272-164">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="bd272-165">Företag</span><span class="sxs-lookup"><span data-stu-id="bd272-165">company</span></span> | <span data-ttu-id="bd272-166">< companycode ></span><span class="sxs-lookup"><span data-stu-id="bd272-166">< companycode ></span></span> |
    | <span data-ttu-id="bd272-167">digicertrole</span><span class="sxs-lookup"><span data-stu-id="bd272-167">digicertrole</span></span> | <span data-ttu-id="bd272-168">CanAccessCertCentral</span><span class="sxs-lookup"><span data-stu-id="bd272-168">CanAccessCertCentral</span></span> |

    > [!Note]
    > <span data-ttu-id="bd272-169">Hej värdet för **företagets** attributet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="bd272-169">hello value of **company** attribute is not real.</span></span> <span data-ttu-id="bd272-170">Uppdatera det här värdet med verkligt företagskod.</span><span class="sxs-lookup"><span data-stu-id="bd272-170">Update this value with actual company code.</span></span> <span data-ttu-id="bd272-171">tooget hello värdet för **företagets** attributet Kontakta [DigiCert supportteamet](mailto:support@digicert.com).</span><span class="sxs-lookup"><span data-stu-id="bd272-171">tooget hello value of **company** attribute contact [DigiCert support team](mailto:support@digicert.com).</span></span>

    <span data-ttu-id="bd272-172">a.</span><span class="sxs-lookup"><span data-stu-id="bd272-172">a.</span></span> <span data-ttu-id="bd272-173">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd272-173">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="bd272-176">b.</span><span class="sxs-lookup"><span data-stu-id="bd272-176">b.</span></span> <span data-ttu-id="bd272-177">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="bd272-177">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="bd272-178">c.</span><span class="sxs-lookup"><span data-stu-id="bd272-178">c.</span></span> <span data-ttu-id="bd272-179">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="bd272-179">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="bd272-180">d.</span><span class="sxs-lookup"><span data-stu-id="bd272-180">d.</span></span> <span data-ttu-id="bd272-181">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd272-181">Click **Ok**.</span></span> 

6. <span data-ttu-id="bd272-182">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="bd272-182">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_certificate.png) 

7. <span data-ttu-id="bd272-184">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="bd272-184">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="bd272-186">tooconfigure enkel inloggning på **DigiCert** sida, behöver du toosend hello hämtas **XML-Metadata för** för[DigiCert supportteamet](mailto:support@digicert.com).</span><span class="sxs-lookup"><span data-stu-id="bd272-186">tooconfigure single sign-on on **DigiCert** side, you need toosend hello downloaded **Metadata XML** too[DigiCert support team](mailto:support@digicert.com).</span></span> <span data-ttu-id="bd272-187">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="bd272-187">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="bd272-188">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="bd272-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bd272-189">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="bd272-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bd272-190">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bd272-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bd272-191">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="bd272-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="bd272-192">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bd272-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="bd272-194">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bd272-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bd272-195">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bd272-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bd272-197">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="bd272-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bd272-199">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd272-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bd272-201">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd272-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bd272-203">a.</span><span class="sxs-lookup"><span data-stu-id="bd272-203">a.</span></span> <span data-ttu-id="bd272-204">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bd272-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bd272-205">b.</span><span class="sxs-lookup"><span data-stu-id="bd272-205">b.</span></span> <span data-ttu-id="bd272-206">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bd272-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bd272-207">c.</span><span class="sxs-lookup"><span data-stu-id="bd272-207">c.</span></span> <span data-ttu-id="bd272-208">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="bd272-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="bd272-209">d.</span><span class="sxs-lookup"><span data-stu-id="bd272-209">d.</span></span> <span data-ttu-id="bd272-210">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bd272-210">Click **Create**.</span></span>
 
### <a name="creating-a-digicert-test-user"></a><span data-ttu-id="bd272-211">Skapa en testanvändare DigiCert</span><span class="sxs-lookup"><span data-stu-id="bd272-211">Creating a DigiCert test user</span></span>

<span data-ttu-id="bd272-212">I det här avsnittet skapar du en användare som kallas Britta Simon i DigiCert.</span><span class="sxs-lookup"><span data-stu-id="bd272-212">In this section, you create a user called Britta Simon in DigiCert.</span></span> <span data-ttu-id="bd272-213">Se tillsammans med [DigiCert supportteamet](mailto:support@digicert.com) tooadd hello användare i DigiCert.</span><span class="sxs-lookup"><span data-stu-id="bd272-213">Please work with [DigiCert support team](mailto:support@digicert.com) tooadd hello users in DigiCert.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="bd272-214">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="bd272-214">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="bd272-215">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDigiCert.</span><span class="sxs-lookup"><span data-stu-id="bd272-215">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDigiCert.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="bd272-217">**tooassign Britta Simon tooDigiCert utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bd272-217">**tooassign Britta Simon tooDigiCert, perform hello following steps:**</span></span>

1. <span data-ttu-id="bd272-218">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bd272-218">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="bd272-220">Välj i listan med program hello **DigiCert**.</span><span class="sxs-lookup"><span data-stu-id="bd272-220">In hello applications list, select **DigiCert**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_app.png) 

3. <span data-ttu-id="bd272-222">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="bd272-222">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="bd272-224">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="bd272-224">Click **Add** button.</span></span> <span data-ttu-id="bd272-225">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd272-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="bd272-227">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="bd272-227">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bd272-228">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd272-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bd272-229">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd272-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bd272-230">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd272-230">Testing single sign-on</span></span>

<span data-ttu-id="bd272-231">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="bd272-231">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="bd272-232">Du bör få automatiskt inloggade tooyour DeigiCert programmet när du klickar på hello DigiCert-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="bd272-232">When you click hello DigiCert tile in hello Access Panel, you should get automatically signed-on tooyour DeigiCert application.</span></span>
<span data-ttu-id="bd272-233">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bd272-233">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="bd272-234">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="bd272-234">Additional resources</span></span>

* [<span data-ttu-id="bd272-235">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bd272-235">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bd272-236">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bd272-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_203.png

