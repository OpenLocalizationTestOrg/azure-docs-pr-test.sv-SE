---
title: "Självstudier: Azure Active Directory-integrering med MaxxPoint | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och MaxxPoint."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ba026e-96fc-4ae8-b135-0169da810e99
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: 8a7481b71df5ca407dbed5da3d3cc26991504c82
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-maxxpoint"></a><span data-ttu-id="71b30-103">Självstudier: Azure Active Directory-integrering med MaxxPoint</span><span class="sxs-lookup"><span data-stu-id="71b30-103">Tutorial: Azure Active Directory integration with MaxxPoint</span></span>

<span data-ttu-id="71b30-104">I kursen får lära du att integrera MaxxPoint med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="71b30-104">In this tutorial, you learn how to integrate MaxxPoint with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="71b30-105">Integrera MaxxPoint med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="71b30-105">Integrating MaxxPoint with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="71b30-106">Du kan styra i Azure AD som har åtkomst till MaxxPoint</span><span class="sxs-lookup"><span data-stu-id="71b30-106">You can control in Azure AD who has access to MaxxPoint</span></span>
- <span data-ttu-id="71b30-107">Du kan aktivera användarna att automatiskt hämta loggat in på MaxxPoint (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="71b30-107">You can enable your users to automatically get signed-on to MaxxPoint (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="71b30-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="71b30-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="71b30-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="71b30-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71b30-110">Krav</span><span class="sxs-lookup"><span data-stu-id="71b30-110">Prerequisites</span></span>

<span data-ttu-id="71b30-111">För att konfigurera Azure AD-integrering med MaxxPoint, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="71b30-111">To configure Azure AD integration with MaxxPoint, you need the following items:</span></span>

- <span data-ttu-id="71b30-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="71b30-112">An Azure AD subscription</span></span>
- <span data-ttu-id="71b30-113">En MaxxPoint enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="71b30-113">A MaxxPoint single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="71b30-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="71b30-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="71b30-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="71b30-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="71b30-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="71b30-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="71b30-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="71b30-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="71b30-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="71b30-118">Scenario description</span></span>
<span data-ttu-id="71b30-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="71b30-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="71b30-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="71b30-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="71b30-121">Att lägga till MaxxPoint från galleriet</span><span class="sxs-lookup"><span data-stu-id="71b30-121">Adding MaxxPoint from the gallery</span></span>
2. <span data-ttu-id="71b30-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="71b30-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-maxxpoint-from-the-gallery"></a><span data-ttu-id="71b30-123">Att lägga till MaxxPoint från galleriet</span><span class="sxs-lookup"><span data-stu-id="71b30-123">Adding MaxxPoint from the gallery</span></span>
<span data-ttu-id="71b30-124">Du måste lägga till MaxxPoint från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av MaxxPoint i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="71b30-124">To configure the integration of MaxxPoint into Azure AD, you need to add MaxxPoint from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="71b30-125">**Utför följande steg för att lägga till MaxxPoint från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="71b30-125">**To add MaxxPoint from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="71b30-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="71b30-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="71b30-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="71b30-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="71b30-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="71b30-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="71b30-131">Klicka på **nytt program** knappen överst för att lägga till nya program.</span><span class="sxs-lookup"><span data-stu-id="71b30-131">Click **New application** button on the top of dialog to add new application.</span></span>

    ![Program][3]

4. <span data-ttu-id="71b30-133">I sökrutan skriver **MaxxPoint**.</span><span class="sxs-lookup"><span data-stu-id="71b30-133">In the search box, type **MaxxPoint**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_001.png)

5. <span data-ttu-id="71b30-135">Välj i resultatpanelen **MaxxPoint**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="71b30-135">In the results panel, select **MaxxPoint**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="71b30-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="71b30-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="71b30-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med MaxxPoint baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="71b30-138">In this section, you configure and test Azure AD single sign-on with MaxxPoint based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="71b30-139">Azure AD måste du känna till användaren i MaxxPoint motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="71b30-139">For single sign-on to work, Azure AD needs to know what the counterpart user in MaxxPoint is to a user in Azure AD.</span></span> <span data-ttu-id="71b30-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i MaxxPoint upprättas.</span><span class="sxs-lookup"><span data-stu-id="71b30-140">In other words, a link relationship between an Azure AD user and the related user in MaxxPoint needs to be established.</span></span>

<span data-ttu-id="71b30-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i MaxxPoint.</span><span class="sxs-lookup"><span data-stu-id="71b30-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in MaxxPoint.</span></span>

<span data-ttu-id="71b30-142">Om du vill konfigurera och testa Azure AD enkel inloggning med MaxxPoint, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="71b30-142">To configure and test Azure AD single sign-on with MaxxPoint, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="71b30-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="71b30-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="71b30-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="71b30-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="71b30-145">**[Skapa en testanvändare MaxxPoint](#creating-a-maxxpoint-test-user)**  – du har en motsvarighet för Britta Simon i MaxxPoint som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="71b30-145">**[Creating a MaxxPoint test user](#creating-a-maxxpoint-test-user)** - to have a counterpart of Britta Simon in MaxxPoint that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="71b30-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="71b30-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="71b30-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="71b30-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="71b30-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="71b30-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="71b30-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt MaxxPoint program.</span><span class="sxs-lookup"><span data-stu-id="71b30-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your MaxxPoint application.</span></span>

<span data-ttu-id="71b30-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med MaxxPoint:**</span><span class="sxs-lookup"><span data-stu-id="71b30-150">**To configure Azure AD single sign-on with MaxxPoint, perform the following steps:**</span></span>

1. <span data-ttu-id="71b30-151">I Azure-portalen på den **MaxxPoint** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="71b30-151">In the Azure portal, on the **MaxxPoint** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="71b30-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="71b30-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_300.png)

3. <span data-ttu-id="71b30-155">På den **MaxxPoint domän och URL: er** om du vill konfigurera programmet i **IDP initierade läge**, behöver du inte utföra några steg.</span><span class="sxs-lookup"><span data-stu-id="71b30-155">On the **MaxxPoint Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**, no need to perform any steps.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_02.png)
    
