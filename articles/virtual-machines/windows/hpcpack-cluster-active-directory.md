---
title: aaaHPC Pack kluster med Azure Active Directory | Microsoft Docs
description: "Lär dig hur toointegrate ett HPC Pack 2016 kluster i Azure med Azure Active Directory"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
ms.assetid: 9edf9559-db02-438b-8268-a6cba7b5c8b7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/14/2016
ms.author: danlep
ms.openlocfilehash: 0806e544a468e27ca0567e18c55554811584fbc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-hpc-pack-cluster-in-azure-using-azure-active-directory"></a>Hantera ett HPC Pack kluster i Azure med hjälp av Azure Active Directory
[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) stöder integration med [Azure Active Directory](../../active-directory/index.md) (Azure AD) för administratörer som distribuerar ett HPC Pack kluster i Azure.



Följ hello stegen i den här artikeln för följande uppgifter på hög hello: 
* Integrera manuellt HPC Pack klustret med Azure AD-klient
* Hantera och schemalägga jobb i HPC Pack klustret i Azure 

En klusterlösning HPC Pack integrera med Azure AD följer standard steg toointegrate andra program och tjänster. Den här artikeln förutsätter att du är bekant med grundläggande användarhantering i Azure AD. Mer information och bakgrunden finns hello [dokumentationen för Azure Active Directory](../../active-directory/index.md) och hello efter avsnittet.

## <a name="benefits-of-integration"></a>Fördelar med integrering


Azure Active Directory (AD Azure) är en flera innehavare molnbaserade katalog och identity management-tjänst som tillhandahåller enkel inloggning (SSO) toocloud lösningar.

Integrering av ett HPC Pack kluster med Azure AD kan hjälpa dig att uppnå följande mål hello:

* Ta bort hello traditionella Active Directory-domänkontrollant från hello HPC Pack kluster. Det kan minska hello driftskostnader hello klustret om det inte är nödvändiga för ditt företag och påskynda hello distributionsprocessen.
* Använda hello följande fördelar som ansluts av Azure AD:
    *   Enkel inloggning 
    *   Med hjälp av en lokal AD-identitet för hello HPC Pack kluster i Azure 

    ![Azure Active Directory-miljö](./media/hpcpack-cluster-active-directory/aad.png)


## <a name="prerequisites"></a>Krav
* **HPC Pack 2016-kluster som distribueras på Azure-datorer** – anvisningar finns i [distribuera ett HPC Pack 2016-kluster i Azure](hpcpack-2016-cluster.md). Du behöver hello DNS-namnet på hello huvudnod och hello autentiseringsuppgifterna för en Klusteradministratör för att slutföra hello stegen i den här artikeln.

  > [!NOTE]
  > Azure Active Directory-integrering stöds inte i versioner av HPC Pack före HPC Pack 2016.



* **Klientdatorn** -du behöver ett Windows eller Windows Server klienten datorn kör för HPC Pack klientverktyg. Om du bara vill toouse hello HPC Pack webbportal eller REST API toosubmit jobb kan du använda alla klientdatorer som du väljer.

* **HPC Pack klientverktyg** -installera hello HPC Pack klientverktyg på hello klientdatorn med hello ledigt installationspaket från hello Microsoft Download Center.


## <a name="step-1-register-hello-hpc-cluster-server-with-your-azure-ad-tenant"></a>Steg 1: Registrera hello HPC cluster server med Azure AD-klient
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Klicka på **Active Directory** i hello vänstra menyn och klicka sedan på önskad hello-katalogen i din prenumeration. Du måste ha behörigheten tooaccess resurser i hello directory.
3. Klicka på **användare**, och kontrollera att det finns användarkonton redan skapats eller konfigurerade.
4. Klicka på **program** > **Lägg till**, och klicka sedan på **Lägg till ett program som min organisation utvecklar**. Ange följande information i guiden hello hello:
    * **Namnet** -HPCPackClusterServer
    * **Typen** – Välj **webb-program och/eller webb-API**
    * **URL: en inloggning**- hello bas-URL för hello-exemplet som är som standard`https://hpcserver`
    * **App-ID URI** - `https://<Directory_name>/<application_name>`. Ersätt `<Directory_name`> med hello fullständigt namn för din Azure AD-klient, till exempel `hpclocal.onmicrosoft.com`, och Ersätt `<application_name>` med hello-namn som du valde tidigare.

