---
title: "XPath 데이터 모델을 사용하여 XML 데이터 처리"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-standard
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 536c6fce-1453-4654-9c72-bca54d47e081
caps.latest.revision: "2"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.openlocfilehash: 3d2c8db03d494be13a93df06a359e4e4294c22a2
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2017
---
# <a name="process-xml-data-using-the-xpath-data-model"></a>XPath 데이터 모델을 사용하여 XML 데이터 처리
<xref:System.Xml?displayProperty=nameWithType> 네임스페이스는 <xref:System.Xml.XmlDocument> 또는 <xref:System.Xml.XPath.XPathDocument> 클래스를 사용하여 XML 문서, 조각, 노드 또는 메모리 내 노드 집합의 프로그래밍 방식 표현을 제공합니다.  
  
 <xref:System.Xml.XPath.XPathDocument> 클래스는 XPath 데이터 모델을 사용하여 빠른 속도의 읽기 전용 메모리 내 XML 문서 표현을 제공합니다. <xref:System.Xml.XmlDocument> 클래스는 W3C DOM(문서 개체 모델) Level 1 Core 및 DOM Level 2 Core를 구현하는 XML 문서의 편집 가능한 메모리 내 표현을 제공합니다. 두 클래스는 <xref:System.Xml.XPath.IXPathNavigable> 인터페이스를 구현하며 기본 XML 데이터를 선택, 평가 및 탐색하고 경우에 따라 편집하는 데 사용되는 <xref:System.Xml.XPath.XPathNavigator> 개체를 반환합니다.  
  
 다음 단원에서는 <xref:System.Xml.XPath.XPathNavigator> 클래스를 반환하는 클래스를 기반으로 이 클래스의 기능에 대해 설명합니다.  
  
## <a name="in-this-section"></a>단원 내용  
 [XPathDocument 및 XmlDocument를 사용 하 여 XML 데이터 읽기](../../../../docs/standard/data/xml/reading-xml-data-using-xpathdocument-and-xmldocument.md)  
 읽기 전용 <xref:System.Xml.XPath.XPathDocument> 클래스 개체를 만들어 XML 문서를 읽는 방법 및 편집 가능한 <xref:System.Xml.XmlDocument> 클래스 개체를 만들어 XML 문서를 읽고 편집하는 방법을 설명합니다. 또한 이 항목에서는 각 클래스에서 <xref:System.Xml.XPath.XPathNavigator> 개체를 반환하여 XML 문서를 탐색하고 편집하는 방법을 설명합니다.  
  
 [XPathNavigator를 사용 하 여 일치 하는 XML 데이터 선택, 평가 및](../../../../docs/standard/data/xml/selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)  
 XPath 쿼리를 사용하여 <xref:System.Xml.XPath.XPathNavigator> 또는 <xref:System.Xml.XPath.XPathDocument> 개체에서 노드를 선택하고, XPath 식 결과를 평가 및 검사하고, XML 문서에 있는 노드가 지정된 XPath 식과 일치하는지 확인하는 <xref:System.Xml.XmlDocument> 클래스의 메서드에 대해 설명합니다.  
  
 [XPathNavigator를 사용 하 여 XML 데이터에 액세스](../../../../docs/standard/data/xml/accessing-xml-data-using-xpathnavigator.md)  
 <xref:System.Xml.XPath.XPathNavigator> 또는 <xref:System.Xml.XPath.XPathDocument> 개체에서 노드를 탐색하고, XML 데이터를 추출하고, 강력한 형식의 XML 데이터에 액세스하는 <xref:System.Xml.XmlDocument> 클래스의 메서드에 대해 설명합니다.  
  
 [XPathNavigator를 사용 하 여 XML 데이터 편집](../../../../docs/standard/data/xml/editing-xml-data-using-xpathnavigator.md)  
 <xref:System.Xml.XPath.XPathNavigator> 개체에 포함된 XML 문서에서 노드와 값을 삽입하거나 수정 및 제거하는 <xref:System.Xml.XmlDocument> 클래스의 메서드에 대해 설명합니다.  
  
 [XPathNavigator를 사용 하 여 스키마 유효성 검사](../../../../docs/standard/data/xml/schema-validation-using-xpathnavigator.md)  
 <xref:System.Xml.XPath.XPathDocument> 또는 <xref:System.Xml.XmlDocument> 개체에 포함된 XML 내용의 유효성을 검사하는 방법을 설명합니다.  
  
## <a name="see-also"></a>참고 항목  
 <xref:System.Xml.XmlDocument>  
 <xref:System.Xml.XPath.XPathDocument>  
 <xref:System.Xml.XPath.XPathNavigator>  
 [DOM 모델을 사용 하 여 XML 데이터 처리](../../../../docs/standard/data/xml/process-xml-data-using-the-dom-model.md)
