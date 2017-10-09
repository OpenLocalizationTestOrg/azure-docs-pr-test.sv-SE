---
title: "Självstudier: Azure Active Directory-integrering med BenefitHub | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och BenefitHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4069fe32-a452-463f-973e-7aa0baa4c2fa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: c07d6e44e8cbc79afd79c900664011b059206b56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefithub"></a><span data-ttu-id="85523-103">Självstudier: Azure Active Directory-integrering med BenefitHub</span><span class="sxs-lookup"><span data-stu-id="85523-103">Tutorial: Azure Active Directory integration with BenefitHub</span></span>

<span data-ttu-id="85523-104">I kursen får du lära dig hur toointegrate BenefitHub med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="85523-104">In this tutorial, you learn how toointegrate BenefitHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="85523-105">Integrera BenefitHub med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="85523-105">Integrating BenefitHub with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="85523-106">Du kan styra i Azure AD som har åtkomst till tooBenefitHub</span><span class="sxs-lookup"><span data-stu-id="85523-106">You can control in Azure AD who has access tooBenefitHub</span></span>
- <span data-ttu-id="85523-107">Du kan aktivera din användare tooautomatically get inloggade tooBenefitHub (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="85523-107">You can enable your users tooautomatically get signed-on tooBenefitHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="85523-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="85523-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="85523-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="85523-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="85523-110">Krav</span><span class="sxs-lookup"><span data-stu-id="85523-110">Prerequisites</span></span>

<span data-ttu-id="85523-111">tooconfigure Azure AD-integrering med BenefitHub, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="85523-111">tooconfigure Azure AD integration with BenefitHub, you need hello following items:</span></span>

- <span data-ttu-id="85523-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="85523-112">An Azure AD subscription</span></span>
- <span data-ttu-id="85523-113">En BenefitHub enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="85523-113">A BenefitHub single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="85523-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="85523-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="85523-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="85523-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="85523-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="85523-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="85523-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="85523-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="85523-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="85523-118">Scenario description</span></span>
<span data-ttu-id="85523-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="85523-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="85523-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="85523-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="85523-121">Att lägga till BenefitHub från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="85523-121">Adding BenefitHub from hello gallery</span></span>
2. <span data-ttu-id="85523-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="85523-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-benefithub-from-hello-gallery"></a><span data-ttu-id="85523-123">Att lägga till BenefitHub från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="85523-123">Adding BenefitHub from hello gallery</span></span>
<span data-ttu-id="85523-124">tooconfigure hello integrering av BenefitHub i Azure AD, behöver du tooadd BenefitHub hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="85523-124">tooconfigure hello integration of BenefitHub into Azure AD, you need tooadd BenefitHub from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="85523-125">**tooadd BenefitHub från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="85523-125">**tooadd BenefitHub from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="85523-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="85523-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="85523-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="85523-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="85523-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="85523-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="85523-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="85523-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="85523-133">Skriv i sökrutan hello **BenefitHub**.</span><span class="sxs-lookup"><span data-stu-id="85523-133">In hello search box, type **BenefitHub**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_search.png)

5. <span data-ttu-id="85523-135">Markera hello resultat på panelen **BenefitHub**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="85523-135">In hello results panel, select **BenefitHub**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="85523-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="85523-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="85523-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BenefitHub baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="85523-138">In this section, you configure and test Azure AD single sign-on with BenefitHub based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="85523-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i BenefitHub är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="85523-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BenefitHub is tooa user in Azure AD.</span></span> <span data-ttu-id="85523-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i BenefitHub toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="85523-140">In other words, a link relationship between an Azure AD user and hello related user in BenefitHub needs toobe established.</span></span>

