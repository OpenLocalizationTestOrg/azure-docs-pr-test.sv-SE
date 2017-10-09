---
title: aaaAzure Mobile Engagement - serverdelen integrering
description: "Anslut Azure Mobile Engagement med en SharePoint-serverdel toocreate kampanjer från SharePoint"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 06297b43-579f-46e6-8a58-961a68f9aa09
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 89e1ef57db607d63c326a760b20260ad439f08b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement---api-integration"></a><span data-ttu-id="c381b-103">Azure Mobile Engagement - API-integrering</span><span class="sxs-lookup"><span data-stu-id="c381b-103">Azure Mobile Engagement - API integration</span></span>
<span data-ttu-id="c381b-104">I ett automatiserat marknadsföring system inträffa skapa och aktivera hello marknadsföringskampanjer även du automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c381b-104">In an automated marketing system, creating and activating hello marketing campaigns also occur automatically.</span></span> <span data-ttu-id="c381b-105">För detta ändamål - Azure Mobile Engagement kan skapa dessa automatiserade marknadsföringskampanjer via API: er samt.</span><span class="sxs-lookup"><span data-stu-id="c381b-105">For this purpose - Azure Mobile Engagement enables creating such automated marketing campaigns using APIs as well.</span></span> 

<span data-ttu-id="c381b-106">Kunder som använder vanligtvis hello Mobile Engagement klientdelen gränssnittet toocreate meddelanden/avsökningar etc som en del av deras marknadsföringskampanjer.</span><span class="sxs-lookup"><span data-stu-id="c381b-106">Typically customers use hello Mobile Engagement front end interface toocreate announcements/polls etc as part of their marketing campaigns.</span></span> <span data-ttu-id="c381b-107">Men som hello marknadsföringskampanjer blir mogen, krävs ett tooleverage hello data låst i hello serverdelssystem (till exempel alla CRM-system eller CMS-system som SharePoint) så att du kan skapa en helt automatiserad pipeline som skapar kampanjer i Mobile Engagement dynamiskt baserat på hello data flödar i från hello serverdelssystem.</span><span class="sxs-lookup"><span data-stu-id="c381b-107">However as hello marketing campaigns become mature, there is a need tooleverage hello data locked in hello backend systems (like any CRM system or CMS system like SharePoint) so that a fully automated pipeline can be created which creates campaigns in Mobile Engagement dynamically based on hello data flowing in from hello backend systems.</span></span> 

![][5]

<span data-ttu-id="c381b-108">Den här självstudien blir via ett scenario där SharePoint företagsanvändare fyller på en SharePoint-lista med marknadsföring data och en automatiserad process hämtar objekt från hello lista och ansluter med hello Mobile Engagement system hello tillgängliga REST API: er toocreate marknadsföringskampanj från hello SharePoint-data.</span><span class="sxs-lookup"><span data-stu-id="c381b-108">This tutorial goes through such a scenario where a SharePoint business user populates a SharePoint list with marketing data and an automated process picks up items from hello list and connects with hello Mobile Engagement system using hello available REST APIs toocreate a marketing campaign from hello SharePoint data.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="c381b-109">I allmänhet kan du använda det här exemplet som utgångspunkt för att förstå hur toocall Mobile Engagement REST API: er som det beskrivs hello två viktiga aspekter av anropar hello API- och autentiseras skickar parametrar.</span><span class="sxs-lookup"><span data-stu-id="c381b-109">In general, you can use this sample as a starting point for understanding how toocall any Mobile Engagement REST API as it details hello two key aspects of calling hello APIs - authenticating and passing parameters.</span></span> 
> 
> 

