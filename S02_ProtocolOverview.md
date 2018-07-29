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

## 2.2. Session Establishment 会话建立

The RTSP client can request the establishment of an RTSP session after having used the presentation description to determine which media streams are available, which media delivery protocol is used, and the resource identifiers of the media streams. The RTSP session is a common context between the client and the server that consists of one or more media resources that are to be under common media delivery control.

**在使用描述信息确定了哪些媒体流可用，使用哪种媒体传送协议以及媒体流的资源标识符之后，RTSP 客户端可以请求建立 RTSP 会话。RTSP 会话是客户端和服务器之间的通用上下文，其由一个或多个要在通用媒体传送控制下的媒体资源组成。**

The client creates an RTSP session by sending a request using the SETUP method ([Section 13.3](TODO)) to the server. In the Transport header ([Section 18.54](TODO)) of the SETUP request, the client also includes all the transport parameters necessary to enable the media delivery protocol to function. This includes parameters that are preestablished by the presentation description but necessary for any middlebox to correctly handle the media delivery protocols. The Transport header in a request may contain multiple alternatives for media delivery in a prioritized list, which the server can select from. These alternatives are typically based on information in the presentation description.

**客户端通过使用 SETUP 方法（[第 13.3 节](TODO)）向服务器发送请求来创建 RTSP 会话。在 SETUP 请求的传输头（[第 18.54 节](TODO)）中，客户端还写入了包括使媒体传送协议生效所需的所有传输参数。这包括由描述信息预先建立但是任何中间框正确处理媒体传送协议所必需的参数。请求中的传输头可以包含用于在优先级列表中进行媒体传送的多个备选，服务器可以从中选择。这些替代方案通常基于描述信息。**

When receiving a SETUP request, the server determines if the media resource is available and if one or more of the of the transport parameter specifications are acceptable. If that is successful, an RTSP session context is created and the relevant parameters and state is stored. An identifier is created for the RTSP session and included in the response in the Session header ([Section 18.49](TODO)). The SETUP response includes a Transport header that specifies which of the alternatives has been selected and relevant parameters.

**当接收到 SETUP 请求时，服务器确定媒体资源是否可用，以及一个或多个传输参数是否可接受。如果成功，则创建 RTSP 会话上下文并存储相关参数和状态。RTSP 会话会同时创建一个标识符，该标识符包含在会头部的响应中（[第 18.49 节](TODO)）。SETUP 响应包括了传输头，其指定了哪个备选项已被选择以及相关的参数。**

A SETUP request that references an existing RTSP session but identifies a new media resource is a request to add that media resource under common control with the already-present media resources in an aggregated session. A client can expect this to work for all media resources under RTSP control within a multimedia content container. However, a server will likely refuse to aggregate resources from different content containers. Even if an RTSP session contains only a single media stream, the RTSP session can be referenced by the aggregate control URI.

**引用现有 RTSP 会话，但标识了新媒体资源的另一个 SETUP 请求，会在聚合会话中，将已经存在的媒体资源，添加到共同控制中。客户端可以认为这适用于多媒体内容容器内，所有 RTSP 控制下的媒体资源。但是，服务器仍然可能会拒绝聚合来自不同内容容器内资源的请求。即使 RTSP 会话仅包含单个媒体流，聚合控制 URI 也会引用 RTSP 会话。**

To avoid an extra round trip in the session establishment of aggregated RTSP sessions, RTSP 2.0 supports pipelined requests; i.e., the client can send multiple requests back-to-back without waiting first for the completion of any of them. The client uses a client-selected identifier in the Pipelined-Requests header ([Section 18.33](TODO)) to instruct the server to bind multiple requests together as if they included the session identifier.

**为避免在聚合 RTSP 会话的会话建立中多次发送与接收，RTSP 2.0 支持管线化请求。即，客户端可以一个接一个地发送多个请求，而无需先等待任何请求的完成。客户端在管线化请求头（[第 18.33 节](TODO)）中，使用客户端已选择标识符，来指示服务器将多个请求绑定在一起，就好像它们包含会话标识符一样。**

