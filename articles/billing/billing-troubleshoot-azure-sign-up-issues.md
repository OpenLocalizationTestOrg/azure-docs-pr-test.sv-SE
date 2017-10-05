---
title: "Felsöka Azure registreringsproblem | Microsoft Docs"
description: "Beskriver hur du felsöker vissa vanliga Azure logga upp problem."
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: a0907da1-cb2d-41d1-a97f-43841fabe355
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 647509ea36e487aca5db661adb3268e845988f78
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-sign-up-issues-for-azure"></a>Felsökning av problem med registreringen för Azure
Om du inte registrera dig för Azure använda tipsen i den här artikeln för att felsöka vanliga problem. Om du har ett problem med ditt kreditkort under registreringen, se [din bankkort eller kreditkort har nekats på Azure anmälan](billing-credit-card-fails-during-azure-sign-up.md). Om du har ett Azure-konto men kan inte logga in, se [jag kan inte logga in för att hantera min Azure-prenumeration](billing-cannot-login-subscription.md).

## <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Förloppsindikator låser sig i avsnittet ”identitetsverifiering kort”

För att slutföra identitetsverifiering genom kort, måste cookies från tredje part tillåtas för webbläsaren.

![Skärmbild av identitetsverifiering kort avsnittet hänger sig under registreringen](./media/billing-troubleshoot-azure-sign-up-issues/identity-verification-hangs.PNG)

Använd följande steg om du vill uppdatera din webbläsare cookie-inställningar.

1. Om du använder Chrome, gå till **inställningar** > **visa avancerade inställningar** > **sekretess** > **innehåll inställningar**. Avmarkera **blockerar cookies från tredje part och platsdata**.
2. Om du använder Edge, gå till **inställningar** > **visa avancerade inställningar** > **Cookies**. Välj **inte blockera cookies**.
3. Uppdatera registreringssidan för Azure och kontrollera om problemet är löst.
4. Om uppdateringen inte löser problemet, avsluta och starta om webbläsaren och försök sedan igen.

## <a name="credit-card-form-doesnt-support-my-billing-address"></a>Kreditkort formuläret har inte stöd för min faktureringsadress
Din faktureringsadress måste finnas i det land som du har valt i den **om du** avsnitt. Kontrollera att du väljer rätt land.

## <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Inga textmeddelanden eller samtal vid registreringen konto verifieringen
Det är vanligtvis mycket snabbare, kan det ta upp till fyra minuter för Verifieringskod ska kunna levereras. Det telefonnummer som du anger för verifiering lagras inte som kontaktnummer för kontot.

Här följer några ytterligare tips:
* Ett VOIP-telefonnummer kan inte användas för telefon verifieringsprocessen.
* Kontrollera telefonnummer du anger, inklusive landskoden som du har valt i den nedrullningsbara menyn.
* Om telefonen inte ta emot textmeddelanden (SMS), försöker den **ringa mig** alternativet.
* Se till att din telefon kan ta emot samtal eller SMS-meddelanden från USA baserat flera.

När du får ett SMS eller telefonsamtal, ange koden som visas i textrutan.

## <a name="credit-card-declined-or-not-accepted"></a>Kreditkort avvisade eller accepterades inte
Virtuella eller förbetalt kort för kredit- eller inte är godkänd som betalning för Azure-prenumerationer. Vad kan orsaka kortet nekas finns [din bankkort eller kreditkort har nekats på Azure anmälan](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="free-trial-is-not-available"></a>”Kostnadsfri utvärderingsversion är inte tillgänglig”
Har du använt en Azure-prenumeration tidigare? Den Azure användarvillkoren begränsar kostnadsfri utvärderingsversion aktivering bara för användare som är ny i Azure. Om du har haft någon annan typ av Azure-prenumeration kan du aktivera en kostnadsfri utvärderingsversion. Överväg att registrera dig för en [betala per användning prenumeration](https://azure.microsoft.com/offers/ms-azr-0003p/).

## <a name="i-saw-a-charge-on-my-free-trial-account"></a>Jag såg en avgift på mitt konto kostnadsfri utvärderingsversion
Du kan se en liten verifiering håller på ditt kreditkortskonto när du har loggat in som tas bort inom 3 till 5 dagar. Om du är oroliga om att hantera kostnaderna kan du läsa mer om [förhindrar oväntade kostnader](https://docs.microsoft.com/azure/billing/billing-getting-started).

## <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Det går inte att aktivera Azure förmånsplan MSDN, BizSpark, BizSparkPlus eller MPN
Kontrollera att du använder de högra inloggningsuppgifter. Kontrollera sedan förmånsprogrammet att se till att du är berättigad. 

* MSDN
  * Kontrollera din behörighet status i din [MSDN kontosida](https://msdn.microsoft.com/subscriptions/manage/default.aspx).
  * Om du inte kan kontrollera din status, kontaktar du den [kundtjänst för MSDN-prenumerationer](https://msdn.microsoft.com/subscriptions/contactus.aspx)
* BizSpark
  * Logga in på den [BizSpark portal](https://www.microsoft.com/bizspark/default.aspx#start-two) och verifiera din behörighet status för BizSpark och BizSpark Plus.
  * Om du inte kan kontrollera din status, kan du [få hjälp i forumen BizSpark](http://aka.ms/bzforums).
* MPN
  * Logga in på den [MPN portal](https://mspartner.microsoft.com/en/us/Pages/Locale.aspx) och verifiera din behörighet status. Om du har rätt [Cloud Platform befogenheter](https://mspartner.microsoft.com/en/us/pages/membership/cloud-platform-competency.aspx), du kan vara berättigad till ytterligare fördelar.
  * Om du inte kan verifiera din status, kontakta [MPN support](https://mspartner.microsoft.com/en/us/Pages/Support/Premium/contact-support.aspx).

## <a name="cant-activate-new-azure-in-open-subscription"></a>Det går inte att aktivera Azure i Open prenumeration
Du måste ha en giltig Online Service aktivering OSA nyckel med minst en Azure i Open token som är kopplade till den för att skapa en Azure i Open-prenumeration. Om du inte har en OSA nyckel, kontakta någon av Microsoft-Partners som anges i [Microsoft Pinpoint](http://pinpoint.microsoft.com/).

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.
