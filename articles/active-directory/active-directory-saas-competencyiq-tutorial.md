---
title: "Självstudier: Azure Active Directory-integrering med CompetencyIQ | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och CompetencyIQ."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e262bf7e-cc7d-4d0e-aea7-861f00d8837d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: ad3cec3de9034ddab2161952620d31540ae51978
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-competencyiq"></a><span data-ttu-id="c42df-103">Självstudier: Azure Active Directory-integrering med CompetencyIQ</span><span class="sxs-lookup"><span data-stu-id="c42df-103">Tutorial: Azure Active Directory integration with CompetencyIQ</span></span>

<span data-ttu-id="c42df-104">I kursen får lära du att integrera CompetencyIQ med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c42df-104">In this tutorial, you learn how to integrate CompetencyIQ with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c42df-105">Integrera CompetencyIQ med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c42df-105">Integrating CompetencyIQ with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c42df-106">Du kan styra i Azure AD som har åtkomst till CompetencyIQ</span><span class="sxs-lookup"><span data-stu-id="c42df-106">You can control in Azure AD who has access to CompetencyIQ</span></span>
- <span data-ttu-id="c42df-107">Du kan aktivera användarna att automatiskt hämta loggat in på CompetencyIQ (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c42df-107">You can enable your users to automatically get signed-on to CompetencyIQ (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c42df-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c42df-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c42df-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c42df-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c42df-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c42df-110">Prerequisites</span></span>

<span data-ttu-id="c42df-111">För att konfigurera Azure AD-integrering med CompetencyIQ, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c42df-111">To configure Azure AD integration with CompetencyIQ, you need the following items:</span></span>

- <span data-ttu-id="c42df-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c42df-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c42df-113">En CompetencyIQ enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c42df-113">A CompetencyIQ single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c42df-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c42df-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c42df-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c42df-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c42df-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c42df-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c42df-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c42df-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c42df-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c42df-118">Scenario description</span></span>
<span data-ttu-id="c42df-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c42df-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c42df-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c42df-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c42df-121">Att lägga till CompetencyIQ från galleriet</span><span class="sxs-lookup"><span data-stu-id="c42df-121">Adding CompetencyIQ from the gallery</span></span>
2. <span data-ttu-id="c42df-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c42df-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-competencyiq-from-the-gallery"></a><span data-ttu-id="c42df-123">Att lägga till CompetencyIQ från galleriet</span><span class="sxs-lookup"><span data-stu-id="c42df-123">Adding CompetencyIQ from the gallery</span></span>
<span data-ttu-id="c42df-124">Du måste lägga till CompetencyIQ från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av CompetencyIQ i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c42df-124">To configure the integration of CompetencyIQ into Azure AD, you need to add CompetencyIQ from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c42df-125">**Utför följande steg för att lägga till CompetencyIQ från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c42df-125">**To add CompetencyIQ from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c42df-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c42df-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c42df-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c42df-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c42df-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c42df-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c42df-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c42df-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c42df-133">I sökrutan skriver **CompetencyIQ**.</span><span class="sxs-lookup"><span data-stu-id="c42df-133">In the search box, type **CompetencyIQ**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_search.png)

5. <span data-ttu-id="c42df-135">Välj i resultatpanelen **CompetencyIQ**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c42df-135">In the results panel, select **CompetencyIQ**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c42df-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c42df-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c42df-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med CompetencyIQ baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c42df-138">In this section, you configure and test Azure AD single sign-on with CompetencyIQ based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c42df-139">Azure AD måste du känna till användaren i CompetencyIQ motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c42df-139">For single sign-on to work, Azure AD needs to know what the counterpart user in CompetencyIQ is to a user in Azure AD.</span></span> <span data-ttu-id="c42df-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i CompetencyIQ upprättas.</span><span class="sxs-lookup"><span data-stu-id="c42df-140">In other words, a link relationship between an Azure AD user and the related user in CompetencyIQ needs to be established.</span></span>

