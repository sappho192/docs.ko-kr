---
title: 'Custom Message Encoder: Custom Text Encoder'
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 68ff5c74-3d33-4b44-bcae-e1d2f5dea0de
caps.latest.revision: "28"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: 54e2b87a9e104ea61c32b06ffc604fc864283f3d
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="custom-message-encoder-custom-text-encoder"></a>Custom Message Encoder: Custom Text Encoder
이 샘플에서는 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]를 사용하여 사용자 지정 텍스트 메시지 인코더를 구현하는 방법을 보여 줍니다.  
  
> [!WARNING]
>  컴퓨터에 이 샘플이 이미 설치되어 있을 수도 있습니다. 계속하기 전에 다음(기본) 디렉터리를 확인하세요.  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  이 디렉터리가 없으면 [Windows Communication Foundation (WCF) and Windows Workflow Foundation (WF) Samples for .NET Framework 4(.NET Framework 4용 WCF(Windows Communication Foundation) 및 WF(Windows Workflow Foundation) 샘플)](http://go.microsoft.com/fwlink/?LinkId=150780) 로 이동하여 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] 및 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 샘플을 모두 다운로드하세요. 이 샘플은 다음 디렉터리에 있습니다.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageEncoder\Text`  
  
 <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>의 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]는 UTF-8, UTF-16 및 Big Endian 유니코드 인코딩만 지원합니다. 이 샘플의 사용자 지정 텍스트 메시지 인코더는 상호 운용성에 필요할 수 있는 모든 플랫폼에서 지원하는 문자 인코딩을 지원합니다. 이 샘플은 클라이언트 콘솔 프로그램(.exe), IIS(인터넷 정보 서비스)에서 호스팅하는 서비스 라이브러리(.dll) 및 텍스트 메시지 인코더 라이브러리(.dll)로 구성됩니다. 이 서비스는 요청-회신 통신 패턴을 정의하는 계약을 구현합니다. 계약은 수학 연산(Add, Subtract, Multiply 및 Divide)을 노출시키는 `ICalculator` 인터페이스에 의해 정의됩니다. 클라이언트에서 지정된 수학 작업을 동기적으로 요청하면 서비스에서 결과로 회신합니다. 클라이언트와 서비스는 모두 기본 `CustomTextMessageEncoder` 대신 <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>를 사용합니다.  
  
 사용자 지정 인코더 구현은 메시지 인코더 팩터리, 메시지 인코더, 메시지 인코딩 바인딩 요소 및 구성 처리기로 구성되며 다음 방법을 보여 줍니다.  
  
-   사용자 지정 인코더 및 인코더 팩터리 빌드  
  
-   사용자 지정 인코더의 바인딩 요소 만들기  
  
-   사용자 지정 바인딩 요소 통합에 사용자 지정 바인딩 구성 사용  
  
-   사용자 지정 바인딩 요소의 파일 구성을 허용하는 사용자 지정 구성 처리기 개발  
  
### <a name="to-set-up-build-and-run-the-sample"></a>샘플을 설치, 빌드 및 실행하려면  
  
1.  다음 명령을 사용하여 [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)] 4.0을 설치합니다.  
  
    ```  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2.  수행 했는지 확인 하십시오.는 [Windows Communication Foundation 샘플의 일회 설치 절차](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)합니다.  
  
3.  지침에 따라 솔루션을 빌드하려면 [Windows Communication Foundation 샘플 빌드](../../../../docs/framework/wcf/samples/building-the-samples.md)합니다.  
  
4.  지침에 따라 단일 또는 다중 컴퓨터 구성에서 샘플을 실행 하려면 [Windows Communication Foundation 샘플 실행](../../../../docs/framework/wcf/samples/running-the-samples.md)합니다.  
  
## <a name="message-encoder-factory-and-the-message-encoder"></a>메시지 인코더 팩터리 및 메시지 인코더  
 <xref:System.ServiceModel.ServiceHost> 또는 클라이언트 채널이 열려 있으면 디자인 타임 구성 요소 `CustomTextMessageBindingElement`는 `CustomTextMessageEncoderFactory`를 만듭니다. 이 팩터리는 `CustomTextMessageEncoder`를 만듭니다. 메시지 인코더는 스트리밍 모드와 버퍼링 모드에서 모두 작동하며 <xref:System.Xml.XmlReader> 및 <xref:System.Xml.XmlWriter>를 사용하여 각각 메시지를 읽고 씁니다. UTF-8, UTF-16 및 Big-Endian 유니코드만 지원하도록 최적화된 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]의 XML 판독기 및 작성기와 달리 이러한 판독기와 작성기는 모든 플랫폼에서 지원하는 인코딩을 지원합니다.  
  
 다음 코드 예제는 CustomTextMessageEncoder를 보여 줍니다.  
  
```csharp  
public class CustomTextMessageEncoder : MessageEncoder  
{  
    private CustomTextMessageEncoderFactory factory;  
    private XmlWriterSettings writerSettings;  
    private string contentType;  
  
