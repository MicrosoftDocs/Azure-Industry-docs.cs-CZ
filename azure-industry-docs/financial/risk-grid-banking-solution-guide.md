---
title: Přehled – Azure Batch analýza rizik v Gridech, Azure Data Lake
author: dstarr
ms.author: dastarr
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Přináší technické aspekty implementace Azure Batch pro výpočetní funkce v bankovnictví.
ms.openlocfilehash: 542fb820870048ac2ec2cb67c2bbf13988588ea1
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77053177"
---
# <a name="risk-grid-computing-in-banking-solution-guide"></a>Průvodce pro řešení rizikové sítě v tabulce s bankovním řešením

Tento článek obsahuje technický přehled použití Microsoft Azure k podpoře a vylepšení výpočetního prostředí v síti s riziky v bankovnictví, včetně doporučených systémů a architektur na nejvyšší úrovni.

Tento dokument je určený pro architekty řešení a v některých případech techničtí pracovníci rozhodování, kteří chtějí podrobně na navrhovaná řešení pro rizikové computingy.

## <a name="introduction"></a>Úvod

Modely analýzy finančních rizik jsou obvykle zpracovávány jako dávkové úlohy s velkým objemem výpočetních dat generujících vysoké požadavky na výpočetní výkon, přístup k datům a analýzy. Poptávka po výpočetních výpočtech v tabulce rizik často roste v průběhu času a odpovídajícím způsobem se zvyšuje potřeba zvýšit nároky na výpočetní prostředky.

Široká škála dostupných produktů a služeb v Azure znamená, že pro většinu problémů může existovat více než jedno řešení. Tento článek poskytuje přehled technologií, vzorů a postupů vydaných v rámci řešení výpočetní sítě s rizikovou sítí v oblasti bankovnictví pomocí [Microsoft Azure Batch](/azure/batch/batch-dotnet-get-started?WT.mc_id=gridbanksg-docs-dastarr).

Azure Batch je bezplatná služba, která poskytuje nákladově efektivní a bezpečná řešení pro infrastrukturu i různé fáze dávkového zpracování, které se obvykle používají s modely s výpočetními tabulkami s rizikovou sítí. Azure Batch může rozšířit, rozšířit nebo dokonce nahradit aktuální místní investice k výpočetním prostředkům pomocí hybridních sítí nebo přesunutím celého procesu Batch do Azure.  Data mohou Procházet směrem nahoru a dolů z cloudu nebo zůstat v místním prostředí, zatímco jiná data mohou být zpracována výpočetními uzly v modelu shlukového přenosu v případě nízkého provozu místních prostředků.

## <a name="anatomy-of-a-batch-run"></a>Anatomie dávkového spuštění

V dávkovém spuštění jsou obvykle k dispozici alespoň dvě aplikace. Jedna aplikace, obvykle spuštěná na hlavním uzlu, odesílá úlohu do fondu a někdy orchestruje výpočetní uzly. Orchestraci je také možné nakonfigurovat prostřednictvím Azure Portal. Druhá aplikace je spouštěna výpočetními uzly jako úloha (viz obrázek 1).

Aplikace výpočetního uzlu provádí úlohu modelování souborů s riziky paralelního zpracování. Na výpočetních uzlech může být nainstalovaná a spuštěná víc než jedna aplikace.

Tyto aplikace je možné nahrávat prostřednictvím [rozhraní API služby Batch](/azure/batch/batch-apis-tools?WT.mc_id=gridbanksg-docs-dastarr)přímo prostřednictvím webu Azure Portal nebo prostřednictvím [příkazů Azure CLI pro službu Batch](/azure/batch/cli-samples?WT.mc_id=gridbanksg-docs-dastarr).

![Azure Batch grid computing](./assets/risk-grid-compute-assets/05-batch-grid-computing.png)

**Obrázek 1:** Azure Batch grid computing

Spuštění dávky se skládá z několika logických prvků. Obrázek 2 znázorňuje logický model úlohy Batch. Fond je kontejner pro virtuální počítače zapojené do dávkového spuštění a zřídí virtuální počítače výpočetních uzlů. Fond je také kontejner pro aplikace nainstalované na výpočetních uzlech. Úlohy se vytvářejí a spouštějí v rámci fondu. Úlohy jsou spouštěny úlohami. Úlohy jsou spuštění aplikace pracovní proces a jsou vyvolány instrukcí příkazového řádku.

