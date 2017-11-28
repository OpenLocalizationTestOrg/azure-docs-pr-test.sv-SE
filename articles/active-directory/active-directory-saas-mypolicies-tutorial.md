---
title: "Självstudier: Azure Active Directory-integrering med myPolicies | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och myPolicies."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bf79e858-1dfb-4ab3-a6df-74b2d5a878d2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: jeedes
ms.openlocfilehash: d8890457ebdb1b80e0d3126d4210e6265ae7f1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mypolicies"></a><span data-ttu-id="fab5f-103">Självstudier: Azure Active Directory-integrering med myPolicies</span><span class="sxs-lookup"><span data-stu-id="fab5f-103">Tutorial: Azure Active Directory integration with myPolicies</span></span>

<span data-ttu-id="fab5f-104">I kursen får du lära dig hur toointegrate myPolicies med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="fab5f-104">In this tutorial, you learn how toointegrate myPolicies with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fab5f-105">Integrera myPolicies med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="fab5f-105">Integrating myPolicies with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="fab5f-106">Du kan styra i Azure AD som har åtkomst till toomyPolicies</span><span class="sxs-lookup"><span data-stu-id="fab5f-106">You can control in Azure AD who has access toomyPolicies</span></span>
- <span data-ttu-id="fab5f-107">Du kan aktivera din användare tooautomatically get inloggade toomyPolicies (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="fab5f-107">You can enable your users tooautomatically get signed-on toomyPolicies (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fab5f-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fab5f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="fab5f-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fab5f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fab5f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="fab5f-110">Prerequisites</span></span>

<span data-ttu-id="fab5f-111">tooconfigure Azure AD-integrering med myPolicies, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="fab5f-111">tooconfigure Azure AD integration with myPolicies, you need hello following items:</span></span>

- <span data-ttu-id="fab5f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="fab5f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fab5f-113">En myPolicies enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="fab5f-113">A myPolicies single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fab5f-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="fab5f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fab5f-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="fab5f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fab5f-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="fab5f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fab5f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fab5f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fab5f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="fab5f-118">Scenario description</span></span>
<span data-ttu-id="fab5f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="fab5f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fab5f-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="fab5f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fab5f-121">Att lägga till myPolicies från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="fab5f-121">Adding myPolicies from hello gallery</span></span>
2. <span data-ttu-id="fab5f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fab5f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mypolicies-from-hello-gallery"></a><span data-ttu-id="fab5f-123">Att lägga till myPolicies från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="fab5f-123">Adding myPolicies from hello gallery</span></span>
<span data-ttu-id="fab5f-124">tooconfigure hello integrering av myPolicies i Azure AD, behöver du tooadd myPolicies hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="fab5f-124">tooconfigure hello integration of myPolicies into Azure AD, you need tooadd myPolicies from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="fab5f-125">**tooadd myPolicies från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fab5f-125">**tooadd myPolicies from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="fab5f-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fab5f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fab5f-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="fab5f-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="fab5f-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fab5f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="fab5f-133">Skriv i sökrutan hello **myPolicies**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-133">In hello search box, type **myPolicies**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_search.png)

5. <span data-ttu-id="fab5f-135">Markera hello resultat på panelen **myPolicies**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="fab5f-135">In hello results panel, select **myPolicies**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fab5f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fab5f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fab5f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med myPolicies baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="fab5f-138">In this section, you configure and test Azure AD single sign-on with myPolicies based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fab5f-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i myPolicies är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fab5f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in myPolicies is tooa user in Azure AD.</span></span> <span data-ttu-id="fab5f-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i myPolicies toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="fab5f-140">In other words, a link relationship between an Azure AD user and hello related user in myPolicies needs toobe established.</span></span>

<span data-ttu-id="fab5f-141">I myPolicies, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="fab5f-141">In myPolicies, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="fab5f-142">tooconfigure och testa Azure AD enkel inloggning med myPolicies, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="fab5f-142">tooconfigure and test Azure AD single sign-on with myPolicies, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="fab5f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="fab5f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="fab5f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fab5f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fab5f-145">**[Skapa en testanvändare myPolicies](#creating-a-mypolicies-test-user)**  -toohave en motsvarighet för Britta Simon i myPolicies som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="fab5f-145">**[Creating a myPolicies test user](#creating-a-mypolicies-test-user)** - toohave a counterpart of Britta Simon in myPolicies that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="fab5f-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fab5f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fab5f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="fab5f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fab5f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fab5f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fab5f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt myPolicies program.</span><span class="sxs-lookup"><span data-stu-id="fab5f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your myPolicies application.</span></span>

<span data-ttu-id="fab5f-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med myPolicies:**</span><span class="sxs-lookup"><span data-stu-id="fab5f-150">**tooconfigure Azure AD single sign-on with myPolicies, perform hello following steps:**</span></span>

1. <span data-ttu-id="fab5f-151">I hello Azure-portalen på hello **myPolicies** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-151">In hello Azure portal, on hello **myPolicies** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="fab5f-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fab5f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_samlbase.png)

3. <span data-ttu-id="fab5f-155">På hello **myPolicies domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fab5f-155">On hello **myPolicies Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_url.png)

    <span data-ttu-id="fab5f-157">a.</span><span class="sxs-lookup"><span data-stu-id="fab5f-157">a.</span></span> <span data-ttu-id="fab5f-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.mypolicies.com/`</span><span class="sxs-lookup"><span data-stu-id="fab5f-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.mypolicies.com/`</span></span>

    <span data-ttu-id="fab5f-159">b.</span><span class="sxs-lookup"><span data-stu-id="fab5f-159">b.</span></span> <span data-ttu-id="fab5f-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.mypolicies.com/users/auth/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="fab5f-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<tenantname>.mypolicies.com/users/auth/saml/callback`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fab5f-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="fab5f-161">These values are not real.</span></span> <span data-ttu-id="fab5f-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="fab5f-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="fab5f-163">Kontakta [myPolicies supportteam](mailto:support@mypolicies.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="fab5f-163">Contact [myPolicies support team](mailto:support@mypolicies.com) tooget these values.</span></span>

4. <span data-ttu-id="fab5f-164">Hej myPolicies program förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="fab5f-164">hello myPolicies application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="fab5f-165">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="fab5f-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="fab5f-166">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="fab5f-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="fab5f-167">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="fab5f-167">hello following screenshot shows an example for this.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_attribute.png)

5. <span data-ttu-id="fab5f-169">Klicka på **visa och redigera andra användarattribut** kryssrutan i hello **användarattribut** avsnittet tooexpand hello attribut.</span><span class="sxs-lookup"><span data-stu-id="fab5f-169">Click **View and edit all other user attributes** checkbox in hello **User Attributes** section tooexpand hello attributes.</span></span> <span data-ttu-id="fab5f-170">Utför följande steg på varje hello visas attribut - hello</span><span class="sxs-lookup"><span data-stu-id="fab5f-170">Perform hello following steps on each of hello displayed attributes-</span></span>

    | <span data-ttu-id="fab5f-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="fab5f-171">Attribute Name</span></span> | <span data-ttu-id="fab5f-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="fab5f-172">Attribute Value</span></span> |
    | ------------------- | ---------- |
    | <span data-ttu-id="fab5f-173">givenName</span><span class="sxs-lookup"><span data-stu-id="fab5f-173">givenname</span></span> | <span data-ttu-id="fab5f-174">User.givenName</span><span class="sxs-lookup"><span data-stu-id="fab5f-174">user.givenname</span></span> |
    | <span data-ttu-id="fab5f-175">Efternamn</span><span class="sxs-lookup"><span data-stu-id="fab5f-175">surname</span></span> | <span data-ttu-id="fab5f-176">User.surname</span><span class="sxs-lookup"><span data-stu-id="fab5f-176">user.surname</span></span> |
    | <span data-ttu-id="fab5f-177">e-postadress</span><span class="sxs-lookup"><span data-stu-id="fab5f-177">emailaddress</span></span> | <span data-ttu-id="fab5f-178">User.Mail</span><span class="sxs-lookup"><span data-stu-id="fab5f-178">user.mail</span></span> |
    | <span data-ttu-id="fab5f-179">namn</span><span class="sxs-lookup"><span data-stu-id="fab5f-179">name</span></span> | <span data-ttu-id="fab5f-180">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="fab5f-180">user.userprincipalname</span></span> |
    
    <span data-ttu-id="fab5f-181">a.</span><span class="sxs-lookup"><span data-stu-id="fab5f-181">a.</span></span> <span data-ttu-id="fab5f-182">Klicka på hello attributet tooopen hello **Redigera attribut** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fab5f-182">Click on hello attribute tooopen hello **Edit Attribute** dialog.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mypolicies-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="fab5f-184">b.</span><span class="sxs-lookup"><span data-stu-id="fab5f-184">b.</span></span> <span data-ttu-id="fab5f-185">Ta bort hello URL-värdet från hello **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-185">Delete hello URL value from hello **Namespace**.</span></span>
    
    <span data-ttu-id="fab5f-186">c.</span><span class="sxs-lookup"><span data-stu-id="fab5f-186">c.</span></span> <span data-ttu-id="fab5f-187">Klicka på **Ok** toosave hello inställningen.</span><span class="sxs-lookup"><span data-stu-id="fab5f-187">Click **Ok** toosave hello setting.</span></span>
    
6. <span data-ttu-id="fab5f-188">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="fab5f-188">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_certificate.png) 

7. <span data-ttu-id="fab5f-190">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="fab5f-190">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mypolicies-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="fab5f-192">På hello **myPolicies Configuration** klickar du på **konfigurera myPolicies** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="fab5f-192">On hello **myPolicies Configuration** section, click **Configure myPolicies** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="fab5f-193">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="fab5f-193">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_configure.png) 

