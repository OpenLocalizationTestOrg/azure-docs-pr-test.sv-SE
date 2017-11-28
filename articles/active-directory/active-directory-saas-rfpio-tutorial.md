---
title: "Självstudier: Azure Active Directory-integrering med RFPIO | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och RFPIO."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: e0c692276276edd8f859e73d81cf54d75a65957a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a><span data-ttu-id="c2624-103">Självstudier: Azure Active Directory-integrering med RFPIO</span><span class="sxs-lookup"><span data-stu-id="c2624-103">Tutorial: Azure Active Directory integration with RFPIO</span></span>

<span data-ttu-id="c2624-104">I kursen får du lära dig hur toointegrate RFPIO med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c2624-104">In this tutorial, you learn how toointegrate RFPIO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c2624-105">Integrera RFPIO med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c2624-105">Integrating RFPIO with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c2624-106">Du kan styra vem i Azure AD som har åtkomst till tooRFPIO.</span><span class="sxs-lookup"><span data-stu-id="c2624-106">You can control who in Azure AD who has access tooRFPIO.</span></span>
- <span data-ttu-id="c2624-107">Du kan låta dina användare tooautomatically get inloggade tooRFPIO (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="c2624-107">You can enable your users tooautomatically get signed-on tooRFPIO (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="c2624-108">Du kan hantera dina konton i en central plats--hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c2624-108">You can manage your accounts in one central location--hello Azure portal.</span></span>

<span data-ttu-id="c2624-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c2624-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2624-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c2624-110">Prerequisites</span></span>

<span data-ttu-id="c2624-111">tooconfigure Azure AD-integrering med RFPIO, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c2624-111">tooconfigure Azure AD integration with RFPIO, you need hello following items:</span></span>

- <span data-ttu-id="c2624-112">En Azure AD-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c2624-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="c2624-113">En RFPIO enkel inloggning på aktiverat prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c2624-113">A RFPIO single sign-on-enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="c2624-114">Vi rekommenderar inte att du använder en tootest hello produktionsmiljön i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="c2624-114">We don't recommend that you use a production environment tootest hello steps in this tutorial.</span></span>

<span data-ttu-id="c2624-115">tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c2624-115">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="c2624-116">Använd inte din produktionsmiljö om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c2624-116">Do not use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="c2624-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c2624-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c2624-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c2624-118">Scenario description</span></span>
<span data-ttu-id="c2624-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c2624-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c2624-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c2624-120">hello scenario that's outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c2624-121">Lägger till RFPIO från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="c2624-121">Adding RFPIO from hello gallery.</span></span>
2. <span data-ttu-id="c2624-122">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c2624-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-rfpio-from-hello-gallery"></a><span data-ttu-id="c2624-123">Lägg till RFPIO från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c2624-123">Add RFPIO from hello gallery</span></span>
<span data-ttu-id="c2624-124">tooconfigure hello integrering av RFPIO i Azure AD, behöver du tooadd RFPIO hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c2624-124">tooconfigure hello integration of RFPIO into Azure AD, you need tooadd RFPIO from hello gallery tooyour list of managed SaaS apps.</span></span>

### <a name="tooadd-rfpio-from-hello-gallery"></a><span data-ttu-id="c2624-125">tooadd RFPIO från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c2624-125">tooadd RFPIO from hello gallery</span></span>

1. <span data-ttu-id="c2624-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, Välj hello **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c2624-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation pane, select hello **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c2624-128">Välj **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c2624-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c2624-130">tooadd ett nytt program väljer hello **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2624-130">tooadd a new application, select hello **New application** button on hello top of dialog box.</span></span>

    ![Program][3]

4. <span data-ttu-id="c2624-132">Skriv i sökrutan hello **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="c2624-132">In hello search box, type **RFPIO**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_search.png)

5. <span data-ttu-id="c2624-134">Markera hello resultat på panelen **RFPIO**, och välj sedan hello **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="c2624-134">In hello results panel, select **RFPIO**, and then select hello **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c2624-136">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2624-136">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="c2624-137">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med RFPIO baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c2624-137">In this section, you configure and test Azure AD single sign-on with RFPIO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c2624-138">För enkel inloggning toowork måste Azure AD tooknow vilka hello relationen är mellan motsvarighet användare i RFPIO och en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c2624-138">For single sign-on toowork, Azure AD needs tooknow what hello relationship is between counterpart user in RFPIO and a user in Azure AD.</span></span> <span data-ttu-id="c2624-139">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i RFPIO toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="c2624-139">In other words, a link relationship between an Azure AD user and hello related user in RFPIO needs toobe established.</span></span>

<span data-ttu-id="c2624-140">Tilldela hello värdet i RFPIO, **användarnamn** i Azure AD som hello värde för **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c2624-140">In RFPIO, assign hello value of **user name** in Azure AD as hello value of  **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c2624-141">tooconfigure och testa Azure AD enkel inloggning med RFPIO, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c2624-141">tooconfigure and test Azure AD single sign-on with RFPIO, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c2624-142">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**--tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c2624-142">**[Configure Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**--tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c2624-143">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**--tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2624-143">**[Create an Azure AD test user](#creating-an-azure-ad-test-user)**-- tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c2624-144">**[Skapa en testanvändare RFPIO](#creating-a-rfpio-test-user)**  --toohave en motsvarighet för Britta Simon i RFPIO som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c2624-144">**[Create a RFPIO test user](#creating-a-rfpio-test-user)** --toohave a counterpart of Britta Simon in RFPIO that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c2624-145">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**--tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c2624-145">**[Assign hello Azure AD test user](#assigning-the-azure-ad-test-user)**--tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c2624-146">**[Testa enkel inloggning](#testing-single-sign-on)**  --tooverify om hello konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c2624-146">**[Test Single Sign-On](#testing-single-sign-on)** --tooverify if hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c2624-147">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2624-147">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c2624-148">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt RFPIO program.</span><span class="sxs-lookup"><span data-stu-id="c2624-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RFPIO application.</span></span>

<span data-ttu-id="c2624-149">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med RFPIO:**</span><span class="sxs-lookup"><span data-stu-id="c2624-149">**tooconfigure Azure AD single sign-on with RFPIO, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2624-150">I hello Azure-portalen på hello **RFPIO** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c2624-150">In hello Azure portal, on hello **RFPIO** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c2624-152">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c2624-152">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. <span data-ttu-id="c2624-154">På hello **RFPIO domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="c2624-154">On hello **RFPIO Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url.png)

    <span data-ttu-id="c2624-156">a.</span><span class="sxs-lookup"><span data-stu-id="c2624-156">a.</span></span> <span data-ttu-id="c2624-157">I hello **identifierare** textruta typen hello URL:`https://www.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="c2624-157">In hello **Identifier** textbox, type hello URL: `https://www.rfpio.com`</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url1.png)

    <span data-ttu-id="c2624-159">b.</span><span class="sxs-lookup"><span data-stu-id="c2624-159">b.</span></span> <span data-ttu-id="c2624-160">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="c2624-160">Check **Show advanced URL settings**.</span></span>

    <span data-ttu-id="c2624-161">c.</span><span class="sxs-lookup"><span data-stu-id="c2624-161">c.</span></span> <span data-ttu-id="c2624-162">I hello **Relay tillstånd** textrutan anger du ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="c2624-162">In hello **Relay State** textbox enter a string value.</span></span> <span data-ttu-id="c2624-163">Kontakta [RFPIO supportteam](https://www.rfpio.com/contact/) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="c2624-163">Contact [RFPIO support team](https://www.rfpio.com/contact/) tooget this value.</span></span> 

4. <span data-ttu-id="c2624-164">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="c2624-164">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="c2624-165">Om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="c2624-165">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>   

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url2.png)

    <span data-ttu-id="c2624-167">I hello **inloggning URL** textruta typen hello URL:`https://www.app.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="c2624-167">In hello **Sign on URL** textbox, type hello URL: `https://www.app.rfpio.com`</span></span>

5. <span data-ttu-id="c2624-168">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="c2624-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. <span data-ttu-id="c2624-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c2624-170">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="c2624-172">I en annan webbläsarfönster inloggningen toohello **RFPIO** webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="c2624-172">In a different web browser window, login toohello **RFPIO** website as an administrator.</span></span>

8. <span data-ttu-id="c2624-173">Klicka på listrutan för hello nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="c2624-173">Click on hello bottom left corner dropdown.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app1.png)

