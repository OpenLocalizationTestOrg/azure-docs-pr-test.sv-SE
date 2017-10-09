---
title: aaaAzure Active Directory B2B collaboration kod och PowerShell-exempel | Microsoft Docs
description: "Koden och PowerShell-exempel för Azure Active Directory B2B-samarbete"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/11/2017
ms.author: sasubram
ms.openlocfilehash: 8e4f66fcb50d190899304831ea7ccd2203c5468c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-code-and-powershell-samples"></a><span data-ttu-id="0bb00-103">Azure Active Directory B2B-samarbete koden och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="0bb00-103">Azure Active Directory B2B collaboration code and PowerShell samples</span></span>

## <a name="powershell-example"></a><span data-ttu-id="0bb00-104">PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="0bb00-104">PowerShell example</span></span>
<span data-ttu-id="0bb00-105">Du kan bulk-inbjudan externa användare tooan organisation från e-postadresser som du har lagrat i en. CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="0bb00-105">You can bulk-invite external users tooan organization from email addresses that you have stored in a .CSV file.</span></span>

1. <span data-ttu-id="0bb00-106">Förbered hello. CSV-filen skapa en ny CSV-fil och ger den namnet invitations.csv.</span><span class="sxs-lookup"><span data-stu-id="0bb00-106">Prepare hello .CSV file Create a new CSV file and name it invitations.csv.</span></span> <span data-ttu-id="0bb00-107">I det här exemplet hello fil sparas i C:\data och innehåller hello följande information:</span><span class="sxs-lookup"><span data-stu-id="0bb00-107">In this example, hello file is saved in C:\data, and contains hello following information:</span></span>
  
  <span data-ttu-id="0bb00-108">Namn</span><span class="sxs-lookup"><span data-stu-id="0bb00-108">Name</span></span>                  |  <span data-ttu-id="0bb00-109">InvitedUserEmailAddress</span><span class="sxs-lookup"><span data-stu-id="0bb00-109">InvitedUserEmailAddress</span></span>
  --------------------- | --------------------------
  <span data-ttu-id="0bb00-110">Gmail B2B bjudits in</span><span class="sxs-lookup"><span data-stu-id="0bb00-110">Gmail B2B Invitee</span></span>     | b2binvitee@gmail.com
  <span data-ttu-id="0bb00-111">Outlook B2B bjudits in</span><span class="sxs-lookup"><span data-stu-id="0bb00-111">Outlook B2B invitee</span></span>   | b2binvitee@outlook.com