    public CustomTextMessageEncoder(CustomTextMessageEncoderFactory factory)  
    {  
        this.factory = factory;  
  
        this.writerSettings = new XmlWriterSettings();  
        this.writerSettings.Encoding = Encoding.GetEncoding(factory.CharSet);  
        this.contentType = string.Format("{0}; charset={1}",   
            this.factory.MediaType, this.writerSettings.Encoding.HeaderName);  
    }  
  
    public override string ContentType  
    {  
        get  
        {  
            return this.contentType;  
        }  
    }  
  
    public override string MediaType  
    {  
        get   
        {  
            return factory.MediaType;  
        }  
    }  
  
    public override MessageVersion MessageVersion  
    {  
        get   
        {  
            return this.factory.MessageVersion;  
        }  
    }  
  
    public override Message ReadMessage(ArraySegment<byte> buffer, BufferManager bufferManager, string contentType)  
    {     
        byte[] msgContents = new byte[buffer.Count];  
        Array.Copy(buffer.Array, buffer.Offset, msgContents, 0, msgContents.Length);  
        bufferManager.ReturnBuffer(buffer.Array);  
  
        MemoryStream stream = new MemoryStream(msgContents);  
        return ReadMessage(stream, int.MaxValue);  
    }  
  
    public override Message ReadMessage(Stream stream, int maxSizeOfHeaders, string contentType)  
    {  
        XmlReader reader = XmlReader.Create(stream);  
        return Message.CreateMessage(reader, maxSizeOfHeaders, this.MessageVersion);  
    }  
  
    public override ArraySegment<byte> WriteMessage(Message message, int maxMessageSize, BufferManager bufferManager, int messageOffset)  
    {  
        MemoryStream stream = new MemoryStream();  
        XmlWriter writer = XmlWriter.Create(stream, this.writerSettings);  
        message.WriteMessage(writer);  
        writer.Close();  
  
        byte[] messageBytes = stream.GetBuffer();  
        int messageLength = (int)stream.Position;  
        stream.Close();  
  
        int totalLength = messageLength + messageOffset;  
        byte[] totalBytes = bufferManager.TakeBuffer(totalLength);  
        Array.Copy(messageBytes, 0, totalBytes, messageOffset, messageLength);  
  
        ArraySegment<byte> byteArray = new ArraySegment<byte>(totalBytes, messageOffset, messageLength);  
        return byteArray;  
    }  
  
    public override void WriteMessage(Message message, Stream stream)  
    {  
        XmlWriter writer = XmlWriter.Create(stream, this.writerSettings);  
        message.WriteMessage(writer);  
        writer.Close();  
    }  
}  
```  
  
 다음 코드 예제는 메시지 인코더 팩터리를 빌드하는 방법을 보여 줍니다.  
  
```csharp  
public class CustomTextMessageEncoderFactory : MessageEncoderFactory  
{  
    private MessageEncoder encoder;  
    private MessageVersion version;  
    private string mediaType;  
    private string charSet;  
  
    internal CustomTextMessageEncoderFactory(string mediaType, string charSet,  
        MessageVersion version)  
    {  
        this.version = version;  
        this.mediaType = mediaType;  
        this.charSet = charSet;  
        this.encoder = new CustomTextMessageEncoder(this);  
    }  
  
    public override MessageEncoder Encoder  
    {  
        get   
        {   
            return this.encoder;  
        }  
    }  
  
    public override MessageVersion MessageVersion  
    {  
        get   
        {   
            return this.version;  
        }  
    }  
  
    internal string MediaType  
    {  
        get  
        {  
            return this.mediaType;  
        }  
    }  
  
