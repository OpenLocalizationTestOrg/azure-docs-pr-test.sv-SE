---
title: "Självstudier: Azure Active Directory-integrering med iLMS | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och iLMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: da0936de23afcd5a4213aa6f699165f9bfa82c35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a><span data-ttu-id="9a024-103">Självstudier: Azure Active Directory-integrering med iLMS</span><span class="sxs-lookup"><span data-stu-id="9a024-103">Tutorial: Azure Active Directory integration with iLMS</span></span>

<span data-ttu-id="9a024-104">I kursen får du lära dig hur toointegrate iLMS med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9a024-104">In this tutorial, you learn how toointegrate iLMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9a024-105">Integrera iLMS med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9a024-105">Integrating iLMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9a024-106">Du kan styra i Azure AD som har åtkomst till tooiLMS</span><span class="sxs-lookup"><span data-stu-id="9a024-106">You can control in Azure AD who has access tooiLMS</span></span>
- <span data-ttu-id="9a024-107">Du kan aktivera din användare tooautomatically get inloggade tooiLMS (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9a024-107">You can enable your users tooautomatically get signed-on tooiLMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9a024-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9a024-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9a024-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9a024-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a024-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9a024-110">Prerequisites</span></span>

<span data-ttu-id="9a024-111">tooconfigure Azure AD-integrering med iLMS, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9a024-111">tooconfigure Azure AD integration with iLMS, you need hello following items:</span></span>

- <span data-ttu-id="9a024-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9a024-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9a024-113">En iLMS enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="9a024-113">An iLMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9a024-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9a024-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9a024-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9a024-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9a024-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9a024-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="9a024-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9a024-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9a024-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9a024-118">Scenario description</span></span>
<span data-ttu-id="9a024-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9a024-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9a024-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9a024-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9a024-121">Att lägga till iLMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9a024-121">Adding iLMS from hello gallery</span></span>
2. <span data-ttu-id="9a024-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a024-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ilms-from-hello-gallery"></a><span data-ttu-id="9a024-123">Att lägga till iLMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9a024-123">Adding iLMS from hello gallery</span></span>
<span data-ttu-id="9a024-124">tooconfigure hello integrering av iLMS i Azure AD, behöver du tooadd iLMS hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9a024-124">tooconfigure hello integration of iLMS into Azure AD, you need tooadd iLMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9a024-125">**tooadd iLMS från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9a024-125">**tooadd iLMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9a024-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9a024-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9a024-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9a024-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9a024-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9a024-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9a024-131">tooadd nya program, klickar du på **nytt program** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a024-131">tooadd new application, click **New application** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9a024-133">Skriv i sökrutan hello **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="9a024-133">In hello search box, type **iLMS**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. <span data-ttu-id="9a024-135">Markera hello resultat på panelen **iLMS**, klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9a024-135">In hello results panel, select **iLMS**, then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9a024-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a024-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9a024-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med iLMS baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9a024-138">In this section, you configure and test Azure AD single sign-on with iLMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9a024-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i iLMS är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9a024-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in iLMS is tooa user in Azure AD.</span></span> <span data-ttu-id="9a024-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i iLMS toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9a024-140">In other words, a link relationship between an Azure AD user and hello related user in iLMS needs toobe established.</span></span>

<span data-ttu-id="9a024-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i iLMS.</span><span class="sxs-lookup"><span data-stu-id="9a024-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in iLMS.</span></span>

<span data-ttu-id="9a024-142">tooconfigure och testa Azure AD enkel inloggning med iLMS, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9a024-142">tooconfigure and test Azure AD single sign-on with iLMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9a024-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9a024-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9a024-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9a024-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9a024-145">**[Skapa en testanvändare iLMS](#creating-an-ilms-test-user)**  -toohave en motsvarighet för Britta Simon i iLMS som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="9a024-145">**[Creating an iLMS test user](#creating-an-ilms-test-user)** - toohave a counterpart of Britta Simon in iLMS that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="9a024-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9a024-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9a024-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9a024-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9a024-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a024-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9a024-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt iLMS program.</span><span class="sxs-lookup"><span data-stu-id="9a024-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your iLMS application.</span></span>

<span data-ttu-id="9a024-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med iLMS:**</span><span class="sxs-lookup"><span data-stu-id="9a024-150">**tooconfigure Azure AD single sign-on with iLMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="9a024-151">I hello Azure-portalen på hello **iLMS** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9a024-151">In hello Azure portal, on hello **iLMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9a024-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9a024-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. <span data-ttu-id="9a024-155">På hello **iLMS domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="9a024-155">On hello **iLMS Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    <span data-ttu-id="9a024-157">a.</span><span class="sxs-lookup"><span data-stu-id="9a024-157">a.</span></span> <span data-ttu-id="9a024-158">I hello **identifierare** textruta klistra in hello **identifierare** värdet du kopiera från **tjänstleverantör** SAML-inställningarna i iLMS administrationsportal.</span><span class="sxs-lookup"><span data-stu-id="9a024-158">In hello **Identifier** textbox, paste hello **Identifier** value you copy from **Service Provider** section of SAML settings in iLMS admin portal.</span></span>

    <span data-ttu-id="9a024-159">b.</span><span class="sxs-lookup"><span data-stu-id="9a024-159">b.</span></span> <span data-ttu-id="9a024-160">I hello **Reply URL** textruta klistra in hello **slutpunkt (URL)** värdet du kopiera från **tjänstleverantör** SAML-inställningarna i iLMS administrationsportal med hello följande mönstret`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="9a024-160">In hello **Reply URL** textbox, paste hello **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal having hello following pattern `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>

    >[!Note]
    ><span data-ttu-id="9a024-161">Den här 123456 är en exempel-värdet för identifieraren.</span><span class="sxs-lookup"><span data-stu-id="9a024-161">This '123456' is an example value of identifier.</span></span>

4. <span data-ttu-id="9a024-162">Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="9a024-162">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    <span data-ttu-id="9a024-164">I hello **inloggnings-URL** textruta klistra in hello **slutpunkt (URL)** värdet du kopiera från **tjänstleverantör** SAML inställningarna iLMS administrationsportalen som`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="9a024-164">In hello **Sign-on URL** textbox, paste hello **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal as `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>     

5. <span data-ttu-id="9a024-165">tooenable JIT-etablering, iLMS program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="9a024-165">tooenable JIT provisioning, iLMS application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="9a024-166">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="9a024-166">Configure hello following claims for this application.</span></span> <span data-ttu-id="9a024-167">Du kan hantera hello värden för attributen från hello **användarattribut** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="9a024-167">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="9a024-168">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="9a024-168">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/4.png)
    
    <span data-ttu-id="9a024-170">Skapa **avdelning, Region** och **Division** attribut och lägga till hello namn av dessa attribut i iLMS.</span><span class="sxs-lookup"><span data-stu-id="9a024-170">Create **Department, Region** and **Division** attributes and add hello name of these attributes in iLMS.</span></span> <span data-ttu-id="9a024-171">Dessa attribut som visas ovan krävs.</span><span class="sxs-lookup"><span data-stu-id="9a024-171">All these attributes shown above are required.</span></span>    

    > [!NOTE] 
    > <span data-ttu-id="9a024-172">Du har tooenable **skapa Un-recognized användarkonto** i iLMS toomap attributen.</span><span class="sxs-lookup"><span data-stu-id="9a024-172">You have tooenable **Create Un-recognized User Account** in iLMS toomap these attributes.</span></span> <span data-ttu-id="9a024-173">Följ instruktionerna för hello [här](http://support.inspiredelearning.com/customer/portal/articles/2204526) tooget en idé på hello attribut konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9a024-173">Follow hello instructions [here](http://support.inspiredelearning.com/customer/portal/articles/2204526) tooget an idea on hello attributes configuration.</span></span>

6. <span data-ttu-id="9a024-174">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a024-174">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="9a024-175">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="9a024-175">Attribute Name</span></span> | <span data-ttu-id="9a024-176">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="9a024-176">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="9a024-177">Division</span><span class="sxs-lookup"><span data-stu-id="9a024-177">division</span></span> | <span data-ttu-id="9a024-178">User.Department</span><span class="sxs-lookup"><span data-stu-id="9a024-178">user.department</span></span> |
    | <span data-ttu-id="9a024-179">Region</span><span class="sxs-lookup"><span data-stu-id="9a024-179">region</span></span> | <span data-ttu-id="9a024-180">User.state</span><span class="sxs-lookup"><span data-stu-id="9a024-180">user.state</span></span> |
    | <span data-ttu-id="9a024-181">Avdelning</span><span class="sxs-lookup"><span data-stu-id="9a024-181">department</span></span> | <span data-ttu-id="9a024-182">User.jobtitle</span><span class="sxs-lookup"><span data-stu-id="9a024-182">user.jobtitle</span></span> |

    <span data-ttu-id="9a024-183">a.</span><span class="sxs-lookup"><span data-stu-id="9a024-183">a.</span></span> <span data-ttu-id="9a024-184">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a024-184">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    <span data-ttu-id="9a024-187">b.</span><span class="sxs-lookup"><span data-stu-id="9a024-187">b.</span></span> <span data-ttu-id="9a024-188">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="9a024-188">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="9a024-189">c.</span><span class="sxs-lookup"><span data-stu-id="9a024-189">c.</span></span> <span data-ttu-id="9a024-190">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="9a024-190">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="9a024-191">d.</span><span class="sxs-lookup"><span data-stu-id="9a024-191">d.</span></span> <span data-ttu-id="9a024-192">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="9a024-192">Click **Ok**</span></span>

7. <span data-ttu-id="9a024-193">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="9a024-193">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. <span data-ttu-id="9a024-195">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9a024-195">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="9a024-197">Logga in tooyour i ett annat webbläsarfönster **iLMS administrationsportalen** som administratör.</span><span class="sxs-lookup"><span data-stu-id="9a024-197">In a different web browser window, log in tooyour **iLMS admin portal** as an administrator.</span></span>

10. <span data-ttu-id="9a024-198">Klicka på **SSO:SAML** under **inställningar** tooopen SAML-inställningar på fliken och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a024-198">Click **SSO:SAML** under **Settings** tab tooopen SAML settings and perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/1.png) 

    <span data-ttu-id="9a024-200">a.</span><span class="sxs-lookup"><span data-stu-id="9a024-200">a.</span></span> <span data-ttu-id="9a024-201">Expandera hello **tjänstleverantör** avsnitt och kopiera hello **identifierare** och **slutpunkt (URL)** värde.</span><span class="sxs-lookup"><span data-stu-id="9a024-201">Expand hello **Service Provider** section and copy hello **Identifier** and **Endpoint (URL)** value.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/2.png) 

    <span data-ttu-id="9a024-203">b.</span><span class="sxs-lookup"><span data-stu-id="9a024-203">b.</span></span> <span data-ttu-id="9a024-204">Under **identitetsleverantör** klickar du på **importera Metadata**.</span><span class="sxs-lookup"><span data-stu-id="9a024-204">Under **Identity Provider** section, click **Import Metadata**.</span></span>
    
    <span data-ttu-id="9a024-205">c.</span><span class="sxs-lookup"><span data-stu-id="9a024-205">c.</span></span> <span data-ttu-id="9a024-206">Välj hello **Metadata** hämta filen från Azure-portalen från **SAML-signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9a024-206">Select hello **Metadata** file downloaded from Azure Portal from **SAML Signing Certificate** section.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    <span data-ttu-id="9a024-208">d.</span><span class="sxs-lookup"><span data-stu-id="9a024-208">d.</span></span> <span data-ttu-id="9a024-209">Om du vill tooenable JIT etablering toocreate iLMS konton för un-känna igen användare, följa nedanstående steg:</span><span class="sxs-lookup"><span data-stu-id="9a024-209">If you want tooenable JIT provisioning toocreate iLMS accounts for un-recognize users, follow below steps:</span></span>
        
       - <span data-ttu-id="9a024-210">Kontrollera **skapa icke godkänd användarkonto**.</span><span class="sxs-lookup"><span data-stu-id="9a024-210">Check **Create Un-recognized User Account**.</span></span>
       
       ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  <span data-ttu-id="9a024-212">Mappa hello attribut i Azure AD med hello attribut i iLMS.</span><span class="sxs-lookup"><span data-stu-id="9a024-212">Map hello attributes in Azure AD with hello attributes in iLMS.</span></span> <span data-ttu-id="9a024-213">Ange hello attribut namn eller hello standardvärdet i hello attributkolumnen.</span><span class="sxs-lookup"><span data-stu-id="9a024-213">In hello attribute column, specify hello attributes name or hello default value.</span></span>

    <span data-ttu-id="9a024-214">e.</span><span class="sxs-lookup"><span data-stu-id="9a024-214">e.</span></span> <span data-ttu-id="9a024-215">Gå för**affärsregler** fliken och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a024-215">Go too**Business Rules** tab and perform hello following steps:</span></span> 
        
       ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/5.png)

       - <span data-ttu-id="9a024-217">Kontrollera **skapa Un-recognized regioner, avdelningar och avdelningar** toocreate regioner, avdelningar och avdelningar som inte redan finns för närvarande hello av enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9a024-217">Check **Create Un-recognized Regions, Divisions and Departments** toocreate Regions, Divisions, and Departments that do not already exist at hello time of Single Sign-on.</span></span>
        
       - <span data-ttu-id="9a024-218">Kontrollera **uppdatering användarens profil under inloggning** toospecify om hello användarens profil uppdateras med varje enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9a024-218">Check **Update User Profile During Sign-in** toospecify whether hello user’s profile is updated with each Single Sign-on.</span></span> 
        
       - <span data-ttu-id="9a024-219">Om hello **”uppdatering tomma värden för ej obligatoriska fält i användarprofil”** alternativet är markerat, valfri profil fält som är tom vid inloggning kommer även att hello iLMS användarprofil toocontain tomma värden för fälten.</span><span class="sxs-lookup"><span data-stu-id="9a024-219">If hello **“Update Blank Values for Non Mandatory Fields in User Profile”** option is checked, optional profile fields that are blank upon sign in will also cause hello user’s iLMS profile toocontain blank values for those fields.</span></span>
        
       - <span data-ttu-id="9a024-220">Kontrollera **skicka fel e-postmeddelande** och ange hello e-postadress för hello användare där du vill att tooreceive hello fel e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="9a024-220">Check **Send Error Notification Email** and enter hello email of hello user where you want tooreceive hello error notification email.</span></span>

