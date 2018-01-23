---
title: "F # 둘러보기"
description: "F # 프로그래밍 언어에서이 둘러보기는 샘플 코드의 주요 기능 중 일부를 검사 합니다."
keywords: "visual f #, f #, 기능 프로그래밍,.NET, 둘러보기"
author: cartermp
ms.author: phcart
ms.date: 01/24/2017
ms.topic: article
ms.prod: .net
ms.technology: devlang-fsharp
ms.devlang: fsharp
ms.assetid: 49775139-082e-442f-b5a2-dd402399b5d2
ms.openlocfilehash: c027e6b71f35fc3b58750eb164124de145244825
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2017
---
# <a name="tour-of-f"></a>F # 둘러보기 #

F#을 가장 잘 배울 수 있는 방법은 직접 F# 코드를 읽고 쓰는 것입니다. 이 문서는 F# 언어의 주요 기능 중 몇 가지를 안내하고 컴퓨터에서 실행할 수 있는 몇 가지 코드 조각을 제공합니다. 개발 환경 설정에 대한 자세한 내용은 [F# 시작](tutorials/getting-started/index.md)을 참고하세요.

F#의 함수 및 자료형에는 두 가지 기본 개념이 있습니다. 이 문서는 두 개념에 해당하는 언어의 기능들을 강조합니다.

## <a name="how-to-run-the-code-samples"></a>코드 샘플을 실행하는 방법

