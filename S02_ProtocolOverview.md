# 2. Protocol Overview 协议概要

This section provides an informative overview of the different mechanisms in the RTSP 2.0 protocol to give the reader a high-level understanding before getting into all the specific details. In case of conflict with this description and the later sections, the later sections take precedence. For more information about use cases considered for RTSP, see [Appendix E](./AppendixE.md).

**本节提供了 RTSP 2.0 协议中不同机制的信息概述，以便在深入了解所有具体细节之前为读者提供宏观的理解。如果与此描述和后面的部分发生冲突，则以后面的部分为准。有关 RTSP 考虑的用例的更多信息，请参阅 [附录 E](./AppendixE.md)。**

RTSP 2.0 is a bidirectional request and response protocol that first establishes a context including content resources (the media) and then controls the delivery of these content resources from the provider to the consumer. RTSP has three fundamental parts: Session Establishment, Media Delivery Control, and an extensibility model described below. The protocol is based on some assumptions about existing functionality to provide a complete solution for client-controlled real-time media delivery.

**RTSP 2.0 是双向的响应式协议，首先建立内容资源（媒体）的上下文，然后控制这些内容资源从提供者到使用者的传递。RTSP 有三个基本部分：会话建立，媒体传送控制和底层可扩展性模型。该协议基于对现有功能的一些假设，为客户端控制的实时媒体传送提供完整的解决方案。**

RTSP uses text-based messages, requests and responses, that may contain a binary message body. An RTSP request starts with a method line that identifies the method, the protocol, and version and the resource on which to act. The resource is identified by a URI and the hostname part of the URI is used by RTSP client to resolve the IPv4 or IPv6 address of the RTSP server. Following the method line are a number of RTSP headers. These lines are ended by two consecutive carriage return line feed (CRLF) character pairs. The message body, if present, follows the two CRLF character pairs, and the body's length is described by a message header. RTSP responses are similar, but they start with a response line with the protocol and version followed by a status code and a reason phrase. RTSP messages are sent over a reliable transport protocol between the client and server. RTSP 2.0 requires clients and servers to implement TCP and TLS over TCP as mandatory transports for RTSP messages.

**RTSP 使用基于文本的消息，请求和响应，并可能包含二进制的消息体。一个 RTSP 请求以方法行开始，该方法行定义了方法，协议，版本以及要操作的资源。资源由 URI 标识，RTSP 客户端使用 URI 的主机名部分来解析 RTSP 服务器的 IPv4 或 IPv6 地址。方法行之后是许多个 RTSP 消息头。这些行以两个连续的回车换行（CRLF）字符对结束。消息体（如果存在）遵循两个 CRLF 字符对的形式，并且消息体的长度由消息头描述。RTSP 响应类似，但它们以响应行开头，包括协议和版本，后面跟着状态代码和响应结果短语。RTSP 消息通过客户端和服务器之间的可靠传输协议发送。RTSP 2.0 要求客户端和服务器强制实现 TCP 和 TLS over TCP 来传输 RTSP 消息。**