<span data-ttu-id="c42df-141">I CompetencyIQ, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c42df-141">In CompetencyIQ, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c42df-142">Om du vill konfigurera och testa Azure AD enkel inloggning med CompetencyIQ, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c42df-142">To configure and test Azure AD single sign-on with CompetencyIQ, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c42df-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c42df-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c42df-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c42df-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c42df-145">**[Skapa en testanvändare CompetencyIQ](#creating-a-competencyiq-test-user)**  – du har en motsvarighet för Britta Simon i CompetencyIQ som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c42df-145">**[Creating a CompetencyIQ test user](#creating-a-competencyiq-test-user)** - to have a counterpart of Britta Simon in CompetencyIQ that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c42df-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c42df-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c42df-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c42df-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c42df-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c42df-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c42df-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt CompetencyIQ program.</span><span class="sxs-lookup"><span data-stu-id="c42df-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your CompetencyIQ application.</span></span>

<span data-ttu-id="c42df-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med CompetencyIQ:**</span><span class="sxs-lookup"><span data-stu-id="c42df-150">**To configure Azure AD single sign-on with CompetencyIQ, perform the following steps:**</span></span>

1. <span data-ttu-id="c42df-151">I Azure-portalen på den **CompetencyIQ** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c42df-151">In the Azure portal, on the **CompetencyIQ** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c42df-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c42df-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_samlbase.png)

3. <span data-ttu-id="c42df-155">På den **CompetencyIQ domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c42df-155">On the **CompetencyIQ Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_url1.png)

    <span data-ttu-id="c42df-157">a.</span><span class="sxs-lookup"><span data-stu-id="c42df-157">a.</span></span> <span data-ttu-id="c42df-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<customer>.competencyiq.com/`</span><span class="sxs-lookup"><span data-stu-id="c42df-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<customer>.competencyiq.com/`</span></span>
    
    <span data-ttu-id="c42df-159">b.</span><span class="sxs-lookup"><span data-stu-id="c42df-159">b.</span></span> <span data-ttu-id="c42df-160">I den **identifierare** textruta anger du URL:`https://www.competencyiq.com/`</span><span class="sxs-lookup"><span data-stu-id="c42df-160">In the **Identifier** textbox, type the URL: `https://www.competencyiq.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c42df-161">Inloggnings-URL-värdet är inte riktigt så att uppdatera det med faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="c42df-161">The Sign-on URL value is not real so update this with actual Sign-On URL.</span></span> <span data-ttu-id="c42df-162">Kontakta [CompetencyIQ klienten supportteamet](https://www.competencyiq.com/) du behöver.</span><span class="sxs-lookup"><span data-stu-id="c42df-162">Contact [CompetencyIQ Client support team](https://www.competencyiq.com/) to get this.</span></span> 
 
4. <span data-ttu-id="c42df-163">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c42df-163">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_certificate.png) 

5. <span data-ttu-id="c42df-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c42df-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c42df-167">På den **CompetencyIQ Configuration** klickar du på **konfigurera CompetencyIQ** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="c42df-167">On the **CompetencyIQ Configuration** section, click **Configure CompetencyIQ** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c42df-168">Kopiera den **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="c42df-168">Copy the **SAML Entity ID**, and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_configure.png) 

7. <span data-ttu-id="c42df-170">Konfigurera enkel inloggning på **CompetencyIQ** sida, måste du skicka den hämtade **XML-Metadata för**, **SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** till [CompetencyIQ supportteamet](https://www.competencyiq.com/).</span><span class="sxs-lookup"><span data-stu-id="c42df-170">To configure single sign-on on **CompetencyIQ** side, you need to send the downloaded **Metadata XML**, **SAML Entity ID** and **SAML Single Sign-On Service URL** to [CompetencyIQ support team](https://www.competencyiq.com/).</span></span> <span data-ttu-id="c42df-171">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="c42df-171">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="c42df-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="c42df-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c42df-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="c42df-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c42df-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c42df-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c42df-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c42df-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="c42df-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c42df-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c42df-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c42df-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c42df-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c42df-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c42df-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c42df-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c42df-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c42df-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c42df-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c42df-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c42df-187">a.</span><span class="sxs-lookup"><span data-stu-id="c42df-187">a.</span></span> <span data-ttu-id="c42df-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c42df-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c42df-189">b.</span><span class="sxs-lookup"><span data-stu-id="c42df-189">b.</span></span> <span data-ttu-id="c42df-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c42df-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c42df-191">c.</span><span class="sxs-lookup"><span data-stu-id="c42df-191">c.</span></span> <span data-ttu-id="c42df-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c42df-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c42df-193">d.</span><span class="sxs-lookup"><span data-stu-id="c42df-193">d.</span></span> <span data-ttu-id="c42df-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c42df-194">Click **Create**.</span></span>
 
### <a name="creating-a-competencyiq-test-user"></a><span data-ttu-id="c42df-195">Skapa en testanvändare CompetencyIQ</span><span class="sxs-lookup"><span data-stu-id="c42df-195">Creating a CompetencyIQ test user</span></span>

<span data-ttu-id="c42df-196">Om du vill aktivera Azure AD-användare kan logga in på CompetencyIQ etableras de i CompetencyIQ.</span><span class="sxs-lookup"><span data-stu-id="c42df-196">To enable Azure AD users to log in to CompetencyIQ, they must be provisioned into CompetencyIQ.</span></span> <span data-ttu-id="c42df-197">Kontakta [CompetencyIQ supportteamet](https://www.competencyiq.com/) att skapa användare i programmet.</span><span class="sxs-lookup"><span data-stu-id="c42df-197">Contact [CompetencyIQ support team](https://www.competencyiq.com/) to create users in the application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c42df-198">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c42df-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c42df-199">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till CompetencyIQ.</span><span class="sxs-lookup"><span data-stu-id="c42df-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to CompetencyIQ.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c42df-201">**Om du vill tilldela CompetencyIQ Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c42df-201">**To assign Britta Simon to CompetencyIQ, perform the following steps:**</span></span>

1. <span data-ttu-id="c42df-202">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c42df-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c42df-204">Välj i listan med program **CompetencyIQ**.</span><span class="sxs-lookup"><span data-stu-id="c42df-204">In the applications list, select **CompetencyIQ**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_app.png) 

3. <span data-ttu-id="c42df-206">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c42df-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c42df-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c42df-208">Click **Add** button.</span></span> <span data-ttu-id="c42df-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c42df-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c42df-211">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c42df-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c42df-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c42df-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c42df-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c42df-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c42df-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c42df-214">Testing single sign-on</span></span>

<span data-ttu-id="c42df-215">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c42df-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c42df-216">När du klickar på panelen CompetencyIQ på åtkomstpanelen bör du få automatiskt inloggad i programmet.</span><span class="sxs-lookup"><span data-stu-id="c42df-216">When you click the CompetencyIQ tile in the Access Panel, you should get automatically logged into the application.</span></span>
<span data-ttu-id="c42df-217">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c42df-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c42df-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c42df-218">Additional resources</span></span>

* [<span data-ttu-id="c42df-219">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c42df-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c42df-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c42df-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_203.png

