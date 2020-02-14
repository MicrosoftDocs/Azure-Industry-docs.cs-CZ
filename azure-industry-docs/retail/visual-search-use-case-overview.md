---
title: Úvod do vizuálního vyhledávání v maloobchodě s CosmosDB
author: scseely
ms.author: scseely
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Tento článek vysvětluje fáze migrace infrastruktury elektronického obchodování z místního prostředí do Azure.
ms.openlocfilehash: b43ea305e11ac32da58e4d0521d79f90d5c23d85
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77053143"
---
# <a name="visual-search-overview"></a>Přehled Vizuální vyhledávání

## <a name="executive-summary"></a>Shrnutí

Umělá logika nabízí potenciál k transformaci maloobchodních prodejů, jak je dnes ví. Je vhodné se domnívat, že maloobchodní prodejci budou vyvíjet architekturu prostředí pro zákazníky podporovanou AI. Je potřeba, aby platforma rozšířená s AI poskytovala při individuálním přizpůsobení technologie Hyper z tržeb. Digitální obchodování nadále zvýší očekávání zákazníků, preference a chování. Požadavky, jako je zapojení v reálném čase, relevantní doporučení a technologie Hyper-v, představují rychlost a pohodlí při kliknutí na tlačítko. Umožnění inteligentních funkcí v aplikacích prostřednictvím přirozeného rozpoznávání řeči, vize atd. umožňuje vylepšení v maloobchodě, která zvyšují hodnotu a zároveň přerušuje způsob, jakým zákazníci Nakupujte.

Tento dokument se zaměřuje na koncept AI **vizuálního vyhledávání** a nabízí několik klíčových důležitých informací o jeho implementaci. Poskytuje příklad pracovního postupu a mapuje své fáze na relevantní technologie Azure. Koncept je založený na zákazníkovi schopným využít image pořízenou mobilním zařízením nebo na internetu, aby provedla hledání relevantních a/nebo podobných položek v závislosti na úmyslu prostředí. Vizuální vyhledávání proto zlepšuje rychlost z textu s využitím textu na obrázek s více body meta-data, aby bylo možné rychle naplochit dostupné položky.

## <a name="visual-search-engines"></a>Moduly Vizuální vyhledávání

Vizuální vyhledávací moduly načítají informace pomocí obrázků jako vstupů a často, ale ne výhradně – jako výstup.

Moduly se v maloobchodním průmyslu stanou více a běžnější a z velmi dobrých důvodů:

