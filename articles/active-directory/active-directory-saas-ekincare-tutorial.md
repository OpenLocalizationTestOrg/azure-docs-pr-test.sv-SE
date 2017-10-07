---
title: "Självstudier: Azure Active Directory-integrering med eKincare | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och eKincare."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 57f56d14-83cf-4cbb-b342-fac4fc60078f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 16129e3384132bb34744aadf088bb65f07ed7a96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ekincare"></a><span data-ttu-id="63dea-103">Självstudier: Azure Active Directory-integrering med eKincare</span><span class="sxs-lookup"><span data-stu-id="63dea-103">Tutorial: Azure Active Directory integration with eKincare</span></span>

<span data-ttu-id="63dea-104">I kursen får du lära dig hur toointegrate eKincare med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="63dea-104">In this tutorial, you learn how toointegrate eKincare with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="63dea-105">Integrera eKincare med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="63dea-105">Integrating eKincare with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="63dea-106">Du kan styra i Azure AD som har åtkomst till tooeKincare</span><span class="sxs-lookup"><span data-stu-id="63dea-106">You can control in Azure AD who has access tooeKincare</span></span>
- <span data-ttu-id="63dea-107">Du kan aktivera din användare tooautomatically get inloggade tooeKincare (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="63dea-107">You can enable your users tooautomatically get signed-on tooeKincare (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="63dea-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="63dea-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="63dea-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="63dea-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63dea-110">Krav</span><span class="sxs-lookup"><span data-stu-id="63dea-110">Prerequisites</span></span>

<span data-ttu-id="63dea-111">tooconfigure Azure AD-integrering med eKincare, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="63dea-111">tooconfigure Azure AD integration with eKincare, you need hello following items:</span></span>

- <span data-ttu-id="63dea-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="63dea-112">An Azure AD subscription</span></span>
- <span data-ttu-id="63dea-113">En eKincare enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="63dea-113">A eKincare single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="63dea-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="63dea-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="63dea-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="63dea-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="63dea-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="63dea-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="63dea-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="63dea-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="63dea-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="63dea-118">Scenario description</span></span>
<span data-ttu-id="63dea-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="63dea-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="63dea-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="63dea-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="63dea-121">Att lägga till eKincare från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="63dea-121">Adding eKincare from hello gallery</span></span>
2. <span data-ttu-id="63dea-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="63dea-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ekincare-from-hello-gallery"></a><span data-ttu-id="63dea-123">Att lägga till eKincare från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="63dea-123">Adding eKincare from hello gallery</span></span>
<span data-ttu-id="63dea-124">tooconfigure hello integrering av eKincare i Azure AD, behöver du tooadd eKincare hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="63dea-124">tooconfigure hello integration of eKincare into Azure AD, you need tooadd eKincare from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="63dea-125">**tooadd eKincare från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="63dea-125">**tooadd eKincare from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="63dea-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="63dea-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="63dea-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="63dea-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="63dea-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="63dea-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="63dea-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="63dea-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="63dea-133">Skriv i sökrutan hello **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="63dea-133">In hello search box, type **eKincare**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_search.png)

5. <span data-ttu-id="63dea-135">Markera hello resultat på panelen **eKincare**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="63dea-135">In hello results panel, select **eKincare**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="63dea-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="63dea-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="63dea-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med eKincare baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="63dea-138">In this section, you configure and test Azure AD single sign-on with eKincare based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="63dea-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i eKincare är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="63dea-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in eKincare is tooa user in Azure AD.</span></span> <span data-ttu-id="63dea-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i eKincare toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="63dea-140">In other words, a link relationship between an Azure AD user and hello related user in eKincare needs toobe established.</span></span>

<span data-ttu-id="63dea-141">I eKincare, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="63dea-141">In eKincare, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="63dea-142">tooconfigure och testa Azure AD enkel inloggning med eKincare, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="63dea-142">tooconfigure and test Azure AD single sign-on with eKincare, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="63dea-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="63dea-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="63dea-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="63dea-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="63dea-145">**[Skapa en testanvändare eKincare](#creating-a-ekincare-test-user)**  -toohave en motsvarighet för Britta Simon i eKincare som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="63dea-145">**[Creating a eKincare test user](#creating-a-ekincare-test-user)** - toohave a counterpart of Britta Simon in eKincare that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="63dea-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="63dea-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="63dea-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="63dea-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="63dea-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="63dea-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="63dea-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt eKincare program.</span><span class="sxs-lookup"><span data-stu-id="63dea-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your eKincare application.</span></span>

<span data-ttu-id="63dea-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med eKincare:**</span><span class="sxs-lookup"><span data-stu-id="63dea-150">**tooconfigure Azure AD single sign-on with eKincare, perform hello following steps:**</span></span>

1. <span data-ttu-id="63dea-151">I hello Azure-portalen på hello **eKincare** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="63dea-151">In hello Azure portal, on hello **eKincare** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="63dea-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="63dea-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_samlbase.png)

3. <span data-ttu-id="63dea-155">På hello **eKincare domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="63dea-155">On hello **eKincare Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_url.png)

    <span data-ttu-id="63dea-157">a.</span><span class="sxs-lookup"><span data-stu-id="63dea-157">a.</span></span> <span data-ttu-id="63dea-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.ekincare.com/`</span><span class="sxs-lookup"><span data-stu-id="63dea-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.ekincare.com/`</span></span>

    <span data-ttu-id="63dea-159">b.</span><span class="sxs-lookup"><span data-stu-id="63dea-159">b.</span></span> <span data-ttu-id="63dea-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.ekincare.com/hul/saml`</span><span class="sxs-lookup"><span data-stu-id="63dea-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<instancename>.ekincare.com/hul/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="63dea-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="63dea-161">These values are not real.</span></span> <span data-ttu-id="63dea-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="63dea-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="63dea-163">Kontakta [eKincare supportteamet](mailto:tech@ekincare.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="63dea-163">Contact [eKincare support team](mailto:tech@ekincare.com) tooget these values.</span></span>
 
4. <span data-ttu-id="63dea-164">eKincare program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="63dea-164">eKincare application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="63dea-165">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="63dea-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="63dea-166">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="63dea-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="63dea-167">hello följande skärmbild visar ett exempel på den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="63dea-167">hello following screenshot shows an example for this configuration.</span></span>

    <span data-ttu-id="63dea-168">hello anspråkets namn kommer alltid att **”employeeid”** och hello-värde som vi har mappat toouser.extensionattribute1 som innehåller hello employeeid för hello användare.</span><span class="sxs-lookup"><span data-stu-id="63dea-168">hello claim name will always be **"employeeid"** and hello value of which we have mapped toouser.extensionattribute1, that contains hello employeeid of hello user.</span></span> <span data-ttu-id="63dea-169">Hej andra två anspråk namn engångsfaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="63dea-169">hello other two claims' name i.e</span></span> <span data-ttu-id="63dea-170">**”organisations-ID”** och **”organisationsnamn”** är alltid samma och deras värden innehåller hello information hello organisationen av hello användare.</span><span class="sxs-lookup"><span data-stu-id="63dea-170">**"organizationid"** and **"organizationname"** will always be same and their values contain hello details of hello organization of hello user respectively.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/attribute.png)
    
5. <span data-ttu-id="63dea-172">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="63dea-172">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="63dea-173">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="63dea-173">Attribute Name</span></span> | <span data-ttu-id="63dea-174">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="63dea-174">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="63dea-175">EmployeeID</span><span class="sxs-lookup"><span data-stu-id="63dea-175">employeeid</span></span> | <span data-ttu-id="63dea-176">*User.extensionattribute1*</span><span class="sxs-lookup"><span data-stu-id="63dea-176">*user.extensionattribute1*</span></span> |
    | <span data-ttu-id="63dea-177">organisations-ID</span><span class="sxs-lookup"><span data-stu-id="63dea-177">organizationid</span></span> | <span data-ttu-id="63dea-178">*”uniquevalue”*</span><span class="sxs-lookup"><span data-stu-id="63dea-178">*"uniquevalue"*</span></span> |
    | <span data-ttu-id="63dea-179">organisationsnamn</span><span class="sxs-lookup"><span data-stu-id="63dea-179">organizationname</span></span> | <span data-ttu-id="63dea-180">*User.CompanyName*</span><span class="sxs-lookup"><span data-stu-id="63dea-180">*user.companyname*</span></span> |

    <span data-ttu-id="63dea-181">a.</span><span class="sxs-lookup"><span data-stu-id="63dea-181">a.</span></span> <span data-ttu-id="63dea-182">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="63dea-182">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/05.png)
    
    <span data-ttu-id="63dea-185">b.</span><span class="sxs-lookup"><span data-stu-id="63dea-185">b.</span></span> <span data-ttu-id="63dea-186">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="63dea-186">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="63dea-187">c.</span><span class="sxs-lookup"><span data-stu-id="63dea-187">c.</span></span> <span data-ttu-id="63dea-188">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="63dea-188">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="63dea-189">d.</span><span class="sxs-lookup"><span data-stu-id="63dea-189">d.</span></span> <span data-ttu-id="63dea-190">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="63dea-190">Click **Ok**</span></span>

