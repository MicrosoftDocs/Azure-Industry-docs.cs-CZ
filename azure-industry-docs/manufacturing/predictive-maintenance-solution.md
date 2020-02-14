---
title: Prediktivní údržba pomocí Azure ML a IoT v výrobě
author: ercenk
ms.author: ercenk
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Popis řešení pro vývoj prediktivní údržby pro výrobní zákazníky v Azure.
ms.openlocfilehash: c32893d534279cda35f7c6a142869d2983eaca67
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77053840"
---
# <a name="predictive-maintenance-in-manufacturing-solution-guide"></a>Průvodce prediktivní údržbou v procesu výrobního řešení


## <a name="introduction"></a>Úvod

Prediktivní údržba používá ke optimalizaci údržby vybavení kombinaci senzorů, umělých inteligentních funkcí a datových věd. Předvídání údržby vybavení potřebuje minimalizovat náklady na údržbu a maximalizovat dobu provozu, která výrobcům přináší významnou hodnotu.

Data leží na srdci řešení. Data musí mít odpovídající indikátory selhání a také další aspekty, které popisují kontext. Může pocházet z několika zdrojů, jako jsou senzory, protokoly počítačů a výrobní protokoly aplikací.

Tento článek obsahuje možnosti pro vytvoření řešení prediktivní údržby. Prezentují různé perspektivy a odkazují na stávající materiály, které vám pomohou začít. Požadavky na řešení prediktivní údržby se liší podle vybavení, prostředí, procesu a organizace. Snažíme se poskytnout alternativním přístupům a technologiím, které vás zavedou k řešení vašich potřeb.

Pojďme začít s komponentami na vysoké úrovni řešení prediktivní údržby.


![Řešení na vysoké úrovni](./assets/pdm-assets/highlevelsolution.png)

V tomto členění dojde k následujícím aktivitám vysoké úrovně:

1. Shromažďování školicích dat, včetně dat o chybách

2. Pomocí těchto školicích dat můžete vytvořit model Machine Learning (ML) pro předpověď budoucích selhání assetů při dané sadě podmínek.

3. Pokračování shromažďování dat průběžně