9. <span data-ttu-id="c2624-175">Klicka på hello **organisationsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="c2624-175">Click on hello **Organization Settings**.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app2.png)

10. <span data-ttu-id="c2624-177">Klicka på hello **funktioner & INTEGRATION**.</span><span class="sxs-lookup"><span data-stu-id="c2624-177">Click on hello **FEATURES & INTEGRATION**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app4.png)

11. <span data-ttu-id="c2624-179">I hello **SAML SSO Configuration** klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="c2624-179">In hello **SAML SSO Configuration** Click **Edit**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app3.png)

12. <span data-ttu-id="c2624-181">Utför följande åtgärder i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="c2624-181">In this Section perform following actions:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app5.png)
    
    <span data-ttu-id="c2624-183">a.</span><span class="sxs-lookup"><span data-stu-id="c2624-183">a.</span></span> <span data-ttu-id="c2624-184">Kopiera hello innehållet i hello **hämtas XML-Metadata för** och klistra in den i hello **identitet configuration** fältet.</span><span class="sxs-lookup"><span data-stu-id="c2624-184">Copy hello content of hello **Downloaded Metadata XML** and paste it into hello **identity configuration** field.</span></span>

    > [!NOTE]
    ><span data-ttu-id="c2624-185">toocopy hello innehållet i hämtas **XML-Metadata för** Använd **anteckningar ++** eller rätt **XML-redigerare**.</span><span class="sxs-lookup"><span data-stu-id="c2624-185">toocopy hello content of downloaded **Metadata XML** Use **Notepad++** or proper **XML Editor**.</span></span> 

    <span data-ttu-id="c2624-186">b.</span><span class="sxs-lookup"><span data-stu-id="c2624-186">b.</span></span> <span data-ttu-id="c2624-187">Klicka på **Validera**.</span><span class="sxs-lookup"><span data-stu-id="c2624-187">Click **Validate**.</span></span>

    <span data-ttu-id="c2624-188">c.</span><span class="sxs-lookup"><span data-stu-id="c2624-188">c.</span></span> <span data-ttu-id="c2624-189">När du klickar på **verifiera**, vänd **SAML(Enabled)** tooon.</span><span class="sxs-lookup"><span data-stu-id="c2624-189">After Clicking **Validate**, Flip **SAML(Enabled)** tooon.</span></span>

    <span data-ttu-id="c2624-190">d.</span><span class="sxs-lookup"><span data-stu-id="c2624-190">d.</span></span> <span data-ttu-id="c2624-191">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="c2624-191">Click **Submit**.</span></span>

