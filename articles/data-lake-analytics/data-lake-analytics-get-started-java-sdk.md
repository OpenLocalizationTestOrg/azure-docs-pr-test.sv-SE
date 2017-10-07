---
title: aaaUse Data Lake Analytics Java SDK toodevelop program | Microsoft Docs
description: "Använda Azure Data Lake Analytics Java SDK toodevelop program"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 07830b36-2fe3-4809-a846-129cf67b6a9e
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 0d975812fe659ed34ee9befd37ee7c0bf50d3414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-java-sdk"></a><span data-ttu-id="bc5ed-103">Kom igång med Azure Data Lake Analytics med hjälp av Java SDK</span><span class="sxs-lookup"><span data-stu-id="bc5ed-103">Get started with Azure Data Lake Analytics using Java SDK</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="bc5ed-104">Lär dig hur hello toouse Azure Data Lake Analytics Java SDK toocreate ett Azure Data Lake-konto och utföra grundläggande åtgärder som att skapa mappar, ladda upp och hämta datafiler, ta bort ditt konto, och arbetar med jobb.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-104">Learn how toouse hello Azure Data Lake Analytics Java SDK toocreate an Azure Data Lake account and perform basic operations such as create folders, upload and download data files, delete your account, and work with jobs.</span></span> <span data-ttu-id="bc5ed-105">Mer information om Data Lake finns [Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bc5ed-105">For more information about Data Lake, see [Azure Data Lake Analytics](data-lake-analytics-overview.md).</span></span>

<span data-ttu-id="bc5ed-106">I de här självstudierna utvecklar du ett Java-konsolprogram som innehåller exempel på vanliga administrativa uppgifter som att skapa testdata och skickar ett jobb.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-106">In this tutorial, you will develop a Java console application which contains samples of common administrative tasks as well as creating test data and submitting a job.</span></span>  <span data-ttu-id="bc5ed-107">toogo via hello stöds samma självstudier med andra verktyg, klicka på hello flikar på hello upp i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-107">toogo through hello same tutorial using other supported tools, click hello tabs on hello top of this section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc5ed-108">Krav</span><span class="sxs-lookup"><span data-stu-id="bc5ed-108">Prerequisites</span></span>
* <span data-ttu-id="bc5ed-109">Java Development Kit (JDK) 8 (med Java version 1.8).</span><span class="sxs-lookup"><span data-stu-id="bc5ed-109">Java Development Kit (JDK) 8 (using Java version 1.8).</span></span>
* <span data-ttu-id="bc5ed-110">IntelliJ eller en annan lämplig Java Development Environment.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-110">IntelliJ or another suitable Java development environment.</span></span> <span data-ttu-id="bc5ed-111">Det här är valfritt men rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-111">This is optional but recommended.</span></span> <span data-ttu-id="bc5ed-112">hello instruktionerna nedan använder IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-112">hello instructions below use IntelliJ.</span></span>
* <span data-ttu-id="bc5ed-113">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-113">**An Azure subscription**.</span></span> <span data-ttu-id="bc5ed-114">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bc5ed-114">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="bc5ed-115">Skapa ett program med Azure Active Directory (AAD) och hämta dess **klient-ID**, **innehavar-ID** och **nyckel**.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-115">Create an Azure Active Directory (AAD) application and retrieve its **Client ID**, **Tenant ID**, and **Key**.</span></span> <span data-ttu-id="bc5ed-116">Mer information om AAD-program och anvisningar om hur tooget klient-ID finns [Skapa Active Directory-program och tjänstens huvudnamn med hjälp av portalen](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bc5ed-116">For more information about AAD applications and instructions on how tooget a client ID, see [Create Active Directory application and service principal using portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="bc5ed-117">hello Reply URI och nyckel också vara tillgänglig från hello portal när du har hello program skapas och nyckeln genereras.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-117">hello Reply URI and Key will also be available from hello portal once you have hello application created and key generated.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="bc5ed-118">Hur autentiserar jag med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bc5ed-118">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="bc5ed-119">hello kodfragmentet nedan innehåller koden för **icke-interaktiv** autentisering, där hello programmet tillhandahåller egna autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-119">hello code snippet below provides code for **non-interactive** authentication, where hello application provides its own credentials.</span></span>

<span data-ttu-id="bc5ed-120">Du behöver toogive program behörighet toocreate resurser i Azure för den här självstudiekursen toowork.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-120">You will need toogive your application permission toocreate resources in Azure for this tutorial toowork.</span></span> <span data-ttu-id="bc5ed-121">Det är **rekommenderas starkt** att du endast ger det här programmet deltagare behörigheter tooa ny, oanvänd och tom resursgrupp i Azure-prenumerationen för hello syftet med den här kursen.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-121">It is **highly recommended** that you only give this application Contributor permissions tooa new, unused, and empty resource group in your Azure subscription for hello purposes of this tutorial.</span></span>

## <a name="create-a-java-application"></a><span data-ttu-id="bc5ed-122">Skapa ett Java-program</span><span class="sxs-lookup"><span data-stu-id="bc5ed-122">Create a Java application</span></span>
1. <span data-ttu-id="bc5ed-123">Öppna IntelliJ och skapa ett nytt Java-projekt med hello **Kommandoradsapp** mall.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-123">Open IntelliJ and create a new Java project using hello **Command Line App** template.</span></span>
2. <span data-ttu-id="bc5ed-124">Högerklicka på projektet hello hello vänster på skärmen och klicka på **lägga till stöd för Framework**.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-124">Right-click on hello project on hello left-hand side of your screen and click **Add Framework Support**.</span></span> <span data-ttu-id="bc5ed-125">Välj **Maven** och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-125">Choose **Maven** and click **OK**.</span></span>
3. <span data-ttu-id="bc5ed-126">Öppna hello nyskapad **”pom.xml”** och Lägg till följande fragment av text mellan hello hello  **\</version >** taggen och hello  **\< /project >** tagg:</span><span class="sxs-lookup"><span data-stu-id="bc5ed-126">Open hello newly created **"pom.xml"** file and add hello following snippet of text between hello **\</version>** tag and hello **\</project>** tag:</span></span>

    >[!NOTE]
    ><span data-ttu-id="bc5ed-127">Det här steget är tillfällig tills hello Azure Data Lake Analytics SDK är tillgänglig i Maven.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-127">This step is temporary until hello Azure Data Lake Analytics SDK is available in Maven.</span></span> <span data-ttu-id="bc5ed-128">Den här artikeln kommer att uppdateras när hello SDK är tillgänglig i Maven.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-128">This article will be updated once hello SDK is available in Maven.</span></span> <span data-ttu-id="bc5ed-129">Alla framtida uppdateringar toothis SDK blir tillgängliga via Maven.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-129">All future updates toothis SDK will be availble through Maven.</span></span>
    >

        <repositories>
            <repository>
                <id>adx-snapshots</id>
                <name>Azure ADX Snapshots</name>
                <url>http://adxsnapshots.azurewebsites.net/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                </snapshots>
            </repository>
            <repository>
                <id>oss-snapshots</id>
                <name>Open Source Snapshots</name>
                <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                    <updatePolicy>always</updatePolicy>
                </snapshots>
            </repository>
        </repositories>
        <dependencies>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-authentication</artifactId>
                <version>1.0.0-20160513.000802-24</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-runtime</artifactId>
                <version>1.0.0-20160513.000812-28</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.rest</groupId>
                <artifactId>client-runtime</artifactId>
                <version>1.0.0-20160513.000825-29</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-store</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-analytics</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
        </dependencies>
4. <span data-ttu-id="bc5ed-130">Gå för**filen**, sedan **inställningar**, sedan **skapa**, **körning**, **distribution**.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-130">Go too**File**, then **Settings**, then **Build**, **Execution**, **Deployment**.</span></span> <span data-ttu-id="bc5ed-131">Välj **Byggverktygen**, **Maven**, **importera**.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-131">Select **Build Tools**, **Maven**, **Importing**.</span></span> <span data-ttu-id="bc5ed-132">Kontrollera sedan **importera Maven-projekt automatiskt**.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-132">Then check **Import Maven projects automatically**.</span></span>
5. <span data-ttu-id="bc5ed-133">Öppna **Main.java** och Ersätt hello befintliga kodblocket med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-133">Open **Main.java** and replace hello existing code block with hello following code.</span></span> <span data-ttu-id="bc5ed-134">Dessutom ange hello värden för parametrarna som påpekas i kodfragmentet hello som **localFolderPath**, **_adlaAccountName**, **_adlsAccountName**, **_ resourceGroupName** och Ersätt platshållarna för **klient-ID**, **CLIENT-SECRET**, **klient-ID**, och  **PRENUMERATIONS-ID**.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-134">Also, provide hello values for parameters called out in hello code snippet, such as **localFolderPath**, **_adlaAccountName**, **_adlsAccountName**, **_resourceGroupName** and replace placeholders for **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID**, and **SUBSCRIPTION-ID**.</span></span>

    <span data-ttu-id="bc5ed-135">Den här koden går via hello processen att skapa Data Lake Store och Data Lake Analytics-konton, skapa filer i hello store kör ett jobb, hämtar jobbstatus, hämta jobbutdata och tar slutligen bort hello-konto.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-135">This code goes through hello process of creating Data Lake Store and Data Lake Analytics accounts, creating files in hello store, running a job, getting job status, downloading job output, and finally deleting hello account.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bc5ed-136">Det finns ett känt problem med hello Azure Data Lake-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-136">There is currently a known issue with hello Azure Data Lake Service.</span></span>  <span data-ttu-id="bc5ed-137">Om hello exempelapp avbryts eller påträffar ett fel kan behöva toomanually delete hello Data Lake Store & Data Lake Analytics-konton som hello skriptet skapar.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-137">If hello sample app is interrupted or encounters an error, you may need toomanually delete hello Data Lake Store & Data Lake Analytics accounts that hello script creates.</span></span>  <span data-ttu-id="bc5ed-138">Om du inte är bekant med hello Portal hello [hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md) guide hjälper dig att komma igång.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-138">If you're not familiar with hello Portal, hello [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) guide will get you started.</span></span>
   >
   >

        package com.company;

        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.*;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.azure.management.datalake.analytics.*;
        import com.microsoft.azure.management.datalake.analytics.models.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.nio.file.Files;
        import java.nio.file.Paths;
        import java.util.ArrayList;
        import java.util.UUID;
        import java.util.List;

        public class Main {
            private static String _adlsAccountName;
            private static String _adlaAccountName;
            private static String _resourceGroupName;
            private static String _location;

            private static String _tenantId;
            private static String _subId;
            private static String _clientId;
            private static String _clientSecret;

            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
            private static DataLakeAnalyticsCatalogManagementClient _adlaCatalogClient;

            public static void main(String[] args) throws Exception {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>";
                _adlaAccountName = "<DATA-LAKE-ANALYTICS-NAME>";
                _resourceGroupName = "<RESOURCE-GROUP-NAME>";
                _location = "East US 2";

                _tenantId = "<TENANT-ID>";
                _subId =  "<SUBSCRIPTION-ID>";
                _clientId = "<CLIENT-ID>";

                _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring hello application client secret, rather than hard-coding it in hello source code.

                String localFolderPath = "C:\\local_path\\"; // TODO: Change this tooany unused, new, empty folder on your local machine.

                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
                SetupClients(creds);

                // Create Data Lake Store and Analytics accounts
                WaitForNewline("Authenticated.", "Creating NEW accounts.");
                CreateAccounts();
                WaitForNewline("Accounts created.", "Displaying accounts.");

                // List Data Lake Store and Analytics accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", _subId));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.getAccountOperations().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.getName());
                }
                System.out.println(String.format("All ADL Analytics accounts that this app can access in subscription %s:", _subId));
                List<DataLakeAnalyticsAccount> adlaListResult = _adlaClient.getAccountOperations().list().getBody();
                for (DataLakeAnalyticsAccount acct : adlaListResult) {
                    System.out.println(acct.getName());
                }
                WaitForNewline("Accounts displayed.", "Creating files.");

                // Create a file in Data Lake Store: input1.csv
                // TODO: these change order in hello next patch
                byte[] bytesContents = "123,abc".getBytes();
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, "/input1.csv", bytesContents, true);

                WaitForNewline("File created.", "Submitting a job.");

                // Submit a job tooData Lake Analytics
                UUID jobId = SubmitJobByScript("@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input too@\"/output1.csv\" USING Outputters.Csv();", "testJob");
                WaitForNewline("Job submitted.", "Getting job status.");

                // Wait for job completion and output job status
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                System.out.println("Waiting for job completion.");
                WaitForJob(jobId);
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                WaitForNewline("Job completed.", "Downloading job output.");

                // Download job output from Data Lake Store
                DownloadFile("/output1.csv", localFolderPath + "output1.csv");
                WaitForNewline("Job output downloaded.", "Deleting file.");

                // Delete file from Data Lake Store
                DeleteFile("/output1.csv");
                WaitForNewline("File deleted.", "Deleting account.");

                // Delete account
                _adlsClient.getAccountOperations().delete(_resourceGroupName, _adlsAccountName);
                _adlaClient.getAccountOperations().delete(_resourceGroupName, _adlaAccountName);
                WaitForNewline("Account deleted.", "DONE.");
            }

            //Set up clients
            public static void SetupClients(ServiceClientCredentials creds)
            {
                _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
                _adlaClient = new DataLakeAnalyticsAccountManagementClientImpl(creds);
                _adlaJobClient = new DataLakeAnalyticsJobManagementClientImpl(creds);
                _adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClientImpl(creds);
                _adlsClient.setSubscriptionId(_subId);
                _adlaClient.setSubscriptionId(_subId);
            }

            // Helper function tooshow status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";

                System.out.println(reason + "\r\nPress ENTER toocontinue...");
                try{System.in.read();}
                catch(Exception e){}

                if (!nextAction.isEmpty())
                {
                    System.out.println(nextAction);
                }
            }

            // Create Data Lake Store and Analytics accounts
            public static void CreateAccounts() throws InterruptedException, CloudException, IOException {
                // Create ADLS account
                DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
                adlsParameters.setLocation(_location);

                _adlsClient.getAccountOperations().create(_resourceGroupName, _adlsAccountName, adlsParameters);

                // Create ADLA account
                DataLakeStoreAccountInfo adlsInfo = new DataLakeStoreAccountInfo();
                adlsInfo.setName(_adlsAccountName);

                DataLakeStoreAccountInfoProperties adlsInfoProperties = new DataLakeStoreAccountInfoProperties();
                adlsInfo.setProperties(adlsInfoProperties);

                List<DataLakeStoreAccountInfo> adlsInfoList = new ArrayList<DataLakeStoreAccountInfo>();
                adlsInfoList.add(adlsInfo);

                DataLakeAnalyticsAccountProperties adlaProperties = new DataLakeAnalyticsAccountProperties();
                adlaProperties.setDataLakeStoreAccounts(adlsInfoList);
                adlaProperties.setDefaultDataLakeStoreAccount(_adlsAccountName);

                DataLakeAnalyticsAccount adlaParameters = new DataLakeAnalyticsAccount();
                adlaParameters.setLocation(_location);
                adlaParameters.setName(_adlaAccountName);
                adlaParameters.setProperties(adlaProperties);

                    /* If this line generates an error message like "hello deep update for property 'DataLakeStoreAccounts' is not supported", please delete hello ADLS and ADLA accounts via hello portal and re-run your script. */

                _adlaClient.getAccountOperations().create(_resourceGroupName, _adlaAccountName, adlaParameters);
            }

            //todo: this changes in hello next version of hello API
            public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();

                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path, bytesContents, force);
            }

            public static void DeleteFile(String filePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().delete(filePath, _adlsAccountName);
            }

            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException {
                InputStream stream = _adlsFileSystemClient.getFileSystemOperations().open(srcPath, _adlsAccountName).getBody();

                PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());

                String fileContents = "";
                if (stream != null) {
                    Writer writer = new StringWriter();

                    char[] buffer = new char[1024];
                    try {
                        Reader reader = new BufferedReader(
                                new InputStreamReader(stream, "UTF-8"));
                        int n;
                        while ((n = reader.read(buffer)) != -1) {
                            writer.write(buffer, 0, n);
                        }
                    } finally {
                        stream.close();
                    }
                    fileContents =  writer.toString();
                }

                pWriter.println(fileContents);
                pWriter.close();
            }

            // Submit a U-SQL job by providing script contents.
            // Returns hello job ID
            public static UUID SubmitJobByScript(String script, String jobName) throws IOException, CloudException {
                UUID jobId = java.util.UUID.randomUUID();
                USqlJobProperties properties = new USqlJobProperties();
                properties.setScript(script);
                JobInformation parameters = new JobInformation();
                parameters.setName(jobName);
                parameters.setJobId(jobId);
                parameters.setType(JobType.USQL);
                parameters.setProperties(properties);

                JobInformation jobInfo = _adlaJobClient.getJobOperations().create(_adlaAccountName, jobId, parameters).getBody();

                return jobId;
            }

            // Wait for job completion
            public static JobResult WaitForJob(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                while (jobInfo.getState() != JobState.ENDED)
                {
                    jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName,jobId).getBody();
                }
                return jobInfo.getResult();
            }

            // Get job status
            public static String GetJobStatus(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                return jobInfo.getState().toValue();
            }
        }

