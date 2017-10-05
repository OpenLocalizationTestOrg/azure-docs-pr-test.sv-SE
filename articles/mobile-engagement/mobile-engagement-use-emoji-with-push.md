---
title: "Använda Emoji uttryckssymboler i Azure Mobile Engagement"
description: "Hur du använder Emoji uttryckssymboler inom push-meddelanden"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 663317d7-3c93-4e8f-b13d-c6fb342124ee
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: bbb7ce5e95b229a7505c5e97b6866d5a302a1d27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-emoji-emoticon-within-push-notifications"></a><span data-ttu-id="6776b-103">Använd Emoji uttryckssymbol inom Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="6776b-103">Use Emoji emoticon within Push Notifications</span></span>
<span data-ttu-id="6776b-104">Du kan inkludera Emoji uttryckssymboler i du push-meddelanden i några enkla steg:</span><span class="sxs-lookup"><span data-stu-id="6776b-104">You can include Emoji emoticons in you push notification in a few easy steps:</span></span> 

1. <span data-ttu-id="6776b-105">Först av allt du behöver för att hitta Emoji vill du skicka meddelandet.</span><span class="sxs-lookup"><span data-stu-id="6776b-105">First of all you need to find the Emoji you want to send in the message.</span></span> <span data-ttu-id="6776b-106">Kontrollera att Emoji som du väljer kommer att stödas av målenheten som enheten tillverkar ta lite tid att lägga till nya godkända Emojis plattformar för enheten.</span><span class="sxs-lookup"><span data-stu-id="6776b-106">Please ensure that the Emoji you are selecting will be supported by the target device as device manufactures take some time to add newly approved Emojis to the device platforms.</span></span> 
2. <span data-ttu-id="6776b-107">På **Windows** -du kan navigera till den här [länk](http://apps.timwhitlock.info/emoji/tables/unicode) och kopiera ikonen ”inbyggd”.</span><span class="sxs-lookup"><span data-stu-id="6776b-107">On **Windows** - you can navigate to this [link](http://apps.timwhitlock.info/emoji/tables/unicode) and copy the 'Native' icon.</span></span>
   
    ![][7] 
3. <span data-ttu-id="6776b-108">På **Mac** -du kan hitta Emojis i ordlistan program under Redigera -> Emoji & symboler.</span><span class="sxs-lookup"><span data-stu-id="6776b-108">On **Mac** - you can find the Emojis in Dictionary application under Edit -> Emoji & Symbols.</span></span>
   
    ![][6] 
4. <span data-ttu-id="6776b-109">Gå till den **nå** fliken på den Azure Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="6776b-109">Now, go to the **Reach** tab on the the Azure Mobile Engagement portal.</span></span> <span data-ttu-id="6776b-110">Välj typ av push-meddelande (meddelande, avsöker osv.).</span><span class="sxs-lookup"><span data-stu-id="6776b-110">Select the type of your push notification (Announcement, polls etc).</span></span> <span data-ttu-id="6776b-111">Det här exemplet väljer vi ett meddelande push.</span><span class="sxs-lookup"><span data-stu-id="6776b-111">For this example we choose an announcement push.</span></span>
5. <span data-ttu-id="6776b-112">Ange olika fält för meddelandet tills du når texten i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="6776b-112">Specify the different fields of the notification until you reach the text of the notification.</span></span> <span data-ttu-id="6776b-113">Det är där du ska lägga till uttryckssymbolen Emoji.</span><span class="sxs-lookup"><span data-stu-id="6776b-113">This is where you will add your Emoji Emoticon.</span></span> <span data-ttu-id="6776b-114">Du kan välja att placera den i rubriken, meddelandet eller båda.</span><span class="sxs-lookup"><span data-stu-id="6776b-114">You can choose to put it in the title, the message, or both.</span></span> <span data-ttu-id="6776b-115">Du måste dra och släppa eller kopiera Emoji som finns på platser som ovan.</span><span class="sxs-lookup"><span data-stu-id="6776b-115">You will need to drag and drop or copy the Emoji that you find from the locations above.</span></span> 
   
    ![][1]
6. <span data-ttu-id="6776b-116">Fyll i fälten för meddelandet och spara den.</span><span class="sxs-lookup"><span data-stu-id="6776b-116">Complete the other fields for the notification and save it.</span></span> 
7. <span data-ttu-id="6776b-117">När du kör ett test eller aktiverar ett meddelande visas ett meddelande med uttryckssymbolen som anges.</span><span class="sxs-lookup"><span data-stu-id="6776b-117">When you run a test or activate the announcement you will see a notification with the emoticon as specified.</span></span>   
   
    <span data-ttu-id="6776b-118">![][3] ![][4] ![][5]</span><span class="sxs-lookup"><span data-stu-id="6776b-118">![][3] ![][4] ![][5]</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/iOS_Emoji.png
[4]: ./media/mobile-engagement-use-emoji-with-push/Android_Emoji.png
[5]: ./media/mobile-engagement-use-emoji-with-push/WindowsPhone_Emoji.png
[6]: ./media/mobile-engagement-use-emoji-with-push/Mac_SelectEmoji.png
[7]: ./media/mobile-engagement-use-emoji-with-push/Windows_SelectEmoji.png