> [!TIP]
> <span data-ttu-id="c2624-192">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="c2624-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c2624-193">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="c2624-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c2624-194">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c2624-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c2624-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c2624-195">Create an Azure AD test user</span></span>
<span data-ttu-id="c2624-196">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2624-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c2624-198">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c2624-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2624-199">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c2624-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c2624-201">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c2624-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c2624-203">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2624-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c2624-205">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c2624-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c2624-207">a.</span><span class="sxs-lookup"><span data-stu-id="c2624-207">a.</span></span> <span data-ttu-id="c2624-208">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c2624-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c2624-209">b.</span><span class="sxs-lookup"><span data-stu-id="c2624-209">b.</span></span> <span data-ttu-id="c2624-210">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c2624-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c2624-211">c.</span><span class="sxs-lookup"><span data-stu-id="c2624-211">c.</span></span> <span data-ttu-id="c2624-212">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c2624-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c2624-213">d.</span><span class="sxs-lookup"><span data-stu-id="c2624-213">d.</span></span> <span data-ttu-id="c2624-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c2624-214">Click **Create**.</span></span>
 
### <a name="create-a-rfpio-test-user"></a><span data-ttu-id="c2624-215">Skapa en testanvändare RFPIO</span><span class="sxs-lookup"><span data-stu-id="c2624-215">Create a RFPIO test user</span></span>

