---
title: REST-baserat program livscykel exempel | Microsoft Docs
description: "Ett Microsoft Azure Service Fabric-exempel som visar livscykeln för program med hjälp av Service Fabric REST-gränssnittet."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 0a374e53-ff23-4ee8-8cc6-259d41e118e7
ms.service: service-fabric
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/2/2016
ms.author: ryanwi
redirect_url: /rest/api/servicefabric/
ms.openlocfilehash: e0c744c4784deb2ce21abcb9b7e012a38b6d16a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="rest-based-application-lifecycle-sample"></a><span data-ttu-id="1b1b5-103">Exempel på REST-baserad livscykel för program</span><span class="sxs-lookup"><span data-stu-id="1b1b5-103">REST-based application lifecycle sample</span></span>
<span data-ttu-id="1b1b5-104">Det här exemplet visar livscykeln för Service Fabric-program via REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-104">This sample demonstrates the Service Fabric application lifecycle through REST API calls.</span></span> <span data-ttu-id="1b1b5-105">Mer information om livscykeln för Service Fabric-program finns [Service Fabric application livscykel](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="1b1b5-105">For more information on the Service Fabric application lifecycle, see [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

<span data-ttu-id="1b1b5-106">Det här exemplet gör följande:</span><span class="sxs-lookup"><span data-stu-id="1b1b5-106">This sample performs the following:</span></span>

