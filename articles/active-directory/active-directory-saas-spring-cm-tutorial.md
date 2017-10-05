---
title: "Självstudier: Azure Active Directory-integrering med SpringCM | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SpringCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: edfd06a06c730597fee4569ca1ce29092b45244a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a><span data-ttu-id="eb505-103">Självstudier: Azure Active Directory-integrering med SpringCM</span><span class="sxs-lookup"><span data-stu-id="eb505-103">Tutorial: Azure Active Directory integration with SpringCM</span></span>

<span data-ttu-id="eb505-104">I kursen får lära du att integrera SpringCM med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="eb505-104">In this tutorial, you learn how to integrate SpringCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="eb505-105">Integrera SpringCM med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="eb505-105">Integrating SpringCM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="eb505-106">Du kan styra i Azure AD som har åtkomst till SpringCM</span><span class="sxs-lookup"><span data-stu-id="eb505-106">You can control in Azure AD who has access to SpringCM</span></span>
- <span data-ttu-id="eb505-107">Du kan aktivera användarna att automatiskt hämta loggat in på SpringCM (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="eb505-107">You can enable your users to automatically get signed-on to SpringCM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="eb505-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="eb505-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="eb505-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="eb505-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eb505-110">Krav</span><span class="sxs-lookup"><span data-stu-id="eb505-110">Prerequisites</span></span>

<span data-ttu-id="eb505-111">För att konfigurera Azure AD-integrering med SpringCM, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="eb505-111">To configure Azure AD integration with SpringCM, you need the following items:</span></span>

- <span data-ttu-id="eb505-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="eb505-112">An Azure AD subscription</span></span>
- <span data-ttu-id="eb505-113">En SpringCM enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="eb505-113">A SpringCM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="eb505-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="eb505-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="eb505-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="eb505-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="eb505-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="eb505-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="eb505-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eb505-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="eb505-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="eb505-118">Scenario description</span></span>
<span data-ttu-id="eb505-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="eb505-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="eb505-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="eb505-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="eb505-121">Att lägga till SpringCM från galleriet</span><span class="sxs-lookup"><span data-stu-id="eb505-121">Adding SpringCM from the gallery</span></span>
2. <span data-ttu-id="eb505-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eb505-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-springcm-from-the-gallery"></a><span data-ttu-id="eb505-123">Att lägga till SpringCM från galleriet</span><span class="sxs-lookup"><span data-stu-id="eb505-123">Adding SpringCM from the gallery</span></span>
<span data-ttu-id="eb505-124">Du måste lägga till SpringCM från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av SpringCM i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eb505-124">To configure the integration of SpringCM into Azure AD, you need to add SpringCM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="eb505-125">**Utför följande steg för att lägga till SpringCM från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="eb505-125">**To add SpringCM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="eb505-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="eb505-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="eb505-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="eb505-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="eb505-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="eb505-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="eb505-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eb505-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="eb505-133">I sökrutan skriver **SpringCM**.</span><span class="sxs-lookup"><span data-stu-id="eb505-133">In the search box, type **SpringCM**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_search.png)

5. <span data-ttu-id="eb505-135">Välj i resultatpanelen **SpringCM**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="eb505-135">In the results panel, select **SpringCM**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="eb505-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eb505-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="eb505-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SpringCM baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="eb505-138">In this section, you configure and test Azure AD single sign-on with SpringCM based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="eb505-139">Azure AD måste du känna till användaren i SpringCM motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="eb505-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SpringCM is to a user in Azure AD.</span></span> <span data-ttu-id="eb505-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SpringCM upprättas.</span><span class="sxs-lookup"><span data-stu-id="eb505-140">In other words, a link relationship between an Azure AD user and the related user in SpringCM needs to be established.</span></span>

<span data-ttu-id="eb505-141">I SpringCM, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="eb505-141">In SpringCM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="eb505-142">Om du vill konfigurera och testa Azure AD enkel inloggning med SpringCM, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="eb505-142">To configure and test Azure AD single sign-on with SpringCM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="eb505-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="eb505-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="eb505-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eb505-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="eb505-145">**[Skapa en testanvändare SpringCM](#creating-a-springcm-test-user)**  – du har en motsvarighet för Britta Simon i SpringCM som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="eb505-145">**[Creating a SpringCM test user](#creating-a-springcm-test-user)** - to have a counterpart of Britta Simon in SpringCM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="eb505-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="eb505-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="eb505-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="eb505-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="eb505-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eb505-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="eb505-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt SpringCM program.</span><span class="sxs-lookup"><span data-stu-id="eb505-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SpringCM application.</span></span>

<span data-ttu-id="eb505-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SpringCM:**</span><span class="sxs-lookup"><span data-stu-id="eb505-150">**To configure Azure AD single sign-on with SpringCM, perform the following steps:**</span></span>

1. <span data-ttu-id="eb505-151">I Azure-portalen på den **SpringCM** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="eb505-151">In the Azure portal, on the **SpringCM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="eb505-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="eb505-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_samlbase.png)