## <a name="sharepoint-integration"></a><span data-ttu-id="c381b-110">SharePoint-integrering</span><span class="sxs-lookup"><span data-stu-id="c381b-110">SharePoint integration</span></span>
1. <span data-ttu-id="c381b-111">Här är vad hello exempel SharePoint lista ser ut.</span><span class="sxs-lookup"><span data-stu-id="c381b-111">Here is what hello sample SharePoint list looks like.</span></span> <span data-ttu-id="c381b-112">**Rubrik**, **kategori**, **NotificationTitle**, **meddelandet** och **URL** används för att skapa hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c381b-112">**Title**, **Category**, **NotificationTitle**, **Message** and **URL** are used for creating hello announcement.</span></span> <span data-ttu-id="c381b-113">Det finns en kolumn med namnet **IsProcessed** som används av hello exempel automation processen i hello form av ett konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="c381b-113">There is a column called **IsProcessed** which is used by hello sample automation process in hello form of a console program.</span></span> <span data-ttu-id="c381b-114">Du kan antingen köra detta konsolprogram som en Azure-Webbjobb så att du kan schemalägga det eller direkt använda hello SharePoint arbetsflöde tooprogram skapa och genom att aktivera hello-meddelande när ett objekt infogas i hello SharePoint-lista.</span><span class="sxs-lookup"><span data-stu-id="c381b-114">You can either run this console program as an Azure WebJob so that you can schedule it or you can directly use hello SharePoint workflow tooprogram creating and activating hello announcement when an item is inserted into hello SharePoint list.</span></span> <span data-ttu-id="c381b-115">I det här exemplet använder vi hello konsolprogram som går via hello objekt i hello SharePoint och skapa meddelande i Azure Mobile Engagement för dem och slutligen markerar hello **IsProcessed** flaggan toobe true i Skapa en lyckad meddelande.</span><span class="sxs-lookup"><span data-stu-id="c381b-115">In this sample we use hello console program which goes through hello items in hello SharePoint list and create announcement in Azure Mobile Engagement for each of them and then finally marks hello **IsProcessed** flag toobe true on successful announcement creation.</span></span>
   
    ![][1]
2. <span data-ttu-id="c381b-116">Vi använder hello kod från hello exempel *Remote Authentication i SharePoint Online med hjälp av hello klienten Object Model* [här](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) tooauthenticate med hello SharePoint-lista.</span><span class="sxs-lookup"><span data-stu-id="c381b-116">We are using hello code from hello sample *Remote Authentication in SharePoint Online Using hello Client Object Model* [here](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) tooauthenticate with hello SharePoint list.</span></span>
3. <span data-ttu-id="c381b-117">När autentiseringen är vi gå igenom hello lista objekt toofind bort alla objekt som nyligen skapade (som kommer att ha **IsProcessed** = false).</span><span class="sxs-lookup"><span data-stu-id="c381b-117">Once authenticated, we loop through hello list items toofind out any newly created items (which will have **IsProcessed** = false).</span></span> 
   
         static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);
   
                // Load hello SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();
   
                // Loop through hello list toogo through all hello items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;
   
                        // Send an HTTP request toocreate AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;
   
                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this tootrue
                            item["IsProcessed"] = "Yes";
   
                            // Now try tooactivate hello campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a><span data-ttu-id="c381b-118">Mobile Engagement-integration</span><span class="sxs-lookup"><span data-stu-id="c381b-118">Mobile Engagement integration</span></span>
1. <span data-ttu-id="c381b-119">När det att hitta ett objekt som kräver bearbetning – vi extrahera hello information som krävs för toocreate ett meddelande från hello listobjekt och anropa `CreateAzMECampaign` toocreate den och därefter `ActivateAzMECampaign` tooactivate den.</span><span class="sxs-lookup"><span data-stu-id="c381b-119">Once we find an item which requires processing - we extract hello information required toocreate an announcement from hello list item and call `CreateAzMECampaign` toocreate it and subsequently `ActivateAzMECampaign` tooactivate it.</span></span> <span data-ttu-id="c381b-120">Dessa är i stort sett REST API-anrop som är anropar toohello Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="c381b-120">These are essentially REST API calls calling toohello Mobile Engagement backend.</span></span> 
2. <span data-ttu-id="c381b-121">hello Mobile Engagement REST API: er kräver en **authorization HTTP-huvud för grundläggande autentisering schemat** som består av hello `ApplicationId` och hello `ApiKey` som du får från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c381b-121">hello Mobile Engagement REST APIs require a **Basic auth scheme authorization HTTP header** which is composed of hello `ApplicationId` and hello `ApiKey` which you get from hello Azure portal.</span></span> <span data-ttu-id="c381b-122">Kontrollera att du använder hello nyckeln från hello **api-nycklar** avsnitt och *inte* från hello **sdk nycklar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c381b-122">Make sure that you are using hello Key from hello **api keys** section and *not* from hello **sdk keys** section.</span></span> 
   
   ![][2]
   
       static string CreateAuthZHeader()
       {
           string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
           return BasicAuthzHeaderString;
       }
   
       static public string EncodeTo64(string toEncode)
       {
           byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
           string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
           return returnValue;
       }  