2. <span data-ttu-id="0bb00-112">Hämta hello senaste Azure AD PowerShell toouse Hej nya cmdlet: ar, måste du installera hello uppdateras Azure AD PowerShell-modulen som du kan hämta från [hello Powershell-modulen versionen sida](https://www.powershellgallery.com/packages/AzureADPreview)</span><span class="sxs-lookup"><span data-stu-id="0bb00-112">Get hello latest Azure AD PowerShell toouse hello new cmdlets, you must install hello updated Azure AD PowerShell module, which you can download from [hello Powershell module's release page](https://www.powershellgallery.com/packages/AzureADPreview)</span></span>

3. <span data-ttu-id="0bb00-113">Logga in tooyour innehavare</span><span class="sxs-lookup"><span data-stu-id="0bb00-113">Sign in tooyour tenancy</span></span>

    ```
    $cred = Get-Credential
    Connect-AzureAD -Credential $cred
    ```

4. <span data-ttu-id="0bb00-114">Kör hello PowerShell-cmdlet</span><span class="sxs-lookup"><span data-stu-id="0bb00-114">Run hello PowerShell cmdlet</span></span>

  ```
  $invitations = import-csv C:\data\invitations.csv
  $messageInfo = New-Object Microsoft.Open.MSGraph.Model.InvitedUserMessageInfo
  $messageInfo.customizedMessageBody = “Hey there! Check this out. I created an invitation through PowerShell”
  foreach ($email in $invitations) {New-AzureADMSInvitation -InvitedUserEmailAddress $email.InvitedUserEmailAddress -InvitedUserDisplayName $email.Name -InviteRedirectUrl https://wingtiptoysonline-dev-ed.my.salesforce.com -InvitedUserMessageInfo $messageInfo -SendInvitationMessage $true}
  ```

<span data-ttu-id="0bb00-115">Denna cmdlet skickas en inbjudan toohello e-postadresser i invitations.csv.</span><span class="sxs-lookup"><span data-stu-id="0bb00-115">This cmdlet sends an invitation toohello email addresses in invitations.csv.</span></span> <span data-ttu-id="0bb00-116">Ytterligare funktioner för denna cmdlet är:</span><span class="sxs-lookup"><span data-stu-id="0bb00-116">Additional features of this cmdlet include:</span></span>
- <span data-ttu-id="0bb00-117">Anpassad text i hello e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="0bb00-117">Customized text in hello email message</span></span>
- <span data-ttu-id="0bb00-118">Inklusive ett visningsnamn för hello inbjudna användare</span><span class="sxs-lookup"><span data-stu-id="0bb00-118">Including a display name for hello invited user</span></span>
- <span data-ttu-id="0bb00-119">Skicka meddelanden tooCCs eller utelämna e-postmeddelanden helt</span><span class="sxs-lookup"><span data-stu-id="0bb00-119">Sending messages tooCCs or suppressing email messages altogether</span></span>

## <a name="code-sample"></a><span data-ttu-id="0bb00-120">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="0bb00-120">Code sample</span></span>
<span data-ttu-id="0bb00-121">Här visar vi hur toocall hello inbjudan API: T i läget ”endast app”, tooget hello inlösning URL för hello resurs toowhich du bjuder in hello B2B-användare.</span><span class="sxs-lookup"><span data-stu-id="0bb00-121">Here we illustrate how toocall hello invitation API, in "app-only" mode, tooget hello redemption URL for hello resource toowhich you are inviting hello B2B user.</span></span> <span data-ttu-id="0bb00-122">hello målet är toosend en anpassad inbjudan e-post.</span><span class="sxs-lookup"><span data-stu-id="0bb00-122">hello goal is toosend a custom invitation email.</span></span> <span data-ttu-id="0bb00-123">hello e-post kan sammanställas med HTTP-klienter, så att du kan anpassa hur den ser ut och skicka den via Graph API.</span><span class="sxs-lookup"><span data-stu-id="0bb00-123">hello email can be composed with an HTTP client, so you can customize how it looks and send it through Graph API.</span></span>

```
namespace SampleInviteApp
{
    using System;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    class Program
    {
        /// <summary>
        /// Microsoft graph resource.
        /// </summary>
        static readonly string GraphResource = "https://graph.microsoft.com";
 
        /// <summary>
        /// Microsoft graph invite endpoint.
        /// </summary>
        static readonly string InviteEndPoint = "https://graph.microsoft.com/v1.0/invitations";
 
        /// <summary>
        ///  Authentication endpoint tooget token.
        /// </summary>
        static readonly string EstsLoginEndpoint = "https://login.microsoftonline.com";
 
        /// <summary>
        /// This is hello tenantid of hello tenant you want tooinvite users to.
        /// </summary>
        private static readonly string TenantID = "";
 
        /// <summary>
        /// This is hello application id of hello application that is registered in hello above tenant.
        /// hello required scopes are available in hello below link.
        /// https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/invitation_post
        /// </summary>
        private static readonly string TestAppClientId = "";
 
        /// <summary>
        /// Client secret of hello application.
        /// </summary>
        private static readonly string TestAppClientSecret = @"
 
        /// <summary>
        /// This is hello email address of hello user you want tooinvite.
        /// </summary>
        private static readonly string InvitedUserEmailAddress = @"";
 
        /// <summary>
        /// This is hello display name of hello user you want tooinvite.
        /// </summary>
        private static readonly string InvitedUserDisplayName = @"";
 
        /// <summary>
        /// Main method.
        /// </summary>
        /// <param name="args">Optional arguments</param>
        static void Main(string[] args)
        {
            Invitation invitation = CreateInvitation();
            SendInvitation(invitation);
        }
 
        /// <summary>
        /// Create hello invitation object.
        /// </summary>
        /// <returns>Returns hello invitation object.</returns>
        private static Invitation CreateInvitation()
        {
            // Set hello invitation object.
            Invitation invitation = new Invitation();
            invitation.InvitedUserDisplayName = InvitedUserDisplayName;
            invitation.InvitedUserEmailAddress = InvitedUserEmailAddress;
            invitation.InviteRedirectUrl = "https://www.microsoft.com";
            invitation.SendInvitationMessage = true;
            return invitation;
        }
 
        /// <summary>
        /// Send hello guest user invite request.
        /// </summary>
        /// <param name="invitation">Invitation object.</param>
        private static void SendInvitation(Invitation invitation)
        {
            string accessToken = GetAccessToken();
 
            HttpClient httpClient = GetHttpClient(accessToken);
 
            // Make hello invite call. 
            HttpContent content = new StringContent(JsonConvert.SerializeObject(invitation));
            content.Headers.Add("ContentType", "application/json");
            var postResponse = httpClient.PostAsync(InviteEndPoint, content).Result;
            string serverResponse = postResponse.Content.ReadAsStringAsync().Result;
            Console.WriteLine(serverResponse);
        }
 
        /// <summary>
        /// Get hello HTTP client.
        /// </summary>
        /// <param name="accessToken">Access token</param>
        /// <returns>Returns hello Http Client.</returns>
        private static HttpClient GetHttpClient(string accessToken)
        {
            // setup http client.
            HttpClient httpClient = new HttpClient();
            httpClient.Timeout = TimeSpan.FromSeconds(300);
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
            httpClient.DefaultRequestHeaders.Add("client-request-id", Guid.NewGuid().ToString());
            Console.WriteLine(
                "CorrelationID for hello request: {0}",
                httpClient.DefaultRequestHeaders.GetValues("client-request-id").Single());
            return httpClient;
        }
 
        /// <summary>
        /// Get hello access token for our application tootalk toomicrosoft graph.
        /// </summary>
        /// <returns>Returns hello access token for our application tootalk toomicrosoft graph.</returns>
        private static string GetAccessToken()
        {
            string accessToken = null;
 
            // Get hello access token for our application tootalk toomicrosoft graph.
            try
            {
                AuthenticationContext testAuthContext =
                    new AuthenticationContext(string.Format("{0}/{1}", EstsLoginEndpoint, TenantID));
                AuthenticationResult testAuthResult = testAuthContext.AcquireTokenAsync(
                    GraphResource,
                    new ClientCredential(TestAppClientId, TestAppClientSecret)).Result;
                accessToken = testAuthResult.AccessToken;
            }
            catch (AdalException ex)
            {
                Console.WriteLine("An exception was thrown while fetching hello token: {0}.", ex);
                throw;
            }
 
            return accessToken;
        }
 
        /// <summary>
        /// Invitation class.
        /// </summary>
        public class Invitation
        {
            /// <summary>
            /// Gets or sets display name.
            /// </summary>
            public string InvitedUserDisplayName { get; set; }
 
            /// <summary>
            /// Gets or sets display name.
            /// </summary>
            public string InvitedUserEmailAddress { get; set; }
 
            /// <summary>
            /// Gets or sets a value indicating whether Invitation Manager should send hello email tooInvitedUser.
            /// </summary>
            public bool SendInvitationMessage { get; set; }
 
            /// <summary>
            /// Gets or sets invitation redirect URL
            /// </summary>
            public string InviteRedirectUrl { get; set; }
        }
    }
}
```


## <a name="next-steps"></a><span data-ttu-id="0bb00-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0bb00-124">Next steps</span></span>

<span data-ttu-id="0bb00-125">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="0bb00-125">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="0bb00-126">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="0bb00-126">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="0bb00-127">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="0bb00-127">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="0bb00-128">Lägga till en B2B-samarbete tooa användarroll</span><span class="sxs-lookup"><span data-stu-id="0bb00-128">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="0bb00-129">Delegera B2B-samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="0bb00-129">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="0bb00-130">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="0bb00-130">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="0bb00-131">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="0bb00-131">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="0bb00-132">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="0bb00-132">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="0bb00-133">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="0bb00-133">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="0bb00-134">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="0bb00-134">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="0bb00-135">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="0bb00-135">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
