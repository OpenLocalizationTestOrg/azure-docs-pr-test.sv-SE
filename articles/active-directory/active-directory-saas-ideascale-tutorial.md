---
title: "Självstudier: Azure Active Directory-integrering med IdeaScale | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och IdeaScale."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 10722b137e7565ee165e73994fd5a60b994719bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a><span data-ttu-id="9a198-103">Självstudier: Azure Active Directory-integrering med IdeaScale</span><span class="sxs-lookup"><span data-stu-id="9a198-103">Tutorial: Azure Active Directory integration with IdeaScale</span></span>

<span data-ttu-id="9a198-104">I kursen får du lära dig hur toointegrate IdeaScale med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9a198-104">In this tutorial, you learn how toointegrate IdeaScale with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9a198-105">Integrera IdeaScale med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9a198-105">Integrating IdeaScale with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9a198-106">Du kan styra i Azure AD som har åtkomst till tooIdeaScale</span><span class="sxs-lookup"><span data-stu-id="9a198-106">You can control in Azure AD who has access tooIdeaScale</span></span>
- <span data-ttu-id="9a198-107">Du kan aktivera din användare tooautomatically get inloggade tooIdeaScale (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9a198-107">You can enable your users tooautomatically get signed-on tooIdeaScale (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9a198-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9a198-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9a198-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9a198-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a198-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9a198-110">Prerequisites</span></span>

<span data-ttu-id="9a198-111">tooconfigure Azure AD-integrering med IdeaScale, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9a198-111">tooconfigure Azure AD integration with IdeaScale, you need hello following items:</span></span>

- <span data-ttu-id="9a198-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9a198-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9a198-113">En IdeaScale enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="9a198-113">An IdeaScale single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9a198-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9a198-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9a198-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9a198-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9a198-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9a198-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9a198-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9a198-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9a198-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9a198-118">Scenario description</span></span>
<span data-ttu-id="9a198-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9a198-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9a198-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9a198-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9a198-121">Att lägga till IdeaScale från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9a198-121">Adding IdeaScale from hello gallery</span></span>
2. <span data-ttu-id="9a198-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a198-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ideascale-from-hello-gallery"></a><span data-ttu-id="9a198-123">Att lägga till IdeaScale från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9a198-123">Adding IdeaScale from hello gallery</span></span>
<span data-ttu-id="9a198-124">tooconfigure hello integrering av IdeaScale i Azure AD, behöver du tooadd IdeaScale hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9a198-124">tooconfigure hello integration of IdeaScale into Azure AD, you need tooadd IdeaScale from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9a198-125">**tooadd IdeaScale från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9a198-125">**tooadd IdeaScale from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9a198-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9a198-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9a198-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9a198-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9a198-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9a198-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9a198-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a198-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9a198-133">Skriv i sökrutan hello **IdeaScale**.</span><span class="sxs-lookup"><span data-stu-id="9a198-133">In hello search box, type **IdeaScale**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_search.png)

5. <span data-ttu-id="9a198-135">Markera hello resultat på panelen **IdeaScale**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9a198-135">In hello results panel, select **IdeaScale**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9a198-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a198-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9a198-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med IdeaScale baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9a198-138">In this section, you configure and test Azure AD single sign-on with IdeaScale based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9a198-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i IdeaScale är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9a198-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in IdeaScale is tooa user in Azure AD.</span></span> <span data-ttu-id="9a198-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i IdeaScale toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9a198-140">In other words, a link relationship between an Azure AD user and hello related user in IdeaScale needs toobe established.</span></span>

