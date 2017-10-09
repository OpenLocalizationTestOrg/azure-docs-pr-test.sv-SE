---
title: "aaaConfigure domänanslutna HDInsight-kluster med hjälp av PowerShell - Azure | Microsoft Docs"
description: "Lär dig hur tooset upp och konfigurera domänanslutna HDInsight-kluster med Azure PowerShell"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: a13b2f7a-612d-4800-bc92-7fc0524f3e89
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: 49da3439513d1e51171f0f7f7f9c3d967d55cb7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview-using-azure-powershell"></a>Konfigurera domänanslutna HDInsight-kluster (förhandsversion) med Azure PowerShell
Lär dig hur tooset upp ett Azure HDInsight-kluster med Azure Active Directory (Azure AD) och [Apache Ranger](http://hortonworks.com/apache/ranger/) med hjälp av Azure PowerShell. Ett Azure PowerShell-skript har angetts toomake hello configuration snabbare och mindre lätt fel. Domänanslutna HDInsight kan bara konfigureras på Linux-baserade kluster. Mer information finns i [införa domänanslutna HDInsight-kluster](hdinsight-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie har inte aktiverats på domänanslutna HDInsight.

En typisk konfiguration för domänanslutna HDInsight-kluster omfattar hello följande steg:

1. Skapa ett Azure klassiska virtuella nätverk för din Azure AD.  
2. Skapa och konfigurera Azure AD och Azure AD DS.
3. Lägg till en VM-toohello klassiska virtuella nätverk för att skapa organisationsenhet. 
4. Skapa en organisationsenhet för Azure AD DS.
5. Skapa ett HDInsight-VNet i hanteringsläge för hello Azure-resurs.
6. Inställningar för omvänd DNS-zoner för hello Azure AD DS.
7. Peer hello två Vnet.
8. Skapa ett HDInsight-kluster.

hello PowerShell-skript som utför steg 3 till 7. Du måste gå igenom steg 1 och 2 manuellt.  Om du föredrar att inte toouse Azure PowerShell finns [konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md). 

Ett exempel på hello slutliga topologi ser ut som följer:

![Domänanslutna HDInsight-topologi](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Eftersom Azure AD för närvarande stöder endast klassiska virtuella nätverk (Vnet) och Linux-baserade HDInsight-kluster endast stöder Azure Resource Manager baserade Vnet, HDInsight Azure AD-integration kräver två Vnet och en peering mellan dem. Hello jämförelseinformation mellan hello två distributionsmodeller finns [Azure Resource Manager och klassisk distribution: Förstå distributionsmodeller och hello för dina resurser](../azure-resource-manager/resource-manager-deployment-model.md). hello två Vnet måste vara i hello samma region som hello Azure AD DS.

> [!NOTE]
> Den här kursen förutsätter att du inte har en Azure AD. Om du har ett kan du hoppa över hello-delen i steg 2.
> 
> 

## <a name="prerequisites"></a>Krav
Du måste ha hello följande objekt toogo igenom den här kursen:

* Bekanta dig med [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) dess [priser](https://azure.microsoft.com/pricing/details/active-directory-ds/) struktur.
* Se till att prenumerationen är godkända för den här public preview. Du kan göra det genom att skicka ett e-postmeddelande toohdipreview@microsoft.com med ditt prenumerations-ID.
* Ett SSL-certifikat som signerats av en utfärdare för signering för din domän. Det krävs hello certifikat genom att konfigurera säker LDAP. Självsignerade certifikat kan inte användas.
* Azure PowerShell.  Se [installera och konfigurera Azure PowerShell](/powershell/azure/overview).

## <a name="create-an-azure-classic-vnet-for-your-azure-ad"></a>Skapa ett Azure klassiska virtuella nätverk för din Azure AD.
Hello instruktioner finns i [här](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic).

## <a name="create-and-configure-azure-ad-and-azure-ad-ds"></a>Skapa och konfigurera Azure AD och Azure AD DS.
Hello instruktioner finns i [här](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).

## <a name="run-hello-powershell-script"></a>Kör hello PowerShell-skript
hello PowerShell-skript kan hämtas från [GitHub](https://github.com/hdinsight/DomainJoinedHDInsight). Extrahera hello zip-filen och spara hello filer lokalt.

**tooedit hello PowerShell-skript**

1. Öppna run.ps1 med hjälp av Windows PowerShell ISE eller valfri textredigerare.
2. Fyll hello värden för hello följande variabler:
   
   * **$SubscriptionName** – hello namn på hello Azure-prenumeration där du vill att toocreate ditt HDInsight-kluster. Du har redan skapat ett klassiskt virtuellt nätverk i den här prenumerationen och kommer att skapa ett virtuellt nätverk med Azure Resource Manager för hello HDInsight-kluster för prenumerationen.
   * **$ClassicVNetName** -hello klassiska virtuella nätverk som innehåller hello Azure AD DS. Den här virtuella nätverket måste finnas i hello samma prenumeration som anges ovan. Det här virtuella nätverket måste skapas med hjälp av hello Azure-portalen och inte använder klassiska portalen. Om du följer anvisningarna hello i [konfigurera domänanslutna HDInsight-kluster (förhandsgranskning)](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic), hello standardnamnet är contosoaadvnet.
   * **$ClassicResourceGroupName** – hello Resource Manager gruppnamn för hello klassiska virtuella nätverk som nämns ovan. Till exempel contosoaadrg. 
   * **$ArmResourceGroupName** – hello resursgruppens namn inom som du vill toocreate hello HDInsight-kluster. Du kan använda hello samma resursgrupp som $ArmResourceGroupName.  Om hello resursgruppen inte finns, skapar hello skript hello resursgruppen.
   * **$ArmVNetName** -hello Resource Manager virtuella nätverksnamnet inom vilken du vill toocreate hello HDInsight-kluster. Det här virtuella nätverket placeras i $ArmResourceGroupName.  Om hello virtuella nätverk inte finns, skapar hello PowerShell-skript den. Om den finns, ska det tillhör hello resursgrupp som du anger ovan.
   * **$AddressVnetAddressSpace** – hello adressutrymmet för det virtuella nätverket för hello Resource Manager. Se till att det här adressutrymmet är tillgänglig. Det här adressutrymmet får inte överlappa hello klassiska virtuella nätverkets adressutrymme. Till exempel ”10.1.0.0/16”
   * **$ArmVnetSubnetName** -hello Resource Manager virtuella undernät nätverksnamnet inom vilken du vill tooplace hello HDInsight-kluster virtuella datorer. Om hello undernätet inte finns, skapas det hello PowerShell-skript. Om den finns bör det vara en del av hello virtuella nätverk som du anger ovan.
   * **$AddressSubnetAddressSpace** – hello nätverk adressintervall för hello Resource Manager undernät för virtuellt nätverk. Hej HDInsight-kluster är VM-IP-adresser från detta adressintervall för undernätet. Till exempel ”10.1.0.0/24”.
   * **$ActiveDirectoryDomainName** – hello Azure AD-domännamn som du vill toojoin hello HDInsight-kluster virtuella datorerna till. Till exempel ”contoso.onmicrosoft.com”
   * **$ClusterUsersGroups** – hello nätverksnamn hello säkerhetsgrupper från din AD som du vill toosync toohello HDInsight-kluster. hello användare i den här säkerhetsgruppen kommer att kunna toolog på instrumentpanelen för toohello klustret med hjälp av autentiseringsuppgifterna för active directory-domän. Dessa säkerhetsgrupper måste finnas i hello active directory. Till exempel ”hiveusers” eller ”clusteroperatorusers”.
   * **$OrganizationalUnitName** -hello organisationsenhet i hello-domän som du vill tooplace hello HDInsight-kluster virtuella datorer och hello tjänstens huvudnamn som används av hello klustret. hello PowerShell-skript skapar Organisationsenheten om det inte finns. Till exempel ”HDInsightOU”.
3. Spara hello ändringar.

**toorun hello skript**

1. Kör **Windows PowerShell** som administratör.
2. Bläddra mapp toohello run.ps1. 
3. Kör hello skript genom att skriva hello filnamn och tryck **RETUR**.  Den visas 3 inloggning dialogrutor:
   
   1. **Logga in tooAzure klassiska portalen** – ange dina autentiseringsuppgifter som du använder toosign i tooAzure klassiska portalen. Du måste ha skapat hello Azure AD och Azure AD DS med hjälp av autentiseringsuppgifterna.
   2. **Logga in tooAzure Resource Manager portal** – ange dina autentiseringsuppgifter som du använder toosign tooAzure Resource Manager-portalen.
   3. **Namnet på användaren** – ange hello autentiseringsuppgifter för hello namnet på användaren som du vill toobe administratör för hello HDInsight-kluster. Om du har skapat en Azure AD från början måste du har skapat den här användaren med hjälp av den här dokumentationen. 
      
      > [!IMPORTANT]
      > Ange hello användarnamn i formatet: 
      > 
      > Domännamn\användarnamn (till exempel contoso.onmicrosoft.com\clusteradmin)
      > 
      > 
      
      Användaren måste ha behörighet för 3: toojoin datorer toohello angivna Active Directory-domän toocreate tjänstens huvudnamn och datorobjekt inom hello ges organisationsenhet; och tooadd omvänd DNS-proxy regler.

När du skapar omvänd DNS-zoner, hello skript uppmanas du tooenter ett nätverk-ID. Nätverks-ID måste vara hello Resource Manager virtuella nätverkets adressprefix. Till exempel om Resource Manager för virtuella nätverkets undernät adressutrymme är 10.2.0.0/24 ange 10.2.0.0/24 när hello verktyget efterfrågar hello nätverks-ID. 

## <a name="create-hdinsight-cluster"></a>Skapa HDInsight-kluster
I det här avsnittet skapar du ett Linux-baserade Hadoop-kluster i HDInsight med antingen hello Azure-portalen eller [Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md). För andra metoder för att skapa kluster och förstå hello inställningar, se [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md). För mer information om hur du använder Resource Manager mallen toocreate Hadoop-kluster i HDInsight, se [skapa Hadoop-kluster i HDInsight med hjälp av Resource Manager-mallar](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**En domänansluten HDInsight-kluster med toocreate hello Azure-portalen**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **ny**, **Intelligence + analys**, och sedan **HDInsight**.
3. Från hello **nya HDInsight-kluster** bladet ange eller välj hello följande värden:
   
   * **Klusternamnet**: Ange ett nytt namn för klustret för hello domänanslutna HDInsight-kluster.
   * **Prenumerationen**: Välj en Azure-prenumeration används för att skapa det här klustret.
   * **Klusterkonfiguration**:
     
     * **Klustret typen**: Hadoop. Domänanslutna HDInsight är för närvarande endast stöds på Hadoop-kluster.
     * **Operativsystemet**: Linux.  Domänanslutna HDInsight stöds bara på Linux-baserade HDInsight-kluster.
     * **Version**: Hadoop 2.7.3 (HDI 3.5). Domänanslutna HDInsight stöds bara på HDInsight-kluster av version 3.5.
     * **Klustret typen**: PREMIUM
       
       Klicka på **Välj** toosave hello ändringar.
   * **Autentiseringsuppgifter**: Konfigurera hello autentiseringsuppgifter för både hello klustret användar- och hello SSH-användare.
   * **Datakällan**: skapa ett nytt lagringskonto eller använda ett befintligt lagringskonto som hello standardkontot för lagring för hello HDInsight-kluster. hello plats måste hello samtidigt som hello två Vnet.  hello plats är också hello platsen för hello HDInsight-kluster.
   * **Priser**: Välj hello antalet arbetarnoder i klustret.
   * **Avancerade konfigurationer**: 
     
     * **Domänanslutning & Vnet /-undernätet**: 
       
       * **Domäninställningar**: 
         
         * **Domännamn**: contoso.onmicrosoft.com
         * **Namnet på användaren**: Ange ett användarnamn för domänen. Den här domänen måste ha följande behörigheter hello: ansluta till datorer toohello domän och placera dem i hello-organisationsenhet som du konfigurerat tidigare. Skapa tjänstens huvudnamn i hello-organisationsenhet som du konfigurerat tidigare. Skapa omvänd DNS-poster. Den här domänanvändare blir Hej administratör för den här datorn är domänansluten HDInsight-kluster.
         * **Domänlösenord**: Ange hello domänanvändarlösenord.
         * **Organisationsenheten**: Ange hello unika namnet för hello Organisationsenhet som du tidigare har konfigurerat. Till exempel: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com
         * **LDAPS URL**: ldaps://contoso.onmicrosoft.com:636
         * **Åtkomst-användargrupp**: Ange hello säkerhetsgrupp vars användare du wan toosync toohello klustret. Till exempel HiveUsers.
           
           Klicka på **Välj** toosave hello ändringar.
           
           ![Domänanslutna HDInsight portal konfigurera domäninställningen](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Virtuellt nätverk**: contosohdivnet
       * **Undernät**: Undernät1
         
         Klicka på **Välj** toosave hello ändringar.        
         Klicka på **Välj** toosave hello ändringar.
   * **Resursgruppen**: Välj hello resursgruppens namn används för hello HDInsight VNet (contosohdirg).
4. Klicka på **Skapa**.  

Ett annat alternativ för att skapa domänanslutna HDInsight-kluster är toouse Azure Resource Manager-mallen. hello följande procedur visas hur:

**toocreate ett domänanslutna HDInsight-kluster med hjälp av en Resource Manager-mall**

1. Klicka på hello följande bild tooopen en Resource Manager-mall i hello Azure-portalen. hello Resource Manager-mall finns i en offentlig blob-behållare. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure-use-powershell/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Från hello **parametrar** bladet ange hello följande värden:
   
   * **Prenumerationen**: (Välj din Azure-prenumeration).
   * **Resursgruppen**: Klicka på **använda befintliga**, och ange hello samma resursgrupp som du har använt.  Till exempel contosohdirg. 
   * **Plats**: Ange en resursgruppens plats.
   * **Klusternamn**: Ange ett namn för hello Hadoop-kluster som du vill skapa. Till exempel contosohdicluster.
   * **Klustret typen**: Välj en typ av kluster.  hello standardvärdet är **hadoop**.
   * **Plats**: Välj en plats för hello klustret.  hello standardkontot för lagring använder hello samma plats.
   * **Klustret Arbetsnoden antal**: Välj hello antalet arbetarnoder.
   * **Klustrets inloggningsnamn och lösenord**: hello standard inloggningsnamnet är **admin**.
   * **SSH-användarnamn och lösenord**: hello Standardanvändarnamnet är **sshuser**.  Du kan byta namn. 
   * **Virtuella nätverks-Id**: /subscriptions/&lt;SubscriptionID > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >
   * **Virtuella undernät**: /subscriptions/&lt;SubscriptionID > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >/undernät/Undernät1
   * **Domännamn**: contoso.onmicrosoft.com
   * **Organisation enhet DN**: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com
   * **Användare klustergrupp D Ns**”:\"CN = HiveUsers, OU = AADDC Users, DC =<DomainName>, DC = onmicrosoft, DC = com\"”
   * **LDAPUrls**: [”ldaps://contoso.onmicrosoft.com:636”]
   * **DomainAdminUserName**: (Ange hello domain admin användarnamn)
   * **DomainAdminPassword**: (Ange hello domänanvändarlösenord för admin)
   * **Jag accepterar toohello villkoren ovan**: (kontrollera)
   * **PIN-kod toodashboard**: (kontrollera)
3. Klicka på **Köp**. En ny panel visas med rubriken **Skicka malldistribution**. Det tar cirka 20 minuter toocreate ett kluster. När hello klustret har skapats måste du klicka på hello klusterbladet i hello portal tooopen den.

När du har slutfört hello kursen kanske du vill toodelete hello klustret. Med HDInsight lagras dina data i Azure Storage så att du på ett säkert sätt kan ta bort ett kluster när det inte används. Du debiteras också för ett HDInsight-kluster, även när det inte används. Eftersom hello avgifterna för hello klustret är flera gånger större än hello avgifterna för lagring, är det ekonomiskt meningsfullt toodelete kluster när de inte används. Hello instruktioner för att ta bort ett kluster finns i [hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Nästa steg

* Om du vill konfigurera Hive-principer och köra Hive-frågor kan du läsa i [Konfigurera Hive-principer för domänanslutna HDInsight-kluster](hdinsight-domain-joined-run-hive.md).
* Använda SSH-tooconnect tooDomain ansluten HDInsight-kluster finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

