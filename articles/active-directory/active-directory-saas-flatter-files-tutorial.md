---
title: "Självstudier: Azure Active Directory-integrering med plattare filer | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och plattare filer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: e02150cb27768d7b403bdca191bc1f189821def4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a><span data-ttu-id="c6529-103">Självstudier: Azure Active Directory-integrering med plattare filer</span><span class="sxs-lookup"><span data-stu-id="c6529-103">Tutorial: Azure Active Directory integration with Flatter Files</span></span>

<span data-ttu-id="c6529-104">I kursen får lära du att integrera plattare filer med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c6529-104">In this tutorial, you learn how to integrate Flatter Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c6529-105">Integrera plattare filer med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c6529-105">Integrating Flatter Files with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c6529-106">Du kan styra i Azure AD som har åtkomst till plattare filer</span><span class="sxs-lookup"><span data-stu-id="c6529-106">You can control in Azure AD who has access to Flatter Files</span></span>
- <span data-ttu-id="c6529-107">Du kan aktivera användarna att automatiskt hämta loggat in på plattare filer (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c6529-107">You can enable your users to automatically get signed-on to Flatter Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c6529-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c6529-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c6529-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c6529-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6529-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c6529-110">Prerequisites</span></span>

<span data-ttu-id="c6529-111">För att konfigurera Azure AD-integrering med plattare filer, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c6529-111">To configure Azure AD integration with Flatter Files, you need the following items:</span></span>

- <span data-ttu-id="c6529-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c6529-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c6529-113">En plattare filer enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c6529-113">A Flatter Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c6529-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c6529-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c6529-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c6529-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c6529-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c6529-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c6529-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c6529-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c6529-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c6529-118">Scenario description</span></span>
<span data-ttu-id="c6529-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c6529-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c6529-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c6529-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c6529-121">Lägga till plattare filer från galleriet</span><span class="sxs-lookup"><span data-stu-id="c6529-121">Adding Flatter Files from the gallery</span></span>
2. <span data-ttu-id="c6529-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c6529-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-flatter-files-from-the-gallery"></a><span data-ttu-id="c6529-123">Lägga till plattare filer från galleriet</span><span class="sxs-lookup"><span data-stu-id="c6529-123">Adding Flatter Files from the gallery</span></span>
<span data-ttu-id="c6529-124">Du måste lägga till plattare filer från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av plattare filer i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6529-124">To configure the integration of Flatter Files into Azure AD, you need to add Flatter Files from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c6529-125">**Utför följande steg för att lägga till plattare filer från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c6529-125">**To add Flatter Files from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c6529-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c6529-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c6529-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c6529-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c6529-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c6529-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c6529-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c6529-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c6529-133">I sökrutan skriver **plattare filer**.</span><span class="sxs-lookup"><span data-stu-id="c6529-133">In the search box, type **Flatter Files**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_search.png)

5. <span data-ttu-id="c6529-135">Välj i resultatpanelen **plattare filer**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c6529-135">In the results panel, select **Flatter Files**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c6529-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c6529-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c6529-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med plattare filer baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c6529-138">In this section, you configure and test Azure AD single sign-on with Flatter Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c6529-139">Azure AD måste du känna till motsvarande användaren i plattare filer till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c6529-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Flatter Files is to a user in Azure AD.</span></span> <span data-ttu-id="c6529-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i plattare filer upprättas.</span><span class="sxs-lookup"><span data-stu-id="c6529-140">In other words, a link relationship between an Azure AD user and the related user in Flatter Files needs to be established.</span></span>

<span data-ttu-id="c6529-141">I plattare filer, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c6529-141">In Flatter Files, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c6529-142">Om du vill konfigurera och testa Azure AD enkel inloggning med plattare filer, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c6529-142">To configure and test Azure AD single sign-on with Flatter Files, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c6529-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c6529-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c6529-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c6529-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c6529-145">**[Skapa en testanvändare plattare filer](#creating-a-flatter-files-test-user)**  – du har en motsvarighet för Britta Simon i plattare filer som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c6529-145">**[Creating a Flatter Files test user](#creating-a-flatter-files-test-user)** - to have a counterpart of Britta Simon in Flatter Files that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c6529-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c6529-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c6529-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c6529-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c6529-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c6529-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c6529-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet plattare filer.</span><span class="sxs-lookup"><span data-stu-id="c6529-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Flatter Files application.</span></span>

<span data-ttu-id="c6529-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med plattare filer:**</span><span class="sxs-lookup"><span data-stu-id="c6529-150">**To configure Azure AD single sign-on with Flatter Files, perform the following steps:**</span></span>

1. <span data-ttu-id="c6529-151">I Azure-portalen på den **plattare filer** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c6529-151">In the Azure portal, on the **Flatter Files** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c6529-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c6529-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_samlbase.png)

3. <span data-ttu-id="c6529-155">På den **plattare filer domän och URL: er** avsnittet användaren behöver inte utföra några steg som appen före redan är integrerad med Azure.</span><span class="sxs-lookup"><span data-stu-id="c6529-155">On the **Flatter Files Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_url.png)
 