<span data-ttu-id="85523-141">I BenefitHub, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="85523-141">In BenefitHub, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="85523-142">tooconfigure och testa Azure AD enkel inloggning med BenefitHub, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="85523-142">tooconfigure and test Azure AD single sign-on with BenefitHub, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="85523-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="85523-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="85523-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="85523-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="85523-145">**[Skapa en testanvändare BenefitHub](#creating-a-benefithub-test-user)**  -toohave en motsvarighet för Britta Simon i BenefitHub som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="85523-145">**[Creating a BenefitHub test user](#creating-a-benefithub-test-user)** - toohave a counterpart of Britta Simon in BenefitHub that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="85523-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="85523-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="85523-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="85523-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="85523-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="85523-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="85523-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt BenefitHub program.</span><span class="sxs-lookup"><span data-stu-id="85523-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BenefitHub application.</span></span>

<span data-ttu-id="85523-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med BenefitHub:**</span><span class="sxs-lookup"><span data-stu-id="85523-150">**tooconfigure Azure AD single sign-on with BenefitHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="85523-151">I hello Azure-portalen på hello **BenefitHub** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="85523-151">In hello Azure portal, on hello **BenefitHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="85523-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="85523-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_samlbase.png)

3. <span data-ttu-id="85523-155">På hello **BenefitHub domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="85523-155">On hello **BenefitHub Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_url1.png)
  
    <span data-ttu-id="85523-157">a.</span><span class="sxs-lookup"><span data-stu-id="85523-157">a.</span></span> <span data-ttu-id="85523-158">I hello **identifierare** textruta typ:`urn:benefithub:passport`</span><span class="sxs-lookup"><span data-stu-id="85523-158">In hello **Identifier** textbox, type: `urn:benefithub:passport`</span></span>
    
    <span data-ttu-id="85523-159">b.</span><span class="sxs-lookup"><span data-stu-id="85523-159">b.</span></span> <span data-ttu-id="85523-160">I hello **Reply URL** textruta typ:`https://passport.benefithub.info/saml/post/ac`</span><span class="sxs-lookup"><span data-stu-id="85523-160">In hello **Reply URL** textbox, type: `https://passport.benefithub.info/saml/post/ac`</span></span>

4. <span data-ttu-id="85523-161">Hej BenefitHub program förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="85523-161">hello BenefitHub application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="85523-162">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="85523-162">Configure hello following claims for this application.</span></span> <span data-ttu-id="85523-163">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="85523-163">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_attribute.png)

