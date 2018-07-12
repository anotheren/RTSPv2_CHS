# RTSPv2_CHS

[Real-Time Streaming Protocol Version 2.0](https://tools.ietf.org/html/rfc7826) 中文翻译项目

本项目使用中英文对照的方式呈现，方便读者随时查找原文。如果你希望加入项目，欢迎认领章节；如果发现有翻译问题，欢迎提交 PR。

## Abstract 摘要

This memorandum defines the Real-Time Streaming Protocol (RTSP) version 2.0, which obsoletes RTSP version 1.0 defined in [RFC 2326](https://tools.ietf.org/html/rfc2326).

**这个备忘录定义了实时流传输协议（RTSP）版本 2.0，取代了定义在 [RFC 2326](https://tools.ietf.org/html/rfc2326) 中的 RTSP 版本 1.0。**

RTSP is an application-layer protocol for the setup and control of the delivery of data with real-time properties. RTSP provides an extensible framework to enable controlled, on-demand delivery of real-time data, such as audio and video. Sources of data can include both live data feeds and stored clips. This protocol is intended to control multiple data delivery sessions; provide a means for choosing delivery channels such as UDP, multicast UDP, and TCP; and provide a means for choosing delivery mechanisms based upon RTP ([RFC 3550](https://tools.ietf.org/html/rfc3550)).

**RTSP 是一种应用层协议，用于设置和控制具有实时数据传输。RTSP 提供了一个可扩展的框架，可以实现按需提供实时数据，如音频和视频。数据源可以包括实时数据和存储的剪辑。该协议旨在控制多个数据传输会话，并提供基于 UDP、multicast UDP 和 TCP 以及基于 RTP ([RFC 3550](https://tools.ietf.org/html/rfc3550)) 的传输通道。**

## Status of This Memo 本备忘录的状态

This is an Internet Standards Track document.

**这是一份互联网标准跟踪文档。**

This document is a product of the Internet Engineering Task Force (IETF). It represents the consensus of the IETF community. It has received public review and has been approved for publication by the Internet Engineering Steering Group (IESG). Further information on Internet Standards is available in [Section 2 of RFC 7841](https://tools.ietf.org/html/rfc7841#section-2).

**本文档是互联网工程任务组（IETF）的产品。 它代表了 IETF 社区的共识。它已经过公众审查，并已获得互联网工程指导小组（IESG）的批准发布。有关互联网标准的更多信息，请参见 [RFC 7841 第 2 节](https://tools.ietf.org/html/rfc7841#section-2)。**

Information about the current status of this document, any errata, and how to provide feedback on it may be obtained at [http://www.rfc-editor.org/info/rfc7826](http://www.rfc-editor.org/info/rfc7826).

**有关本文件当前状态，任何勘误以及如何提供反馈的信息，请访问 [http://www.rfc-editor.org/info/rfc7826](http://www.rfc-editor.org/info/rfc7826)。**

## Copyright Notice 版权声明

Copyright (c) 2016 IETF Trust and the persons identified as the document authors. All rights reserved.

**版权所有（c）2016 IETF Trust 和所有确认为文档作者的人。**

This document is subject to [BCP 78](https://tools.ietf.org/html/bcp78) and the IETF Trust's Legal Provisions Relating to IETF Documents ([http://trustee.ietf.org/license-info](http://trustee.ietf.org/license-info)) in effect on the date of publication of this document. Please review these documents carefully, as they describe your rights and restrictions with respect to this document. Code Components extracted from this document must include Simplified BSD License text as described in Section 4.e of the Trust Legal Provisions and are provided without warranty as described in the Simplified BSD License.

**本文件受 [BCP 78](https://tools.ietf.org/html/bcp78) 和 IETF Trust 有关 IETF 文件的法律规定的约束（[http://trustee.ietf.org/license-info](http://trustee.ietf.org/license-info)）自本文件发布之日起生效。请仔细阅读这些文档，因为它们描述了您对本文档的权利和限制。从本文档中提取的代码组件必须包含信任法律规定第 4.e 节中所述的简化 BSD 许可文本，并且不提供简化 BSD 许可中所述的保证。**

This document may contain material from IETF Documents or IETF Contributions published or made publicly available before November 10, 2008. The person(s) controlling the copyright in some of this material may not have granted the IETF Trust the right to allow modifications of such material outside the IETF Standards Process. Without obtaining an adequate license from the person(s) controlling the copyright in such materials, this document may not be modified outside the IETF Standards Process, and derivative works of it may not be created outside the IETF Standards Process, except to format it for publication as an RFC or to translate it into languages other than English.

**本文档可能包含 IETF 文档或 IETF 贡献的材料，这些材料在 2008 年 11 月 10 日之前发布或公开发布。控制本部分材料版权的人员可能未授予 IETF Trust 在 IETF 标准流程之外，允许修改此类材料的权利。如果没有从控制此类材料版权的人那里获得足够的许可，本文档不得在 IETF 标准流程之外进行修改，并且不得在 IETF 标准流程之外创建其衍生作品，除非格式化为 作为 RFC 发布或将其翻译成英语以外的语言。**

## Table of Contents 目录

### 1. [Introduction 介绍](./S01_Introduction.md)

### 2. [Protocol Overview 协议概要](./S02_ProtocolOverview.md)
#### 2.1. [Presentation Description 描述信息](./S02_ProtocolOverview.md#21-presentation-description-描述信息)
#### 2.2. [Session Establishment 会话建立](./S02_ProtocolOverview.md#22-session-establishment-会话建立)
#### 2.3. Media Delivery Control 
#### 2.4. Session Parameter Manipulations 
#### 2.5. Media Delivery 
##### 2.5.1. Media Delivery Manipulations
#### 2.6. Session Maintenance and Termination
#### 2.7. Extending RTSP

### 3. Document Conventions
#### 3.1. Notational Conventions
#### 3.2. Terminology

### 4. Protocol Parameters
#### 4.1. RTSP Version
#### 4.2. RTSP IRI and URI
#### 4.3. Session Identifiers
#### 4.4. Media-Time Formats
##### 4.4.1. SMPTE-Relative Timestamps
##### 4.4.2. Normal Play Time
##### 4.4.3. Absolute Time
#### 4.5. Feature Tags
#### 4.6. Message Body Tags
#### 4.7. Media Properties
##### 4.7.1. Random Access and Seeking
##### 4.7.2. Retention
##### 4.7.3. Content Modifications
##### 4.7.4. Supported Scale Factors
##### 4.7.5. Mapping to the Attributes
