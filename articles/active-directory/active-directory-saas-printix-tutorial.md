---
title: "Självstudier: Azure Active Directory-integrering med Printix | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Printix."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4aea7320-b2d5-49e0-9b63-aeaff0f6fe66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 654810116091eb52912b377cc97afef803ee816e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a><span data-ttu-id="b5f8c-103">Självstudier: Azure Active Directory-integrering med Printix</span><span class="sxs-lookup"><span data-stu-id="b5f8c-103">Tutorial: Azure Active Directory integration with Printix</span></span>

<span data-ttu-id="b5f8c-104">I kursen får du lära dig hur toointegrate Printix med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b5f8c-104">In this tutorial, you learn how toointegrate Printix with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b5f8c-105">Integrera Printix med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b5f8c-105">Integrating Printix with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b5f8c-106">Du kan styra i Azure AD som har åtkomst till tooPrintix</span><span class="sxs-lookup"><span data-stu-id="b5f8c-106">You can control in Azure AD who has access tooPrintix</span></span>
- <span data-ttu-id="b5f8c-107">Du kan aktivera din användare tooautomatically get inloggade tooPrintix (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b5f8c-107">You can enable your users tooautomatically get signed-on tooPrintix (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b5f8c-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b5f8c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b5f8c-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b5f8c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5f8c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b5f8c-110">Prerequisites</span></span>

<span data-ttu-id="b5f8c-111">tooconfigure Azure AD-integrering med Printix, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b5f8c-111">tooconfigure Azure AD integration with Printix, you need hello following items:</span></span>

- <span data-ttu-id="b5f8c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b5f8c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b5f8c-113">En Printix enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b5f8c-113">A Printix single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b5f8c-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b5f8c-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b5f8c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b5f8c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b5f8c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b5f8c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b5f8c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b5f8c-118">Scenario description</span></span>
<span data-ttu-id="b5f8c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b5f8c-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b5f8c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b5f8c-121">Att lägga till Printix från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b5f8c-121">Adding Printix from hello gallery</span></span>
2. <span data-ttu-id="b5f8c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5f8c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-printix-from-hello-gallery"></a><span data-ttu-id="b5f8c-123">Att lägga till Printix från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b5f8c-123">Adding Printix from hello gallery</span></span>
<span data-ttu-id="b5f8c-124">tooconfigure hello integrering av Printix i Azure AD, behöver du tooadd Printix hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-124">tooconfigure hello integration of Printix into Azure AD, you need tooadd Printix from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b5f8c-125">**tooadd Printix från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b5f8c-125">**tooadd Printix from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5f8c-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b5f8c-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b5f8c-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b5f8c-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b5f8c-133">Skriv i sökrutan hello **Printix**.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-133">In hello search box, type **Printix**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/tutorial_printix_search.png)

5. <span data-ttu-id="b5f8c-135">Markera hello resultat på panelen **Printix**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-135">In hello results panel, select **Printix**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b5f8c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5f8c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b5f8c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Printix baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-138">In this section, you configure and test Azure AD single sign-on with Printix based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b5f8c-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Printix är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Printix is tooa user in Azure AD.</span></span> <span data-ttu-id="b5f8c-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Printix toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-140">In other words, a link relationship between an Azure AD user and hello related user in Printix needs toobe established.</span></span>

<span data-ttu-id="b5f8c-141">I Printix, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-141">In Printix, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b5f8c-142">tooconfigure och testa Azure AD enkel inloggning med Printix, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b5f8c-142">tooconfigure and test Azure AD single sign-on with Printix, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b5f8c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b5f8c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b5f8c-145">**[Skapa en testanvändare Printix](#creating-a-printix-test-user)**  -toohave en motsvarighet för Britta Simon i Printix som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-145">**[Creating a Printix test user](#creating-a-printix-test-user)** - toohave a counterpart of Britta Simon in Printix that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b5f8c-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b5f8c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b5f8c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5f8c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b5f8c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Printix program.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Printix application.</span></span>

<span data-ttu-id="b5f8c-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Printix:**</span><span class="sxs-lookup"><span data-stu-id="b5f8c-150">**tooconfigure Azure AD single sign-on with Printix, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5f8c-151">I hello Azure-portalen på hello **Printix** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-151">In hello Azure portal, on hello **Printix** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b5f8c-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_samlbase.png)

3. <span data-ttu-id="b5f8c-155">På hello **Printix domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b5f8c-155">On hello **Printix Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_url.png)

    <span data-ttu-id="b5f8c-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.printix.net`</span><span class="sxs-lookup"><span data-stu-id="b5f8c-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.printix.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b5f8c-158">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-158">hello value is not real.</span></span> <span data-ttu-id="b5f8c-159">Hello uppdateringsvärde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="b5f8c-160">Kontakta [Printix klienten supportteamet](mailto:support@printix.net) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-160">Contact [Printix Client support team](mailto:support@printix.net) tooget hello value.</span></span> 
 
4. <span data-ttu-id="b5f8c-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_certificate.png) 

5. <span data-ttu-id="b5f8c-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b5f8c-165">Inloggning tooyour Printix innehavaren som administratör.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-165">Sign-on tooyour Printix tenant as an administrator.</span></span>

7. <span data-ttu-id="b5f8c-166">Klicka hello längst hello övre högra hörnet i hello-menyn hello längst upp och välj ”**autentisering**”.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-166">In hello menu on hello top, click hello icon at hello upper right corner and select "**Authentication**".</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_06.png)

8. <span data-ttu-id="b5f8c-168">På hello **installationsprogrammet** väljer **aktivera Azure/Office 365-autentisering**</span><span class="sxs-lookup"><span data-stu-id="b5f8c-168">On hello **Setup** tab, select **Enable Azure/Office 365 authentication**</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_07.png)

9. <span data-ttu-id="b5f8c-170">På hello **Azure** fliken inkommande federation metadata URL toohello textruta för ”**Federation Metadatadokumentet**”.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-170">On hello **Azure** tab, input federation metadata URL toohello textbox of "**Federation metadata document**".</span></span> 

    <span data-ttu-id="b5f8c-171">Koppla hello metadata XML-fil som du hämtade från Azure AD för[Printix supportteamet](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="b5f8c-171">Attach hello metadata xml file which you downloaded from Azure AD too[Printix support team](mailto:support@printix.net).</span></span> <span data-ttu-id="b5f8c-172">Sedan de överför hello XML-fil och ange en Webbadress för federation metadata.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-172">Then they upload hello xml file and provide a federation metadata URL.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_08.png)
   
10. <span data-ttu-id="b5f8c-174">Klicka på hello ”**testa**” och klicka sedan på ”**OK**” knappen om hello testet lyckades.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-174">Click hello "**Test**" button and click "**OK**" button if hello test was successful.</span></span>
   
     <span data-ttu-id="b5f8c-175">Azure active directory-sidan visas när du klickar på hello **testa** knappen.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-175">Azure active directory page will show after clicking hello **test** button.</span></span> <span data-ttu-id="b5f8c-176">”Hej testet lyckades” innebär här att när du har angett hello autentiseringsuppgifterna för ditt testkonto för Azure som det visas ett meddelande ”inställningar testas OK”. Klicka på hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-176">"hello test was successful" here means after entering hello credentials of your Azure test account it will pop up a message "Settings tested OK".Then click hello **OK** button.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_09.png)

11. <span data-ttu-id="b5f8c-178">Klicka på hello **spara** på knappen ”**autentisering**” sidan.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-178">Click hello **Save** button on "**Authentication**" page.</span></span>


> [!TIP]
> <span data-ttu-id="b5f8c-179">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="b5f8c-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b5f8c-180">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b5f8c-181">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b5f8c-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b5f8c-182">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5f8c-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="b5f8c-183">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b5f8c-185">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b5f8c-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5f8c-186">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b5f8c-188">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b5f8c-190">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b5f8c-192">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b5f8c-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b5f8c-194">a.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-194">a.</span></span> <span data-ttu-id="b5f8c-195">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b5f8c-196">b.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-196">b.</span></span> <span data-ttu-id="b5f8c-197">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b5f8c-198">c.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-198">c.</span></span> <span data-ttu-id="b5f8c-199">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b5f8c-200">d.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-200">d.</span></span> <span data-ttu-id="b5f8c-201">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-201">Click **Create**.</span></span>
 
### <a name="creating-a-printix-test-user"></a><span data-ttu-id="b5f8c-202">Skapa en testanvändare Printix</span><span class="sxs-lookup"><span data-stu-id="b5f8c-202">Creating a Printix test user</span></span>

<span data-ttu-id="b5f8c-203">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Printix.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-203">hello objective of this section is toocreate a user called Britta Simon in Printix.</span></span> <span data-ttu-id="b5f8c-204">Printix stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-204">Printix supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="b5f8c-205">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-205">There is no action item for you in this section.</span></span> <span data-ttu-id="b5f8c-206">En ny användare skapas under ett försök tooaccess Printix om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-206">A new user is created during an attempt tooaccess Printix if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="b5f8c-207">Om du behöver toocreate en användare manuellt, måste toocontact hello [Printix supportteamet](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="b5f8c-207">If you need toocreate a user manually, you need toocontact hello [Printix support team](mailto:support@printix.net).</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b5f8c-208">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5f8c-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b5f8c-209">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPrintix.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPrintix.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b5f8c-211">**tooassign Britta Simon tooPrintix utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b5f8c-211">**tooassign Britta Simon tooPrintix, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5f8c-212">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b5f8c-214">Välj i listan med program hello **Printix**.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-214">In hello applications list, select **Printix**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_app.png) 

3. <span data-ttu-id="b5f8c-216">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b5f8c-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-218">Click **Add** button.</span></span> <span data-ttu-id="b5f8c-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b5f8c-221">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b5f8c-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b5f8c-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b5f8c-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5f8c-224">Testing single sign-on</span></span>

<span data-ttu-id="b5f8c-225">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b5f8c-226">Du bör få automatiskt inloggade tooyour Printix programmet när du klickar på hello Printix panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b5f8c-226">When you click hello Printix tile in hello Access Panel, you should get automatically signed-on tooyour Printix application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5f8c-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b5f8c-227">Additional resources</span></span>

* [<span data-ttu-id="b5f8c-228">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b5f8c-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b5f8c-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b5f8c-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-printix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-printix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-printix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-printix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-printix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-printix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-printix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-printix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-printix-tutorial/tutorial_general_203.png

