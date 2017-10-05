---
title: "Självstudier: Azure Active Directory-integrering med LearnUpon | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och LearnUpon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: b6ac8acc244e9029be01ede5e0865c280171217d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a><span data-ttu-id="d9d1c-103">Självstudier: Azure Active Directory-integrering med LearnUpon</span><span class="sxs-lookup"><span data-stu-id="d9d1c-103">Tutorial: Azure Active Directory integration with LearnUpon</span></span>

<span data-ttu-id="d9d1c-104">I kursen får lära du att integrera LearnUpon med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d9d1c-104">In this tutorial, you learn how to integrate LearnUpon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d9d1c-105">Integrera LearnUpon med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d9d1c-105">Integrating LearnUpon with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d9d1c-106">Du kan styra i Azure AD som har åtkomst till LearnUpon</span><span class="sxs-lookup"><span data-stu-id="d9d1c-106">You can control in Azure AD who has access to LearnUpon</span></span>
- <span data-ttu-id="d9d1c-107">Du kan aktivera användarna att automatiskt hämta loggat in på LearnUpon (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d9d1c-107">You can enable your users to automatically get signed-on to LearnUpon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d9d1c-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d9d1c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d9d1c-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d9d1c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9d1c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d9d1c-110">Prerequisites</span></span>

<span data-ttu-id="d9d1c-111">För att konfigurera Azure AD-integrering med LearnUpon, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d9d1c-111">To configure Azure AD integration with LearnUpon, you need the following items:</span></span>

- <span data-ttu-id="d9d1c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d9d1c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d9d1c-113">En LearnUpon enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d9d1c-113">A LearnUpon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d9d1c-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d9d1c-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d9d1c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d9d1c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d9d1c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d9d1c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d9d1c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d9d1c-118">Scenario description</span></span>
<span data-ttu-id="d9d1c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d9d1c-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d9d1c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d9d1c-121">Att lägga till LearnUpon från galleriet</span><span class="sxs-lookup"><span data-stu-id="d9d1c-121">Adding LearnUpon from the gallery</span></span>
2. <span data-ttu-id="d9d1c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d9d1c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learnupon-from-the-gallery"></a><span data-ttu-id="d9d1c-123">Att lägga till LearnUpon från galleriet</span><span class="sxs-lookup"><span data-stu-id="d9d1c-123">Adding LearnUpon from the gallery</span></span>
<span data-ttu-id="d9d1c-124">Du måste lägga till LearnUpon från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av LearnUpon i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-124">To configure the integration of LearnUpon into Azure AD, you need to add LearnUpon from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d9d1c-125">**Utför följande steg för att lägga till LearnUpon från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="d9d1c-125">**To add LearnUpon from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d9d1c-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d9d1c-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d9d1c-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d9d1c-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d9d1c-133">I sökrutan skriver **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-133">In the search box, type **LearnUpon**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. <span data-ttu-id="d9d1c-135">Välj i resultatpanelen **LearnUpon**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-135">In the results panel, select **LearnUpon**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d9d1c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d9d1c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d9d1c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LearnUpon baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-138">In this section, you configure and test Azure AD single sign-on with LearnUpon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d9d1c-139">Azure AD måste du känna till användaren i LearnUpon motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LearnUpon is to a user in Azure AD.</span></span> <span data-ttu-id="d9d1c-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i LearnUpon upprättas.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-140">In other words, a link relationship between an Azure AD user and the related user in LearnUpon needs to be established.</span></span>

<span data-ttu-id="d9d1c-141">I LearnUpon, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-141">In LearnUpon, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d9d1c-142">Om du vill konfigurera och testa Azure AD enkel inloggning med LearnUpon, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d9d1c-142">To configure and test Azure AD single sign-on with LearnUpon, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d9d1c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d9d1c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d9d1c-145">**[Skapa en testanvändare LearnUpon](#creating-a-learnupon-test-user)**  – du har en motsvarighet för Britta Simon i LearnUpon som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-145">**[Creating a LearnUpon test user](#creating-a-learnupon-test-user)** - to have a counterpart of Britta Simon in LearnUpon that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d9d1c-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d9d1c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d9d1c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d9d1c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d9d1c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt LearnUpon program.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LearnUpon application.</span></span>

<span data-ttu-id="d9d1c-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med LearnUpon:**</span><span class="sxs-lookup"><span data-stu-id="d9d1c-150">**To configure Azure AD single sign-on with LearnUpon, perform the following steps:**</span></span>

1. <span data-ttu-id="d9d1c-151">I Azure-portalen på den **LearnUpon** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-151">In the Azure portal, on the **LearnUpon** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d9d1c-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. <span data-ttu-id="d9d1c-155">På den **LearnUpon domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d9d1c-155">On the **LearnUpon Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    <span data-ttu-id="d9d1c-157">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<companyname>.learnupon.com/saml/consumer`</span><span class="sxs-lookup"><span data-stu-id="d9d1c-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.learnupon.com/saml/consumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d9d1c-158">Observera att detta inte är det verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-158">Please note that this is not the real value.</span></span> <span data-ttu-id="d9d1c-159">Du måste uppdatera det här värdet med det faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-159">you have to update this value with the actual Reply URL.</span></span> <span data-ttu-id="d9d1c-160">Att hämta det här värdet Kontakta [LearnUpon supportteamet](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="d9d1c-160">To get this value Contact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span>



4. <span data-ttu-id="d9d1c-161">På den **SAML-signeringscertifikat** klickar du på **certifikat (Raw)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-161">On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. <span data-ttu-id="d9d1c-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d9d1c-165">På den **LearnUpon Configuration** klickar du på **konfigurera LearnUpon** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-165">On the **LearnUpon Configuration** section, click **Configure LearnUpon** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d9d1c-166">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d9d1c-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. <span data-ttu-id="d9d1c-168">Öppna en annan instans för webbläsaren och logga in till LearnUpon med ett administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-168">Open another browser instance and login into LearnUpon with an administrator account.</span></span> 

8. <span data-ttu-id="d9d1c-169">Klicka på den **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-169">Click the **settings** tab.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. <span data-ttu-id="d9d1c-171">Klicka på **Single Sign On - SAML**, och klicka sedan på **allmänna inställningar** att konfigurera inställningar för SAML.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-171">Click **Single Sign On - SAML**, and then click **General Settings** to configure SAML settings.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. <span data-ttu-id="d9d1c-173">I den **allmänna inställningar** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d9d1c-173">In the **General Settings** section, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    <span data-ttu-id="d9d1c-175">a.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-175">a.</span></span> <span data-ttu-id="d9d1c-176">Välj **aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-176">Select **Enabled**.</span></span>

    <span data-ttu-id="d9d1c-177">b.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-177">b.</span></span> <span data-ttu-id="d9d1c-178">Välj **Version** som **2.0**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-178">Select **Version** as **2.0**.</span></span>

    <span data-ttu-id="d9d1c-179">c.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-179">c.</span></span> <span data-ttu-id="d9d1c-180">Välj **hoppa över villkor** som **nr**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-180">Select **Skip conditions** as **No**.</span></span>

    <span data-ttu-id="d9d1c-181">d.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-181">d.</span></span> <span data-ttu-id="d9d1c-182">I den **SAML-Token efter param namnet** textruta typen namnet på begäran post parameter till URL: en för SAML konsumenten ovan som innehåller SAML-kontroll för att verifiera och autentiserad – till exempel **SAMLResponse**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-182">In the **SAML Token Post param name** textbox, type the name of request post parameter to the SAML consumer URL indicated above that contains the SAML Assertion to be verified and authenticated - for example **SAMLResponse**.</span></span>

    <span data-ttu-id="d9d1c-183">e.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-183">e.</span></span> <span data-ttu-id="d9d1c-184">I den **identifierare namnformat** textruta typ - exempelvis det värde som anger var i SAML-kontroll användare identifierare (e-postadress) finns **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-184">In the **Name Identifier Format** textbox, type the value that indicates where in your SAML Assertion the users identifier (Email address) resides - for example **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>
  
    <span data-ttu-id="d9d1c-185">f.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-185">f.</span></span> <span data-ttu-id="d9d1c-186">I den **identifiera leverantörsplatsen** textruta Skriv det värde som anger där användarna skickas till om de klickar på överförda ikon från Azure portal inloggningsskärmen.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-186">In the **Identify Provider Location** textbox, type the value that indicates where the users are sent to if they click on your uploaded icon from your Azure portal login screen.</span></span>
  
    <span data-ttu-id="d9d1c-187">g.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-187">g.</span></span> <span data-ttu-id="d9d1c-188">I den **logga ut URL** textruta klistra in den **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-188">In the **Sign out URL** textbox, paste the **Sign-Out URL** which you have copied from the Azure portal.</span></span>
    
    <span data-ttu-id="d9d1c-189">h.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-189">h.</span></span> <span data-ttu-id="d9d1c-190">Klicka på **hantera fingeravtrycksläsare utskrifter**, och sedan ladda upp fingeravtryck av hämtade certifikatet.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-190">Click **Manage finger prints**, and then upload the finger print of your downloaded certificate.</span></span>

11. <span data-ttu-id="d9d1c-191">Klicka på **användarinställningar**, och utför sedan följande steg:</span><span class="sxs-lookup"><span data-stu-id="d9d1c-191">Click **User Settings**, and then perform the following steps:</span></span>
   
     ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    <span data-ttu-id="d9d1c-193">a.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-193">a.</span></span> <span data-ttu-id="d9d1c-194">I den **förnamn identifierare Format** textruta typ - exempelvis värdet som talar om för oss var i SAML-kontroll användare firstname finns: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-194">In the **First Name Identifier Format** textbox, type the value that tells us where in your SAML Assertion the users firstname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>
  
    <span data-ttu-id="d9d1c-195">b.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-195">b.</span></span> <span data-ttu-id="d9d1c-196">I den **senaste namnformat identifierare** textruta typ - exempelvis värdet som talar om för oss var i SAML-kontroll användare efternamn finns: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-196">In the **Last Name Identifier Format** textbox, type the value that tells us where in your SAML Assertion the users lastname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

> [!TIP]
> <span data-ttu-id="d9d1c-197">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="d9d1c-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d9d1c-198">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d9d1c-199">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d9d1c-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d9d1c-200">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d9d1c-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="d9d1c-201">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d9d1c-203">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d9d1c-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d9d1c-204">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d9d1c-206">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d9d1c-208">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d9d1c-210">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d9d1c-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d9d1c-212">a.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-212">a.</span></span> <span data-ttu-id="d9d1c-213">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d9d1c-214">b.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-214">b.</span></span> <span data-ttu-id="d9d1c-215">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d9d1c-216">c.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-216">c.</span></span> <span data-ttu-id="d9d1c-217">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d9d1c-218">d.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-218">d.</span></span> <span data-ttu-id="d9d1c-219">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-219">Click **Create**.</span></span>
 
### <a name="creating-a-learnupon-test-user"></a><span data-ttu-id="d9d1c-220">Skapa en testanvändare LearnUpon</span><span class="sxs-lookup"><span data-stu-id="d9d1c-220">Creating a LearnUpon test user</span></span>

<span data-ttu-id="d9d1c-221">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i LearnUpon.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-221">The objective of this section is to create a user called Britta Simon in LearnUpon.</span></span> <span data-ttu-id="d9d1c-222">LearnUpon stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-222">LearnUpon supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="d9d1c-223">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-223">There is no action item for you in this section.</span></span> <span data-ttu-id="d9d1c-224">En ny användare skapas vid ett försök att komma åt LearnUpon om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-224">A new user will be created during an attempt to access LearnUpon if it doesn't exist yet.</span></span> <span data-ttu-id="d9d1c-225">[Konfigurera Azure AD-Single Sign-On](#configuring-azure-ad-single-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="d9d1c-225">[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="d9d1c-226">Om du behöver skapa en användare manuellt måste du kontakta [LearnUpon supportteamet](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="d9d1c-226">If you need to create an user manually, you need to contact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d9d1c-227">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d9d1c-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d9d1c-228">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till LearnUpon.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-228">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LearnUpon.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d9d1c-230">**Om du vill tilldela LearnUpon Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d9d1c-230">**To assign Britta Simon to LearnUpon, perform the following steps:**</span></span>

1. <span data-ttu-id="d9d1c-231">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-231">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d9d1c-233">Välj i listan med program **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-233">In the applications list, select **LearnUpon**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. <span data-ttu-id="d9d1c-235">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d9d1c-237">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-237">Click **Add** button.</span></span> <span data-ttu-id="d9d1c-238">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d9d1c-240">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d9d1c-241">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d9d1c-242">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d9d1c-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d9d1c-243">Testing single sign-on</span></span>

<span data-ttu-id="d9d1c-244">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d9d1c-245">När du klickar på panelen LearnUpon på åtkomstpanelen du bör få automatiskt loggat in på ditt LearnUpon program.</span><span class="sxs-lookup"><span data-stu-id="d9d1c-245">When you click the LearnUpon tile in the Access Panel, you should get automatically signed-on to your LearnUpon application.</span></span>
<span data-ttu-id="d9d1c-246">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d9d1c-246">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9d1c-247">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d9d1c-247">Additional resources</span></span>

* [<span data-ttu-id="d9d1c-248">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d9d1c-248">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d9d1c-249">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d9d1c-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png

