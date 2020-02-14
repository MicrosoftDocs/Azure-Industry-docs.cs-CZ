---
title: Architektura pro extrakci přehledů z IoT pro výrobu
description: Získejte přehledy z dat IoT pomocí služeb Azure.
author: ercenk
ms.author: ercenk
manager: gmarchet
ms.service: industry
ms.topic: article
ms.date: 11/28/2019
ms.openlocfilehash: c08e6bbb1da47084122dae1ed6a9e1cea0b59473
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77053398"
---
# <a name="extracting-actionable-insights-from-iot-data"></a>Extrakce užitečných poznatků z dat IoT

Pokud zodpovídáte za počítače v produkčním prostředí, už jste si vědomi, že Internet věcí (IoT) je dalším krokem při vylepšení procesů a výsledků. Je prvním krokem, když máte snímače na počítačích nebo na továrním patře. Dalším krokem je použití dat, což je objekt tohoto dokumentu. Tato příručka poskytuje technický přehled komponent potřebných k extrakci užitečných poznatků z analýzy dat IoT.

Řešení IoT Analytics slouží k transformaci nezpracovaných dat IoT ze sady zařízení do formuláře, který je lépe vhodný pro analýzu. Jakmile jsou data ve formuláři analyzovatelné, můžeme ve skutečnosti provádět analýzy. Mezi příklady analýz patří:

- jednoduché vizualizace dat telemetrie, například pruhový graf zobrazující teploty v čase
- Výpočet klíčových ukazatelů výkonu, například celkové efektivity vybavení (celkové efektivity zařízení)
- prediktivní analýza s využitím modelů strojového učení

Tyto analýzy pak poskytují přehledy, které informují o akcích. Akce můžou být v rozsahu odeslání jednoduchého příkazu do počítače, aby se prováděla úprava provozních parametrů, aby se mohla provádět akce v jiném softwarovém systému, a to kvůli implementaci programů pro provozní vylepšení v celé společnosti.

Následující obrázek ukazuje řetěz akcí, ke kterým dochází mezi počítačem výrobního procesu a konečným výsledkem, což je reprezentace řídicího panelu "využití", ukazující graf a popisek "87,5%".

![Z továrny do řídicího panelu.](assets/extracting-insights-from-iot/factory-to-dashboard.png)

Pro účely ilustrace budeme používat výpočet jednoduchého klíčového ukazatele výkonu: využití počítače. *Využití počítače* představuje procento času, který počítač ve skutečnosti *vyrábí* . Například pokud je v směně 8 hodin a počítač vyrábí součásti na 7 hodin, pak je využití počítače pro tento posun 87,5% (7/8 × 100).

## <a name="approach"></a>Přístup

Aplikace IoT mají tři komponenty: **věci** (nebo zařízení), odesílají data nebo události, které se používají ke generování **přehledů**, které se používají ke generování **akcí** , které vám pomohou vylepšit firmu nebo proces.

Zařízení ve výrobní závodě (věci) odesílá různé typy dat, když to funguje. Příkladem je počítač pro frézování, který odesílá rychlost kanálu a data o teplotě. používá se k vyhodnocení, jestli počítač běží, nebo ne – vhled, který se používá k optimalizaci závodu – akce. Provedeme vás kroky pro extrakci dat, vizualizaci na řídicím panelu, extrakci nových přehledů a provedení dalších akcí.
![věcí k přehledům akcí.](assets/extracting-insights-from-iot/things-insights-actions.png)

Společnost Microsoft publikovala referenční architekturu s vysokou úrovní pro aplikace IoT, která projde různými subsystémy a doporučenými přístupy pro tyto subsystémy.
Aplikace IoT se skládá z následujících subsystémů:

1. Zařízení nebo místní hraniční brány, což jsou konkrétní druh zařízení, které může bezpečně registrovat zdroje zpráv (zařízení) s cloudem. Hraniční brána může také transformovat zprávy z nativního protokolu na jiný formát (například JSON).
2. Cloudová služba brány nebo centrum (například [azure IoT Hub](/azure/iot-hub/?WT.mc_id=iotinsightssoln-docs-ercenk) nebo [Azure Event Hubs](/azure/event-hubs/event-hubs-about?WT.mc_id=iotinsightssoln-docs-ercenk)), které slouží k bezpečnému ingestování dat a poskytování možností správy zařízení. 
3. Proudové procesory, které využívají streamovaná data. Procesory mohou být také integrovány s obchodními procesy a umísťují data do úložiště.
4. Uživatelské rozhraní ve formě řídicího panelu k vizualizaci dat IoT a usnadnění správy zařízení.

![Architektura řešení.](assets/extracting-insights-from-iot/architecture.png)

V tomto článku se zaměříme na proces extrahování přehledů. Toto jsou hlavní kroky:

1. Přístup k datům a jejich zpracování do datového proudu.
2. Zpracujte a uložte data.
3. Vizualizujte nebo Prezentujte data. 

Na následujícím obrázku je diagram znázorňující tok dat ze zdroje dat, který slouží k převodu, k ingestování, ke zpracování a uložení, k prezentaci a k akcím.

![Data fow: zdroj, který se ingestuje do prezentace a akci.](assets/extracting-insights-from-iot/data-flow.png)

## <a name="converting-the-data-to-a-stream"></a>Převod dat na datový proud

Data IoT jsou data časových řad: hodnoty od "věcí", které mohou být smysluplnější v časovém intervalu. Vybavení na podlaze podniku funguje v průběhu času a během této doby dojde k událostem. Pokud se data v inventáři neodesílají do služby pro příjem dat, jako je třeba [Azure IoT Hub](/azure/iot-hub/?WT.mc_id=iotinsightssoln-docs-ercenk), musíme data pravidelně dotazovat ze svého úložiště, například do výrobního systému pro spouštění nebo z koncového bodu http, a posílat data do služby ingestování.  
Pro převod dat na datový proud máme obvykle:

1. Přístup ke zdroji dat.
2. Transformujte a obohacente data.
3. Publikujte data do koncového bodu, který může ingestovat streamovaná data.

Nepokrýváme vám podrobně přístup ke zdroji dat, protože přístup závisí na tom, kde se data nacházejí, a variace jsou příliš mnoho.

