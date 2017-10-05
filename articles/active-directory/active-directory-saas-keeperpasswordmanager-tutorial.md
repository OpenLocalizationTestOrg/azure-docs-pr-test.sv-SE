---
title: "Självstudier: Azure Active Directory-integrering med sköter Lösenordshanteraren & digitala valvet | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och sköter Lösenordshanteraren & digitala valvet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e1a98f6a-2dae-4734-bdbf-4fba742a61d2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 36504a281756b980e3348e7f892ba08821873b52
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a><span data-ttu-id="5c043-103">Självstudier: Azure Active Directory-integrering med sköter Lösenordshanteraren & digitala valvet</span><span class="sxs-lookup"><span data-stu-id="5c043-103">Tutorial: Azure Active Directory integration with Keeper Password Manager & Digital Vault</span></span>

<span data-ttu-id="5c043-104">I kursen får lära du att integrera sköter Lösenordshanteraren & digitala valvet med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5c043-104">In this tutorial, you learn how to integrate Keeper Password Manager & Digital Vault with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5c043-105">Integrera sköter Lösenordshanteraren & digitala valvet med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5c043-105">Integrating Keeper Password Manager & Digital Vault with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5c043-106">Du kan styra i Azure AD som har åtkomst till sköter Lösenordshanteraren & digitala valvet</span><span class="sxs-lookup"><span data-stu-id="5c043-106">You can control in Azure AD who has access to Keeper Password Manager & Digital Vault</span></span>
- <span data-ttu-id="5c043-107">Du kan aktivera användarna att automatiskt hämta loggat in på sköter Lösenordshanteraren & digitala valvet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5c043-107">You can enable your users to automatically get signed-on to Keeper Password Manager & Digital Vault (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5c043-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5c043-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5c043-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5c043-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c043-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5c043-110">Prerequisites</span></span>

<span data-ttu-id="5c043-111">Om du vill konfigurera Azure AD-integrering med sköter Lösenordshanteraren & digitala valv behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="5c043-111">To configure Azure AD integration with Keeper Password Manager & Digital Vault, you need the following items:</span></span>

- <span data-ttu-id="5c043-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5c043-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5c043-113">En sköter Lösenordshanteraren & digitala valvet enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="5c043-113">A Keeper Password Manager & Digital Vault single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5c043-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5c043-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5c043-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5c043-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5c043-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5c043-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5c043-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5c043-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5c043-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5c043-118">Scenario description</span></span>
<span data-ttu-id="5c043-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5c043-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5c043-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5c043-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5c043-121">Att lägga till sköter Lösenordshanteraren & digitala valvet från galleriet</span><span class="sxs-lookup"><span data-stu-id="5c043-121">Adding Keeper Password Manager & Digital Vault from the gallery</span></span>
2. <span data-ttu-id="5c043-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c043-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-keeper-password-manager--digital-vault-from-the-gallery"></a><span data-ttu-id="5c043-123">Att lägga till sköter Lösenordshanteraren & digitala valvet från galleriet</span><span class="sxs-lookup"><span data-stu-id="5c043-123">Adding Keeper Password Manager & Digital Vault from the gallery</span></span>
<span data-ttu-id="5c043-124">Du måste lägga till sköter Lösenordshanteraren & digitala valvet från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av sköter Lösenordshanteraren & digitala valvet i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c043-124">To configure the integration of Keeper Password Manager & Digital Vault into Azure AD, you need to add Keeper Password Manager & Digital Vault from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5c043-125">**Utför följande steg för att lägga till sköter Lösenordshanteraren & digitala valvet från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="5c043-125">**To add Keeper Password Manager & Digital Vault from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5c043-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5c043-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5c043-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5c043-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5c043-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5c043-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5c043-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c043-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5c043-133">I sökrutan skriver **sköter Lösenordshanteraren & digitala valvet**.</span><span class="sxs-lookup"><span data-stu-id="5c043-133">In the search box, type **Keeper Password Manager & Digital Vault**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_search.png)

5. <span data-ttu-id="5c043-135">Välj i resultatpanelen **sköter Lösenordshanteraren & digitala valvet**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="5c043-135">In the results panel, select **Keeper Password Manager & Digital Vault**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5c043-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c043-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5c043-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med sköter Lösenordshanteraren & digitala valvet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5c043-138">In this section, you configure and test Azure AD single sign-on with Keeper Password Manager & Digital Vault based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5c043-139">Azure AD måste du känna till motsvarande användaren i sköter Lösenordshanteraren & digitala valvet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="5c043-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Keeper Password Manager & Digital Vault is to a user in Azure AD.</span></span> <span data-ttu-id="5c043-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i sköter Lösenordshanteraren & digitala valvet upprättas.</span><span class="sxs-lookup"><span data-stu-id="5c043-140">In other words, a link relationship between an Azure AD user and the related user in Keeper Password Manager & Digital Vault needs to be established.</span></span>

