---
title: "온라인 및 오프라인 상태 추가"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 05e5f51d-81b6-4c17-b364-9dda447a5fce
caps.latest.revision: "5"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: adb767d3b7a7b991ebcfd8c44e55edb8726cb627
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="adding-online-and-offline-status"></a>온라인 및 오프라인 상태 추가
대부분의 경우 응용 프로그램에서 피어 채널 연결 상태에 대한 자세한 내용을 모니터링하는 것이 중요합니다. 이러한 정보는 `GetProperty` 인터페이스 구현에서 <xref:System.ServiceModel.IOnlineStatus> 메서드를 호출하여 얻을 수 있습니다. 이러한 인터페이스가 구현된 개체는 연결 상태를 모니터링하거나 `OnOnline`과 `OnOffline`과 같은 이벤트 처리기를 등록할 수 있으며 온라인 상태가 변경되는 즉시 반응할 수 있습니다.  
  
 피어 채널 인프라에서 클라이언트는 하나 이상의 다른 피어와 연결되어 있으면 온라인으로, 그렇지 않으면 오프라인으로 간주됩니다. 이 점은 개발 응용 프로그램을 디버깅하거나 자세한 정보를 최종 사용자에게 표시하는 데 특히 유용합니다.  
  
> [!NOTE]
>  온라인 이벤트 처리기는 메시지를 보내기 전에 먼저 노드가 열려 있는지 확인합니다.  
  
## <a name="see-also"></a>참고 항목  
 [피어 채널 응용 프로그램 빌드](../../../../docs/framework/wcf/feature-details/building-a-peer-channel-application.md)