## <a name="transforming-and-enriching-the-data"></a>Transformace a rozšíření dat

Nezpracovaná data musí obvykle podléhat transformačním operacím, aby je bylo možné sjednotit a připravit na přijímání.  Konkrétní transformace se budou lišit v závislosti na použitém typu analýzy.  Mezi běžné příklady transformací dat patří data časových řad, ve kterých může chybět měření a musí být zavedená, nebo kde je potřeba vyvažovat časová měřítka v různých počítačích.  Chtěli bychom mít záznamy dat, které mají časové razítko (Pokud je zdroj obsahuje), a v páru název-hodnota. Data se běžně nacházejí v hierarchickém formátu a je nutné je transformovat do ploché struktury.

Následující obrázek ukazuje hierarchickou datovou strukturu s zubatým profilem, která je převedena na standardizovaný formát sloupce a řádku (blok).

![Datový tvar z hierarchického na plochý.](assets/extracting-insights-from-iot/hierarchy-to-flat.png)

Data nejsou většinou dostupná z Internetu. Běžným způsobem je použít hraniční bránu k odesílání dat z továrny do bodu ingestování. [Azure IoT Edge](/azure/iot-edge?WT.mc_id=iotinsightssoln-docs-ercenk) je služba, která se sestavuje nad IoT Hub. Zařízení IoT Edge může fungovat jako brána, aby kryla tři [vzory](/azure/iot-edge/iot-edge-as-gateway?WT.mc_id=iotinsightssoln-docs-ercenk): transparentní bránu, překlad protokolu a překlad identity.

Pokud jsou data k dispozici externě a jsou přístupná z Internetu, můžete k nim použít několik služeb Azure pro přístup k datům, jejich transformaci a rozšíření. Mezi tyto možnosti patří:

- Vlastní kód nasazený v různých výpočetních službách Azure, jako je [App Service](/azure/app-service/?WT.mc_id=iotinsightssoln-docs-ercenk), [Služba Azure Kubernetes](/azure/aks/?WT.mc_id=iotinsightssoln-docs-ercenk) (AKS), [Container Instances](/azure/container-instances/?WT.mc_id=iotinsightssoln-docs-ercenk)nebo [Service Fabric](/azure/service-fabric/service-fabric-overview?WT.mc_id=iotinsightssoln-docs-ercenk).
- [Azure Logic Apps](/azure/logic-apps/?WT.mc_id=iotinsightssoln-docs-ercenk)
- [Kanály a aktivity ve službě Azure Data Factory](/azure/data-factory/copy-activity-overview/?WT.mc_id=iotinsightssoln-docs-ercenk)
- [Azure Functions](/azure/azure-functions/functions-overview)
- [BizTalk Services](https://azure.microsoft.com/services/biztalk-services/)

Každá z výše uvedených služeb má své výhody a náklady, a to v závislosti na scénáři. Logic Apps například poskytuje způsob, jak [transformovat dokumenty XML](/azure/logic-apps/logic-apps-enterprise-integration-transform?WT.mc_id=iotinsightssoln-docs-ercenk). Data však mohou být ve složitém dokumentu XML, takže nemusí být praktické vyvíjet velký skript XSLT pro transformaci dat. V takovém případě může jeden vytvořit hybridní řešení s využitím více mikroslužeb z různých služeb Azure. Například mikroslužba implementovaná v Azure Logic Apps se může dotazovat na koncový bod HTTP, ukládat nezpracovaný výsledek dočasně a upozorňovat na další mikroslužby. Druhá mikroslužba – která transformuje zprávu – může být vlastní kód hostovaný na [Azure Functions hostitele](https://github.com/Azure/azure-functions-host).  

![Protokol HTTPS se dotazuje na data a zpracovává je pomocí funkcí.](assets/extracting-insights-from-iot/poll-logic-process.png)

Případně se můžete rozhodnout pro pracovní postup orchestrující Azure Data Factory, kde posloupnost aktivit provádí transformace. Další podrobnosti o typu dostupných aktivit najdete v tématu [kanály a aktivity v Azure Data Factory](/azure/data-factory/concepts-pipelines-activities).
Zprávy mohou být časové razítko při příjmu, nebo mohou obsahovat časové razítko, které nám umožní rekonstruovat časovou řadu několika měřených hodnot. Proto je zanedbatelná latence příjmu a vysoká propustnost zásadní, aby se zaručila integrita informací a časová osa s případnými odezvami. Abychom minimalizovali latenci, normalizují se časová razítka co nejblíže k podniku.

## <a name="ingesting-the-data-stream"></a>Ingestování datového proudu

Pokud chcete data analyzovat jako datový proud, můžeme pro identifikaci vzorů a vztahů provádět dotazy na data na základě časových oken. Platforma Azure obsahuje různé služby, které mohou ingestovat data při vysoké propustnosti.
Výběr mezi službami závisí na potřebách projektu, jako je Správa zařízení, podpora protokolů, škálovatelnost, preference týmu programovacího modelu atd. Například tým může mít přednost při použití Kafka z důvodu jejich používání nebo pro řešení musí mít zprostředkovatele Kafka. Nebo pro jiný případ může projekt potřebovat systém pro příjem dat, který umožňuje využít [ověření klíče TPM IoT Hub Device Provisioning Service](/azure/iot-dps/?WT.mc_id=iotinsightssoln-docs-ercenk) k zabezpečení přístupu zařízení k bodu přijímání.

- [Azure IoT Hub](/azure/iot-hub/?WT.mc_id=iotinsightssoln-docs-ercenk) je obousměrné komunikační centrum mezi aplikacemi IoT a zařízeními. Jedná se o škálovatelnou službu, která umožňuje plně vybavená řešení IoT, protože zajišťuje zabezpečenou komunikaci, směrování zpráv, integraci s dalšími službami Azure a funkce správy pro řízení a konfiguraci zařízení.

- [Azure Event Hubs](/azure/event-hubs/event-hubs-about?WT.mc_id=iotinsightssoln-docs-ercenk) je vysoce škálovatelná služba, která slouží ke shromažďování dat telemetrie z souběžných zdrojů za účelem překročení vysoké míry propustnosti.
- [Apache Kafka v HDInsight](/azure/hdinsight/kafka/apache-kafka-introduction?WT.mc_id=iotinsightssoln-docs-ercenk) je spravovaná služba, která hostuje [Apache Kafka](https://kafka.apache.org/). Apache Kafka je open source platforma pro distribuované streamování, která poskytuje také funkce zprostředkovatele zpráv. Hostovaná služba má smlouva SLA (SLA) 99,9% v době provozu Kafka.

## <a name="processing-and-storing-the-data"></a>Zpracování a ukládání dat

Aplikace IoT přináší výzvy, protože se jedná o systémy založené na událostech, které také potřebují uchovávat historická data a pracovat s nimi. Příchozí data jsou připojená typ dat a můžou potenciálně růst velká. Je potřeba uchovávat data po delší dobu, hlavně z těchto důvodů: archivace, dávková analýza a vytváření modelů strojového učení (ML). Na druhé straně je datový proud událostí velmi důležitý pro detekci anomálií v reálném čase za účelem detekce anomálií, rozpoznávání vzorů v průběhu vracení časových oken nebo spouštění výstrah, pokud hodnoty přestanou nad nebo pod prahovou hodnotou.
Referenční architektura Azure IoT od Microsoftu prezentuje doporučený tok dat pro zařízení do cloudových zpráv a událostí v řešení IoT pomocí architektury lambda. Architektura lambda umožňuje analýzu dat pro streamování téměř v reálném čase, a také archivovaných dat, což dává nejlepší možnost pro zpracování příchozích dat.

## <a name="lambda-architecture"></a>Architektura lambda

Architektura lambda řeší tento problém vytvořením dvou cest pro tok dat. Těmito dvěma cestami procházejí všechna data, která přicházejí do systému:

- Dávková vrstva (studená cesta) ukládá všechna příchozí data v nezpracované podobě a provádí dávkové zpracování dat. Výsledek tohoto zpracování je uložen jako dávkové zobrazení. Je to pomalé zpracování kanálu, který spouští složitou analýzu, například kombinování dat z více zdrojů a delší dobu (v hodinách, dnech nebo déle) a generování nových informací, jako jsou sestavy, modely strojového učení, etcetera.
- Rychlost (teplé cesty) analyzuje data v reálném čase. Tato vrstva je určená pro nízkou latenci, ovšem na úkor přesnosti. Je rychlejší kanál pro zpracování, který archivuje a zobrazuje příchozí zprávy, a analyzuje tyto záznamy generující krátkodobé kritické informace a akce, jako jsou například alarmy.
- Dávková vrstva dávkové vrstvy do "obsluhované vrstvy", která reaguje na dotazy. Dávková vrstva indexuje dávkové zobrazení pro efektivní dotazování. Rychlostní vrstva přidává do obslužné vrstvy přírůstkové aktualizace založené na nejnovějších datech.

Následující obrázek znázorňuje pět bloků, které reprezentují fáze transformace. Prvním blokem je datový proud, který paralelně přechází mezi vrstvou rychlosti i dávkovou vrstvu. Obě vrstvy jsou pokryté vrstvou obsluhy, vrstva rychlosti a obsluha vrstvy oba zakládá klienta Analytics.
![architektuře lambda.](assets/extracting-insights-from-iot/lambda-schematic.png)

 Platforma Azure poskytuje různé služby, které se dají použít k implementaci architektury. Následující diagram znázorňuje, jak lze tyto služby namapovat k implementaci. Obrázek znázorňuje pět fází transformace s každou fází obsahující relevantní technologie Azure. Tmavší barevná pole označují dostupnost více možností provádění těchto úloh.

![Architektura lambda.](assets/extracting-insights-from-iot/lambda-architecture-all.png)

Možnosti pro službu ingestování dat na vrstvě rychlosti jsou uvedené v předchozí části ingestování datového proudu.

[Apache Kafka v HDInsight](/azure/hdinsight/kafka/apache-kafka-introduction?WT.mc_id=iotinsightssoln-docs-ercenk) může být možnost služby, která implementuje datový proud jak pro službu příjmu dat, tak pro zpracování datových proudů.

Pokud používáte Event Hubs pro službu ingestování dat, použijte [Azure Stream Analytics](/azure/stream-analytics?WT.mc_id=iotinsightssoln-docs-ercenk) (ASA). Azure Stream Analytics je modul pro zpracování událostí, který umožňuje zkoumat velké objemy dat streamované ze zařízení. Příchozí data můžou pocházet ze zařízení, senzorů, webů, informačních kanálů sociálních médií, aplikací a dalších. Podporuje také extrahování informací ze streamů, identifikování vzorů a relace.

Stream Analytics dotazy začínají zdrojem streamování dat, která se ingestují do centra událostí Azure, Azure IoT Hub nebo z úložiště dat, jako je Azure Blob Storage. Pokud chcete prošetřit datový proud, vytvořte úlohu Stream Analytics, která určuje vstupní zdroj, který streamuje data. Úloha také určuje transformační dotaz, který definuje, jak vyhledávat data, vzory nebo relace. Transformační dotaz využívá dotazovací jazyk typu SQL, který se používá k filtrování, řazení, agregaci a připojení streamovaných dat v časovém období.

## <a name="warm-path"></a>Cesta k teplému

Ukázkový scénář pro tento dokument je klíčový ukazatel výkonu využití počítače (zavedený na začátku průvodce). Mohli bychom Naive interpretovat data a předpokládat, že pokud počítač odesílá data, využije se. Počítač ale může odesílat data, ale ve skutečnosti nevyrábí cokoli (například může být nečinný nebo se udržuje). Tím se při pokusu o extrakci informací z dat IoT vysvětluje velmi častý problém: hledaná data nejsou k dispozici v datech, která získáváte. V našem příkladu tedy nezískáváme data jasně a jednoznačně oznamujeme, zda počítač vyrábí nebo ne.  Proto je potřeba odvodit využití kombinováním dat, která shromažďujeme s ostatními zdroji dat, a použít pravidla k určení toho, jestli počítač vyrábí nebo ne. Kromě toho se tato pravidla mohou změnit od společnosti na firmu, protože mohou mít různé výklady toho, co produkce vyrábí. Doba zahřívání je veškerá o analýze dat v systému. Tento Stream zpracováváme prakticky v reálném čase, uložíme ho do teplého úložiště a nahrajeme ho do klientů pro analýzu.

Azure Event Hubs je platforma pro zpracování velkých objemů dat a služba pro příjem událostí, která dokáže přijímat a zpracovávat miliony událostí za sekundu. Služba Event Hubs dokáže zpracovávat a ukládat události, data nebo telemetrické údaje produkované distribuovaným softwarem a zařízeními. Data odeslaná do centra událostí je možné transformovat a uložit pomocí libovolného poskytovatele analýz v reálném čase nebo adaptérů pro dávkové zpracování a ukládání. Event Hubs provede dokonalý shodu prvního kroku v teplé cestě pro tok dat.

Následující obrázek ukazuje fázi rychlosti vrstev. Skládá se z centra událostí, instance Stream Analytics a úložiště dat pro teplé úložiště.

![Architektura lambda: byla zvýrazněna rychlost vrstvy.](assets/extracting-insights-from-iot/lambda-2.png)
  
Platforma Azure poskytuje mnoho možností pro zpracování událostí v centru událostí, ale doporučujeme Stream Analytics. Stream Analytics může do služba Power BI zasílat data a vizualizovat streamovaná data.

Stream Analytics může spouštět složitou analýzu ve velkém měřítku, například bubny, posouvání/skákající Windows, agregace datových proudů a spojení externích zdrojů dat. V případě složitějšího zpracování je možné výkon rozšířit pomocí kaskádování více instancí Event Hubs, Stream Analytics úloh a Azure Functions, jak je znázorněno na následujícím obrázku.

![Event Hubs k analýze Power BI.](assets/extracting-insights-from-iot/event-hubs-to-power-bi.png)
  
Teplé úložiště je možné implementovat s různými službami na platformě Azure, například Azure SQL Database. Doporučujeme [Azure Cosmos DB](/azure/cosmos-db/introduction?WT.mc_id=iotinsightssoln-docs-ercenk). Je to globálně distribuovaná databáze Microsoftu pro více modelů. Je nejvhodnější pro datové sady, které můžou využívat flexibilní rozhraní, schémat-nezávislá, automatické indexování a rozsáhlá rozhraní dotazů. Cosmos DB umožňuje Kromě automatického převzetí služeb při selhání ruční převzetí služeb při selhání, čtení a zápis. Kromě toho Cosmos DB umožňuje uživateli pro svá data nastavit hodnotu TTL (Time to Live), která automaticky vyprší stará data. Tuto funkci doporučujeme použít k řízení času, kdy záznamy zůstanou v databázi, a tím řízení velikosti databáze.

Ceny za Cosmos DB jsou založené na využitém úložišti a zřízených [jednotkách požadavků](/azure/cosmos-db/request-units) . Cosmos DB je nejvhodnější pro scénáře, které nevyžadují dotazy zahrnující agregaci přes velké sady dat, protože tyto dotazy vyžadují více jednotek požadavků než základní dotaz, jako je například poslední událost pro zařízení.  

[Microsoft Power BI](/power-bi/power-bi-overview?WT.mc_id=iotinsightssoln-docs-ercenk) je kolekce softwarových služeb, aplikací a konektorů, které společně usnadňují práci s nesouvisejícími zdroji dat, a to tak, aby byly souvislé, vizuálně atraktivní a interaktivní přehledy. Power BI vám pomůže s informacemi, které jsou v dobrém stavu. Můžete využít [streamování v reálném čase v Power BI](/power-bi/service-real-time-streaming?WT.mc_id=iotinsightssoln-docs-ercenk) k odesílání dat do služby. Tento datový proud v reálném čase může fungovat jako zdroj dat streamování v reálném čase pro různé vizuály na řídicím panelu Power BI.

## <a name="cold-path"></a>Studená cesta

Cesta pro teplu je místo, kde se při zpracování datového proudu objevují vzorce v průběhu času. Chtěli bychom ale také vypočítat využití za určitou dobu v minulosti, s různými pivoty a agregacemi, jako je například Machine, line, závod, vyráběný Part Etcetera. Chceme tyto výsledky sloučit s výsledky teplé cesty, aby bylo možné uživateli prezentovat jednotné zobrazení. Studená cesta zahrnuje dávkovou vrstvu a vrstvy obsluhy. Kombinace poskytuje dlouhodobé zobrazení systému.

Studená cesta obsahuje dlouhodobé úložiště dat pro řešení. Obsahuje také dávkovou vrstvu, která vytváří předem vypočtená agregovaná zobrazení pro zajištění rychlých odpovědí na dotazy po dlouhou dobu. Možnosti technologie dostupné pro tuto vrstvu na platformě Azure jsou poměrně odlišné.

![Architektura lambda: zvýrazní se dávková vrstva.](assets/extracting-insights-from-iot/lambda-3.png)
  
[Azure Time Series Insights](/azure/time-series-insights/?WT.mc_id=iotinsightssoln-docs-ercenk) (TSI) je služba pro analýzy, ukládání a vizualizace pro data časových řad. Poskytuje filtrování a agregaci jako SQL, což řeší nutnost uživatelsky definovaných funkcí. TSI může přijímat data z Event Hubs, IoT Hub nebo úložiště objektů BLOB v Azure. Všechna data v TSI jsou uložená v paměti a v SSD, která zajišťuje, že data jsou vždycky připravená pro interaktivní analýzy. Například typická agregace nad desítkami milionů událostí se vrátí v řádu milisekund. Poskytuje také vizualizace, jako jsou překryvy různých časových řad, porovnávání řídicích panelů, přístupná tabulková zobrazení a Heat mapy. Mezi klíčové funkce TSI patří:

- Předdefinované služby vizualizace pro řešení, která nepotřebují přímo nahlásit data. TSI má přibližnou latenci pro dotazování záznamů dat o 30-60 sekund. 
- Možnost dotazování na velké sady dat.
- Libovolný počet uživatelů může provádět neomezený počet dotazů bez dalších poplatků.

V TSI je maximální doba uchování 400 dní a maximální limit úložiště je 3 TB. Pokud vyžadujete delší dobu uchování nebo větší kapacitu, použijte studenou databázi úložiště (k výměně dat do TSI pro dotazování podle potřeby).

Pro studený objem aplikace IoT se v průběhu času určité období výrazně zvětšuje. Zde je místo, kde jsou data ukládána dlouhodobě a shrnuta v dávkových zobrazeních pro analýzu. Data pro modely ML se také ukládají sem. Pro studené úložiště doporučujeme [Azure Storage](/azure/storage/?WT.mc_id=iotinsightssoln-docs-ercenk) . Je to služba spravovaná Microsoftem, která poskytuje vysoce dostupné, zabezpečené, odolné, škálovatelné a redundantní cloudové úložiště. Azure Storage zahrnuje objekty blob Azure (objekty), Azure Data Lake Storage Gen2, soubory Azure, fronty Azure a tabulky Azure. Studeným úložištěm můžou být objekty blob, Data Lake Storage Gen2 nebo tabulky Azure. nebo jejich kombinace.

[Azure Table Storage](/azure/cosmos-db/table-storage-overview?WT.mc_id=iotinsightssoln-docs-ercenk) je služba, která ukládá strukturovaná NoSQL data v cloudu a poskytuje úložiště klíčů a atributů s návrhem bez schématu. Vzhledem k tomu, že Table Storage je bez schématu, je snadné přizpůsobit data podle potřeb vaší aplikace. Přístup k Table Storage datům je rychlý a nákladově efektivní pro mnoho typů aplikací a obvykle se snižuje náklady než tradiční SQL pro podobné objemy dat. Pro ukázky používáme jednu tabulku a jednu tabulku pro události, které jsou přijaté z datových proudů. Návrh klíče oddílu je obzvláště důležitý koncept; obě tabulky používají hodinu časového razítka události nebo ukázky. Další informace najdete v tématu [Vysvětlení datového modelu služby Table Storage](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?WT.mc_id=iotinsightssoln-docs-ercenk).

Pro ukládání obrovských objemů nestrukturovaných dat, jako jsou JSON nebo dokumenty XML obsahující nezpracovaná data přijatá aplikací IoT, [úložiště objektů BLOB](/azure/storage/blobs/storage-blobs-introduction?WT.mc_id=iotinsightssoln-docs-ercenk), [soubory Azure](/azure/storage/files/storage-files-introduction?WT.mc_id=iotinsightssoln-docs-ercenk)nebo [Azure Data Lake Storage Gen2](/azure/storage/data-lake-storage/introduction?WT.mc_id=iotinsightssoln-docs-ercenk) jsou nejlepší možností.

Služba Azure Blob Storage je k dispozici odkudkoli na světě prostřednictvím protokolu HTTP nebo HTTPS zabezpečeně. Přístup k úložišti objektů BLOB musí být autorizovaný pomocí některého [autorizačního mechanismu](/azure/storage/common/storage-auth?toc=%2fazure%2fstorage%2fblobs%2ftoc.json?WT.mc_id=iotinsightssoln-docs-ercenk) , který služba používá. Služba poskytuje více [možností](/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fblobs%2ftoc.json?WT.mc_id=iotinsightssoln-docs-ercenk)replikace: místně redundantní, zóna – redundantní, geograficky redundantní a přístup pro čtení geograficky redundantní. K dispozici jsou také tři [úrovně přístupu](/azure/storage/blobs/storage-blob-storage-tiers?WT.mc_id=iotinsightssoln-docs-ercenk) , které umožňují nejefektivnější řešení.

Jakmile jsou data v studeném úložišti, je třeba vytvořit dávková zobrazení na vrstvě obsluhující architekturu lambda. [Azure Data Factory](/azure/data-factory/introduction?WT.mc_id=iotinsightssoln-docs-ercenk) je skvělé řešení pro vytváření dávkových zobrazení ve vrstvě obsluhy. Jedná se o cloudovou službu pro integraci spravovaných dat, která umožňuje vytvářet pracovní postupy řízené daty v cloudu pro orchestraci a automatizaci přesunu dat a transformaci dat. Pomocí Azure Data Factory můžete vytvářet a plánovat [pracovní postupy řízené daty](/azure/data-factory/concepts-pipelines-activities?WT.mc_id=iotinsightssoln-docs-ercenk) (nazývané kanály), které mohou ingestovat data z různorodých úložišť dat. Může zpracovávat a transformovat data pomocí služeb, jako jsou [Azure HDInsight Hadoop](/azure/hdinsight/?WT.mc_id=iotinsightssoln-docs-ercenk), [Spark](/azure/hdinsight/?WT.mc_id=iotinsightssoln-docs-ercenk)a [Azure Databricks](/azure/azure-databricks/?WT.mc_id=iotinsightssoln-docs-ercenk). To vám umožní sestavovat modely strojového učení a spotřebovávat je s využitím klientů Analytics.

Například, jak je znázorněno na následujícím obrázku, Data Factory kanály čtou data z hlavního úložiště dat. Jeden kanál shrnuje a agreguje data k naplnění instance služby Azure Data Warehouse. Kanál Data Factory také obsahuje [Azure Databricks aktivity poznámkového bloku](/azure/data-factory/transform-data-using-databricks-notebook?WT.mc_id=iotinsightssoln-docs-ercenk) , které se používají k sestavení modelů ml.

![Architektura lambda: zvýrazní se dávková vrstva.](assets/extracting-insights-from-iot/master-data-to-ml-analytics.png)
  
Nejlepší možností pro hostování zobrazení Batch jsou [Azure SQL Database](/azure/sql-database/?WT.mc_id=iotinsightssoln-docs-ercenk) nebo [Azure SQL Data Warehouse](/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is?WT.mc_id=iotinsightssoln-docs-ercenk) . Tyto služby mohou sloužit předem vypočítané a agregované pohledy na hlavní data. 

Azure SQL Database (SQL DB) je relační databáze jako služba založená na nejnovější verzi služby Microsoft SQL Server Database Engine. SQL DB je vysoce výkonná, spolehlivá a zabezpečená databáze, kterou můžete použít k vytváření aplikací a webů řízených daty. Jako služba Azure není nutné spravovat infrastrukturu. Jak se zvyšuje objem dat, může řešení začít používat techniky k agregaci a ukládání dat pro urychlení dotazů. Předem vypočítané agregace jsou dobře známou technikou, zejména pro připojená data. Je to také užitečné pro správu nákladů.

Azure SQL Data Warehouse poskytuje mnoho dalších funkcí, které mohou být užitečné v některých scénářích. Jedná se o cloudový podnikový datový sklad, který využívá výkonné paralelní zpracování k rychlému spouštění složitých dotazů napříč petabajty dat. Pokud potřebujete zachovat petabajty dat a chcete, aby dotazy běžely rychle, doporučujeme SQL Data Warehouse.

## <a name="visualizing-the-data"></a>Vizualizace dat

V této vrstvě chceme sloučit dva datové kanály (teplé a studené cesty), aby se zobrazily soudržný pohled na data. V tomto příkladu jsme použili více metrik pro odvození využití počítače na teplé i studené cestě. Ve fázi analýzy poskytujeme vizualizace, které kombinují data z těchto cest.

![Architektura lambda: Zvýrazněná vrstva klienti Analytics.](assets/extracting-insights-from-iot/lambda-4.png)

[Microsoft Power BI](/power-bi/?WT.mc_id=iotinsightssoln-docs-ercenk) a [Azure Time Series Insights](/azure/time-series-insights/?WT.mc_id=iotinsightssoln-docs-ercenk) poskytují vizualizace dat předem. Power BI je řešení obchodní analýzy, které umožňuje vizualizovat vaše data a sdílet přehledy napříč vaší organizací nebo je vkládat do vaší aplikace nebo webu. [Power BI Desktop](https://powerbi.microsoft.com/desktop/?WT.mc_id=iotinsightssoln-docs-ercenk) je bezplatný a výkonný nástroj pro sestavy modelování a jejich podkladové zdroje dat.  Aplikace, které vkládá Power BI vizualizace, používají sestavy vytvořené nástrojem Desktop a hostované ve službě Power BI.

Time Series Insights má Průzkumník dat k vizualizaci a dotazování dat i rozhraní REST API pro dotazy. Dále zpřístupňuje knihovnu ovládacích prvků JavaScriptu, která umožňuje vkládat grafy založené na TSI do vlastních aplikací. Toto je základní heatmapu zobrazení na TSI pro příchozí data, která se blíží využití počítačů v dílně, a to tak, že jednoduše prohlížíte počet pozorovaných ukázek.

![Architektura lambda: zvýrazní se dávková vrstva.](assets/extracting-insights-from-iot/client-screen.png)

Pokud vyžadujete uživatelské rozhraní založené na prohlížeči, které agreguje data z více zdrojů, musí obě tyto služby a služby Power BI umožňovat vkládání ovládacích prvků vizualizace. Zároveň poskytují rozhraní REST API ([Power BI rozhraní REST API](/rest/api/power-bi/?WT.mc_id=iotinsightssoln-docs-ercenk), [TSI REST API](/rest/api/time-series-insights/time-series-insights-reference-queryapi?WT.mc_id=iotinsightssoln-docs-ercenk)) a sady JavaScript SDK ([Power BI JavaScript SDK](https://github.com/Microsoft/PowerBI-JavaScript?WT.mc_id=iotinsightssoln-docs-ercenk), [TSI JavaScript SDK](/azure/time-series-insights/tutorial-explore-js-client-lib?WT.mc_id=iotinsightssoln-docs-ercenk)), které umožňují rozsáhlé přizpůsobení.

## <a name="next-steps"></a>Další kroky

Pokryli jsme spoustu konceptů a chtěli bychom dát čtenářům sadu výchozích bodů pro další informace a použít techniky na jejich vlastní požadavky. Tady je několik kurzů, které se domníváme, že pro tento účel může být užitečné.

- Převod dat na datový proud
  - [Vytvoření aplikace logiky spuštěné podle plánu](/azure/logic-apps/tutorial-build-schedule-recurring-logic-app-workflow?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [Příklady kódu pro datové operace na Logic Apps](/azure/logic-apps/logic-apps-data-operations-code-samples?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [Spuštění Azure Functions v kontejneru](/azure/azure-functions/functions-create-function-linux-custom-image?WT.mc_id=iotinsightssoln-docs-ercenk) pro hostování vaší funkce Azure Functions je pokryto na více místech. Vytvoření funkce na platformě Linux s použitím vlastní image, spuštění funkcí na libovolné platformě, imagí Docker pro Modul runtime služby Azure Functions
  - [Používání různých vazeb na Azure Functions](/azure/azure-functions/functions-triggers-bindings?WT.mc_id=iotinsightssoln-docs-ercenk)

- Kritická cesta
  - Kompletní kurzy, které demonstrují použití Event Hubs, Azure Stream Analytics a Power BI. Viz [kurz: vizualizace anomálií dat](/azure/event-hubs/event-hubs-tutorial-visualize-anomalies?WT.mc_id=iotinsightssoln-docs-ercenk) v reálném čase odeslaných do Azure Event Hubs a [Vytvoření úlohy Stream Analytics k analýze dat telefonních hovorů](/azure/stream-analytics/stream-analytics-manage-job?WT.mc_id=iotinsightssoln-docs-ercenk) a vizualizaci výsledků na Power BIovém řídicím panelu.
  -[používání Azure Cosmos DB s .NET](/azure/cosmos-db/sql-api-get-started?WT.mc_id=iotinsightssoln-docs-ercenk)
- Studená cesta
  - [Transformace dat v cloudu pomocí aktivity Sparku](/azure/data-factory/tutorial-transform-data-spark-portal?WT.mc_id=iotinsightssoln-docs-ercenk) v Azure Data Factory
  - [Kurz: Vytvoření prostředí Azure Time Series Insights](/azure/time-series-insights/tutorial-create-populate-tsi-environment?WT.mc_id=iotinsightssoln-docs-ercenk)
- Klienti analýzy
  - [Power BI výuky](/power-bi/guided-learning/?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [Vytváření Time Series Insights SPA](/azure/time-series-insights/tutorial-create-tsi-sample-spa?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [Prozkoumávání Time Series Insights klientské knihovny Java Script](/azure/time-series-insights/tutorial-explore-js-client-lib?WT.mc_id=iotinsightssoln-docs-ercenk)
  - Podívejte se na ukázku a ukázku [Power BI](https://microsoft.github.io/PowerBI-JavaScript/demo/v2-demo/index.html)pro [TSI](https://insights.timeseries.azure.com/demo) .

## <a name="appendix-pillars-of-software-quality-posq"></a>Příloha: pilíře kvality softwaru (PoSQ)

Úspěšná cloudová aplikace je postavená na těchto [pilířích kvality softwaru](/azure/architecture/guide/pillars?WT.mc_id=iotinsightssoln-docs-ercenk): škálovatelnost, dostupnost, odolnost, Správa a zabezpečení. V této části stručně pokryjeme tyto pilíře pro každou komponentu podle potřeby. Nepokrýváme dostupnost, odolnost, správu a DevOps, protože jsou většinou řešeny na úrovni implementace a chceme zmínit platformu Azure, která poskytuje rozsáhlé prostředky pro dosahování rozhraní API, nástrojů, diagnostiky a protokolování. Kromě uvedených pilířů také uvádíme cenovou efektivitu.

Pojďme se rychle podívat na tyto pilíře:

- **Škálovatelnost** je schopnost systému zvládnout zvýšené zatížení. Aplikaci je možné škálovat dvěma hlavními způsoby. Vertikální škálování (vertikální škálování) znamená zvýšení kapacity prostředku, například pomocí větší velikosti virtuálního počítače. Horizontální škálování (horizontální navýšení kapacity) přidává nové instance prostředku, například virtuální počítače nebo repliky databáze. Pilíř škálovatelnosti také zahrnuje výkon a schopnost zpracovávat zatížení.
- **Dostupnost** je poměr doby, po kterou je systém funkční a funguje. Obvykle se měří jako procento doby provozu. Dostupnost mohou snížit chyby aplikace, problémy s infrastrukturou nebo zátěž systému. Smlouvy o úrovni služeb pro Microsoft Azure Services se zveřejňují a k dispozici na [smlouvách o úrovni služeb](https://azure.microsoft.com/support/legal/sla/?WT.mc_id=iotinsightssoln-docs-ercenk). Dostupnost je jediná smysluplná metrika na úrovni systému. Samostatné komponenty přispívají k celkové dostupnosti systému.
- **Odolnost** proti chybám je schopnost systému obnovit selhání a nadále fungovat. Cílem odolnosti proti chybám je obnovení plně funkčního stavu aplikace co nejdříve po selhání. Odolnost proti chybám úzce souvisí s dostupností.
- **Správa a DevOps**. Do tohoto pilíře patří provozní procesy, které udržují aplikaci spuštěnou v produkčním prostředí. Nasazení musí být spolehlivá a předvídatelná. Měla by být automatizovaná, aby se snížilo riziko lidské chyby. Mělo by se jednat o rychlý a rutinní proces, aby nedošlo ke zpomalení vydání nových funkcí nebo oprav chyb. Stejně důležité je, abyste v případě problémů s aktualizací mohli rychle vrátit změny nebo se posunout vpřed.
- **Zabezpečení** by mělo být hlavním detailním bodem v celém životním cyklu řešení, od návrhu a implementace po nasazení a provoz. Správa identit, ochrana vaší infrastruktury, zabezpečení aplikací, autorizace, svrchovanost a šifrování dat, auditování jsou všechny široké oblasti, které je potřeba řešit.

## <a name="posq-converting-the-data-to-a-stream"></a>PoSQ: převod dat na datový proud

**Škálovatelnost**: můžeme se dořešit ke škálovatelnosti ze dvou perspektiv. Nejprve z perspektivy komponenty, od perspektivy systému, který poskytuje zdrojová data.

Každá služba Azure poskytuje možnosti pro vertikální i horizontální škálování. Důrazně doporučujeme zvážit požadavky na škálovatelnost při návrhu řešení.

Stejně jako u systémů, které poskytují zdrojová data, musíme pečlivě vycházet z neúspěchu systému a v podstatě způsobovat útok DoS (Denial of Service) v systému, protože ho často dotazuje. Pokud se systém dotazuje do systému, měli byste mít na paměti, že úprava četnosti cyklického dotazování má dva účinky, členitost dat (častěji se dotazuje na v reálném čase) a zatížení vytvořené ve vzdáleném systému.

**Zabezpečení**: Pokud k vzdálenému systému přistupovali symetrický nebo asymetrický klíč, doporučujeme, abyste si tajných kódů zanechali [Azure Key Vault](/azure/key-vault/?WT.mc_id=iotinsightssoln-docs-ercenk).

## <a name="posq-warm-path"></a>PoSQ: cesta k teplu

**Škálovatelnost**: Pokud je Azure Event Hubs v subsystému pro přijímání, je hlavním mechanismem škálovatelnosti [jednotky propustnosti](/azure/event-hubs/event-hubs-features#throughput-units?WT.mc_id=iotinsightssoln-docs-ercenk). Event Hubs poskytují možnost nastavení jednotek propustnosti staticky nebo pomocí [funkce automatického rozplochení](/azure/event-hubs/event-hubs-auto-inflate?WT.mc_id=iotinsightssoln-docs-ercenk).

[Jednotky streamování](/azure/stream-analytics/stream-analytics-streaming-unit-consumption?WT.mc_id=iotinsightssoln-docs-ercenk) (SUs) v Stream Analytics představují výpočetní prostředky, které jsou přiděleny ke spuštění úlohy. Čím vyšší je počet SU, tím více prostředků CPU a paměti se úloze přidělí. Tato kapacita vám umožní soustředit se na logiku dotazů a abstrakce nutnosti spravovat hardware a včas spouštět úlohy Stream Analytics. Kromě služby SUs je velmi důležité, aby je bylo možné efektivně využívat prostřednictvím [virtuálního dotazů](/azure/stream-analytics/stream-analytics-scale-jobs?WT.mc_id=iotinsightssoln-docs-ercenk) .

Implementace Azure Cosmos DB musí být zřízené se správnými parametry propustnosti a řádným návrhem dělení. Zajištění propustnosti je k dispozici na kontejneru nebo na úrovni základu dat a vyjádřeno v [jednotkách žádosti](/azure/cosmos-db/request-units?WT.mc_id=iotinsightssoln-docs-ercenk) (ru). Cosmos DB poskytuje nástroj k odhadování ru. Kromě zřízení propustnosti je [efektivní rozdělit do oddílů databáze](/azure/cosmos-db/partition-data?WT.mc_id=iotinsightssoln-docs-ercenk) klíč.

**Zabezpečení**: přístup k Azure Event Hubs klienty je kombinací kombinace tokenů sdíleného přístupového podpisu (SAS) a vydavatelů událostí pro ověřování klientů. Zabezpečení pro back-endové aplikace se řídí stejnými koncepty jako Service Bus témata. Podrobný popis modelu zabezpečení Event Hubs je v [přehledu Event Hubsho ověřování a modelu zabezpečení](/azure/event-hubs/event-hubs-authentication-and-security-model-overview?WT.mc_id=iotinsightssoln-docs-ercenk).

Zabezpečení Cosmos DB databází zajišťuje řízený přístup k datům a šifrování v klidovém umístění. Další informace najdete v tématu [Azure Cosmos DB zabezpečení databáze](/azure/cosmos-db/database-security?WT.mc_id=iotinsightssoln-docs-ercenk).

**Nákladová efektivita**: ceny Event Hubs jsou funkcí SKU (Standard nebo Premium) a miliony přijatých událostí a jednotky propustnosti. Optimální kombinaci je možné dosáhnout tak, že si prohlížíte rychlost přijímání dat, která je určena příchozími zprávami.

Při použití Cosmos DB doporučujeme, abyste procházeli optimálním využitím obchodu prostřednictvím použití RU. Cosmos DB má také funkci pro řízení uchovávání dat, jak jsme uvedli dříve, doporučujeme použít tuto funkci k řízení času, kdy záznamy zůstanou v databázi, a tím řízení velikosti databáze.

## <a name="posq-cold-path"></a>PoSQ: studená cesta

**Škálovatelnost**: Azure Time Series Insights (TSI) se škáluje pomocí metriky s názvem "Capacity" (kapacita), což je násobitel, který se používá pro míru příchozího přenosu dat, kapacitu úložiště a náklady spojené s SKU. 

Azure Time Series Insights má několik SKU, které mají přímý vliv na vertikální měřítko. Podrobnosti o škálování najdete v dokumentu [plánování Azure Time Series Insights prostředí](/azure/time-series-insights/time-series-insights-environment-planning?WT.mc_id=iotinsightssoln-docs-ercenk) . Podobně jako u mnoha dalších služeb Azure se také může jednat o omezení, aby nedocházelo k potížím s "přenositelností souseda". Hlučná sousední síť je aplikace ve sdíleném prostředí/Azure/SQL-Database/SQL-Database-Service-Tiers-Vcore? WT. mc_id = iotinsightssoln-docs-ercenk, které nepřivlastnily všechny prostředky a starves ostatním uživatelům. Pro správu omezování si prosím přečtěte [dokumentaci k TSI](/azure/time-series-insights/time-series-insights-environment-mitigate-latency?WT.mc_id=iotinsightssoln-docs-ercenk) . 

Cíle škálovatelnosti účtů úložiště jsou popsány v [Azure Storage škálovatelnost a výkonnostní cíle](/azure/storage/common/storage-scalability-targets?WT.mc_id=iotinsightssoln-docs-ercenk). Běžnou technikou pro ukládání dat, která přesahují kapacitu jednoho účtu úložiště, je vytváření oddílů mezi několika účty úložiště.

Azure SQL Database má mnoho možností pro správu škálovatelnosti, jak vertikálně, tak horizontálně, v závislosti na modelu nákupu ([založeném na DTU](/azure/sql-database/sql-database-service-tiers-dtu?WT.mc_id=iotinsightssoln-docs-ercenk) a na Vcore). Doporučujeme další výzkum a najít nejlepší možnost pro budoucí řešení pomocí [dokumentace SQL Database](/azure/sql-database/sql-database-scale-resources?WT.mc_id=iotinsightssoln-docs-ercenk) v tématu.

**Zabezpečení**: prostředí TSI poskytují [zásady přístupu](/azure/time-series-insights/time-series-insights-data-access?WT.mc_id=iotinsightssoln-docs-ercenk) pro přístup ke správě a přístup k datům nezávisle na sobě. Neexistuje přímý způsob, jak přidat data do prostředí TSI kromě definovaných zdrojů dat. Zásady přístupu pro správu udělují oprávnění související s konfigurací prostředí. Zásady přístupu k datům udělují oprávnění k vydávání dotazů na data, zpracování referenčních dat v rámci prostředí a sdílení uložených dotazů a perspektiv přidruženým k danému prostředí.

Služba Azure Data Factory poskytuje několik metod pro zabezpečení přihlašovacích údajů úložiště dat, a to buď ve spravovaném úložišti, nebo v Azure Key Vault. Šifrování dat v přenosu závisí na přenosu úložiště dat (např. HTTPS nebo TLS). Šifrování dat na základě REST závisí také na úložištích dat. Další podrobnosti najdete [v tématu požadavky na zabezpečení při přesunu dat v Azure Data Factory](/azure/data-factory/data-movement-security-considerations?WT.mc_id=iotinsightssoln-docs-ercenk) .

SQL Database poskytuje rozsáhlou sadu funkcí zabezpečení pro přístup k datům, monitorování a auditování a také šifrování dat v klidovém formátu. Podrobnosti najdete v tématu [Security Center pro SQL Server databázový stroj a Azure SQL Database](/sql/relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database?WT.mc_id=iotinsightssoln-docs-ercenk).

**Cenová efektivita**: na srdce každého analytického řešení je úložiště. Analytické moduly potřebují pro zpracování objemů dat v rozumných časech rychlost, efektivitu, zabezpečení a propustnost. Sestavování mechanismů pro zajištění nejlepšího využití základní platformy, agregací a sumarizace dat a efektivní používání úložišť Polyglot jsou prostředky pro efektivní správu nákladů. Jelikož je Azure cloudová platforma, existují metody pro programové vyřazení z provozu, restavování a změnu velikosti prostředků z provozu. Například [operace vytvořit nebo aktualizovat](/rest/api/sql/databases/createorupdate?WT.mc_id=iotinsightssoln-docs-ercenk) poskytuje způsob, jak změnit velikost databáze Azure SQL Database.