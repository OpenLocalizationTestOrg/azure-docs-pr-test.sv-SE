---
title: "Självstudier: Azure Active Directory-integrering med Weekdone | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Weekdone."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 34921f9a-5637-4420-ab4c-9beb34421909
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 84aa0069dce55a6623398a99e1cac6bb21bf52f7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-weekdone"></a><span data-ttu-id="cee7d-103">Självstudier: Azure Active Directory-integrering med Weekdone</span><span class="sxs-lookup"><span data-stu-id="cee7d-103">Tutorial: Azure Active Directory integration with Weekdone</span></span>

<span data-ttu-id="cee7d-104">I kursen får lära du att integrera Weekdone med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cee7d-104">In this tutorial, you learn how to integrate Weekdone with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cee7d-105">Integrera Weekdone med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cee7d-105">Integrating Weekdone with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cee7d-106">Du kan styra i Azure AD som har åtkomst till Weekdone</span><span class="sxs-lookup"><span data-stu-id="cee7d-106">You can control in Azure AD who has access to Weekdone</span></span>
- <span data-ttu-id="cee7d-107">Du kan aktivera användarna att automatiskt hämta loggat in på Weekdone (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cee7d-107">You can enable your users to automatically get signed-on to Weekdone (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cee7d-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cee7d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cee7d-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cee7d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cee7d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cee7d-110">Prerequisites</span></span>

<span data-ttu-id="cee7d-111">För att konfigurera Azure AD-integrering med Weekdone, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="cee7d-111">To configure Azure AD integration with Weekdone, you need the following items:</span></span>

- <span data-ttu-id="cee7d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cee7d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cee7d-113">En Weekdone enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="cee7d-113">A Weekdone single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cee7d-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cee7d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cee7d-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cee7d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cee7d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cee7d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cee7d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cee7d-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cee7d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cee7d-118">Scenario description</span></span>
<span data-ttu-id="cee7d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cee7d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cee7d-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cee7d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cee7d-121">Att lägga till Weekdone från galleriet</span><span class="sxs-lookup"><span data-stu-id="cee7d-121">Adding Weekdone from the gallery</span></span>
2. <span data-ttu-id="cee7d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cee7d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-weekdone-from-the-gallery"></a><span data-ttu-id="cee7d-123">Att lägga till Weekdone från galleriet</span><span class="sxs-lookup"><span data-stu-id="cee7d-123">Adding Weekdone from the gallery</span></span>
<span data-ttu-id="cee7d-124">Du måste lägga till Weekdone från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Weekdone i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cee7d-124">To configure the integration of Weekdone into Azure AD, you need to add Weekdone from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cee7d-125">**Utför följande steg för att lägga till Weekdone från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="cee7d-125">**To add Weekdone from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cee7d-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cee7d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cee7d-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cee7d-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cee7d-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cee7d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cee7d-133">I sökrutan skriver **Weekdone**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-133">In the search box, type **Weekdone**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_search.png)

5. <span data-ttu-id="cee7d-135">Välj i resultatpanelen **Weekdone**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="cee7d-135">In the results panel, select **Weekdone**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cee7d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cee7d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cee7d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Weekdone baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cee7d-138">In this section, you configure and test Azure AD single sign-on with Weekdone based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cee7d-139">Azure AD måste du känna till användaren i Weekdone motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="cee7d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Weekdone is to a user in Azure AD.</span></span> <span data-ttu-id="cee7d-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Weekdone upprättas.</span><span class="sxs-lookup"><span data-stu-id="cee7d-140">In other words, a link relationship between an Azure AD user and the related user in Weekdone needs to be established.</span></span>

<span data-ttu-id="cee7d-141">I Weekdone, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="cee7d-141">In Weekdone, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="cee7d-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Weekdone, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cee7d-142">To configure and test Azure AD single sign-on with Weekdone, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cee7d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cee7d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cee7d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cee7d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cee7d-145">**[Skapa en testanvändare Weekdone](#creating-a-weekdone-test-user)**  – du har en motsvarighet för Britta Simon i Weekdone som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="cee7d-145">**[Creating a Weekdone test user](#creating-a-weekdone-test-user)** - to have a counterpart of Britta Simon in Weekdone that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cee7d-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cee7d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cee7d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cee7d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cee7d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cee7d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cee7d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Weekdone program.</span><span class="sxs-lookup"><span data-stu-id="cee7d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Weekdone application.</span></span>

<span data-ttu-id="cee7d-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Weekdone:**</span><span class="sxs-lookup"><span data-stu-id="cee7d-150">**To configure Azure AD single sign-on with Weekdone, perform the following steps:**</span></span>

1. <span data-ttu-id="cee7d-151">I Azure-portalen på den **Weekdone** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-151">In the Azure portal, on the **Weekdone** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cee7d-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cee7d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_samlbase.png)

3. <span data-ttu-id="cee7d-155">På den **Weekdone domän och URL: er** om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="cee7d-155">On the **Weekdone Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_url1.png)

    <span data-ttu-id="cee7d-157">a.</span><span class="sxs-lookup"><span data-stu-id="cee7d-157">a.</span></span> <span data-ttu-id="cee7d-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="cee7d-158">In the **Identifier** textbox, type a URL using the following pattern: `https://weekdone.com/a/<tenantname>`</span></span>

    <span data-ttu-id="cee7d-159">b.</span><span class="sxs-lookup"><span data-stu-id="cee7d-159">b.</span></span> <span data-ttu-id="cee7d-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="cee7d-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://weekdone.com/a/<tenantname>`</span></span>

