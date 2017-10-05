---
title: "Självstudier: Azure Active Directory-integrering med Insperity ExpensAble | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Insperity ExpensAble."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c579c453-580e-417d-8a5e-9b6b352795c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: b50e10be54b1fc413be10bee5b58631790629335
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-insperity-expensable"></a><span data-ttu-id="11951-103">Självstudier: Azure Active Directory-integrering med Insperity ExpensAble</span><span class="sxs-lookup"><span data-stu-id="11951-103">Tutorial: Azure Active Directory integration with Insperity ExpensAble</span></span>

<span data-ttu-id="11951-104">I kursen får lära du att integrera Insperity ExpensAble med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="11951-104">In this tutorial, you learn how to integrate Insperity ExpensAble with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="11951-105">Integrera Insperity ExpensAble med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="11951-105">Integrating Insperity ExpensAble with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="11951-106">Du kan styra i Azure AD som har åtkomst till Insperity ExpensAble</span><span class="sxs-lookup"><span data-stu-id="11951-106">You can control in Azure AD who has access to Insperity ExpensAble</span></span>
- <span data-ttu-id="11951-107">Du kan aktivera användarna att automatiskt hämta loggat in på Insperity ExpensAble (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="11951-107">You can enable your users to automatically get signed-on to Insperity ExpensAble (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="11951-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="11951-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="11951-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="11951-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="11951-110">Krav</span><span class="sxs-lookup"><span data-stu-id="11951-110">Prerequisites</span></span>

<span data-ttu-id="11951-111">För att konfigurera Azure AD-integrering med Insperity ExpensAble, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="11951-111">To configure Azure AD integration with Insperity ExpensAble, you need the following items:</span></span>

- <span data-ttu-id="11951-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="11951-112">An Azure AD subscription</span></span>
- <span data-ttu-id="11951-113">En Insperity ExpensAble enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="11951-113">An Insperity ExpensAble single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="11951-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="11951-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="11951-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="11951-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="11951-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="11951-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="11951-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="11951-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="11951-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="11951-118">Scenario description</span></span>
<span data-ttu-id="11951-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="11951-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="11951-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="11951-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="11951-121">Att lägga till Insperity ExpensAble från galleriet</span><span class="sxs-lookup"><span data-stu-id="11951-121">Adding Insperity ExpensAble from the gallery</span></span>
2. <span data-ttu-id="11951-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="11951-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-insperity-expensable-from-the-gallery"></a><span data-ttu-id="11951-123">Att lägga till Insperity ExpensAble från galleriet</span><span class="sxs-lookup"><span data-stu-id="11951-123">Adding Insperity ExpensAble from the gallery</span></span>
<span data-ttu-id="11951-124">Du måste lägga till Insperity ExpensAble från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Insperity ExpensAble i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="11951-124">To configure the integration of Insperity ExpensAble into Azure AD, you need to add Insperity ExpensAble from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="11951-125">**Om du vill lägga till Insperity ExpensAble från galleriet, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="11951-125">**To add Insperity ExpensAble from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="11951-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="11951-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="11951-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="11951-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="11951-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="11951-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="11951-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="11951-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="11951-133">I sökrutan skriver **Insperity ExpensAble**.</span><span class="sxs-lookup"><span data-stu-id="11951-133">In the search box, type **Insperity ExpensAble**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_search.png)

5. <span data-ttu-id="11951-135">Välj i resultatpanelen **Insperity ExpensAble**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="11951-135">In the results panel, select **Insperity ExpensAble**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="11951-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="11951-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="11951-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Insperity ExpensAble baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="11951-138">In this section, you configure and test Azure AD single sign-on with Insperity ExpensAble based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="11951-139">Azure AD måste du känna till användaren i Insperity ExpensAble motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="11951-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Insperity ExpensAble is to a user in Azure AD.</span></span> <span data-ttu-id="11951-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Insperity ExpensAble upprättas.</span><span class="sxs-lookup"><span data-stu-id="11951-140">In other words, a link relationship between an Azure AD user and the related user in Insperity ExpensAble needs to be established.</span></span>

<span data-ttu-id="11951-141">I den ExpensAble Insperity tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="11951-141">In Insperity ExpensAble, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="11951-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Insperity ExpensAble, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="11951-142">To configure and test Azure AD single sign-on with Insperity ExpensAble, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="11951-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="11951-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="11951-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="11951-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="11951-145">**[Skapa en Insperity ExpensAble testanvändare](#creating-an-insperity-expensable-test-user)**  – du har en motsvarighet för Britta Simon i Insperity ExpensAble som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="11951-145">**[Creating an Insperity ExpensAble test user](#creating-an-insperity-expensable-test-user)** - to have a counterpart of Britta Simon in Insperity ExpensAble that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="11951-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="11951-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="11951-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="11951-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="11951-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="11951-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="11951-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Insperity ExpensAble program.</span><span class="sxs-lookup"><span data-stu-id="11951-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Insperity ExpensAble application.</span></span>

<span data-ttu-id="11951-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Insperity ExpensAble:**</span><span class="sxs-lookup"><span data-stu-id="11951-150">**To configure Azure AD single sign-on with Insperity ExpensAble, perform the following steps:**</span></span>

1. <span data-ttu-id="11951-151">I Azure-portalen på den **Insperity ExpensAble** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="11951-151">In the Azure portal, on the **Insperity ExpensAble** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="11951-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="11951-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_samlbase.png)

