---
title: "Självstudier: Azure Active Directory-integrering med svart tavla Läs | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och svart tavla lära dig."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0b8ca505-61ea-487c-9a3e-fa50c936df0c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jeedes
ms.openlocfilehash: e94cdd6eaf876d4f66bdd783c442dc468f104e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a><span data-ttu-id="6b486-103">Självstudier: Azure Active Directory-integrering med svart tavla Läs</span><span class="sxs-lookup"><span data-stu-id="6b486-103">Tutorial: Azure Active Directory integration with Blackboard Learn</span></span>

<span data-ttu-id="6b486-104">I kursen får du lära dig hur toointegrate svart tavla Läs med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6b486-104">In this tutorial, you learn how toointegrate Blackboard Learn with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6b486-105">Integrera svart tavla Läs med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6b486-105">Integrating Blackboard Learn with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6b486-106">Du kan styra i Azure AD som har åtkomst tooBlackboard Läs</span><span class="sxs-lookup"><span data-stu-id="6b486-106">You can control in Azure AD who has access tooBlackboard Learn</span></span>
- <span data-ttu-id="6b486-107">Du kan aktivera din användare tooautomatically get inloggade tooBlackboard Läs (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6b486-107">You can enable your users tooautomatically get signed-on tooBlackboard Learn (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6b486-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6b486-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6b486-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6b486-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b486-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6b486-110">Prerequisites</span></span>

<span data-ttu-id="6b486-111">tooconfigure Azure AD-integrering med svart tavla Läs måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="6b486-111">tooconfigure Azure AD integration with Blackboard Learn, you need hello following items:</span></span>

- <span data-ttu-id="6b486-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6b486-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6b486-113">En svart tavla Läs enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="6b486-113">A Blackboard Learn single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6b486-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6b486-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6b486-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6b486-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6b486-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6b486-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6b486-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6b486-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6b486-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6b486-118">Scenario description</span></span>
<span data-ttu-id="6b486-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6b486-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6b486-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6b486-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6b486-121">Lägger till svart tavla Läs från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6b486-121">Adding Blackboard Learn from hello gallery</span></span>
2. <span data-ttu-id="6b486-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b486-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-blackboard-learn-from-hello-gallery"></a><span data-ttu-id="6b486-123">Lägger till svart tavla Läs från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6b486-123">Adding Blackboard Learn from hello gallery</span></span>
<span data-ttu-id="6b486-124">tooconfigure hello integrering av svart tavla Lär dig i Azure AD, behöver du tooadd svart tavla Läs hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="6b486-124">tooconfigure hello integration of Blackboard Learn into Azure AD, you need tooadd Blackboard Learn from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6b486-125">**tooadd svart tavla Läs från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6b486-125">**tooadd Blackboard Learn from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b486-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6b486-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6b486-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6b486-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6b486-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="6b486-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6b486-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b486-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="6b486-133">Skriv i sökrutan hello **svart tavla Läs**.</span><span class="sxs-lookup"><span data-stu-id="6b486-133">In hello search box, type **Blackboard Learn**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_search.png)

5. <span data-ttu-id="6b486-135">Markera hello resultat på panelen **svart tavla Läs**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="6b486-135">In hello results panel, select **Blackboard Learn**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6b486-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b486-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6b486-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med svart tavla Läs baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6b486-138">In this section, you configure and test Azure AD single sign-on with Blackboard Learn based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6b486-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i svart tavla Läs är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6b486-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Blackboard Learn is tooa user in Azure AD.</span></span> <span data-ttu-id="6b486-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i svart tavla Läs toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="6b486-140">In other words, a link relationship between an Azure AD user and hello related user in Blackboard Learn needs toobe established.</span></span>

<span data-ttu-id="6b486-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i svart tavla Läs.</span><span class="sxs-lookup"><span data-stu-id="6b486-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Blackboard Learn.</span></span>

<span data-ttu-id="6b486-142">tooconfigure och testa Azure AD enkel inloggning med svart tavla Läs, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6b486-142">tooconfigure and test Azure AD single sign-on with Blackboard Learn, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6b486-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6b486-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6b486-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6b486-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6b486-145">**[Skapa en svart tavla Läs testanvändare](#creating-a-blackboard-learn-test-user)**  -toohave en motsvarighet för Britta Simon i svart tavla Läs som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6b486-145">**[Creating a Blackboard Learn test user](#creating-a-blackboard-learn-test-user)** - toohave a counterpart of Britta Simon in Blackboard Learn that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6b486-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6b486-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6b486-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6b486-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6b486-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b486-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6b486-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt svart tavla Läs program.</span><span class="sxs-lookup"><span data-stu-id="6b486-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Blackboard Learn application.</span></span>

<span data-ttu-id="6b486-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med svart tavla Läs:**</span><span class="sxs-lookup"><span data-stu-id="6b486-150">**tooconfigure Azure AD single sign-on with Blackboard Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b486-151">I hello Azure-portalen på hello **svart tavla Läs** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6b486-151">In hello Azure portal, on hello **Blackboard Learn** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6b486-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6b486-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_samlbase.png)

3. <span data-ttu-id="6b486-155">På hello **svart tavla Läs domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6b486-155">On hello **Blackboard Learn Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_url.png)

    <span data-ttu-id="6b486-157">a.</span><span class="sxs-lookup"><span data-stu-id="6b486-157">a.</span></span> <span data-ttu-id="6b486-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.blackboard.com/`</span><span class="sxs-lookup"><span data-stu-id="6b486-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.blackboard.com/`</span></span>

    <span data-ttu-id="6b486-159">b.</span><span class="sxs-lookup"><span data-stu-id="6b486-159">b.</span></span> <span data-ttu-id="6b486-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span><span class="sxs-lookup"><span data-stu-id="6b486-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="6b486-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="6b486-161">These values are not real.</span></span> <span data-ttu-id="6b486-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="6b486-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6b486-163">Kontakta [svart tavla Läs klienten supportteamet](https://www.blackboard.com/support/index.aspx) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="6b486-163">Contact [Blackboard Learn Client support team](https://www.blackboard.com/support/index.aspx) tooget these values.</span></span> 

4. <span data-ttu-id="6b486-164">Svart tavla Läs program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="6b486-164">Blackboard Learn application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="6b486-165">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="6b486-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="6b486-166">Du kan hantera hello värden för attributen från hello **användarattribut** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="6b486-166">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span>
 <span data-ttu-id="6b486-167">hello följande skärmbild visar ett exempel på hur den.</span><span class="sxs-lookup"><span data-stu-id="6b486-167">hello following screenshot shows an example about it.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="6b486-169">I hello **användarattribut** avsnittet på **enkel inloggning** dialogrutan Konfigurera SAML-token attribut enligt hello bild och utföra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="6b486-169">In hello **User Attributes** section on **Single sign-on** dialog, configure SAML token attributes as shown in hello image and perform hello following steps.</span></span> <span data-ttu-id="6b486-170">Vi har mappats hello Userprincipalname som hello unika användarattribut här men du kan mappa den toohello lämpligt värde, som unikt särskiljer hello användare i hello organisation och som mappar tooBlackboard Läs username-fält.</span><span class="sxs-lookup"><span data-stu-id="6b486-170">We have mapped hello Userprincipalname as hello unique user attribute here but you can map it toohello appropriate value, which uniquely distinguishes hello user in hello organization and that maps tooBlackboard Learn username field.</span></span>
           
    | <span data-ttu-id="6b486-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="6b486-171">Attribute Name</span></span> | <span data-ttu-id="6b486-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="6b486-172">Attribute Value</span></span> |   
    | ---------------| ----------------|
    | <span data-ttu-id="6b486-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span><span class="sxs-lookup"><span data-stu-id="6b486-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span></span> |<span data-ttu-id="6b486-174">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="6b486-174">user.userprincipalname</span></span> |

    <span data-ttu-id="6b486-175">a.</span><span class="sxs-lookup"><span data-stu-id="6b486-175">a.</span></span> <span data-ttu-id="6b486-176">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b486-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="6b486-179">b.</span><span class="sxs-lookup"><span data-stu-id="6b486-179">b.</span></span> <span data-ttu-id="6b486-180">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="6b486-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="6b486-181">c.</span><span class="sxs-lookup"><span data-stu-id="6b486-181">c.</span></span> <span data-ttu-id="6b486-182">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="6b486-182">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="6b486-183">d.</span><span class="sxs-lookup"><span data-stu-id="6b486-183">d.</span></span> <span data-ttu-id="6b486-184">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b486-184">Click **Ok**.</span></span>

4. <span data-ttu-id="6b486-185">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="6b486-185">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_certificate.png)

7. <span data-ttu-id="6b486-187">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6b486-187">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="6b486-189">På hello **svart tavla Läs Configuration** klickar du på **konfigurera svart tavla Läs** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6b486-189">On hello **Blackboard Learn Configuration** section, click **Configure Blackboard Learn** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6b486-190">Kopiera hello **SAML enhets-ID** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6b486-190">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_configure.png) 

9. <span data-ttu-id="6b486-192">tooconfigure enkel inloggning på **svart tavla Läs** sida, behöver du toosend hello hämtas **XML-Metadata för** och **SAML enhets-ID** för[svart tavla Läs stöd för](https://www.blackboard.com/support/index.aspx).</span><span class="sxs-lookup"><span data-stu-id="6b486-192">tooconfigure single sign-on on **Blackboard Learn** side, you need toosend hello downloaded **Metadata XML** and **SAML Entity ID** too[Blackboard Learn support](https://www.blackboard.com/support/index.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="6b486-193">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="6b486-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6b486-194">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="6b486-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6b486-195">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6b486-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6b486-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6b486-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="6b486-197">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6b486-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6b486-199">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6b486-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b486-200">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6b486-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6b486-202">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6b486-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6b486-204">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b486-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6b486-206">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6b486-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6b486-208">a.</span><span class="sxs-lookup"><span data-stu-id="6b486-208">a.</span></span> <span data-ttu-id="6b486-209">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6b486-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6b486-210">b.</span><span class="sxs-lookup"><span data-stu-id="6b486-210">b.</span></span> <span data-ttu-id="6b486-211">I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6b486-211">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="6b486-212">c.</span><span class="sxs-lookup"><span data-stu-id="6b486-212">c.</span></span> <span data-ttu-id="6b486-213">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6b486-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6b486-214">d.</span><span class="sxs-lookup"><span data-stu-id="6b486-214">d.</span></span> <span data-ttu-id="6b486-215">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6b486-215">Click **Create**.</span></span>
 
### <a name="creating-a-blackboard-learn-test-user"></a><span data-ttu-id="6b486-216">Skapa en svart tavla Läs testanvändare</span><span class="sxs-lookup"><span data-stu-id="6b486-216">Creating a Blackboard Learn test user</span></span>
<span data-ttu-id="6b486-217">I det här avsnittet kan du skapa en användare som kallas Britta Simon i svart tavla Läs.</span><span class="sxs-lookup"><span data-stu-id="6b486-217">In this section, you create a user called Britta Simon in Blackboard Learn.</span></span> 

<span data-ttu-id="6b486-218">Svart tavla Läs programmet stöder bara i tid användaretablering.</span><span class="sxs-lookup"><span data-stu-id="6b486-218">Blackboard Learn application support just in time user provisioning.</span></span> <span data-ttu-id="6b486-219">Kontrollera att du har konfigurerat hello anspråk enligt beskrivningen i avsnittet hello  **[konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**</span><span class="sxs-lookup"><span data-stu-id="6b486-219">Make sure that you have configured hello claims as described in hello section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**</span></span>
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6b486-220">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="6b486-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6b486-221">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBlackboard Läs.</span><span class="sxs-lookup"><span data-stu-id="6b486-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBlackboard Learn.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6b486-223">**tooassign Britta Simon tooBlackboard Läs, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="6b486-223">**tooassign Britta Simon tooBlackboard Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b486-224">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6b486-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6b486-226">Välj i listan med program hello **svart tavla Läs**.</span><span class="sxs-lookup"><span data-stu-id="6b486-226">In hello applications list, select **Blackboard Learn**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_app.png) 

3. <span data-ttu-id="6b486-228">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6b486-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6b486-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6b486-230">Click **Add** button.</span></span> <span data-ttu-id="6b486-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b486-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6b486-233">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="6b486-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6b486-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b486-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6b486-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b486-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6b486-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b486-236">Testing single sign-on</span></span>

<span data-ttu-id="6b486-237">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6b486-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6b486-238">Du bör få automatiskt inloggade tooyour svart tavla Läs programmet när du klickar på hello svart tavla Läs panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6b486-238">When you click hello Blackboard Learn tile in hello Access Panel, you should get automatically signed-on tooyour Blackboard Learn application.</span></span> <span data-ttu-id="6b486-239">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6b486-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6b486-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6b486-240">Additional resources</span></span>

* [<span data-ttu-id="6b486-241">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6b486-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6b486-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6b486-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_203.png