<span data-ttu-id="c2624-216">tooenable Azure AD-användare toolog i tooRFPIO, måste de etableras i RFPIO.</span><span class="sxs-lookup"><span data-stu-id="c2624-216">tooenable Azure AD users toolog in tooRFPIO, they must be provisioned into RFPIO.</span></span>  
<span data-ttu-id="c2624-217">Hello gäller RFPIO är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c2624-217">In hello case of RFPIO, provisioning is a manual task.</span></span>

<span data-ttu-id="c2624-218">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="c2624-218">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2624-219">Logga in tooyour RFPIO företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="c2624-219">Log in tooyour RFPIO company site as an administrator.</span></span>

2. <span data-ttu-id="c2624-220">Klicka på listrutan för hello nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="c2624-220">Click on hello bottom left corner dropdown.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app1.png)

3. <span data-ttu-id="c2624-222">Klicka på hello **organisationsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="c2624-222">Click on hello **Organization Settings**.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app2.png)

4. <span data-ttu-id="c2624-224">Klicka på **GRUPPMEDLEMMAR**.</span><span class="sxs-lookup"><span data-stu-id="c2624-224">Click **TEAM MEMBERS**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app6.png)

5. <span data-ttu-id="c2624-226">Klicka på **Lägg till MEDLEMMAR**.</span><span class="sxs-lookup"><span data-stu-id="c2624-226">Click on **ADD MEMBERS**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app7.png)

6. <span data-ttu-id="c2624-228">I hello **lägga till nya medlemmar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c2624-228">In hello **Add New Members** section.</span></span> <span data-ttu-id="c2624-229">Utför följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="c2624-229">Perform following actions:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app8.png)

    <span data-ttu-id="c2624-231">a.</span><span class="sxs-lookup"><span data-stu-id="c2624-231">a.</span></span> <span data-ttu-id="c2624-232">Ange **e-postadress** i hello **ange en e-postadress per rad** fältet.</span><span class="sxs-lookup"><span data-stu-id="c2624-232">Enter **Email address** in hello **Enter one email per line** field.</span></span>

    <span data-ttu-id="c2624-233">b.</span><span class="sxs-lookup"><span data-stu-id="c2624-233">b.</span></span> <span data-ttu-id="c2624-234">Välj Plese **rollen** enligt dina krav.</span><span class="sxs-lookup"><span data-stu-id="c2624-234">Plese select **Role** according your requirements.</span></span>

    <span data-ttu-id="c2624-235">c.</span><span class="sxs-lookup"><span data-stu-id="c2624-235">c.</span></span> <span data-ttu-id="c2624-236">Klicka på **Lägg till MEDLEMMAR**.</span><span class="sxs-lookup"><span data-stu-id="c2624-236">Click **ADD MEMBERS**.</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="c2624-237">hello Azure Active Directory användare får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="c2624-237">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="c2624-238">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c2624-238">Assign hello Azure AD test user</span></span>

<span data-ttu-id="c2624-239">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRFPIO.</span><span class="sxs-lookup"><span data-stu-id="c2624-239">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRFPIO.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c2624-241">**tooassign Britta Simon tooRFPIO utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c2624-241">**tooassign Britta Simon tooRFPIO, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2624-242">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c2624-242">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c2624-244">Välj i listan med program hello **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="c2624-244">In hello applications list, select **RFPIO**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_app.png) 

3. <span data-ttu-id="c2624-246">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c2624-246">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c2624-248">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c2624-248">Click **Add** button.</span></span> <span data-ttu-id="c2624-249">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2624-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c2624-251">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="c2624-251">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c2624-252">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2624-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c2624-253">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2624-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c2624-254">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2624-254">Test single sign-on</span></span>

<span data-ttu-id="c2624-255">I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c2624-255">In this section, you test your Azure AD single sign-on configuration by using hello Access Panel.</span></span>

<span data-ttu-id="c2624-256">Du bör få automatiskt inloggade tooyour RFPIO programmet när du klickar på hello RFPIO panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c2624-256">When you click hello RFPIO tile in hello Access Panel, you should get automatically signed-on tooyour RFPIO application.</span></span>
<span data-ttu-id="c2624-257">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c2624-257">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2624-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c2624-258">Additional resources</span></span>

* [<span data-ttu-id="c2624-259">Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c2624-259">List of tutorials about how toointegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c2624-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c2624-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_203.png