Aplikace Worker se nainstaluje do výpočetního uzlu při jeho vytvoření.

![Fond, úlohy a úkoly](./assets/risk-grid-compute-assets/06-pool-job-logical-model.png)

**Obrázek 2:** Model konceptu logické dávky

Když se úloha spustí, fond zřídí všechny potřebné pracovní počítače pracovních procesů a nainstaluje pracovní aplikace. Úloha přiřadí úlohy do těchto výpočetních uzlů, které zase spustí instrukci příkazového řádku, který obvykle volá nainstalované aplikace nebo skripty.
Pomocí Batch se typicky sleduje vzor typický, který je popsaný takto:

1. Vytvořte skupinu prostředků, která bude obsahovat dávkové prostředky.
2. V rámci skupiny prostředků vytvořte účet Batch.
3. Vytvořte propojený účet úložiště.
4. Vytvořte fond, ve kterém se zřídí pracovní virtuální počítače.
5. Do fondu nahrajte aplikaci nebo skripty výpočetního uzlu.
6. Vytvořte úlohu, která přiřadí úlohy do virtuálních počítačů ve fondu.
7. Přidejte úlohu do fondu.
8. Spusťte dávkovou úlohu.
9. Úlohy zařadí do fronty úlohy, které se mají spouštět na výpočetních uzlech.
10. Výpočetní uzly spouštějí úlohy, protože virtuální počítače budou k dispozici.

Ilustrace tohoto procesu je znázorněna na obrázku 3.

![Proces dávkového spuštění](./assets/risk-grid-compute-assets/07-batch-run-process.png)

**Obrázek 3:** Model konceptu logické dávky

Po dokončení úloh může být užitečné odebrat výpočetní uzly, aby nedošlo k poplatkům, pokud se nepoužívají. Pokud je chcete odstranit prostřednictvím kódu nebo portálu, může jeden z nich odstranit daný fond. tím se odebere pracovní virtuální počítače.

Podrobnější pokyny o tom, jak začít s Batch, jsou k dispozici [pět minut rychlých startů](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr) , které vás provedou procesem v několika jazycích nebo prostřednictvím Azure Portal.

## <a name="batch-process-scheduling"></a>Plánování dávkového zpracování

Azure Batch má sestavený Plánovač, takže plánování každého spuštění lze definovat na portálu nebo prostřednictvím rozhraní API. Plánovač úloh služby Batch může definovat více plánů pro spuštění více úloh. Každá úloha má své vlastní vlastnosti, jako je například to, co dělat při zahájení a ukončení úlohy. Plány úloh je možné nastavit v opakovaných intervalech nebo jednorázovém spuštění.

Řada výpočetních systémů pro bankovou mřížku již má vlastní Plánovací službu. Do Azure možná nebude potřeba nic okamžitě přesunout Plánovač. To může pracovat bez problémů, protože Azure Batch může být vyvolán ručně nebo prostřednictvím sady SDK, což umožňuje, aby plánování stále probíhalo místně a úlohy, které mají být zpracovány v Azure.

K dávkovému zpracování může dojít na předem určeném plánu nebo na vyžádání, ale v obou případech není potřeba, aby virtuální počítače výpočetních uzlů byly aktivní, když se nepoužívají.  Pokud používáte stovky, pokud nejsou tisíce výpočetních uzlů virtuálních počítačů, můžou se významné úspory nákladů realizovat zrušením zřízení serverů, když jsou spuštěné úlohy ve frontě.

## <a name="compute-node-applications"></a>Aplikace výpočetního uzlu

Výpočetní uzly potřebují aplikaci, která se spustí při vyvolání úlohy. Tyto aplikace jsou zapsané firmou, aby prováděly úlohy zpracování po instalaci do pracovních procesů. V oblasti výpočetního prostředí pro bankovnictví se tato aplikace často stává úlohou transformace dat do formátů, zejména pro analýzu podřízeného data nebo pro jiné zpracování.

Když aplikaci poskytnete do fondu pro distribuci do výpočetních uzlů, nahraje se v balíčku aplikace. Balíček aplikace může být jiná verze dříve nahraného balíčku aplikace. Do jednoho výpočetního uzlu lze nainstalovat více než jeden balíček aplikace. Úloha obsahuje balíčky aplikací, které se mají načíst do pracovních počítačů.