4. Zajistěte, aby shromážděná data byla v modelu ML, což bude mít za následek selhání, normálně s určitou úrovní jistoty (například: 85% pravděpodobnost, že se počítač v následujících 24 hodinách nezdařil. ")

5. Naplochení předpokládaných případů selhání

6. Plánování a fungování přehledů zobrazených v datech

## <a name="training-the-ml-model"></a>Školení modelu ML

Sestavování modelu ML vyžaduje dostatečná, správná a úplná data. Prediktivní údržba navíc přináší jedinečné výzvy, což je hlavní problém, který je k dispozici pro data o selhání. Selhání jsou relativně vzácné události – zejména v případě vysokého kapitálu zařízení, jako jsou například CNC počítače nebo komponenty pro rafinerie v oleji. Takže i v případě, že jsme shromáždili data ze senzorů po dlouhou dobu, nemusíme mít dostatečná data o selhání. Zvažte, jak je definována chyba. Co přesně znamená selhání? Znamená to, že zařízení přestane neočekávaně fungovat?
Znamená to, že se zařízení snižuje na úroveň, kde se už neprovádí na požadované úrovni? Je případ selhání odstraněný počítač, protože došlo k selhání součásti způsobené únavou kovu nebo jinými indikátory, které navzájemují selhání, před tím, než dojde k pohromě?

## <a name="considering-the-data-needed-for-ml"></a>S ohledem na data potřebná pro ML

Zvažte, že zaznamenáme dostatek dat pro správné nahrání těchto chyb? V mnoha případech nemusí být samotná data snímače dostatečně velká, aby bylo možné identifikovat selhání. V některých případech můžeme potřebovat externí data příznakem stavu počítače jako stav selhání nebo sekundární zdroj informací, jako je například operátor zachycení případu selhání prostřednictvím jiného systému. Tato data se můžou nacházet v externích systémech, jako je ERP, systémy pro spouštění výroby (historians atd.), a překračovat inFamous oddělení IT/oddělení tak, aby se v zpracovatelských podnicích provádělo zabezpečení potřebných dat navíc.

Prediktivní údržba je podle povahy dynamický problém a v takovém případě se musí přidružené modely strojového učení průběžně aktualizovat (nebo znovu vyškoleně). Pokud je to hotové, prediktivní údržba by měla omezit výskyty selhání – což je dobré, ale má za následek méně dat o selhání. Funkce, které mají vliv na selhání, můžou také změnit invalidování předchozích modelů strojového učení. Pravidelně doporučujeme školicí modely s případnými změnami v podmínkách selhání.

"Čerstvá" data znamenají také nové podmínky, které se přenesou do modelu, který je jiný než ten, který byl dříve použit pro školení modelu. Jinými slovy můžeme chybu namodelovat jako funkci proměnných _x<sub>1</sub>, x<sub>2</sub>, ⋯, x<sub>n</sub>, f (x<sub>1</sub>, x<sub>2</sub>, ⋯, x<sub>n</sub>)_ , ale nakonec můžeme zjistit, že proměnné _x<sub>(n + 1)</sub>, ⋯, x<sub>(m + n)</sub>_  jsou také ovlivňovány selháním, proto může být nutné upravit naše školení modelu pro _f (x<sub>1</sub>, x<sub>2</sub>, ⋯, x<sub>(m + n)</sub>)_ . Model nemusí být vhodný pro detekci selhání a nový model může být sestavený, včetně datových bodů z protokolů nástroje Machine status a dalších iterací.

I když se do cloudu nestreamují data o cloudovém prostředí IoT, můžou být data potřebná k výuce modelů strojového učení už ve vašich historians a jiných produkčních systémech. Je to jenom pro přípravu dat, aby je bylo možné využít ke studiu modelů strojového učení.

Následující obrázek znázorňuje typický pracovní postup pro školení modelu ML. Šipka s označením "opakovat" naznačuje, že se jedná o iterativní proces – jeden, kde budeme průběžně přeškolovat modely, protože se mění podmínky a změny podmínek. Kdy a jak často se tato smyčka musí opakovat, závisí na konkrétních podmínkách implementace a vyžaduje pečlivé sledování výkonu dříve vytvořených modelů při předvídání selhání zjišťování, když jsou modely "stárnutí" nebo "degradace".

Průběžné školení nových modelů a jejich nasazení přináší také výzvu pro jejich správu. Společnost Microsoft nabízí Azure Machine Learning služby Správa modelů pro CI/CD (průběžná integrace a průběžné doručování) modelů.

![Fáze vytváření modelů ML](assets/pdm-assets/mlmodelbuildingstages.png)

Společnost Microsoft publikovala [podrobného průvodce](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance?WT.mc_id=pdmsolution-docs-ercenk) pro přípravu dat a výuku modelu Machine Learning. Existují tři typické otázky údržby a související algoritmy strojového učení:

- _V případě assetu je to pravděpodobnost, že k selhání dojde během příštích X hodin?_ Odpověď: 0-100%
  - **Binární klasifikace:** Binární klasifikace je metoda strojového učení, která používá data k určení kategorie, typu nebo třídy položky nebo řádku dat jako člena jedné ze dvou tříd. Existuje několik typů klasifikačních algoritmů, společnost Microsoft zveřejnila sadu algoritmů, které jsou k dispozici jako [Machine Learning Studio moduly](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/machine-learning-initialize-model-classification?WT.mc_id=pdmsolution-docs-ercenk).
- _Jaká je zbývající životnost assetu?_ Odpověď: X hod.
  - **Regrese:** Regrese je třída algoritmů strojového učení, která předpovídá hodnotu proměnné s ohledem na sadu dalších proměnných. Machine Learning Studio zahrnuje sadu regresních algoritmů jako [modulů](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/machine-learning-initialize-model-regression?WT.mc_id=pdmsolution-docs-ercenk).
    - **Dlouhodobá krátkodobá paměť (LSTM):** [LSTM](https://colah.github.io/posts/2015-08-Understanding-LSTMs/?WT.mc_id=pdmsolution-docs-ercenk) sítě jsou typy neuronovéch sítí (DNN). Inspiraci z hluboké pochází z modelování chování jednotlivých neurons v mozku. Společnost Microsoft publikovala [podrobný průvodce](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-deep-learning-for-predictive-maintenance?WT.mc_id=pdmsolution-docs-ercenk) , který popisuje použití LSTM pro prediktivní údržbu.
- _Který prostředek vyžaduje obsluhu co nejnaléhavější?_ Odpověď: Asset X
  - **Klasifikace více tříd:** Klasifikace více tříd je metoda strojového učení, která používá data k určení kategorie, typu nebo třídy položky nebo řádku dat jako člena více než dvou tříd.

Opětovné uvedení v datech může znamenat, že se bude používat víc kanálů, inicializovat ho nejdřív v hromadném prostředí a pak dál přijímat streamovaná data pro předpověď selhání a zároveň je použít pro další buildy modelu.

## <a name="bringing-data-to-azure"></a>Přenesení dat do Azure

Microsoft Azure poskytuje různé služby pro přijímání a ukládání dat. Pro získání dat přenesených do Azure doporučujeme dávkové metody, pokud tam ještě není. Pokud můžete data exportovat jako soubory do známých formátů, jako je například CSV, JSON, XML atd. Jedná se o dobré možnosti. Můžete se také rozhodnout komprimovat je před nahráním a dále je zpracovávat na straně cloudu.

- Nahrávání pomocí [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy?WT.mc_id=pdmsolution-docs-ercenk) do úložiště objektů BLOB (Windows i Linux)

- [Připojení úložiště objektů BLOB](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux?WT.mc_id=pdmsolution-docs-ercenk) jako systému souborů na platformě Linux

- Použijte [službu import/export](https://docs.microsoft.com/azure/storage/common/storage-import-export-service?WT.mc_id=pdmsolution-docs-ercenk), pokud je velikost dat velká a trvá příliš dlouho k nahrání

- [Připojení](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows?WT.mc_id=pdmsolution-docs-ercenk) sdílené složky Azure v systému Windows, Linux a MacOS

Pokud jsou data ve SQL Server databázi, můžete k odeslání dat do Azure SQL Database použít také [Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?WT.mc_id=pdmsolution-docs-ercenk) .

Platforma Azure obsahuje celou řadu nástrojů a služeb pro operace extrakce, transformace a načítání (ETL). Nejvýraznějším způsobem služby je [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/?WT.mc_id=pdmsolution-docs-ercenk) , která poskytuje úplnou sadu funkcí pro manipulaci s daty. Další možnosti pro manipulaci s daty jsou k dispozici v mnoha službách, které jsou dostupné v Azure, prostřednictvím open source knihoven.

Jako u školení v režimu ML Microsoft Azure poskytuje mnoho možností, které je možné použít v různých kombinacích.

- [Služby Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/?WT.mc_id=pdmsolution-docs-ercenk)

- [Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/?WT.mc_id=pdmsolution-docs-ercenk)

- [Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/?WT.mc_id=pdmsolution-docs-ercenk)

- [Spark MLLib ve službě HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-machine-learning-mllib-ipython?WT.mc_id=pdmsolution-docs-ercenk)

- [Služba školení Batch AI](https://docs.microsoft.com/azure/batch-ai/?WT.mc_id=pdmsolution-docs-ercenk)

Rozhodnutí o tom, který nástroj použít, závisí na složitosti operací, zkušenostech týmu a velikosti dat.

Cenová rovnice u cloudových řešení obsahuje mnoho proměnných, kromě nákladů na cloudové služby, jako jsou například další analýzy, Správa, přenosy dat atd. Tyto proměnné použijte při vyhodnocování nákladů a zajistěte si rozhodnutí v informování. Služby pouze netvoří rovnici celkových nákladů.

Návrh procesu analýzy dat a publikování modelu jsou detailní témata a liší se podle používaných technologií. Tato témata jsou mimo rámec tohoto článku. K dispozici je řada článků, které vysvětlují proces a služby Azure, které je možné použít při generování modelu. Microsoft také poskytuje systematický přístup k vytváření řešení, které umožňuje týmům vědeckých pracovníků dat efektivně spolupracovat v životním cyklu dat.

Dokumentace k [Machine Learning](https://docs.microsoft.com/azure/machine-learning?WT.mc_id=pdmsolution-docs-ercenk) Microsoftu je dobrým výchozím bodem pro zkoumání možností pro vytváření, nasazování a správu modelů ml a AI do cloudu.

Microsoft Azure platforma nabízí možnosti bohatou pro zpracování dat ve škálování a sestavování modelů ML. K dispozici skoro nekonečné, škálovatelné výpočetní a úložné síly na cloudových platformách umožňují vytvářet modely ML a AI. Proto použití služeb Azure pro sestavování modelů představuje největší logickou možnost pro implementaci tohoto toku dat.

## <a name="using-the-model"></a>Použití modelu

Až budeme mít model ML, potřebujeme mechanismus pro jeho využívání (nebo "pomocí" IT) k předpovídání nutnosti údržby zařízení. Po přijetí dat ze zařízení se tato vrstva pohybuje přes vrstvu zpracování a předpovídá budoucí případy selhání a prezentují různé způsoby, kterými se týmy údržby chovají.

![Použití modelu](assets/pdm-assets/usingthemodel.png)

Ingestování dat je možné provést online streamování dat živého senzoru do řešení nebo offline pomocí pravidelného importu dat ze senzorů do řešení.

Microsoft Azure platforma poskytuje nejrůznější služby pro ingestování, zpracování a ukládání dat, jako například:

- [Event Hubs Azure](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs?WT.mc_id=pdmsolution-docs-ercenk)

- [Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview?WT.mc_id=pdmsolution-docs-ercenk)

- [IoT Hub Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub?WT.mc_id=pdmsolution-docs-ercenk)

- [Apache Kafka pro HDInsight](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-introduction?WT.mc_id=pdmsolution-docs-ercenk)

Na rozdíl od procesu sestavování modelu ML nevyžadují využívání spousty výpočetních prostředků. V závislosti na vašich potřebách je možné model nasadit do služby v cloudu nebo místně na produkčním patře.

Existují dvě hlavní alternativy, kde se model ML spustí, jenom místně nebo v cloudu.

## <a name="local-execution"></a>Místní spuštění

Model ML se spotřebovává místně, zatímco data se odesílají do cloudu pro přijímání, ukládání a další zpracování. Tato možnost je vhodná pro scénáře, ve kterých je prvotní detekce kritická.

![místní spuštění](assets/pdm-assets/localandcloud.png)

## <a name="cloud-execution"></a>Spuštění cloudu

Ingestování, zpracování a ukládání a provádění modelu ML může probíhat v cloudu Azure. Tato možnost může být lépe vhodná v případech, kdy při sdílení výsledků provádění modelu ML mezi několika klienty nebo geografickými oblastmi (a latencí není kritická). Volitelná součást, která se často označuje jako "hraniční brána", se dá místně přidat k provedení některých úkolů, jako jsou agregace a projekce dat, Stream Analytics atd., a to podle vzoru známého jako ["velvyslanc" vzoru](https://docs.microsoft.com/azure/architecture/patterns/ambassador?WT.mc_id=pdmsolution-docs-ercenk).

Model v Azure můžete použít několika způsoby. [Azure Machine Learning webová služba](https://docs.microsoft.com/azure/machine-learning/studio/consume-web-services?WT.mc_id=pdmsolution-docs-ercenk) je nejjednodušší a používá [Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/what-is-ml-studio?WT.mc_id=pdmsolution-docs-ercenk) jako volbu pro vytváření modelu. Jednu z těchto možností můžete také zvolit [Azure Machine Learning Správa modelů](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview?WT.mc_id=pdmsolution-docs-ercenk) , která poskytuje komplexní sadu služeb pro správu modelů a poskytuje REST API koncových bodů s ověřováním, vyrovnáváním zatížení, automatickými funkcemi škálování a šifrování. Model se dá nasadit do jednoho počítače (například Data Science Virtual Machine, zařízení IoT, místního počítače) nebo [Azure Container Service](https://docs.microsoft.com/azure/aks/intro-kubernetes?WT.mc_id=pdmsolution-docs-ercenk). Po zpřístupnění modelu prostřednictvím REST API jsou možnosti jeho použití nekonečné, od vlastních aplikací až po integraci podnikových řešení.

![Jen v cloudu](assets/pdm-assets/cloudonly.png)

Pouze cloudové nasazení neznamená nutně, že se bude jednat jenom o živý datový proud dat. Potenciální strategií je pravidelně exportovat data z místního systému (například historian nebo oddělení), importovat je do cloudového systému a prezentovat výsledek. Tato možnost může být vhodným přístupem, pokud zařízení nemůžou přímo do systému předávat data, stávající systémy už shromažďují data nebo nejsou potřeba žádné zpracování dat téměř v reálném čase. V těchto případech není nutné uvažovat o hraniční bráně.

## <a name="predictive-maintenance-in-the-iot-context"></a>Prediktivní údržba v kontextu IoT

Mnoho řešení IoT ingestuje a ukládá data jako součást jejich sady funkcí. A jako řešení prediktivní údržby často spoléhá na data IoT, může se jednat o přirozených funkcí přidaných do řešení IoT. Rozhodujícím bodem, který je potřeba zvýraznit v tomto kontextu, je důležitost, která nahrála chyby ve stávajících datech, aby se mohla naučit prediktivní model pro chyby.

Některé případy použití vyžadují zpracování dat téměř v reálném čase. V těchto případech potřebujeme škálovatelné řešení IoT s možnostmi vysoké rychlosti přijímání dat. Platforma Microsoft Azure poskytuje mnoho služeb, které umožňují řešení pro vysoce škálovatelné potřeby IoT. [Architektura řešení IoT od Microsoftu](https://docs.microsoft.com/azure/iot-suite/iot-suite-what-is-azure-iot?WT.mc_id=pdmsolution-docs-ercenk) na platformě Azure má logické komponenty ve třech fázích:

- Připojení zařízení

- Analýzy a zpracování dat

- Prezentace

![Architektura řešení IoT](assets/pdm-assets/iot.png)

Podrobnosti architektury řešení Azure IoT jsou [k dispozici online](https://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf?WT.mc_id=pdmsolution-docs-ercenk).
Existují však jedinečné výzvy, které mohou nastat v důsledku potenciálně významného počtu zařízení, která se připojují ke službám back-end.

## <a name="data-ingestion-and-stream-processing"></a>Přijímání dat a zpracování datových proudů

Uvedení dat ze zařízení představuje problém komunikace mezi dvěma samostatnými službami. To znamená, že systémy generující data (zařízení) a systémy zpracovávající tato data (tj. školení modelu ML s využitím příchozích datových bodů s vyškolený model pro předpověď zbývající doby životnosti).

Distribuované systémy, podle definice, se skládají z různých komponent s potřebnou vzájemnou komunikaci. Jedna možnost pro povolení komunikace může mít související součásti, které se vzájemně mluví přímo. Tím dojde k vytvoření systému, který je obtížné udržovat a škálovat. Vzhledem k tomu, že se zvyšuje počet komponent, vytvoří se složitost propojení odkazů _o (n<sup>2</sup>)_ . Lepší přístup je, kde jsou data publikována a načítána do a ze společného centra.

![Komunikace mezi součástmi](./assets/pdm-assets/subcomponentcommunication.png)

Vložením nové součásti pro příjem dat zajistíte škálovatelnost komunikace. Tato součást musí být škálovatelná, bezpečná a pravděpodobně globálně přístupná, a to s možností geografického dělení procesu přijímání dat. 

S ohledem na prediktivní údržbu je funkce řešení IoT. Jako datové proudy prostřednictvím brány musí být směrovány na služby související s funkcí prediktivní údržby.
Dalším vzorem, který je třeba zvážit, je [Směrování bran](https://docs.microsoft.com/azure/architecture/patterns/gateway-routing?WT.mc_id=pdmsolution-docs-ercenk).

Oba vzory se dají implementovat pomocí služby Azure, [IoT Hub](https://azure.microsoft.com/services/iot-hub/?WT.mc_id=pdmsolution-docs-ercenk) a [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/?WT.mc_id=pdmsolution-docs-ercenk).

## <a name="edge-and-cloud-processing-cooperation"></a>Spolupráce na hraničních zařízeních a v oblasti cloudového zpracování

Ne všechna zařízení a zařízení mají přístup k Internetu přímo a konzistentně.
Někdy je potřeba, aby se jejich data z běžné brány vyžádala. Například agenti [MTConnect](https://www.mtconnect.org/) poskytují pouze rozhraní REST pro přijímání dat.

Může se jednat o další okolnosti, jako je latence, nutnost před odesláním dat z místního zařízení do cloudu (víceklientské případy) a nutnost provádět projekce nebo agregace dat zařízení. [Vzor velvyslanců](https://docs.microsoft.com/azure/architecture/patterns/ambassador?WT.mc_id=pdmsolution-docs-ercenk) je dobrým přístupem k řešení těchto potřeb. [Microsoft Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works?WT.mc_id=pdmsolution-docs-ercenk) je implementace, která může fungovat jako proxy server [Microsoft Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/?WT.mc_id=pdmsolution-docs-ercenk)a také poskytovat možnosti místního zpracování pomocí vzdálené správy.

Běžné nasazení může zahrnovat výstrahy téměř v reálném čase v dílenském řízení, přičemž zároveň stále prochází a odesílá data do cloudového řešení v cloudu za účelem archivace, školení modelů a generování sestav nekritického času. Díky funkcím Azure IoT Edge a IoT Hub můžou zákazníci řídit možnosti filtrování dat na hraničním zařízení a také komunikovat s jinými systémy dílenského řízení k doručování výstrah.

![Více tenantů](assets/pdm-assets/multitenant.png)

## <a name="multitenant-perspective"></a>Perspektiva víceklientské architektury

Jak už bylo zmíněno dříve, někteří výrobci nebo třetí strany mohou chtít zákazníkům poskytovat prediktivní služby údržby. Tyto služby budou pravděpodobně nabízeny ve víceklientském nasazení cloudu, které představují svou vlastní sadu výzev:

### <a name="data-security-and-isolation"></a>Zabezpečení a izolace dat

Smluvní strana, která poskytuje službu, musí zajistit, aby byly identifikovány a správně zabezpečeny nebo popsány důvěrné informace od svých zákazníků. Microsoft Azure poskytuje možnosti pro šifrování dat v závislosti na použité službě úložiště.

Způsob, jakým zařízení generují a odesílají data, musí být také zabezpečená, a to pomocí známých metod, jako jsou certifikáty pro zařízení, povolení/zakázání, zabezpečení TLS, podpora X. 509, povolený/zakázaný přístup k IP adresám a zásady sdíleného přístupu. Smluvní strana, která poskytuje službu, musí zajistit, aby se důvěrné informace od zákazníků identifikovaly a správně zabezpečily nebo přecházely. Příklady služeb, které se dají použít k šifrování neaktivních dat, jsou [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-encryption?WT.mc_id=pdmsolution-docs-ercenk), [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?WT.mc_id=pdmsolution-docs-ercenk), [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest?WT.mc_id=pdmsolution-docs-ercenk)a [Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?WT.mc_id=pdmsolution-docs-ercenk) . Poskytovatelé řešení by také měli zvážit, jak rozdělit data buď v rámci stejného prostředku (např. databáze), nebo v několika dalších. 

### <a name="geographical-considerations"></a>Zeměpisné požadavky

Zařízení generující data budou pravděpodobně geograficky rozptýlená. Řešení potřebuje poskytnout bod příjmu nejblíže zdroji dat. Může nastat situace, kdy dochází k potížím s průběžným připojením a data bude možná potřeba ingestovat hromadně, nebo musí být k dispozici mechanismus místního úložiště a přeposílání.

### <a name="scalability"></a>Škálovatelnost

Sestavování modelů ML vyžaduje výpočetní prostředky, které se můžou elasticky škálovat.
Poskytovatel řešení musí navrhnout procesy, které umožňují efektivní využívání výpočetních prostředků, a používat procesy pro škálování řešení na vyžádání.

### <a name="provisioning-tenants-and-secure-access"></a>Zřizování klientů a zabezpečený přístup

Poskytovatel služeb potřebuje navrhnout metody pro efektivní připojování nových tenantů a poskytování prostředků pro správu svých účtů sami. To je také v případě, kdy se provádí rozhodnutí o nasazení na exkluzivní nebo sdílené prostředky.


## <a name="pillars-of-software-quality-review"></a>Pilíře přezkoumání kvality softwaru 

Složité systémy vyžadují další kontrolu, která je jiná než splnění funkčních požadavků. Úspěšná cloudová řešení se zaměřují na tyto pět pilířů, škálovatelnost, dostupnost, odolnost, správu a zabezpečení. Kromě pěti pilířů bychom také chtěli přinést nákladovou efektivitu řešení.

Podrobnosti najdete v článku o [kvalitě softwaru](https://docs.microsoft.com/azure/architecture/guide/pillars?WT.mc_id=pdmsolution-docs-ercenk) .

| Pilíř                      |                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Škálovatelnost                 | Většina služeb Azure podporuje vertikální a horizontální možnosti škálování. Využijte výhod nasazení prostředků na platformě Azure na vyžádání a řízení jejich škálování (velikosti a počtu instancí) prostřednictvím automatizovaných služeb.                                                                                                                                                                   |
| Dostupnost a odolnost | Výpočetní prostředky a prostředky úložiště je možné v případě potřeby zřídit pomocí řady služeb Azure. Všechny služby Azure poskytují různé úrovně SLA, ale řešení musí podle potřeby podle správných principů návrhu zvážit a využívat úrovně SLA.                                                                                                                    |
| Správa                  | Prostředky Azure je možné nasadit a spravovat různými prostředky, jako jsou například šablony ARM, nástroje příkazového řádku a rutiny prostředí PowerShell a rozhraní API pro správu Azure. Zvažte sestavování automatizovaných řešení při správě prostředků Azure místo používání nástrojů pomocí uživatelská rozhraní.                                                                                                                                |
| Zabezpečení                    | Azure IoT Hub podporuje symetrický a asymetrický klíč (certifikáty x509 a čip TPM) prostřednictvím protokolu TLS. Úložiště dat jsou chráněná pomocí nastavení nástroje pro správu identit a přístupu (IAM) a také podporují šifrování neaktivních uložených dat. Jako obecný kontrolní seznam zabezpečení v nejvyšší úrovni zvažte kontrolu autorizace, ověřování, přenosu, šifrování a omezení a mechanismy auditování. |
| Cenově výhodné              | V případě potřeby zvažte zřízení prostředků a jejich zahození, pokud je Služba Automation nepoužívá.                                                                                                                                                                                                                                                                                                  |

## <a name="conclusion"></a>Závěr

Prediktivní údržba je téma diskuze po dlouhou dobu. Poslední vývoj na cloudových platformách, jako je Microsoft Azure, umožňuje implementátorům provádět prediktivní údržbu a překonat mnoho výzev, které byly při práci s daty v minulosti překážkou. Díky elastickému škálování na výpočetní úrovni a kapacitě úložiště nabízejí cloudové platformy nové příležitosti pro implementaci prediktivní údržby a také nové příležitosti pro tržby. Platforma Azure od Microsoftu nabízí mnoho služeb s různými možnostmi, které umožňují dosáhnout obchodních cílů řešení prediktivní údržby.

V tomto článku se poskytla informace o tom, jak shromažďovat data a dávat datové modely a využívat k tomu vyškolený model, který v předchozích částech provede akci s předpokládanými výsledky.

## <a name="further-reading"></a>Další čtení

1. [Budoucí zaměření: Zamyslete se v minulosti a získejte před neočekávaným využitím IoT](https://blogs.microsoft.com/iot/2017/02/28/future-focused-stop-thinking-in-the-past-and-get-ahead-of-the-unexpected-with-iot-2/?WT.mc_id=pdmsolution-docs-ercenk)

2. [Zvýšení spolehlivosti zařízení pomocí prediktivní údržby s podporou IoT](https://www.microsoft.com/internet-of-things/predictive-maintenance?WT.mc_id=pdmsolution-docs-ercenk)

3. [Zachytit hodnotu z Internet věcí: Postup přístupu k projektu prediktivní údržby](https://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF?WT.mc_id=pdmsolution-docs-ercenk)

4. [Perspektiva partnerů: prediktivní Údržba na Frontlines](https://blogs.microsoft.com/iot/2017/03/21/partner-perspectives-predictive-maintenance-on-the-frontlines/?WT.mc_id=pdmsolution-docs-ercenk)

5. [Od commoditization do servitization: transformace vaší firmy na konkurenci v novém stáří služby pole s IoT](https://blogs.microsoft.com/iot/2016/11/07/from-commodization-to-servitization-transforming-your-business-to-compete-in-the-new-age-of-field-service-with-iot/?WT.mc_id=pdmsolution-docs-ercenk)
