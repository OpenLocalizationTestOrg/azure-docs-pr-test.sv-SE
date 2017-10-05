---
title: "Självstudier: Azure Active Directory-integrering med molnet för Microsoft Azure-hanteringsportalen | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och molnet hanteringsportal för Microsoft Azure."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4ea9f47c-25ca-42b0-a878-9e7aa6f34973
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: bae5f05a161b2730bf662bcb47f20ab3e1799951
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a><span data-ttu-id="82e33-103">Självstudier: Azure Active Directory-integrering med molntjänster Management Portal för Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="82e33-103">Tutorial: Azure Active Directory integration with Cloud Management Portal for Microsoft Azure</span></span>

<span data-ttu-id="82e33-104">I kursen får lära du att integrera molnet hanteringsportal för Microsoft Azure med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="82e33-104">In this tutorial, you learn how to integrate Cloud Management Portal for Microsoft Azure with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="82e33-105">Integrera molnet hanteringsportal för Microsoft Azure med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="82e33-105">Integrating Cloud Management Portal for Microsoft Azure with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="82e33-106">Du kan styra i Azure AD som har åtkomst till molnet Management Portal för Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="82e33-106">You can control in Azure AD who has access to Cloud Management Portal for Microsoft Azure</span></span>
- <span data-ttu-id="82e33-107">Du kan aktivera användarna att automatiskt hämta loggat in på molnet hanteringsportal för Microsoft Azure (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="82e33-107">You can enable your users to automatically get signed-on to Cloud Management Portal for Microsoft Azure (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="82e33-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="82e33-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="82e33-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="82e33-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82e33-110">Krav</span><span class="sxs-lookup"><span data-stu-id="82e33-110">Prerequisites</span></span>

<span data-ttu-id="82e33-111">För att konfigurera Azure AD-integrering med molntjänster Management Portal för Microsoft Azure, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="82e33-111">To configure Azure AD integration with Cloud Management Portal for Microsoft Azure, you need the following items:</span></span>

- <span data-ttu-id="82e33-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="82e33-112">An Azure AD subscription</span></span>
- <span data-ttu-id="82e33-113">En molnet Management Portal för Microsoft Azure enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="82e33-113">A Cloud Management Portal for Microsoft Azure single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="82e33-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="82e33-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="82e33-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="82e33-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="82e33-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="82e33-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="82e33-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="82e33-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="82e33-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="82e33-118">Scenario description</span></span>
<span data-ttu-id="82e33-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="82e33-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="82e33-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="82e33-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="82e33-121">Att lägga till molnet hanteringsportal för Microsoft Azure från galleriet</span><span class="sxs-lookup"><span data-stu-id="82e33-121">Adding Cloud Management Portal for Microsoft Azure from the gallery</span></span>
2. <span data-ttu-id="82e33-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="82e33-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cloud-management-portal-for-microsoft-azure-from-the-gallery"></a><span data-ttu-id="82e33-123">Att lägga till molnet hanteringsportal för Microsoft Azure från galleriet</span><span class="sxs-lookup"><span data-stu-id="82e33-123">Adding Cloud Management Portal for Microsoft Azure from the gallery</span></span>
<span data-ttu-id="82e33-124">Du måste lägga till molnet hanteringsportal för Microsoft Azure från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av molnet hanteringsportal för Microsoft Azure i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82e33-124">To configure the integration of Cloud Management Portal for Microsoft Azure into Azure AD, you need to add Cloud Management Portal for Microsoft Azure from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="82e33-125">**Utför följande steg för att lägga till molnet hanteringsportal för Microsoft Azure från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="82e33-125">**To add Cloud Management Portal for Microsoft Azure from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="82e33-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="82e33-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="82e33-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="82e33-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="82e33-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="82e33-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="82e33-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="82e33-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="82e33-133">I sökrutan skriver **molnet för Microsoft Azure-hanteringsportalen**.</span><span class="sxs-lookup"><span data-stu-id="82e33-133">In the search box, type **Cloud Management Portal for Microsoft Azure**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_search.png)

5. <span data-ttu-id="82e33-135">I resultatpanelen väljer **molnet för Microsoft Azure-hanteringsportalen**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="82e33-135">In the results panel, select **Cloud Management Portal for Microsoft Azure**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="82e33-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="82e33-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="82e33-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med molnet hanteringsportal för Microsoft Azure baserad på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="82e33-138">In this section, you configure and test Azure AD single sign-on with Cloud Management Portal for Microsoft Azure based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="82e33-139">Azure AD måste du känna till användaren i molnet för Microsoft Azure-hanteringsportalen motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="82e33-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cloud Management Portal for Microsoft Azure is to a user in Azure AD.</span></span> <span data-ttu-id="82e33-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i molnet Management Portal för Microsoft Azure upprättas.</span><span class="sxs-lookup"><span data-stu-id="82e33-140">In other words, a link relationship between an Azure AD user and the related user in Cloud Management Portal for Microsoft Azure needs to be established.</span></span>

<span data-ttu-id="82e33-141">I molnet Management Portal för Microsoft Azure, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="82e33-141">In Cloud Management Portal for Microsoft Azure, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="82e33-142">Om du vill konfigurera och testa Azure AD enkel inloggning med molnet för Microsoft Azure-hanteringsportalen, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="82e33-142">To configure and test Azure AD single sign-on with Cloud Management Portal for Microsoft Azure, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="82e33-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="82e33-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="82e33-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="82e33-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="82e33-145">**[Skapa en moln Management Portal för Microsoft Azure testanvändare](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)**  – du har en motsvarighet för Britta Simon i molnet Management Portal för Microsoft Azure som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="82e33-145">**[Creating a Cloud Management Portal for Microsoft Azure test user](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)** - to have a counterpart of Britta Simon in Cloud Management Portal for Microsoft Azure that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="82e33-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="82e33-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="82e33-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="82e33-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="82e33-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="82e33-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="82e33-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i molnet Management-portalen för Microsoft Azure-program.</span><span class="sxs-lookup"><span data-stu-id="82e33-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cloud Management Portal for Microsoft Azure application.</span></span>

<span data-ttu-id="82e33-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med molnet för Microsoft Azure-hanteringsportalen:**</span><span class="sxs-lookup"><span data-stu-id="82e33-150">**To configure Azure AD single sign-on with Cloud Management Portal for Microsoft Azure, perform the following steps:**</span></span>

1. <span data-ttu-id="82e33-151">I Azure-portalen på den **moln hanteringsportal för Microsoft Azure** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="82e33-151">In the Azure portal, on the **Cloud Management Portal for Microsoft Azure** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="82e33-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="82e33-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_samlbase.png)

3. <span data-ttu-id="82e33-155">På den **moln hanteringsportal för URL: er och Microsoft Azure Domain** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="82e33-155">On the **Cloud Management Portal for Microsoft Azure Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_url.png)

    <span data-ttu-id="82e33-157">a.</span><span class="sxs-lookup"><span data-stu-id="82e33-157">a.</span></span> <span data-ttu-id="82e33-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="82e33-158">In the **Sign-on URL** textbox, type a URL using the following patterns:</span></span> 
    
    | |
    |--|
    | `https://portal.newsignature.com/<instancename>` |   
    | `https://portal.igcm.com/<instancename>` |
    
    <span data-ttu-id="82e33-159">b.</span><span class="sxs-lookup"><span data-stu-id="82e33-159">b.</span></span> <span data-ttu-id="82e33-160">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="82e33-160">In the **Identifier** textbox, type a URL using the following patterns:</span></span> 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com` |
    | `https://<subdomain>.newsignature.com` |

    <span data-ttu-id="82e33-161">c.</span><span class="sxs-lookup"><span data-stu-id="82e33-161">c.</span></span> <span data-ttu-id="82e33-162">I den **Reply URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="82e33-162">In the **Reply URL** textbox, type a URL using the following patterns:</span></span> 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com/<instancename>` |
    | `https://<subdomain>.newsignature.com` |
    | `https://<subdomain>.newsignature.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="82e33-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="82e33-163">These values are not real.</span></span> <span data-ttu-id="82e33-164">Uppdatera dessa värden med faktiska inloggnings-URL, identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="82e33-164">Update these values with the actual Sign-On URL, Identifier and Reply URL.</span></span> <span data-ttu-id="82e33-165">Kontakta [moln Management Portal för Microsoft Azure-klient supportteamet](mailto:jczernuszka@newsignature.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="82e33-165">Contact [Cloud Management Portal for Microsoft Azure Client support team](mailto:jczernuszka@newsignature.com) to get these values.</span></span> 
 
4. <span data-ttu-id="82e33-166">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="82e33-166">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_certificate.png) 

5. <span data-ttu-id="82e33-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="82e33-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="82e33-170">På den **moln hanteringsportal för Microsoft Azure Configuration** klickar du på **konfigurera Portal för molnet för Microsoft Azure** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="82e33-170">On the **Cloud Management Portal for Microsoft Azure Configuration** section, click **Configure Cloud Management Portal for Microsoft Azure** to open **Configure sign-on** window.</span></span> <span data-ttu-id="82e33-171">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="82e33-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_configure.png) 

7. <span data-ttu-id="82e33-173">Konfigurera enkel inloggning på **molnet för Microsoft Azure-hanteringsportalen** sida, måste du skicka den hämtade **certifikat**, **Sign-Out URL**, **SAML inloggning tjänst-URL för enkel** och **SAML enhets-ID** till [molnet hanteringsportal för Microsoft Azure-teamet](mailto:jczernuszka@newsignature.com).</span><span class="sxs-lookup"><span data-stu-id="82e33-173">To configure single sign-on on **Cloud Management Portal for Microsoft Azure** side, you need to send the downloaded **Certificate**, **Sign-Out URL**, **SAML Single Sign-On Service URL** and **SAML Entity ID** to [Cloud Management Portal for Microsoft Azure support team](mailto:jczernuszka@newsignature.com).</span></span> <span data-ttu-id="82e33-174">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="82e33-174">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="82e33-175">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="82e33-175">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="82e33-176">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="82e33-176">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="82e33-177">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="82e33-177">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="82e33-178">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="82e33-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="82e33-179">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="82e33-179">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="82e33-181">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="82e33-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="82e33-182">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="82e33-182">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="82e33-184">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="82e33-184">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="82e33-186">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="82e33-186">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="82e33-188">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="82e33-188">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="82e33-190">a.</span><span class="sxs-lookup"><span data-stu-id="82e33-190">a.</span></span> <span data-ttu-id="82e33-191">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="82e33-191">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="82e33-192">b.</span><span class="sxs-lookup"><span data-stu-id="82e33-192">b.</span></span> <span data-ttu-id="82e33-193">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="82e33-193">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="82e33-194">c.</span><span class="sxs-lookup"><span data-stu-id="82e33-194">c.</span></span> <span data-ttu-id="82e33-195">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="82e33-195">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="82e33-196">d.</span><span class="sxs-lookup"><span data-stu-id="82e33-196">d.</span></span> <span data-ttu-id="82e33-197">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="82e33-197">Click **Create**.</span></span>
 
### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a><span data-ttu-id="82e33-198">Skapa en moln Management Portal för Microsoft Azure testanvändare</span><span class="sxs-lookup"><span data-stu-id="82e33-198">Creating a Cloud Management Portal for Microsoft Azure test user</span></span>

<span data-ttu-id="82e33-199">Syftet med det här avsnittet är att skapa en användare som heter Britta Simon i molnet Management-portalen för Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="82e33-199">The objective of this section is to create a user called Britta Simon in Cloud Management Portal for Microsoft Azure.</span></span> <span data-ttu-id="82e33-200">Se tillsammans med [moln hanteringsportal för Microsoft Azure-teamet](mailto:jczernuszka@newsignature.com) att lägga till användare i hanteringsportalen molnet för Microsoft Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="82e33-200">Please work with [Cloud Management Portal for Microsoft Azure support team](mailto:jczernuszka@newsignature.com) to add the users in the Cloud Management Portal for Microsoft Azure account.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="82e33-201">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="82e33-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="82e33-202">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till molnet för Microsoft Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="82e33-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cloud Management Portal for Microsoft Azure.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="82e33-204">**Om du vill tilldela moln hanteringsportal för Microsoft Azure Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="82e33-204">**To assign Britta Simon to Cloud Management Portal for Microsoft Azure, perform the following steps:**</span></span>

1. <span data-ttu-id="82e33-205">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="82e33-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="82e33-207">Välj i listan med program **molnet för Microsoft Azure-hanteringsportalen**.</span><span class="sxs-lookup"><span data-stu-id="82e33-207">In the applications list, select **Cloud Management Portal for Microsoft Azure**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_app.png) 

3. <span data-ttu-id="82e33-209">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="82e33-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="82e33-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="82e33-211">Click **Add** button.</span></span> <span data-ttu-id="82e33-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="82e33-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="82e33-214">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="82e33-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="82e33-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="82e33-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="82e33-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="82e33-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="82e33-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="82e33-217">Testing single sign-on</span></span>

<span data-ttu-id="82e33-218">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="82e33-218">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>
<span data-ttu-id="82e33-219">När du klickar på hanteringsportalen molnet för Microsoft Azure-panelen på åtkomstpanelen du bör få automatiskt loggat in på ditt moln Management Portal för Microsoft Azure-program.</span><span class="sxs-lookup"><span data-stu-id="82e33-219">When you click the Cloud Management Portal for Microsoft Azure tile in the Access Panel, you should get automatically signed-on to your Cloud Management Portal for Microsoft Azure application.</span></span>

<span data-ttu-id="82e33-220">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="82e33-220">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="82e33-221">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="82e33-221">Additional resources</span></span>

* [<span data-ttu-id="82e33-222">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="82e33-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="82e33-223">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="82e33-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png

