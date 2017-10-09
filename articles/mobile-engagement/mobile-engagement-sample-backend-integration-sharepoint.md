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
# <a name="azure-mobile-engagement---api-integration"></a>Azure Mobile Engagement - API-integrering
I ett automatiserat marknadsföring system inträffa skapa och aktivera hello marknadsföringskampanjer även du automatiskt. För detta ändamål - Azure Mobile Engagement kan skapa dessa automatiserade marknadsföringskampanjer via API: er samt. 

Kunder som använder vanligtvis hello Mobile Engagement klientdelen gränssnittet toocreate meddelanden/avsökningar etc som en del av deras marknadsföringskampanjer. Men som hello marknadsföringskampanjer blir mogen, krävs ett tooleverage hello data låst i hello serverdelssystem (till exempel alla CRM-system eller CMS-system som SharePoint) så att du kan skapa en helt automatiserad pipeline som skapar kampanjer i Mobile Engagement dynamiskt baserat på hello data flödar i från hello serverdelssystem. 

![][5]

Den här självstudien blir via ett scenario där SharePoint företagsanvändare fyller på en SharePoint-lista med marknadsföring data och en automatiserad process hämtar objekt från hello lista och ansluter med hello Mobile Engagement system hello tillgängliga REST API: er toocreate marknadsföringskampanj från hello SharePoint-data. 

> [!IMPORTANT]
> I allmänhet kan du använda det här exemplet som utgångspunkt för att förstå hur toocall Mobile Engagement REST API: er som det beskrivs hello två viktiga aspekter av anropar hello API- och autentiseras skickar parametrar. 
> 
> 

## <a name="sharepoint-integration"></a>SharePoint-integrering
1. Här är vad hello exempel SharePoint lista ser ut. **Rubrik**, **kategori**, **NotificationTitle**, **meddelandet** och **URL** används för att skapa hello-meddelande. Det finns en kolumn med namnet **IsProcessed** som används av hello exempel automation processen i hello form av ett konsolprogram. Du kan antingen köra detta konsolprogram som en Azure-Webbjobb så att du kan schemalägga det eller direkt använda hello SharePoint arbetsflöde tooprogram skapa och genom att aktivera hello-meddelande när ett objekt infogas i hello SharePoint-lista. I det här exemplet använder vi hello konsolprogram som går via hello objekt i hello SharePoint och skapa meddelande i Azure Mobile Engagement för dem och slutligen markerar hello **IsProcessed** flaggan toobe true i Skapa en lyckad meddelande.
   
    ![][1]
2. Vi använder hello kod från hello exempel *Remote Authentication i SharePoint Online med hjälp av hello klienten Object Model* [här](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) tooauthenticate med hello SharePoint-lista.
3. När autentiseringen är vi gå igenom hello lista objekt toofind bort alla objekt som nyligen skapade (som kommer att ha **IsProcessed** = false). 
   
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

## <a name="mobile-engagement-integration"></a>Mobile Engagement-integration
1. När det att hitta ett objekt som kräver bearbetning – vi extrahera hello information som krävs för toocreate ett meddelande från hello listobjekt och anropa `CreateAzMECampaign` toocreate den och därefter `ActivateAzMECampaign` tooactivate den. Dessa är i stort sett REST API-anrop som är anropar toohello Mobile Engagement-serverdelen. 
2. hello Mobile Engagement REST API: er kräver en **authorization HTTP-huvud för grundläggande autentisering schemat** som består av hello `ApplicationId` och hello `ApiKey` som du får från hello Azure-portalen. Kontrollera att du använder hello nyckeln från hello **api-nycklar** avsnitt och *inte* från hello **sdk nycklar** avsnitt. 
   
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
3. För att skapa hello-meddelande typen kampanj – Se toohello [dokumentationen](https://msdn.microsoft.com/library/azure/mt683750.aspx). Du behöver toomake till att du anger hello kampanj `kind` som *meddelande* och hello [nyttolast](https://msdn.microsoft.com/library/azure/mt683751.aspx) och passerar den som FormUrlEncodedContent. 
   
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
4. När du har skapat hello-meddelande, visas något som liknar hello följa på hello Mobile Engagement-portalen (Observera att hello tillstånd = utkast och aktiverad = ej tillämpligt)
   
    ![][3]
5. `CreateAzMECampaign`skapar ett meddelande kampanj och returnerar dess Id toohello anroparen. `ActivateAzMECampaign`kräver detta Id som hello parametern tooactivate hello kampanj. 
   
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
6. När du har aktiverat hello-meddelande, visas något som liknar följande hello på hello Mobile Engagement-portalen:
   
    ![][4]
7. Så snart hello kampanj hämtar aktiverat kan startar alla enheter som uppfyller kriteriet för hello hello kampanj ser meddelanden. 
8. Du ser också att hello listobjekt markerad med IsProcessed = false har ställts in tooTrue när hello-meddelande kampanjen skapades.  

Det här exemplet skapas en enkel meddelande kampanj att ange främst hello krävs egenskaper. Du kan anpassa så mycket som du kan från hello-portalen med hello information [här](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png



