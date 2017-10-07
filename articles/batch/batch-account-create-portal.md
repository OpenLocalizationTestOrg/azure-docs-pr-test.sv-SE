---
title: aaaCreate Batch-kontot i hello Azure-portalen | Microsoft Docs
description: "Lär dig hur toocreate ett Azure Batch konto i hello Azure portal toorun storskaliga parallella arbetsbelastningar i hello moln"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2176f88ba0a1a3298023de8f520d46ef28a664b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-batch-account-with-hello-azure-portal"></a>Skapa ett batchkonto med hello Azure-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
>
>

Lär dig hur toocreate ett Azure Batch-kontot i hello [Azure-portalen][azure_portal], och välj Egenskaper för hello som passar din situation för beräkning. Lär dig där toofind viktiga egenskaper som kontot URL: er och åtkomstnycklar.

Information om Batch-konton och scenarier finns hello [funktion översikt](batch-api-basics.md).



## <a name="create-a-batch-account"></a>Skapa ett Batch-konto

Använd hello portal toocreate ett Batch-konto i en hello två *poolens fördelning lägen*: **Batch-tjänsten** läge eller hello senare **användarens prenumeration** läge, som kräver mer konfiguration. Information om dessa två lägen finns hello [funktion översikt](batch-api-basics.md#account). Funktioner i hello användarläge prenumeration finns även hello [blogginlägget](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).

## <a name="batch-service-mode"></a>Batch-tjänstläge



1. Logga in toohello [Azure-portalen][azure_portal].
2. Klicka på **Ny** > **Beräkna** > **Batch-tjänst**.

    ![Batch i hello Marketplace][marketplace_portal]
3. Hej **nytt Batchkonto** bladet visas. Se hello beskrivningarna nedan för varje element i bladet.

    ![Skapa ett Batch-konto][account_portal]

    a. **Kontonamn**: hello Batch-kontonamn du väljer måste vara unika inom hello Azure-region där hello kontot skapas (se **plats** nedan). hello-kontonamnet får innehålla endast små bokstäver eller siffror och måste bestå av 3 till 24 tecken.

    b. **Prenumerationen**: hello prenumerationen i vilka toocreate hello Batch-kontot. Om du bara har en prenumeration väljs den som standard.

    c. **Poolallokeringsläge**: Välj **Batch-tjänst**.

    c. **Resursgrupp**: Välj en befintlig resursgrupp för ditt nya Batch-konto. Du kan också skapa en ny resursgrupp.

    d. **Plats**: hello Azure-region i vilken toocreate hello Batch-kontot. Endast hello regioner som stöds av din prenumeration och resursgrupp visas som alternativ.

    e. **Lagringskonto** (valfritt): Ett generellt Azure Storage-konto som du associerar med ditt Batch-konto. Detta rekommenderas för de flesta Batch-konton. Mer information finns i [Länkat Azure Storage-konto](#linked-azure-storage-account) senare i den här artikeln.

4. Klicka på **skapa** toocreate hello-konto.

   hello portal anger distribution pågår. När åtgärden har slutförts visas meddelandet **Distributionen är klar** i **Meddelanden**.

## <a name="user-subscription-mode"></a>Användarprenumerationsläge

### <a name="allow-azure-batch-tooaccess-hello-subscription-one-time-operation"></a>Tillåt Azure Batch tooaccess hello prenumeration (engångsåtgärd)
När du skapar din första Batch-kontot i användarläge för prenumerationen kan du utföra hello följande steg tooregister prenumerationen med Batch. (Om du tidigare har gjort det, hoppar du över toohello nästa avsnitt.)

1. Logga in toohello [Azure-portalen][azure_portal].

2. Klicka på **fler tjänster** > **prenumerationer**, och klicka på hello-prenumeration som du vill använda toouse för hello Batch-kontot.

3. I hello **prenumeration** bladet, klickar du på **åtkomstkontroll (IAM)** > **Lägg till**.

    ![Åtkomstkontroll för prenumeration][subscription_access]

4. På hello **lägga till behörigheter** bladet, Välj hello **deltagare** roll, söka efter hello Batch-API. Sök efter var och en av de här strängarna tills du hittar hello-API:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Nyare Azure AD-klientorganisationer kan använda det här namnet.
    3. **ddbf3205-c6bd-46ae-8127-60eb93363864** är hello-ID för hello Batch-API. 

5. När du har hittat hello Batch-API och på **spara**.

    ![Lägg till Batch-behörigheter][add_permission]

### <a name="create-a-key-vault"></a>Skapa ett nyckelvalv
I användarläge för prenumerationen, krävs en Azure key vault som tillhör toothe samma resursgrupp som hello Batch-kontot toobe skapas. Se till att hello resursgrupp i en region där Batch är [tillgängliga](https://azure.microsoft.com/regions/services/) och som har stöd för din prenumeration.

1. I hello [Azure-portalen][azure_portal], klickar du på **ny** > **säkerhet + identitet** > **Key Vault** .

2. I hello **skapa Nyckelvalvet** bladet, ange ett namn för hello nyckelvalvet och skapa en resursgrupp i hello region som du vill använda för Batch-kontot. Lämna hello återstående inställningar till standardvärden och klicka sedan på **skapa**.

### <a name="create-a-batch-account"></a>Skapa ett Batch-konto

1. I hello [Azure-portalen][azure_portal], klickar du på **ny** > **Compute** > **Batchtjänsten**.

    ![Batch i hello Marketplace][marketplace_portal]
3. Hej **nytt Batchkonto** bladet visas. Se hello beskrivningarna nedan för varje element i bladet.

    ![Skapa ett Batch-konto][account_portal_byos]

    a. **Kontonamn**: hello Batch-kontonamn du väljer måste vara unika inom hello Azure-region där hello kontot skapas (se **plats** nedan). hello-kontonamnet får innehålla endast små bokstäver eller siffror och måste bestå av 3 till 24 tecken.

    b. **Prenumerationen**: Om du har mer än en prenumeration väljer du hello-prenumeration som du har registrerat med hello Batch-tjänsten.

    c. **Poolallokeringsläge**: Välj **Användarprenumeration**.

    d. **Nyckelvalv**: Välj hello nyckelvalv som du skapade för Batch-kontot i hello föregående avsnitt. Om du vill kan du skapa ett nytt nyckelvalv. När du har valt hello valvet, Välj hello kryssrutan toogrant Azure Batch åtkomst toohello nyckelvalvet.

    c. **Resursgruppen**: Välj hello resursgrupp som du skapade hello nyckelvalvet.

    d. **Plats**: hello Azure-region där du skapade hello nyckelvalv för hello Batch-kontot.

    e. **Lagringskonto** (valfritt): Ett generellt Azure Storage-konto som du associerar med ditt Batch-konto. Detta rekommenderas för de flesta Batch-konton. Mer information finns i [Länkat Azure Storage-konto](#linked-azure-storage-account) nedan.

4. Klicka på **skapa** toocreate hello-konto.

   hello portal anger distribution pågår. När åtgärden har slutförts visas meddelandet **Distributionen är klar** i **Meddelanden**.



## <a name="view-batch-account-properties"></a>Visa egenskaper för ett Batch-konto
När hello-konto har skapats kan du öppna hello **Batch-kontoblad** tooaccess inställningar och egenskaper. Du kan komma åt alla egenskaper och inställningar med hjälp av hello vänstra menyn hello Batch-kontot bladet.

![Bladet för Batch-kontot på Azure Portal][account_blade]

* **Batch-kontots URL**: när du utvecklar ett program med hello [Batch-API: er](batch-apis-tools.md#azure-accounts-for-batch-development), behöver du ett konto URL tooaccess Batch-resurser. En URL för Batch-kontot har hello följande format:

    `https://<account_name>.<region>.batch.azure.com`

![Batch-kontots URL på portalen][account_url]

* **Åtkomstnycklar** (Batch-tjänsten-läge): tooauthenticate åtkomst tooyour Batch-kontot från ditt program behöver du en åtkomstnyckel. (Den här inställningen är inte tillgänglig i användarprenumerationsläge, där du använder Azure Active Directory-autentisering.)

    tooview eller generera åtkomstnycklar för Batch-kontot, ange `keys` hello vänstra menyn **Sök** på bladet för hello Batch-konto och sedan markera **nycklar**.

    ![Batch-kontonycklar på Azure Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Länkat Azure Storage-konto

Alternativt kan du koppla en generell Azure Storage-konto tooyour Batch-kontot. Hej [programpaket](batch-application-packages.md) funktion i Batch använder Azure Blob storage hello [Batch filen konventioner .NET](batch-task-output.md) bibliotek. De här valfria funktionerna hjälpa dig distribuera hello-program som körs Batch-aktiviteter och spara hello data de producerar.

Vi rekommenderar att du skapar ett nytt lagringskonto för exklusiv användning av Batch-kontot.

![Skapa ett allmänt lagringskonto][storage_account]

> [!NOTE]
> Azure Batch stöder för närvarande endast hello allmänna lagringskontotypen. Den här kontotypen beskrivs i steg 5, [Skapa ett lagringskonto] (../storage/common/storage-create-storage-account.md#create-a-storage-account), i [Om Azure Storage-konton](../storage/common/storage-create-storage-account.md).
>
>

> [!WARNING]
> Var försiktig när du återskapar hello snabbtangenter av länkade lagringskonton. Återskapa endast en lagringskontonyckel och klicka på **synkronisera nycklar** på hello länkade Storage-konto bladet. Vänta 5 minuter tooallow hello nycklar toopropagate toohello compute-noder i din pooler och sedan återskapa och synkronisera hello andra nyckeln om det behövs. Om du återskapa båda nycklarna på hello samma tid, compute-noderna inte kan toosynchronize antingen nyckel, och de kommer att förlora åtkomst toohello Storage-konto.
>
>

![Återskapa lagringskontonycklar][4]

## <a name="batch-service-quotas-and-limits"></a>Kvoter och begränsningar för Batch-tjänsten
Du är medveten om att som med din Azure-prenumeration och andra Azure-tjänster, vissa [kvoter och gränser](batch-quota-limit.md) gäller tooBatch konton. Aktuella kvoter för Batch-kontot visas i hello-portal i hello konto **egenskaper**.

![Kvoter för Batch-konto på Azure Portal][quotas]



Dessutom att många av dessa kvoter kan ökas bara med en kostnadsfri produkten supportbegäran skickade i hello Azure-portalen. Se [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md) information om begär kvoten ökar.

## <a name="other-batch-account-management-options"></a>Andra alternativ för Batch-kontohantering
Dessutom toousing hello Azure-portalen, du kan också skapa och hantera Batch-konton med hello följande:

* [PowerShell-cmdletar för Batch](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Nästa steg
* Se hello [Batch funktionsöversikt](batch-api-basics.md) toolearn mer om Batch-koncept och funktioner. hello artikeln beskriver hello primära Batch resurser, till exempel pooler, compute-noder, jobb och uppgifter och ger en översikt över hello tjänstens funktioner som möjliggör körning av storskaliga beräkning arbetsbelastning.
* Hello grundläggande information om hur utvecklingen av ett Batch-aktiverade program som använder hello [Batch .NET-klientbibliotek](batch-dotnet-get-started.md) eller [Python](batch-python-tutorial.md). De här inledande artiklarna leder dig igenom ett fungerande program som använder hello Batch-tjänsten tooexecute en arbetsbelastning på flera datornoder och användning av Azure Storage för arbetsbelastningen filen mellanlagrings- och hämtning.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Återskapa lagringskontonycklar"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[account_portal_byos]: ./media/batch-account-create-portal/batch_acct_portal_byos.png