5. När hello app läggs klickar du på **konfigurera**. Konfigurera hello följande egenskaper:
    * Välj **Ja** för **program är flera innehavare**
    * Välj **Ja** för **användaren tilldelning krävs tooaccess app**.

6. Klicka på **Spara**. När du sparar är klar klickar du på **hantera Manifest**. Den här åtgärden hämtar programmets JavaScript object notation (JSON) manifestfilen. Redigera hello hämtas manifestet genom att söka efter hello `appRoles` inställning och lägga till följande programrollen hello:
    ```json
    "appRoles": [
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "displayName": "HpcAdminMirror",
        "id": "61e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "description": "HpcAdminMirror",
        "value": "HpcAdminMirror"
        },
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "description": "HpcUsers",
        "displayName": "HpcUsers",
        "id": "91e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "value": "HpcUsers"
        }
    ],
    ```
7. Spara hello-filen. Klicka sedan på hello portal **hantera Manifest** > **överför Manifest**. Du kan sedan överföra hello redigerade manifest.
8. Klicka på **användare**, Välj en användare och klicka sedan på **tilldela**. Tilldela en hello tillgängliga roller (HpcUsers eller HpcAdminMirror) toohello användare. Upprepa detta steg med ytterligare användare i hello directory. Bakgrundsinformation om klustret användare finns [hantera klustret användare](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).

   > [!NOTE] 
   > toomanage användare, bör du använda hello Azure Active Directory preview bladet i hello [Azure-portalen](https://portal.azure.com).
   >


## <a name="step-2-register-hello-hpc-cluster-client-with-your-azure-ad-tenant"></a>Steg 2: Registrera hello HPC-kluster klienten med Azure AD-klient

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Klicka på **Active Directory** i hello vänstra menyn och klicka sedan på önskad hello-katalogen i din prenumeration. Du måste ha behörigheten tooaccess resurser i hello directory.
3. Klicka på **program** > **Lägg till**, och klicka sedan på **Lägg till ett program som min organisation utvecklar**. Ange följande information i guiden hello hello:

    * **Namnet** -HPCPackClusterClient
    * **Typen** – Välj **Native Client-program**
    * **Omdirigerings-URI** - `http://hpcclient`

4. När hello app läggs klickar du på **konfigurera**. Kopiera hello **klient-ID** värde och spara den. Du behöver det senare när du konfigurerar ditt program.

5. I **behörigheter tooother program**, klickar du på **Lägg till program**. Söka och lägga till hello HpcPackClusterServer program (som du skapade i steg 1).

6. I hello **delegerade behörigheter** listrutan **åtkomst HpcClusterServer**. Klicka sedan på **Spara**.


## <a name="step-3-configure-hello-hpc-cluster"></a>Steg 3: Konfigurera hello HPC-kluster

1. Ansluta toohello HPC Pack 2016 huvudnod i Azure.

2. Starta HPC PowerShell.

3. Kör följande kommando hello:

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    där

    * `AADTenant`Anger hello Azure AD-klientnamn, exempel`hpclocal.onmicrosoft.com`
    * `AADClientAppId`Anger hello klient-ID för hello app skapade i steg 2.

4. Starta om hello HpcSchedulerStateful.

    I ett kluster med flera huvudnoderna kan du köra följande PowerShell-kommandon på hello huvudnod tooswitch hello primära repliken för hello HpcSchedulerStateful tjänsten hello:

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName “fabric:/HpcApplication/SchedulerStatefulService”

    ```

## <a name="step-4-manage-and-submit-jobs-from-hello-client"></a>Steg 4: Hantera och skicka jobb hello-klient

Hej tooinstall HPC Pack klientverktyg på datorn, hämta HPC Pack 2016-installationsfiler (fullständig installation) från hello Microsoft Download Center. När du börjar installationen hello väljer hello installationsalternativ för hello **HPC Pack klientverktyg**.

tooprepare hello klientdatorn, installera hello-certifikat som används under [konfiguration av HPC](hpcpack-2016-cluster.md) på hello-klientdator. Använd standard Windows certificate management procedurer tooinstall hello certifikatets offentliga toohello **certifikat – aktuell användare** > **betrodda rotcertifikatutfärdare** lagras. 

Du kan nu köra hello HPC Pack kommandon eller använda hello HPC Pack Job manager GUI toosubmit och hantera kluster-jobb med hjälp av hello Azure AD-konto. Skicka alternativ, se [skicka HPC jobb tooan HPC Pack kluster i Azure](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).

> [!NOTE]
> När du försöker tooconnect toohello HPC Pack kluster i Azure för hello första gången, visas ett popup-fönster. Ange dina autentiseringsuppgifter toolog i Azure AD i. hello token cachelagras sedan. Senare anslutningar toohello kluster i Azure används hello cachelagras token om autentisering ändringar eller hello cachelagras är avmarkerad.
>
  
Till exempel när du har slutfört hello föregående steg kan du fråga efter jobb från en lokal klient på följande sätt:

```powershell 
Get-HpcJob –State All –Scheduler https://<Azure load balancer DNS name> -Owner <Azure AD account>
```

## <a name="useful-cmdlets-for-job-submission-with-azure-ad-integration"></a>Användbar cmdlets för jobbet med Azure AD-integrering 

### <a name="manage-hello-local-token-cache"></a>Hantera hello lokala token-cache

HPC Pack 2016 innehåller två nya HPC PowerShell-cmdlets toomanage hello lokala token-cache. Dessa cmdlet: ar är användbara för att skicka jobb icke-interaktivt. Se följande exempel hello:

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-hello-credentials-for-submitting-jobs-using-hello-azure-ad-account"></a>Ange hello autentiseringsuppgifter för att skicka jobb med hjälp av hello Azure AD-konto 

Du kan ibland vill toorun hello jobbet under hello HPC-kluster användare (för en domänansluten HPC-kluster körs som en domänanvändare; för ett icke-domänanslutna HPC-kluster kör som en lokal användare på hello huvudnod).

1. Använd följande kommandon tooset hello autentiseringsuppgifter hello:

    ```powershell
    $localUser = “<username>”

    $localUserPassword=”<password>”

    $secpasswd = ConvertTo-SecureString $localUserPassword -AsPlainText -Force

    $mycreds = New-Object System.Management.Automation.PSCredential ($localUser, $secpasswd)

    Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name>
    ```

2. Sedan skicka hello jobb. hello projektaktivitet körs under $localUser på hello compute-noder.

    ```powershell
    $emptycreds = New-Object System.Management.Automation.PSCredential ($localUser, (new-object System.Security.SecureString))
    ...
    $job = New-HpcJob –Scheduler https://<Azure load balancer DNS name>

    Add-HpcTask -Job $job -CommandLine "ping localhost" -Scheduler https://<Azure load balancer DNS name>

    Submit-HpcJob -Job $job -Scheduler https://<Azure load balancer DNS name> -Credential $emptycreds
    ```
    
   Om `–Credential` inte anges med `Submit-HpcJob`, hello jobbet körs under en lokal mappade användare som hello Azure AD-kontot. (hello HPC-kluster skapas en lokal användare med hello samma namn som hello Azure AD-kontot toorun hello aktivitet.)
    
3. Ange utökade data för hello Azure AD-kontot. Detta är användbart när du kör ett MPI-jobb på Linux-noder med hello Azure AD-konto.

   * Ange utökade data för hello Azure AD-kontot sig själv

      ```powershell
      Set-HpcJobCredential -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data> -AadUser
      ```
      
   * Ange utökade data och köra som användaren för HPC-kluster
   
      ```powershell
      Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data>
      ```