    internal string CharSet  
    {  
        get  
        {  
            return this.charSet;  
        }  
    }  
}  
```  
  
## <a name="message-encoding-binding-element"></a>메시지 인코딩 바인딩 요소  
 바인딩 요소를 사용하면 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 런타임 스택을 구성할 수 있습니다. [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 응용 프로그램에서 사용자 지정 메시지 인코더를 사용하려면 런타임 스택의 적절한 수준에서 적절한 설정으로 메시지 인코더 팩터리를 만드는 바인딩 요소가 필요합니다.  
  
 `CustomTextMessageBindingElement`는 <xref:System.ServiceModel.Channels.BindingElement> 기본 클래스에서 파생되며 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> 클래스에서 상속됩니다. 따라서 다른 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 구성 요소가 이 바인딩 요소를 메시지 인코딩 바인딩 요소로 인식할 수 있습니다. <xref:System.ServiceModel.Channels.MessageEncodingBindingElement.CreateMessageEncoderFactory%2A>의 구현은 일치하는 메시지 인코더 팩터리의 인스턴스를 알맞은 설정과 함께 반환합니다.  
  
 `CustomTextMessageBindingElement`는 속성을 통해 `MessageVersion`, `ContentType` 및 `Encoding`에 대한 설정을 노출합니다. 인코더는 Soap11Addressing 및 Soap12Addressing1 버전을 모두 지원하며 기본값은 Soap11Addressing1입니다. `ContentType`의 기본값은 "text/xml"입니다. `Encoding` 속성을 사용하면 원하는 문자 인코딩의 값을 설정할 수 있습니다. 이 샘플의 클라이언트 및 서비스는 <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>의 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]에서는 지원되지 않는 ISO-8859-1(Latin1) 문자 인코딩을 사용합니다.  
  
 다음 코드는 사용자 지정 텍스트 메시지 인코더를 사용하여 프로그래밍 방식으로 바인딩을 만드는 방법을 보여 줍니다.  
  
```  
ICollection<BindingElement> bindingElements = new List<BindingElement>();  
HttpTransportBindingElement httpBindingElement = new HttpTransportBindingElement();  
CustomTextMessageBindingElement textBindingElement = new CustomTextMessageBindingElement();  
bindingElements.Add(textBindingElement);  
bindingElements.Add(httpBindingElement);  
CustomBinding binding = new CustomBinding(bindingElements);  
```  
  
## <a name="adding-metadata-support-to-the-message-encoding-binding-element"></a>메시지 인코딩 바인딩 요소에 메타데이터 지원 추가  
 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>에서 파생되는 모든 형식은 서비스에 대해 생성되는 WSDL 문서에서 SOAP 바인딩 버전을 업데이트합니다. 이 작업은 `ExportEndpoint` 인터페이스에서 <xref:System.ServiceModel.Description.IWsdlExportExtension> 메서드를 구현한 다음 생성된 WSDL을 수정하여 수행됩니다. 이 샘플에서 `CustomTextMessageBindingElement`는 `TextMessageEncodingBinidngElement`의 WSDL 내보내기 논리를 사용합니다.  
  
 이 샘플에서 클라이언트 구성은 수동 구성입니다. `CustomTextMessageBindingElement`는 그 동작을 설명하기 위해 정책 어설션을 내보내지 않으므로 Svcutil.exe를 사용하여 클라이언트 구성을 생성할 수 없습니다. 일반적으로 사용자 지정 바인딩 요소에서 <xref:System.ServiceModel.Description.IPolicyExportExtension> 인터페이스를 구현하여 바인딩 요소에서 구현한 동작 또는 기능을 설명하는 사용자 지정 정책 어설션을 내보내야 합니다. 사용자 지정 바인딩 요소에 대 한 정책 어설션을 내보내야 하는 방법의 예제를 보려면는 [전송: UDP](../../../../docs/framework/wcf/samples/transport-udp.md) 샘플.  
  
## <a name="message-encoding-binding-configuration-handler"></a>메시지 인코딩 바인딩 구성 처리기  
 이전 단원에서는 프로그래밍 방식으로 사용자 지정 텍스트 메시지 인코더를 사용하는 방법을 보여 줍니다. `CustomTextMessageEncodingBindingSection`은 구성 파일 내에서 사용자 지정 텍스트 메시지 인코더를 사용하도록 지정할 수 있는 구성 처리기를 구현합니다. `CustomTextMessageEncodingBindingSection` 클래스는 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> 클래스에서 파생됩니다. `BindingElementType` 속성은 이 섹션에서 만들 바인딩 요소의 형식을 구성 시스템에 알립니다.  
  
 `CustomTextMessageBindingElement`에 정의된 모든 설정은 `CustomTextMessageEncodingBindingSection`에 속성으로 노출됩니다. <xref:System.Configuration.ConfigurationPropertyAttribute>는 구성 요소 특성을 속성에 매핑하고 특성이 설정되지 않은 경우 기본값을 설정하도록 합니다. 구성의 값이 로드되어 형식의 속성에 적용된 후에는 속성을 바인딩 요소의 구체적인 인스턴스로 변환하는 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement.CreateBindingElement%2A> 메서드가 호출됩니다.  
  
 이 구성 처리기는 서비스나 클라이언트의 App.config 또는 Web.config에 있는 다음 표현에 매핑됩니다.  
  
```xml  
<customTextMessageEncoding encoding="utf-8" contentType="text/xml" messageVersion="Soap11Addressing1" />  
```  
  
 이 샘플은 ISO-8859-1 인코딩을 사용합니다.  
  
 이 구성 처리기를 사용하려면 다음 구성 요소를 사용하여 등록해야 합니다.  
  
```xml  
<extensions>  
    <bindingElementExtensions>  
        <add name="customTextMessageEncoding" type="   
Microsoft.ServiceModel.Samples.CustomTextMessageEncodingBindingSection,   
                  CustomTextMessageEncoder" />  
    </bindingElementExtensions>  
</extensions>  
```  
  
## <a name="see-also"></a>참고 항목
