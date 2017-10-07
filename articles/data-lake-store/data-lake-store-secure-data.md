---
title: aaaSecuring data som lagras i Azure Data Lake Store | Microsoft Docs
description: "Lär dig hur toosecure data i Azure Data Lake Store med hjälp av grupper och åtkomst styra listor"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 2b4ed7e322e1843ca47d6968ec8801ac19ea6399
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-data-stored-in-azure-data-lake-store"></a>Att säkra data som lagras i Azure Data Lake Store
Skydda data i Azure Data Lake Store är en metod för tre steg.

1. Börja med att skapa säkerhetsgrupper i Azure Active Directory (AAD). Dessa säkerhetsgrupper används tooimplement rollbaserad åtkomstkontroll (RBAC) i Azure Portal. Mer information finns i [rollbaserad åtkomstkontroll i Microsoft Azure](../active-directory/role-based-access-control-configure.md).
2. Tilldela hello AAD säkerhet grupper toohello Azure Data Lake Store-konto. Detta styr åtkomst toohello Data Lake Store-konto från hello portal och hantering från portalen hello eller API: er.
3. Tilldela hello Add-säkerhetsgrupper som åtkomstkontrollistor (ACL) på hello Data Lake Store-filsystem.
4. Dessutom kan du också ange ett IP-adressintervall för klienter som kan komma åt hello data i Data Lake Store.