The SETUP response also provides additional information about the established sessions in a couple of different headers. The Media-Properties header ([Section 18.29](TODO)) includes a number of properties that apply for the aggregate that is valuable when doing media delivery control and configuring user interface. The Accept-Ranges header ([Section 18.5](TODO)) informs the client about range formats that the server supports for these media resources. The Media-Range header ([Section 18.30](TODO)) informs the client about the time range of the media currently available.

**SETUP 响应还在几个不同的请求头中，提供有关已建立会话的附加信息。Media-Properties header（[第 18.29 节](TODO)）包括许多适用于聚合的属性，这些属性在进行媒体传送控制和配置用户界面时很有用。Accept-Ranges header（[第 18.5 节](TODO)）通知客户端，服务器当前支持的媒体资源的格式范围。Media-Range header（[第 18.30 节](TODO)）通知客户端，当前媒体可用的时间范围。**

## 2.3. Media Delivery Control 媒体传送

After having established an RTSP session, the client can start controlling the media delivery. The basic operations are "begin playback", using the PLAY method (Section 13.4) and "suspend (pause) playback" by using the PAUSE method (Section 13.6). PLAY also allows for choosing the starting media position from which the server should deliver the media. The positioning is done by using the Range header (Section 18.40) that supports several different time formats: Normal Play Time (NPT) (Section 4.4.2), Society of Motion Picture and Television Engineers (SMPTE) Timestamps (Section 4.4.1), and absolute time (Section 4.4.3). The Range header also allows the client to specify a position where delivery should end, thus allowing a specific interval to be delivered.

**建立 RTSP 会话后，客户端可以开始控制媒体传送。基本操作是“开始播放”，使用 PLAY 方法（第 13.4 节）和“暂停（暂停）播放”，使用 PAUSE 方法（第 13.6 节）。PLAY 方法还允许选择服务器传送媒体的开始位置。定位通过使用支持多种不同时间格式的 Range header（第 18.40 节）来完成：正常播放时间（NPT）（第 4.4.2 节），电影和电视工程师协会（SMPTE）时间戳（第 4.4.1 节） 和绝对时间（第 4.4.3 节）。Range header 还允许客户端指定交付应该结束的位置，从而允许传递特定的一段媒体。**

The support for positioning/searching within media content depends on the content's media properties. Content exists in a number of different types, such as on-demand, live, and live with simultaneous recording. Even within these categories, there are differences in how the content is generated and distributed, which affect how it can be accessed for playback. The properties applicable for the RTSP session are provided by the server in the SETUP response using the Media-Properties header (Section 18.29). These are expressed using one or several independent attributes. A first attribute is Random-Access, which indicates whether positioning is possible, and with what granularity. Another aspect is whether the content will change during the lifetime of the session. While on-demand content will be provided in full from the beginning, a live stream being recorded results in the length of the accessible content growing as the session goes on. There also exists content that is dynamically built by a protocol other than RTSP and, thus, also changes in steps during the session, but maybe not continuously. Furthermore, when content is recorded, there are cases where the complete content is not maintained, but, for example, only the last hour. All of these properties result in the need for mechanisms that will be discussed below.

**对媒体内容中的定位/搜索的支持，取决于内容的媒体属性。内容以多种不同类型存在，例如点播、直播和带有同步录制的直播。即使在这些类别中，内容的生成和分发方式也存在差异，这会影响内容的播放方式。适用于 RTSP 会话的属性由服务器在 SETUP 响应中使用 Media-Properties header 提供（第 18.29 节）。它们使用一个或多个独立属性表示的。第一个属性是随机访问，它指示是否可以进行定位，以及具有什么样的精度。另一个属性是内容是否会在会话的生命周期内发生变化。虽然点播内容将从头开始全部提供，但正在录制的直播流会导致可访问内容的长度随着会话的进行而增长。还存在由除 RTSP 之外的协议动态构建的内容，因此，在会话期间也可以改变步骤，但可能不是连续的。此外，当记录内容时，存在不保持完整内容的情况，例如仅保存在最后一小时内容。所有这些属性的机制需要将在下面进一步讨论。**

