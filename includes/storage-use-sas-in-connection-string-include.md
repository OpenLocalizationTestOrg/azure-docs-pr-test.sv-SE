<span data-ttu-id="20790-101">Om du har en URL för delad åtkomst signatur (SAS) som ger åtkomst till resurser i ett lagringskonto kan du använda SAS i en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="20790-101">If you possess a shared access signature (SAS) URL that grants you access to resources in a storage account, you can use the SAS in a connection string.</span></span> <span data-ttu-id="20790-102">Eftersom Signaturen innehåller information som krävs för att autentisera begäran, ger en anslutningssträng med en SAS protokollet, tjänstslutpunkten och autentiseringsuppgifterna som krävs för åtkomst till resursen.</span><span class="sxs-lookup"><span data-stu-id="20790-102">Because the SAS contains the information required to authenticate the request, a connection string with a SAS provides the protocol, the service endpoint, and the necessary credentials to access the resource.</span></span>

<span data-ttu-id="20790-103">Ange strängen du vill skapa en anslutningssträng som innehåller en signatur för delad åtkomst i följande format:</span><span class="sxs-lookup"><span data-stu-id="20790-103">To create a connection string that includes a shared access signature, specify the string in the following format:</span></span>

```
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
SharedAccessSignature=sasToken
```

<span data-ttu-id="20790-104">Varje tjänstslutpunkten är valfritt, även om anslutningssträngen måste innehålla minst ett.</span><span class="sxs-lookup"><span data-stu-id="20790-104">Each service endpoint is optional, although the connection string must contain at least one.</span></span>

> [!NOTE]
> <span data-ttu-id="20790-105">Du bör använda HTTPS med en SAS som bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="20790-105">Using HTTPS with a SAS is recommended as a best practice.</span></span>
>
> <span data-ttu-id="20790-106">Om du anger en SAS i en anslutningssträng i en konfigurationsfil, kan du behöva koda specialtecken i URL: en.</span><span class="sxs-lookup"><span data-stu-id="20790-106">If you are specifying a SAS in a connection string in a configuration file, you may need to encode special characters in the URL.</span></span>
>
>

### <a name="service-sas-example"></a><span data-ttu-id="20790-107">Tjänst-SAS-exempel</span><span class="sxs-lookup"><span data-stu-id="20790-107">Service SAS example</span></span>
<span data-ttu-id="20790-108">Här är ett exempel på en anslutningssträng som innehåller en tjänst-SAS för Blob storage:</span><span class="sxs-lookup"><span data-stu-id="20790-108">Here's an example of a connection string that includes a service SAS for Blob storage:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

<span data-ttu-id="20790-109">Och här är ett exempel på samma anslutningssträng kodning av specialtecken:</span><span class="sxs-lookup"><span data-stu-id="20790-109">And here's an example of the same connection string with encoding of special characters:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

### <a name="account-sas-example"></a><span data-ttu-id="20790-110">Kontot SAS-exempel</span><span class="sxs-lookup"><span data-stu-id="20790-110">Account SAS example</span></span>
<span data-ttu-id="20790-111">Här är ett exempel på en anslutningssträng som innehåller ett SAS-konto för Blob- och lagring.</span><span class="sxs-lookup"><span data-stu-id="20790-111">Here's an example of a connection string that includes an account SAS for Blob and File storage.</span></span> <span data-ttu-id="20790-112">Observera att slutpunkter för båda tjänsterna har angetts:</span><span class="sxs-lookup"><span data-stu-id="20790-112">Note that endpoints for both services are specified:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl
```

<span data-ttu-id="20790-113">Och här är ett exempel på samma anslutningssträng med URL-kodning:</span><span class="sxs-lookup"><span data-stu-id="20790-113">And here's an example of the same connection string with URL encoding:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl
```