>[!NOTE]
코드 샘플을 실행하기 위한 두 페이지는 [F# 체험하기](http://www.tryfsharp.org/Create) (Silverlight가 필요)와 Microsoft Azure에서 제공하는 [Azure 전자필기장을 위한 F#](https://notebooks.azure.com/Microsoft/libraries/fsharp/html/FSharp%20for%20Azure%20Notebooks.ipynb)입니다.

아래의 코드 샘플을 실행하는 가장 빠른 방법은 [F# Interactive](tutorials/fsharp-interactive/index.md)을 이용하는 것입니다. 그냥 코드 예제를 해당 페이지에서 복사/붙여넣기하여 실행할 수 있습니다. 또는 비주얼 스튜디오에서 F# 프로젝트를 만들고 컴파일하여 콘솔 응용 프로그램 코드를 실행할 수 있습니다. [시작](./get-started/index.md)섹션을 참조하세요.

## <a name="functions-and-modules"></a>함수와 모듈

모든 F# 프로그램의 가장 기본적인 요소는 ***함수***들로 구성된 ***모듈***들입니다. [함수](language-reference/functions/index.md)는 입력을 받아 출력을 만드는 역할을 담당하고, 이 함수들은 목적에 맞게 모여 [모듈](language-reference/modules.md)이 됩니다. 이것은 F#에서 임의의 항목들을 그룹화하는 기본적인 방법입니다. 이들은 [ `let` 바인딩](language-reference/functions/let-bindings.md)을 이용하여 정의되며, 이 정의를 통해 함수의 이름을 정하고 인자들을 정의합니다.

[!code-fsharp[BasicFunctions](../../samples/snippets/fsharp/tour.fs#L101-L133)]

또한 `let`바인딩은 다른 언어들에서의 변수 개념처럼 값을 이름에 연결하는 방법입니다.  `let`바인딩으로 연결된 값 또는 함수는 기본적으로 ***변경할 수 없습니다***. 이것은 다른 언어에서 변수들이 언제든 값을 ***변경 가능한*** 점과 반대됩니다. 만약 값을 바꿀 수 있도록 바인딩을 하고 싶다면 `let mutable ...` 구문을 쓰면 됩니다.

[!code-fsharp[Immutability](../../samples/snippets/fsharp/tour.fs#L75-L94)]

## <a name="numbers-booleans-and-strings"></a>숫자, 부울 및 문자열

F#은 .NET 언어이기 때문에 .NET에 존재하는 [기본 타입](language-reference/primitive-types.md)을 지원합니다.  
F#에서 표현되는 다양한 숫자 타입은 아래와 같습니다.

[!code-fsharp[Numbers](../../samples/snippets/fsharp/tour.fs#L49-L68)]

아래 코드를 통해 Boolean 타입의 값들과 이를 이용한 기본적인 논리 연산을 확인할 수 있습니다.

[!code-fsharp[Bools](../../samples/snippets/fsharp/tour.fs#L142-L152)]

다음 코드는 기본적인 [문자열](language-reference/strings.md)조작을 보여줍니다.

[!code-fsharp[Strings](../../samples/snippets/fsharp/tour.fs#L158-L180)]

## <a name="tuples"></a>튜플

[튜플](language-reference/tuples.md) F#의 강력한 기능 중 하나입니다. 이것은 익명의 정렬된 값들을 그룹으로 모아주며 이 그룹 자체가 하나의 값으로서 다뤄집니다. 튜플은 다양한 용도로 쓰일 수 있는데, 그 중 하나는 함수에서 여러 값들을 간단하게 반환하는 것이며 기타 여러 상황에서 유용하게 쓰입니다.

[!code-fsharp[Tuples](../../samples/snippets/fsharp/tour.fs#L186-L203)]

F# 4.1을 기준으로, `struct` 튜플도 만들 수 있습니다. 해당 자료형은 C#7/Visual Basic 15의 `struct` 튜플과 완전히 호환됩니다.  

[!code-fsharp[Tuples](../../samples/snippets/fsharp/tour.fs#L205-L218)]

유의해야 할 점은 `struct` 튜플은 값 타입이기 때문에 참조 튜플과 암시적으로 상호 변환될 수 없으므로 위 코드와 같이 명시적으로 변환해야 합니다.

## <a name="pipelines-and-composition"></a>파이프라인 및 구성

파이프 연산자 (`|>`, `<|`, `||>`, `<||`, `|||>`, `<|||`)와 합성 연산자 (`>>` 및 `<<`)는 F#에서 데이터를 처리할 때 널리 사용됩니다. 이러한 연산자는 유연한 방법으로 함수의 "파이프라인"을 설정할 수 있습니다. 다음 예제는 이러한 연산자를 활용하여 간단한 기능을 하는 파이프라인을 만듭니다.

[!code-fsharp[Pipelines](../../samples/snippets/fsharp/tour.fs#L227-L300)]

위의 코드는 리스트를 처리하는 함수를 포함하여 일급 객체 기능과 [인수의 부분 적용](language-reference/functions/index.md#partial-application-of-arguments)과 같은 F#의 여러 기능을 사용하고 있습니다. 각 이러한 개념에 대한 심층적 이해가 다소 어려울 수 있지만, 파이프라인을 만들 때 함수가 데이터를 처리 하기 위해 얼마나 쉽게 사용할 수 있는지를 명확하게 알아둬야 합니다.

## <a name="lists-arrays-and-sequences"></a>목록, 배열 및 시퀀스

리스트, 배열, 그리고 시퀀스는 F# 핵심 라이브러리의 세 가지 기본 컬렉션 타입입니다.

[리스트](language-reference/lists.md)는 동일한 타입의 요소들로 이루어진, 내용을 변경할 수 없는(immutable) 정렬된 컬렉션입니다. 즉, 이것은 나열을 목적으로 한 단일 연결 리스트이지만 그렇기 때문에 리스트의 크기가 커질 수록 임의의 요소에 접근하거나 리스트끼리 이어 붙이는 작업의 속도는 느려집니다. 이와 달리 다른 인기 있는 언어에서의 리스트는 일반적으로 단일 연결 리스트로 구현되지 않습니다.

[!code-fsharp[Lists](../../samples/snippets/fsharp/tour.fs#L309-L359)]

[배열](language-reference/arrays.md)은 같은 타입의 요소를 담는 고정된 크기의 *변경 가능한* 컬렉션입니다. 이들은 요소에 대한 빠른 임의 액세스를 지원하며 F#의 리스트보다 빠른데 이는 연속된 메모리 공간을 이용하기 때문입니다.

[!code-fsharp[Arrays](../../samples/snippets/fsharp/tour.fs#L368-L407)]

[시퀀스](language-reference/sequences.md)는 같은 타입의 요소들의 논리적인 집합입니다. 이것은 리스트나 배열보다 더 일반적인 타입으로, 당신의 관점을 요소들의 어떤 논리적인 집합으로든 만들어낼 수 있습니다. 시퀀스의 또다른 장점으로는 ***게으른*** 동작이 가능하여 요소들을 필요할 때만 가져와서 다룰 수 있다는 것입니다.

[!code-fsharp[Sequences](../../samples/snippets/fsharp/tour.fs#L418-L452)]

## <a name="recursive-functions"></a>재귀 함수

F#에서 컬렉션 또는 시퀀스의 요소를 처리하는 작업은 일반적으로 [재귀](language-reference/functions/index.md#recursive-functions)를 이용합니다. F#이 반복문 및 명령형 프로그래밍을 지원하긴 하지만, 재귀를 이용하는것이 더 쉽게 정확성을 보장할 수 있기 때문에 권장됩니다.

>[!NOTE]
다음 예제에서는 `match`를 이용해 패턴 매칭을 수행합니다. 이 기본 구조는 문서의 뒷부분에서 다룹니다.

[!code-fsharp[RecursiveFunctions](../../samples/snippets/fsharp/tour.fs#L461-L500)]

F# 또한 마무리 호출 최적화를 완전히 지원하는데, 이것은 재귀 호출을 최적화하여 반복문만큼 빠르게 돌아가도록 해줍니다.

## <a name="record-and-discriminated-union-types"></a>기록 및 구별된 공용 구조체 타입

기록 및 공용 구조체 타입은 F# 코드에서 사용되는 두 가지 기본 데이터 타입이며, 일반적으로 F# 프로그램에서 데이터를 나타내는 가장 좋은 방법입니다. 비록 이들이 다른 언어의 클래스와 비슷해보이지만, 이 두 타입은 구조적 동등한 의미를 가진다는 것이 클래스와 차별되는 부분입니다. 즉, 이 둘은 "태생적으로" 비교 가능하고 동등한지의 여부는 직관적으로 가능합니다. 그냥 하나가 다른 하나와 같은지 비교만 하면 됩니다.

[레코드](language-reference/records.md)는 이름이 있는 값들을 선택적 멤버(예: 메서드)와 함께 모은 것입니다. C# 또는 Java를 잘 알고 있다면 구조적으로 동등하고 규칙이 적다는 점에서 POCOs 또는 POJOs(Plain Old Java Object)와 비슷하다고 느끼실 겁니다. 

[!code-fsharp[Records](../../samples/snippets/fsharp/tour.fs#L507-L559)]

F# 4.1을 기준으로, 레코드를 `struct`들로 나타낼 수도 있습니다. 아래와 같이 `[<Struct>]` 속성을 이용하면 됩니다.

[!code-fsharp[Records](../../samples/snippets/fsharp/tour.fs#L561-L568)]

[구별된 공용 구조체 (DUs)](language-reference/discriminated-unions.md)는 이름이 있는 폼이나 경우들의 갯수가 될 수 있는 값입니다. 데이터 타입에 저장된 타입은 여러 고유 값들 중 하나일 수 있습니다.

[!code-fsharp[Unions](../../samples/snippets/fsharp/tour.fs#L575-L631)]

또한 DUs를 *단일 대/소문자 구별된 DUs*로 사용할 수도 있는데, 이로 인하여 기본 타입을 이용한 도메인 모델링에 도움이 될 수도 있습니다. 종종 시간, 문자열 및 다른 기본 타입들이 무언가를 대표하기 위해 사용되기 때문에 특별한 의미를 가지게 됩니다. 그러나 기본 데이터 형식만을 사용하면 실수로 잘못된 값을 할당할 수도 있습니다! 각 정보의 타입을 단일 대/소문자 구별된 DUs로 표현하면 그런 경우에 정확성을 높일 수 있습니다.

[!code-fsharp[Unions](../../samples/snippets/fsharp/tour.fs#L633-L654)]

위의 샘플에서 보여주듯이, 단일 대/소문자 구별된 DUs에서 값을 얻기 위해선 명시적으로 값들을 빼내야 합니다.

또한 DUs는 재귀 정의를 지원하는데, 이것으로 트리와 내재되어있는 재귀 데이터를 쉽게 표현할 수 있습니다. 예를 들어 아래 코드 예제를 통해 `exists` 및 `insert` 함수를 이용해 이진 검색 트리를 표시할 수 있습니다.

[!code-fsharp[Unions](../../samples/snippets/fsharp/tour.fs#L656-L683)]

DUs 데이터 타입을 이용해 트리의 재귀 구조를 나타낼 수 있기 때문에, 이 재귀 구조를 다루는 것은 직관적이며 정확성을 보장합니다. 아래와 같이 패턴 일치에서도 지원됩니다.

또한 DUs는 `struct`들과 `[<Struct>]` 속성으로 나타낼 수 있습니다.

[!code-fsharp[Unions](../../samples/snippets/fsharp/tour.fs#L685-L696)]

그러나 이렇게 할 때 염두에 두 가지 주요 사항 있습니다.

1. DU 구조체를 재귀적으로 정의 될 수 없습니다.
2. DU 구조체에는 각 해당 경우에 대 한 고유한 이름을 가져야 합니다.

위의 따르도록 실패 하면 컴파일 오류가 발생 합니다.

## <a name="pattern-matching"></a>패턴 일치

[패턴 일치](language-reference/pattern-matching.md) 은 F # 언어는 기능 정확성 F # 타입에 대해 작업을 사용 하도록 설정 합니다.  위의 샘플에서 이와 많은 양의 `match x with ...` 구문입니다.  이 구조를 사용 하면 컴파일러에서 일치 하는 철저 한 패턴을 통해 데이터 타입을 사용 하는 경우 모든 가능한 사례 수에 대 한 계정 해도 데이터 타입의 "shape" 이해할 수 있습니다.  올바른지 만큼 강력한 이며 컴파일 타임에 런타임 문제가 리라 어떤 "리프트"를 영리 하 게 사용할 수 있습니다.

[!code-fsharp[PatternMatching](../../samples/snippets/fsharp/tour.fs#L705-L739)]

줄임을 사용할 수도 있습니다 `function` 패턴 일치를 위해 사용 하도록 하는 함수를 작성 하는 경우 유용 구문을 [부분 응용 프로그램](language-reference/functions/index.md#partial-application-of-arguments):

[!code-fsharp[PatternMatching](../../samples/snippets/fsharp/tour.fs#L741-L759)]

사용 하는 단어로 것은 `_` 패턴입니다.  로 알려져는 [와일드 카드 패턴](language-reference/pattern-matching.md#wildcard-pattern), "기능이 문제가 있습니다" 방법도 방법입니다.  실수로 철저 한 패턴 일치를 무시 하 고 더 이상 사용 하 여 주의 하지 않은 경우 컴파일 타임 강요에서 이점을 얻을 수 편리 하지만 `_`합니다.  분해 된 타입의 특정 부분에 대 한 중요 하지 않으면 할 경우 유용 때 패턴을 일치 또는 마지막 절 식과 일치 하는 패턴의 의미 있는 모든 사례를 열거 했습니다.

[활성 패턴](language-reference/active-patterns.md) 패턴 일치를 사용 하는 다른 강력한 구성 됩니다.  패턴 일치 호출 사이트에서 분해 하는 사용자 지정 양식, 입력된 데이터를 분할할 수 있습니다 수 있습니다.  또한 매개 변수화 할 수, 되므로 다음 파티션 함수로 정의 합니다.  확장 활성 패턴을 지원 하기 위해 앞의 예제는 다음과 같습니다.

[!code-fsharp[ActivePatterns](../../samples/snippets/fsharp/tour.fs#L761-L783)]

## <a name="optional-types"></a>선택적 타입

구별 된 공용 구조체 유형의 특수 한 사례는는 F # 핵심 라이브러리의 부분이 아니라는 유용한 옵션 종류입니다.

[옵션 종류](language-reference/options.md) 는 두 가지 경우 중 하나를 나타내는 타입이:는 값 또는 전혀 nothing입니다.  값 수 또는 특정 작업으로 발생 하지 않을 수 있는 모든 시나리오에서 사용 됩니다.  다음 런타임 문제 보다는 컴파일 타임 문제가 있어서 두 경우를 고려 하 여 강제로 합니다.  Api에서 종종 사용 됩니다 여기서 `null` 나타내는 데 사용 됩니다 "없음" 대신에 대해 걱정할 필요가 없도록 `NullReferenceException` 대부분의 경우에서.

[!code-fsharp[Options](../../samples/snippets/fsharp/tour.fs#L791-L811)]

## <a name="units-of-measure"></a>측정 단위

F #의 타입 시스템의 한 가지 고유한 특징 측정 단위를 통해 숫자 리터럴에 대 한 컨텍스트를 제공할 수 있다는 점입니다.

[측정 단위](language-reference/units-of-measure.md) 미터, 등의 단위를 숫자 유형 연관 시킬 수 있는 함수 작업을 수행 숫자 리터럴을 보다는 단위에 있습니다.  그러면 컴파일러가 특정 컨텍스트에서 의미가에 전달 된 숫자 리터럴 타입, 작업의 종류와 관련 된 런타임 오류 헤드를 확인할 수 있습니다.

[!code-fsharp[UnitsOfMeasure](../../samples/snippets/fsharp/tour.fs#L818-L839)]

F # 핵심 라이브러리는 다양 한 SI 단위 유형과 단위 변환 정의합니다.  자세한 내용을 보려면 체크 아웃 된 [Microsoft.FSharp.Data.UnitSystems.SI Namespace](https://msdn.microsoft.com/visualfsharpdocs/conceptual/microsoft.fsharp.data.unitsystems.si-namespace-%5bfsharp%5d)합니다.

## <a name="classes-and-interfaces"></a>클래스 및 인터페이스

F #에.NET 클래스에 대 한 완벽 하 게 지원 [인터페이스](language-reference/interfaces.md), [추상 클래스](language-reference/abstract-classes.md), [상속](language-reference/inheritance.md)등입니다.

[클래스](language-reference/classes.md) .NET 개체를 나타내는 속성, 메서드 및 이벤트를 사용할 수 있는 해당 [멤버](language-reference/members/index.md)합니다.

[!code-fsharp[Classes](../../samples/snippets/fsharp/tour.fs#L848-L877)]

제네릭 클래스 정의 매우 간단 이기도 합니다.

[!code-fsharp[Classes](../../samples/snippets/fsharp/tour.fs#L884-L905)]

인터페이스를 구현 하려면 하나를 사용 `interface ... with` 구문 또는 [개체 식](language-reference/object-expressions.md)합니다.

[!code-fsharp[Classes](../../samples/snippets/fsharp/tour.fs#L912-L931)]

## <a name="which-types-to-use"></a>사용 하는 타입

중요 한 질문에 대 한 잠재 고객 클래스, 레코드, 구분 된 공용 구조체 및 튜플의 현재 상태: 하는 언제 사용 하나요?  요소와 마찬가지로 대부분의 수명, 대답 하면 상황에 따라 다릅니다.

튜플은 자체를 값으로 임시 집계 값의를 사용 하는 함수에서 여러 값을 반환 하는 데 적합 합니다.

레코드는 "의 다음 단계인" Tuples, 레이블 및 선택적 멤버에 대 한 지원을 이름이 필요 합니다.  데이터 전송 중인 프로그램을 통해 낮은 공식 절차 표현에 대 한 훌륭한 않은 것입니다.  구조적 같음 했기 때문에 비교를 통해 사용 하기 쉬운 됩니다.

구별 된 공용 구조체는 여러 가지 있지만 핵심 장점은 모든 가능한 "셰이프에 대해" 데이터를 가질 수 있는 계정에 패턴 일치와 함께에서를 사용할 수입니다.  

클래스는 엄청난 양의 정보를 나타내고도 기능에 해당 정보를 연결 해야 하는 경우 등의 이유로 적합 합니다.  경험상,으로 개념적으로 일부 데이터에 연결 되어 있는 기능을 사용 하는 경우 클래스 및 개체 지향 프로그래밍의 원칙을 사용 하 여 큰 장점은 합니다.  클래스는 또한 기본 데이터 타입을 C# 및 Visual Basic의 경우와 상호 작용할 때 이러한 언어 클래스를 사용 하 여 거의 모든 항목에 대 한 합니다.

## <a name="next-steps"></a>다음 단계

이제 살펴보았으므로 언어의 기본 기능 중 일부를 첫 번째 F # 프로그램 작성을 준비 해야 합니다!  체크 아웃 [시작](tutorials/getting-started/index.md) 개발 환경을 설정 하 고 일부 코드를 작성 하는 방법을 배울 수 있습니다.

좀 더 배우려면를 위한 다음 단계를 든 수 있지만 권장 [고급 값으로 함수](introduction-to-functional-programming/functions-as-first-class-values.md) <!--[Introduction to Functional Programming in F#](introduction-to-functional-programming/index.md)--> 핵심 함수형 프로그래밍 개념에 익숙하지 얻으려고 합니다.  이러한 필수 견고한 프로그램 F #에서 작성 됩니다.

또한 체크 아웃의 [F # 언어 참조](language-reference/index.md) 개념 콘텐츠의 광범위 한 F # 상에 서 볼 수 있습니다.
