---
title: "Självstudier: Azure Active Directory-integrering med BetterWorks | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och BetterWorks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5bb9505a-be02-46ae-9979-5308715d2b47
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 9803593124318ea82e5a8888cc5a95b5da84472e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a><span data-ttu-id="46270-103">Självstudier: Azure Active Directory-integrering med BetterWorks</span><span class="sxs-lookup"><span data-stu-id="46270-103">Tutorial: Azure Active Directory integration with BetterWorks</span></span>

<span data-ttu-id="46270-104">I kursen får du lära dig hur toointegrate BetterWorks med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="46270-104">In this tutorial, you learn how toointegrate BetterWorks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="46270-105">Integrera BetterWorks med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="46270-105">Integrating BetterWorks with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="46270-106">Du kan styra i Azure AD som har åtkomst till tooBetterWorks</span><span class="sxs-lookup"><span data-stu-id="46270-106">You can control in Azure AD who has access tooBetterWorks</span></span>
- <span data-ttu-id="46270-107">Du kan aktivera din användare tooautomatically get inloggade tooBetterWorks (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="46270-107">You can enable your users tooautomatically get signed-on tooBetterWorks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="46270-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="46270-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="46270-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="46270-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46270-110">Krav</span><span class="sxs-lookup"><span data-stu-id="46270-110">Prerequisites</span></span>

<span data-ttu-id="46270-111">tooconfigure Azure AD-integrering med BetterWorks, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="46270-111">tooconfigure Azure AD integration with BetterWorks, you need hello following items:</span></span>

- <span data-ttu-id="46270-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="46270-112">An Azure AD subscription</span></span>
- <span data-ttu-id="46270-113">En BetterWorks enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="46270-113">A BetterWorks single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="46270-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="46270-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="46270-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="46270-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="46270-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="46270-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="46270-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="46270-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="46270-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="46270-118">Scenario description</span></span>
<span data-ttu-id="46270-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="46270-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="46270-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="46270-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="46270-121">Att lägga till BetterWorks från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="46270-121">Adding BetterWorks from hello gallery</span></span>
2. <span data-ttu-id="46270-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46270-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-betterworks-from-hello-gallery"></a><span data-ttu-id="46270-123">Att lägga till BetterWorks från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="46270-123">Adding BetterWorks from hello gallery</span></span>
<span data-ttu-id="46270-124">tooconfigure hello integrering av BetterWorks i Azure AD, behöver du tooadd BetterWorks hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="46270-124">tooconfigure hello integration of BetterWorks into Azure AD, you need tooadd BetterWorks from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="46270-125">**tooadd BetterWorks från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="46270-125">**tooadd BetterWorks from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="46270-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="46270-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="46270-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="46270-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="46270-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="46270-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="46270-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46270-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="46270-133">Skriv i sökrutan hello **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="46270-133">In hello search box, type **BetterWorks**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_search.png)

5. <span data-ttu-id="46270-135">Markera hello resultat på panelen **BetterWorks**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="46270-135">In hello results panel, select **BetterWorks**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="46270-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46270-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="46270-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BetterWorks baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="46270-138">In this section, you configure and test Azure AD single sign-on with BetterWorks based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="46270-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i BetterWorks är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="46270-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BetterWorks is tooa user in Azure AD.</span></span> <span data-ttu-id="46270-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i BetterWorks toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="46270-140">In other words, a link relationship between an Azure AD user and hello related user in BetterWorks needs toobe established.</span></span>

<span data-ttu-id="46270-141">I BetterWorks, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="46270-141">In BetterWorks, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="46270-142">tooconfigure och testa Azure AD enkel inloggning med BetterWorks, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="46270-142">tooconfigure and test Azure AD single sign-on with BetterWorks, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="46270-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="46270-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="46270-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="46270-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="46270-145">**[Skapa en testanvändare BetterWorks](#creating-a-betterworks-test-user)**  -toohave en motsvarighet för Britta Simon i BetterWorks som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="46270-145">**[Creating a BetterWorks test user](#creating-a-betterworks-test-user)** - toohave a counterpart of Britta Simon in BetterWorks that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="46270-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="46270-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="46270-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="46270-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="46270-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46270-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="46270-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt BetterWorks program.</span><span class="sxs-lookup"><span data-stu-id="46270-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BetterWorks application.</span></span>

<span data-ttu-id="46270-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med BetterWorks:**</span><span class="sxs-lookup"><span data-stu-id="46270-150">**tooconfigure Azure AD single sign-on with BetterWorks, perform hello following steps:**</span></span>

1. <span data-ttu-id="46270-151">I hello Azure-portalen på hello **BetterWorks** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="46270-151">In hello Azure portal, on hello **BetterWorks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="46270-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="46270-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_samlbase.png)

3. <span data-ttu-id="46270-155">På hello **BetterWorks domän och URL: er** om du vill tooconfigure hello programmet i **IDP initierade läge**:</span><span class="sxs-lookup"><span data-stu-id="46270-155">On hello **BetterWorks Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url.png)

    <span data-ttu-id="46270-157">a.</span><span class="sxs-lookup"><span data-stu-id="46270-157">a.</span></span> <span data-ttu-id="46270-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://app.betterworks.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="46270-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://app.betterworks.com/saml2/metadata/`</span></span>

    <span data-ttu-id="46270-159">b.</span><span class="sxs-lookup"><span data-stu-id="46270-159">b.</span></span> <span data-ttu-id="46270-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://app.betterworks.com/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="46270-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://app.betterworks.com/saml2/acs/`</span></span>

