---
title: "-target:exe(C# 컴파일러 옵션)"
ms.date: 07/20/2015
ms.prod: .net
ms.technology: devlang-csharp
ms.topic: article
f1_keywords: /exe
helpviewer_keywords:
- target compiler options [C#], /target:exe
- /target compiler options [C#], /target:exe
- -target compiler options [C#], /target:exe
ms.assetid: bda5717d-1b91-4848-956b-fcf85c30e432
caps.latest.revision: "12"
author: BillWagner
ms.author: wiwagn
ms.openlocfilehash: b66ab51b30e17ab2f34f88158c3f6095e185468d
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2017
---
# <a name="targetexe-c-compiler-options"></a>/target:exe(C# 컴파일러 옵션)
**/target:exe** 옵션을 사용하면 컴파일러가 콘솔 응용 프로그램 실행 파일(EXE)을 만듭니다.  
  
## <a name="syntax"></a>구문  
  
```console  
/target:exe  
```  
  
## <a name="remarks"></a>주의  
 기본적으로 **/target:exe** 옵션이 적용됩니다. 실행 파일은 .exe 확장명으로 생성됩니다.  
  
 [/target:winexe](../../../csharp/language-reference/compiler-options/target-winexe-compiler-option.md)를 사용하여 Windows 프로그램 실행 파일을 만들 수 있습니다.  
  
 [/out](../../../csharp/language-reference/compiler-options/out-compiler-option.md) 옵션을 사용하여 지정하지 않으면 [Main](../../../csharp/programming-guide/main-and-command-args/index.md) 메서드가 포함된 입력 파일의 이름이 출력 파일의 이름으로 사용됩니다.  
  
 명령줄에서 지정된 경우, 다음 **/out** 또는 **/target: module** 옵션까지의 모든 파일이 .exe 파일을 만드는 데 사용됩니다.  
  
 .exe 파일로 컴파일되는 소스 코드 파일에 **Main** 메서드가 하나만 있어야 합니다. [/main](../../../csharp/language-reference/compiler-options/main-compiler-option.md) 컴파일러 옵션을 사용하면 코드에 **Main** 메서드를 포함하는 클래스가 둘 이상 있는 경우 **Main** 메서드를 포함하는 클래스를 지정할 수 있습니다.  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 개발 환경에서 이 컴파일러 옵션을 설정하려면  
  
1.  프로젝트 **속성** 페이지를 엽니다.  
  
2.  **응용 프로그램** 속성 페이지를 클릭합니다.  
  
3.  **출력 형식** 속성을 수정합니다.  
  
 이 컴파일러 옵션을 프로그래밍 방식으로 설정하는 방법에 대한 자세한 내용은 <xref:VSLangProj80.ProjectProperties3.OutputType%2A>를 참조하세요.  
  
## <a name="example"></a>예제  
 다음 명령줄은 각각 `in.cs`를 컴파일하고 `in.exe`를 만듭니다.  
  
```console  
csc /target:exe in.cs  
csc in.cs  
```  
  
## <a name="see-also"></a>참고 항목  
 [/target (C# 컴파일러 옵션)](../../../csharp/language-reference/compiler-options/target-compiler-option.md)  
 [C# 컴파일러 옵션](../../../csharp/language-reference/compiler-options/index.md)