4. <span data-ttu-id="c6529-157">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c6529-157">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_certificate.png) 

5. <span data-ttu-id="c6529-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c6529-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c6529-161">På den **plattare filer Configuration** klickar du på **konfigurera plattare filer** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="c6529-161">On the **Flatter Files Configuration** section, click **Configure Flatter Files** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c6529-162">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="c6529-162">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_configure.png) 

7. <span data-ttu-id="c6529-164">Inloggning till plattare filer programmet som administratör.</span><span class="sxs-lookup"><span data-stu-id="c6529-164">Sign-on to your Flatter Files application as an administrator.</span></span>

8. <span data-ttu-id="c6529-165">Klicka på **INSTRUMENTPANELEN**.</span><span class="sxs-lookup"><span data-stu-id="c6529-165">Click **DASHBOARD**.</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  

9. <span data-ttu-id="c6529-167">Klicka på **inställningar**, och utför följande steg på den **företagets** fliken:</span><span class="sxs-lookup"><span data-stu-id="c6529-167">Click **Settings**, and then perform the following steps on the **Company** tab:</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    <span data-ttu-id="c6529-169">a.</span><span class="sxs-lookup"><span data-stu-id="c6529-169">a.</span></span> <span data-ttu-id="c6529-170">Välj **använder SAML 2.0 för autentisering**.</span><span class="sxs-lookup"><span data-stu-id="c6529-170">Select **Use SAML 2.0 for Authentication**.</span></span>
    
    <span data-ttu-id="c6529-171">b.</span><span class="sxs-lookup"><span data-stu-id="c6529-171">b.</span></span> <span data-ttu-id="c6529-172">Klicka på **konfigurerar du SAML**.</span><span class="sxs-lookup"><span data-stu-id="c6529-172">Click **Configure SAML**.</span></span>

8. <span data-ttu-id="c6529-173">På den **SAML-konfiguration** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c6529-173">On the **SAML Configuration** dialog, perform the following steps:</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    <span data-ttu-id="c6529-175">a.</span><span class="sxs-lookup"><span data-stu-id="c6529-175">a.</span></span> <span data-ttu-id="c6529-176">I den **domän** textruta, ange en registrerad domän.</span><span class="sxs-lookup"><span data-stu-id="c6529-176">In the **Domain** textbox, type your registered domain.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="c6529-177">Om du inte har en registrerad domän ännu, kontakta din plattare filer supportteam via [ support@flatterfiles.com ](mailto:support@flatterfiles.com).</span><span class="sxs-lookup"><span data-stu-id="c6529-177">If you don't have a registered domain yet, contact your Flatter Files support team via [support@flatterfiles.com](mailto:support@flatterfiles.com).</span></span> 
    
    <span data-ttu-id="c6529-178">b.</span><span class="sxs-lookup"><span data-stu-id="c6529-178">b.</span></span> <span data-ttu-id="c6529-179">I **identitet providern URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat formuläret Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c6529-179">In **Identity Provider URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied form Azure portal.</span></span>
   
    <span data-ttu-id="c6529-180">c.</span><span class="sxs-lookup"><span data-stu-id="c6529-180">c.</span></span>  <span data-ttu-id="c6529-181">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **providern identitetscertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="c6529-181">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="c6529-182">d.</span><span class="sxs-lookup"><span data-stu-id="c6529-182">d.</span></span> <span data-ttu-id="c6529-183">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="c6529-183">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="c6529-184">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="c6529-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c6529-185">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="c6529-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c6529-186">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c6529-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c6529-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c6529-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="c6529-188">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c6529-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c6529-190">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c6529-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c6529-191">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c6529-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c6529-193">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c6529-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c6529-195">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c6529-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c6529-197">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c6529-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c6529-199">a.</span><span class="sxs-lookup"><span data-stu-id="c6529-199">a.</span></span> <span data-ttu-id="c6529-200">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c6529-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c6529-201">b.</span><span class="sxs-lookup"><span data-stu-id="c6529-201">b.</span></span> <span data-ttu-id="c6529-202">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c6529-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c6529-203">c.</span><span class="sxs-lookup"><span data-stu-id="c6529-203">c.</span></span> <span data-ttu-id="c6529-204">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c6529-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c6529-205">d.</span><span class="sxs-lookup"><span data-stu-id="c6529-205">d.</span></span> <span data-ttu-id="c6529-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c6529-206">Click **Create**.</span></span>
 