11. <span data-ttu-id="9a024-221">Klicka på **spara** knappen toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="9a024-221">Click **Save** button toosave hello settings.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> <span data-ttu-id="9a024-223">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="9a024-223">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9a024-224">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="9a024-224">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9a024-225">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9a024-225">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
    
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9a024-226">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a024-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="9a024-227">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9a024-227">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9a024-229">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9a024-229">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9a024-230">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9a024-230">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9a024-232">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="9a024-232">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9a024-234">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a024-234">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9a024-236">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a024-236">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9a024-238">a.</span><span class="sxs-lookup"><span data-stu-id="9a024-238">a.</span></span> <span data-ttu-id="9a024-239">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9a024-239">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9a024-240">b.</span><span class="sxs-lookup"><span data-stu-id="9a024-240">b.</span></span> <span data-ttu-id="9a024-241">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9a024-241">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9a024-242">c.</span><span class="sxs-lookup"><span data-stu-id="9a024-242">c.</span></span> <span data-ttu-id="9a024-243">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9a024-243">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9a024-244">d.</span><span class="sxs-lookup"><span data-stu-id="9a024-244">d.</span></span> <span data-ttu-id="9a024-245">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9a024-245">Click **Create**.</span></span>
 
### <a name="creating-an-ilms-test-user"></a><span data-ttu-id="9a024-246">Skapa en testanvändare iLMS</span><span class="sxs-lookup"><span data-stu-id="9a024-246">Creating an iLMS test user</span></span>

