# 2. Protocol Overview 协议概要

This section provides an informative overview of the different mechanisms in the RTSP 2.0 protocol to give the reader a high-level understanding before getting into all the specific details. In case of conflict with this description and the later sections, the later sections take precedence. For more information about use cases considered for RTSP, see [Appendix E](./AppendixE.md).

**本节提供了 RTSP 2.0 协议中不同机制的信息概述，以便在深入了解所有具体细节之前为读者提供宏观的理解。如果与此描述和后面的部分发生冲突，则以后面的部分为准。有关 RTSP 考虑的用例的更多信息，请参阅 [附录 E](./AppendixE.md)。**

RTSP 2.0 is a bidirectional request and response protocol that first establishes a context including content resources (the media) and then controls the delivery of these content resources from the provider to the consumer. RTSP has three fundamental parts: Session Establishment, Media Delivery Control, and an extensibility model described below. The protocol is based on some assumptions about existing functionality to provide a complete solution for client-controlled real-time media delivery.

**RTSP 2.0 是双向的响应式协议，首先建立内容资源（媒体）的上下文，然后控制这些内容资源从提供者到使用者的传递。RTSP 有三个基本部分：会话建立，媒体传送控制和底层可扩展性模型。该协议基于对现有功能的一些假设，为客户端控制的实时媒体传送提供完整的解决方案。**

RTSP uses text-based messages, requests and responses, that may contain a binary message body. An RTSP request starts with a method line that identifies the method, the protocol, and version and the resource on which to act. The resource is identified by a URI and the hostname part of the URI is used by RTSP client to resolve the IPv4 or IPv6 address of the RTSP server. Following the method line are a number of RTSP headers. These lines are ended by two consecutive carriage return line feed (CRLF) character pairs. The message body, if present, follows the two CRLF character pairs, and the body's length is described by a message header. RTSP responses are similar, but they start with a response line with the protocol and version followed by a status code and a reason phrase. RTSP messages are sent over a reliable transport protocol between the client and server. RTSP 2.0 requires clients and servers to implement TCP and TLS over TCP as mandatory transports for RTSP messages.

**RTSP 使用基于文本的消息，请求和响应，并可能包含二进制的消息体。一个 RTSP 请求以方法行开始，该方法行定义了方法，协议，版本以及要操作的资源。资源由 URI 标识，RTSP 客户端使用 URI 的主机名部分来解析 RTSP 服务器的 IPv4 或 IPv6 地址。方法行之后是许多个 RTSP 消息头。这些行以两个连续的回车换行（CRLF）字符对结束。消息体（如果存在）遵循两个 CRLF 字符对的形式，并且消息体的长度由消息头描述。RTSP 响应类似，但它们以响应行开头，包括协议和版本，后面跟着状态代码和响应结果短语。RTSP 消息通过客户端和服务器之间的可靠传输协议发送。RTSP 2.0 要求客户端和服务器强制实现 TCP 和 TLS over TCP 来传输 RTSP 消息。**

## 2.1. Presentation Description 描述信息

RTSP exists to provide access to multimedia presentations and content but tries to be agnostic about the media type or the actual media delivery protocol that is used. To enable a client to implement a complete system, an RTSP-external mechanism for describing the presentation and the delivery protocol(s) is used. RTSP assumes that this description is either delivered completely out of band or as a data object in the response to a client's request using the DESCRIBE method ([Section 13.2](TODO)).

**RTSP 的存在是为了提供对多媒体演示和内容的访问，却不了解媒体类型或实际使用的媒体传送协议。为了使客户端能够实现完整的系统，使用了一个 RTSP 扩展机制，来描述信息和使用的（一种或多种）传输协议。RTSP 假定这些描述信息要么完全依赖外部传输，要么作为一个数据对象，在应答客户端的响应中使用 DESCRIBE 方法描述（[第 13.2 节](TODO)）。**

Parameters that commonly have to be included in the presentation description are the following:

* The number of media streams;
* the resource identifier for each media stream/resource that is to be controlled by RTSP;
* the protocol that will be used to deliver each media stream;
* the transport protocol parameters that are not negotiated or vary with each client;
* the media-encoding information enabling a client to correctly decode the media upon reception; and
* an aggregate control resource identifier.

**通常必须包含在表示描述中的参数如下：**

* **媒体流的数量；**
* **由 RTSP 控制的每个媒体流/资源的资源标识符；**
* **将用于传递每个媒体流的协议；**
* **未协商的传输协议参数或随每个客户端而变化的传输协议参数；**
* **媒体编码信息，来确保客户端能够在接收时正确地解码媒体；**
* **聚合控制资源标识符。**

RTSP uses its own URI schemes ("rtsp" and "rtsps") to reference media resources and aggregates under common control (see [Section 4.2](TODO)).

**RTSP 使用自有的 URI schemes（“rtsp” 和 “rtsps”）来引用媒体资源和共同控制下的聚合（参见 [第 4.2 节](TODO)）。**

