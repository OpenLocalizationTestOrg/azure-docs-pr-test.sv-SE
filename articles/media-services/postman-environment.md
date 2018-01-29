 ---
Rubrik: importera Postman miljön för Azure Media Services REST-anrop beskrivning: det här avsnittet innehåller en definition av Postman miljön för Azure Media Services REST-anrop.
tjänster: media services dokumentationcenter: '' författare: Juliako manager: cfowler editor: ''

MS.Service: media services ms.workload: media ms.tgt_pltfrm: na ms.devlang: na ms.topic: artikel ms.date: 2017-01/04 ms.author: juliako

---

# <a name="import-the-postman-environment"></a>Importera Postman-miljö 

Den här artikeln innehåller en definition av den **Postman** miljövariabler som används i [Postman samling](postman-collection.md) som innehåller grupperade HTTP-begäranden som anropar Media Services REST API: er. Miljö och samling filer som används av den [konfigurera Postman för Media Services REST API-anrop](media-rest-apis-with-postman.md) kursen.

```
{
  "id": "2dbce3ce-74c2-2ceb-0461-c4c2323f5b09",
  "name": "AzureMedia",
  "values": [
    {
      "enabled": true,
      "key": "TenantID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AzureADSTSEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "RESTAPIEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientSecret",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AccessToken",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAssetId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAccessPolicyId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "UploadURL",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "MediaFileName",
      "value": "BigBuckBunny.mp4",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastChannelId",
      "value": "",
      "type": "text"
    }
  ],
  "_postman_variable_scope": "environment",
  "_postman_exported_using": "Postman/5.5.0"
}
```
