---
title: "방법: LINQ to XML XPath (Visual Basic)를 사용 하 여 쿼리"
ms.custom: 
ms.date: 07/20/2015
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: devlang-visual-basic
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: e1f69a20-1efa-452d-9089-c472fa84b3d5
caps.latest.revision: "3"
author: dotnet-bot
ms.author: dotnetcontent
ms.openlocfilehash: 97f885a76a79832b06f2f5d4a270d418bf244cb2
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2017
---
# <a name="how-to-query-linq-to-xml-using-xpath-visual-basic"></a>방법: LINQ to XML XPath (Visual Basic)를 사용 하 여 쿼리
이 항목에서는 XPath를 사용하여 XML 트리를 쿼리할 수 있도록 하는 확장명 메서드에 대해 소개합니다. 이러한 확장 메서드 사용에 대한 자세한 내용은 <xref:System.Xml.XPath.Extensions?displayProperty=nameWithType>를 참조하세요.  
  
 레거시 코드의 광범위한 사용과 같이 XPath를 사용하여 쿼리할 매우 구체적인 이유가 없는 한 LINQ to XML과 함께 XPath를 사용하지 않는 것이 좋습니다. XPath 쿼리의 성능은 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 쿼리의 성능보다 좋지 않습니다.  
  
## <a name="example"></a>예제  
 다음 예제에서는 작은 XML 트리를 만들고 <xref:System.Xml.XPath.Extensions.XPathSelectElements%2A>를 사용하여 요소 집합을 선택합니다.  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <Child1>1</Child1>  
        <Child1>2</Child1>  
        <Child1>3</Child1>  
        <Child2>4</Child2>  
        <Child2>5</Child2>  
        <Child2>6</Child2>  
    </Root>  
  
Dim list As IEnumerable(Of XElement) = root.XPathSelectElements("./Child2")  
For Each el As XElement In list  
    Console.WriteLine(el)  
Next  
```  
  
 이 예제는 다음과 같은 출력을 생성합니다.  
  
```xml  
<Child2>4</Child2>  
<Child2>5</Child2>  
<Child2>6</Child2>  
```  
  
## <a name="see-also"></a>참고 항목  
 [고급 쿼리 기술 (LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/advanced-query-techniques-linq-to-xml.md)