9. <span data-ttu-id="fab5f-195">tooconfigure enkel inloggning på **myPolicies** sida, behöver du toosend hello hämtas **Certificate(Base64)** och **SAML inloggning tjänst-URL för enkel** för[myPolicies supportteam](mailto:support@mypolicies.com).</span><span class="sxs-lookup"><span data-stu-id="fab5f-195">tooconfigure single sign-on on **myPolicies** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** too[myPolicies support team](mailto:support@mypolicies.com).</span></span> 

> [!TIP]
> <span data-ttu-id="fab5f-196">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="fab5f-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="fab5f-197">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="fab5f-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="fab5f-198">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fab5f-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fab5f-199">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="fab5f-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="fab5f-200">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fab5f-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="fab5f-202">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fab5f-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="fab5f-203">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fab5f-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fab5f-205">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fab5f-207">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fab5f-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fab5f-209">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fab5f-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fab5f-211">a.</span><span class="sxs-lookup"><span data-stu-id="fab5f-211">a.</span></span> <span data-ttu-id="fab5f-212">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fab5f-213">b.</span><span class="sxs-lookup"><span data-stu-id="fab5f-213">b.</span></span> <span data-ttu-id="fab5f-214">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fab5f-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fab5f-215">c.</span><span class="sxs-lookup"><span data-stu-id="fab5f-215">c.</span></span> <span data-ttu-id="fab5f-216">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="fab5f-217">d.</span><span class="sxs-lookup"><span data-stu-id="fab5f-217">d.</span></span> <span data-ttu-id="fab5f-218">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-218">Click **Create**.</span></span>
 
