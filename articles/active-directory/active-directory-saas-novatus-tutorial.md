---
title: "Självstudier: Azure Active Directory-integrering med Novatus | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Novatus."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2f13779-bdb7-4408-9738-be67ed3de4e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2017
ms.author: jeedes
ms.openlocfilehash: ec67e96309a8877e6fb65b30da1501e4f34a9ee4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-novatus"></a><span data-ttu-id="36bac-103">Självstudier: Azure Active Directory-integrering med Novatus</span><span class="sxs-lookup"><span data-stu-id="36bac-103">Tutorial: Azure Active Directory integration with Novatus</span></span>

<span data-ttu-id="36bac-104">I kursen får lära du att integrera Novatus med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="36bac-104">In this tutorial, you learn how to integrate Novatus with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="36bac-105">Integrera Novatus med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="36bac-105">Integrating Novatus with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="36bac-106">Du kan styra i Azure AD som har åtkomst till Novatus</span><span class="sxs-lookup"><span data-stu-id="36bac-106">You can control in Azure AD who has access to Novatus</span></span>
- <span data-ttu-id="36bac-107">Du kan aktivera användarna att automatiskt hämta loggat in på Novatus (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="36bac-107">You can enable your users to automatically get signed-on to Novatus (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="36bac-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="36bac-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="36bac-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="36bac-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36bac-110">Krav</span><span class="sxs-lookup"><span data-stu-id="36bac-110">Prerequisites</span></span>

<span data-ttu-id="36bac-111">För att konfigurera Azure AD-integrering med Novatus, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="36bac-111">To configure Azure AD integration with Novatus, you need the following items:</span></span>

- <span data-ttu-id="36bac-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="36bac-112">An Azure AD subscription</span></span>
- <span data-ttu-id="36bac-113">En Novatus enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="36bac-113">A Novatus single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="36bac-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="36bac-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="36bac-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="36bac-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="36bac-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="36bac-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="36bac-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="36bac-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="36bac-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="36bac-118">Scenario description</span></span>
<span data-ttu-id="36bac-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="36bac-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="36bac-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="36bac-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="36bac-121">Att lägga till Novatus från galleriet</span><span class="sxs-lookup"><span data-stu-id="36bac-121">Adding Novatus from the gallery</span></span>
2. <span data-ttu-id="36bac-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="36bac-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-novatus-from-the-gallery"></a><span data-ttu-id="36bac-123">Att lägga till Novatus från galleriet</span><span class="sxs-lookup"><span data-stu-id="36bac-123">Adding Novatus from the gallery</span></span>
<span data-ttu-id="36bac-124">Du måste lägga till Novatus från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Novatus i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="36bac-124">To configure the integration of Novatus into Azure AD, you need to add Novatus from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="36bac-125">**Utför följande steg för att lägga till Novatus från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="36bac-125">**To add Novatus from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="36bac-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="36bac-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="36bac-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="36bac-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="36bac-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="36bac-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="36bac-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="36bac-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="36bac-133">I sökrutan skriver **Novatus**.</span><span class="sxs-lookup"><span data-stu-id="36bac-133">In the search box, type **Novatus**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_search.png)

5. <span data-ttu-id="36bac-135">Välj i resultatpanelen **Novatus**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="36bac-135">In the results panel, select **Novatus**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="36bac-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="36bac-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="36bac-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Novatus baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="36bac-138">In this section, you configure and test Azure AD single sign-on with Novatus based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="36bac-139">Azure AD måste du känna till användaren i Novatus motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="36bac-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Novatus is to a user in Azure AD.</span></span> <span data-ttu-id="36bac-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Novatus upprättas.</span><span class="sxs-lookup"><span data-stu-id="36bac-140">In other words, a link relationship between an Azure AD user and the related user in Novatus needs to be established.</span></span>

<span data-ttu-id="36bac-141">I Novatus, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="36bac-141">In Novatus, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="36bac-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Novatus, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="36bac-142">To configure and test Azure AD single sign-on with Novatus, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="36bac-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="36bac-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="36bac-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="36bac-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="36bac-145">**[Skapa en testanvändare Novatus](#creating-a-novatus-test-user)**  – du har en motsvarighet för Britta Simon i Novatus som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="36bac-145">**[Creating a Novatus test user](#creating-a-novatus-test-user)** - to have a counterpart of Britta Simon in Novatus that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="36bac-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="36bac-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="36bac-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="36bac-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="36bac-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="36bac-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="36bac-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Novatus program.</span><span class="sxs-lookup"><span data-stu-id="36bac-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Novatus application.</span></span>

<span data-ttu-id="36bac-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Novatus:**</span><span class="sxs-lookup"><span data-stu-id="36bac-150">**To configure Azure AD single sign-on with Novatus, perform the following steps:**</span></span>

1. <span data-ttu-id="36bac-151">I Azure-portalen på den **Novatus** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="36bac-151">In the Azure portal, on the **Novatus** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="36bac-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="36bac-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_samlbase.png)

