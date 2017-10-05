---
title: "Självstudier: Azure Active Directory-integrering med RFPIO | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och RFPIO."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 26a8bb17dad5a01b401ce7f9b484f09822825cbf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a><span data-ttu-id="4d163-103">Självstudier: Azure Active Directory-integrering med RFPIO</span><span class="sxs-lookup"><span data-stu-id="4d163-103">Tutorial: Azure Active Directory integration with RFPIO</span></span>

<span data-ttu-id="4d163-104">I kursen får lära du att integrera RFPIO med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4d163-104">In this tutorial, you learn how to integrate RFPIO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4d163-105">Integrera RFPIO med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4d163-105">Integrating RFPIO with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4d163-106">Du kan styra vem i Azure AD som har åtkomst till RFPIO.</span><span class="sxs-lookup"><span data-stu-id="4d163-106">You can control who in Azure AD who has access to RFPIO.</span></span>
- <span data-ttu-id="4d163-107">Du kan aktivera användarna att automatiskt hämta loggat in på RFPIO (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="4d163-107">You can enable your users to automatically get signed-on to RFPIO (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="4d163-108">Du kan hantera dina konton i en central plats--Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4d163-108">You can manage your accounts in one central location--the Azure portal.</span></span>

<span data-ttu-id="4d163-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4d163-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4d163-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4d163-110">Prerequisites</span></span>

<span data-ttu-id="4d163-111">För att konfigurera Azure AD-integrering med RFPIO, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="4d163-111">To configure Azure AD integration with RFPIO, you need the following items:</span></span>

- <span data-ttu-id="4d163-112">En Azure AD-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4d163-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="4d163-113">En RFPIO enkel inloggning på aktiverat prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4d163-113">A RFPIO single sign-on-enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="4d163-114">Vi rekommenderar inte att du använder en produktionsmiljö för att testa stegen i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="4d163-114">We don't recommend that you use a production environment to test the steps in this tutorial.</span></span>

<span data-ttu-id="4d163-115">Följ dessa rekommendationer för att testa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="4d163-115">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="4d163-116">Använd inte din produktionsmiljö om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4d163-116">Do not use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="4d163-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4d163-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4d163-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4d163-118">Scenario description</span></span>
<span data-ttu-id="4d163-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4d163-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4d163-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4d163-120">The scenario that's outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4d163-121">Lägger till RFPIO från galleriet.</span><span class="sxs-lookup"><span data-stu-id="4d163-121">Adding RFPIO from the gallery.</span></span>
2. <span data-ttu-id="4d163-122">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4d163-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-rfpio-from-the-gallery"></a><span data-ttu-id="4d163-123">Lägg till RFPIO från galleriet</span><span class="sxs-lookup"><span data-stu-id="4d163-123">Add RFPIO from the gallery</span></span>
<span data-ttu-id="4d163-124">Du måste lägga till RFPIO från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av RFPIO i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4d163-124">To configure the integration of RFPIO into Azure AD, you need to add RFPIO from the gallery to your list of managed SaaS apps.</span></span>

### <a name="to-add-rfpio-from-the-gallery"></a><span data-ttu-id="4d163-125">Lägga till RFPIO från galleriet</span><span class="sxs-lookup"><span data-stu-id="4d163-125">To add RFPIO from the gallery</span></span>

1. <span data-ttu-id="4d163-126">I den  **[Azure-portalen](https://portal.azure.com)**, på det vänstra navigeringsfönstret väljer du den **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4d163-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation pane, select the **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4d163-128">Välj **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4d163-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4d163-130">Om du vill lägga till ett nytt program, Välj den **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4d163-130">To add a new application, select the **New application** button on the top of dialog box.</span></span>

    ![Program][3]

4. <span data-ttu-id="4d163-132">I sökrutan skriver **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="4d163-132">In the search box, type **RFPIO**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_search.png)

5. <span data-ttu-id="4d163-134">Välj i resultatpanelen **RFPIO**, och välj sedan den **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="4d163-134">In the results panel, select **RFPIO**, and then select the **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="4d163-136">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4d163-136">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="4d163-137">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med RFPIO baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4d163-137">In this section, you configure and test Azure AD single sign-on with RFPIO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4d163-138">Azure AD måste du känna till relationen mellan motsvarighet användare i RFPIO och en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="4d163-138">For single sign-on to work, Azure AD needs to know what the relationship is between counterpart user in RFPIO and a user in Azure AD.</span></span> <span data-ttu-id="4d163-139">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i RFPIO upprättas.</span><span class="sxs-lookup"><span data-stu-id="4d163-139">In other words, a link relationship between an Azure AD user and the related user in RFPIO needs to be established.</span></span>

<span data-ttu-id="4d163-140">I RFPIO, tilldela värdet för **användarnamn** i Azure AD som värde för **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4d163-140">In RFPIO, assign the value of **user name** in Azure AD as the value of  **Username** to establish the link relationship.</span></span>

<span data-ttu-id="4d163-141">Om du vill konfigurera och testa Azure AD enkel inloggning med RFPIO, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4d163-141">To configure and test Azure AD single sign-on with RFPIO, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4d163-142">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**--att användarna ska kunna använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4d163-142">**[Configure Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**--to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4d163-143">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**--att testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4d163-143">**[Create an Azure AD test user](#creating-an-azure-ad-test-user)**-- to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4d163-144">**[Skapa en testanvändare RFPIO](#creating-a-rfpio-test-user)**  --har en motsvarighet för Britta Simon RFPIO som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4d163-144">**[Create a RFPIO test user](#creating-a-rfpio-test-user)** --to have a counterpart of Britta Simon in RFPIO that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4d163-145">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**--att aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4d163-145">**[Assign the Azure AD test user](#assigning-the-azure-ad-test-user)**--to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4d163-146">**[Testa enkel inloggning](#testing-single-sign-on)**  --att kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4d163-146">**[Test Single Sign-On](#testing-single-sign-on)** --to verify if the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="4d163-147">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4d163-147">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="4d163-148">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt RFPIO program.</span><span class="sxs-lookup"><span data-stu-id="4d163-148">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your RFPIO application.</span></span>

<span data-ttu-id="4d163-149">**Utför följande steg för att konfigurera Azure AD enkel inloggning med RFPIO:**</span><span class="sxs-lookup"><span data-stu-id="4d163-149">**To configure Azure AD single sign-on with RFPIO, perform the following steps:**</span></span>

1. <span data-ttu-id="4d163-150">I Azure-portalen på den **RFPIO** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4d163-150">In the Azure portal, on the **RFPIO** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4d163-152">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4d163-152">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. <span data-ttu-id="4d163-154">På den **RFPIO domän och URL: er** om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="4d163-154">On the **RFPIO Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url.png)

    <span data-ttu-id="4d163-156">a.</span><span class="sxs-lookup"><span data-stu-id="4d163-156">a.</span></span> <span data-ttu-id="4d163-157">I den **identifierare** textruta anger du URL:`https://www.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="4d163-157">In the **Identifier** textbox, type the URL: `https://www.rfpio.com`</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url1.png)

    <span data-ttu-id="4d163-159">b.</span><span class="sxs-lookup"><span data-stu-id="4d163-159">b.</span></span> <span data-ttu-id="4d163-160">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="4d163-160">Check **Show advanced URL settings**.</span></span>

    <span data-ttu-id="4d163-161">c.</span><span class="sxs-lookup"><span data-stu-id="4d163-161">c.</span></span> <span data-ttu-id="4d163-162">I den **Relay tillstånd** textrutan anger du ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="4d163-162">In the **Relay State** textbox enter a string value.</span></span> <span data-ttu-id="4d163-163">Kontakta [RFPIO supportteam](https://www.rfpio.com/contact/) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="4d163-163">Contact [RFPIO support team](https://www.rfpio.com/contact/) to get this value.</span></span> 

4. <span data-ttu-id="4d163-164">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="4d163-164">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="4d163-165">Om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="4d163-165">If you wish to configure the application in **SP** initiated mode:</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url2.png)

    <span data-ttu-id="4d163-167">I den **inloggning URL** textruta anger du URL:`https://www.app.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="4d163-167">In the **Sign on URL** textbox, type the URL: `https://www.app.rfpio.com`</span></span>

5. <span data-ttu-id="4d163-168">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="4d163-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. <span data-ttu-id="4d163-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4d163-170">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="4d163-172">I en annan webbläsarfönstret, logga in på den **RFPIO** webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="4d163-172">In a different web browser window, login to the **RFPIO** website as an administrator.</span></span>

8. <span data-ttu-id="4d163-173">Klicka på listrutan längst ned till vänster.</span><span class="sxs-lookup"><span data-stu-id="4d163-173">Click on the bottom left corner dropdown.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app1.png)

9. <span data-ttu-id="4d163-175">Klicka på den **organisationsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="4d163-175">Click on the **Organization Settings**.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app2.png)

10. <span data-ttu-id="4d163-177">Klicka på den **funktioner & INTEGRATION**.</span><span class="sxs-lookup"><span data-stu-id="4d163-177">Click on the **FEATURES & INTEGRATION**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app4.png)