5. <span data-ttu-id="85523-165">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i föregående bild hello och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="85523-165">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello preceding image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="85523-166">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="85523-166">Attribute Name</span></span> | <span data-ttu-id="85523-167">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="85523-167">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="85523-168">organisations-ID</span><span class="sxs-lookup"><span data-stu-id="85523-168">organizationid</span></span> | <span data-ttu-id="85523-169">organisations-< ID ></span><span class="sxs-lookup"><span data-stu-id="85523-169">< organizationid ></span></span> |

    > [!NOTE]
    > <span data-ttu-id="85523-170">Det här attributvärdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="85523-170">This attribute value is not real.</span></span> <span data-ttu-id="85523-171">Uppdatera det här värdet med faktiska organisations-ID.</span><span class="sxs-lookup"><span data-stu-id="85523-171">Update this value with actual organizationid.</span></span> <span data-ttu-id="85523-172">Kontakta [BenefitHub supportteamet](https://www.benefithub.com/Home/ContactUs) tooget hello faktiska organisations-ID.</span><span class="sxs-lookup"><span data-stu-id="85523-172">Contact [BenefitHub support team](https://www.benefithub.com/Home/ContactUs) tooget hello actual organizationid.</span></span>
    
    <span data-ttu-id="85523-173">a.</span><span class="sxs-lookup"><span data-stu-id="85523-173">a.</span></span> <span data-ttu-id="85523-174">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="85523-174">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="85523-177">b.</span><span class="sxs-lookup"><span data-stu-id="85523-177">b.</span></span> <span data-ttu-id="85523-178">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="85523-178">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="85523-179">c.</span><span class="sxs-lookup"><span data-stu-id="85523-179">c.</span></span> <span data-ttu-id="85523-180">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="85523-180">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="85523-181">d.</span><span class="sxs-lookup"><span data-stu-id="85523-181">d.</span></span> <span data-ttu-id="85523-182">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="85523-182">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="85523-183">Innan du kan konfigurera hello SAML-kontroll måste toocontact din [BenefitHub stöd](https://www.benefithub.com/Home/ContactUs) och begära hello värdet för attributet för hello-Unik identifierare för din klient.</span><span class="sxs-lookup"><span data-stu-id="85523-183">Before you can configure hello SAML assertion, you need toocontact your [BenefitHub support](https://www.benefithub.com/Home/ContactUs) and request hello value of hello unique identifier attribute for your tenant.</span></span> <span data-ttu-id="85523-184">Du behöver det här värdet tooconfigure hello anpassat anspråk för ditt program.</span><span class="sxs-lookup"><span data-stu-id="85523-184">You need this value tooconfigure hello custom claim for your application.</span></span>

6. <span data-ttu-id="85523-185">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="85523-185">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_certificate.png) 

7. <span data-ttu-id="85523-187">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="85523-187">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="85523-189">tooconfigure enkel inloggning på **BenefitHub** sida, behöver du toosend hello hämtas **XML-Metadata för** för[BenefitHub supportteamet](https://www.benefithub.com/Home/ContactUs).</span><span class="sxs-lookup"><span data-stu-id="85523-189">tooconfigure single sign-on on **BenefitHub** side, you need toosend hello downloaded **Metadata XML** too[BenefitHub support team](https://www.benefithub.com/Home/ContactUs).</span></span> <span data-ttu-id="85523-190">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="85523-190">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="85523-191">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="85523-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="85523-192">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="85523-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="85523-193">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="85523-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="85523-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="85523-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="85523-195">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="85523-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="85523-197">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="85523-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="85523-198">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="85523-198">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="85523-200">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="85523-200">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="85523-202">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="85523-202">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="85523-204">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="85523-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="85523-206">a.</span><span class="sxs-lookup"><span data-stu-id="85523-206">a.</span></span> <span data-ttu-id="85523-207">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="85523-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="85523-208">b.</span><span class="sxs-lookup"><span data-stu-id="85523-208">b.</span></span> <span data-ttu-id="85523-209">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="85523-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="85523-210">c.</span><span class="sxs-lookup"><span data-stu-id="85523-210">c.</span></span> <span data-ttu-id="85523-211">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="85523-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="85523-212">d.</span><span class="sxs-lookup"><span data-stu-id="85523-212">d.</span></span> <span data-ttu-id="85523-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="85523-213">Click **Create**.</span></span>
 
### <a name="creating-a-benefithub-test-user"></a><span data-ttu-id="85523-214">Skapa en testanvändare BenefitHub</span><span class="sxs-lookup"><span data-stu-id="85523-214">Creating a BenefitHub test user</span></span>

<span data-ttu-id="85523-215">I det här avsnittet skapar du en användare som kallas Britta Simon i BenefitHub.</span><span class="sxs-lookup"><span data-stu-id="85523-215">In this section, you create a user called Britta Simon in BenefitHub.</span></span> <span data-ttu-id="85523-216">Arbeta med [BenefitHub supportteamet](https://www.benefithub.com/Home/ContactUs) att lägga till hello användare i hello BenefitHub plattform.</span><span class="sxs-lookup"><span data-stu-id="85523-216">Work with [BenefitHub support team](https://www.benefithub.com/Home/ContactUs) to add hello users in hello BenefitHub platform.</span></span> <span data-ttu-id="85523-217">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="85523-217">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="85523-218">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="85523-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="85523-219">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBenefitHub.</span><span class="sxs-lookup"><span data-stu-id="85523-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBenefitHub.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="85523-221">**tooassign Britta Simon tooBenefitHub utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="85523-221">**tooassign Britta Simon tooBenefitHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="85523-222">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="85523-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="85523-224">Välj i listan med program hello **BenefitHub**.</span><span class="sxs-lookup"><span data-stu-id="85523-224">In hello applications list, select **BenefitHub**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_app.png) 

3. <span data-ttu-id="85523-226">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="85523-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="85523-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="85523-228">Click **Add** button.</span></span> <span data-ttu-id="85523-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="85523-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="85523-231">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="85523-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="85523-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="85523-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="85523-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="85523-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="85523-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="85523-234">Testing single sign-on</span></span>

<span data-ttu-id="85523-235">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="85523-235">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="85523-236">Du bör få automatiskt inloggade tooyour BenefitHub programmet när du klickar på hello BenefitHub panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="85523-236">When you click hello BenefitHub tile in hello Access Panel, you should get automatically signed-on tooyour BenefitHub application.</span></span>
<span data-ttu-id="85523-237">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="85523-237">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="85523-238">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="85523-238">Additional resources</span></span>

* [<span data-ttu-id="85523-239">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="85523-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="85523-240">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="85523-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_203.png