### <a name="creating-a-flatter-files-test-user"></a><span data-ttu-id="c6529-207">Skapa en testanvändare plattare filer</span><span class="sxs-lookup"><span data-stu-id="c6529-207">Creating a Flatter Files test user</span></span>

<span data-ttu-id="c6529-208">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i plattare filer.</span><span class="sxs-lookup"><span data-stu-id="c6529-208">The objective of this section is to create a user called Britta Simon in Flatter Files.</span></span>

<span data-ttu-id="c6529-209">**Utför följande steg för att skapa en användare som kallas Britta Simon i plattare filer:**</span><span class="sxs-lookup"><span data-stu-id="c6529-209">**To create a user called Britta Simon in Flatter Files, perform the following steps:**</span></span>

1. <span data-ttu-id="c6529-210">Logga in på ditt **plattare filer** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="c6529-210">Sign on to your **Flatter Files** company site as administrator.</span></span>

2. <span data-ttu-id="c6529-211">I navigeringsfönstret till vänster klickar du på **inställningar**, och klicka sedan på den **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="c6529-211">In the navigation pane on the left, click **Settings**, and then click the **Users** tab.</span></span>
   
    ![Skapa en plattare filer användare](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. <span data-ttu-id="c6529-213">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="c6529-213">Click **Add User**.</span></span> 

4. <span data-ttu-id="c6529-214">På den **Lägg till användare** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c6529-214">On the **Add User** dialog, perform the following steps:</span></span>
   
    ![Skapa en plattare filer användare](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    <span data-ttu-id="c6529-216">a.</span><span class="sxs-lookup"><span data-stu-id="c6529-216">a.</span></span> <span data-ttu-id="c6529-217">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="c6529-217">In the **First Name** textbox, type **Britta**.</span></span>
   
    <span data-ttu-id="c6529-218">b.</span><span class="sxs-lookup"><span data-stu-id="c6529-218">b.</span></span> <span data-ttu-id="c6529-219">I den **efternamn** textruta typen **Simon**.</span><span class="sxs-lookup"><span data-stu-id="c6529-219">In the **Last Name** textbox, type **Simon**.</span></span> 
   
    <span data-ttu-id="c6529-220">c.</span><span class="sxs-lookup"><span data-stu-id="c6529-220">c.</span></span> <span data-ttu-id="c6529-221">I den **e-postadress** textruta Skriv Brittas e-postadress i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c6529-221">In the **Email Address** textbox, type Britta's email address in the Azure portal.</span></span>
   
    <span data-ttu-id="c6529-222">d.</span><span class="sxs-lookup"><span data-stu-id="c6529-222">d.</span></span> <span data-ttu-id="c6529-223">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="c6529-223">Click **Submit**.</span></span>   


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c6529-224">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c6529-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c6529-225">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till plattare filer.</span><span class="sxs-lookup"><span data-stu-id="c6529-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Flatter Files.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c6529-227">**Om du vill tilldela Britta Simon plattare filer, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c6529-227">**To assign Britta Simon to Flatter Files, perform the following steps:**</span></span>

1. <span data-ttu-id="c6529-228">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c6529-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c6529-230">Välj i listan med program **plattare filer**.</span><span class="sxs-lookup"><span data-stu-id="c6529-230">In the applications list, select **Flatter Files**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_app.png) 

3. <span data-ttu-id="c6529-232">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c6529-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c6529-234">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c6529-234">Click **Add** button.</span></span> <span data-ttu-id="c6529-235">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c6529-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c6529-237">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c6529-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c6529-238">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c6529-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c6529-239">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c6529-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c6529-240">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c6529-240">Testing single sign-on</span></span>

<span data-ttu-id="c6529-241">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c6529-241">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c6529-242">När du klickar på panelen plattare filer på åtkomstpanelen du bör få automatiskt loggat in i tillämpningsprogrammet plattare filer.</span><span class="sxs-lookup"><span data-stu-id="c6529-242">When you click the Flatter Files tile in the Access Panel, you should get automatically signed-on to your Flatter Files application.</span></span>
<span data-ttu-id="c6529-243">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c6529-243">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6529-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c6529-244">Additional resources</span></span>

* [<span data-ttu-id="c6529-245">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c6529-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c6529-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c6529-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png

