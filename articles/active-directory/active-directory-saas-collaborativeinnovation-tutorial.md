---
title: "Självstudier: Azure Active Directory-integrering med samarbetsfunktioner Innovation | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och samarbetsfunktioner Innovation."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bba95df3-75a4-4a93-8805-b3a8aa3d4861
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: e85fabfe11a380129f319a101aa7c7a9491260f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-collaborative-innovation"></a><span data-ttu-id="2b111-103">Självstudier: Azure Active Directory-integrering med samarbetsfunktioner Innovation</span><span class="sxs-lookup"><span data-stu-id="2b111-103">Tutorial: Azure Active Directory integration with Collaborative Innovation</span></span>

<span data-ttu-id="2b111-104">I kursen får du lära dig hur toointegrate samarbetsfunktioner Innovation med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2b111-104">In this tutorial, you learn how toointegrate Collaborative Innovation with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2b111-105">Integrera samarbetsfunktioner Innovation med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2b111-105">Integrating Collaborative Innovation with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2b111-106">Du kan styra i Azure AD som har åtkomst tooCollaborative Innovation</span><span class="sxs-lookup"><span data-stu-id="2b111-106">You can control in Azure AD who has access tooCollaborative Innovation</span></span>
- <span data-ttu-id="2b111-107">Du kan aktivera din användare tooautomatically get inloggade tooCollaborative Innovation (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2b111-107">You can enable your users tooautomatically get signed-on tooCollaborative Innovation (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2b111-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2b111-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2b111-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2b111-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b111-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2b111-110">Prerequisites</span></span>

<span data-ttu-id="2b111-111">tooconfigure Azure AD-integrering med samarbetsfunktioner Innovation, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2b111-111">tooconfigure Azure AD integration with Collaborative Innovation, you need hello following items:</span></span>

- <span data-ttu-id="2b111-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2b111-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2b111-113">En gemensam Innovation enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="2b111-113">A Collaborative Innovation single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2b111-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2b111-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2b111-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2b111-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2b111-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2b111-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2b111-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2b111-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2b111-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2b111-118">Scenario description</span></span>
<span data-ttu-id="2b111-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2b111-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2b111-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2b111-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2b111-121">Att lägga till samarbetsfunktioner Innovation från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2b111-121">Adding Collaborative Innovation from hello gallery</span></span>
2. <span data-ttu-id="2b111-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2b111-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-collaborative-innovation-from-hello-gallery"></a><span data-ttu-id="2b111-123">Att lägga till samarbetsfunktioner Innovation från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2b111-123">Adding Collaborative Innovation from hello gallery</span></span>
<span data-ttu-id="2b111-124">tooconfigure hello integrering av samarbetsfunktioner Innovation i Azure AD, behöver du tooadd samarbetsfunktioner Innovation hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="2b111-124">tooconfigure hello integration of Collaborative Innovation into Azure AD, you need tooadd Collaborative Innovation from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2b111-125">**tooadd samarbetsfunktioner Innovation från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2b111-125">**tooadd Collaborative Innovation from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b111-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2b111-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2b111-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2b111-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2b111-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="2b111-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2b111-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2b111-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="2b111-133">Skriv i sökrutan hello **samarbetsfunktioner Innovation**.</span><span class="sxs-lookup"><span data-stu-id="2b111-133">In hello search box, type **Collaborative Innovation**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_search.png)

5. <span data-ttu-id="2b111-135">Markera hello resultat på panelen **samarbetsfunktioner Innovation**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="2b111-135">In hello results panel, select **Collaborative Innovation**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2b111-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2b111-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2b111-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med samarbetsfunktioner Innovation baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2b111-138">In this section, you configure and test Azure AD single sign-on with Collaborative Innovation based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2b111-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren samarbetsfunktioner innovation är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2b111-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Collaborative Innovation is tooa user in Azure AD.</span></span> <span data-ttu-id="2b111-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i samarbetsfunktioner Innovation toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="2b111-140">In other words, a link relationship between an Azure AD user and hello related user in Collaborative Innovation needs toobe established.</span></span>

<span data-ttu-id="2b111-141">Tilldela hello värdet hello samarbetsfunktioner innovation **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="2b111-141">In Collaborative Innovation, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2b111-142">tooconfigure och testa Azure AD enkel inloggning med samarbetsfunktioner Innovation, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2b111-142">tooconfigure and test Azure AD single sign-on with Collaborative Innovation, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2b111-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2b111-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2b111-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2b111-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2b111-145">**[Skapa en testanvändare samarbetsfunktioner Innovation](#creating-a-collaborative-innovation-test-user)**  -toohave en motsvarighet för Britta Simon samarbetsfunktioner innovation som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="2b111-145">**[Creating a Collaborative Innovation test user](#creating-a-collaborative-innovation-test-user)** - toohave a counterpart of Britta Simon in Collaborative Innovation that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2b111-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2b111-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2b111-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2b111-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2b111-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2b111-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2b111-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet samarbetsfunktioner Innovation.</span><span class="sxs-lookup"><span data-stu-id="2b111-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Collaborative Innovation application.</span></span>

<span data-ttu-id="2b111-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med samarbetsfunktioner Innovation:**</span><span class="sxs-lookup"><span data-stu-id="2b111-150">**tooconfigure Azure AD single sign-on with Collaborative Innovation, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b111-151">I hello Azure-portalen på hello **samarbetsfunktioner Innovation** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2b111-151">In hello Azure portal, on hello **Collaborative Innovation** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2b111-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2b111-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_samlbase.png)

3. <span data-ttu-id="2b111-155">På hello **samarbetsfunktioner Innovation domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2b111-155">On hello **Collaborative Innovation Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_url.png)

    <span data-ttu-id="2b111-157">a.</span><span class="sxs-lookup"><span data-stu-id="2b111-157">a.</span></span> <span data-ttu-id="2b111-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.foundry.<companyname>.com/`</span><span class="sxs-lookup"><span data-stu-id="2b111-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instancename>.foundry.<companyname>.com/`</span></span>

    <span data-ttu-id="2b111-159">b.</span><span class="sxs-lookup"><span data-stu-id="2b111-159">b.</span></span> <span data-ttu-id="2b111-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.foundry.<companyname>.com`</span><span class="sxs-lookup"><span data-stu-id="2b111-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.foundry.<companyname>.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="2b111-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="2b111-161">These values are not real.</span></span> <span data-ttu-id="2b111-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="2b111-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2b111-163">Kontakta [samarbetsfunktioner Innovation klienten supportteamet](https://www.unilever.com/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="2b111-163">Contact [Collaborative Innovation Client support team](https://www.unilever.com/contact/) tooget these values.</span></span>  

4. <span data-ttu-id="2b111-164">Samarbetsfunktioner Innovation program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="2b111-164">Collaborative Innovation application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="2b111-165">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="2b111-165">Please configure hello following claims for this application.</span></span> <span data-ttu-id="2b111-166">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="2b111-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="2b111-167">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="2b111-167">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/attribute.png)
    
5. <span data-ttu-id="2b111-169">Klicka på **visa och redigera andra användarattribut** kryssrutan i hello **användarattribut** avsnittet tooexpand hello attribut.</span><span class="sxs-lookup"><span data-stu-id="2b111-169">Click **View and edit all other user attributes** checkbox in hello **User Attributes** section tooexpand hello attributes.</span></span> <span data-ttu-id="2b111-170">Utför följande steg på varje hello visas attribut - hello</span><span class="sxs-lookup"><span data-stu-id="2b111-170">Perform hello following steps on each of hello displayed attributes-</span></span>

    | <span data-ttu-id="2b111-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="2b111-171">Attribute Name</span></span> | <span data-ttu-id="2b111-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="2b111-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="2b111-173">givenName</span><span class="sxs-lookup"><span data-stu-id="2b111-173">givenname</span></span> | <span data-ttu-id="2b111-174">User.givenName</span><span class="sxs-lookup"><span data-stu-id="2b111-174">user.givenname</span></span> |
    | <span data-ttu-id="2b111-175">Efternamn</span><span class="sxs-lookup"><span data-stu-id="2b111-175">surname</span></span> | <span data-ttu-id="2b111-176">User.surname</span><span class="sxs-lookup"><span data-stu-id="2b111-176">user.surname</span></span> |
    | <span data-ttu-id="2b111-177">e-postadress</span><span class="sxs-lookup"><span data-stu-id="2b111-177">emailaddress</span></span> | <span data-ttu-id="2b111-178">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="2b111-178">user.userprincipalname</span></span> |
    | <span data-ttu-id="2b111-179">namn</span><span class="sxs-lookup"><span data-stu-id="2b111-179">name</span></span> | <span data-ttu-id="2b111-180">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="2b111-180">user.userprincipalname</span></span> |

    <span data-ttu-id="2b111-181">a.</span><span class="sxs-lookup"><span data-stu-id="2b111-181">a.</span></span> <span data-ttu-id="2b111-182">Klicka på hello attributet tooopen hello **Redigera attribut** fönster.</span><span class="sxs-lookup"><span data-stu-id="2b111-182">Click hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/url_update.png)

    <span data-ttu-id="2b111-184">b.</span><span class="sxs-lookup"><span data-stu-id="2b111-184">b.</span></span> <span data-ttu-id="2b111-185">Ta bort hello URL-värdet från hello **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="2b111-185">Delete hello URL value from hello **Namespace**.</span></span>
    
    <span data-ttu-id="2b111-186">c.</span><span class="sxs-lookup"><span data-stu-id="2b111-186">c.</span></span> <span data-ttu-id="2b111-187">Klicka på **Ok** toosave hello inställningen.</span><span class="sxs-lookup"><span data-stu-id="2b111-187">Click **Ok** toosave hello setting.</span></span>