6. <span data-ttu-id="63dea-191">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="63dea-191">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_certificate.png) 

7. <span data-ttu-id="63dea-193">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="63dea-193">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="63dea-195">tooconfigure enkel inloggning på **eKincare** sida, behöver du toosend hello hämtas **XML-Metadata för** för[eKincare supportteamet](mailto:tech@ekincare.com).</span><span class="sxs-lookup"><span data-stu-id="63dea-195">tooconfigure single sign-on on **eKincare** side, you need toosend hello downloaded **Metadata XML** too[eKincare support team](mailto:tech@ekincare.com).</span></span> <span data-ttu-id="63dea-196">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="63dea-196">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="63dea-197">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="63dea-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="63dea-198">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="63dea-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="63dea-199">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="63dea-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="63dea-200">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="63dea-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="63dea-201">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="63dea-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="63dea-203">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="63dea-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="63dea-204">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="63dea-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="63dea-206">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="63dea-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="63dea-208">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="63dea-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="63dea-210">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="63dea-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="63dea-212">a.</span><span class="sxs-lookup"><span data-stu-id="63dea-212">a.</span></span> <span data-ttu-id="63dea-213">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="63dea-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="63dea-214">b.</span><span class="sxs-lookup"><span data-stu-id="63dea-214">b.</span></span> <span data-ttu-id="63dea-215">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="63dea-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="63dea-216">c.</span><span class="sxs-lookup"><span data-stu-id="63dea-216">c.</span></span> <span data-ttu-id="63dea-217">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="63dea-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="63dea-218">d.</span><span class="sxs-lookup"><span data-stu-id="63dea-218">d.</span></span> <span data-ttu-id="63dea-219">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="63dea-219">Click **Create**.</span></span>
 