11. <span data-ttu-id="4d163-179">I den **SAML SSO Configuration** klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="4d163-179">In the **SAML SSO Configuration** Click **Edit**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app3.png)

12. <span data-ttu-id="4d163-181">Utför följande åtgärder i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="4d163-181">In this Section perform following actions:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app5.png)
    
    <span data-ttu-id="4d163-183">a.</span><span class="sxs-lookup"><span data-stu-id="4d163-183">a.</span></span> <span data-ttu-id="4d163-184">Kopiera innehållet i den **hämtas XML-Metadata för** och klistrar in det i den **identitet configuration** fältet.</span><span class="sxs-lookup"><span data-stu-id="4d163-184">Copy the content of the **Downloaded Metadata XML** and paste it into the **identity configuration** field.</span></span>

    > [!NOTE]
    ><span data-ttu-id="4d163-185">Att kopiera innehållet i nedladdade **XML-Metadata för** Använd **anteckningar ++** eller rätt **XML-redigerare**.</span><span class="sxs-lookup"><span data-stu-id="4d163-185">To copy the content of downloaded **Metadata XML** Use **Notepad++** or proper **XML Editor**.</span></span> 

    <span data-ttu-id="4d163-186">b.</span><span class="sxs-lookup"><span data-stu-id="4d163-186">b.</span></span> <span data-ttu-id="4d163-187">Klicka på **Validera**.</span><span class="sxs-lookup"><span data-stu-id="4d163-187">Click **Validate**.</span></span>

    <span data-ttu-id="4d163-188">c.</span><span class="sxs-lookup"><span data-stu-id="4d163-188">c.</span></span> <span data-ttu-id="4d163-189">När du klickar på **Validera**, vänd **SAML(Enabled)** till on.</span><span class="sxs-lookup"><span data-stu-id="4d163-189">After Clicking **Validate**, Flip **SAML(Enabled)** to on.</span></span>

    <span data-ttu-id="4d163-190">d.</span><span class="sxs-lookup"><span data-stu-id="4d163-190">d.</span></span> <span data-ttu-id="4d163-191">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="4d163-191">Click **Submit**.</span></span>