4. <span data-ttu-id="cee7d-161">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="cee7d-162">Om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="cee7d-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_url2.png)

    <span data-ttu-id="cee7d-164">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="cee7d-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://weekdone.com/a/<tenantname>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="cee7d-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="cee7d-165">These values are not real.</span></span> <span data-ttu-id="cee7d-166">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="cee7d-166">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="cee7d-167">Kontakta [Weekdone klienten supportteamet](mailto:hello@weekdone.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="cee7d-167">Contact [Weekdone Client support team](mailto:hello@weekdone.com) to get these values.</span></span> 

5. <span data-ttu-id="cee7d-168">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="cee7d-168">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_certificate.png) 

6. <span data-ttu-id="cee7d-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cee7d-170">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="cee7d-172">På den **Weekdone Configuration** klickar du på **konfigurera Weekdone** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="cee7d-172">On the **Weekdone Configuration** section, click **Configure Weekdone** to open **Configure sign-on** window.</span></span> <span data-ttu-id="cee7d-173">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="cee7d-173">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_configure.png) 

8. <span data-ttu-id="cee7d-175">Konfigurera enkel inloggning på **Weekdone** sida, måste du skicka den hämtade **XML-Metadata, Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** till [Weekdone supportteamet ](mailto:hello@weekdone.com).</span><span class="sxs-lookup"><span data-stu-id="cee7d-175">To configure single sign-on on **Weekdone** side, you need to send the downloaded **Metadata XML, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Weekdone support team](mailto:hello@weekdone.com).</span></span>

> [!TIP]
> <span data-ttu-id="cee7d-176">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="cee7d-176">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cee7d-177">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="cee7d-177">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cee7d-178">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cee7d-178">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cee7d-179">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cee7d-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="cee7d-180">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cee7d-180">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cee7d-182">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="cee7d-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cee7d-183">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cee7d-183">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cee7d-185">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-185">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cee7d-187">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cee7d-187">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cee7d-189">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cee7d-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cee7d-191">a.</span><span class="sxs-lookup"><span data-stu-id="cee7d-191">a.</span></span> <span data-ttu-id="cee7d-192">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cee7d-193">b.</span><span class="sxs-lookup"><span data-stu-id="cee7d-193">b.</span></span> <span data-ttu-id="cee7d-194">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cee7d-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cee7d-195">c.</span><span class="sxs-lookup"><span data-stu-id="cee7d-195">c.</span></span> <span data-ttu-id="cee7d-196">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cee7d-197">d.</span><span class="sxs-lookup"><span data-stu-id="cee7d-197">d.</span></span> <span data-ttu-id="cee7d-198">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-198">Click **Create**.</span></span>
 
### <a name="creating-a-weekdone-test-user"></a><span data-ttu-id="cee7d-199">Skapa en testanvändare Weekdone</span><span class="sxs-lookup"><span data-stu-id="cee7d-199">Creating a Weekdone test user</span></span>

<span data-ttu-id="cee7d-200">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Weekdone.</span><span class="sxs-lookup"><span data-stu-id="cee7d-200">The objective of this section is to create a user called Britta Simon in Weekdone.</span></span> <span data-ttu-id="cee7d-201">Weekdone stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="cee7d-201">Weekdone supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="cee7d-202">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="cee7d-202">There is no action item for you in this section.</span></span> <span data-ttu-id="cee7d-203">En ny användare skapas under ett försök att komma åt Weekdone om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="cee7d-203">A new user is created during an attempt to access Weekdone if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="cee7d-204">Om du behöver skapa en användare manuellt, måste du kontakta den [Weekdone klienten supportteamet](mailto:hello@weekdone.com).</span><span class="sxs-lookup"><span data-stu-id="cee7d-204">If you need to create a user manually, you need to contact the [Weekdone Client support team](mailto:hello@weekdone.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cee7d-205">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="cee7d-205">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cee7d-206">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Weekdone.</span><span class="sxs-lookup"><span data-stu-id="cee7d-206">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Weekdone.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cee7d-208">**Om du vill tilldela Weekdone Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cee7d-208">**To assign Britta Simon to Weekdone, perform the following steps:**</span></span>

1. <span data-ttu-id="cee7d-209">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-209">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cee7d-211">Välj i listan med program **Weekdone**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-211">In the applications list, select **Weekdone**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_app.png) 

3. <span data-ttu-id="cee7d-213">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cee7d-213">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cee7d-215">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cee7d-215">Click **Add** button.</span></span> <span data-ttu-id="cee7d-216">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cee7d-216">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cee7d-218">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="cee7d-218">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cee7d-219">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cee7d-219">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cee7d-220">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cee7d-220">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cee7d-221">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cee7d-221">Testing single sign-on</span></span>

<span data-ttu-id="cee7d-222">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cee7d-222">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="cee7d-223">När du klickar på panelen Weekdone på åtkomstpanelen du bör få automatiskt loggat in på ditt Weekdone program.</span><span class="sxs-lookup"><span data-stu-id="cee7d-223">When you click the Weekdone tile in the Access Panel, you should get automatically signed-on to your Weekdone application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cee7d-224">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cee7d-224">Additional resources</span></span>

* [<span data-ttu-id="cee7d-225">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cee7d-225">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cee7d-226">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cee7d-226">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_203.png

