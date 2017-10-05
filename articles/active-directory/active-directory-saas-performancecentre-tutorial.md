---
title: "Självstudier: Azure Active Directory-integrering med PerformanceCentre | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och PerformanceCentre."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65288c32-f7e6-4eb3-a6dc-523c3d748d1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: e86adaf4bd9b4752f2aece8207a8a423ec5590a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a><span data-ttu-id="43a15-103">Självstudier: Azure Active Directory-integrering med PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="43a15-103">Tutorial: Azure Active Directory integration with PerformanceCentre</span></span>

<span data-ttu-id="43a15-104">I kursen får lära du att integrera PerformanceCentre med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="43a15-104">In this tutorial, you learn how to integrate PerformanceCentre with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="43a15-105">Integrera PerformanceCentre med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="43a15-105">Integrating PerformanceCentre with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="43a15-106">Du kan styra i Azure AD som har åtkomst till PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="43a15-106">You can control in Azure AD who has access to PerformanceCentre</span></span>
- <span data-ttu-id="43a15-107">Du kan aktivera användarna att automatiskt hämta loggat in på PerformanceCentre (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="43a15-107">You can enable your users to automatically get signed-on to PerformanceCentre (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="43a15-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="43a15-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="43a15-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="43a15-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43a15-110">Krav</span><span class="sxs-lookup"><span data-stu-id="43a15-110">Prerequisites</span></span>

<span data-ttu-id="43a15-111">För att konfigurera Azure AD-integrering med PerformanceCentre, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="43a15-111">To configure Azure AD integration with PerformanceCentre, you need the following items:</span></span>

- <span data-ttu-id="43a15-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="43a15-112">An Azure AD subscription</span></span>
- <span data-ttu-id="43a15-113">En PerformanceCentre enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="43a15-113">A PerformanceCentre single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="43a15-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="43a15-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="43a15-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="43a15-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="43a15-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="43a15-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="43a15-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="43a15-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="43a15-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="43a15-118">Scenario description</span></span>
<span data-ttu-id="43a15-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="43a15-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="43a15-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="43a15-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="43a15-121">Att lägga till PerformanceCentre från galleriet</span><span class="sxs-lookup"><span data-stu-id="43a15-121">Adding PerformanceCentre from the gallery</span></span>
2. <span data-ttu-id="43a15-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43a15-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-performancecentre-from-the-gallery"></a><span data-ttu-id="43a15-123">Att lägga till PerformanceCentre från galleriet</span><span class="sxs-lookup"><span data-stu-id="43a15-123">Adding PerformanceCentre from the gallery</span></span>
<span data-ttu-id="43a15-124">Du måste lägga till PerformanceCentre från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av PerformanceCentre i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="43a15-124">To configure the integration of PerformanceCentre into Azure AD, you need to add PerformanceCentre from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="43a15-125">**Utför följande steg för att lägga till PerformanceCentre från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="43a15-125">**To add PerformanceCentre from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="43a15-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="43a15-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="43a15-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="43a15-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="43a15-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="43a15-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="43a15-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43a15-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="43a15-133">I sökrutan skriver **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="43a15-133">In the search box, type **PerformanceCentre**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_search.png)

5. <span data-ttu-id="43a15-135">Välj i resultatpanelen **PerformanceCentre**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="43a15-135">In the results panel, select **PerformanceCentre**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="43a15-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43a15-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="43a15-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PerformanceCentre baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="43a15-138">In this section, you configure and test Azure AD single sign-on with PerformanceCentre based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="43a15-139">Azure AD måste du känna till användaren i PerformanceCentre motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="43a15-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PerformanceCentre is to a user in Azure AD.</span></span> <span data-ttu-id="43a15-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i PerformanceCentre upprättas.</span><span class="sxs-lookup"><span data-stu-id="43a15-140">In other words, a link relationship between an Azure AD user and the related user in PerformanceCentre needs to be established.</span></span>

<span data-ttu-id="43a15-141">I PerformanceCentre, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="43a15-141">In PerformanceCentre, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="43a15-142">Om du vill konfigurera och testa Azure AD enkel inloggning med PerformanceCentre, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="43a15-142">To configure and test Azure AD single sign-on with PerformanceCentre, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="43a15-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="43a15-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="43a15-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="43a15-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="43a15-145">**[Skapa en testanvändare PerformanceCentre](#creating-a-performancecentre-test-user)**  – du har en motsvarighet för Britta Simon i PerformanceCentre som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="43a15-145">**[Creating a PerformanceCentre test user](#creating-a-performancecentre-test-user)** - to have a counterpart of Britta Simon in PerformanceCentre that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="43a15-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="43a15-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="43a15-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="43a15-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="43a15-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43a15-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="43a15-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt PerformanceCentre program.</span><span class="sxs-lookup"><span data-stu-id="43a15-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PerformanceCentre application.</span></span>

<span data-ttu-id="43a15-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med PerformanceCentre:**</span><span class="sxs-lookup"><span data-stu-id="43a15-150">**To configure Azure AD single sign-on with PerformanceCentre, perform the following steps:**</span></span>

1. <span data-ttu-id="43a15-151">I Azure-portalen på den **PerformanceCentre** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="43a15-151">In the Azure portal, on the **PerformanceCentre** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="43a15-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="43a15-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_samlbase.png)

3. <span data-ttu-id="43a15-155">På den **PerformanceCentre domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="43a15-155">On the **PerformanceCentre Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_url.png)

    <span data-ttu-id="43a15-157">a.</span><span class="sxs-lookup"><span data-stu-id="43a15-157">a.</span></span> <span data-ttu-id="43a15-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`http://companyname.performancecentre.com/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="43a15-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://companyname.performancecentre.com/saml/SSO`</span></span>

    <span data-ttu-id="43a15-159">b.</span><span class="sxs-lookup"><span data-stu-id="43a15-159">b.</span></span> <span data-ttu-id="43a15-160">I den **identifierare** textruta Skriv en URL med följande mönster:`http://companyname.performancecentre.com`</span><span class="sxs-lookup"><span data-stu-id="43a15-160">In the **Identifier** textbox, type a URL using the following pattern: `http://companyname.performancecentre.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="43a15-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="43a15-161">These values are not real.</span></span> <span data-ttu-id="43a15-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="43a15-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="43a15-163">Kontakta [PerformanceCentre klienten supportteamet](https://www.performancecentre.com/contact-us/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="43a15-163">Contact [PerformanceCentre Client support team](https://www.performancecentre.com/contact-us/) to get these values.</span></span> 

4. <span data-ttu-id="43a15-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="43a15-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_certificate.png) 

5. <span data-ttu-id="43a15-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="43a15-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="43a15-168">På den **PerformanceCentre Configuration** klickar du på **konfigurera PerformanceCentre** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="43a15-168">On the **PerformanceCentre Configuration** section, click **Configure PerformanceCentre** to open **Configure sign-on** window.</span></span> <span data-ttu-id="43a15-169">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="43a15-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_configure.png) 

7. <span data-ttu-id="43a15-171">Logga in på ditt **PerformanceCentre** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="43a15-171">Sign-on to your **PerformanceCentre** company site as administrator.</span></span>

8. <span data-ttu-id="43a15-172">Klicka på fliken på vänster sida **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="43a15-172">In the tab on the left side, click **Configure**.</span></span>
   
    ![Azure AD-Single Sign-On][10]

9. <span data-ttu-id="43a15-174">Klicka på fliken på vänster sida **diverse**, och klicka sedan på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="43a15-174">In the tab on the left side, click **Miscellaneous**, and then click **Single Sign On**.</span></span>
   
    ![Azure AD-Single Sign-On][11]

10. <span data-ttu-id="43a15-176">Som **protokollet**väljer **SAML**.</span><span class="sxs-lookup"><span data-stu-id="43a15-176">As **Protocol**, select **SAML**.</span></span>
   
    ![Azure AD-Single Sign-On][12]

11. <span data-ttu-id="43a15-178">Öppna hämtade metadatafilen i anteckningar, kopiera innehållet, klistrar in det i den **identitet providern Metadata** textruta och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="43a15-178">Open your downloaded metadata file in notepad, copy the content, paste it into the **Identity Provider Metadata** textbox, and then click **Save**.</span></span>
   
    ![Azure AD-Single Sign-On][13]

12. <span data-ttu-id="43a15-180">Kontrollera att värdena för den **entitet bas-URL** och **entitet ID URL** är korrekta.</span><span class="sxs-lookup"><span data-stu-id="43a15-180">Verify that the values for the **Entity Base URL** and **Entity ID URL** are correct.</span></span>
    
     ![Azure AD-Single Sign-On][14]

> [!TIP]
> <span data-ttu-id="43a15-182">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="43a15-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="43a15-183">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="43a15-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="43a15-184">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="43a15-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="43a15-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="43a15-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="43a15-186">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="43a15-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="43a15-188">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="43a15-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="43a15-189">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="43a15-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="43a15-191">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="43a15-191">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="43a15-193">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43a15-193">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="43a15-195">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="43a15-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="43a15-197">a.</span><span class="sxs-lookup"><span data-stu-id="43a15-197">a.</span></span> <span data-ttu-id="43a15-198">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="43a15-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="43a15-199">b.</span><span class="sxs-lookup"><span data-stu-id="43a15-199">b.</span></span> <span data-ttu-id="43a15-200">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="43a15-200">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="43a15-201">c.</span><span class="sxs-lookup"><span data-stu-id="43a15-201">c.</span></span> <span data-ttu-id="43a15-202">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="43a15-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="43a15-203">d.</span><span class="sxs-lookup"><span data-stu-id="43a15-203">d.</span></span> <span data-ttu-id="43a15-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="43a15-204">Click **Create**.</span></span>
 
### <a name="creating-a-performancecentre-test-user"></a><span data-ttu-id="43a15-205">Skapa en testanvändare PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="43a15-205">Creating a PerformanceCentre test user</span></span>

<span data-ttu-id="43a15-206">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i PerformanceCentre.</span><span class="sxs-lookup"><span data-stu-id="43a15-206">The objective of this section is to create a user called Britta Simon in PerformanceCentre.</span></span>

<span data-ttu-id="43a15-207">**Utför följande steg för att skapa en användare som kallas Britta Simon i PerformanceCentre:**</span><span class="sxs-lookup"><span data-stu-id="43a15-207">**To create a user called Britta Simon in PerformanceCentre, perform the following steps:**</span></span>

1. <span data-ttu-id="43a15-208">Logga in på webbplatsen PerformanceCentre företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="43a15-208">Sign on to your PerformanceCentre company site as administrator.</span></span>

2. <span data-ttu-id="43a15-209">Klicka på menyn till vänster **Interrelate**, och klicka sedan på **skapa deltagare**.</span><span class="sxs-lookup"><span data-stu-id="43a15-209">In the menu on the left, click **Interrelate**, and then click **Create Participant**.</span></span>
   
    ![Skapa användare][400]

3. <span data-ttu-id="43a15-211">På den **samband - skapa deltagare** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="43a15-211">On the **Interrelate - Create Participant** dialog, perform the following steps:</span></span>
   
    ![Skapa användare][401]
    
    <span data-ttu-id="43a15-213">a.</span><span class="sxs-lookup"><span data-stu-id="43a15-213">a.</span></span> <span data-ttu-id="43a15-214">Ange attribut som krävs för Britta Simon till relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="43a15-214">Type the required attributes for Britta Simon into related textboxes.</span></span>
    
    >[!IMPORTANT]
    ><span data-ttu-id="43a15-215">Britta's User Name-attribut i PerformanceCentre måste vara samma som användarnamnet i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="43a15-215">Britta's User Name attribute in PerformanceCentre must be the same as the User Name in Azure AD.</span></span>
    
    <span data-ttu-id="43a15-216">b.</span><span class="sxs-lookup"><span data-stu-id="43a15-216">b.</span></span> <span data-ttu-id="43a15-217">Välj **administratör** som **Välj rollen**.</span><span class="sxs-lookup"><span data-stu-id="43a15-217">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="43a15-218">c.</span><span class="sxs-lookup"><span data-stu-id="43a15-218">c.</span></span> <span data-ttu-id="43a15-219">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="43a15-219">Click **Save**.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="43a15-220">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="43a15-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="43a15-221">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till PerformanceCentre.</span><span class="sxs-lookup"><span data-stu-id="43a15-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PerformanceCentre.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="43a15-223">**Om du vill tilldela PerformanceCentre Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="43a15-223">**To assign Britta Simon to PerformanceCentre, perform the following steps:**</span></span>

1. <span data-ttu-id="43a15-224">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="43a15-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="43a15-226">Välj i listan med program **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="43a15-226">In the applications list, select **PerformanceCentre**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_app.png) 

3. <span data-ttu-id="43a15-228">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="43a15-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="43a15-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="43a15-230">Click **Add** button.</span></span> <span data-ttu-id="43a15-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43a15-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="43a15-233">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="43a15-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="43a15-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43a15-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="43a15-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43a15-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="43a15-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43a15-236">Testing single sign-on</span></span>

<span data-ttu-id="43a15-237">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="43a15-237">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="43a15-238">När du klickar på panelen PerformanceCentre på åtkomstpanelen du bör få automatiskt loggat in på ditt PerformanceCentre program.</span><span class="sxs-lookup"><span data-stu-id="43a15-238">When you click the PerformanceCentre tile in the Access Panel, you should get automatically signed-on to your PerformanceCentre application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43a15-239">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="43a15-239">Additional resources</span></span>

* [<span data-ttu-id="43a15-240">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="43a15-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="43a15-241">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="43a15-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png

[100]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png