4. <span data-ttu-id="46270-161">På hello **BetterWorks domän och URL: er** om du vill tooconfigure hello programmet i **SP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="46270-161">On hello **BetterWorks Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url1.png)

    <span data-ttu-id="46270-163">a.</span><span class="sxs-lookup"><span data-stu-id="46270-163">a.</span></span> <span data-ttu-id="46270-164">Klicka på hello **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="46270-164">Click on hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="46270-165">b.</span><span class="sxs-lookup"><span data-stu-id="46270-165">b.</span></span> <span data-ttu-id="46270-166">I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://app.betterworks.com`</span><span class="sxs-lookup"><span data-stu-id="46270-166">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://app.betterworks.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="46270-167">Detta är inte verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="46270-167">These are not real values.</span></span> <span data-ttu-id="46270-168">Uppdatera dessa värden med hello Reply URL identifierare och den faktiska logga URL.</span><span class="sxs-lookup"><span data-stu-id="46270-168">Update these values with hello Reply URL, Identifier and actual Sign On URL.</span></span> <span data-ttu-id="46270-169">Kontakta [BetterWorks supportteam](mailto:support@betterworks.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="46270-169">Contact [BetterWorks support team](mailto:support@betterworks.com) tooget these values.</span></span>
 
4. <span data-ttu-id="46270-170">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="46270-170">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_certificate.png)  

5. <span data-ttu-id="46270-172">BetterWorks program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="46270-172">BetterWorks application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="46270-173">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="46270-173">Configure hello following claims for this application.</span></span> <span data-ttu-id="46270-174">Du kan hantera hello värden för attributen från hello ”**attributet**” fliken av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="46270-174">You can manage hello values of these attributes from hello "**Attribute**" tab of hello application.</span></span> <span data-ttu-id="46270-175">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="46270-175">hello following screenshot shows an example for this.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_attribute.png)

6. <span data-ttu-id="46270-177">På hello **SAML-token attribut** dialogrutan för varje rad som visas i hello nedan, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="46270-177">On hello **SAML token attributes** dialog, for each row shown in hello table below, perform hello following steps:</span></span>
 
   | <span data-ttu-id="46270-178">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="46270-178">Attribute Name</span></span> | <span data-ttu-id="46270-179">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="46270-179">Attribute Value</span></span> |
   | -------------- |  ------------ |
   | <span data-ttu-id="46270-180">saml_token</span><span class="sxs-lookup"><span data-stu-id="46270-180">saml_token</span></span>     | <span data-ttu-id="46270-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span><span class="sxs-lookup"><span data-stu-id="46270-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span></span> |

   <span data-ttu-id="46270-182">a.</span><span class="sxs-lookup"><span data-stu-id="46270-182">a.</span></span> <span data-ttu-id="46270-183">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46270-183">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_05.png)

   <span data-ttu-id="46270-186">b.</span><span class="sxs-lookup"><span data-stu-id="46270-186">b.</span></span> <span data-ttu-id="46270-187">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="46270-187">In hello **Name** textbox, type hello attribute name shown for that row.</span></span> 

   <span data-ttu-id="46270-188">c.</span><span class="sxs-lookup"><span data-stu-id="46270-188">c.</span></span> <span data-ttu-id="46270-189">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="46270-189">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
   <span data-ttu-id="46270-190">d.</span><span class="sxs-lookup"><span data-stu-id="46270-190">d.</span></span> <span data-ttu-id="46270-191">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="46270-191">Click **Ok**.</span></span>

