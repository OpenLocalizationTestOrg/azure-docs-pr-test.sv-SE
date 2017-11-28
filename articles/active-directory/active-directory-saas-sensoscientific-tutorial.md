---
title: "Självstudier: Azure Active Directory-integrering med SensoScientific trådlösa temperatur övervakningssystem | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SensoScientific trådlösa temperatur övervakning av systemet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: 4eabf7fc6457c217fd5c0c2539ab88c8110055e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a><span data-ttu-id="4f5ea-103">Självstudier: Azure Active Directory-integrering med SensoScientific trådlösa temperatur övervakningssystem</span><span class="sxs-lookup"><span data-stu-id="4f5ea-103">Tutorial: Azure Active Directory integration with SensoScientific Wireless Temperature Monitoring System</span></span>

<span data-ttu-id="4f5ea-104">I kursen får du lära dig hur toointegrate SensoScientific trådlösa temperatur övervakningssystem med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4f5ea-104">In this tutorial, you learn how toointegrate SensoScientific Wireless Temperature Monitoring System with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4f5ea-105">Integrera SensoScientific trådlösa temperatur övervakningssystem med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4f5ea-105">Integrating SensoScientific Wireless Temperature Monitoring System with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4f5ea-106">Du kan styra i Azure AD som har åtkomst tooSensoScientific övervakningssystem för trådlösa temperatur</span><span class="sxs-lookup"><span data-stu-id="4f5ea-106">You can control in Azure AD who has access tooSensoScientific Wireless Temperature Monitoring System</span></span>
- <span data-ttu-id="4f5ea-107">Du kan aktivera din användare tooautomatically get inloggade tooSensoScientific trådlösa temperatur övervakningssystem (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="4f5ea-107">You can enable your users tooautomatically get signed-on tooSensoScientific Wireless Temperature Monitoring System (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4f5ea-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4f5ea-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4f5ea-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4f5ea-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f5ea-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4f5ea-110">Prerequisites</span></span>

<span data-ttu-id="4f5ea-111">tooconfigure Azure AD-integrering med SensoScientific trådlösa temperatur övervakningssystem måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4f5ea-111">tooconfigure Azure AD integration with SensoScientific Wireless Temperature Monitoring System, you need hello following items:</span></span>

- <span data-ttu-id="4f5ea-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4f5ea-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4f5ea-113">En SensoScientific trådlösa temperatur övervakningssystem enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="4f5ea-113">A SensoScientific Wireless Temperature Monitoring System single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4f5ea-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4f5ea-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4f5ea-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4f5ea-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4f5ea-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4f5ea-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4f5ea-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4f5ea-118">Scenario description</span></span>
<span data-ttu-id="4f5ea-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4f5ea-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4f5ea-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4f5ea-121">Att lägga till SensoScientific trådlösa temperatur övervakningssystem från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4f5ea-121">Adding SensoScientific Wireless Temperature Monitoring System from hello gallery</span></span>
2. <span data-ttu-id="4f5ea-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f5ea-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-hello-gallery"></a><span data-ttu-id="4f5ea-123">Att lägga till SensoScientific trådlösa temperatur övervakningssystem från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4f5ea-123">Adding SensoScientific Wireless Temperature Monitoring System from hello gallery</span></span>
<span data-ttu-id="4f5ea-124">tooconfigure hello integrering av SensoScientific trådlösa temperatur övervakningssystem i Azure AD, behöver du tooadd SensoScientific trådlösa temperatur övervakningssystem hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-124">tooconfigure hello integration of SensoScientific Wireless Temperature Monitoring System into Azure AD, you need tooadd SensoScientific Wireless Temperature Monitoring System from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4f5ea-125">**tooadd SensoScientific trådlösa temperatur övervakningssystem från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4f5ea-125">**tooadd SensoScientific Wireless Temperature Monitoring System from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4f5ea-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4f5ea-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4f5ea-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4f5ea-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="4f5ea-133">Skriv i sökrutan hello **SensoScientific trådlösa temperatur övervakningssystem**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-133">In hello search box, type **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

5. <span data-ttu-id="4f5ea-135">Markera hello resultat på panelen **SensoScientific trådlösa temperatur övervakningssystem**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-135">In hello results panel, select **SensoScientific Wireless Temperature Monitoring System**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4f5ea-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f5ea-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4f5ea-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SensoScientific trådlösa temperatur övervakningssystem baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-138">In this section, you configure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4f5ea-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SensoScientific trådlösa temperatur övervakningssystem är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SensoScientific Wireless Temperature Monitoring System is tooa user in Azure AD.</span></span> <span data-ttu-id="4f5ea-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i SensoScientific trådlösa temperatur övervakningssystem toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-140">In other words, a link relationship between an Azure AD user and hello related user in SensoScientific Wireless Temperature Monitoring System needs toobe established.</span></span>

<span data-ttu-id="4f5ea-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i SensoScientific trådlösa temperatur övervakningssystem.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SensoScientific Wireless Temperature Monitoring System.</span></span>

<span data-ttu-id="4f5ea-142">tooconfigure och testa Azure AD enkel inloggning med SensoScientific trådlösa temperatur övervakningssystem, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4f5ea-142">tooconfigure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4f5ea-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4f5ea-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4f5ea-145">**[Skapa en testanvändare SensoScientific trådlösa temperatur övervakningssystem](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**  -toohave en motsvarighet för Britta Simon i SensoScientific trådlösa temperatur övervakningssystem som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-145">**[Creating a SensoScientific Wireless Temperature Monitoring System test user](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)** - toohave a counterpart of Britta Simon in SensoScientific Wireless Temperature Monitoring System that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4f5ea-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4f5ea-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4f5ea-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f5ea-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4f5ea-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt övervakningssystem för SensoScientific trådlösa temperatur-program.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SensoScientific Wireless Temperature Monitoring System application.</span></span>

<span data-ttu-id="4f5ea-150">**tooconfigure Azure AD enkel inloggning med SensoScientific trådlösa temperatur övervakning, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="4f5ea-150">**tooconfigure Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, perform hello following steps:**</span></span>

1. <span data-ttu-id="4f5ea-151">I hello Azure-portalen på hello **SensoScientific trådlösa temperatur övervakningssystem** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-151">In hello Azure portal, on hello **SensoScientific Wireless Temperature Monitoring System** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4f5ea-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

3. <span data-ttu-id="4f5ea-155">På hello **SensoScientific trådlösa temperatur övervakning domän och URL: er** avsnitt, utan behov av tooperform eventuella åtgärder som hello app redan redan är integrerade med Azure:</span><span class="sxs-lookup"><span data-stu-id="4f5ea-155">On hello **SensoScientific Wireless Temperature Monitoring System Domain and URLs** section, no need tooperform any steps as hello app is already pre-integrated with Azure:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

4. <span data-ttu-id="4f5ea-157">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-157">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

5. <span data-ttu-id="4f5ea-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4f5ea-161">På hello **övervakning systemkonfigurationen för SensoScientific trådlösa temperatur** klickar du på **konfigurera SensoScientific trådlösa temperatur övervakningssystem** tooopen  **Konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-161">On hello **SensoScientific Wireless Temperature Monitoring System Configuration** section, click **Configure SensoScientific Wireless Temperature Monitoring System** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4f5ea-162">Kopiera hello **Sign-Out URL, SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4f5ea-162">Copy hello **Sign-Out URL, SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

7. <span data-ttu-id="4f5ea-164">Logga in tooyour SensoScientific trådlösa temperatur övervakningssystem programmet som administratör.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-164">Sign on tooyour SensoScientific Wireless Temperature Monitoring System application as an administrator.</span></span>

8. <span data-ttu-id="4f5ea-165">Klicka i hello navigeringsmenyn överst hello **Configuration** och gå till **konfigurera** under **enkel inloggning** tooopen hello enkel inloggning på inställningar.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-165">In hello navigation menu on hello top, click **Configuration** and goto **Configure** under **Single Sign On** tooopen hello Single Sign On Settings.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

9. <span data-ttu-id="4f5ea-167">I **enkel inloggning på inställningar** formuläret utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4f5ea-167">In **Single Sign On Settings** form perform hello following steps:</span></span>
 
    <span data-ttu-id="4f5ea-168">a.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-168">a.</span></span> <span data-ttu-id="4f5ea-169">Välj **utfärdarnamnet** som Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-169">Select **Issuer Name** as Azure AD.</span></span>
    
    <span data-ttu-id="4f5ea-170">b.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-170">b.</span></span> <span data-ttu-id="4f5ea-171">Klistra in hello **SAML enhets-ID** som du har kopierat från Azure-portalen i utfärdar-URL-textrutan.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-171">Paste hello **SAML Entity ID** which you have copied from Azure portal into Issuer URL textbox.</span></span>
    
    <span data-ttu-id="4f5ea-172">c.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-172">c.</span></span> <span data-ttu-id="4f5ea-173">Klistra in hello **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen i textrutan för enkel inloggning tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-173">Paste hello **SAML Single Sign-On Service URL** which you have copied from Azure portal into Single Sign-On Service URL textbox.</span></span>

    <span data-ttu-id="4f5ea-174">d.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-174">d.</span></span> <span data-ttu-id="4f5ea-175">Klistra in hello **Sign-Out URL** som du har kopierat från Azure-portalen i tjänst-URL för enkel Sign-Out textruta.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-175">Paste hello **Sign-Out URL** which you have copied from Azure portal into Single Sign-Out Service URL textbox.</span></span>

    <span data-ttu-id="4f5ea-176">e.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-176">e.</span></span> <span data-ttu-id="4f5ea-177">Bläddra hello-certifikat som du har hämtat från Azure-portalen och överföra här.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-177">Browse hello certificate which you have downloaded from Azure portal and upload here.</span></span>
    
    <span data-ttu-id="4f5ea-178">f.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-178">f.</span></span> <span data-ttu-id="4f5ea-179">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-179">Click **Save**.</span></span>
  
> [!TIP]
> <span data-ttu-id="4f5ea-180">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="4f5ea-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4f5ea-181">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4f5ea-182">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4f5ea-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4f5ea-183">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4f5ea-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="4f5ea-184">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4f5ea-186">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4f5ea-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4f5ea-187">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4f5ea-189">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4f5ea-191">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4f5ea-193">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4f5ea-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4f5ea-195">a.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-195">a.</span></span> <span data-ttu-id="4f5ea-196">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4f5ea-197">b.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-197">b.</span></span> <span data-ttu-id="4f5ea-198">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4f5ea-199">c.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-199">c.</span></span> <span data-ttu-id="4f5ea-200">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4f5ea-201">d.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-201">d.</span></span> <span data-ttu-id="4f5ea-202">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-202">Click **Create**.</span></span>
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a><span data-ttu-id="4f5ea-203">Skapa en SensoScientific trådlösa temperatur övervakningssystem testanvändare</span><span class="sxs-lookup"><span data-stu-id="4f5ea-203">Creating a SensoScientific Wireless Temperature Monitoring System test user</span></span>

<span data-ttu-id="4f5ea-204">tooenable Azure AD-användare toolog i tooSensoScientific övervakningssystem för trådlösa temperatur de att etableras i SensoScientific trådlösa temperatur övervakningssystem.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-204">tooenable Azure AD users toolog in tooSensoScientific Wireless Temperature Monitoring System, they must be provisioned into SensoScientific Wireless Temperature Monitoring System.</span></span> <span data-ttu-id="4f5ea-205">Arbeta med [SensoScientific trådlösa temperatur övervakningssystem supportteamet](https://www.sensoscientific.com/contact-us/) att lägga till hello användare i hello SensoScientific trådlösa temperatur övervakning plattform.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-205">Work with [SensoScientific Wireless Temperature Monitoring System support team](https://www.sensoscientific.com/contact-us/) to add hello users in hello SensoScientific Wireless Temperature Monitoring System platform.</span></span> <span data-ttu-id="4f5ea-206">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-206">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4f5ea-207">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="4f5ea-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4f5ea-208">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSensoScientific övervakningssystem för trådlösa temperatur.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSensoScientific Wireless Temperature Monitoring System.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4f5ea-210">**tooassign Britta Simon tooSensoScientific trådlösa temperatur övervakningssystem, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4f5ea-210">**tooassign Britta Simon tooSensoScientific Wireless Temperature Monitoring System, perform hello following steps:**</span></span>

1. <span data-ttu-id="4f5ea-211">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4f5ea-213">Välj i listan med program hello **SensoScientific trådlösa temperatur övervakningssystem**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-213">In hello applications list, select **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

3. <span data-ttu-id="4f5ea-215">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4f5ea-217">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-217">Click **Add** button.</span></span> <span data-ttu-id="4f5ea-218">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4f5ea-220">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4f5ea-221">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4f5ea-222">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4f5ea-223">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f5ea-223">Testing single sign-on</span></span>

<span data-ttu-id="4f5ea-224">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span> <span data-ttu-id="4f5ea-225">Klicka på hello SensoScientific trådlösa temperatur övervakningssystem panelen i hello åtkomstpanelen, kommer du att automatiskt inloggade tooyour SensoScientific trådlösa temperatur övervakning systemprogram.</span><span class="sxs-lookup"><span data-stu-id="4f5ea-225">Click hello SensoScientific Wireless Temperature Monitoring System tile in hello Access Panel, you will be automatically signed-on tooyour SensoScientific Wireless Temperature Monitoring System application.</span></span> <span data-ttu-id="4f5ea-226">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="4f5ea-226">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4f5ea-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4f5ea-227">Additional resources</span></span>

* [<span data-ttu-id="4f5ea-228">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4f5ea-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4f5ea-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4f5ea-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_203.png

