---
title: "Självstudier: Azure Active Directory-integrering med Neota logik Studio | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Neota logik Studio."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 842605e6-a91d-42cc-a0bb-e23e67173ae2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: jeedes
ms.openlocfilehash: 99018277392cab44a6b579ad45b4611739a803d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-neota-logic-studio"></a><span data-ttu-id="fee96-103">Självstudier: Azure Active Directory-integrering med Neota logik Studio</span><span class="sxs-lookup"><span data-stu-id="fee96-103">Tutorial: Azure Active Directory integration with Neota Logic Studio</span></span>

<span data-ttu-id="fee96-104">I kursen får lära du att integrera Neota logik Studio med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="fee96-104">In this tutorial, you learn how to integrate Neota Logic Studio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fee96-105">Integrera Neota logik Studio med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="fee96-105">Integrating Neota Logic Studio with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fee96-106">Du kan styra i Azure AD som har åtkomst till Neota logik Studio</span><span class="sxs-lookup"><span data-stu-id="fee96-106">You can control in Azure AD who has access to Neota Logic Studio</span></span>
- <span data-ttu-id="fee96-107">Du kan aktivera användarna att automatiskt hämta loggat in på Neota logik Studio (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="fee96-107">You can enable your users to automatically get signed-on to Neota Logic Studio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fee96-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fee96-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="fee96-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns i.</span><span class="sxs-lookup"><span data-stu-id="fee96-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="fee96-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fee96-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fee96-111">Krav</span><span class="sxs-lookup"><span data-stu-id="fee96-111">Prerequisites</span></span>

<span data-ttu-id="fee96-112">För att konfigurera Azure AD-integrering med Neota logik Studio, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="fee96-112">To configure Azure AD integration with Neota Logic Studio, you need the following items:</span></span>

- <span data-ttu-id="fee96-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="fee96-113">An Azure AD subscription</span></span>
- <span data-ttu-id="fee96-114">En Neota logik Studio enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="fee96-114">A Neota Logic Studio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fee96-115">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="fee96-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fee96-116">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="fee96-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fee96-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="fee96-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fee96-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fee96-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fee96-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="fee96-119">Scenario description</span></span>

<span data-ttu-id="fee96-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="fee96-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fee96-121">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="fee96-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fee96-122">Att lägga till Neota logik Studio från galleriet</span><span class="sxs-lookup"><span data-stu-id="fee96-122">Adding Neota Logic Studio from the gallery</span></span>
2. <span data-ttu-id="fee96-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fee96-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-neota-logic-studio-from-the-gallery"></a><span data-ttu-id="fee96-124">Att lägga till Neota logik Studio från galleriet</span><span class="sxs-lookup"><span data-stu-id="fee96-124">Adding Neota Logic Studio from the gallery</span></span>

<span data-ttu-id="fee96-125">Du måste lägga till Neota logik Studio från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Neota logik Studio i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fee96-125">To configure the integration of Neota Logic Studio into Azure AD, you need to add Neota Logic Studio from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fee96-126">**Utför följande steg för att lägga till Neota logik Studio från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="fee96-126">**To add Neota Logic Studio from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fee96-127">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fee96-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fee96-129">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="fee96-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fee96-130">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fee96-130">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="fee96-132">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fee96-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="fee96-134">I sökrutan skriver **Neota logik Studio**.</span><span class="sxs-lookup"><span data-stu-id="fee96-134">In the search box, type **Neota Logic Studio**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_search.png)

5. <span data-ttu-id="fee96-136">Välj i resultatpanelen **Neota logik Studio**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="fee96-136">In the results panel, select **Neota Logic Studio**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fee96-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fee96-138">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="fee96-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Neota logik Studio baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="fee96-139">In this section, you configure and test Azure AD single sign-on with Neota Logic Studio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="fee96-140">Azure AD måste du känna till användaren i Neota logik Studio motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="fee96-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Neota Logic Studio is to a user in Azure AD.</span></span> <span data-ttu-id="fee96-141">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Neota logik Studio upprättas.</span><span class="sxs-lookup"><span data-stu-id="fee96-141">In other words, a link relationship between an Azure AD user and the related user in Neota Logic Studio needs to be established.</span></span>

<span data-ttu-id="fee96-142">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Neota logik Studio.</span><span class="sxs-lookup"><span data-stu-id="fee96-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Neota Logic Studio.</span></span>

<span data-ttu-id="fee96-143">Om du vill konfigurera och testa Azure AD enkel inloggning med Neota logik Studio, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="fee96-143">To configure and test Azure AD single sign-on with Neota Logic Studio, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fee96-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="fee96-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fee96-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fee96-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fee96-146">**[Skapa en testanvändare Neota logik Studio](#creating-a-neota-logic-studio-test-user)**  – du har en motsvarighet för Britta Simon i Neota logik Studio som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="fee96-146">**[Creating a Neota Logic Studio test user](#creating-a-neota-logic-studio-test-user)** - to have a counterpart of Britta Simon in Neota Logic Studio that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fee96-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fee96-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fee96-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="fee96-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fee96-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fee96-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fee96-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Neota logik Studio.</span><span class="sxs-lookup"><span data-stu-id="fee96-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Neota Logic Studio application.</span></span>

<span data-ttu-id="fee96-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Neota logik Studio:**</span><span class="sxs-lookup"><span data-stu-id="fee96-151">**To configure Azure AD single sign-on with Neota Logic Studio, perform the following steps:**</span></span>

1. <span data-ttu-id="fee96-152">I Azure-portalen på den **Neota logik Studio** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="fee96-152">In the Azure portal, on the **Neota Logic Studio** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="fee96-154">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fee96-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_samlbase.png)

3. <span data-ttu-id="fee96-156">På den **Neota logik Studio domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fee96-156">On the **Neota Logic Studio Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_url.png)

    <span data-ttu-id="fee96-158">a.</span><span class="sxs-lookup"><span data-stu-id="fee96-158">a.</span></span> <span data-ttu-id="fee96-159">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<sub domain>.neotalogic.com/a/<sub application>`</span><span class="sxs-lookup"><span data-stu-id="fee96-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<sub domain>.neotalogic.com/a/<sub application>`</span></span>

    <span data-ttu-id="fee96-160">b.</span><span class="sxs-lookup"><span data-stu-id="fee96-160">b.</span></span> <span data-ttu-id="fee96-161">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<sub domain>.neotalogic.com/wb`</span><span class="sxs-lookup"><span data-stu-id="fee96-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<sub domain>.neotalogic.com/wb`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fee96-162">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="fee96-162">These values are not the real.</span></span> <span data-ttu-id="fee96-163">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="fee96-163">Update these values with the actual Sign-On URL & Identifier.</span></span> <span data-ttu-id="fee96-164">Här rekommenderar vi att du om du vill använda det unika värdet på strängen i identifieraren.</span><span class="sxs-lookup"><span data-stu-id="fee96-164">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="fee96-165">Kontakta [Neota logik Studio klienten supportteamet](https://www.neotalogic.com/contact-us/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="fee96-165">Contact [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="fee96-166">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="fee96-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_certificate.png) 

