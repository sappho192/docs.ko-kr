---
title: "XML과 관계형 데이터 및 ADO.NET의 통합"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-standard
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: f6ebb1a1-f2ca-49b9-92c9-0150940cf6e6
caps.latest.revision: "4"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.openlocfilehash: 5d03a0ca7518b06c08d98967d7c5ae864f1c04ac
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2017
---
# <a name="xml-integration-with-relational-data-and-adonet"></a>XML과 관계형 데이터 및 ADO.NET의 통합
**XmlDataDocument** 클래스의 파생된 클래스는는 **XmlDocument**, XML 데이터를 포함 합니다. 이점은 **XmlDataDocument** 관계형 및 계층적 데이터 간의 브리지를 제공 한다는 것입니다. **XmlDocument** 에 바인딩될 수는 **DataSet** 두 클래스에는 두 클래스에 포함 된 데이터에 대 한 변경 내용을 동기화 할 수 있습니다. **XmlDocument** 에 바인딩되는 **데이터 집합** 사용 하면 관계형 데이터와 통합 하 여 XML 및 XML로 또는 관계형 형식으로 나타낼 데이터 필요가 없습니다. 데이터를 두 가지 형식으로 나타낼 수 있으며 한 가지 형식으로만 제약되지 않습니다.  
  
 두 가지 뷰로 데이터를 사용할 수 있기 때문에 다음과 같은 이점을 얻을 수 있습니다.  
  
-   XML 문서의 구조화된 부분을 데이터 집합에 매핑할 수 있으며 효율적으로 저장하고 인덱싱하거나 검색할 수 있습니다.  
  
-   커서 모델을 통해 관계형으로 저장되는 XML 데이터에 대한 변형, 유효성 검사 및 탐색 작업을 효율적으로 수행할 수 있습니다. 시간에 완료할 수 보다 효율적으로에 저장 된 XML에 경우 보다 관계형 구조에 대해는 **XmlDocument** 모델입니다.  
  
-   **DataSet** XML의 일부를 저장할 수 있습니다. 즉, 사용할 수 있습니다 **XPath** 또는 **XslTransform** 저장소로 **DataSet** 요소 및 관심 있는 특성입니다. 여기에서 변경할 수의 데이터를 더 작은, 필터링 된 하위 집합을 더 큰 데이터에 전파 변경 내용으로는 **XmlDataDocument**합니다.  
  
 에 로드 된 데이터에 대해 변환을 실행할 수도 있습니다는 **DataSet** SQL Server에서 합니다. 또 다른 옵션은.NET Framework 클래스 스타일 관리 WinForm 바인딩할 및 WebForm 컨트롤을 한 **DataSet** XML 입력 스트림으로부터 채워진 합니다.  
  
 지원할 뿐 아니라 **XslTransform**, **XmlDataDocument** 관계형 데이터를 노출 **XPath** 쿼리 및 유효성 검사 합니다.  기본적으로 관계형 데이터에 대해 모든 XML 서비스를 사용할 수 있으며 XML 신뢰도를 떨어뜨리지 않으면서 구조적으로 투영된 XML에 대해 컨트롤 바인딩, 코드 생성 등과 같은 관계형 기능을 사용할 수 있습니다.  
  
 때문에 **XmlDataDocument** 에서 상속 되는 **XmlDocument**, W3C DOM의 구현을 제공 팩트는는 **XmlDataDocument** 내부에 해당 데이터의 하위 집합을 저장 하 고 연관 된는 **데이터 집합** 제한 하거나 변경으로 사용 하지 않습니다는 **XmlDocument** 어떤 방식으로든에서 합니다. 사용 하도록 작성 된 코드는 **XmlDocument** 에 대해서도 동일 works는 **XmlDataDocument**합니다. **DataSet** 테이블, 열, 관계 및 제약 조건을 정의 하 여 동일한 데이터의 관계형 뷰를 제공 하 고는 독립 실행형, 메모리 내 사용자 데이터 저장소입니다.  
  
 다음 그림과 다르게 연결 된 XML 데이터에는 **DataSet** 및 **XmlDataDocument**합니다.  
  
 ![XML 데이터 집합](../../../../docs/standard/data/xml/media/xmlintegrationwithrelationaldataandadodotnet.gif "xmlIntegrationWithRelationalDataAndADOdotNet")  
  
 그림에 나와 있는 XML 데이터에 직접 로드할 수 있는지는 **데이터 집합**을 관계형 방식으로 XML로 직접 조작할 수 있습니다. 또는 DOM의 파생 클래스로 XML을 로드할 수 있는지는 **XmlDataDocument**, 로드 이후에 동기화 할는 **DataSet**합니다. 때문에 **데이터 집합** 및 **XmlDataDocument** 단일 집합에 대해 동기화 된 데이터를 한 저장소에서 데이터 변경 내용이 다른 저장소에 반영 됩니다.  
  
 **XmlDataDocument** 에서 모든 편집 및 탐색 기능을 상속 된 **XmlDocument**합니다. 하는 사용 하는 경우는 **XmlDataDocument** 와 동기화 되어 상속 된 기능는 **데이터 집합**, XML에 직접 로드 보다 더 적절 한 옵션은는 **데이터집합**. 다음 표에 나와 로드를 사용 하는 방법을 선택할 때 고려해 야 할 항목의 **DataSet**합니다.  
  
|XML을 DataSet으로 직접 로드해야 할 때|XmlDataDocument를 DataSet과 동기화해야 할 때|  
|----------------------------------------------|-----------------------------------------------------------|  
|데이터 쿼리는 **DataSet** XPath 보다 SQL을 사용 하 여 보다 쉽습니다.|데이터에 대해 XPath 쿼리가 필요는 **DataSet**합니다.|  
|소스 XML에서 요소 순서를 유지하지 않아도 됩니다.|소스 XML에서 요소 순서를 유지해야 합니다.|  
|소스 XML에서 요소 간의 공백과 서식을 유지할 필요가 없습니다.|소스 XML에서 공백과 서식을 유지해야 합니다.|  
  
 로드 하 고 직접 내부 / 외부로 XML을 작성 하는 경우는 **데이터 집합** 사용자 요구 사항을 해결 참조 [XML 로부터 DataSet 로드](../../../../docs/framework/data/adonet/dataset-datatable-dataview/loading-a-dataset-from-xml.md) 및 [DataSet을 XML 데이터로 작성](../../../../docs/framework/data/adonet/dataset-datatable-dataview/writing-dataset-contents-as-xml-data.md)합니다.  
  
 로드 하는 경우는 **DataSet** 에서 **XmlDataDocument** 사용자 요구 사항을 해결 참조 [dataset XML 문서와 동기화](../../../../docs/framework/data/adonet/dataset-datatable-dataview/dataset-and-xmldatadocument-synchronization.md)합니다.  
  
## <a name="see-also"></a>참고 항목  
 [데이터 집합에서 XML 사용](../../../../docs/framework/data/adonet/dataset-datatable-dataview/using-xml-in-a-dataset.md)
