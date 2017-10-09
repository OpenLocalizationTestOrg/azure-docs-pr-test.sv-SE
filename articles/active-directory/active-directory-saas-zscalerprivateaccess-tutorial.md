---
title: "Självstudier: Azure Active Directory-integrering med Zscaler privat åtkomst (ZPA) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zscaler privat åtkomst (ZPA)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 0370cff60c8ac15bd1919acccc924da1e50dc45b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a><span data-ttu-id="99cf7-103">Självstudier: Azure Active Directory-integrering med Zscaler privat åtkomst (ZPA)</span><span class="sxs-lookup"><span data-stu-id="99cf7-103">Tutorial: Azure Active Directory integration with Zscaler Private Access (ZPA)</span></span>

<span data-ttu-id="99cf7-104">I kursen får du lära dig hur toointegrate Zscaler privat åtkomst (ZPA) med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="99cf7-104">In this tutorial, you learn how toointegrate Zscaler Private Access (ZPA) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="99cf7-105">Integrera Zscaler privat åtkomst (ZPA) med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="99cf7-105">Integrating Zscaler Private Access (ZPA) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="99cf7-106">Du kan styra i Azure AD som har åtkomst tooZscaler privat åtkomst (ZPA)</span><span class="sxs-lookup"><span data-stu-id="99cf7-106">You can control in Azure AD who has access tooZscaler Private Access (ZPA)</span></span>
- <span data-ttu-id="99cf7-107">Du kan aktivera din användare tooautomatically get inloggade tooZscaler privat åtkomst (ZPA) (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="99cf7-107">You can enable your users tooautomatically get signed-on tooZscaler Private Access (ZPA) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="99cf7-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="99cf7-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="99cf7-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="99cf7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99cf7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="99cf7-110">Prerequisites</span></span>

<span data-ttu-id="99cf7-111">tooconfigure Azure AD-integrering med Zscaler privat åtkomst (ZPA), behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="99cf7-111">tooconfigure Azure AD integration with Zscaler Private Access (ZPA), you need hello following items:</span></span>

- <span data-ttu-id="99cf7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="99cf7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="99cf7-113">En Zscaler privat åtkomst (ZPA) enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="99cf7-113">A Zscaler Private Access (ZPA) single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="99cf7-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="99cf7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="99cf7-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="99cf7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="99cf7-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="99cf7-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="99cf7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="99cf7-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="99cf7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="99cf7-118">Scenario description</span></span>
<span data-ttu-id="99cf7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="99cf7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="99cf7-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="99cf7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="99cf7-121">Att lägga till Zscaler privat åtkomst (ZPA) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="99cf7-121">Adding Zscaler Private Access (ZPA) from hello gallery</span></span>
2. <span data-ttu-id="99cf7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99cf7-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zscaler-private-access-zpa-from-hello-gallery"></a><span data-ttu-id="99cf7-123">Att lägga till Zscaler privat åtkomst (ZPA) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="99cf7-123">Adding Zscaler Private Access (ZPA) from hello gallery</span></span>
<span data-ttu-id="99cf7-124">tooconfigure hello integrering av Zscaler privat åtkomst (ZPA) i Azure AD, behöver du tooadd Zscaler privat åtkomst (ZPA) från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="99cf7-124">tooconfigure hello integration of Zscaler Private Access (ZPA) into Azure AD, you need tooadd Zscaler Private Access (ZPA) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="99cf7-125">**tooadd Zscaler privat åtkomst (ZPA) från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="99cf7-125">**tooadd Zscaler Private Access (ZPA) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="99cf7-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="99cf7-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="99cf7-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="99cf7-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="99cf7-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99cf7-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="99cf7-133">Skriv i sökrutan hello **Zscaler privat åtkomst (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-133">In hello search box, type **Zscaler Private Access (ZPA)**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

5. <span data-ttu-id="99cf7-135">Markera hello resultat på panelen **Zscaler privat åtkomst (ZPA)**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="99cf7-135">In hello results panel, select **Zscaler Private Access (ZPA)**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="99cf7-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99cf7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="99cf7-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zscaler privat åtkomst (ZPA) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="99cf7-138">In this section, you configure and test Azure AD single sign-on with Zscaler Private Access (ZPA) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="99cf7-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Zscaler privat åtkomst (ZPA) är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99cf7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler Private Access (ZPA) is tooa user in Azure AD.</span></span> <span data-ttu-id="99cf7-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Zscaler privat åtkomst (ZPA) toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="99cf7-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler Private Access (ZPA) needs toobe established.</span></span>

<span data-ttu-id="99cf7-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Zscaler privat åtkomst (ZPA).</span><span class="sxs-lookup"><span data-stu-id="99cf7-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Zscaler Private Access (ZPA).</span></span>

<span data-ttu-id="99cf7-142">tooconfigure och testa Azure AD enkel inloggning med Zscaler privat åtkomst (ZPA), behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="99cf7-142">tooconfigure and test Azure AD single sign-on with Zscaler Private Access (ZPA), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="99cf7-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="99cf7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="99cf7-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99cf7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="99cf7-145">**[Skapa en testanvändare Zscaler privat åtkomst (ZPA)](#creating-a-zscaler-private-access-(zpa)-test-user)**  -toohave en motsvarighet för Britta Simon i Zscaler privat åtkomst (ZPA) som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="99cf7-145">**[Creating a Zscaler Private Access (ZPA) test user](#creating-a-zscaler-private-access-(zpa)-test-user)** - toohave a counterpart of Britta Simon in Zscaler Private Access (ZPA) that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="99cf7-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="99cf7-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="99cf7-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="99cf7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="99cf7-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99cf7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="99cf7-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i tillämpningsprogrammet Zscaler privat åtkomst (ZPA).</span><span class="sxs-lookup"><span data-stu-id="99cf7-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Zscaler Private Access (ZPA) application.</span></span>

<span data-ttu-id="99cf7-150">**tooconfigure Azure AD enkel inloggning med Zscaler privat åtkomst (ZPA), utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="99cf7-150">**tooconfigure Azure AD single sign-on with Zscaler Private Access (ZPA), perform hello following steps:**</span></span>

1. <span data-ttu-id="99cf7-151">I hello Azure Management portal på hello **Zscaler privat åtkomst (ZPA)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-151">In hello Azure Management portal, on hello **Zscaler Private Access (ZPA)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="99cf7-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="99cf7-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
3. <span data-ttu-id="99cf7-155">På hello **Zscaler privat åtkomst (ZPA)-domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="99cf7-155">On hello **Zscaler Private Access (ZPA) Domain and URLs** section, perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    <span data-ttu-id="99cf7-157">a.</span><span class="sxs-lookup"><span data-stu-id="99cf7-157">a.</span></span> <span data-ttu-id="99cf7-158">I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span><span class="sxs-lookup"><span data-stu-id="99cf7-158">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span></span>

    <span data-ttu-id="99cf7-159">b.</span><span class="sxs-lookup"><span data-stu-id="99cf7-159">b.</span></span> <span data-ttu-id="99cf7-160">I hello **identifierare** textruta typ:`https://samlsp.private.zscaler.com/auth/metadata`</span><span class="sxs-lookup"><span data-stu-id="99cf7-160">In hello **Identifier** textbox, type: `https://samlsp.private.zscaler.com/auth/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="99cf7-161">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="99cf7-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="99cf7-162">Du har tooupdate dessa värden med hello faktiska inloggning URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="99cf7-162">You have tooupdate these values with hello actual Sign On URL and Identifier.</span></span> <span data-ttu-id="99cf7-163">Vi rekommenderar här du toouse hello unikt värde för URL: en i hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="99cf7-163">Here we suggest you toouse hello unique value of URL in hello Identifier.</span></span> <span data-ttu-id="99cf7-164">Kontakta [Zscaler privat åtkomst (ZPA) supportteamet](https://help.zscaler.com/zpa-submit-ticket) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="99cf7-164">Contact [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) tooget these values.</span></span>

4. <span data-ttu-id="99cf7-165">På hello **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-165">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_400.png)   

5. <span data-ttu-id="99cf7-167">På hello **skapa nya certifikat** dialogrutan Klicka hello kalendern och välj en **förfallodatum**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-167">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="99cf7-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="99cf7-168">Then click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_500.png)

6. <span data-ttu-id="99cf7-170">På hello **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="99cf7-170">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

7. <span data-ttu-id="99cf7-172">På popup-fönster hello **förnyelsecertifikat** -fönstret klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-172">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_600.png)

8. <span data-ttu-id="99cf7-174">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="99cf7-174">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

9. <span data-ttu-id="99cf7-176">Logga in på webbplatsen för företagets Zscaler privat åtkomst (ZPA) som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="99cf7-176">In a different web browser window, log into your Zscaler Private Access (ZPA) company site as an administrator.</span></span>

10. <span data-ttu-id="99cf7-177">Navigera för**administratör** och klicka sedan på **Idp Configuration**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-177">Navigate too**Administrator** and then click **Idp Configuration**.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

11. <span data-ttu-id="99cf7-179">I hello **Idp Configuration** klickar du på **lägga till nya IDP konfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-179">In hello **Idp Configuration** section, click **Add New IDP Configuration**.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

12. <span data-ttu-id="99cf7-181">I hello **nya IDP konfigurationen** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="99cf7-181">In hello **New IDP Configuration** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    <span data-ttu-id="99cf7-183">a.</span><span class="sxs-lookup"><span data-stu-id="99cf7-183">a.</span></span> <span data-ttu-id="99cf7-184">Klicka på **Välj fil** och ladda upp din hämtade metadatafil.</span><span class="sxs-lookup"><span data-stu-id="99cf7-184">Click **Select File** and upload your downloaded metadata file.</span></span>

    <span data-ttu-id="99cf7-185">b.</span><span class="sxs-lookup"><span data-stu-id="99cf7-185">b.</span></span> <span data-ttu-id="99cf7-186">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="99cf7-186">Click **Save** button.</span></span>
    


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="99cf7-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="99cf7-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="99cf7-188">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99cf7-188">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="99cf7-190">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="99cf7-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="99cf7-191">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="99cf7-191">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="99cf7-193">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="99cf7-193">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="99cf7-195">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99cf7-195">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="99cf7-197">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="99cf7-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="99cf7-199">a.</span><span class="sxs-lookup"><span data-stu-id="99cf7-199">a.</span></span> <span data-ttu-id="99cf7-200">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="99cf7-201">b.</span><span class="sxs-lookup"><span data-stu-id="99cf7-201">b.</span></span> <span data-ttu-id="99cf7-202">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="99cf7-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="99cf7-203">c.</span><span class="sxs-lookup"><span data-stu-id="99cf7-203">c.</span></span> <span data-ttu-id="99cf7-204">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="99cf7-205">d.</span><span class="sxs-lookup"><span data-stu-id="99cf7-205">d.</span></span> <span data-ttu-id="99cf7-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-206">Click **Create**.</span></span> 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a><span data-ttu-id="99cf7-207">Skapa en testanvändare Zscaler privat åtkomst (ZPA)</span><span class="sxs-lookup"><span data-stu-id="99cf7-207">Creating a Zscaler Private Access (ZPA) test user</span></span>

<span data-ttu-id="99cf7-208">I det här avsnittet skapar du en användare som kallas Britta Simon i Zscaler privat åtkomst (ZPA).</span><span class="sxs-lookup"><span data-stu-id="99cf7-208">In this section, you create a user called Britta Simon in Zscaler Private Access (ZPA).</span></span> <span data-ttu-id="99cf7-209">Se tillsammans med [Zscaler privat åtkomst (ZPA) supportteamet](https://help.zscaler.com/zpa-submit-ticket) tooadd hello användare i hello Zscaler privat åtkomst (ZPA)-plattformen.</span><span class="sxs-lookup"><span data-stu-id="99cf7-209">Please work with [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) tooadd hello users in hello Zscaler Private Access (ZPA) platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="99cf7-210">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="99cf7-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="99cf7-211">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooZscaler privat åtkomst (ZPA).</span><span class="sxs-lookup"><span data-stu-id="99cf7-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooZscaler Private Access (ZPA).</span></span>

![Tilldela användare][200] 

<span data-ttu-id="99cf7-213">**tooassign Britta Simon tooZscaler privat åtkomst (ZPA), utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="99cf7-213">**tooassign Britta Simon tooZscaler Private Access (ZPA), perform hello following steps:**</span></span>

1. <span data-ttu-id="99cf7-214">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-214">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="99cf7-216">Välj i listan med program hello **Zscaler privat åtkomst (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-216">In hello applications list, select **Zscaler Private Access (ZPA)**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

3. <span data-ttu-id="99cf7-218">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="99cf7-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="99cf7-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="99cf7-220">Click **Add** button.</span></span> <span data-ttu-id="99cf7-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99cf7-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="99cf7-223">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="99cf7-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="99cf7-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99cf7-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="99cf7-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99cf7-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="99cf7-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99cf7-226">Testing single sign-on</span></span>

<span data-ttu-id="99cf7-227">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="99cf7-227">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="99cf7-228">När du klickar på hello Zscaler privat åtkomst (ZPA)-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Zscaler privat åtkomst (ZPA) program.</span><span class="sxs-lookup"><span data-stu-id="99cf7-228">When you click hello Zscaler Private Access (ZPA) tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler Private Access (ZPA) application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="99cf7-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="99cf7-229">Additional resources</span></span>

* [<span data-ttu-id="99cf7-230">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="99cf7-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="99cf7-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="99cf7-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_203.png