> [!TIP]
> <span data-ttu-id="4d163-192">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="4d163-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4d163-193">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="4d163-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4d163-194">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4d163-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="4d163-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4d163-195">Create an Azure AD test user</span></span>
<span data-ttu-id="4d163-196">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4d163-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4d163-198">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="4d163-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4d163-199">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4d163-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4d163-201">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4d163-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4d163-203">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4d163-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4d163-205">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4d163-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4d163-207">a.</span><span class="sxs-lookup"><span data-stu-id="4d163-207">a.</span></span> <span data-ttu-id="4d163-208">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4d163-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4d163-209">b.</span><span class="sxs-lookup"><span data-stu-id="4d163-209">b.</span></span> <span data-ttu-id="4d163-210">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4d163-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4d163-211">c.</span><span class="sxs-lookup"><span data-stu-id="4d163-211">c.</span></span> <span data-ttu-id="4d163-212">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4d163-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4d163-213">d.</span><span class="sxs-lookup"><span data-stu-id="4d163-213">d.</span></span> <span data-ttu-id="4d163-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4d163-214">Click **Create**.</span></span>
 
### <a name="create-a-rfpio-test-user"></a><span data-ttu-id="4d163-215">Skapa en testanvändare RFPIO</span><span class="sxs-lookup"><span data-stu-id="4d163-215">Create a RFPIO test user</span></span>

<span data-ttu-id="4d163-216">Om du vill aktivera Azure AD-användare kan logga in på RFPIO etableras de i RFPIO.</span><span class="sxs-lookup"><span data-stu-id="4d163-216">To enable Azure AD users to log in to RFPIO, they must be provisioned into RFPIO.</span></span>  
<span data-ttu-id="4d163-217">När det gäller RFPIO är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="4d163-217">In the case of RFPIO, provisioning is a manual task.</span></span>