4. <span data-ttu-id="71b30-157">På den **MaxxPoint domän och URL: er** om du vill konfigurera programmet i **SP initierade läge**, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="71b30-157">On the **MaxxPoint Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_03.png)

    <span data-ttu-id="71b30-159">a.</span><span class="sxs-lookup"><span data-stu-id="71b30-159">a.</span></span> <span data-ttu-id="71b30-160">Klicka på **visa avancerade inställningar för URL: en** alternativet</span><span class="sxs-lookup"><span data-stu-id="71b30-160">Click **Show advanced URL settings** option</span></span>

    <span data-ttu-id="71b30-161">b.</span><span class="sxs-lookup"><span data-stu-id="71b30-161">b.</span></span> <span data-ttu-id="71b30-162">I den **logga URL** textruta Skriv en URL med följande mönster:`https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`</span><span class="sxs-lookup"><span data-stu-id="71b30-162">In the **Sign On URL** textbox, type a URL using the following pattern: `https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="71b30-163">Observera att detta inte är det verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="71b30-163">Please note that this is not the real value.</span></span> <span data-ttu-id="71b30-164">Du måste uppdatera det här värdet med faktiska logga på URL: en.</span><span class="sxs-lookup"><span data-stu-id="71b30-164">You have to update this value with the actual Sign On URL.</span></span> <span data-ttu-id="71b30-165">Anropa MaxxPoint team i **888-728-0950** att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="71b30-165">Call MaxxPoint team on **888-728-0950** to get this value.</span></span>

5. <span data-ttu-id="71b30-166">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="71b30-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_06.png) 

6. <span data-ttu-id="71b30-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="71b30-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="71b30-170">För att få SSO konfigurerats för ditt program kan anropa MaxxPoint supportteamet **888-728-0950** och de kommer hjälpa dig mer om hur du ger dem den hämtade **XML-Metadata för** fil.</span><span class="sxs-lookup"><span data-stu-id="71b30-170">To get SSO configured for your application, call MaxxPoint support team on **888-728-0950** and they'll assist you further on how to provide them the downloaded **Metadata XML** file.</span></span> 

> [!TIP]
> <span data-ttu-id="71b30-171">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="71b30-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="71b30-172">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="71b30-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="71b30-173">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="71b30-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="71b30-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="71b30-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="71b30-175">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="71b30-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="71b30-177">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="71b30-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="71b30-178">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="71b30-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="71b30-180">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="71b30-180">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="71b30-182">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="71b30-182">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="71b30-184">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="71b30-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="71b30-186">a.</span><span class="sxs-lookup"><span data-stu-id="71b30-186">a.</span></span> <span data-ttu-id="71b30-187">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="71b30-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="71b30-188">b.</span><span class="sxs-lookup"><span data-stu-id="71b30-188">b.</span></span> <span data-ttu-id="71b30-189">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="71b30-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="71b30-190">c.</span><span class="sxs-lookup"><span data-stu-id="71b30-190">c.</span></span> <span data-ttu-id="71b30-191">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="71b30-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="71b30-192">d.</span><span class="sxs-lookup"><span data-stu-id="71b30-192">d.</span></span> <span data-ttu-id="71b30-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="71b30-193">Click **Create**.</span></span> 

### <a name="creating-a-maxxpoint-test-user"></a><span data-ttu-id="71b30-194">Skapa en testanvändare MaxxPoint</span><span class="sxs-lookup"><span data-stu-id="71b30-194">Creating a MaxxPoint test user</span></span>

<span data-ttu-id="71b30-195">I det här avsnittet skapar du en användare som kallas Britta Simon i MaxxPoint.</span><span class="sxs-lookup"><span data-stu-id="71b30-195">In this section, you create a user called Britta Simon in MaxxPoint.</span></span> <span data-ttu-id="71b30-196">Kontakta supportteamet för MaxxPoint på **888-728-0950** att lägga till användare i MaxxPoint-programmet.</span><span class="sxs-lookup"><span data-stu-id="71b30-196">Please call MaxxPoint support team on **888-728-0950** to add the users in the MaxxPoint application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="71b30-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="71b30-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="71b30-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till MaxxPoint.</span><span class="sxs-lookup"><span data-stu-id="71b30-198">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to MaxxPoint.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="71b30-200">**Om du vill tilldela MaxxPoint Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="71b30-200">**To assign Britta Simon to MaxxPoint, perform the following steps:**</span></span>

1. <span data-ttu-id="71b30-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="71b30-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="71b30-203">Välj i listan med program **MaxxPoint**.</span><span class="sxs-lookup"><span data-stu-id="71b30-203">In the applications list, select **MaxxPoint**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_50.png) 

3. <span data-ttu-id="71b30-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="71b30-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="71b30-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="71b30-207">Click **Add** button.</span></span> <span data-ttu-id="71b30-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="71b30-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="71b30-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="71b30-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="71b30-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="71b30-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="71b30-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="71b30-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="71b30-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="71b30-213">Testing single sign-on</span></span>

<span data-ttu-id="71b30-214">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="71b30-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="71b30-215">När du klickar på panelen MaxxPoint på åtkomstpanelen du bör få automatiskt loggat in på ditt MaxxPoint program.</span><span class="sxs-lookup"><span data-stu-id="71b30-215">When you click the MaxxPoint tile in the Access Panel, you should get automatically signed-on to your MaxxPoint application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="71b30-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="71b30-216">Additional resources</span></span>

* [<span data-ttu-id="71b30-217">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="71b30-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="71b30-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="71b30-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_203.png