6. <span data-ttu-id="2b111-188">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="2b111-188">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_certificate.png) 

7. <span data-ttu-id="2b111-190">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2b111-190">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="2b111-192">tooconfigure enkel inloggning på **samarbetsfunktioner Innovation** sida, behöver du toosend hello hämtas **XML-Metadata för** för[samarbetsfunktioner Innovation supportteamet](https://www.unilever.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="2b111-192">tooconfigure single sign-on on **Collaborative Innovation** side, you need toosend hello downloaded **Metadata XML** too[Collaborative Innovation support team](https://www.unilever.com/contact/).</span></span> <span data-ttu-id="2b111-193">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="2b111-193">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="2b111-194">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="2b111-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2b111-195">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="2b111-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2b111-196">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2b111-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2b111-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2b111-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="2b111-198">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2b111-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2b111-200">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2b111-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b111-201">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2b111-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2b111-203">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2b111-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2b111-205">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2b111-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2b111-207">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2b111-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2b111-209">a.</span><span class="sxs-lookup"><span data-stu-id="2b111-209">a.</span></span> <span data-ttu-id="2b111-210">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2b111-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2b111-211">b.</span><span class="sxs-lookup"><span data-stu-id="2b111-211">b.</span></span> <span data-ttu-id="2b111-212">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2b111-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2b111-213">c.</span><span class="sxs-lookup"><span data-stu-id="2b111-213">c.</span></span> <span data-ttu-id="2b111-214">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2b111-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2b111-215">d.</span><span class="sxs-lookup"><span data-stu-id="2b111-215">d.</span></span> <span data-ttu-id="2b111-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2b111-216">Click **Create**.</span></span>
 
