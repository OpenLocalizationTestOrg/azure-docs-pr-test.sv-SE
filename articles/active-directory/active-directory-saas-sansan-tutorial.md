---
title: "Självstudier: Azure Active Directory-integrering med Sansan | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Sansan."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: f58cc613a2e3a240e555b61a34db4155eb9dff71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a><span data-ttu-id="ada96-103">Självstudier: Azure Active Directory-integrering med Sansan</span><span class="sxs-lookup"><span data-stu-id="ada96-103">Tutorial: Azure Active Directory integration with Sansan</span></span>

<span data-ttu-id="ada96-104">I kursen får du lära dig hur toointegrate Sansan med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ada96-104">In this tutorial, you learn how toointegrate Sansan with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ada96-105">Integrera Sansan med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ada96-105">Integrating Sansan with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ada96-106">Du kan styra i Azure AD som har åtkomst till tooSansan</span><span class="sxs-lookup"><span data-stu-id="ada96-106">You can control in Azure AD who has access tooSansan</span></span>
- <span data-ttu-id="ada96-107">Du kan aktivera din användare tooautomatically get inloggade tooSansan (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ada96-107">You can enable your users tooautomatically get signed-on tooSansan (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ada96-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ada96-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ada96-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ada96-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ada96-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ada96-110">Prerequisites</span></span>

<span data-ttu-id="ada96-111">tooconfigure Azure AD-integrering med Sansan, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ada96-111">tooconfigure Azure AD integration with Sansan, you need hello following items:</span></span>

- <span data-ttu-id="ada96-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ada96-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ada96-113">En Sansan enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="ada96-113">A Sansan single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ada96-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ada96-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ada96-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ada96-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ada96-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ada96-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ada96-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ada96-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ada96-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ada96-118">Scenario description</span></span>
<span data-ttu-id="ada96-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ada96-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ada96-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ada96-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ada96-121">Att lägga till Sansan från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ada96-121">Adding Sansan from hello gallery</span></span>
2. <span data-ttu-id="ada96-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ada96-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sansan-from-hello-gallery"></a><span data-ttu-id="ada96-123">Att lägga till Sansan från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ada96-123">Adding Sansan from hello gallery</span></span>
<span data-ttu-id="ada96-124">tooconfigure hello integrering av Sansan i Azure AD, behöver du tooadd Sansan hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="ada96-124">tooconfigure hello integration of Sansan into Azure AD, you need tooadd Sansan from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ada96-125">**tooadd Sansan från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ada96-125">**tooadd Sansan from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ada96-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ada96-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ada96-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ada96-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ada96-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="ada96-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ada96-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ada96-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ada96-133">Skriv i sökrutan hello **Sansan**.</span><span class="sxs-lookup"><span data-stu-id="ada96-133">In hello search box, type **Sansan**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_search.png)

5. <span data-ttu-id="ada96-135">Markera hello resultat på panelen **Sansan**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="ada96-135">In hello results panel, select **Sansan**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ada96-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ada96-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ada96-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Sansan baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ada96-138">In this section, you configure and test Azure AD single sign-on with Sansan based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ada96-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Sansan är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ada96-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Sansan is tooa user in Azure AD.</span></span> <span data-ttu-id="ada96-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Sansan toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="ada96-140">In other words, a link relationship between an Azure AD user and hello related user in Sansan needs toobe established.</span></span>

