---
title: marshalCleanupError MDA
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- cleanup operations
- marshaling, run-time errors
- managed debugging assistants (MDAs), marshaling
- MDAs (managed debugging assistants), marshaling
- marshaling cleanup error
- MarshalCleanupError MDA
- memory, cleanup errors
ms.assetid: 2f5d9e7c-ae51-4155-a435-54347aa1f091
caps.latest.revision: "12"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: 641d12131f1502ce1ef00c6cf3889c803bd9fce3
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="marshalcleanuperror-mda"></a>marshalCleanupError MDA
`marshalCleanupError` MDA(관리 디버깅 도우미)는 네이티브 코드 경계와 관리 코드 경계 간에 데이터 형식을 마샬링하는 데 사용되는 임시 구조 및 메모리를 정리하려고 하는 동안 CLR(공용 언어 런타임)에 오류가 발생하면 활성화됩니다.  
  
## <a name="symptoms"></a>증상  
 네이티브 및 관리 코드 변환을 수행할 때 메모리 누수가 발생하고 런타임 상태(예: 스레드 문화권)가 복원되지 않거나 <xref:System.Runtime.InteropServices.SafeHandle> 정리에서 오류가 발생합니다.  
  
## <a name="cause"></a>원인  
 임시 구조를 정리하는 동안 예기치 않은 오류가 발생했습니다.  
  
## <a name="resolution"></a>해결 방법  
 모든 <xref:System.Runtime.InteropServices.SafeHandle> 소멸자, 종료자 및 사용자 지정 마샬러 구현에 오류가 있는지 검토합니다.  
  
## <a name="effect-on-the-runtime"></a>런타임에 대한 영향  
 이 MDA는 CLR에 아무런 영향을 미치지 않습니다.  
  
## <a name="output"></a>출력  
 정리 중에 실패한 작업을 보고하는 메시지입니다.  
  
## <a name="configuration"></a>구성  
  
```xml  
<mdaConfig>  
  <assistants>  
    <marshalCleanupError />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>참고 항목  
 <xref:System.Runtime.InteropServices.MarshalAsAttribute>  
 [관리 디버깅 도우미를 사용하여 오류 진단](../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)  
 [interop 마샬링](../../../docs/framework/interop/interop-marshaling.md)