### <a name="creating-a-mypolicies-test-user"></a><span data-ttu-id="fab5f-219">Skapa en testanvändare myPolicies</span><span class="sxs-lookup"><span data-stu-id="fab5f-219">Creating a myPolicies test user</span></span>

<span data-ttu-id="fab5f-220">I det här avsnittet skapar du en användare som kallas Britta Simon i myPolicies.</span><span class="sxs-lookup"><span data-stu-id="fab5f-220">In this section, you create a user called Britta Simon in myPolicies.</span></span> <span data-ttu-id="fab5f-221">Arbeta med [myPolicies supportteam](mailto:support@mypolicies.com) att lägga till hello användare i hello myPolicies plattform.</span><span class="sxs-lookup"><span data-stu-id="fab5f-221">Work with [myPolicies support team](mailto:support@mypolicies.com) to add hello users in hello myPolicies platform.</span></span> <span data-ttu-id="fab5f-222">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fab5f-222">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="fab5f-223">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="fab5f-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="fab5f-224">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst toomyPolicies.</span><span class="sxs-lookup"><span data-stu-id="fab5f-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access toomyPolicies.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="fab5f-226">**tooassign Britta Simon toomyPolicies utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fab5f-226">**tooassign Britta Simon toomyPolicies, perform hello following steps:**</span></span>

1. <span data-ttu-id="fab5f-227">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="fab5f-229">Välj i listan med program hello **myPolicies**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-229">In hello applications list, select **myPolicies**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_app.png) 

3. <span data-ttu-id="fab5f-231">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="fab5f-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="fab5f-233">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="fab5f-233">Click **Add** button.</span></span> <span data-ttu-id="fab5f-234">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fab5f-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="fab5f-236">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="fab5f-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="fab5f-237">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fab5f-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fab5f-238">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fab5f-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fab5f-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fab5f-239">Testing single sign-on</span></span>

<span data-ttu-id="fab5f-240">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="fab5f-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="fab5f-241">Du bör få automatiskt inloggade tooyour myPolicies programmet när du klickar på hello myPolicies panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="fab5f-241">When you click hello myPolicies tile in hello Access Panel, you should get automatically signed-on tooyour myPolicies application.</span></span>
<span data-ttu-id="fab5f-242">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fab5f-242">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fab5f-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="fab5f-243">Additional resources</span></span>

* [<span data-ttu-id="fab5f-244">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fab5f-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fab5f-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fab5f-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_203.png