3. <span data-ttu-id="11951-155">På den **Insperity ExpensAble domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="11951-155">On the **Insperity ExpensAble Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_url.png)

    <span data-ttu-id="11951-157">a.</span><span class="sxs-lookup"><span data-stu-id="11951-157">a.</span></span> <span data-ttu-id="11951-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://server.expensable.com/esapp/Authenticate?companyId=<company ID>`</span><span class="sxs-lookup"><span data-stu-id="11951-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://server.expensable.com/esapp/Authenticate?companyId=<company ID>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="11951-159">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="11951-159">This value is not real.</span></span> <span data-ttu-id="11951-160">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="11951-160">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="11951-161">Kontakta [Insperity ExpensAble klienten supportteamet](http://expensable.com/support/support-overview) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="11951-161">Contact [Insperity ExpensAble Client support team](http://expensable.com/support/support-overview) to get this value.</span></span> 
 
4. <span data-ttu-id="11951-162">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="11951-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_certificate.png) 

5. <span data-ttu-id="11951-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="11951-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="11951-166">På den **Insperity ExpensAble Configuration** klickar du på **konfigurera Insperity ExpensAble** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="11951-166">On the **Insperity ExpensAble Configuration** section, click **Configure Insperity ExpensAble** to open **Configure sign-on** window.</span></span> <span data-ttu-id="11951-167">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="11951-167">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_configure.png) 

7. <span data-ttu-id="11951-169">Konfigurera enkel inloggning på **Insperity ExpensAble** sida, måste du skicka den hämtade **XML-Metadata för**, **SAML enkel inloggning Tjänstwebbadress** och **SAML enhets-ID** till [Insperity ExpensAble supportteamet](http://expensable.com/support/support-overview).</span><span class="sxs-lookup"><span data-stu-id="11951-169">To configure single sign-on on **Insperity ExpensAble** side, you need to send the downloaded **Metadata XML**, **SAML Single Sign-On Service URL** and **SAML Entity ID** to [Insperity ExpensAble support team](http://expensable.com/support/support-overview).</span></span> <span data-ttu-id="11951-170">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="11951-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="11951-171">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="11951-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="11951-172">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="11951-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="11951-173">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="11951-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="11951-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="11951-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="11951-175">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="11951-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="11951-177">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="11951-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="11951-178">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="11951-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insperityexpensable-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="11951-180">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="11951-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insperityexpensable-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="11951-182">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="11951-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insperityexpensable-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="11951-184">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="11951-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insperityexpensable-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="11951-186">a.</span><span class="sxs-lookup"><span data-stu-id="11951-186">a.</span></span> <span data-ttu-id="11951-187">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="11951-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="11951-188">b.</span><span class="sxs-lookup"><span data-stu-id="11951-188">b.</span></span> <span data-ttu-id="11951-189">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="11951-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="11951-190">c.</span><span class="sxs-lookup"><span data-stu-id="11951-190">c.</span></span> <span data-ttu-id="11951-191">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="11951-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="11951-192">d.</span><span class="sxs-lookup"><span data-stu-id="11951-192">d.</span></span> <span data-ttu-id="11951-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="11951-193">Click **Create**.</span></span>
 
### <a name="creating-an-insperity-expensable-test-user"></a><span data-ttu-id="11951-194">Skapa en Insperity ExpensAble testanvändare</span><span class="sxs-lookup"><span data-stu-id="11951-194">Creating an Insperity ExpensAble test user</span></span>

<span data-ttu-id="11951-195">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Insperity ExpensAble.</span><span class="sxs-lookup"><span data-stu-id="11951-195">The objective of this section is to create a user called Britta Simon in Insperity ExpensAble.</span></span> <span data-ttu-id="11951-196">Se tillsammans med [Insperity ExpensAble supportteamet](http://expensable.com/support/support-overview) att lägga till användare i Insperity ExpensAble kontot.</span><span class="sxs-lookup"><span data-stu-id="11951-196">Please work with [Insperity ExpensAble support team](http://expensable.com/support/support-overview) to add the users in the Insperity ExpensAble account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="11951-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="11951-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="11951-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Insperity ExpensAble.</span><span class="sxs-lookup"><span data-stu-id="11951-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Insperity ExpensAble.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="11951-200">**Om du vill tilldela Britta Simon Insperity ExpensAble, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="11951-200">**To assign Britta Simon to Insperity ExpensAble, perform the following steps:**</span></span>

1. <span data-ttu-id="11951-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="11951-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="11951-203">Välj i listan med program **Insperity ExpensAble**.</span><span class="sxs-lookup"><span data-stu-id="11951-203">In the applications list, select **Insperity ExpensAble**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_app.png) 

3. <span data-ttu-id="11951-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="11951-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="11951-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="11951-207">Click **Add** button.</span></span> <span data-ttu-id="11951-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="11951-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="11951-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="11951-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="11951-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="11951-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="11951-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="11951-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="11951-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="11951-213">Testing single sign-on</span></span>

<span data-ttu-id="11951-214">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="11951-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="11951-215">När du klickar på panelen Insperity ExpensAble på åtkomstpanelen du bör få automatiskt loggat in på ditt Insperity ExpensAble program.</span><span class="sxs-lookup"><span data-stu-id="11951-215">When you click the Insperity ExpensAble tile in the Access Panel, you should get automatically signed-on to your Insperity ExpensAble application.</span></span>
<span data-ttu-id="11951-216">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="11951-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11951-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="11951-217">Additional resources</span></span>

* [<span data-ttu-id="11951-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="11951-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="11951-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="11951-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_04.png
[100]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_203.png