When the client accesses on-demand content that allows random access, the client can issue the PLAY request for any point in the content between the start and the end. The server will deliver media from the closest random access point prior to the requested point and indicate that in its PLAY response. If the client issues a PAUSE, the delivery will be halted and the point at which the server stopped will be reported back in the response. The client can later resume by sending a PLAY request without a Range header. When the server is about to complete the PLAY request by delivering the end of the content or the requested range, the server will send a PLAY_NOTIFY request (Section 13.5) indicating this.

**当客户端访问允许随机访问的点播内容时，客户端可以在发布内容始末间的任何一点使用 PLAY 请求。服务器将在请求的点之前，从最近的随机访问点开始传送媒体，并在其 PLAY 响应中指明该媒体。如果客户端发出 PAUSE 请求，则将停止传递，并且将在响应中报告服务器停止的位置。客户端稍后可以通过发送没有 Range header 的 PLAY 请求来恢复。当服务器即将传送内容或请求范围的结尾，来完成 PLAY 请求时，服务器将发送 PLAY_NOTIFY 请求（第 13.5 节）来指示这一点。**

When playing live content with no extra functions, such as recording, the client will receive the live media from the server after having sent a PLAY request. Seeking in such content is not possible as the server does not store it, but only forwards it from the source of the session. Thus, delivery continues until the client sends a PAUSE request, tears down the session, or the content ends.

**当播放没有额外功能（例如录制）的直播内容时，客户端将在发送 PLAY 请求后从服务器接收直播媒体。Seeking 在此类内容上是不可能的，因为服务器不存储它，而只是从会话源转发它。因此，传递将继续，直到客户端发送 PAUSE 请求，断开会话或内容结束。**

For live sessions that are being recorded, the client will need to keep track of how the recording progresses. Upon session establishment, the client will learn the current duration of the recording from the Media-Range header. Because the recording is ongoing, the content grows in direct relation to the time passed. Therefore, each server's response to a PLAY request will contain the current Media-Range header. The server should also regularly send (approximately every 5 minutes) the current media range in a PLAY_NOTIFY request (Section 13.5.2). If the live transmission ends, the server must send a PLAY_NOTIFY request with the updated Media-Properties indicating that the content stopped being a recorded live session and instead became on-demand content; the request also contains the final media range. While the live delivery continues, the client can request to play the current live point by using the NPT timescale symbol "now", or it can request a specific point in the available content by an explicit range request for that point. If the requested point is outside of the available interval, the server will adjust the position to the closest available point, i.e., either at the beginning or the end.

**对于正在录制的实时会话，客户端需要跟踪录制的进度。会话建立后，客户端将从 Media-Range header 了解当前记录的持续时间。由于录制正在进行，内容与经过的时间直接相关。因此，每个服务器对 PLAY 请求的响应将包含当前的 Media-Range header。服务器还应定期发送（大约每 5 分钟）PLAY_NOTIFY 请求，来说明当前的媒体范围（第 13.5.2 节）。如果实时传输结束，则服务器必须发送带有更新的 Media-Properties 的 PLAY_NOTIFY 请求，该属性指示内容停止只是录制，并成为点播内容。同时，该请求还包含了最终媒体范围。当直播传送继续时，客户端可以通过使用 NPT 时间刻度符号“现在”来请求播放当前实时点，或者它可以通过针对该时间点的显式范围请求，来请求可用内容中的特定时间点。如果请求的时间点在可用内容之外，则服务器将位置调整到最接近的可用点，不论是开始或是结束。**

A special case of recording is that where the recording is not retained longer than a specific time period; thus, as the live delivery continues, the client can access any media within a moving window that covers, for example, "now" to "now" minus 1 hour. A client that pauses on a specific point within the content may not be able to retrieve the content anymore. If the client waits too long before resuming the pause point, the content may no longer be available. In this case, the pause point will be adjusted to the closest point in the available media.

**录制的一个特例是，录制保留时间不能超过特定时间段。因此，当实时传送继续时，客户端可以访问已记录内容中的任何媒体，例如“现在”到“现在”减去1小时。暂停内容中特定点的客户端可能无法再次检索内容。如果客户端在恢复暂停点之前等待太长时间，则内容可能不再可用。在这种情况下，暂停点将调整到可用媒体中的最近点。**