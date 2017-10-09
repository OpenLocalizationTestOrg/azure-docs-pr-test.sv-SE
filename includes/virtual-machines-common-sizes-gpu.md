
hello NC och NV storlek kallas även GPU-aktiverade instanser. De är specialiserade virtuella datorer som använder NVIDIA:s GPU-kort och som är optimerade för olika scenarier och användningsfall. hello NV storlekar är optimerad och utformade för fjärranslutna visualisering, strömning, spel, kodning och VDI-scenarier som använder ramar, till exempel OpenGL och DirectX. hello NC-storlekar är mer optimerade för algoritmer, inklusive CUDA - och OpenCL-baserade program och simulering och beräkningsintensiva och nätverks-intensiva program. 


hello NV instanser är drivs av NVIDIA Tesla M60 GPU-kort och NVIDIA rutnät för skrivbordet snabbare program och virtuella skrivbord där kunder är kan toovisualize sina data eller simulering. Användare kommer kan toovisualize sina grafik beräkningsintensiva arbetsflöden på kapacitet för hello NV instanser tooget överlägsen grafik och dessutom köra enkel precision arbetsbelastningar som till exempel kodning och återgivning. hello Tesla M60 levererar 4096 CUDA kärnor i en dubbla GPU-design med upp 1080p H.264 too36 dataströmmar. 

hello NC instanser drivs av NVIDIA Tesla K80 kort. Nu kan användarna bearbeta data mycket snabbare genom att utnyttja CUDA för energiutforskningstillämpningar, kraschsimulering, strålspårning (raytracing), deep learning och mycket annat. hello Tesla K80 levererar 4992 CUDA kärnor med dubbla GPU design, too2.91 Teraflops dubbel precision och upp too8.93 Teraflops enkel precision prestanda.

## <a name="nv-instances"></a>NV-instanser

| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | GPU | Maximalt antal datadiskar |
| --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 8 |
| Standard_NV12 |12 |112 |680 | 2 | 16 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 |

1 GPU = ett halvt M60-kort.

## <a name="nc-instances"></a>NC-instanser

| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | GPU | Maximalt antal datadiskar |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 8 |
| Standard_NC12 |12 |112 | 680 | 2 | 16 |
| Standard_NC24 |24 |224 | 1440 | 4 | 32 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 32 |

1 GPU = ett halvt K80-kort.

*RDMA-stöd