* <span data-ttu-id="1b1b5-107">Tillhandahåller den **WordCount 1.0.0** exempel från WordCount-programpaket i image store.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-107">Provisions the **WordCount 1.0.0** sample from the WordCount application package in the image store.</span></span>
* <span data-ttu-id="1b1b5-108">Visar listan över programtyper, som innehåller WordCount 1.0.0.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-108">Displays the list of application types, which includes WordCount 1.0.0.</span></span>
* <span data-ttu-id="1b1b5-109">Skapar WordCount-program som **fabric: / WordCount**.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-109">Creates the WordCount application as **fabric:/WordCount**.</span></span>
* <span data-ttu-id="1b1b5-110">Visar en lista med program, vilket innefattar fabric: / WordCount version 1.0.0.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-110">Displays the list of applications, which includes fabric:/WordCount version 1.0.0.</span></span>
* <span data-ttu-id="1b1b5-111">Tillhandahåller 1.1.0 versionen av WordCount-exemplet från den **WordCountUpgrade** programpaket i image store.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-111">Provisions the 1.1.0 version of the WordCount sample from the **WordCountUpgrade** application package in the image store.</span></span>
* <span data-ttu-id="1b1b5-112">Visar en lista över programtyper av som innehåller både WordCount 1.0.0 och **WordCount 1.1.0**.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-112">Displays the list of application types, which includes both WordCount 1.0.0 and **WordCount 1.1.0**.</span></span>
* <span data-ttu-id="1b1b5-113">Uppgraderar WordCount-programmet till version 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-113">Upgrades the WordCount application to version 1.1.0.</span></span>
* <span data-ttu-id="1b1b5-114">Visar listan över program, vilket inkluderar WordCount version 1.1.0, men innehåller inte längre WordCount version 1.0.0.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-114">Displays the list of applications, which includes WordCount version 1.1.0, but no longer includes WordCount version 1.0.0.</span></span>
* <span data-ttu-id="1b1b5-115">Tar bort WordCount-programmet.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-115">Deletes the WordCount application.</span></span>
* <span data-ttu-id="1b1b5-116">Visar en lista över program som inte längre innehåller fabric: / WordCount.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-116">Displays the list of applications, which no longer includes fabric:/WordCount.</span></span>
* <span data-ttu-id="1b1b5-117">Unprovisions 1.1.0 version WordCount-exemplet.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-117">Unprovisions the 1.1.0 version of the WordCount sample.</span></span>
* <span data-ttu-id="1b1b5-118">Visar listan över programtyper, vilket inkluderar WordCount 1.0.0, men innehåller inte längre WordCount 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-118">Displays the list of application types, which includes WordCount 1.0.0, but no longer includes WordCount 1.1.0.</span></span>
* <span data-ttu-id="1b1b5-119">Unprovisions 1.0.0 version WordCount-exemplet.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-119">Unprovisions the 1.0.0 version of the WordCount sample.</span></span>
* <span data-ttu-id="1b1b5-120">Visar listan över programtyper, som inte längre innehåller WordCount.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-120">Displays the list of application types, which no longer includes WordCount.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1b1b5-121">Krav</span><span class="sxs-lookup"><span data-stu-id="1b1b5-121">Prerequisites</span></span>
<span data-ttu-id="1b1b5-122">Det här exemplet använder den [WordCount-exemplet](http://aka.ms/servicefabricsamples) (hittas i den **komma igång** exempel).</span><span class="sxs-lookup"><span data-stu-id="1b1b5-122">This sample uses the [WordCount sample](http://aka.ms/servicefabricsamples) (found in the **Getting Started** samples).</span></span> <span data-ttu-id="1b1b5-123">WordCount-exemplet måste skapas först och sedan två programpaket måste kopieras till image store.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-123">The WordCount sample must be built first, and then two application packages must be copied to the image store.</span></span>

| <span data-ttu-id="1b1b5-124">Mapp</span><span class="sxs-lookup"><span data-stu-id="1b1b5-124">Folder</span></span> | <span data-ttu-id="1b1b5-125">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1b1b5-125">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1b1b5-126">WordCount</span><span class="sxs-lookup"><span data-stu-id="1b1b5-126">WordCount</span></span> |<span data-ttu-id="1b1b5-127">WordCount exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-127">The WordCount sample application.</span></span> <span data-ttu-id="1b1b5-128">Den **ApplicationManifest.xml** filen innehåller **ApplicationTypeVersion = ”1.0.0”**.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-128">The **ApplicationManifest.xml** file contains **ApplicationTypeVersion="1.0.0"**.</span></span> |
| <span data-ttu-id="1b1b5-129">WordCountUpgrade</span><span class="sxs-lookup"><span data-stu-id="1b1b5-129">WordCountUpgrade</span></span> |<span data-ttu-id="1b1b5-130">WordCount exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-130">The WordCount sample application.</span></span> <span data-ttu-id="1b1b5-131">Filen ApplicationManifest.xml måste ändras till **ApplicationTypeVersion = ”1.1.0”** så att programmet uppgraderingen ska ske.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-131">The ApplicationManifest.xml file must be changed to **ApplicationTypeVersion="1.1.0"** to allow the application upgrade to occur.</span></span> |

<span data-ttu-id="1b1b5-132">Om du vill skapa programpaketen och kopiera dem till image store, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="1b1b5-132">To create the application packages and copy them to the image store, take the following steps:</span></span>

1. <span data-ttu-id="1b1b5-133">Kopiera **C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug** till **C:\Temp\WordCount**.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-133">Copy **C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug** to **C:\Temp\WordCount**.</span></span> <span data-ttu-id="1b1b5-134">Detta skapar WordCount-programpaket.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-134">This creates the WordCount application package.</span></span>
2. <span data-ttu-id="1b1b5-135">Kopiera C:\Temp\WordCount till **C:\Temp\WordCountUpgrade**.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-135">Copy C:\Temp\WordCount to **C:\Temp\WordCountUpgrade**.</span></span> <span data-ttu-id="1b1b5-136">Detta skapar den **WordCountUpgrade programmet** paketet.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-136">This creates the **WordCountUpgrade application** package.</span></span>
3. <span data-ttu-id="1b1b5-137">Öppna **C:\Temp\WordCountUpgrade\ApplicationManifest.xml** i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-137">Open **C:\Temp\WordCountUpgrade\ApplicationManifest.xml** in a text editor.</span></span>
4. <span data-ttu-id="1b1b5-138">I den **ApplicationManifest** element, ändring av **ApplicationTypeVersion** attributet **”1.1.0”**.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-138">In the **ApplicationManifest** element, change the **ApplicationTypeVersion** attribute to **"1.1.0"**.</span></span>  <span data-ttu-id="1b1b5-139">Detta uppdaterar versionsnumret för programmet.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-139">This updates the version number of the application.</span></span>
5. <span data-ttu-id="1b1b5-140">Spara filen ändrade ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-140">Save the changed ApplicationManifest.xml file.</span></span>
6. <span data-ttu-id="1b1b5-141">Kör följande PowerShell-skript som en administratör för att kopiera program till image store:</span><span class="sxs-lookup"><span data-stu-id="1b1b5-141">Run the following PowerShell script as an administrator to copy the applications to the image store:</span></span>

```powershell
# Deploy the WordCount and upgrade applications
$applicationPathWordCount = "C:\Temp\WordCount"
$applicationPathUpgrade = "C:\Temp\WordCountUpgrade"

# LOCAL:
$imageStoreConnection = "file:C:\SfDevCluster\Data\ImageStoreShare"
$cluster = 'localhost:19000'

Connect-ServiceFabricCluster $cluster

Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $applicationPathWordCount -ImageStoreConnectionString $imageStoreConnection
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $applicationPathUpgrade -ImageStoreConnectionString $imageStoreConnection
```

<span data-ttu-id="1b1b5-142">Det här programmet är redo att köras när PowerShell-skriptet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-142">When the PowerShell script finishes, this application is ready to run.</span></span>

## <a name="example"></a><span data-ttu-id="1b1b5-143">Exempel</span><span class="sxs-lookup"><span data-stu-id="1b1b5-143">Example</span></span>
<span data-ttu-id="1b1b5-144">Exemplet nedan visar livscykeln för Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="1b1b5-144">The following example demonstrates the Service Fabric application lifecycle.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Fabric.Health;
using System.Fabric.Query;
using System.IO;
using System.Net;
using System.Text;
using System.Web.Script.Serialization;

namespace ServiceFabricRestCaller
{
    class Program
    {
        static void Main(string[] args)
        {
            Uri clusterUri = new Uri("http://localhost:19080");
            string buildPathApplication = "WordCount";
            string applicationVersionNumber = "1.0.0";
            string buildPathUpgrade = "WordCountUpgrade";
            string updateVersionNumber = "1.1.0";

            Console.WriteLine("\nProvision the 1.0.0 WordCount application for the first time.");
            ProvisionAnApplication(clusterUri, buildPathApplication);
            Console.WriteLine("\nPress Enter to get the list of application types: ");
            Console.ReadLine();


            Console.WriteLine("\nGet the list of application types.");
            GetListOfApplicationTypes(clusterUri);
            Console.WriteLine("\nPress Enter to create the fabric:/WordCount application: ");
            Console.ReadLine();


            Console.WriteLine("\nCreate the fabric:/WordCount application.");
            CreateApplication(clusterUri);
            Console.WriteLine("\nPress Enter to get the list of applications: ");
            Console.ReadLine();


            Console.WriteLine("\nGet the list of applications.");
            GetApplicationList(clusterUri);
            Console.WriteLine("\nPress Enter to provision the 1.1.0 upgrade to the WordCount application: ");
            Console.ReadLine();


            Console.WriteLine("\nProvision the 1.1.0 upgrade to the WordCount application.");
            ProvisionAnApplication(clusterUri, buildPathUpgrade);
            Console.WriteLine("\nPress Enter to get the list of application types: ");
            Console.ReadLine();


            Console.WriteLine("\nGet the list of application types.");
            GetListOfApplicationTypes(clusterUri);
            Console.WriteLine("\nPress Enter to upgrade the fabric:/WordCount application: ");
            Console.ReadLine();


            Console.WriteLine("\nUpgrade the fabric:/WordCount application.");
            UpgradeApplicationByApplicationType(clusterUri);
            Console.WriteLine("\nPress Enter to get the list of applications: ");
            Console.ReadLine();


            Console.WriteLine("\nGet the list of applications.");
            GetApplicationList(clusterUri);
            Console.WriteLine("\nPress Enter to delete the fabric:/WordCount application: ");
            Console.ReadLine();


            Console.WriteLine("\nDelete the fabric:/WordCount application.");
            DeleteApplication(clusterUri);
            Console.WriteLine("\nPress Enter to get the list of applications: ");
            Console.ReadLine();


            Console.WriteLine("\nGet the list of applications.");
            GetApplicationList(clusterUri);
            Console.WriteLine("\nPress Enter to unprovision the WordCount 1.1.0 application: ");
            Console.ReadLine();


            Console.WriteLine("\nUnprovision the WordCount 1.1.0 application.");
            UnprovisionAnApplication(clusterUri, updateVersionNumber);
            Console.WriteLine("\nPress Enter to get the list of application types: ");
            Console.ReadLine();


            Console.WriteLine("\nGet the list of application types.");
            GetListOfApplicationTypes(clusterUri);
            Console.WriteLine("\nPress Enter to unprovision the WordCount 1.0.0 application: ");
            Console.ReadLine();


            Console.WriteLine("\nUnprovision the WordCount 1.0.0 application.");
            UnprovisionAnApplication(clusterUri, applicationVersionNumber);
            Console.WriteLine("\nPress Enter to get the final list of application types: ");
            Console.ReadLine();


            Console.WriteLine("\nGet the final list of application types.");
            GetListOfApplicationTypes(clusterUri);
            Console.WriteLine("\nPress Enter to end this program: ");
            Console.ReadLine();
        }

        #region Classes

        /// <summary>
        /// Class similar to ApplicationType. Designed for use with JavaScriptSerializer.
        /// </summary>
        public class AppType
        {
            public string Name { get; set; }
            public string Version { get; set; }
            public List<ApplicationParameter> DefaultParameterList { get; set; }
        }

        /// <summary>
        /// Class designed for use with JavaScriptSerializer.
        /// </summary>
        public class ApplicationInfo
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string TypeName { get; set; }
            public string TypeVersion { get; set; }
            public ApplicationStatus Status { get; set; }
            public List<Parameter> Parameters { get; set; }
            public HealthState HealthState { get; set; }
        }

        /// <summary>
        /// Class similar to Parameter. Designed for use with JavaScriptSerializer.
        /// </summary>
        public class Parameter
        {
            public string Name { get; set; }
            public string Value { get; set; }
        }

        #endregion


        #region Get List of Application Types (REST API)

        /// <summary>
        /// Gets the list of application types.
        /// </summary>
        /// <param name="clusterUri">The URI to access the cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool GetListOfApplicationTypes(Uri clusterUri)
        {
            // String to capture the response stream.
            string responseString = string.Empty;

            // Create the request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/ApplicationTypes?api-version={0}",
            "1.0"));    // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.Method = "GET";

            // Execute the request and obtain the response.
            try
            {
                using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                {
                    using (StreamReader streamReader = new StreamReader(response.GetResponseStream(), true))
                    {
                        // Capture the response string.
                        responseString = streamReader.ReadToEnd();
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display the error message.
                Console.WriteLine("Error getting the list of application types:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            // Deserialize the response string.
            JavaScriptSerializer jss = new JavaScriptSerializer();
            List<AppType> applicationTypes = jss.Deserialize<List<AppType>>(responseString);

            // Display application type information for each application type.
            Console.WriteLine("Application types:");
            foreach (AppType applicationType in applicationTypes)
            {
                Console.WriteLine("  Application Type:");
                Console.WriteLine("    Name: " + applicationType.Name);
                Console.WriteLine("    Version: " + applicationType.Version);
                Console.WriteLine("    Default Parameter List:");

                foreach (var parameter in applicationType.DefaultParameterList)
                {
                    Console.WriteLine("      Name: " + parameter.Name);
                    Console.WriteLine("      Value: " + parameter.Value);
                }
            }

            return true;
        }

        #endregion


        #region Provision an Application (REST API)

        /// <summary>
        /// Provisions an application to the image store.
        /// </summary>
        /// <param name="clusterUri">The URI to access the cluster.</param>
        /// <param name="applicationTypeBuildPath">The application type build path ("WordCount" or "WordCountUpgrade").</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool ProvisionAnApplication(Uri clusterUri, string applicationTypeBuildPath)
        {
            // Create the request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/ApplicationTypes/$/Provision?api-version={0}",
                "1.0"));    // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.Method = "POST";
            request.ContentType = "application/json; charset=utf-8";

            // Create the byte array that will become the request body.
            string requestBody = "{\"ApplicationTypeBuildPath\":\"" + applicationTypeBuildPath + "\"}";
            byte[] requestBodyBytes = Encoding.UTF8.GetBytes(requestBody);
            request.ContentLength = requestBodyBytes.Length;

            // Stores the response status code.
            HttpStatusCode statusCode;

            // Create the request body.
            try
            {
                using (Stream requestStream = request.GetRequestStream())
                {
                    requestStream.Write(requestBodyBytes, 0, requestBodyBytes.Length);
                    requestStream.Close();

                    // Execute the request and obtain the response.
                    using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                    {
                        statusCode = response.StatusCode;
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display the error message.
                Console.WriteLine("Error provisioning the application:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            Console.WriteLine("Provision an Application response status = " + statusCode.ToString());
            return true;
        }

        #endregion

        #region Unprovision an Application (REST API)

        /// <summary>
        /// Unprovisions an application.
        /// </summary>
        /// <param name="clusterUri">The URI to access the cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool UnprovisionAnApplication(Uri clusterUri, string versionToUnprovision)
        {
            // Create the request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/ApplicationTypes/{0}/$/Unprovision?api-version={1}",
                "WordCount",     // Application Type Name
                "1.0"));            // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.Method = "POST";
            request.ContentType = "application/json; charset=utf-8";

            // Stores the response status code.
            HttpStatusCode statusCode;

            // Create the byte array that will become the request body.
            string requestBody = "{\"ApplicationTypeVersion\":\"" + versionToUnprovision + "\"}";
            byte[] requestBodyBytes = Encoding.UTF8.GetBytes(requestBody);
            request.ContentLength = requestBodyBytes.Length;

            // Create the request body.
            try
            {
                using (Stream requestStream = request.GetRequestStream())
                {
                    requestStream.Write(requestBodyBytes, 0, requestBodyBytes.Length);
                    requestStream.Close();

                    // Execute the request and obtain the response.
                    using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                    {
                        statusCode = response.StatusCode;
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display the error message.
                Console.WriteLine("Error unprovisioning the application:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            Console.WriteLine("Unprovision an Application response status = " + statusCode.ToString());
            return true;
        }

        #endregion

        #region Get Application List (REST API)

        /// <summary>
        /// Gets the list of applications.
        /// </summary>
        /// <param name="clusterUri">The URI to access the cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool GetApplicationList(Uri clusterUri)
        {
            // String to capture the response stream.
            string responseString = string.Empty;

            // Create the request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/Applications?api-version={0}",
                "1.0")); // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.Method = "GET";

            // Execute the request and obtain the response.
            try
            {
                using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                {
                    using (StreamReader streamReader = new StreamReader(response.GetResponseStream(), true))
                    {
                        // Capture the response string.
                        responseString = streamReader.ReadToEnd();
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display the error message.
                Console.WriteLine("Error getting the application list:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }


            // Deserialize the response string.
            JavaScriptSerializer jss = new JavaScriptSerializer();
            List<ApplicationInfo> applicationInfos = jss.Deserialize<List<ApplicationInfo>>(responseString);

            // Display some application information for each application.
            Console.WriteLine("Application List:");
            foreach (ApplicationInfo applicationInfo in applicationInfos)
            {
                Console.WriteLine("  Application:");
                Console.WriteLine("    Id: " + applicationInfo.Id);
                Console.WriteLine("    Name: " + applicationInfo.Name);
                Console.WriteLine("    TypeName: " + applicationInfo.TypeName);
                Console.WriteLine("    TypeVersion: " + applicationInfo.TypeVersion);
                Console.WriteLine("    Status: " + applicationInfo.Status);
                Console.WriteLine("    HealthState: " + applicationInfo.HealthState);

                Console.WriteLine("    Parameters:");

                foreach (Parameter parameter in applicationInfo.Parameters)
                {
                    Console.WriteLine("      Parameter:");
                    Console.WriteLine("        Name: " + parameter.Name);
                    Console.WriteLine("        Value: " + parameter.Value);
                }
            }

            return true;
        }

        #endregion

        #region Create Application (REST API)

        /// <summary>
        /// Creates an application.
        /// </summary>
        /// <param name="clusterUri">The URI to access the cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool CreateApplication(Uri clusterUri)
        {
            // String to capture the response stream.
            string responseString = string.Empty;

            // Stores the response status code.
            HttpStatusCode statusCode;

            // Create the request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/Applications/$/Create?api-version={0}",
                "1.0"));    // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.ContentType = "text/json";
            request.Method = "POST";

            // Create the byte array that will become the request body.
            string requestBody = "{\"Name\":\"fabric:/WordCount\"," +
                                    "\"TypeName\":\"WordCount\"," +
                                    "\"TypeVersion\":\"1.0.0\"," +
                                    "\"ParameterList\":[]}";
            byte[] requestBodyBytes = Encoding.UTF8.GetBytes(requestBody);
            request.ContentLength = requestBodyBytes.Length;

            // Create the request body.
            try
            {
                using (Stream requestStream = request.GetRequestStream())
                {
                    requestStream.Write(requestBodyBytes, 0, requestBodyBytes.Length);
                    requestStream.Close();

                    // Execute the request and obtain the response.
                    using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                    {
                        statusCode = response.StatusCode;
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display the error message.
                Console.WriteLine("Error creating application:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            Console.WriteLine("Create Application response status = " + statusCode.ToString());

            return true;
        }

        #endregion


        #region Delete Application (REST API)

        /// <summary>
        /// Deletes an application.
        /// </summary>
        /// <param name="clusterUri">The URI to access the cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool DeleteApplication(Uri clusterUri)
        {
            // Create the request and add URL parameters.
            Uri requestUri = new Uri(clusterUri,
                string.Format("/Applications/{0}/$/Delete?api-version={1}",
                "WordCount",    // Application Name
                "1.0"));        // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.Method = "POST";
            request.ContentLength = 0;

            // Stores the response status code.
            HttpStatusCode statusCode;

            // Execute the request and obtain the response.
            try
            {
                using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                {
                    statusCode = response.StatusCode;
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display the error message.
                Console.WriteLine("Error deleting application:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            Console.WriteLine("Delete Application response status = " + statusCode.ToString());

            return true;
        }

        #endregion

        #region Upgrade Application by Application Type (REST API)

        /// <summary>
        /// Upgrades an application by application type.
        /// </summary>
        /// <param name="clusterUri">The URI to access the cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool UpgradeApplicationByApplicationType(Uri clusterUri)
        {
            // String to capture the response stream.
            string responseString = string.Empty;

            // Stores the response status code.
            HttpStatusCode statusCode;

            // Create the request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/Applications/{0}/$/Upgrade?api-version={1}",
                "WordCount",     // Application Name
                "1.0"));                // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.ContentType = "text/json";
            request.Method = "POST";


            // Create the Health Policy.
            string requestBody = "{\"Name\":\"fabric:/WordCount\"," +
                                    "\"TargetApplicationTypeVersion\":\"1.1.0\"," +
                                    "\"Parameters\":[]," +
                                    "\"UpgradeKind\":1," +
                                    "\"RollingUpgradeMode\":1," +
                                    "\"UpgradeReplicaSetCheckTimeoutInSeconds\":5," +
                                    "\"ForceRestart\":true," +
                                    "\"MonitoringPolicy\":" +
                                    "{\"FailureAction\":1," +
                                    "\"HealthCheckWaitDurationInMilliseconds\":\"5000\"," +
                                    "\"HealthCheckStableDurationInMilliseconds\":\"10000\"," +
                                    "\"HealthCheckRetryTimeoutInMilliseconds\":\"20000\"," +
                                    "\"UpgradeTimeoutInMilliseconds\":\"60000\"," +
                                    "\"UpgradeDomainTimeoutInMilliseconds\":\"30000\"}}";

            // Create the byte array that will become the request body.
            byte[] requestBodyBytes = Encoding.UTF8.GetBytes(requestBody);
            request.ContentLength = requestBodyBytes.Length;

            // Create the request body.
            try
            {
                using (Stream requestStream = request.GetRequestStream())
                {
                    requestStream.Write(requestBodyBytes, 0, requestBodyBytes.Length);
                    requestStream.Close();

                    // Execute the request and obtain the response.
                    using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                    {
                        statusCode = response.StatusCode;
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display the error message.
                Console.WriteLine("Error upgrading application:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            Console.WriteLine("Update Application response status = " + statusCode.ToString());

            return true;
        }

        #endregion
    }
}
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="1b1b5-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1b1b5-145">Next steps</span></span>
[<span data-ttu-id="1b1b5-146">Livscykel för Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="1b1b5-146">Service Fabric application lifecycle</span></span>](service-fabric-application-lifecycle.md)

