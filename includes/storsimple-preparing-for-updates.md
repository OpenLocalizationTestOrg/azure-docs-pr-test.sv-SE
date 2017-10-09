<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a>Förbereda för uppdateringar
Du behöver tooperform hello följande innan du skanna och tillämpa hello uppdatering:

1. Ta en ögonblicksbild i molnet av hello data på enheten.
2. Kontrollera att din styrenhets-fästa IP-adresser är dirigerbara och kan ansluta toohello Internet. De statiska IP-adresser kommer att använda tooservice uppdateringar tooyour enhet. Du kan testa detta genom att köra följande cmdlet på varje domänkontrollant från hello Windows PowerShell-gränssnittet för hello enhet hello:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    **Exempel på utdata för Test-Connection när fasta IP-adresser kan ansluta toohello Internet**

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

När du har slutfört dessa manuella före kontroller, kan du fortsätta tooscan och installera hello uppdateringar.