<span data-ttu-id="4d163-218">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="4d163-218">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="4d163-219">Logga in på webbplatsen RFPIO företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="4d163-219">Log in to your RFPIO company site as an administrator.</span></span>

2. <span data-ttu-id="4d163-220">Klicka på listrutan längst ned till vänster.</span><span class="sxs-lookup"><span data-stu-id="4d163-220">Click on the bottom left corner dropdown.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app1.png)

3. <span data-ttu-id="4d163-222">Klicka på den **organisationsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="4d163-222">Click on the **Organization Settings**.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app2.png)

4. <span data-ttu-id="4d163-224">Klicka på **GRUPPMEDLEMMAR**.</span><span class="sxs-lookup"><span data-stu-id="4d163-224">Click **TEAM MEMBERS**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app6.png)

5. <span data-ttu-id="4d163-226">Klicka på **Lägg till MEDLEMMAR**.</span><span class="sxs-lookup"><span data-stu-id="4d163-226">Click on **ADD MEMBERS**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app7.png)

6. <span data-ttu-id="4d163-228">I den **lägga till nya medlemmar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4d163-228">In the **Add New Members** section.</span></span> <span data-ttu-id="4d163-229">Utför följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="4d163-229">Perform following actions:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app8.png)

    <span data-ttu-id="4d163-231">a.</span><span class="sxs-lookup"><span data-stu-id="4d163-231">a.</span></span> <span data-ttu-id="4d163-232">Ange **e-postadress** i den **ange en e-postadress per rad** fältet.</span><span class="sxs-lookup"><span data-stu-id="4d163-232">Enter **Email address** in the **Enter one email per line** field.</span></span>

    <span data-ttu-id="4d163-233">b.</span><span class="sxs-lookup"><span data-stu-id="4d163-233">b.</span></span> <span data-ttu-id="4d163-234">Välj Plese **rollen** enligt dina krav.</span><span class="sxs-lookup"><span data-stu-id="4d163-234">Plese select **Role** according your requirements.</span></span>

    <span data-ttu-id="4d163-235">c.</span><span class="sxs-lookup"><span data-stu-id="4d163-235">c.</span></span> <span data-ttu-id="4d163-236">Klicka på **Lägg till MEDLEMMAR**.</span><span class="sxs-lookup"><span data-stu-id="4d163-236">Click **ADD MEMBERS**.</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="4d163-237">Azure Active Directory kontoinnehavaren får ett e-postmeddelande och följer en länk för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="4d163-237">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="4d163-238">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="4d163-238">Assign the Azure AD test user</span></span>

<span data-ttu-id="4d163-239">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till RFPIO.</span><span class="sxs-lookup"><span data-stu-id="4d163-239">In this section, you enable Britta Simon to use Azure single sign-on by granting access to RFPIO.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4d163-241">**Om du vill tilldela RFPIO Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4d163-241">**To assign Britta Simon to RFPIO, perform the following steps:**</span></span>

1. <span data-ttu-id="4d163-242">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4d163-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4d163-244">Välj i listan med program **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="4d163-244">In the applications list, select **RFPIO**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_app.png) 

3. <span data-ttu-id="4d163-246">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4d163-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4d163-248">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4d163-248">Click **Add** button.</span></span> <span data-ttu-id="4d163-249">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4d163-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4d163-251">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="4d163-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4d163-252">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4d163-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4d163-253">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4d163-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="4d163-254">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4d163-254">Test single sign-on</span></span>

<span data-ttu-id="4d163-255">I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4d163-255">In this section, you test your Azure AD single sign-on configuration by using the Access Panel.</span></span>

<span data-ttu-id="4d163-256">När du klickar på panelen RFPIO på åtkomstpanelen du bör få automatiskt loggat in på ditt RFPIO program.</span><span class="sxs-lookup"><span data-stu-id="4d163-256">When you click the RFPIO tile in the Access Panel, you should get automatically signed-on to your RFPIO application.</span></span>
<span data-ttu-id="4d163-257">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4d163-257">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4d163-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4d163-258">Additional resources</span></span>

* [<span data-ttu-id="4d163-259">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4d163-259">List of tutorials about how to integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4d163-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4d163-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_203.png