3. <span data-ttu-id="c381b-123">För att skapa hello-meddelande typen kampanj – Se toohello [dokumentationen](https://msdn.microsoft.com/library/azure/mt683750.aspx).</span><span class="sxs-lookup"><span data-stu-id="c381b-123">For creating hello announcement type campaign - refer toohello [documentation](https://msdn.microsoft.com/library/azure/mt683750.aspx).</span></span> <span data-ttu-id="c381b-124">Du behöver toomake till att du anger hello kampanj `kind` som *meddelande* och hello [nyttolast](https://msdn.microsoft.com/library/azure/mt683751.aspx) och passerar den som FormUrlEncodedContent.</span><span class="sxs-lookup"><span data-stu-id="c381b-124">You need toomake sure that you are specifying hello campaign `kind` as *announcement* and hello [payload](https://msdn.microsoft.com/library/azure/mt683751.aspx) and passing it as FormUrlEncodedContent.</span></span> 
   
        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";
   
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                // Create hello payload toosend hello content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });
   
                // Send hello POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is hello campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }
4. <span data-ttu-id="c381b-125">När du har skapat hello-meddelande, visas något som liknar hello följa på hello Mobile Engagement-portalen (Observera att hello tillstånd = utkast och aktiverad = ej tillämpligt)</span><span class="sxs-lookup"><span data-stu-id="c381b-125">Once you have hello announcement created, you will see something like hello following on hello Mobile Engagement portal (note that hello State=Draft and Activated = N/A)</span></span>
   
    ![][3]
5. <span data-ttu-id="c381b-126">`CreateAzMECampaign`skapar ett meddelande kampanj och returnerar dess Id toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="c381b-126">`CreateAzMECampaign` creates an announcement campaign and returns its Id toohello caller.</span></span> <span data-ttu-id="c381b-127">`ActivateAzMECampaign`kräver detta Id som hello parametern tooactivate hello kampanj.</span><span class="sxs-lookup"><span data-stu-id="c381b-127">`ActivateAzMECampaign` requires this Id as hello parameter tooactivate hello campaign.</span></span> 
   
        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });
   
                // Send hello POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }
6. <span data-ttu-id="c381b-128">När du har aktiverat hello-meddelande, visas något som liknar följande hello på hello Mobile Engagement-portalen:</span><span class="sxs-lookup"><span data-stu-id="c381b-128">Once you have hello announcement activated, you will see something like hello following on hello Mobile Engagement portal:</span></span>
   
    ![][4]
7. <span data-ttu-id="c381b-129">Så snart hello kampanj hämtar aktiverat kan startar alla enheter som uppfyller kriteriet för hello hello kampanj ser meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c381b-129">As soon as hello campaign gets activated, any devices which satisfy hello criterion on hello campaign will start seeing notifications.</span></span> 
8. <span data-ttu-id="c381b-130">Du ser också att hello listobjekt markerad med IsProcessed = false har ställts in tooTrue när hello-meddelande kampanjen skapades.</span><span class="sxs-lookup"><span data-stu-id="c381b-130">You will also notice that hello list item marked with IsProcessed = false has been set tooTrue once hello announcement campaign is created.</span></span>  

<span data-ttu-id="c381b-131">Det här exemplet skapas en enkel meddelande kampanj att ange främst hello krävs egenskaper.</span><span class="sxs-lookup"><span data-stu-id="c381b-131">This sample created a simple announcement campaign specifying mostly hello required properties.</span></span> <span data-ttu-id="c381b-132">Du kan anpassa så mycket som du kan från hello-portalen med hello information [här](https://msdn.microsoft.com/library/azure/mt683751.aspx).</span><span class="sxs-lookup"><span data-stu-id="c381b-132">You can customize this as much as you can from hello portal by using hello information [here](https://msdn.microsoft.com/library/azure/mt683751.aspx).</span></span> 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png