### <a name="creating-a-ekincare-test-user"></a><span data-ttu-id="63dea-220">Skapa en testanvändare eKincare</span><span class="sxs-lookup"><span data-stu-id="63dea-220">Creating a eKincare test user</span></span>

<span data-ttu-id="63dea-221">Programmet stöder bara i tid användaretablering och authentication-användare skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="63dea-221">Application supports Just in time user provisioning and after authentication users are created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="63dea-222">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="63dea-222">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="63dea-223">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooeKincare.</span><span class="sxs-lookup"><span data-stu-id="63dea-223">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooeKincare.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="63dea-225">**tooassign Britta Simon tooeKincare utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="63dea-225">**tooassign Britta Simon tooeKincare, perform hello following steps:**</span></span>

1. <span data-ttu-id="63dea-226">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="63dea-226">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="63dea-228">Välj i listan med program hello **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="63dea-228">In hello applications list, select **eKincare**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_app.png) 

3. <span data-ttu-id="63dea-230">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="63dea-230">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="63dea-232">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="63dea-232">Click **Add** button.</span></span> <span data-ttu-id="63dea-233">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="63dea-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="63dea-235">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="63dea-235">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="63dea-236">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="63dea-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="63dea-237">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="63dea-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="63dea-238">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="63dea-238">Testing single sign-on</span></span>

<span data-ttu-id="63dea-239">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="63dea-239">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="63dea-240">Du bör få automatiskt inloggade tooyour eKincare programmet när du klickar på hello eKincare panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="63dea-240">When you click hello eKincare tile in hello Access Panel, you should get automatically signed-on tooyour eKincare application.</span></span>
<span data-ttu-id="63dea-241">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="63dea-241">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="63dea-242">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="63dea-242">Additional resources</span></span>

* [<span data-ttu-id="63dea-243">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="63dea-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="63dea-244">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="63dea-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_203.png

