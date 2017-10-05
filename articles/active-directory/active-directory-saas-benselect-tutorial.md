---
title: "Självstudier: Azure Active Directory-integrering med BenSelect | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och BenSelect."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffa17478-3ea1-4356-a289-545b5b9a4494
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: f8caa023da05863372b7ef92b47a92168445d741
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benselect"></a><span data-ttu-id="7d040-103">Självstudier: Azure Active Directory-integrering med BenSelect</span><span class="sxs-lookup"><span data-stu-id="7d040-103">Tutorial: Azure Active Directory integration with BenSelect</span></span>

<span data-ttu-id="7d040-104">I kursen får lära du att integrera BenSelect med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7d040-104">In this tutorial, you learn how to integrate BenSelect with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7d040-105">Integrera BenSelect med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7d040-105">Integrating BenSelect with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7d040-106">Du kan styra i Azure AD som har åtkomst till BenSelect</span><span class="sxs-lookup"><span data-stu-id="7d040-106">You can control in Azure AD who has access to BenSelect</span></span>
- <span data-ttu-id="7d040-107">Du kan aktivera användarna att automatiskt hämta loggat in på BenSelect (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7d040-107">You can enable your users to automatically get signed-on to BenSelect (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7d040-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7d040-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7d040-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7d040-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d040-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7d040-110">Prerequisites</span></span>

<span data-ttu-id="7d040-111">För att konfigurera Azure AD-integrering med BenSelect, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="7d040-111">To configure Azure AD integration with BenSelect, you need the following items:</span></span>

- <span data-ttu-id="7d040-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7d040-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7d040-113">En BenSelect enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7d040-113">A BenSelect single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7d040-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7d040-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7d040-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7d040-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7d040-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7d040-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7d040-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7d040-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7d040-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7d040-118">Scenario description</span></span>
<span data-ttu-id="7d040-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7d040-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7d040-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7d040-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7d040-121">Att lägga till BenSelect från galleriet</span><span class="sxs-lookup"><span data-stu-id="7d040-121">Adding BenSelect from the gallery</span></span>
2. <span data-ttu-id="7d040-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7d040-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-benselect-from-the-gallery"></a><span data-ttu-id="7d040-123">Att lägga till BenSelect från galleriet</span><span class="sxs-lookup"><span data-stu-id="7d040-123">Adding BenSelect from the gallery</span></span>
<span data-ttu-id="7d040-124">Du måste lägga till BenSelect från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av BenSelect i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d040-124">To configure the integration of BenSelect into Azure AD, you need to add BenSelect from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7d040-125">**Utför följande steg för att lägga till BenSelect från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="7d040-125">**To add BenSelect from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7d040-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7d040-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7d040-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7d040-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7d040-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7d040-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7d040-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7d040-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7d040-133">I sökrutan skriver **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="7d040-133">In the search box, type **BenSelect**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_search.png)

5. <span data-ttu-id="7d040-135">Välj i resultatpanelen **BenSelect**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="7d040-135">In the results panel, select **BenSelect**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7d040-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7d040-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7d040-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BenSelect baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7d040-138">In this section, you configure and test Azure AD single sign-on with BenSelect based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7d040-139">Azure AD måste du känna till användaren i BenSelect motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="7d040-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BenSelect is to a user in Azure AD.</span></span> <span data-ttu-id="7d040-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i BenSelect upprättas.</span><span class="sxs-lookup"><span data-stu-id="7d040-140">In other words, a link relationship between an Azure AD user and the related user in BenSelect needs to be established.</span></span>

<span data-ttu-id="7d040-141">I BenSelect, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7d040-141">In BenSelect, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7d040-142">Om du vill konfigurera och testa Azure AD enkel inloggning med BenSelect, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7d040-142">To configure and test Azure AD single sign-on with BenSelect, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7d040-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7d040-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7d040-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7d040-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7d040-145">**[Skapa en testanvändare BenSelect](#creating-a-benselect-test-user)**  – du har en motsvarighet för Britta Simon i BenSelect som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7d040-145">**[Creating a BenSelect test user](#creating-a-benselect-test-user)** - to have a counterpart of Britta Simon in BenSelect that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7d040-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7d040-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7d040-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7d040-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7d040-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7d040-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7d040-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt BenSelect program.</span><span class="sxs-lookup"><span data-stu-id="7d040-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BenSelect application.</span></span>

<span data-ttu-id="7d040-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med BenSelect:**</span><span class="sxs-lookup"><span data-stu-id="7d040-150">**To configure Azure AD single sign-on with BenSelect, perform the following steps:**</span></span>

1. <span data-ttu-id="7d040-151">I Azure-portalen på den **BenSelect** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7d040-151">In the Azure portal, on the **BenSelect** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7d040-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7d040-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_samlbase.png)

3. <span data-ttu-id="7d040-155">På den **BenSelect domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7d040-155">On the **BenSelect Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_url.png)

    <span data-ttu-id="7d040-157">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span><span class="sxs-lookup"><span data-stu-id="7d040-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7d040-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7d040-158">This value is not real.</span></span> <span data-ttu-id="7d040-159">Uppdatera det här värdet med det faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="7d040-159">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="7d040-160">Kontakta [BenSelect supportteamet](mailto:support@selerix.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="7d040-160">Contact [BenSelect support team](mailto:support@selerix.com) to get this value.</span></span>
 
4. <span data-ttu-id="7d040-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Raw)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="7d040-161">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_certificate.png) 

5. <span data-ttu-id="7d040-163">BenSelect program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="7d040-163">BenSelect application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="7d040-164">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="7d040-164">Configure the following claims for this application.</span></span> <span data-ttu-id="7d040-165">Du kan hantera värden för attributen från den **användarattribut** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="7d040-165">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="7d040-166">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="7d040-166">The following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_06.png)

6. <span data-ttu-id="7d040-168">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="7d040-168">In the **User Attributes** section on the **Single sign-on** dialog:</span></span>

    <span data-ttu-id="7d040-169">a.</span><span class="sxs-lookup"><span data-stu-id="7d040-169">a.</span></span> <span data-ttu-id="7d040-170">I den **användar-ID** listrutan, Välj **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="7d040-170">In the **User Identifier** dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="7d040-171">b.</span><span class="sxs-lookup"><span data-stu-id="7d040-171">b.</span></span> <span data-ttu-id="7d040-172">I den **e** listrutan, Välj **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="7d040-172">In the **Mail** dropdown list, select **user.userprincipalname**.</span></span>

7. <span data-ttu-id="7d040-173">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7d040-173">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="7d040-175">På den **BenSelect Configuration** klickar du på **konfigurera BenSelect** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7d040-175">On the **BenSelect Configuration** section, click **Configure BenSelect** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7d040-176">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="7d040-176">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_configure.png) 

9. <span data-ttu-id="7d040-178">Konfigurera enkel inloggning på **BenSelect** sida, måste du skicka den hämtade **Certificate(Raw)** och **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel**till [BenSelect supportteamet](mailto:support@selerix.com).</span><span class="sxs-lookup"><span data-stu-id="7d040-178">To configure single sign-on on **BenSelect** side, you need to send the downloaded **Certificate(Raw)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [BenSelect support team](mailto:support@selerix.com).</span></span>

   >[!NOTE]
   ><span data-ttu-id="7d040-179">Du måste ange att den här integrationen kräver SHA256-algoritmen (SHA1 inte stöds) Ange SSO på rätt server som app2101 osv.</span><span class="sxs-lookup"><span data-stu-id="7d040-179">You need to mention that this integration requires the SHA256 algorithm (SHA1 is not supported) to set the SSO on the appropriate server like app2101 etc.</span></span> 
   
> [!TIP]
> <span data-ttu-id="7d040-180">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="7d040-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7d040-181">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="7d040-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7d040-182">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7d040-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7d040-183">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d040-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="7d040-184">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7d040-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7d040-186">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="7d040-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7d040-187">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7d040-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7d040-189">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7d040-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7d040-191">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7d040-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7d040-193">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7d040-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7d040-195">a.</span><span class="sxs-lookup"><span data-stu-id="7d040-195">a.</span></span> <span data-ttu-id="7d040-196">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7d040-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7d040-197">b.</span><span class="sxs-lookup"><span data-stu-id="7d040-197">b.</span></span> <span data-ttu-id="7d040-198">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7d040-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7d040-199">c.</span><span class="sxs-lookup"><span data-stu-id="7d040-199">c.</span></span> <span data-ttu-id="7d040-200">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7d040-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7d040-201">d.</span><span class="sxs-lookup"><span data-stu-id="7d040-201">d.</span></span> <span data-ttu-id="7d040-202">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7d040-202">Click **Create**.</span></span>
 
### <a name="creating-a-benselect-test-user"></a><span data-ttu-id="7d040-203">Skapa en testanvändare BenSelect</span><span class="sxs-lookup"><span data-stu-id="7d040-203">Creating a BenSelect test user</span></span>

<span data-ttu-id="7d040-204">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i BenSelect.</span><span class="sxs-lookup"><span data-stu-id="7d040-204">The objective of this section is to create a user called Britta Simon in BenSelect.</span></span> <span data-ttu-id="7d040-205">Arbeta med [BenSelect supportteamet](mailto:support@selerix.com) att lägga till användare i BenSelect-konto.</span><span class="sxs-lookup"><span data-stu-id="7d040-205">Work with [BenSelect support team](mailto:support@selerix.com) to add the users in the BenSelect account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7d040-206">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="7d040-206">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7d040-207">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till BenSelect.</span><span class="sxs-lookup"><span data-stu-id="7d040-207">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BenSelect.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7d040-209">**Om du vill tilldela BenSelect Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7d040-209">**To assign Britta Simon to BenSelect, perform the following steps:**</span></span>

1. <span data-ttu-id="7d040-210">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7d040-210">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7d040-212">Välj i listan med program **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="7d040-212">In the applications list, select **BenSelect**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_app.png) 

3. <span data-ttu-id="7d040-214">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7d040-214">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7d040-216">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7d040-216">Click **Add** button.</span></span> <span data-ttu-id="7d040-217">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7d040-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7d040-219">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="7d040-219">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7d040-220">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7d040-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7d040-221">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7d040-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7d040-222">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7d040-222">Testing single sign-on</span></span>

<span data-ttu-id="7d040-223">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7d040-223">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="7d040-224">När du klickar på panelen BenSelect på åtkomstpanelen du bör få automatiskt loggat in på ditt BenSelect program.</span><span class="sxs-lookup"><span data-stu-id="7d040-224">When you click the BenSelect tile in the Access Panel, you should get automatically signed-on to your BenSelect application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d040-225">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7d040-225">Additional resources</span></span>

* [<span data-ttu-id="7d040-226">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7d040-226">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7d040-227">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7d040-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_203.png