Nasazení balíčku aplikace může být také spravováno verzí. Pokud byl do fondu načteno více verzí balíčku aplikace, může být určena konkrétní verze pro použití v dávkovém spuštění, jak je znázorněno na obrázku 4. To může být nezbytné v prostředích auditu nebo v případě, že podnik chce znovu vytvořit předchozí spuštění. Lze ji také použít pro účely vrácení se změnami, pokud je do aplikace pracovního procesu zavedena chyba.

![Proces dávkového spuštění](./assets/risk-grid-compute-assets/08-versioning-worker-applications.png)

**Obrázek 4:** Správa verzí aplikací úkolů výpočetních uzlů

Balíček aplikace se nahraje do fondu jako soubor. zip obsahující binární soubory aplikace a podpůrné soubory, které se vyžadují pro úlohy ke spuštění aplikace. Existují dva obory pro balíčky aplikací. Balíček aplikace můžete určit v oboru fondu nebo v rozsahu úkolů.

## <a name="pool-application-packages"></a>Balíčky aplikací fondu

Tyto balíčky se nasazují do všech výpočetních uzlů ve fondu. Když se zřídí, restartuje nebo obnoví virtuální počítač se výpočetním uzlem, nainstalují se nové kopie všech balíčků aplikací fondu, aby existovala aktualizovaná aplikace. Jeden nebo více balíčků aplikace lze přiřadit fondu, což znamená, že výpočetní uzly získají všechny určené balíčky.

## <a name="task-application-packages"></a>Balíčky aplikací úloh

Balíčky aplikací, které cílí na úroveň úlohy, se nainstalují jenom do výpočetních uzlů naplánovaných pro spuštění úlohy. Balíčky úloh aplikace jsou určené k použití v případě, že je více než jedna úloha spuštěna v jednom fondu.

Aplikace úkolů jsou užitečné při agregaci dat vytvořených úlohami na úrovni fondu, které mohou být relevantní v případě výpočetních scénářů v oblasti rizikových sítí. Například aplikace úloh může spustit sadu výpočtů rizik, které generují data, která se použijí později v pracovním postupu výpočtu rizik.

## <a name="scaling-batch-jobs"></a>Škálování dávkových úloh

Banky často provádějí dávkovou analýzu rizik za víkendy nebo v noci po nevyužití výpočetních prostředků. I když tento model funguje pro některé, může se rychle vypěstovat a vyžadovat větší kapitál, aby bylo možné do mřížky přidat další pracovní počítače.

Pokud spuštění dávkové úlohy trvá příliš dlouho nebo pokud chcete v dávce spustit více výpočetního výkonu, Azure nabízí několik možností.

1. Přidělte více počítačů výpočetních uzlů pro horizontální navýšení kapacity.
2. Přidělte výkonnější počítače výpočetních uzlů pro horizontální navýšení kapacity. Počítače Azure můžou být zřízené tak, aby splňovaly vysoce výkonné požadavky pro jádra a paměť a dokonce i výpočetní výkon GPU.

> Poznámka: používání sady Microsoft HPC Pack se službou Batch je složitější model a není popsané v tomto článku.

V clusteru služby Batch se může jednat o dva virtuální počítače pro zpracování nebo tisíce souběžných úloh spuštěných na tisících výpočetních uzlů virtuálních počítačů s desítkami tisíc jader. Každý virtuální počítač je zodpovědný za spuštění jedné úlohy v jednom okamžiku. Počet virtuálních počítačů ve fondu je možné škálovat ručně nebo automaticky podle konfigurace při zvýšení nebo snížení zátěže.

### <a name="burst-to-cloud"></a>Nárůst zatížení do cloudu

Když výpočetní prostředky v místní mřížce vycházejí z důvodu provádění rozsáhlých úloh analýzy, nabízí možnost "shlukování na Cloud" způsob, jak tyto prostředky rozšířit přidáním dalších výpočetních uzlů do Azure. Nárůst do cloudu je model, ve kterém privátní cloudy nebo infrastruktura distribuují své úlohy na cloudové servery, pokud je poptávka vysoká pro místní prostředky.

Tyto výpočetní uzly můžou být předem nakonfigurované jako virtuální počítače se systémem Linux nebo Windows, aby je bylo možné zřídit v IaaS platformě Azure. Servery je možné zřizovat a automaticky konfigurovat tak, aby fungovaly s existujícími investicemi, jako je TIBCO Gridserver a IBM Symphony.

### <a name="automatic-scaling-formulas"></a>Vzorce automatického škálování