1. <span data-ttu-id="bc5ed-139">Följ hello prompter toorun och fullständig hello program.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-139">Follow hello prompts toorun and complete hello application.</span></span>

## <a name="see-also"></a><span data-ttu-id="bc5ed-140">Se även</span><span class="sxs-lookup"><span data-stu-id="bc5ed-140">See also</span></span>
* <span data-ttu-id="bc5ed-141">toosee hello samma självstudier med andra verktyg klickar du på hello flikväljarna överst hello hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="bc5ed-141">toosee hello same tutorial using other tools, click hello tab selectors on hello top of hello page.</span></span>
* <span data-ttu-id="bc5ed-142">toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="bc5ed-142">toosee a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="bc5ed-143">tooget utveckla U-SQL-program, se [utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bc5ed-143">tooget started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="bc5ed-144">toolearn U-SQL finns [Kom igång med Azure Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md), och [U-SQL-Språkreferens](http://go.microsoft.com/fwlink/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="bc5ed-144">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md), and [U-SQL language reference](http://go.microsoft.com/fwlink/?LinkId=691348).</span></span>
* <span data-ttu-id="bc5ed-145">Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bc5ed-145">For management tasks, see [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md).</span></span>
* <span data-ttu-id="bc5ed-146">tooget en översikt över Data Lake Analytics finns [översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bc5ed-146">tooget an overview of Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>