### <a name="creating-a-collaborative-innovation-test-user"></a><span data-ttu-id="2b111-217">Skapa en testanvändare samarbetsfunktioner Innovation</span><span class="sxs-lookup"><span data-stu-id="2b111-217">Creating a Collaborative Innovation test user</span></span>

<span data-ttu-id="2b111-218">tooenable Azure AD-användare toolog i tooCollaborative Innovation, måste de etableras i samarbetsfunktioner Innovation.</span><span class="sxs-lookup"><span data-stu-id="2b111-218">tooenable Azure AD users toolog in tooCollaborative Innovation, they must be provisioned into Collaborative Innovation.</span></span>  

<span data-ttu-id="2b111-219">Vid det här programmet är etablering automatisk eftersom programmet hello stöder bara i tid användaretablering.</span><span class="sxs-lookup"><span data-stu-id="2b111-219">In case of this application provisioning is automatic as hello application supports just in time user provisioning.</span></span> <span data-ttu-id="2b111-220">Så finns det inget behov av tooperform här några steg.</span><span class="sxs-lookup"><span data-stu-id="2b111-220">So there is no need tooperform any steps here.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2b111-221">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="2b111-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2b111-222">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCollaborative Innovation.</span><span class="sxs-lookup"><span data-stu-id="2b111-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCollaborative Innovation.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2b111-224">**tooassign Britta Simon tooCollaborative Innovation, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="2b111-224">**tooassign Britta Simon tooCollaborative Innovation, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b111-225">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2b111-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2b111-227">Välj i listan med program hello **samarbetsfunktioner Innovation**.</span><span class="sxs-lookup"><span data-stu-id="2b111-227">In hello applications list, select **Collaborative Innovation**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_app.png) 

3. <span data-ttu-id="2b111-229">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2b111-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2b111-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2b111-231">Click **Add** button.</span></span> <span data-ttu-id="2b111-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2b111-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2b111-234">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="2b111-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2b111-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2b111-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2b111-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2b111-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2b111-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2b111-237">Testing single sign-on</span></span>

<span data-ttu-id="2b111-238">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2b111-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2b111-239">Du bör få inloggningssidan för samarbetsfunktioner Innovation program när du klickar på hello samarbetsfunktioner Innovation panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2b111-239">When you click hello Collaborative Innovation tile in hello Access Panel, you should get login page of Collaborative Innovation application.</span></span>
<span data-ttu-id="2b111-240">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2b111-240">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2b111-241">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2b111-241">Additional resources</span></span>

* [<span data-ttu-id="2b111-242">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2b111-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2b111-243">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2b111-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_203.png