Tuto pružnost lze nakonfigurovat v Azure Portal nebo pomocí [vzorců automatického škálování](/azure/batch/batch-automatic-scaling?WT.mc_id=gridbanksg-docs-dastarr). Vzorce automatického škálování jsou skripty odeslané do plánovače dávkového zpracování za účelem detailního řízení chování služby Batch. Automatické škálování ve fondu výpočetních uzlů se provádí přidružením uzlů ke vzorcům automatického škálování.

Níže je uveden příklad vzorce automatického škálování, který směruje automatické škálování na začátek s jedním virtuálním počítačem a podle potřeby škáluje až 50 virtuálních počítačů. Po dokončení úloh se virtuální počítače uvolní o jeden po jednom a vzorec automatického škálování zmenší fond.

```Formula
startingNumberOfVMs = 1;
maxNumberofVMs = 50;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

### <a name="other-scaling-techniques"></a>Další techniky škálování

Automatické škálování je možné povolit i pomocí rutiny Enable-AzureBatchAutoScale prostředí PowerShell. Rutina Enable-AzureBatchAutoScale umožňuje automatické škálování zadaného fondu. Následuje příklad.

1. První příkaz definuje vzorec a uloží jej do proměnné `$Formula`.
2. Druhý příkaz umožňuje automatické škálování ve fondu s názvem `RiskGridPool` pomocí vzorce v `$Formula`.

```console
C:\> $Formula = ‘startingNumberOfVMs = 1;
maxNumberofVMs = 50;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second?WT.mc_id=gridbanksg-docs-dastarr);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);’;