This specification describes in [Appendix D](./AppendixD.md) how one uses SDP [RFC4566](https://tools.ietf.org/html/rfc4566) for describing the presentation.

**本规范在 [附录 D](./AppendixE.md) 中描述了如何使用 SDP [[RFC4566](https://tools.ietf.org/html/rfc4566)] 来描述信息。**

## 2.2 Session Establishment 会话建立

The RTSP client can request the establishment of an RTSP session after having used the presentation description to determine which media streams are available, which media delivery protocol is used, and the resource identifiers of the media streams. The RTSP session is a common context between the client and the server that consists of one or more media resources that are to be under common media delivery control.

**在使用描述信息确定了哪些媒体流可用，使用哪种媒体传送协议以及媒体流的资源标识符之后，RTSP 客户端可以请求建立 RTSP 会话。RTSP 会话是客户端和服务器之间的通用上下文，其由一个或多个要在通用媒体传送控制下的媒体资源组成。**

The client creates an RTSP session by sending a request using the SETUP method (Section 13.3) to the server. In the Transport header (Section 18.54) of the SETUP request, the client also includes all the transport parameters necessary to enable the media delivery protocol to function. This includes parameters that are preestablished by the presentation description but necessary for any middlebox to correctly handle the media delivery protocols. The Transport header in a request may contain multiple alternatives for media delivery in a prioritized list, which the server can select from. These alternatives are typically based on information in the presentation description.

**客户端通过使用 SETUP 方法（第 13.3 节）向服务器发送请求来创建 RTSP 会话。在 SETUP 请求的传输头（第 18.54 节）中，客户端还写入了包括使媒体传送协议生效所需的所有传输参数。这包括由描述信息预先建立但是任何中间框正确处理媒体传送协议所必需的参数。请求中的传输头可以包含用于在优先级列表中进行媒体传送的多个备选，服务器可以从中选择。这些替代方案通常基于描述信息。**

When receiving a SETUP request, the server determines if the media resource is available and if one or more of the of the transport parameter specifications are acceptable. If that is successful, an RTSP session context is created and the relevant parameters and state is stored. An identifier is created for the RTSP session and included in the response in the Session header (Section 18.49). The SETUP response includes a Transport header that specifies which of the alternatives has been selected and relevant parameters.

**当接收到 SETUP 请求时，服务器确定媒体资源是否可用，以及一个或多个传输参数是否可接受。如果成功，则创建 RTSP 会话上下文并存储相关参数和状态。RTSP 会话会同时创建一个标识符，该标识符包含在会头部的响应中（第 18.49 节）。SETUP 响应包括了传输头，其指定了哪个备选项已被选择以及相关的参数。**

A SETUP request that references an existing RTSP session but identifies a new media resource is a request to add that media resource under common control with the already-present media resources in an aggregated session. A client can expect this to work for all media resources under RTSP control within a multimedia content container. However, a server will likely refuse to aggregate resources from different content containers. Even if an RTSP session contains only a single media stream, the RTSP session can be referenced by the aggregate control URI.

**引用现有 RTSP 会话，但标识了新媒体资源的另一个 SETUP 请求，会在聚合会话中，将已经存在的媒体资源，添加到共同控制中。客户端可以认为这适用于多媒体内容容器内，所有 RTSP 控制下的媒体资源。但是，服务器仍然可能会拒绝聚合来自不同内容容器内资源的请求。即使 RTSP 会话仅包含单个媒体流，聚合控制 URI 也会引用 RTSP 会话。**

To avoid an extra round trip in the session establishment of aggregated RTSP sessions, RTSP 2.0 supports pipelined requests; i.e., the client can send multiple requests back-to-back without waiting first for the completion of any of them. The client uses a client-selected identifier in the Pipelined-Requests header (Section 18.33) to instruct the server to bind multiple requests together as if they included the session identifier.

**为避免在聚合 RTSP 会话的会话建立中多次发送与接收，RTSP 2.0 支持管线化请求。即，客户端可以一个接一个地发送多个请求，而无需先等待任何请求的完成。客户端在管线化请求头（第 18.33 节）中，使用客户端已选择标识符，来指示服务器将多个请求绑定在一起，就好像它们包含会话标识符一样。**

The SETUP response also provides additional information about the established sessions in a couple of different headers. The Media-Properties header (Section 18.29) includes a number of properties that apply for the aggregate that is valuable when doing media delivery control and configuring user interface. The Accept-Ranges header (Section 18.5) informs the client about range formats that the server supports for these media resources. The Media-Range header (Section 18.30) informs the client about the time range of the media currently available.

**SETUP 响应还在几个不同的请求头中，提供有关已建立会话的附加信息。Media-Properties header（第 18.29 节）包括许多适用于聚合的属性，这些属性在进行媒体传送控制和配置用户界面时很有用。Accept-Ranges header（第 18.5 节）通知客户端，服务器当前支持的媒体资源的格式范围。Media-Range header（第 18.30 节）通知客户端，当前媒体可用的时间范围。**