<span data-ttu-id="9a198-141">I IdeaScale, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="9a198-141">In IdeaScale, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9a198-142">tooconfigure och testa Azure AD enkel inloggning med IdeaScale, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9a198-142">tooconfigure and test Azure AD single sign-on with IdeaScale, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9a198-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9a198-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9a198-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9a198-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9a198-145">**[Skapa en testanvändare IdeaScale](#creating-an-ideascale-test-user)**  -toohave en motsvarighet för Britta Simon i IdeaScale som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9a198-145">**[Creating an IdeaScale test user](#creating-an-ideascale-test-user)** - toohave a counterpart of Britta Simon in IdeaScale that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9a198-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9a198-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9a198-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9a198-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9a198-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a198-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9a198-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt IdeaScale program.</span><span class="sxs-lookup"><span data-stu-id="9a198-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your IdeaScale application.</span></span>

<span data-ttu-id="9a198-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med IdeaScale:**</span><span class="sxs-lookup"><span data-stu-id="9a198-150">**tooconfigure Azure AD single sign-on with IdeaScale, perform hello following steps:**</span></span>

1. <span data-ttu-id="9a198-151">I hello Azure-portalen på hello **IdeaScale** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9a198-151">In hello Azure portal, on hello **IdeaScale** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9a198-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9a198-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_samlbase.png)

3. <span data-ttu-id="9a198-155">På hello **IdeaScale domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a198-155">On hello **IdeaScale Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_url.png)

    <span data-ttu-id="9a198-157">a.</span><span class="sxs-lookup"><span data-stu-id="9a198-157">a.</span></span> <span data-ttu-id="9a198-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.ideascale.com`</span><span class="sxs-lookup"><span data-stu-id="9a198-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.ideascale.com`</span></span>

    <span data-ttu-id="9a198-159">b.</span><span class="sxs-lookup"><span data-stu-id="9a198-159">b.</span></span> <span data-ttu-id="9a198-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="9a198-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > <span data-ttu-id="9a198-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="9a198-161">These values are not real.</span></span> <span data-ttu-id="9a198-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="9a198-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9a198-163">Kontakta [IdeaScale klienten supportteamet](http://support.ideascale.com/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="9a198-163">Contact [IdeaScale Client support team](http://support.ideascale.com/) tooget these values.</span></span> 
 
4. <span data-ttu-id="9a198-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="9a198-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_certificate.png) 

5. <span data-ttu-id="9a198-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9a198-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9a198-168">På hello **IdeaScale Configuration** klickar du på **konfigurera IdeaScale** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="9a198-168">On hello **IdeaScale Configuration** section, click **Configure IdeaScale** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9a198-169">Kopiera hello **Sign-Out URL och SAML enhets-ID** från hello **Snabbreferens avsnittet**.</span><span class="sxs-lookup"><span data-stu-id="9a198-169">Copy hello **Sign-Out URL, and SAML Entity ID** from hello **Quick Reference section**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_configure.png) 

7. <span data-ttu-id="9a198-171">Logga in tooyour IdeaScale företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="9a198-171">In a different web browser window, log in tooyour IdeaScale company site as an administrator.</span></span>

8. <span data-ttu-id="9a198-172">Gå för**Community inställningar**.</span><span class="sxs-lookup"><span data-stu-id="9a198-172">Go too**Community Settings**.</span></span>
   
    <span data-ttu-id="9a198-173">![Community-inställningar](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community-inställningar")</span><span class="sxs-lookup"><span data-stu-id="9a198-173">![Community Settings](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community Settings")</span></span>

9. <span data-ttu-id="9a198-174">Gå för**säkerhet \> inställningar för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9a198-174">Go too**Security \> Single Signon Settings**.</span></span>
   
    <span data-ttu-id="9a198-175">![Enkel inloggning inställningar](./media/active-directory-saas-ideascale-tutorial/ic790848.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="9a198-175">![Single Signon Settings](./media/active-directory-saas-ideascale-tutorial/ic790848.png "Single Signon Settings")</span></span>

10. <span data-ttu-id="9a198-176">Som **enkel inloggning typen**väljer **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="9a198-176">As **Single-Signon Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="9a198-177">![Enkel inloggning typen](./media/active-directory-saas-ideascale-tutorial/ic790849.png "enkel inloggning typ")</span><span class="sxs-lookup"><span data-stu-id="9a198-177">![Single Signon Type](./media/active-directory-saas-ideascale-tutorial/ic790849.png "Single Signon Type")</span></span>

11. <span data-ttu-id="9a198-178">På hello **inställningar för enkel inloggning** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a198-178">On hello **Single Signon Settings** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="9a198-179">![Enkel inloggning inställningar](./media/active-directory-saas-ideascale-tutorial/ic790850.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="9a198-179">![Single Signon Settings](./media/active-directory-saas-ideascale-tutorial/ic790850.png "Single Signon Settings")</span></span>
   
    <span data-ttu-id="9a198-180">a.</span><span class="sxs-lookup"><span data-stu-id="9a198-180">a.</span></span> <span data-ttu-id="9a198-181">I **SAML IdP enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9a198-181">In **SAML IdP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="9a198-182">b.</span><span class="sxs-lookup"><span data-stu-id="9a198-182">b.</span></span> <span data-ttu-id="9a198-183">Kopiera hello innehållet hämtas metadata från Azure-portalen och klistra in den i hello **SAML IdP Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="9a198-183">Copy hello content of your downloaded metadata file from Azure portal, and paste it into hello **SAML IdP Metadata** textbox.</span></span>

    <span data-ttu-id="9a198-184">c.</span><span class="sxs-lookup"><span data-stu-id="9a198-184">c.</span></span> <span data-ttu-id="9a198-185">I **logga ut lyckade URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9a198-185">In **Logout Success URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="9a198-186">d.</span><span class="sxs-lookup"><span data-stu-id="9a198-186">d.</span></span> <span data-ttu-id="9a198-187">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="9a198-187">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="9a198-188">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="9a198-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9a198-189">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="9a198-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9a198-190">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9a198-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9a198-191">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a198-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="9a198-192">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9a198-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9a198-194">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9a198-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9a198-195">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9a198-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9a198-197">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9a198-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9a198-199">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a198-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9a198-201">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a198-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9a198-203">a.</span><span class="sxs-lookup"><span data-stu-id="9a198-203">a.</span></span> <span data-ttu-id="9a198-204">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9a198-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9a198-205">b.</span><span class="sxs-lookup"><span data-stu-id="9a198-205">b.</span></span> <span data-ttu-id="9a198-206">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9a198-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9a198-207">c.</span><span class="sxs-lookup"><span data-stu-id="9a198-207">c.</span></span> <span data-ttu-id="9a198-208">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9a198-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9a198-209">d.</span><span class="sxs-lookup"><span data-stu-id="9a198-209">d.</span></span> <span data-ttu-id="9a198-210">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9a198-210">Click **Create**.</span></span>
 
### <a name="creating-an-ideascale-test-user"></a><span data-ttu-id="9a198-211">Skapa en testanvändare IdeaScale</span><span class="sxs-lookup"><span data-stu-id="9a198-211">Creating an IdeaScale test user</span></span>

<span data-ttu-id="9a198-212">tooenable Azure AD-användare toolog i IdeaScale, måste de vara etablerade i tooIdeaScale.</span><span class="sxs-lookup"><span data-stu-id="9a198-212">tooenable Azure AD users toolog into IdeaScale, they must be provisioned in tooIdeaScale.</span></span> <span data-ttu-id="9a198-213">Hello gäller IdeaScale är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9a198-213">In hello case of IdeaScale, provisioning is a manual task.</span></span>

<span data-ttu-id="9a198-214">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="9a198-214">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="9a198-215">Logga in tooyour **IdeaScale** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="9a198-215">Log in tooyour **IdeaScale** company site as administrator.</span></span>

2. <span data-ttu-id="9a198-216">Gå för**Community inställningar**.</span><span class="sxs-lookup"><span data-stu-id="9a198-216">Go too**Community Settings**.</span></span>
   
    <span data-ttu-id="9a198-217">![Community-inställningar](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community-inställningar")</span><span class="sxs-lookup"><span data-stu-id="9a198-217">![Community Settings](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community Settings")</span></span>

3. <span data-ttu-id="9a198-218">Gå för**grundläggande inställningar \> medlem Management**.</span><span class="sxs-lookup"><span data-stu-id="9a198-218">Go too**Basic Settings \> Member Management**.</span></span>

4. <span data-ttu-id="9a198-219">Klicka på **lägga till medlem**.</span><span class="sxs-lookup"><span data-stu-id="9a198-219">Click **Add Member**.</span></span>
   
    <span data-ttu-id="9a198-220">![Hantering av medlem](./media/active-directory-saas-ideascale-tutorial/ic790852.png "medlem Management")</span><span class="sxs-lookup"><span data-stu-id="9a198-220">![Member Management](./media/active-directory-saas-ideascale-tutorial/ic790852.png "Member Management")</span></span>

5. <span data-ttu-id="9a198-221">I hello Lägg till ny medlem avsnitt, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a198-221">In hello Add New Member section, perform hello following steps:</span></span>
   
    <span data-ttu-id="9a198-222">![Lägg till ny medlem](./media/active-directory-saas-ideascale-tutorial/ic790853.png "Lägg till ny medlem")</span><span class="sxs-lookup"><span data-stu-id="9a198-222">![Add New Member](./media/active-directory-saas-ideascale-tutorial/ic790853.png "Add New Member")</span></span>
   
    <span data-ttu-id="9a198-223">a.</span><span class="sxs-lookup"><span data-stu-id="9a198-223">a.</span></span> <span data-ttu-id="9a198-224">I hello **e-postadresser** textruta typen hello e-postadress på en giltig AAD-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="9a198-224">In hello **Email Addresses** textbox, type hello email address of a valid AAD account you want tooprovision.</span></span>
   
    <span data-ttu-id="9a198-225">b.</span><span class="sxs-lookup"><span data-stu-id="9a198-225">b.</span></span> <span data-ttu-id="9a198-226">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="9a198-226">Click **Save Changes**.</span></span> 
   
    >[!NOTE]
    ><span data-ttu-id="9a198-227">hello Azure Active Directory användare får ett e-postmeddelande med en länk tooconfirm hello-konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="9a198-227">hello Azure Active Directory account holder gets an email with a link tooconfirm hello account before it becomes active.</span></span>
      
>[!NOTE]
><span data-ttu-id="9a198-228">Du kan använda något annat IdeaScale användarens konto skapas verktyg eller API: er som tillhandahålls av IdeaScale tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="9a198-228">You can use any other IdeaScale user account creation tools or APIs provided by IdeaScale tooprovision AAD user accounts.</span></span>
 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9a198-229">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a198-229">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9a198-230">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooIdeaScale.</span><span class="sxs-lookup"><span data-stu-id="9a198-230">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIdeaScale.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9a198-232">**tooassign Britta Simon tooIdeaScale utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9a198-232">**tooassign Britta Simon tooIdeaScale, perform hello following steps:**</span></span>

1. <span data-ttu-id="9a198-233">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9a198-233">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9a198-235">Välj i listan med program hello **IdeaScale**.</span><span class="sxs-lookup"><span data-stu-id="9a198-235">In hello applications list, select **IdeaScale**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_app.png) 

3. <span data-ttu-id="9a198-237">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9a198-237">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9a198-239">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9a198-239">Click **Add** button.</span></span> <span data-ttu-id="9a198-240">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a198-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9a198-242">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9a198-242">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9a198-243">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a198-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9a198-244">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a198-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9a198-245">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a198-245">Testing single sign-on</span></span>


<span data-ttu-id="9a198-246">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9a198-246">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9a198-247">Du bör få automatiskt inloggade tooyour IdeaScale programmet när du klickar på hello IdeaScale panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9a198-247">When you click hello IdeaScale tile in hello Access Panel, you should get automatically signed-on tooyour IdeaScale application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9a198-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9a198-248">Additional resources</span></span>

* [<span data-ttu-id="9a198-249">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9a198-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9a198-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9a198-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_203.png