<span data-ttu-id="ada96-141">I Sansan, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ada96-141">In Sansan, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ada96-142">tooconfigure och testa Azure AD enkel inloggning med Sansan, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ada96-142">tooconfigure and test Azure AD single sign-on with Sansan, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ada96-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ada96-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ada96-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ada96-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ada96-145">**[Skapa en testanvändare Sansan](#creating-a-sansan-test-user)**  -toohave en motsvarighet för Britta Simon i Sansan som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ada96-145">**[Creating a Sansan test user](#creating-a-sansan-test-user)** - toohave a counterpart of Britta Simon in Sansan that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ada96-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ada96-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ada96-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ada96-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ada96-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ada96-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ada96-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Sansan program.</span><span class="sxs-lookup"><span data-stu-id="ada96-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Sansan application.</span></span>

<span data-ttu-id="ada96-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Sansan:**</span><span class="sxs-lookup"><span data-stu-id="ada96-150">**tooconfigure Azure AD single sign-on with Sansan, perform hello following steps:**</span></span>

1. <span data-ttu-id="ada96-151">I hello Azure-portalen på hello **Sansan** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ada96-151">In hello Azure portal, on hello **Sansan** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ada96-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ada96-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_samlbase.png)

3. <span data-ttu-id="ada96-155">På hello **Sansan domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ada96-155">On hello **Sansan Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_url.png)

    <span data-ttu-id="ada96-157">a.</span><span class="sxs-lookup"><span data-stu-id="ada96-157">a.</span></span> <span data-ttu-id="ada96-158">I hello **inloggnings-URL** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="ada96-158">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span> 
    
    | <span data-ttu-id="ada96-159">Miljö</span><span class="sxs-lookup"><span data-stu-id="ada96-159">Environment</span></span> | <span data-ttu-id="ada96-160">URL: EN</span><span class="sxs-lookup"><span data-stu-id="ada96-160">URL</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="ada96-161">Dator</span><span class="sxs-lookup"><span data-stu-id="ada96-161">PC web</span></span> |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | <span data-ttu-id="ada96-162">Inbyggda mobila appen</span><span class="sxs-lookup"><span data-stu-id="ada96-162">Native Mobile app</span></span> |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | <span data-ttu-id="ada96-163">Inställningar för mobila webbläsare</span><span class="sxs-lookup"><span data-stu-id="ada96-163">Mobile browser settings</span></span> |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    <span data-ttu-id="ada96-164">b.</span><span class="sxs-lookup"><span data-stu-id="ada96-164">b.</span></span> <span data-ttu-id="ada96-165">I hello **identifierare** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="ada96-165">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span>
    | <span data-ttu-id="ada96-166">Miljö</span><span class="sxs-lookup"><span data-stu-id="ada96-166">Environment</span></span>             | <span data-ttu-id="ada96-167">URL: EN</span><span class="sxs-lookup"><span data-stu-id="ada96-167">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="ada96-168">Dator</span><span class="sxs-lookup"><span data-stu-id="ada96-168">PC web</span></span>                  | `https://ap.sansan.com/v/saml2/<company name>`|
    | <span data-ttu-id="ada96-169">Inbyggda mobila appen</span><span class="sxs-lookup"><span data-stu-id="ada96-169">Native Mobile app</span></span>       | `https://internal.api.sansan.com/saml2/<company name>` |
    | <span data-ttu-id="ada96-170">Inställningar för mobila webbläsare</span><span class="sxs-lookup"><span data-stu-id="ada96-170">Mobile browser settings</span></span> | `https://ap.sansan.com/s/saml2/<company name>` |

    > [!NOTE] 
    > <span data-ttu-id="ada96-171">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ada96-171">These values are not real.</span></span> <span data-ttu-id="ada96-172">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="ada96-172">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ada96-173">Kontakta [Sansan klienten supportteamet](https://www.sansan.com/form/contact) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ada96-173">Contact [Sansan Client support team](https://www.sansan.com/form/contact) tooget these values.</span></span> 

4. <span data-ttu-id="ada96-174">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="ada96-174">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_certificate.png) 

5. <span data-ttu-id="ada96-176">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ada96-176">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ada96-178">På hello **Sansan Configuration** klickar du på **konfigurera Sansan** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ada96-178">On hello **Sansan Configuration** section, click **Configure Sansan** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ada96-179">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ada96-179">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_configure.png) 

7. <span data-ttu-id="ada96-181">tooconfigure enkel inloggning på **Sansan** sida, behöver du toosend hello hämtas **certifikat**, **Sign-Out URL**, **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** för[Sansan supportteamet](https://www.sansan.com/form/contact).</span><span class="sxs-lookup"><span data-stu-id="ada96-181">tooconfigure single sign-on on **Sansan** side, you need toosend hello downloaded **Certificate**, **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** too[Sansan support team](https://www.sansan.com/form/contact).</span></span> <span data-ttu-id="ada96-182">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="ada96-182">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

>[!NOTE]
><span data-ttu-id="ada96-183">PC webbläsarinställningen fungerar även för mobila appar och mobila webbläsare tillsammans med dator.</span><span class="sxs-lookup"><span data-stu-id="ada96-183">PC browser setting also work for Mobile app and Mobile browser along with PC web.</span></span>  

> [!TIP]
> <span data-ttu-id="ada96-184">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="ada96-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ada96-185">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="ada96-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ada96-186">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ada96-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ada96-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ada96-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="ada96-188">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ada96-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ada96-190">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ada96-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ada96-191">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ada96-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ada96-193">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ada96-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ada96-195">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ada96-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ada96-197">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ada96-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ada96-199">a.</span><span class="sxs-lookup"><span data-stu-id="ada96-199">a.</span></span> <span data-ttu-id="ada96-200">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ada96-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ada96-201">b.</span><span class="sxs-lookup"><span data-stu-id="ada96-201">b.</span></span> <span data-ttu-id="ada96-202">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ada96-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ada96-203">c.</span><span class="sxs-lookup"><span data-stu-id="ada96-203">c.</span></span> <span data-ttu-id="ada96-204">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ada96-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ada96-205">d.</span><span class="sxs-lookup"><span data-stu-id="ada96-205">d.</span></span> <span data-ttu-id="ada96-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ada96-206">Click **Create**.</span></span>
 
### <a name="creating-a-sansan-test-user"></a><span data-ttu-id="ada96-207">Skapa en testanvändare Sansan</span><span class="sxs-lookup"><span data-stu-id="ada96-207">Creating a Sansan test user</span></span>

<span data-ttu-id="ada96-208">I det här avsnittet skapar du en användare som kallas Britta Simon i SanSan.</span><span class="sxs-lookup"><span data-stu-id="ada96-208">In this section, you create a user called Britta Simon in SanSan.</span></span> <span data-ttu-id="ada96-209">SanSan programmet måste hello användaren toobe etableras i hello program innan du utför enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ada96-209">SanSan application needs hello user toobe provisioned in hello application before doing SSO.</span></span> 

>[!NOTE]
><span data-ttu-id="ada96-210">Om du vill toocreate en användare manuellt eller batch-användare, behöver du toocontact hello [Sansan supportteamet](https://www.sansan.com/form/contact).</span><span class="sxs-lookup"><span data-stu-id="ada96-210">If you need toocreate a user manually or batch of users, you need toocontact hello [Sansan support team](https://www.sansan.com/form/contact).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ada96-211">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="ada96-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ada96-212">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSansan.</span><span class="sxs-lookup"><span data-stu-id="ada96-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSansan.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ada96-214">**tooassign Britta Simon tooSansan utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ada96-214">**tooassign Britta Simon tooSansan, perform hello following steps:**</span></span>

1. <span data-ttu-id="ada96-215">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ada96-215">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ada96-217">Välj i listan med program hello **Sansan**.</span><span class="sxs-lookup"><span data-stu-id="ada96-217">In hello applications list, select **Sansan**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_app.png) 

3. <span data-ttu-id="ada96-219">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ada96-219">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ada96-221">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ada96-221">Click **Add** button.</span></span> <span data-ttu-id="ada96-222">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ada96-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ada96-224">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="ada96-224">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ada96-225">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ada96-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ada96-226">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ada96-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ada96-227">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ada96-227">Testing single sign-on</span></span>

<span data-ttu-id="ada96-228">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ada96-228">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ada96-229">Du bör få automatiskt inloggade tooyour Sansan programmet när du klickar på hello Sansan panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ada96-229">When you click hello Sansan tile in hello Access Panel, you should get automatically signed-on tooyour Sansan application.</span></span>
<span data-ttu-id="ada96-230">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ada96-230">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ada96-231">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ada96-231">Additional resources</span></span>

* [<span data-ttu-id="ada96-232">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ada96-232">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ada96-233">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ada96-233">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_203.png