3. <span data-ttu-id="eb505-155">På den **SpringCM domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="eb505-155">On the **SpringCM Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_url.png)

    <span data-ttu-id="eb505-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`</span><span class="sxs-lookup"><span data-stu-id="eb505-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="eb505-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="eb505-158">This value is not real.</span></span> <span data-ttu-id="eb505-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="eb505-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="eb505-160">Kontakta [SpringCM klienten supportteamet](https://knowledge.springcm.com/support) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="eb505-160">Contact [SpringCM Client support team](https://knowledge.springcm.com/support) to get this value.</span></span> 
 
4. <span data-ttu-id="eb505-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Raw)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="eb505-161">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_certificate.png) 

5. <span data-ttu-id="eb505-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="eb505-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="eb505-165">På den **SpringCM Configuration** klickar du på **konfigurera SpringCM** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="eb505-165">On the **SpringCM Configuration** section, click **Configure SpringCM** to open **Configure sign-on** window.</span></span> <span data-ttu-id="eb505-166">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="eb505-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_configure.png)   

7. <span data-ttu-id="eb505-168">I en annan webbläsarfönstret, logga in på ditt **SpringCM** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="eb505-168">In a different web browser window, sign on to your **SpringCM** company site as administrator.</span></span>

8. <span data-ttu-id="eb505-169">Klicka på menyn högst upp **gå till**, klickar du på **inställningar**, och sedan, i den **kontoinställningar** klickar du på **SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="eb505-169">In the menu on the top, click **GO TO**, click **Preferences**, and then, in the **Account Preferences** section, click **SAML SSO**.</span></span>
   
    <span data-ttu-id="eb505-170">![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "SAML SSO")</span><span class="sxs-lookup"><span data-stu-id="eb505-170">![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "SAML SSO")</span></span>

9. <span data-ttu-id="eb505-171">Utför följande steg i konfigurationsavsnittet identitet providern:</span><span class="sxs-lookup"><span data-stu-id="eb505-171">In the Identity Provider Configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="eb505-172">![Identitet providerkonfigurationen](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "identitet providerkonfigurationen")</span><span class="sxs-lookup"><span data-stu-id="eb505-172">![Identity Provider Configuration](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "Identity Provider Configuration")</span></span>
    
    <span data-ttu-id="eb505-173">a.</span><span class="sxs-lookup"><span data-stu-id="eb505-173">a.</span></span> <span data-ttu-id="eb505-174">Om du vill överföra din hämtat Azure Active Directory-certifikat, klickar du på **Välj utfärdarcertifikatet** eller **ändra utfärdarcertifikatet**.</span><span class="sxs-lookup"><span data-stu-id="eb505-174">To upload your downloaded Azure Active Directory certificate, click **Select Issuer Certificate** or **Change Issuer Certificate**.</span></span>
    
    <span data-ttu-id="eb505-175">b.</span><span class="sxs-lookup"><span data-stu-id="eb505-175">b.</span></span> <span data-ttu-id="eb505-176">Klistra in **SAML enhets-ID** -värde som du har kopierat från Azure-portalen i den **utfärdaren** textruta.</span><span class="sxs-lookup"><span data-stu-id="eb505-176">Paste **SAML Entity ID** value, which you have copied from Azure portal into the **Issuer** textbox.</span></span>
    
    <span data-ttu-id="eb505-177">c.</span><span class="sxs-lookup"><span data-stu-id="eb505-177">c.</span></span> <span data-ttu-id="eb505-178">Klistra in **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från Azure-portalen i den **Service Provider (SP) initierade Endpoint** textruta.</span><span class="sxs-lookup"><span data-stu-id="eb505-178">Paste **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **Service Provider (SP) Initiated Endpoint** textbox.</span></span>
            
    <span data-ttu-id="eb505-179">d.</span><span class="sxs-lookup"><span data-stu-id="eb505-179">d.</span></span> <span data-ttu-id="eb505-180">Välj **SAML aktiverat** som **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="eb505-180">Select **SAML Enabled** as **Enable**.</span></span>

    <span data-ttu-id="eb505-181">e.</span><span class="sxs-lookup"><span data-stu-id="eb505-181">e.</span></span> <span data-ttu-id="eb505-182">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="eb505-182">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="eb505-183">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="eb505-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="eb505-184">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="eb505-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="eb505-185">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="eb505-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="eb505-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="eb505-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="eb505-187">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eb505-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="eb505-189">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="eb505-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="eb505-190">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="eb505-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="eb505-192">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="eb505-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="eb505-194">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eb505-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="eb505-196">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="eb505-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="eb505-198">a.</span><span class="sxs-lookup"><span data-stu-id="eb505-198">a.</span></span> <span data-ttu-id="eb505-199">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="eb505-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="eb505-200">b.</span><span class="sxs-lookup"><span data-stu-id="eb505-200">b.</span></span> <span data-ttu-id="eb505-201">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="eb505-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="eb505-202">c.</span><span class="sxs-lookup"><span data-stu-id="eb505-202">c.</span></span> <span data-ttu-id="eb505-203">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="eb505-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="eb505-204">d.</span><span class="sxs-lookup"><span data-stu-id="eb505-204">d.</span></span> <span data-ttu-id="eb505-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="eb505-205">Click **Create**.</span></span>
 
