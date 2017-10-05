---
title: "Självstudier: Azure Active Directory-integrering med LogicMonitor | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och LogicMonitor."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 496156c3-0e22-4492-b36f-2c29c055e087
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: e49960cac868f80af3e9165a9f75e49be87515f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a><span data-ttu-id="94b1c-103">Självstudier: Azure Active Directory-integrering med LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="94b1c-103">Tutorial: Azure Active Directory integration with LogicMonitor</span></span>

<span data-ttu-id="94b1c-104">I kursen får lära du att integrera LogicMonitor med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="94b1c-104">In this tutorial, you learn how to integrate LogicMonitor with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="94b1c-105">Integrera LogicMonitor med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="94b1c-105">Integrating LogicMonitor with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="94b1c-106">Du kan styra i Azure AD som har åtkomst till LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="94b1c-106">You can control in Azure AD who has access to LogicMonitor</span></span>
- <span data-ttu-id="94b1c-107">Du kan aktivera användarna att automatiskt hämta loggat in på LogicMonitor (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="94b1c-107">You can enable your users to automatically get signed-on to LogicMonitor (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="94b1c-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="94b1c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="94b1c-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="94b1c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94b1c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="94b1c-110">Prerequisites</span></span>

<span data-ttu-id="94b1c-111">För att konfigurera Azure AD-integrering med LogicMonitor, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="94b1c-111">To configure Azure AD integration with LogicMonitor, you need the following items:</span></span>

- <span data-ttu-id="94b1c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="94b1c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="94b1c-113">En LogicMonitor enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="94b1c-113">A LogicMonitor single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="94b1c-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="94b1c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="94b1c-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="94b1c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="94b1c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="94b1c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="94b1c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="94b1c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="94b1c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="94b1c-118">Scenario description</span></span>
<span data-ttu-id="94b1c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="94b1c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="94b1c-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="94b1c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="94b1c-121">Att lägga till LogicMonitor från galleriet</span><span class="sxs-lookup"><span data-stu-id="94b1c-121">Adding LogicMonitor from the gallery</span></span>
2. <span data-ttu-id="94b1c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="94b1c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-logicmonitor-from-the-gallery"></a><span data-ttu-id="94b1c-123">Att lägga till LogicMonitor från galleriet</span><span class="sxs-lookup"><span data-stu-id="94b1c-123">Adding LogicMonitor from the gallery</span></span>
<span data-ttu-id="94b1c-124">Du måste lägga till LogicMonitor från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av LogicMonitor i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="94b1c-124">To configure the integration of LogicMonitor into Azure AD, you need to add LogicMonitor from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="94b1c-125">**Utför följande steg för att lägga till LogicMonitor från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="94b1c-125">**To add LogicMonitor from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="94b1c-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="94b1c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="94b1c-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="94b1c-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="94b1c-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="94b1c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="94b1c-133">I sökrutan skriver **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-133">In the search box, type **LogicMonitor**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_search.png)

5. <span data-ttu-id="94b1c-135">Välj i resultatpanelen **LogicMonitor**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="94b1c-135">In the results panel, select **LogicMonitor**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="94b1c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="94b1c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="94b1c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LogicMonitor baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="94b1c-138">In this section, you configure and test Azure AD single sign-on with LogicMonitor based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="94b1c-139">Azure AD måste du känna till användaren i LogicMonitor motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="94b1c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LogicMonitor is to a user in Azure AD.</span></span> <span data-ttu-id="94b1c-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i LogicMonitor upprättas.</span><span class="sxs-lookup"><span data-stu-id="94b1c-140">In other words, a link relationship between an Azure AD user and the related user in LogicMonitor needs to be established.</span></span>

<span data-ttu-id="94b1c-141">I LogicMonitor, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="94b1c-141">In LogicMonitor, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="94b1c-142">Om du vill konfigurera och testa Azure AD enkel inloggning med LogicMonitor, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="94b1c-142">To configure and test Azure AD single sign-on with LogicMonitor, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="94b1c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="94b1c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="94b1c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="94b1c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="94b1c-145">**[Skapa en testanvändare LogicMonitor](#creating-a-logicmonitor-test-user)**  – du har en motsvarighet för Britta Simon i LogicMonitor som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="94b1c-145">**[Creating a LogicMonitor test user](#creating-a-logicmonitor-test-user)** - to have a counterpart of Britta Simon in LogicMonitor that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="94b1c-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="94b1c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="94b1c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="94b1c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="94b1c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="94b1c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="94b1c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt LogicMonitor program.</span><span class="sxs-lookup"><span data-stu-id="94b1c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LogicMonitor application.</span></span>

<span data-ttu-id="94b1c-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med LogicMonitor:**</span><span class="sxs-lookup"><span data-stu-id="94b1c-150">**To configure Azure AD single sign-on with LogicMonitor, perform the following steps:**</span></span>

1. <span data-ttu-id="94b1c-151">I Azure-portalen på den **LogicMonitor** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-151">In the Azure portal, on the **LogicMonitor** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="94b1c-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="94b1c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_samlbase.png)

3. <span data-ttu-id="94b1c-155">På den **LogicMonitor domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="94b1c-155">On the **LogicMonitor Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_url.png)

    <span data-ttu-id="94b1c-157">a.</span><span class="sxs-lookup"><span data-stu-id="94b1c-157">a.</span></span> <span data-ttu-id="94b1c-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="94b1c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    <span data-ttu-id="94b1c-159">b.</span><span class="sxs-lookup"><span data-stu-id="94b1c-159">b.</span></span> <span data-ttu-id="94b1c-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="94b1c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="94b1c-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="94b1c-161">These values are not real.</span></span> <span data-ttu-id="94b1c-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="94b1c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="94b1c-163">Kontakta [LogicMonitor klienten supportteamet](https://www.logicmonitor.com/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="94b1c-163">Contact [LogicMonitor Client support team](https://www.logicmonitor.com/contact/) to get these values.</span></span> 
 


4. <span data-ttu-id="94b1c-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="94b1c-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_certificate.png) 

5. <span data-ttu-id="94b1c-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="94b1c-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="94b1c-168">Logga in på ditt **LogicMonitor** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="94b1c-168">Log in to your **LogicMonitor** company site as an administrator.</span></span>

7. <span data-ttu-id="94b1c-169">Klicka på menyn högst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-169">In the menu on the top, click **Settings**.</span></span>
   
   <span data-ttu-id="94b1c-170">![Inställningar för](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="94b1c-170">![Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "Settings")</span></span>

8. <span data-ttu-id="94b1c-171">I navigeringsfönstret bat på vänster sida, klickar du på **enkel inloggning**</span><span class="sxs-lookup"><span data-stu-id="94b1c-171">In the navigation bat on the left side, click **Single Sign On**</span></span>
   
   <span data-ttu-id="94b1c-172">![Enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="94b1c-172">![Single Sign-On](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "Single Sign-On")</span></span>

9. <span data-ttu-id="94b1c-173">I den **inställningar för enkel inloggning (SSO)** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="94b1c-173">In the **Single Sign-on (SSO) settings** section, perform the following steps:</span></span>
   
   <span data-ttu-id="94b1c-174">![Enkel inloggning inställningar](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="94b1c-174">![Single Sign-On Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "Single Sign-On Settings")</span></span>
   
   <span data-ttu-id="94b1c-175">a.</span><span class="sxs-lookup"><span data-stu-id="94b1c-175">a.</span></span> <span data-ttu-id="94b1c-176">Välj **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-176">Select **Enable Single Sign-on**.</span></span>

   <span data-ttu-id="94b1c-177">b.</span><span class="sxs-lookup"><span data-stu-id="94b1c-177">b.</span></span> <span data-ttu-id="94b1c-178">Som **standard rolltilldelning**väljer **readonly**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-178">As **Default Role Assignment**, select **readonly**.</span></span>
   
   <span data-ttu-id="94b1c-179">c.</span><span class="sxs-lookup"><span data-stu-id="94b1c-179">c.</span></span> <span data-ttu-id="94b1c-180">Öppna metadatafilen hämtade i anteckningar och klistra in innehållet i filen till den **identitet providern Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="94b1c-180">Open the downloaded metadata file in notepad, and then paste content of the file into the **Identity Provider Metadata** textbox.</span></span>
   
   <span data-ttu-id="94b1c-181">d.</span><span class="sxs-lookup"><span data-stu-id="94b1c-181">d.</span></span> <span data-ttu-id="94b1c-182">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-182">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="94b1c-183">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="94b1c-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="94b1c-184">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="94b1c-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="94b1c-185">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="94b1c-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="94b1c-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="94b1c-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="94b1c-187">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="94b1c-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="94b1c-189">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="94b1c-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="94b1c-190">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="94b1c-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="94b1c-192">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="94b1c-194">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="94b1c-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="94b1c-196">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="94b1c-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="94b1c-198">a.</span><span class="sxs-lookup"><span data-stu-id="94b1c-198">a.</span></span> <span data-ttu-id="94b1c-199">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="94b1c-200">b.</span><span class="sxs-lookup"><span data-stu-id="94b1c-200">b.</span></span> <span data-ttu-id="94b1c-201">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="94b1c-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="94b1c-202">c.</span><span class="sxs-lookup"><span data-stu-id="94b1c-202">c.</span></span> <span data-ttu-id="94b1c-203">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="94b1c-204">d.</span><span class="sxs-lookup"><span data-stu-id="94b1c-204">d.</span></span> <span data-ttu-id="94b1c-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-205">Click **Create**.</span></span>
 
### <a name="creating-a-logicmonitor-test-user"></a><span data-ttu-id="94b1c-206">Skapa en testanvändare LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="94b1c-206">Creating a LogicMonitor test user</span></span>

<span data-ttu-id="94b1c-207">För AAD-användare för att kunna logga in, måste de etableras till LogicMonitor programmet med hjälp av deras användarnamn för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="94b1c-207">For AAD users to be able to sign in, they must be provisioned to the LogicMonitor application using their Azure Active Directory user names.</span></span>

<span data-ttu-id="94b1c-208">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="94b1c-208">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="94b1c-209">Logga in på webbplatsen LogicMonitor företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="94b1c-209">Log in to your LogicMonitor company site as an administrator.</span></span>

2. <span data-ttu-id="94b1c-210">Klicka på menyn högst upp **inställningar**, och klicka sedan på **roller och användare**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-210">In the menu on the top, click **Settings**, and then click **Roles and Users**.</span></span>
   
   <span data-ttu-id="94b1c-211">![Roller och användare](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "roller och användare")</span><span class="sxs-lookup"><span data-stu-id="94b1c-211">![Roles and Users](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "Roles and Users")</span></span>

3. <span data-ttu-id="94b1c-212">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-212">Click **Add**.</span></span>

4. <span data-ttu-id="94b1c-213">I den **Lägg till ett konto** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="94b1c-213">In the **Add an account** section, perform the following steps:</span></span>
   
   <span data-ttu-id="94b1c-214">![Lägg till ett konto](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "Lägg till ett konto")</span><span class="sxs-lookup"><span data-stu-id="94b1c-214">![Add an account](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "Add an account")</span></span>
   
   <span data-ttu-id="94b1c-215">a.</span><span class="sxs-lookup"><span data-stu-id="94b1c-215">a.</span></span> <span data-ttu-id="94b1c-216">Typ av **användarnamn**, **e-post**, **lösenord**, och **ange lösenord** värden för Azure Active Directory-användare som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="94b1c-216">Type the **Username**, **Email**, **Password**, and **Retype password** values of the Azure Active Directory user you want to provision into the related textboxes.</span></span>
   
   <span data-ttu-id="94b1c-217">b.</span><span class="sxs-lookup"><span data-stu-id="94b1c-217">b.</span></span> <span data-ttu-id="94b1c-218">Välj **roller**, **behörighet att visa**, och **Status**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-218">Select **Roles**, **View Permissions**, and the **Status**.</span></span>
   
   <span data-ttu-id="94b1c-219">c.</span><span class="sxs-lookup"><span data-stu-id="94b1c-219">c.</span></span> <span data-ttu-id="94b1c-220">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-220">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="94b1c-221">Du kan använda något annat LogicMonitor användarens konto skapas verktyg eller API: er som tillhandahålls av LogicMonitor för att etablera Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="94b1c-221">You can use any other LogicMonitor user account creation tools or APIs provided by LogicMonitor to provision Azure Active Directory user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="94b1c-222">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="94b1c-222">Assigning the Azure AD test user</span></span>

<span data-ttu-id="94b1c-223">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till LogicMonitor.</span><span class="sxs-lookup"><span data-stu-id="94b1c-223">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LogicMonitor.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="94b1c-225">**Om du vill tilldela LogicMonitor Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="94b1c-225">**To assign Britta Simon to LogicMonitor, perform the following steps:**</span></span>

1. <span data-ttu-id="94b1c-226">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-226">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="94b1c-228">Välj i listan med program **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-228">In the applications list, select **LogicMonitor**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_app.png) 

3. <span data-ttu-id="94b1c-230">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="94b1c-230">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="94b1c-232">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="94b1c-232">Click **Add** button.</span></span> <span data-ttu-id="94b1c-233">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="94b1c-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="94b1c-235">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="94b1c-235">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="94b1c-236">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="94b1c-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="94b1c-237">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="94b1c-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="94b1c-238">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="94b1c-238">Testing single sign-on</span></span>

<span data-ttu-id="94b1c-239">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="94b1c-239">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
 
<span data-ttu-id="94b1c-240">När du klickar på panelen LogicMonitor på åtkomstpanelen du bör få automatiskt loggat in på ditt LogicMonitor program.</span><span class="sxs-lookup"><span data-stu-id="94b1c-240">When you click the LogicMonitor tile in the Access Panel, you should get automatically signed-on to your LogicMonitor application.</span></span>
<span data-ttu-id="94b1c-241">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="94b1c-241">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="94b1c-242">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="94b1c-242">Additional resources</span></span>

* [<span data-ttu-id="94b1c-243">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="94b1c-243">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="94b1c-244">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="94b1c-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_203.png