<span data-ttu-id="9a024-247">Programmet stöder bara i tid användaretablering och authentication-användare skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9a024-247">Application supports Just in time user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="9a024-248">JIT fungerar om du har klickat på hello **skapa Un-recognized användarkonto** kryssrutan under SAML Konfigurationsinställningen på administrationsportalen för iLMS.</span><span class="sxs-lookup"><span data-stu-id="9a024-248">JIT will work, if you have clicked hello **Create Un-recognized User Account** checkbox during SAML configuration setting at iLMS admin portal.</span></span>

<span data-ttu-id="9a024-249">Följ nedanstående steg om du behöver toocreate en användare manuellt:</span><span class="sxs-lookup"><span data-stu-id="9a024-249">If you need toocreate an user manually, then follow below steps :</span></span>

1. <span data-ttu-id="9a024-250">Logga in tooyour iLMS företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="9a024-250">Log in tooyour iLMS company site as an administrator.</span></span>

2. <span data-ttu-id="9a024-251">Klicka på **”registrera användare”** under **användare** fliken tooopen **registrera användaren** sidan.</span><span class="sxs-lookup"><span data-stu-id="9a024-251">Click **“Register User”** under **Users** tab tooopen **Register User** page.</span></span> 
   
   ![Lägga till medarbetare](./media/active-directory-saas-ilms-tutorial/3.png)