5. <span data-ttu-id="fee96-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="fee96-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fee96-170">För att få SSO konfigurerats för ditt program, kontakta [Neota logik Studio stöd](https://www.neotalogic.com/contact-us/) grupp- och ge dem med hämtade **XML-Metadata för** fil.</span><span class="sxs-lookup"><span data-stu-id="fee96-170">To get SSO configured for your application, Contact [Neota Logic Studio support](https://www.neotalogic.com/contact-us/) team and provide them with downloaded **Metadata XML** file.</span></span>

> [!TIP]
> <span data-ttu-id="fee96-171">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="fee96-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fee96-172">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="fee96-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fee96-173">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fee96-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fee96-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="fee96-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="fee96-175">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fee96-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="fee96-177">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="fee96-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fee96-178">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fee96-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fee96-180">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="fee96-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fee96-182">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fee96-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fee96-184">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fee96-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fee96-186">a.</span><span class="sxs-lookup"><span data-stu-id="fee96-186">a.</span></span> <span data-ttu-id="fee96-187">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fee96-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fee96-188">b.</span><span class="sxs-lookup"><span data-stu-id="fee96-188">b.</span></span> <span data-ttu-id="fee96-189">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fee96-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fee96-190">c.</span><span class="sxs-lookup"><span data-stu-id="fee96-190">c.</span></span> <span data-ttu-id="fee96-191">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="fee96-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="fee96-192">d.</span><span class="sxs-lookup"><span data-stu-id="fee96-192">d.</span></span> <span data-ttu-id="fee96-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fee96-193">Click **Create**.</span></span>
 
### <a name="creating-a-neota-logic-studio-test-user"></a><span data-ttu-id="fee96-194">Skapa en testanvändare Neota logik Studio</span><span class="sxs-lookup"><span data-stu-id="fee96-194">Creating a Neota Logic Studio test user</span></span>

<span data-ttu-id="fee96-195">I det här avsnittet skapar du en användare som kallas Britta Simon i Neota logik Studio.</span><span class="sxs-lookup"><span data-stu-id="fee96-195">In this section, you create a user called Britta Simon in Neota Logic Studio.</span></span> <span data-ttu-id="fee96-196">Arbeta med [Neota logik Studio klienten supportteamet](https://www.neotalogic.com/contact-us/) att lägga till användare i Neota logik Studio-plattformen.</span><span class="sxs-lookup"><span data-stu-id="fee96-196">work with [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) to add the users in the Neota Logic Studio platform.</span></span> <span data-ttu-id="fee96-197">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fee96-197">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="fee96-198">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="fee96-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="fee96-199">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Neota logik Studio.</span><span class="sxs-lookup"><span data-stu-id="fee96-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Neota Logic Studio.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="fee96-201">**Om du vill tilldela Neota logik Studio Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fee96-201">**To assign Britta Simon to Neota Logic Studio, perform the following steps:**</span></span>

1. <span data-ttu-id="fee96-202">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fee96-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="fee96-204">Välj i listan med program **Neota logik Studio**.</span><span class="sxs-lookup"><span data-stu-id="fee96-204">In the applications list, select **Neota Logic Studio**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_app.png) 

3. <span data-ttu-id="fee96-206">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="fee96-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="fee96-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="fee96-208">Click **Add** button.</span></span> <span data-ttu-id="fee96-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fee96-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="fee96-211">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="fee96-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fee96-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fee96-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fee96-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fee96-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fee96-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fee96-214">Testing single sign-on</span></span>

<span data-ttu-id="fee96-215">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fee96-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fee96-216">Klicka på panelen Neota logik Studio på åtkomstpanelen, du omdirigeras till organisation inloggning sida.</span><span class="sxs-lookup"><span data-stu-id="fee96-216">Click the Neota Logic Studio tile in the Access Panel, you will be redirected to Organization sign-on page.</span></span> <span data-ttu-id="fee96-217">Efter genomförd inloggning du kommer att loggat in på ditt Neota logik Studio-program.</span><span class="sxs-lookup"><span data-stu-id="fee96-217">After successful login, you will be signed-on to your Neota Logic Studio application.</span></span> <span data-ttu-id="fee96-218">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="fee96-218">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

<span data-ttu-id="fee96-219">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="fee96-219">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="fee96-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="fee96-220">Additional resources</span></span>

* [<span data-ttu-id="fee96-221">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fee96-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fee96-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fee96-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_203.png