Den här artikeln innehåller anvisningar för hur toouse hello Azure portal tooperform hello ovan uppgifter. Detaljerad information om hur Data Lake Store implementerar säkerhet på hello konto- och se [säkerhet i Azure Data Lake Store](data-lake-store-security-overview.md). Djupgående information om hur ACL: er implementeras i Azure Data Lake Store, se [översikt över åtkomstkontroll i Data Lake Store](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Ett Azure Data Lake Store-konto**. Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Skapa säkerhetsgrupper i Azure Active Directory
Anvisningar för hur toocreate Add-säkerhetsgrupper och hur tooadd toohello användargruppen Se [hantera säkerhetsgrupper i Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

> [!NOTE] 
> Du kan lägga till både användare och andra grupper tooa grupp i Azure AD med hjälp av hello Azure-portalen. Men i order tooadd en service principal tooa grupp, Använd [Azure AD PowerShell-modulen](../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).
> 
> ```powershell
> # Get hello desired group and service principal and identify hello correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add hello service principal toohello group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-tooazure-data-lake-store-accounts"></a>Tilldela användarna eller säkerhetsgrupperna tooAzure Data Lake Store-konton
När du tilldelar användarna eller säkerhetsgrupperna tooAzure datasjölagerkonton, styra åtkomst toohello hanteringsåtgärder på hello-kontot med hello Azure-portalen och Azure Resource Manager API: er. 

1. Öppna ett Azure Data Lake Store-konto. Hello till vänster och klicka på **Bläddra**, klickar du på **Datasjölager**, och klicka sedan på hello konto namnet toowhich du vill tooassign en grupp med användare eller säkerhetsgrupp från hello Data Lake Store-bladet.

2. I din Data Lake Store-konto inställningar-bladet klickar du på **Access Control (IAM)**. hello bladet som standard visar **prenumerationsadministratörer** som en ägare.
   
    ![Tilldela Data Lake Store säkerhetskonto grupp tooAzure](./media/data-lake-store-secure-data/adl.select.user.icon.png "tilldela säkerhet grupp tooAzure Data Lake Store-konto")

    Det finns två sätt tooadd en grupp och tilldela relevanta roller.
   
    * Lägga till en användare/grupp toohello och sedan tilldela en roll eller
    * Lägg till en roll och tilldela användare eller grupper toorole.
     
    I det här avsnittet ska titta vi på hello första tillvägagångssättet, lägga till en grupp och sedan tilldela roller. Du kan utföra liknande steg toofirst Välj en roll och sedan tilldela grupper toothat roll.
4. I hello **användare** bladet, klickar du på **Lägg till** tooopen hello **Lägg till åtkomst** bladet. I hello **Lägg till åtkomst** bladet, klickar du på **Välj en roll**, och välj sedan en roll för hello användargrupp.
   
    ![Lägga till en roll för hello användare](./media/data-lake-store-secure-data/adl.add.user.1.png "lägga till en roll för hello användare")
   
    Hej **ägare** och **deltagare** rollen ger åtkomst tooa olika funktioner för administration på hello data lake-konto. Användare som kommer att interagera med data i hello data lake kan du lägga till dem toohello ** Reader ** roll. hello omfattningen av dessa roller är begränsad toohello management operationer relaterade toohello Azure Data Lake Store-konto.
   
    Operations enskilda filsystembehörigheter definiera vilka hello användarna kan göra för data. Därför kan en användare med rollen Läsare kan bara visa de inställningar som associeras med hello konto men kan potentiellt läsa och skriva data baserat på filsystemsbehörigheter som tilldelats toothem. Data Lake Store-filsystembehörigheterna beskrivs på [tilldela säkerhetsgrupp som ACL: er toohello Azure Data Lake Store-filsystem](#filepermissions).
5. I hello **Lägg till åtkomst** bladet, klickar du på **lägga till användare** tooopen hello **lägga till användare** bladet. Leta efter hello säkerhetsgrupp du skapade tidigare i Azure Active Directory i det här bladet. Om du har en stor mängd grupper toosearch från Använd hello textrutan på hello översta toofilter på hello gruppnamn. Klicka på **Välj**.
   
    ![Lägg till en säkerhetsgrupp](./media/data-lake-store-secure-data/adl.add.user.2.png "lägger till en säkerhetsgrupp")
   
    Om du vill tooadd en grupp/användare som inte visas kan du bjuda in dem med hjälp av hello **bjuda in** ikon och ange hello e-postadress för hello användargrupp.
6. Klicka på **OK**. Du bör se hello säkerhetsgrupp som lagts till som visas nedan.
   
    ![Lägga till säkerhetsgruppen](./media/data-lake-store-secure-data/adl.add.user.3.png "säkerhetsgrupp som lagts till")

7. Användaren/säkerhetsgrupp har nu åtkomst toohello Azure Data Lake Store-konto. Om du vill tooprovide-toospecific användare, du kan lägga till dem toohello säkerhetsgrupp. På samma sätt om du vill toorevoke för en användare, du kan ta bort dem från hello säkerhetsgruppen. Du kan också tilldela flera säkerhetsgrupper tooan konto. 

## <a name="filepermissions"></a>Tilldela användare eller säkerhetsgrupp som ACL: er toohello Azure Data Lake Store-filsystem
Genom att tilldela användaren/säkerhetsgrupper toohello Azure Data Lake-filsystem kan du ställa in åtkomstkontroll på hello data som lagras i Azure Data Lake Store.

1. I ditt Data Lake Store-kontoblad klickar du på **Data Explorer**.
   
    ![Skapa kataloger i Data Lake Store-konto](./media/data-lake-store-secure-data/adl.start.data.explorer.png "skapa kataloger i Data Lake-konto")
2. I hello **Data Explorer** bladet Klicka hello filen eller mappen som du vill tooconfigure hello ACL och klicka sedan på **åtkomst**. tooassign ACL tooa fil, måste du klicka på **åtkomst** från hello **Filförhandsgranskning** bladet.
   
    ![Ange ACL: er i Data Lake filsystemet](./media/data-lake-store-secure-data/adl.acl.1.png "Ange ACL: er i Data Lake-filsystem")
3. Hej **åtkomst** bladet visar hello standard åtkomst och anpassade åtkomst har redan tilldelats toohello rot. Klicka på hello **Lägg till** ikonen tooadd Anpassad-nivå ACL: er.
   
    ![Visa en lista över standard och anpassad åtkomst](./media/data-lake-store-secure-data/adl.acl.2.png "listan standard och anpassad åtkomst")
   
   * **Standard åtkomst** är hello UNIX-format åtkomst, anger du läsa, skriva, köra (rwx) toothree distinkta användarklasser: ägare och gruppen.
   * **Anpassade åtkomst** motsvarar toohello POSIX-ACL: er som du kan använda tooset behörigheter för specifika namngivna användare eller grupper, och inte bara hello filens ägare eller grupp. 
     
     Mer information finns i [HDFS ACL: er](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Mer information om hur ACL: er implementeras i Data Lake Store finns [åtkomstkontroll i Data Lake Store](data-lake-store-access-control.md).
4. Klicka på hello **Lägg till** ikonen tooopen hello **Lägg till anpassad åtkomst** bladet. I det här bladet, klickar du på **Välj användare eller grupp**, och klicka sedan på **Välj användare eller grupp** bladet, leta efter hello säkerhetsgrupp du skapade tidigare i Azure Active Directory. Om du har en stor mängd grupper toosearch från Använd hello textrutan på hello översta toofilter på hello gruppnamn. Klicka på hello grupp du tooadd och klickar sedan på **Välj**.
   
    ![Lägga till en grupp](./media/data-lake-store-secure-data/adl.acl.3.png "lägga till en grupp")
5. Klicka på **Välj behörigheter**hello behörigheter och välj om du vill tooassign hello behörigheter som standard ACL åtkomst till ACL eller båda. Klicka på **OK**.
   
    ![Tilldela behörigheter toogroup](./media/data-lake-store-secure-data/adl.acl.4.png "tilldela behörigheter toogroup")
   
    Mer information om behörigheter i Data Lake Store och standard-/ Access ACL: er finns [åtkomstkontroll i Data Lake Store](data-lake-store-access-control.md).
6. I hello **Lägg till anpassad åtkomst** bladet, klickar du på **OK**. hello nyligen tillagda grupp med hello som är associerade behörigheter visas nu i hello **åtkomst** bladet.
   
    ![Tilldela behörigheter toogroup](./media/data-lake-store-secure-data/adl.acl.5.png "tilldela behörigheter toogroup")
   
   > [!IMPORTANT]
   > I hello aktuella versionen kan du bara ha 9 poster under **anpassad åtkomst**. Om du vill tooadd mer än 9-användare, bör du skapa säkerhetsgrupper, lägga till användare toosecurity grupper, lägga till ge åtkomst toothose säkerhetsgrupper för hello Data Lake Store-konto.
   > 
   > 
7. Om det behövs kan ändra du också hello behörigheter när du har lagt till hello grupp. Rensa eller välja hello kryssrutan för varje behörighet (läsa, skriva, köra) baserat på om du vill tooremove eller tilldela behörigheten toohello säkerhetsgruppen. Klicka på **spara** toosave hello ändringar eller **Ignorera** tooundo hello ändringar.

## <a name="set-ip-address-range-for-data-access"></a>Ange IP-adressintervall för dataåtkomst
Azure Data Lake Store kan du toofurther Lås åtkomst tooyour data store på nätverksnivån. Du kan aktivera brandväggen, ange en IP-adress eller definiera ett intervall med IP-adresser för dina betrodda klienter. När du har aktiverat, kan klienter som har hello IP-adresser inom definierade intervallet ansluta toohello store.

![Brandväggsinställningar och IP-åtkomst till](./media/data-lake-store-secure-data/firewall-ip-access.png "inställningar och IP-adress för brandvägg")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Ta bort säkerhetsgrupper för ett Azure Data Lake Store-konto
När du tar bort säkerhetsgrupper från Azure Data Lake Store-konton kan ändrar du endast åtkomst toohello hanteringsåtgärder på hello-kontot med hello Azure-portalen och Azure Resource Manager API: er.

1. I ditt Data Lake Store-kontoblad klickar du på **inställningar**. Från hello **inställningar** bladet, klickar du på **användare**.
   
    ![Tilldela säkerhet grupp tooAzure datasjökontot](./media/data-lake-store-secure-data/adl.select.user.icon.png "tilldela säkerhet grupp tooAzure Data Lake-konto")
2. I hello **användare** bladet klickar du på hello säkerhetsgrupp som du vill tooremove.
   
    ![Säkerhet grupp tooremove](./media/data-lake-store-secure-data/adl.add.user.3.png "säkerhet grupp tooremove")
3. Klicka i hello bladet för hello säkerhetsgruppen **ta bort**.
   
    ![Säkerhetsgrupp bort](./media/data-lake-store-secure-data/adl.remove.group.png "säkerhetsgrupp tas bort")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Ta bort säkerhetsgruppen ACL: er från Azure Data Lake Store-filsystem
När du tar bort säkerhetsgrupper ACL: er från Azure Data Lake Store-filsystem kan du ändra toohello åtkomst till data i hello Data Lake Store.

1. I ditt Data Lake Store-kontoblad klickar du på **Data Explorer**.
   
    ![Skapa kataloger i Data Lake-konto](./media/data-lake-store-secure-data/adl.start.data.explorer.png "skapa kataloger i Data Lake-konto")
2. I hello **Data Explorer** bladet Klicka hello filen eller mappen som du vill tooremove hello ACL och klicka sedan på hello i ditt kontoblad **åtkomst** ikon. tooremove ACL för en fil som du måste klicka på **åtkomst** från hello **Filförhandsgranskning** bladet.
   
    ![Ange ACL: er i Data Lake filsystemet](./media/data-lake-store-secure-data/adl.acl.1.png "Ange ACL: er i Data Lake-filsystem")
3. I hello **åtkomst** bladet från hello **anpassad åtkomst** klickar du på hello säkerhetsgrupp som du vill tooremove. I hello **anpassad åtkomst** bladet, klickar du på **ta bort** och klicka sedan på **OK**.
   
    ![Tilldela behörigheter toogroup](./media/data-lake-store-secure-data/adl.remove.acl.png "tilldela behörigheter toogroup")

## <a name="see-also"></a>Se även
* [Översikt över Azure Data Lake Store](data-lake-store-overview.md)
* [Kopiera data från Azure Storage BLOB tooData Datasjölager](data-lake-store-copy-data-azure-storage-blob.md)
* [Använd Azure Data Lake Analytics med Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Använd Azure HDInsight med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Kom igång med Data Lake Store med hjälp av PowerShell](data-lake-store-get-started-powershell.md)
* [Kom igång med Data Lake Store med hjälp av .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Åtkomst till diagnostikloggar för Data Lake Store](data-lake-store-diagnostic-logs.md)