<span data-ttu-id="5c043-141">I sköter Lösenordshanteraren & digitala valvet, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="5c043-141">In Keeper Password Manager & Digital Vault, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5c043-142">Om du vill konfigurera och testa Azure AD enkel inloggning med sköter Lösenordshanteraren & digitala valvet, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5c043-142">To configure and test Azure AD single sign-on with Keeper Password Manager & Digital Vault, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5c043-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5c043-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5c043-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c043-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5c043-145">**[Skapa en testanvändare sköter Lösenordshanteraren & digitala valvet](#creating-a-keeperpasswordmanager-test-user)**  – du har en motsvarighet för Britta Simon i sköter Lösenordshanteraren & digitala valvet som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5c043-145">**[Creating a Keeper Password Manager & Digital Vault test user](#creating-a-keeperpasswordmanager-test-user)** - to have a counterpart of Britta Simon in Keeper Password Manager & Digital Vault that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5c043-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5c043-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5c043-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5c043-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5c043-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c043-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5c043-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet sköter Lösenordshanteraren & digitala valvet.</span><span class="sxs-lookup"><span data-stu-id="5c043-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Keeper Password Manager & Digital Vault application.</span></span>

<span data-ttu-id="5c043-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med sköter Lösenordshanteraren & digitala valvet:**</span><span class="sxs-lookup"><span data-stu-id="5c043-150">**To configure Azure AD single sign-on with Keeper Password Manager & Digital Vault, perform the following steps:**</span></span>

1. <span data-ttu-id="5c043-151">I Azure-portalen på den **sköter Lösenordshanteraren & digitala valvet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5c043-151">In the Azure portal, on the **Keeper Password Manager & Digital Vault** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5c043-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5c043-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_samlbase.png)

3. <span data-ttu-id="5c043-155">På den **sköter Lösenordshanteraren & digitala valvet domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c043-155">On the **Keeper Password Manager & Digital Vault Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_url.png)

    <span data-ttu-id="5c043-157">a.</span><span class="sxs-lookup"><span data-stu-id="5c043-157">a.</span></span> <span data-ttu-id="5c043-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://{SSO CONNECT SERVER}/sso-connect/saml/login`</span><span class="sxs-lookup"><span data-stu-id="5c043-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://{SSO CONNECT SERVER}/sso-connect/saml/login`</span></span>

    <span data-ttu-id="5c043-159">b.</span><span class="sxs-lookup"><span data-stu-id="5c043-159">b.</span></span> <span data-ttu-id="5c043-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://{SSO CONNECT SERVER}/sso-connect/saml/sso`</span><span class="sxs-lookup"><span data-stu-id="5c043-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://{SSO CONNECT SERVER}/sso-connect/saml/sso`</span></span>

    <span data-ttu-id="5c043-161">c.</span><span class="sxs-lookup"><span data-stu-id="5c043-161">c.</span></span> <span data-ttu-id="5c043-162">I den **identifierare** textruta Skriv en URL med följande mönster:`https://{SSO CONNECT SERVER}/sso-connect`</span><span class="sxs-lookup"><span data-stu-id="5c043-162">In the **Identifier** textbox, type a URL using the following pattern: `https://{SSO CONNECT SERVER}/sso-connect`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5c043-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="5c043-163">These values are not real.</span></span> <span data-ttu-id="5c043-164">Uppdatera dessa värden med den faktiska Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="5c043-164">Update these values with the actual Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="5c043-165">Kontakta [sköter Lösenordshanteraren & digitala valvet klienten supportteam](https://keepersecurity.com/contact.html) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="5c043-165">Contact [Keeper Password Manager & Digital Vault Client support team](https://keepersecurity.com/contact.html) to get these values.</span></span> 

4. <span data-ttu-id="5c043-166">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="5c043-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_certificate.png) 

5. <span data-ttu-id="5c043-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5c043-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="5c043-170">På den **sköter Lösenordshanteraren & digitala valvet Configuration** klickar du på **konfigurera sköter Lösenordshanteraren & digitala valvet** att öppna **konfigurera inloggning** fönstret.</span><span class="sxs-lookup"><span data-stu-id="5c043-170">On the **Keeper Password Manager & Digital Vault Configuration** section, click **Configure Keeper Password Manager & Digital Vault** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5c043-171">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="5c043-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_configure.png) 

7. <span data-ttu-id="5c043-173">Konfigurera enkel inloggning på **sköter Lösenordshanteraren & digitala valvet Configuration** sida, Följ riktlinjerna i [sköter supportguide](https://keepersecurity.com/assets/pdf/SettingupAzurewithKeeperSSOConnect.pdf)</span><span class="sxs-lookup"><span data-stu-id="5c043-173">To configure single sign-on on **Keeper Password Manager & Digital Vault Configuration** side, follow the guidelines given at [Keeper Support Guide](https://keepersecurity.com/assets/pdf/SettingupAzurewithKeeperSSOConnect.pdf)</span></span>

> [!TIP]
> <span data-ttu-id="5c043-174">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="5c043-174">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5c043-175">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="5c043-175">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5c043-176">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5c043-176">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5c043-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c043-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="5c043-178">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c043-178">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5c043-180">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="5c043-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5c043-181">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5c043-181">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5c043-183">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5c043-183">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5c043-185">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c043-185">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5c043-187">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c043-187">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5c043-189">a.</span><span class="sxs-lookup"><span data-stu-id="5c043-189">a.</span></span> <span data-ttu-id="5c043-190">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5c043-190">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5c043-191">b.</span><span class="sxs-lookup"><span data-stu-id="5c043-191">b.</span></span> <span data-ttu-id="5c043-192">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5c043-192">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5c043-193">c.</span><span class="sxs-lookup"><span data-stu-id="5c043-193">c.</span></span> <span data-ttu-id="5c043-194">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5c043-194">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5c043-195">d.</span><span class="sxs-lookup"><span data-stu-id="5c043-195">d.</span></span> <span data-ttu-id="5c043-196">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5c043-196">Click **Create**.</span></span>
 
### <a name="creating-a-keeper-password-manager--digital-vault-test-user"></a><span data-ttu-id="5c043-197">Skapa en testanvändare sköter Lösenordshanteraren & digitala valvet</span><span class="sxs-lookup"><span data-stu-id="5c043-197">Creating a Keeper Password Manager & Digital Vault test user</span></span>

<span data-ttu-id="5c043-198">Om du vill aktivera Azure AD-användare kan logga in på sköter Lösenordshanteraren & digitala valvet, måste de etableras i sköter Lösenordshanteraren & digitala valvet.</span><span class="sxs-lookup"><span data-stu-id="5c043-198">To enable Azure AD users to log in to Keeper Password Manager & Digital Vault, they must be provisioned into Keeper Password Manager & Digital Vault.</span></span> <span data-ttu-id="5c043-199">Programmet stöder bara i tid användaretablering och authentication-användare kommer automatiskt att skapas i programmet.</span><span class="sxs-lookup"><span data-stu-id="5c043-199">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span> <span data-ttu-id="5c043-200">Du kan kontakta [sköter stöd](https://keepersecurity.com/contact.html), om du vill installera användare manuellt.</span><span class="sxs-lookup"><span data-stu-id="5c043-200">You can contact [Keeper Support](https://keepersecurity.com/contact.html), if you want to setup users manually.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5c043-201">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="5c043-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5c043-202">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till sköter Lösenordshanteraren & digitala valvet.</span><span class="sxs-lookup"><span data-stu-id="5c043-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Keeper Password Manager & Digital Vault.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5c043-204">**Om du vill tilldela sköter Lösenordshanteraren & digitala valvet Britta Simon, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c043-204">**To assign Britta Simon to Keeper Password Manager & Digital Vault, perform the following steps:**</span></span>

1. <span data-ttu-id="5c043-205">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5c043-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5c043-207">Välj i listan med program **sköter Lösenordshanteraren & digitala valvet**.</span><span class="sxs-lookup"><span data-stu-id="5c043-207">In the applications list, select **Keeper Password Manager & Digital Vault**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_app.png) 

3. <span data-ttu-id="5c043-209">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5c043-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5c043-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5c043-211">Click **Add** button.</span></span> <span data-ttu-id="5c043-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c043-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5c043-214">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="5c043-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5c043-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c043-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5c043-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c043-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5c043-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c043-217">Testing single sign-on</span></span>

<span data-ttu-id="5c043-218">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5c043-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5c043-219">Du bör få inloggningssidan för sköter Lösenordshanteraren & digitala valvet program när du klickar på panelen sköter Lösenordshanteraren & digitala valvet på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5c043-219">When you click the Keeper Password Manager & Digital Vault tile in the Access Panel, you should get login page of Keeper Password Manager & Digital Vault application.</span></span> <span data-ttu-id="5c043-220">Du bör få till programmet när autentiseringen lyckas.</span><span class="sxs-lookup"><span data-stu-id="5c043-220">Upon successful authentication, you should get into the application.</span></span> <span data-ttu-id="5c043-221">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5c043-221">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5c043-222">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5c043-222">Additional resources</span></span>

* [<span data-ttu-id="5c043-223">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5c043-223">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5c043-224">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5c043-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_203.png