C:\> Enable-AzureBatchAutoScale -Id "RiskGridPool" -AutoScaleFormula $Formula -BatchContext $Context
```

Škálování se dá také provést pomocí Azure CLI pomocí příkazu `az batch pool resize` a webu Azure Portal.

## <a name="data-storage-and-retention"></a>Ukládání a uchovávání dat

Po ingestování a zpracování dat výpočetním uzlem můžou být výsledná výstupní data uložená v databázi pro další zpracování a analýzu nebo se transformují po ingestování před tím, než se zajistí správné formáty pro zpracování pro příjem dat. Microsoft Azure nabízí několik možností úložiště. Volba [technologie úložiště dat, která se má použít](/azure/architecture/data-guide/?WT.mc_id=gridbanksg-docs-dastarr) , závisí hlavně na analýze a na požadavcích vytváření sestav v rámci navazujících procesů.

Při použití hybridní sítě může být cíl úložiště dat místní. Při použití služby Batch přes hybridní síť můžou výpočetní uzly zapisovat data zpátky do místního úložiště dat bez použití umístění úložiště založeného na Azure. Zaměstnanci můžou také zapisovat do služby Azure File Storage, která se dá připojit jako disk na místním počítači a získat tak snadný přístup k jakémukoli procesu, který pracuje s místními soubory.

## <a name="monitoring-and-logging"></a>Monitorování a protokolování

Pro optimalizaci budoucích spuštění úlohy Batch by se měla zaznamenat data, která vám pomůžou identifikovat oblasti optimalizace. Pokud například zaměstnanci běží blízko kapacity procesoru, může přidání jader do výpočetních uzlů zabránit tomu, aby bylo vázáno na procesor a aby bylo možné úlohu dokončit rychleji. Každá aplikace spuštěná v dávkové úloze má své vlastní charakteristiky a optimalizace provedené u virtuálních počítačů v dávkovém spuštění se mohou lišit. Pro úlohy náročné na paměť je možné přidělit více paměti konfigurací různých počítačů při příštím spuštění.

Protokolování je možné provést pomocí aplikací výpočetního uzlu a hlaví mřížky nebo pomocí [dávkového protokolování diagnostiky](/azure/batch/batch-diagnostics?WT.mc_id=gridbanksg-docs-dastarr). Protokolování informací o výkonu dávkových běhů lze nakonfigurovat tak, aby bylo možné identifikovat oblasti, které se mají zlepšit pro lepší výkon a rychlejší dokončování úloh.

### <a name="custom-batch-monitoring-and-logging"></a>Vlastní sledování a protokolování dávky

Aplikace řízení a výpočetního uzlu můžou tato data vygenerovat a uložit pro další analýzu. Data, která jsou užitečná při optimalizaci dávkových úloh, zahrnují:

- Počáteční a koncový časy pro každý úkol
- Čas, kdy jsou jednotlivé výpočetní uzly aktivní a spouštějí úlohy
- Čas, kdy je každý výpočetní uzel aktivní a neběží úlohy
- Celková doba běhu úlohy Batch

### <a name="batch-diagnostic-logging"></a>Protokolování diagnostiky Batch

Existuje alternativa k vygenerování dat instrumentace pomocí aplikace kontroleru a výpočetního uzlu. [Protokolování dávkové diagnostiky](/azure/batch/batch-diagnostics?WT.mc_id=gridbanksg-docs-dastarr) může zachytit spoustu dat spuštění. Protokolování diagnostiky služby Batch není ve výchozím nastavení povolené a musí být povolené pro účet Batch.

Protokolování diagnostiky v programu Batch poskytuje významné množství dat, která se týkají řešení potíží s odstraňováním a optimalizací dávkových běhů. Počáteční a koncové časy pro úlohy a úlohy, počet jader, celkový počet uzlů a mnoho dalších metrik.

Dávkové protokolování vyžaduje pro vygenerované protokoly cíl úložiště a ukládá události generované dávkovým spuštěním, jako je vytváření fondů, provádění úloh, provádění úloh atd. Kromě ukládání událostí diagnostického protokolu v Azure Storage účet můžou události protokolu služby Batch být streamované do [centra událostí Azure](/azure/event-hubs/event-hubs-what-is-event-hubs?WT.mc_id=gridbanksg-docs-dastarr)a odesílají se do [Azure Log Analytics](/azure/log-analytics/log-analytics-overview?WT.mc_id=gridbanksg-docs-dastarr).

Pomocí těchto dat můžete optimalizovat základní výpočetní aplikace a aplikace hlavního uzlu. To může snížit náklady z důvodů, jako je rychlejší zrušení zřízení pracovních počítačů, pokud už je nepotřebujete, a ne čekat na dokončení spuštění dávky.

### <a name="batch-management-tools"></a>Nástroje pro správu dávky

Azure Portal poskytuje řídicí panel pro monitorování služby Batch, který zobrazuje informace o dávce jako spuštěné úlohy a dokonce i využití kvóty účtu. Je dostačující pro mnoho aplikací dávkových úloh.

Kromě nástrojů pro správu služby Batch a vizualizace dostupných v Azure Portal existuje bezplatný open source nástroj [BatchLabs](https://github.com/Azure/BatchLabs) pro správu služby Batch. Toto je samostatný klientský nástroj, který vám umožní vytvářet, ladit a monitorovat aplikace Azure Batch. Stažení instalačního balíčku pro Mac, Linux nebo Windows.

## <a name="network-models"></a>Síťové modely

Analýza rizik často vyžaduje stovky, pokud nejsou tisíce, z dokumentů, které se mají ingestovat do výpočetního procesu v oblasti rizikových sítí. Tyto soubory se často nacházejí místně v úložišti souborů, v síťové sdílené složce nebo v jiném úložišti. Při použití virtuálních počítačů založených na Azure pro přístup k těmto souborům a jejich zpracování je často vhodné, aby byla místní síť bezproblémově připojena k síti Azure, takže přístup k souborům je jednoduchý a rychlý. Tento přístup může dokonce znamenat, že kód, který provádí zpracování na výpočetních uzlech, nevyžaduje žádné změny kódu.

Azure nabízí dva modely pro bezpečné a spolehlivé připojení současných místních systémů k Azure, [Microsoft Azure ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbanksg-docs-dastarr) a [VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbanksg-docs-dastarr). Obě nabízí zabezpečené spolehlivé připojení, i když existují rozdíly v implementaci, výkonu a [dalších atributech](/azure/networking/networking-overview?WT.mc_id=gridbanksg-docs-dastarr).

Alternativně je hlavní uzel computingová mřížka náročné na místní provoz a spustí úlohu Batch prostřednictvím rozhraní REST API nebo sad SDK v rozhraní .NET a dalších jazycích.

Existují i další postupy pro přemostění mezer mezi Azure a místními prostředky bez řešení hybridní sítě. Další informace najdete níže.

### <a name="expressroute"></a>ExpressRoute

ExpressRoute spojuje vaši místní nebo Datacenter k Azure prostřednictvím privátního připojení, které usnadňuje partner pro připojení, jako je například aktuální poskytovatel internetových služeb (ISP)? WT. mc_id = gridbanksg-docs-dastarr). To umožňuje, aby se obě sítě vzájemně viděli jako stejná instance sítě a poskytovaly bezproblémový přístup mezi sítěmi. Integrace sítě je kritická, pokud chcete integrovat stávající místní systémy se sítí Azure a ExpressRoute nabízí nejrychlejší možné rychlosti připojení.

Další informace o cenách pro Azure ExpressRoute najdete [tady](https://azure.microsoft.com/pricing/details/expressroute/?WT.mc_id=gridbanksg-docs-dastarr).

### <a name="vpn-gateway"></a>VPN Gateway

VPN Gateway je jiný způsob, jak připojit síť k Azure. Nevýhodou tohoto modelu je přenos toků přes Internet. Připojení může být méně odolné jako výsledek a rychlosti sítě se nemůžou spojit s ExpressRouteem, ale nemusí se jednat o překážku pro výpočetní scénář v oblasti rizikových sítí, protože čtení datových souborů je obvykle rychlá operace.

Další informace o cenách pro VPN Gateway najdete [tady](https://azure.microsoft.com/pricing/details/expressroute/?WT.mc_id=gridbanksg-docs-dastarr).

### <a name="choices-for-connectivity-details"></a>Volby pro podrobnosti o připojení

Existují v podstatě dva modely pro rozšíření vaší sítě do Azure, jak je znázorněno na obrázku 5.

1. Virtuální brána – site-to-site
2. ExpressRoute – Exchange nebo poskytovatel internetových služeb

![Lokalita do lokality a ExpressRoute](./assets/risk-grid-compute-assets/10-s2s-expressroute.png)

**Obrázek 5:** Site-to-site a ExpressRoute

#### <a name="virtual-gateway-site-to-site-integration"></a>Integrace mezi lokalitami virtuální brány

[Síť Site-to-site VPN Gateway](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal?WT.mc_id=gridbanksg-docs-dastarr) připojuje vaši místní síť k virtuální síti Azure. Toto přemostění mezi sítěmi v podstatě přiřadí části stejné sítě se dvěma způsoby přístupu k prostředkům, serverům a artefaktům. To umožňuje přímý přístup k datovým souborům z virtuálních počítačů Azure Worker s využitím úlohy Batch s rizikovou sítí.

#### <a name="expressroute-integration"></a>Integrace ExpressRoute

Připojení ExpressRoute, které usnadňuje poskytovatel služby Azure Partner Network, využívá stejné výhody jako připojení typu Site-to-site, ale s vyššími rychlostmi a spolehlivostí.

Získat další informace o [modely připojení ExpressRoute] (/Azure/ExpressRoute/ExpressRoute-Connectivity-Models? WT. mc_id = gridbanksg-docs-dastarr).

### <a name="batch-processing-without-an-azure-hybrid-network"></a>Dávkové zpracování bez hybridní sítě Azure

Další scénář služby Batch odesílá všechny datové soubory do úložiště Azure pro pozdější zpracování výpočetními počítači založenými na Azure. Úložiště souborů a BLOB Storage jsou nejspíš kandidáty na ukládání dat v tabulce rizikového zpracování sítě.

V tomto scénáři je řadič úlohy a všechny výpočetní uzly v Azure v provozu, jak ukazuje obrázek 6. Pravděpodobný cíl pro zpracovaná data je úložiště dat Azure, které je v přípravě na další zpracování Azure Machine Learning řešeními nebo jinými systémy. Toto dodatečné zpracování překračuje rozsah tohoto článku.

![Lokalita do lokality a ExpressRoute](./assets/risk-grid-compute-assets/09-batch-upload-process.png)

**Obrázek 6:** Dávkové nahrávání do životního cyklu provádění

### <a name="hybrid-network-connectivity-resources"></a>Prostředky připojení k hybridní síti

Ve vaší situaci může být možné použít několik konfigurací. Pomoc s rozhodováním a pokyny pro architekturu týkající se propojení síťového připojení k Azure najdete v článku _[připojení místní sítě k Azure](/azure/architecture/reference-architectures/hybrid-networking/?WT.mc_id=gridbanksg-docs-dastarr)_ pomocí vzorových &ch postupů.

- [V tomto článku najdete informace](/azure/vpn-gateway/vpn-gateway-about-vpngateways?WT.mc_id=gridbanksg-docs-dastarr) o VPN Gateway alternativách konfigurace.
- Seznamte se s [modely připojení ExpressRoute](/azure/expressroute/expressroute-connectivity-models?WT.mc_id=gridbanksg-docs-dastarr).
- Vypočítat [ceny ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/?WT.mc_id=gridbanksg-docs-dastarr)
- Vypočítá [ceny VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway/?WT.mc_id=gridbanksg-docs-dastarr).

## <a name="security-considerations"></a>Aspekty zabezpečení

Může se vytvořit Azure [Virtual Network (VNET)](/azure/virtual-network/virtual-networks-overview?WT.mc_id=gridbanksg-docs-dastarr) a výpočetní uzly fondu, které jsou v něm vytvořené. Získáte tak další úroveň izolace pro dávkovou úlohu a povolíte ověřování pomocí [Azure Active Directory (AAD)](/azure/active-directory/active-directory-whatis?WT.mc_id=gridbanksg-docs-dastarr). Další informace najdete v tématu [Konfigurace sítě fondu](/azure/batch/batch-api-basics#pool-network-configuration?WT.mc_id=gridbanksg-docs-dastarr) .

Existují dva způsoby, jak pomocí AAD ověřit aplikaci Batch:

1. **Integrované ověřování**. Dávková aplikace používající účty AAD může účet použít k získání prostředků do úložišť dat a dalších prostředků.

2. **Instanční objekt** Objekty služby AAD definují zásady přístupu a oprávnění pro uživatele a aplikace. Instanční objekt poskytuje ověřování uživatelům pomocí tajného klíče vázaného na danou aplikaci. To umožňuje ověřování bezobslužné aplikace s tajným klíčem. Instanční objekt definuje zásadu a oprávnění pro aplikaci, která představuje aplikaci při přístupu k prostředkům za běhu. [Další informace najdete tady](/azure/active-directory/develop/active-directory-application-objects?WT.mc_id=gridbanksg-docs-dastarr).

Další informace o zabezpečení v dávkovém zpracování pomocí AAD [najdete v tomto článku](/azure/batch/batch-aad-auth?WT.mc_id=gridbanksg-docs-dastarr).

Služba Batch se také může ověřit pomocí sdíleného klíče. Ověřovací služba vyžaduje, aby se do požadavku HTTP, dat a autorizace přidaly dvě hodnoty hlaviček. [Další informace](/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service?WT.mc_id=gridbanksg-docs-dastarr) o ověřování sdíleného klíče najdete tady.

## <a name="cost-considerations"></a>Důležité informace o nákladech

Za použití Azure Batch se neúčtují žádné poplatky. Platíte jenom za spotřebované základní prostředky, třeba pro dobu provozu, úložiště a sítě virtuálních počítačů. Virtuální počítače výpočetního uzlu se ale pořád účtují za nečinné, takže je vhodné zrušit zřízení počítačů, když už je nepotřebujete. To se často provádí odstraněním fondu obsahujícího tyto objekty.
>
Při vytváření fondu můžete určit typy výpočetních uzlů, které chcete, a počet každého z nich. Následující dva typy výpočetních uzlů:
>
>**Vyhrazené výpočetní uzly** jsou rezervované pro vaše úlohy. Jsou dražší než uzly s nízkou prioritou, ale nabízejí záruku toho, že nikdy nedojde k jejich zrušení.
>
>**Výpočetní uzly s nízkou prioritou** využívají nadbytečné kapacity v Azure ke spouštění dávkových úloh. Uzly s nízkou prioritou mají nižší hodinové náklady než vyhrazené uzly a umožňují používat úlohy náročné na výpočetní výkon. Další informace najdete v tématu [Použití virtuálních počítačů s nízkou prioritou ve službě Batch](/azure/batch/batch-low-pri-vms?WT.mc_id=gridbanksg-docs-dastarr).
>
>Ve stejném fondu můžou existovat vyhrazené uzly s nízkou prioritou.
>
>Informace o cenách výpočetních uzlů s nízkou prioritou a vyhrazených uzlů najdete v tématu [Ceny služby Batch](https://azure.microsoft.com/pricing/details/batch/?WT.mc_id=gridbanksg-docs-dastarr).

Při použití služby Batch Diagnostic Logging se data vygenerovaná do Azure Storage účtují za cenu. Jedná se o data úložiště jako jakákoli jiná data a ceny se tím ovlivňují množství uchovávaných diagnostických dat.

## <a name="getting-started"></a>Začínáme

I když máte spoustu míst, kde začínáte složitou doménou, jako je Batch Computing, pro výpočetní práci v tabulce rizikových sítí, tady jsou některé logické startovací body, které lépe pochopí technologii Batch.

[Toto je skvělé místo pro zahájení](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr) práce se službou Batch s využitím příkladů kódování a Azure Portal. Ukázkové aplikace Azure Batch jsou také volně dostupné [na GitHubu](https://github.com/Azure/azure-batch-samples).

Níže jsou uvedeny některé rychlé kurzy, které vám pomůžou vytvořit jednoduchou aplikaci pro vytváření a spouštění výpočetních úloh Batch. Možnosti pro sestavování aplikace jsou následující:

- [Rozhraní Batch .NET API](/azure/batch/batch-dotnet-get-started?WT.mc_id=gridbanksg-docs-dastarr)
- [Sada Batch SDK pro Python](/azure/batch/batch-python-tutorial?WT.mc_id=gridbanksg-docs-dastarr)
- [Sada Batch SDK pro Node. js](/azure/batch/batch-nodejs-get-started?WT.mc_id=gridbanksg-docs-dastarr)
- [Správa dávek pomocí PowerShellu](/azure/batch/batch-powershell-cmdlets-get-started?WT.mc_id=gridbanksg-docs-dastarr)
- [Správa služby Batch pomocí Azure CLI](/azure/batch/batch-cli-get-started?WT.mc_id=gridbanksg-docs-dastarr)

Vezměte v úvahu, jak spustit iniciativu pro vyhodnocování konceptů. Jaký bude váš přístup k přijímání dat do Azure? Použijete hybridní síť nebo nahrajete data prostřednictvím sady SDK nebo rozhraní REST? Pokud zvažujete síť bybrid, zvažte spuštění pilotního projektu, na který se bude dát.

Vyhodnoťte velikost výpočetních úloh služby Batch a vyberte správné řešení škálování. [Vzorce automatického škálování](/azure/batch/batch-automatic-scaling?WT.mc_id=gridbanksg-docs-dastarr) umožňují složité scénáře plánování, zatímco jednodušší scénáře jsou achivable pomocí Azure Portal.

## <a name="technologies-presented"></a>Prezentované technologie

[Azure Batch](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr) poskytuje možnosti pro spouštění rozsáhlých úloh paralelního zpracování ve velkém měřítku v cloudu.

[Azure Active Directory](/azure/active-directory/active-directory-whatis?WT.mc_id=gridbanksg-docs-dastarr) je víceklientské cloudové adresáře a služba pro správu identit, které kombinují základní adresářové služby, správu přístupu k aplikacím a ochranu identit do jediného řešení.

[Vzorce automatického škálování](/azure/batch/batch-automatic-scaling?WT.mc_id=gridbanksg-docs-dastarr) jsou skripty odeslané do plánovače dávkového zpracování za účelem detailního řízení chování při škálování dávky.

[Protokolování služby Batch Diagnostics](/azure/batch/batch-diagnostics?WT.mc_id=gridbanksg-docs-dastarr) je funkce Azure Batch, která umožňuje vytvoření podrobného protokolu z vašich dávkových běhů a generovaných událostí. Protokoly jsou uloženy v Azure Storage.

[BatchLabs](https://github.com/Azure/BatchLabs) je samostatná aplikace pro systémy Windows, MacOS a Linux, která je k dispozici pro monitorování a správu.

[ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbanksg-docs-dastarr) je vysoce rychlá a spolehlivá řešení hybridní sítě pro připojení k místním i sítím Azure.

[VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbanksg-docs-dastarr) je hybridní síťové řešení využívající Internet pro připojení k místním i sítím Azure.

## <a name="conclusion"></a>Závěr

Tento dokument poskytuje přehled o technických řešeních a doporučeních pro použití Azure Batch pro práci v oblasti rizikových sítí pro bankovnictví. Tento článek se věnuje hodně doplněním z definice Azure Batch [možností sítě](/azure/architecture/reference-architectures/hybrid-networking/?WT.mc_id=gridbanksg-docs-dastarr)a dokonce i s ohledem na náklady.

Při zvažování přesunu při vyhodnocování Azure Batch v oblasti výpočetního prostředí s rizikovou sítí je [Tato stránka](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr) dobrým prostředkem pro zahájení práce. Poskytuje ukázková výuková průvodce pro paralelní zpracování souborů, což je podstata výpočetního prostředí v oblasti rizikových sítí. Kurzy jsou k dispozici prostřednictvím webu Azure Portal, Azure CLI, .NET a Pythonu.