3. <span data-ttu-id="36bac-155">På den **Novatus domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="36bac-155">On the **Novatus Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_url.png)

     <span data-ttu-id="36bac-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://sso.novatuscontracts.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="36bac-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://sso.novatuscontracts.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="36bac-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="36bac-158">This value is not real.</span></span> <span data-ttu-id="36bac-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="36bac-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="36bac-160">Kontakta [Novatus klienten supportteamet](mailto:jvinci@novatusinc.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="36bac-160">Contact [Novatus Client support team](mailto:jvinci@novatusinc.com) to get this value.</span></span> 
 


4. <span data-ttu-id="36bac-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="36bac-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_certificate.png) 

5. <span data-ttu-id="36bac-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="36bac-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="36bac-165">På den **Novatus Configuration** klickar du på **konfigurera Novatus** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="36bac-165">On the **Novatus Configuration** section, click **Configure Novatus** to open **Configure sign-on** window.</span></span> <span data-ttu-id="36bac-166">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="36bac-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_configure.png) 

7. <span data-ttu-id="36bac-168">För att få SSO konfigurerats för ditt program, kontakta din [Novatus supportteam](mailto:jvinci@novatusinc.com).</span><span class="sxs-lookup"><span data-stu-id="36bac-168">To get SSO configured for your application, contact your [Novatus support team](mailto:jvinci@novatusinc.com).</span></span> <span data-ttu-id="36bac-169">Bifoga den **hämtat certifikat** filen till din e-post och dela den **metadata URL: er** (**Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel**) med Novatus team för att konfigurera enkel inloggning på sidan.</span><span class="sxs-lookup"><span data-stu-id="36bac-169">Attach the **downloaded certificate** file to your mail and share the **metadata urls** (**Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL**) with Novatus team to set up SSO on their side.</span></span>

> [!TIP]
> <span data-ttu-id="36bac-170">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="36bac-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="36bac-171">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="36bac-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="36bac-172">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="36bac-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="36bac-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="36bac-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="36bac-174">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="36bac-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="36bac-176">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="36bac-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="36bac-177">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="36bac-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="36bac-179">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="36bac-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="36bac-181">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="36bac-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="36bac-183">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="36bac-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="36bac-185">a.</span><span class="sxs-lookup"><span data-stu-id="36bac-185">a.</span></span> <span data-ttu-id="36bac-186">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="36bac-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="36bac-187">b.</span><span class="sxs-lookup"><span data-stu-id="36bac-187">b.</span></span> <span data-ttu-id="36bac-188">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="36bac-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="36bac-189">c.</span><span class="sxs-lookup"><span data-stu-id="36bac-189">c.</span></span> <span data-ttu-id="36bac-190">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="36bac-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="36bac-191">d.</span><span class="sxs-lookup"><span data-stu-id="36bac-191">d.</span></span> <span data-ttu-id="36bac-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="36bac-192">Click **Create**.</span></span>
 
### <a name="creating-a-novatus-test-user"></a><span data-ttu-id="36bac-193">Skapa en testanvändare Novatus</span><span class="sxs-lookup"><span data-stu-id="36bac-193">Creating a Novatus test user</span></span>

<span data-ttu-id="36bac-194">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Novatus.</span><span class="sxs-lookup"><span data-stu-id="36bac-194">The objective of this section is to create a user called Britta Simon in Novatus.</span></span> <span data-ttu-id="36bac-195">Novatus stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="36bac-195">Novatus supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="36bac-196">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="36bac-196">There is no action item for you in this section.</span></span> <span data-ttu-id="36bac-197">En ny användare skapas vid ett försök att komma åt Novatus om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="36bac-197">A new user will be created during an attempt to access Novatus if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="36bac-198">Om du behöver skapa en användare manuellt måste du kontakta den [Novatus supportteam](mailto:jvinci@novatusinc.com).</span><span class="sxs-lookup"><span data-stu-id="36bac-198">If you need to create an user manually, you need to contact the [Novatus support team](mailto:jvinci@novatusinc.com).</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="36bac-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="36bac-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="36bac-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Novatus.</span><span class="sxs-lookup"><span data-stu-id="36bac-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Novatus.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="36bac-202">**Om du vill tilldela Novatus Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="36bac-202">**To assign Britta Simon to Novatus, perform the following steps:**</span></span>

1. <span data-ttu-id="36bac-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="36bac-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="36bac-205">Välj i listan med program **Novatus**.</span><span class="sxs-lookup"><span data-stu-id="36bac-205">In the applications list, select **Novatus**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_app.png) 

3. <span data-ttu-id="36bac-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="36bac-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="36bac-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="36bac-209">Click **Add** button.</span></span> <span data-ttu-id="36bac-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="36bac-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="36bac-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="36bac-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="36bac-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="36bac-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="36bac-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="36bac-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="36bac-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="36bac-215">Testing single sign-on</span></span>

<span data-ttu-id="36bac-216">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="36bac-216">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="36bac-217">När du klickar på panelen Novatus på åtkomstpanelen du bör få automatiskt loggat in på ditt Novatus program.</span><span class="sxs-lookup"><span data-stu-id="36bac-217">When you click the Novatus tile in the Access Panel, you should get automatically signed-on to your Novatus application.</span></span> <span data-ttu-id="36bac-218">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="36bac-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="36bac-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="36bac-219">Additional resources</span></span>

* [<span data-ttu-id="36bac-220">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="36bac-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="36bac-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="36bac-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_203.png

