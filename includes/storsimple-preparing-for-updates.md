<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a><span data-ttu-id="faef7-101">Förbereda för uppdateringar</span><span class="sxs-lookup"><span data-stu-id="faef7-101">Preparing for updates</span></span>
<span data-ttu-id="faef7-102">Du behöver tooperform hello följande innan du skanna och tillämpa hello uppdatering:</span><span class="sxs-lookup"><span data-stu-id="faef7-102">You will need tooperform hello following steps before you scan and apply hello update:</span></span>

1. <span data-ttu-id="faef7-103">Ta en ögonblicksbild i molnet av hello data på enheten.</span><span class="sxs-lookup"><span data-stu-id="faef7-103">Take a cloud snapshot of hello device data.</span></span>
2. <span data-ttu-id="faef7-104">Kontrollera att din styrenhets-fästa IP-adresser är dirigerbara och kan ansluta toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="faef7-104">Ensure that your controller fixed IPs are routable and can connect toohello Internet.</span></span> <span data-ttu-id="faef7-105">De statiska IP-adresser kommer att använda tooservice uppdateringar tooyour enhet.</span><span class="sxs-lookup"><span data-stu-id="faef7-105">These fixed IPs will be used tooservice updates tooyour device.</span></span> <span data-ttu-id="faef7-106">Du kan testa detta genom att köra följande cmdlet på varje domänkontrollant från hello Windows PowerShell-gränssnittet för hello enhet hello:</span><span class="sxs-lookup"><span data-stu-id="faef7-106">You can test this by running hello following cmdlet on each controller from hello Windows PowerShell interface of hello device:</span></span>
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    <span data-ttu-id="faef7-107">**Exempel på utdata för Test-Connection när fasta IP-adresser kan ansluta toohello Internet**</span><span class="sxs-lookup"><span data-stu-id="faef7-107">**Sample output for Test-Connection when fixed IPs can connect toohello Internet**</span></span>

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

<span data-ttu-id="faef7-108">När du har slutfört dessa manuella före kontroller, kan du fortsätta tooscan och installera hello uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="faef7-108">After you have successfully completed these manual pre-checks, you can proceed tooscan and install hello updates.</span></span>