### <a name="creating-a-springcm-test-user"></a><span data-ttu-id="eb505-206">Skapa en testanvändare SpringCM</span><span class="sxs-lookup"><span data-stu-id="eb505-206">Creating a SpringCM test user</span></span>

<span data-ttu-id="eb505-207">Om du vill aktivera Azure Active Directory-användare kan logga in på SpringCM etableras de i SpringCM.</span><span class="sxs-lookup"><span data-stu-id="eb505-207">To enable Azure Active Directory users to log in to SpringCM, they must be provisioned into SpringCM.</span></span> <span data-ttu-id="eb505-208">När det gäller SpringCM är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="eb505-208">In the case of SpringCM, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="eb505-209">Mer information finns i [skapa och redigera användare SpringCM](http://knowledge.springcm.com/create-and-edit-a-springcm-user).</span><span class="sxs-lookup"><span data-stu-id="eb505-209">For more information, see [Create and Edit a SpringCM User](http://knowledge.springcm.com/create-and-edit-a-springcm-user).</span></span> 

<span data-ttu-id="eb505-210">**Utför följande steg för att etablera ett användarkonto för SpringCM:**</span><span class="sxs-lookup"><span data-stu-id="eb505-210">**To provision a user account to SpringCM, perform the following steps:**</span></span>

1. <span data-ttu-id="eb505-211">Logga in på ditt **SpringCM** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="eb505-211">Log in to your **SpringCM** company site as administrator.</span></span>

2. <span data-ttu-id="eb505-212">Klicka på **GOTO**, och klicka sedan på **ADRESSBOKEN**.</span><span class="sxs-lookup"><span data-stu-id="eb505-212">Click **GOTO**, and then click **ADDRESS BOOK**.</span></span>
   
    <span data-ttu-id="eb505-213">![Skapa användare](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="eb505-213">![Create User](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "Create User")</span></span>

3. <span data-ttu-id="eb505-214">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="eb505-214">Click **Create User**.</span></span>

4. <span data-ttu-id="eb505-215">Välj en **användarrollen**.</span><span class="sxs-lookup"><span data-stu-id="eb505-215">Select a **User Role**.</span></span>

5. <span data-ttu-id="eb505-216">Välj **skickar Aktiveringsmeddelandet**.</span><span class="sxs-lookup"><span data-stu-id="eb505-216">Select **Send Activation Email**.</span></span>

6. <span data-ttu-id="eb505-217">Ange förnamn, efternamn och e-postadressen till ett giltigt Azure Active Directory-användarkonto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="eb505-217">Type the first name, last name, and email address of a valid Azure Active Directory user account you want to provision into the related textboxes.</span></span>

7. <span data-ttu-id="eb505-218">Lägg till användaren till en **säkerhetsgrupp**.</span><span class="sxs-lookup"><span data-stu-id="eb505-218">Add the user to a **Security group**.</span></span>

8. <span data-ttu-id="eb505-219">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="eb505-219">Click **Save**.</span></span>

  >[!NOTE]
  ><span data-ttu-id="eb505-220">Du kan använda något annat SpringCM användarens konto skapas verktyg eller API: er som tillhandahålls av SpringCM att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="eb505-220">You can use any other SpringCM user account creation tools or APIs provided by SpringCM to provision AAD user accounts.</span></span>  
  > 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="eb505-221">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="eb505-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="eb505-222">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till SpringCM.</span><span class="sxs-lookup"><span data-stu-id="eb505-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SpringCM.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="eb505-224">**Om du vill tilldela SpringCM Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="eb505-224">**To assign Britta Simon to SpringCM, perform the following steps:**</span></span>

1. <span data-ttu-id="eb505-225">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="eb505-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="eb505-227">Välj i listan med program **SpringCM**.</span><span class="sxs-lookup"><span data-stu-id="eb505-227">In the applications list, select **SpringCM**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_app.png) 

3. <span data-ttu-id="eb505-229">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="eb505-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="eb505-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="eb505-231">Click **Add** button.</span></span> <span data-ttu-id="eb505-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eb505-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="eb505-234">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="eb505-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="eb505-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eb505-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="eb505-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eb505-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="eb505-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eb505-237">Testing single sign-on</span></span>

<span data-ttu-id="eb505-238">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="eb505-238">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
 
<span data-ttu-id="eb505-239">När du klickar på panelen SpringCM på åtkomstpanelen du bör få automatiskt loggat in på ditt SpringCM program.</span><span class="sxs-lookup"><span data-stu-id="eb505-239">When you click the SpringCM tile in the Access Panel, you should get automatically signed-on to your SpringCM application.</span></span>

<span data-ttu-id="eb505-240">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="eb505-240">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="eb505-241">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="eb505-241">Additional resources</span></span>

* [<span data-ttu-id="eb505-242">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eb505-242">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="eb505-243">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="eb505-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_203.png