7. <span data-ttu-id="46270-192">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="46270-192">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="46270-194">tooconfigure enkel inloggning på **BetterWorks** sida, behöver du toosend hello hämtas **XML-Metadata för** för[BetterWorks supportteam](mailto:support@betterworks.com).</span><span class="sxs-lookup"><span data-stu-id="46270-194">tooconfigure single sign-on on **BetterWorks** side, you need toosend hello downloaded **Metadata XML** too[BetterWorks support team](mailto:support@betterworks.com).</span></span>


> [!TIP]
> <span data-ttu-id="46270-195">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="46270-195">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="46270-196">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="46270-196">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="46270-197">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="46270-197">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="46270-198">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="46270-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="46270-199">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="46270-199">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="46270-201">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="46270-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="46270-202">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="46270-202">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="46270-204">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="46270-204">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="46270-206">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46270-206">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="46270-208">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="46270-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="46270-210">a.</span><span class="sxs-lookup"><span data-stu-id="46270-210">a.</span></span> <span data-ttu-id="46270-211">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="46270-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="46270-212">b.</span><span class="sxs-lookup"><span data-stu-id="46270-212">b.</span></span> <span data-ttu-id="46270-213">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="46270-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="46270-214">c.</span><span class="sxs-lookup"><span data-stu-id="46270-214">c.</span></span> <span data-ttu-id="46270-215">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="46270-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="46270-216">d.</span><span class="sxs-lookup"><span data-stu-id="46270-216">d.</span></span> <span data-ttu-id="46270-217">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="46270-217">Click **Create**.</span></span>
 
### <a name="creating-a-betterworks-test-user"></a><span data-ttu-id="46270-218">Skapa en testanvändare BetterWorks</span><span class="sxs-lookup"><span data-stu-id="46270-218">Creating a BetterWorks test user</span></span>

<span data-ttu-id="46270-219">I det här avsnittet skapar du en användare som kallas Britta Simon i BetterWorks.</span><span class="sxs-lookup"><span data-stu-id="46270-219">In this section, you create a user called Britta Simon in BetterWorks.</span></span> <span data-ttu-id="46270-220">Arbeta med [BetterWorks supportteam](mailto:support@betterworks.com) tooadd hello användare i hello BetterWorks plattform.</span><span class="sxs-lookup"><span data-stu-id="46270-220">Work with [BetterWorks support team](mailto:support@betterworks.com) tooadd hello users in hello BetterWorks platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="46270-221">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="46270-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="46270-222">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBetterWorks.</span><span class="sxs-lookup"><span data-stu-id="46270-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBetterWorks.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="46270-224">**tooassign Britta Simon tooBetterWorks utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="46270-224">**tooassign Britta Simon tooBetterWorks, perform hello following steps:**</span></span>

1. <span data-ttu-id="46270-225">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="46270-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="46270-227">Välj i listan med program hello **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="46270-227">In hello applications list, select **BetterWorks**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_app.png) 

3. <span data-ttu-id="46270-229">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="46270-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="46270-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="46270-231">Click **Add** button.</span></span> <span data-ttu-id="46270-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46270-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="46270-234">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="46270-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="46270-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46270-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="46270-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46270-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="46270-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46270-237">Testing single sign-on</span></span>

<span data-ttu-id="46270-238">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="46270-238">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="46270-239">Du bör få automatiskt inloggade tooyour BetterWorks programmet när du klickar på hello BetterWorks panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="46270-239">When you click hello BetterWorks tile in hello Access Panel, you should get automatically signed-on tooyour BetterWorks application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="46270-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="46270-240">Additional resources</span></span>

* [<span data-ttu-id="46270-241">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="46270-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="46270-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="46270-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_203.png