- Přibližně 75% uživatelů z Internetu vyhledává obrázky nebo videa produktu před zahájením nákupu podle sestavy [eMarketer](https://www.emarketer.com/Report/Visual-Commerce-2017-How-Image-Recognition-Augmentation-Changing-Retail/2002059) publikované v 2017.
- 74% výsledků hledání textu je neefektivní, podle [Slyce](https://slyce.it/wp-content/uploads/2015/11/Visual_Search_Technology_and_Market.pdf) (společnost pro vizuální vyhledávání) 2015.

Proto by trh pro rozpoznávání imagí měl být více než $25 000 000 000 2019 na základě výzkumu na [trzích &amp; trhů](https://www.marketsandmarkets.com/PressReleases/image-recognition.asp).

Technologie už podrží hlavní značky elektronického obchodování, které také významně přispěly k vývoji. Nejvýraznějších prvních přijímají je pravděpodobně:

- eBay s jejich Vyhledávání obrázků a "najít na eBay" ve své aplikaci (aktuálně se jedná jenom o mobilní prostředí).
- Pinterestu se pomocí nástroje pro vizuální zjišťování objektivu.
- Microsoft s Vizuální vyhledávání Bingu.

## <a name="adopt-and-adapt"></a>Přijmout a upravit

Naštěstí nepotřebujete k zisku z vizuálního vyhledávání obrovské množství výpočetní síly. Všechny firmy s katalogem imagí můžou využít odbornosti Microsoftu, které jsou integrované do svých služeb Azure.

[Vizuální vyhledávání Bingu](https://azure.microsoft.com/services/cognitive-services/bing-visual-search/?WT.mc_id=vsearchgio-article-gmarchet) Rozhraní API poskytuje způsob, jak extrahovat kontextové informace z imagí, identifikovat – např. – domácí zařízení, způsob, několik druhů produktů atd.

Budou taky vracet vizuálně podobné obrázky z vlastního katalogu a produkty s relativními nákupními zdroji a související hledání. I když vaše společnost není jedním z těchto zdrojů, bude mít omezení za zajímavé účely.

Bing bude také poskytovat:

- Značky, které umožňují prozkoumat objekty nebo koncepty, které se nacházejí v imagi.
- Ohraničující pole pro oblasti zájmu v obrázku (např. oděvy, položky nábytku).

Tyto informace můžete využít k tomu, abyste snížili prostor pro hledání (a čas) na katalog produktů společnosti významně a omezili jsme ho na objekty, jako jsou ty v oblasti a kategorii zájmu.

## <a name="implement-your-own"></a>Implementace vlastního

Při implementaci vizuálního vyhledávání je potřeba zvážit několik klíčových komponent:

- Ingestování a filtrování imagí
- Metody úložiště a načítání
- Featurization, Encoding nebo "hashing"
- Míry podobnosti nebo vzdálenosti a hodnocení

 ![](./assets/visual-search-use-case-overview/visual-search-pipeline.png)

*Obrázek 1: příklad kanálu Vizuální vyhledávání*

### <a name="sourcing-the-pictures"></a>Navýšení obrázků

Pokud nevlastníte katalog obrázků, možná budete muset vyškolit algoritmy v otevřených dostupných datových sadách, jako je například [mnist ručně zapsaných](https://www.kaggle.com/zalando-research/fashionmnist) [, hlubokou](http://mmlab.ie.cuhk.edu.hk/projects/DeepFashion.html) a podobným způsobem. Obsahují několik kategorií produktů a často se používají ke kategorizaci a algoritmům pro vyhledávání v testování obrazu.

 ![](./assets/visual-search-use-case-overview/deep-fashion-dataset.png)

*Obrázek 2: příklad z datové sady s hloubkovým způsobem*

### <a name="filtering-the-images"></a>Filtrování imagí

Většina datových sad srovnávacích testů – například ty, které byly zmíněny před – již byly zpracovány předem.

Pokud vytváříte vlastní, budete mít přinejmenším v tom, že mají všechny obrázky stejnou velikost, hlavně vynásobené vstupem, pro který je váš model vyškolený.

V mnoha případech je nejlepší také normalizovat světlost imagí. V závislosti na úrovni podrobností hledání může být barva také redundantní informace, takže snížení na černou a bílou vám pomůže s časy zpracování.

A ne alespoň, datová sada obrázku by měla být vyrovnávána napříč různými třídami, které představuje.

### <a name="image-database"></a>Databáze obrázků

Datová vrstva je zvlášť jemná komponenta vaší architektury. Bude obsahovat:

- Image
- Všechna metadata o obrázcích (velikost, značky, SKU produktů, popis)
- Data generovaná modelem strojového učení (například číselná vektor 4096 prvku na obrázek)

Při načítání imagí z různých zdrojů nebo použití několika modelů strojového učení za účelem optimálního výkonu se struktura dat změní. Je proto důležité zvolit technologii nebo kombinaci, která může pracovat s částečně strukturovanými daty a bez pevného schématu.

Možná budete chtít vyžadovat minimální počet užitečných datových bodů (například identifikátor obrázku nebo klíč, SKU produktu, popis, pole značka).

[Azure CosmosDB](https://azure.microsoft.com/services/cosmos-db/?WT.mc_id=vsearchgio-article-gmarchet) nabízí požadovanou flexibilitu a řadu přístupových mechanismů pro aplikace, které jsou na ní postavené (což vám pomůže při hledání v katalogu). Jedna z nich však musí být opatrní, aby vystavila nejlepší poměr ceny a výkonu. CosmosDB umožňuje uložit přílohy dokumentů, ale celkový limit pro každý účet může být nákladný. Běžný postup je uložit skutečné soubory imagí do objektů BLOB a vložit na ně odkazy v databázi. V případě CosmosDB to znamená vytvoření dokumentu, který obsahuje vlastnosti katalogu přidružené k této imagi (SKU, značky atd.), a přílohu, která obsahuje adresu URL souboru obrázku (např. v Azure Blob Storage, OneDrive atd.).

 ![](./assets/visual-search-use-case-overview/cosmosdb-data-model.png)

*Obrázek 3: CosmosDB hierarchické modely prostředků*

Pokud máte v úmyslu využít globální distribuci Cosmos DB, pamatujte na to, že se budou replikovat dokumenty a přílohy, ale ne propojené soubory. Možná budete chtít zvážit síť pro distribuci obsahu.

Další použitelné technologie jsou kombinací Azure SQL Database (Pokud je pevné schéma přijatelné) a objektů blob, nebo tabulek Azure a objektů BLOB pro levné a rychlé ukládání a načítání.

### <a name="feature-extraction-amp-encoding"></a>&amp; kódování pro extrakci funkcí

Proces kódování extrahuje funkce nejdůležitějšími z obrázků v databázi a mapuje je na zhuštěné vektory "funkce" (vektor s mnoha nulami), které mohou mít tisíce komponent. Tento vektor je numerická reprezentace funkcí (např. hrany, tvary), které charakterizují obrázek – podobají na kód.

Techniky extrakce funkcí obvykle používají _mechanismy přenosu_. K tomu dojde, když vyberete předem proučenou neuronové síť, spustíte každý z nich a uložíte vektor funkce, který jste v databázi imagí vygenerovali zpátky. Tímto způsobem můžete "přenést" učení od toho, kdo tuto síť vyškole. Společnost Microsoft vyvinula a publikovala několik předem vyškolených sítí, které byly široce používány pro úlohy rozpoznávání imagí, jako je například [ResNet50](https://www.kaggle.com/keras/resnet50).

V závislosti na síti neuronové bude mít vektor funkcí víc nebo méně dlouhých a řídkých, takže požadavky na paměť a úložiště se budou lišit.

Můžete také zjistit, že různé sítě se vztahují na různé kategorie, takže implementace vizuálního vyhledávání může ve skutečnosti generovat vektory různých velikostí.

Předem připravené sítě neuronové se poměrně snadno používají, ale nemusí být efektivní pro vlastní model vyškolený v katalogu imagí. Tyto předem připravené sítě jsou obvykle navržené pro klasifikaci srovnávacích datových sad místo hledání ve vaší konkrétní kolekci imagí.

Můžete je chtít upravit a znovu vytvořit, aby vybíraly předpověď kategorií i hustý vektor (tj. menší, nikoli zhuštěný), což bude velmi užitečné omezit prostor pro hledání a snížit požadavky na paměť a úložiště. Binární vektory lze použít a často se označují jako " [sémantická hodnota hash](https://www.cs.utoronto.ca/~rsalakhu/papers/semantic_final.pdf)" – termín odvozený od technik kódování a načítání dokumentů. Binární reprezentace usnadňuje další výpočty.

 ![](./assets/visual-search-use-case-overview/resnet-modifications.png)

*Obrázek 4: úpravy ResNet pro Vizuální vyhledávání – F. Yang et al., 2017*

Bez ohledu na to, jestli si zvolíte předem připravené modely nebo vyvíjíte vlastní, budete se muset rozhodnout, kde spustit featurization a/nebo školení samotného modelu.

Azure nabízí několik možností: virtuální počítače, Azure Batch, [Batch AI](https://azure.microsoft.com/services/batch-ai/?WT.mc_id=vsearchgio-article-gmarchet), clustery datacihly. Ve všech případech ale nejlepší cena/výkon je dána pomocí GPU.

Společnost Microsoft také nedávno oznámila dostupnost FPGA pro rychlé výpočty za zlomek nákladů GPU (projekt [Brainwave](https://www.microsoft.com/research/blog/microsoft-unveils-project-brainwave/?WT.mc_id=vsearchgio-article-gmarchet)). V době psaní však je tato nabídka omezená na určité síťové architektury, takže budete muset svůj výkon vyhodnotit pečlivě.

### <a name="similarity-measure-or-distance"></a>Míra podobnosti nebo vzdálenost

Pokud jsou obrázky reprezentovány ve vektorovém prostoru funkce, hledání podobnosti se projeví jako otázka definování míry vzdálenosti mezi body v takovém prostoru. Po definování vzdálenosti můžete vypočítat clustery podobných obrázků nebo definovat matice podobnosti. V závislosti na vybrané metrikě vzdálenosti se výsledky můžou lišit. Nejběžnější míry Euclidean vzdálenosti nad vektory reálného čísla je například snadno pochopit: zachycuje velikost vzdálenosti. Je ale spíše neefektivní z podmínek výpočtu.

[Kosinusová](https://en.wikipedia.org/wiki/Cosine_similarity) vzdálenost se často používá k zachycení orientace vektoru namísto jeho velikosti.

Alternativy, jako je [Hammingá](https://en.wikipedia.org/wiki/Hamming_distance) vzdálenost přes binární reprezentace, představují určitou přesnost pro efektivitu a rychlost.

Kombinace velikosti vektoru a míry vzdálenosti určuje, jak se bude vyhodnocovat výpočetní výkon a velký objem paměti.

### <a name="search-amp-ranking"></a>Hledat &amp; hodnocení

Po definování podobnosti musíme navrhnout efektivní metodu pro načtení nejbližších N položek předaných jako vstup a pak vracet seznam identifikátorů. Tato možnost se označuje také jako "hodnocení obrázku". V případě velkých datových sad je čas na výpočet každé vzdálenosti zakázán, takže použijeme přibližné algoritmy s nejbližším sousedem. Pro ty existují některé knihovny open source, takže nebudete muset vytvářet kód od začátku.

Požadavky na paměť a výpočet budou nakonec určovat volbu technologie nasazení pro trained model a také vysokou dostupnost. Prostor pro hledání se obvykle dělí na oddíly a několik instancí algoritmu hodnocení se spustí paralelně. Jedna z možností, která umožňuje škálovatelnost a dostupnost, je clustery [Azure Kubernetes](https://azure.microsoft.com/services/container-service/kubernetes/?WT.mc_id=vsearchgio-article-gmarchet) . V takovém případě je vhodné nasadit model hodnocení mezi několik kontejnerů (zpracováním oddílu každého hledaného prostoru) a několika uzly (pro vysokou dostupnost).

## <a name="next-steps"></a>Další kroky

Implementace vizuálního vyhledávání nemusí být složitá. Můžete využít Bing nebo sestavit vlastní se službami Azure a těžit z nástrojů pro vyhledávání a používání společnosti Microsoft.

### <a name="trial"></a>Zkušební verze

- Vyzkoušejte si [konzolu pro testování vizuální vyhledávání API](https://dev.cognitive.microsoft.com/docs/services/878c38e705b84442845e22c7bff8c9ac)

### <a name="develop"></a>Vyvinout

- Pokud chcete začít vytvářet vlastní službu, přečtěte si téma [rozhraní API pro vizuální vyhledávání Bingu Overview](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview/?WT.mc_id=vsearchgio-article-gmarchet) .
- Chcete-li vytvořit první požadavek, přečtěte si téma [C#](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/csharp) rychlý start: | [Java](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/java) | [Node. js](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/nodejs) | [Python](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/python)
- Seznamte se s [referenčními informacemi k rozhraní API pro vizuální vyhledávání](https://aka.ms/bingvisualsearchreferencedoc).

### <a name="background"></a>Podrobnosti

- [Segmentace obrázků s hloubkovým učením](https://www.microsoft.com/developerblog/2018/04/18/deep-learning-image-segmentation-for-ecommerce-catalogue-visual-search/?WT.mc_id=vsearchgio-article-gmarchet): Microsoft paper popisuje proces oddělení imagí z pozadí.
- [Vizuální vyhledávání na adrese eBay](https://arxiv.org/abs/1706.03154): Cornell University Research
- [Vizuální zjišťování na Pinterestu](https://arxiv.org/abs/1702.04680) Cornell University Research
- [Sémantické hashing](https://www.cs.utoronto.ca/~rsalakhu/papers/semantic_final.pdf) Univerzita Toronto Research

_Tento článek vytvořila služba Giovanni Marchetti a Mariya Zorotovich._