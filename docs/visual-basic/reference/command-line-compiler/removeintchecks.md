---
title: /removeintchecks
ms.date: 07/20/2015
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: devlang-visual-basic
ms.topic: article
f1_keywords:
- removeintchecks
- /removeintchecks
helpviewer_keywords:
- removeintchecks compiler option [Visual Basic]
- /removeintchecks compiler option [Visual Basic]
- -removeintchecks compiler option [Visual Basic]
ms.assetid: c1835bd5-1e38-4fba-bd2f-6984774765d4
caps.latest.revision: "14"
author: dotnet-bot
ms.author: dotnetcontent
ms.openlocfilehash: 917977d8e5e12c231370ab3c956aca9d96e8a8a8
ms.sourcegitcommit: 34ec7753acf76f90a0fa845235ef06663dc9e36e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/21/2017
---
# <a name="removeintchecks"></a>/removeintchecks
오버플로 오류 설정 하거나 해제 하는 정수 연산에 대 한 검사를 설정 합니다.  
  
## <a name="syntax"></a>구문  
  
```  
/removeintchecks[+ | -]  
```  
  
## <a name="arguments"></a>인수  
  
|용어|정의|  
|---|---|  
|`+` &#124; `-`|선택 사항입니다. `/removeintchecks-` 옵션을 사용 하면 오버플로 오류에 대 한 모든 정수 연산 검사 합니다. 기본값은 `/removeintchecks-`입니다.<br /><br /> 지정 `/removeintchecks` 또는 `/removeintchecks+` 오류 검사를 차단 하 고 정수 계산을 더욱 빠르게 만들 수 있습니다. 오류를 검사 하지 않는 반면 및 잘못 된 결과 데이터 형식 용량 오버플로 하는 경우 오류를 일으키지 않고 저장 될 수 있습니다.|  
  
|Visual Studio에서 /removeintchecks를 설정 하려면 통합 개발 환경|  
|---|  
|1.  **솔루션 탐색기**에서 프로젝트를 선택합니다. **프로젝트** 메뉴에서 **속성**을 클릭합니다. <br />2.  **컴파일** 탭을 클릭합니다.<br />3.  **고급** 단추를 클릭합니다.<br />4.  값을 수정 된 **정수 오버플로 검사 해제** 상자입니다.|  
  
## <a name="example"></a>예  
 다음 코드에서는 `Test.vb` 정수 오버플로 오류 검사를 해제 합니다.  
  
```  
vbc /removeintchecks+ test.vb  
```  
  
## <a name="see-also"></a>참고 항목  
 [Visual Basic 명령줄 컴파일러](../../../visual-basic/reference/command-line-compiler/index.md)  
 [샘플 컴파일 명령줄](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
