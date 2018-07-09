# Introduction 介绍

This memo defines version 2.0 of the Real-Time Streaming Protocol (RTSP 2.0). RTSP 2.0 is an application-layer protocol for the setup and control over the delivery of data with real-time properties, typically streaming media. Streaming media is, for instance, video on demand or audio live streaming. Put simply, RTSP acts as a "network remote control" for multimedia servers.

**本备忘录定义了实时流协议（RTSP 2.0）的 2.0 版本。RTSP 2.0 是一种应用层协议，用于设置和控制具有实时属性（通常是流媒体）的数据传输。例如，流媒体是视频点播或音频直播。 简而言之，RTSP 充当多媒体服务器的“网络远程控制”。**

The protocol operates between RTSP 2.0 clients and servers, but it also supports the use of proxies placed between clients and servers. Clients can request information about streaming media from servers by asking for a description of the media or use media description provided externally. The media delivery protocol is used to establish the media streams described by the media description. Clients can then request to play out the media, pause it, or stop it completely. The requested media can consist of multiple audio and video streams that are delivered as time-synchronized streams from servers to clients.

**该协议在使用 RTSP 2.0 客户端和服务器之间运行，但它也支持在客户端和服务器之间使用代理。客户端可以通过媒体描述，或者使用外部提供的媒体描述，来从服务器请求有关流媒体的信息。媒体传送协议用于从媒体描述来建立媒体流。然后，客户端可以请求播放媒体，暂停播放或完全停止播放。请求的媒体可以包含多个音频和视频流，这些流以时间同步流的形式从服务器到客户端传递。**

RTSP 2.0 is a replacement of RTSP 1.0 [[RFC2326](https://tools.ietf.org/html/rfc2326)] and this document obsoletes that specification. This protocol is based on RTSP 1.0 but is not backwards compatible other than in the basic version negotiation mechanism. The changes between the two documents are listed in [Appendix I](./AppendixI.md). There are many reasons why RTSP 2.0 can't be backwards compatible with RTSP 1.0; some of the main ones are as follows:
* Most headers that needed to be extensible did not define the allowed syntax, preventing safe deployment of extensions;
* the changed behavior of the PLAY method when received in Play state;
* the changed behavior of the extensibility model and its mechanism; and 
* the change of syntax for some headers.

**RTSP 2.0 是 RTSP 1.0 [[RFC2326](https://tools.ietf.org/html/rfc2326)] 的替代品，本文档废弃了该规范。此协议基于 RTSP 1.0，但除了基本版本协商机制外，不向后兼容。这两份文件之间的变化列于 [附录 I](./AppendixI.md) 中。 RTSP 2.0 无法与 RTSP 1.0 向后兼容的原因有很多，一些主要原因如下：**
* **大多数需要可扩展的 Header 都没有定义允许的语法，从而阻止了扩展的安全部署；**
* **在播放状态下接收时 PLAY 方法的行为发生改变；**
* **可扩展性模型及其机制的行为发生改变；**
* **某些 Header 的语法发生改变。**

There are so many small updates that changing versions became necessary to enable clarification and consistent behavior. Anyone implementing RTSP for a new use case in which they have not installed RTSP 1.0 should only implement RTSP 2.0 to avoid having to deal with RTSP 1.0 inconsistencies.

**有如此多细小的更新，使得需要变更版本来确保行为的一致。为新用例引入 RTSP 时都应该只实现 RTSP 2.0，以避免必须处理与 RTSP 1.0 的不一致。**

This document is structured as follows. It begins with an overview of the protocol operations and its functions in an informal way. Then, a set of definitions of terms used and document conventions is introduced. These are followed by the actual RTSP 2.0 core protocol specification. The appendices describe and define some functionalities that are not part of the core RTSP specification, but which are still important to enable some usages. Among them, the RTP usage is defined in [Appendix C](./AppendixC.md), the Session Description Protocol (SDP) usage with RTSP is defined in [Appendix D](./AppendixD.md), and the "text/parameters" file format [Appendix F](./AppendixF.md), are three normative specification appendices. Other appendices include a number of informational parts discussing the changes, use cases, different considerations or motivations.

**本文档的结构如下。它首先以非正式的方式概述该协议的操作及其功能。然后，介绍了一组使用的术语定义和文档约定。接下来是实际的 RTSP 2.0 核心协议规范。附录描述并定义了一些不属于核心 RTSP 规范的功能，但对于实现某些用法仍然很重要。其中，RTP 的使用在 [附录 C](./AppendixC.md) 中定义，会话描述协议（SDP）与 RTSP 的使用在 [附录 D](./AppendixD.md) 中定义，而“text/parameters”文件格式在 [附录 F](./AppendixF.md) 中定义，这三个是规范性附录。其他附录包括一些讨论变更，用例，考虑不同因素或动机的信息。**

[返回目录](./README.md#table-of-contents-目录)
