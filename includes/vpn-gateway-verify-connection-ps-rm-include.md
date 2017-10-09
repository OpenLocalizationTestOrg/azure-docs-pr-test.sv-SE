<span data-ttu-id="bc569-101">Du kan kontrollera att anslutningen lyckades genom att använda hello 'Get-AzureRmVirtualNetworkGatewayConnection-cmdlet med eller utan '-Debug ”.</span><span class="sxs-lookup"><span data-stu-id="bc569-101">You can verify that your connection succeeded by using hello 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet, with or without '-Debug'.</span></span> 

1. <span data-ttu-id="bc569-102">Använd hello följande cmdlet-exemplet, konfigurerar hello värden toomatch egna.</span><span class="sxs-lookup"><span data-stu-id="bc569-102">Use hello following cmdlet example, configuring hello values toomatch your own.</span></span> <span data-ttu-id="bc569-103">Om du uppmanas, väljer du ”A” i ordning toorun 'All'.</span><span class="sxs-lookup"><span data-stu-id="bc569-103">If prompted, select 'A' in order toorun 'All'.</span></span> <span data-ttu-id="bc569-104">I exemplet hello ”-namnet ' refererar toohello namnet på hello-anslutning som du vill tootest.</span><span class="sxs-lookup"><span data-stu-id="bc569-104">In hello example, '-Name' refers toohello name of hello connection that you want tootest.</span></span>

  ```powershell
  Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG
  ```
2. <span data-ttu-id="bc569-105">Visa hello värden när hello cmdleten har slutförts.</span><span class="sxs-lookup"><span data-stu-id="bc569-105">After hello cmdlet has finished, view hello values.</span></span> <span data-ttu-id="bc569-106">I hello exemplet nedan visar hello anslutningens status som ”ansluten” och du kan se ingående och utgående byte.</span><span class="sxs-lookup"><span data-stu-id="bc569-106">In hello example below, hello connection status shows as 'Connected' and you can see ingress and egress bytes.</span></span>
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```