3. <span data-ttu-id="9a024-253">På hello **”registrera användare”** utför hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="9a024-253">On hello **“Register User”** page, perform hello following steps.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    <span data-ttu-id="9a024-255">a.</span><span class="sxs-lookup"><span data-stu-id="9a024-255">a.</span></span> <span data-ttu-id="9a024-256">I hello **Förnamn** textruta typnamn hello första Britta.</span><span class="sxs-lookup"><span data-stu-id="9a024-256">In hello **First Name** textbox, type hello first name Britta.</span></span>
   
    <span data-ttu-id="9a024-257">b.</span><span class="sxs-lookup"><span data-stu-id="9a024-257">b.</span></span> <span data-ttu-id="9a024-258">I hello **efternamn** textruta typen hello efternamn Simon.</span><span class="sxs-lookup"><span data-stu-id="9a024-258">In hello **Last Name** textbox, type hello last name Simon.</span></span>

    <span data-ttu-id="9a024-259">c.</span><span class="sxs-lookup"><span data-stu-id="9a024-259">c.</span></span> <span data-ttu-id="9a024-260">I hello **e-post-ID** textruta typen hello e-postadress för Britta Simon konto.</span><span class="sxs-lookup"><span data-stu-id="9a024-260">In hello **Email ID** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="9a024-261">d.</span><span class="sxs-lookup"><span data-stu-id="9a024-261">d.</span></span> <span data-ttu-id="9a024-262">I hello **Region** listrutan, Välj hello värde för region.</span><span class="sxs-lookup"><span data-stu-id="9a024-262">In hello **Region** dropdown, select hello value for region.</span></span>

    <span data-ttu-id="9a024-263">e.</span><span class="sxs-lookup"><span data-stu-id="9a024-263">e.</span></span> <span data-ttu-id="9a024-264">I hello **Division** listrutan, Välj hello värde för delning.</span><span class="sxs-lookup"><span data-stu-id="9a024-264">In hello **Division** dropdown, select hello value for division.</span></span>

    <span data-ttu-id="9a024-265">f.</span><span class="sxs-lookup"><span data-stu-id="9a024-265">f.</span></span> <span data-ttu-id="9a024-266">I hello **avdelning** listrutan, Välj hello värde för avdelning.</span><span class="sxs-lookup"><span data-stu-id="9a024-266">In hello **Department** dropdown, select hello value for department.</span></span>

    <span data-ttu-id="9a024-267">g.</span><span class="sxs-lookup"><span data-stu-id="9a024-267">g.</span></span> <span data-ttu-id="9a024-268">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9a024-268">Click **Save**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9a024-269">Du kan skicka e-post toouser för registrering genom att välja **skicka e-post från registrering** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="9a024-269">You can send registration mail toouser by selecting **Send Registration Mail** checkbox.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9a024-270">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a024-270">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9a024-271">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooiLMS sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="9a024-271">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooiLMS.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9a024-273">**tooassign Britta Simon tooiLMS utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9a024-273">**tooassign Britta Simon tooiLMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="9a024-274">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9a024-274">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9a024-276">Välj i listan med program hello **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="9a024-276">In hello applications list, select **iLMS**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. <span data-ttu-id="9a024-278">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9a024-278">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9a024-280">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9a024-280">Click **Add** button.</span></span> <span data-ttu-id="9a024-281">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a024-281">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9a024-283">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9a024-283">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9a024-284">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a024-284">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9a024-285">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a024-285">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9a024-286">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a024-286">Testing single sign-on</span></span>

<span data-ttu-id="9a024-287">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9a024-287">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9a024-288">Du bör få automatiskt inloggade tooyour iLMS programmet när du klickar på hello iLMS panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9a024-288">When you click hello iLMS tile in hello Access Panel, you should get automatically signed-on tooyour iLMS application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9a024-289">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9a024-289">Additional resources</span></span>

* [<span data-ttu-id="9a024-290">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9a024-290">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9a024-291">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9a024-291">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png

