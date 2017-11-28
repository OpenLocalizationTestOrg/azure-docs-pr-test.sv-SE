---
title: "Självstudier: Azure Active Directory-integrering med Zendesk | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zendesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 46ccd57a4adeb810af459caaa1e592cf2b62cb8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a><span data-ttu-id="3169b-103">Självstudier: Azure Active Directory-integrering med Zendesk</span><span class="sxs-lookup"><span data-stu-id="3169b-103">Tutorial: Azure Active Directory integration with Zendesk</span></span>

<span data-ttu-id="3169b-104">I kursen får du lära dig hur toointegrate Zendesk med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3169b-104">In this tutorial, you learn how toointegrate Zendesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3169b-105">Integrera Zendesk med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3169b-105">Integrating Zendesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3169b-106">Du kan styra i Azure AD som har åtkomst till tooZendesk</span><span class="sxs-lookup"><span data-stu-id="3169b-106">You can control in Azure AD who has access tooZendesk</span></span>
- <span data-ttu-id="3169b-107">Du kan aktivera din användare tooautomatically get inloggade tooZendesk (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3169b-107">You can enable your users tooautomatically get signed-on tooZendesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3169b-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3169b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3169b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3169b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3169b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3169b-110">Prerequisites</span></span>

<span data-ttu-id="3169b-111">tooconfigure Azure AD-integrering med Zendesk, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="3169b-111">tooconfigure Azure AD integration with Zendesk, you need hello following items:</span></span>

- <span data-ttu-id="3169b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3169b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3169b-113">En Zendesk enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3169b-113">A Zendesk single sign-on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="3169b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3169b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="3169b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3169b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3169b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3169b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3169b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3169b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="3169b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3169b-118">Scenario description</span></span>
<span data-ttu-id="3169b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3169b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3169b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3169b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3169b-121">Att lägga till Zendesk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3169b-121">Adding Zendesk from hello gallery</span></span>
2. <span data-ttu-id="3169b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3169b-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zendesk-from-hello-gallery"></a><span data-ttu-id="3169b-123">Att lägga till Zendesk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3169b-123">Adding Zendesk from hello gallery</span></span>
<span data-ttu-id="3169b-124">tooconfigure hello integrering av Zendesk i Azure AD, behöver du tooadd Zendesk hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="3169b-124">tooconfigure hello integration of Zendesk into Azure AD, you need tooadd Zendesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3169b-125">**tooadd Zendesk från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3169b-125">**tooadd Zendesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3169b-126">I hello ** [Azure Portal](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3169b-126">In hello **[Azure Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3169b-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3169b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3169b-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="3169b-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3169b-131">Klicka på **nytt program** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3169b-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3169b-133">Skriv i sökrutan hello **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="3169b-133">In hello search box, type **Zendesk**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. <span data-ttu-id="3169b-135">Markera hello resultat på panelen **Zendesk**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="3169b-135">In hello results panel, select **Zendesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3169b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3169b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3169b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zendesk baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3169b-138">In this section, you configure and test Azure AD single sign-on with Zendesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3169b-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Zendesk är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3169b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zendesk is tooa user in Azure AD.</span></span> <span data-ttu-id="3169b-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Zendesk toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="3169b-140">In other words, a link relationship between an Azure AD user and hello related user in Zendesk needs toobe established.</span></span>

<span data-ttu-id="3169b-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Zendesk.</span><span class="sxs-lookup"><span data-stu-id="3169b-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Zendesk.</span></span>

<span data-ttu-id="3169b-142">tooconfigure och testa Azure AD enkel inloggning med Zendesk, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3169b-142">tooconfigure and test Azure AD single sign-on with Zendesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3169b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3169b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3169b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3169b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3169b-145">**[Skapa en testanvändare Zendesk](#creating-a-zendesk-test-user) ** -toohave en motsvarighet för Britta Simon i Zendesk som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3169b-145">**[Creating a Zendesk test user](#creating-a-zendesk-test-user)** - toohave a counterpart of Britta Simon in Zendesk that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3169b-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3169b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3169b-147">**[Testa enkel inloggning](#testing-single-sign-on) ** -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3169b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3169b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3169b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3169b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Zendesk-program.</span><span class="sxs-lookup"><span data-stu-id="3169b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zendesk application.</span></span>

<span data-ttu-id="3169b-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Zendesk:**</span><span class="sxs-lookup"><span data-stu-id="3169b-150">**tooconfigure Azure AD single sign-on with Zendesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="3169b-151">I hello Azure-portalen på hello **Zendesk** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3169b-151">In hello Azure portal, on hello **Zendesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3169b-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3169b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. <span data-ttu-id="3169b-155">På hello **Zendesk domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3169b-155">On hello **Zendesk Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    <span data-ttu-id="3169b-157">a.</span><span class="sxs-lookup"><span data-stu-id="3169b-157">a.</span></span> <span data-ttu-id="3169b-158">I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="3169b-158">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<subdomain>.zendesk.com`</span></span>

    <span data-ttu-id="3169b-159">b.</span><span class="sxs-lookup"><span data-stu-id="3169b-159">b.</span></span> <span data-ttu-id="3169b-160">I hello **identifierare** textruta hello TYPVÄRDE med hello följande mönster:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="3169b-160">In hello **Identifier** textbox, type hello value using hello following pattern: `https://<subdomain>.zendesk.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3169b-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3169b-161">These values are not real.</span></span> <span data-ttu-id="3169b-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och Identifierare.</span><span class="sxs-lookup"><span data-stu-id="3169b-162">Update these values with hello actual Sign-on URL and Identifier URL.</span></span> <span data-ttu-id="3169b-163">Kontakta [Zendesk supportteamet](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3169b-163">Contact [Zendesk support team](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) tooget these values.</span></span> 

4. <span data-ttu-id="3169b-164">Zendesk förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="3169b-164">Zendesk expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="3169b-165">Det finns inga obligatoriska SAML-attribut men om du vill kan du lägga till ett attribut från **användarattribut** avsnitt genom följande hello nedanstående steg:</span><span class="sxs-lookup"><span data-stu-id="3169b-165">There are no mandatory SAML attributes but optionally you can add an attribute from **User Attributes** section by following hello below steps:</span></span> 

     ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    <span data-ttu-id="3169b-167">a.</span><span class="sxs-lookup"><span data-stu-id="3169b-167">a.</span></span> <span data-ttu-id="3169b-168">Klicka på hello **visa och redigera alla hello andra attribut** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="3169b-168">Click hello **View and edit all hello other attributes** check box.</span></span>
     
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    <span data-ttu-id="3169b-170">b.</span><span class="sxs-lookup"><span data-stu-id="3169b-170">b.</span></span> <span data-ttu-id="3169b-171">Klicka på hello **lägga till attributet** tooopen **Lägg till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3169b-171">Click hello **Add Attribute** tooopen **Add attribute** dialog.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="3169b-173">c.</span><span class="sxs-lookup"><span data-stu-id="3169b-173">c.</span></span> <span data-ttu-id="3169b-174">I hello **namn** textruta typen hello attributets namn (till exempel **e-postadress**).</span><span class="sxs-lookup"><span data-stu-id="3169b-174">In hello **Name** textbox, type hello attribute name (for example **emailaddress**).</span></span>
    
    <span data-ttu-id="3169b-175">d.</span><span class="sxs-lookup"><span data-stu-id="3169b-175">d.</span></span> <span data-ttu-id="3169b-176">Från hello **värdet** listan, Välj hello attributvärde (som **user.mail**).</span><span class="sxs-lookup"><span data-stu-id="3169b-176">From hello **Value** list, select hello attribute value (as **user.mail**).</span></span>
    
    <span data-ttu-id="3169b-177">e.</span><span class="sxs-lookup"><span data-stu-id="3169b-177">e.</span></span> <span data-ttu-id="3169b-178">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="3169b-178">Click **Ok**</span></span>
 
    > [!NOTE] 
    > <span data-ttu-id="3169b-179">Du kan använda attribut tooadd tilläggsattribut som inte ingår i Azure AD som standard.</span><span class="sxs-lookup"><span data-stu-id="3169b-179">You use extension attributes tooadd attributes that are not in Azure AD by default.</span></span> <span data-ttu-id="3169b-180">Klicka på [användarattribut som kan anges i SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tooget hello fullständig lista över SAML attribut som **Zendesk** accepterar.</span><span class="sxs-lookup"><span data-stu-id="3169b-180">Click [User attributes that can be set in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tooget hello complete list of SAML attributes that **Zendesk** accepts.</span></span> 

5. <span data-ttu-id="3169b-181">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="3169b-181">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. <span data-ttu-id="3169b-183">På hello **Zendesk Configuration** klickar du på **konfigurera Zendesk** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3169b-183">On hello **Zendesk Configuration** section, click **Configure Zendesk** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="3169b-184">Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="3169b-184">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. <span data-ttu-id="3169b-186">Logga in på webbplatsen Zendesk företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="3169b-186">In a different web browser window, log into your Zendesk company site as an administrator.</span></span>

8. <span data-ttu-id="3169b-187">Klicka på **Admin**.</span><span class="sxs-lookup"><span data-stu-id="3169b-187">Click **Admin**.</span></span>

9. <span data-ttu-id="3169b-188">Hello vänstra navigeringsfönstret, klicka på **inställningar**, och klicka sedan på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="3169b-188">In hello left navigation pane, click **Settings**, and then click **Security**.</span></span>

10. <span data-ttu-id="3169b-189">På hello **säkerhet** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3169b-189">On hello **Security** page, perform hello following steps:</span></span> 
   
     <span data-ttu-id="3169b-190">![Säkerhet](./media/active-directory-saas-zendesk-tutorial/ic773089.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="3169b-190">![Security](./media/active-directory-saas-zendesk-tutorial/ic773089.png "Security")</span></span>

    <span data-ttu-id="3169b-191">![Enkel inloggning](./media/active-directory-saas-zendesk-tutorial/ic773090.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="3169b-191">![Single sign-on](./media/active-directory-saas-zendesk-tutorial/ic773090.png "Single sign-on")</span></span>

     <span data-ttu-id="3169b-192">a.</span><span class="sxs-lookup"><span data-stu-id="3169b-192">a.</span></span> <span data-ttu-id="3169b-193">Klicka på hello **Admin & agenter** fliken.</span><span class="sxs-lookup"><span data-stu-id="3169b-193">Click hello **Admin & Agents** tab.</span></span>

     <span data-ttu-id="3169b-194">b.</span><span class="sxs-lookup"><span data-stu-id="3169b-194">b.</span></span> <span data-ttu-id="3169b-195">Välj **enkel inloggning (SSO) och SAML**, och välj sedan **SAML**.</span><span class="sxs-lookup"><span data-stu-id="3169b-195">Select **Single sign-on (SSO) and SAML**, and then select **SAML**.</span></span>

     <span data-ttu-id="3169b-196">c.</span><span class="sxs-lookup"><span data-stu-id="3169b-196">c.</span></span> <span data-ttu-id="3169b-197">I **SAML SSO URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3169b-197">In **SAML SSO URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

     <span data-ttu-id="3169b-198">d.</span><span class="sxs-lookup"><span data-stu-id="3169b-198">d.</span></span> <span data-ttu-id="3169b-199">I **Remote logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3169b-199">In **Remote Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
        
     <span data-ttu-id="3169b-200">e.</span><span class="sxs-lookup"><span data-stu-id="3169b-200">e.</span></span> <span data-ttu-id="3169b-201">I **certifikat fingeravtryck** textruta klistra in hello **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3169b-201">In **Certificate Fingerprint** textbox, paste hello **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>
     
     <span data-ttu-id="3169b-202">f.</span><span class="sxs-lookup"><span data-stu-id="3169b-202">f.</span></span> <span data-ttu-id="3169b-203">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3169b-203">Click **Save**.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3169b-204">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3169b-204">Creating an Azure AD test user</span></span>
<span data-ttu-id="3169b-205">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3169b-205">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3169b-207">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3169b-207">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3169b-208">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3169b-208">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3169b-210">toodisplay hello lista över användare gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3169b-210">toodisplay hello list of users go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3169b-212">Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3169b-212">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3169b-214">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3169b-214">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3169b-216">a.</span><span class="sxs-lookup"><span data-stu-id="3169b-216">a.</span></span> <span data-ttu-id="3169b-217">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3169b-217">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3169b-218">b.</span><span class="sxs-lookup"><span data-stu-id="3169b-218">b.</span></span> <span data-ttu-id="3169b-219">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3169b-219">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3169b-220">c.</span><span class="sxs-lookup"><span data-stu-id="3169b-220">c.</span></span> <span data-ttu-id="3169b-221">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3169b-221">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3169b-222">d.</span><span class="sxs-lookup"><span data-stu-id="3169b-222">d.</span></span> <span data-ttu-id="3169b-223">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3169b-223">Click **Create**.</span></span> 

### <a name="creating-a-zendesk-test-user"></a><span data-ttu-id="3169b-224">Skapa en testanvändare Zendesk</span><span class="sxs-lookup"><span data-stu-id="3169b-224">Creating a Zendesk test user</span></span>

<span data-ttu-id="3169b-225">tooenable Azure AD-användare toolog till **Zendesk**, de måste etableras i **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="3169b-225">tooenable Azure AD users toolog into **Zendesk**, they must be provisioned into **Zendesk**.</span></span>  
<span data-ttu-id="3169b-226">Beroende på hello roll som tilldelats i hello appar, är dess hello förväntat:</span><span class="sxs-lookup"><span data-stu-id="3169b-226">Depending on hello role assigned in hello apps, it's hello expected behavior:</span></span>

 1. <span data-ttu-id="3169b-227">**Slutanvändarens** konton tillhandahålls automatiskt när du loggar in.</span><span class="sxs-lookup"><span data-stu-id="3169b-227">**End-user** accounts are automatically provisioned when signing in.</span></span>
 2. <span data-ttu-id="3169b-228">**Agenten** och **Admin** konton måste toobe manuellt etableras i **Zendesk** innan du loggar in.</span><span class="sxs-lookup"><span data-stu-id="3169b-228">**Agent** and **Admin** accounts need toobe manually provisioned in **Zendesk** before signing in.</span></span>
 
<span data-ttu-id="3169b-229">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="3169b-229">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="3169b-230">Logga in tooyour **Zendesk** klient.</span><span class="sxs-lookup"><span data-stu-id="3169b-230">Log in tooyour **Zendesk** tenant.</span></span>

2. <span data-ttu-id="3169b-231">Välj hello **lista över kunder** fliken.</span><span class="sxs-lookup"><span data-stu-id="3169b-231">Select hello **Customer List** tab.</span></span>

3. <span data-ttu-id="3169b-232">Välj hello **användare** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3169b-232">Select hello **User** tab, and click **Add**.</span></span>
   
    <span data-ttu-id="3169b-233">![Lägg till användare](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Lägg till användare")</span><span class="sxs-lookup"><span data-stu-id="3169b-233">![Add user](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Add user")</span></span>
4. <span data-ttu-id="3169b-234">Skriv hello e-postadress för ett befintligt Azure AD-konto du vill tooprovision och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3169b-234">Type hello email address of an existing Azure AD account you want tooprovision, and then click **Save**.</span></span>
   
    <span data-ttu-id="3169b-235">![Ny användare](./media/active-directory-saas-zendesk-tutorial/ic773633.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="3169b-235">![New user](./media/active-directory-saas-zendesk-tutorial/ic773633.png "New user")</span></span>

> [!NOTE]
> <span data-ttu-id="3169b-236">Du kan använda något annat Zendesk användarens konto skapas verktyg eller API: er som tillhandahålls av Zendesk tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="3169b-236">You can use any other Zendesk user account creation tools or APIs provided by Zendesk tooprovision AAD user accounts.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3169b-237">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="3169b-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3169b-238">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooZendesk.</span><span class="sxs-lookup"><span data-stu-id="3169b-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZendesk.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3169b-240">**tooassign Britta Simon tooZendesk utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3169b-240">**tooassign Britta Simon tooZendesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="3169b-241">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3169b-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3169b-243">Välj i listan med program hello **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="3169b-243">In hello applications list, select **Zendesk**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. <span data-ttu-id="3169b-245">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3169b-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3169b-247">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3169b-247">Click **Add** button.</span></span> <span data-ttu-id="3169b-248">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3169b-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3169b-250">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="3169b-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3169b-251">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3169b-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3169b-252">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3169b-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3169b-253">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3169b-253">Testing single sign-on</span></span>

<span data-ttu-id="3169b-254">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3169b-254">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3169b-255">Du bör få automatiskt inloggade tooyour Zendesk programmet när du klickar på hello Zendesk-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3169b-255">When you click hello Zendesk tile in hello Access Panel, you should get automatically signed-on tooyour Zendesk application.</span></span>
<span data-ttu-id="3169b-256">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3169b-256">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3169b-257">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3169b-257">Additional resources</span></span>

* [<span data-ttu-id="3169b-258">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3169b-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3169b-259">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3169b-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
