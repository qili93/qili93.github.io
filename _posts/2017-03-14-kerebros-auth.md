---
layout: post
title: Kerberos认证原理
categories: 技术解读
tags: Kerberos
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

参考文章
1. [Kerberos认证原理](http://blog.csdn.net/wulantian/article/details/42418231)
2. [Kerberos Explained](https://msdn.microsoft.com/en-us/library/bb742516.aspx)

Kerberos认证机制中共有三个角色：KDC(Kerberos Distribution Center)，Client和Server。KDC在整个Kerberos Authentication中作为Client和Server共同信任的第三方起着重要的作用。KDC主要提供两种服务： Authentication Service (AS) 和 Ticket-Granting Service (TGS)

![img](../images/kerberos_ticket_exchange.gif)

### 一、Authentication基本原理

先引入两个重要概念：

> - **Long-term Key/Master Key**：在Security的领域中，有的Key可能长期内保持不变，比如你在密码，可能几年都不曾改变，这样的Key、以及由此派生的Key被称为Long-term Key。对于Long-term Key的使用有这样的原则：被Long-term Key加密的数据不应该在网络上传输。原因很简单，一旦这些被Long-term Key加密的数据包被恶意的网络监听者截获，在原则上，只要有充足的时间，他是可以通过计算获得你用于加密的Long-term Key的——任何加密算法都不可能做到绝对保密。
> - 在一般情况下，对于一个Account来说，密码往往仅仅限于该Account的所有者知晓，甚至对于任何Domain的Administrator，密码仍然应该是保密的。但是密码却又是证明身份的凭据，所以必须通过基于你密码的派生的信息来证明用户的真实身份，在这种情况下，一般将你的密码进行Hash运算得到一个Hash code, 我们一般管这样的Hash Code叫做Master Key。由于Hash Algorithm是不可逆的，同时保证密码和Master Key是一一对应的，这样既保证了你密码的保密性，有同时保证你的Master Key和密码本身在证明你身份的时候具有相同的效力。
> - **Short-term Key/Session Key**：由于被Long-term Key加密的数据包不能用于网络传送，所以我们使用另一种Short-term Key来加密需要进行网络传输的数据。由于这种Key只在一段时间内有效，即使被加密的数据包被黑客截获，等他把Key计算出来的时候，这个Key早就已经过期了。

KDC在整个Kerberos Authentication中作为Client和Server共同信任的第三方起着重要的作用，而Kerberos的认证过程就是通过这3方协作完成。对于一个Windows Domain来说，Domain Controller扮演着KDC的角色。KDC维护着一个存储着该Domain中所有帐户的Account Database（一般地，这个Account Database由AD来维护），也就是说，他知道属于每个Account的名称和派生于该Account Password的Master Key。而用于Client和Server相互认证的sessionKey就是有KDC分发。下面我们来看看KDC分发sessionKey的过程。

{% mermaid %}
sequenceDiagram
participant KDC
participant ClientA
participant ServerB
ClientA ->> KDC: [1] I'm clientA, I want to visit serverA
Note over KDC,ClientA: KDC create new sessionKey
KDC -->> ClientA: [2] msg1: sessionKey(encrypt by masterKeyA)
KDC -->> ClientA: [2] SessionTicket: sessionKey+clientAInfo(encrypt by masterKeyB)
Note over ClientA,ServerB: decrypt sessionKey by hash(pwdA)
ClientA ->> ServerB: SessionTicket: sessionKey+clientAInfo(encrypt by masterKeyB)
ClientA ->> ServerB: msg3: TimeStamp+clientAInfo+(encrypt by sessionKey)
Note over ClientA,ServerB: decrypt sessionKey by hash(pwdB) + timeStamp by sessionKey
ServerB -->> ClientA: msg4: TimeStamp (for mutual authentication)
{% endmermaid %}

具体步骤分析如下：

1. ClientA向KDC发送一个对sessionKey的申请"我是ClientA，我需要一个Session Key用于访问ServerB"。KDC在接收到这个请求的时候，生成一个SessionKey，为了保证这个SessionKey仅仅限于发送请求的ClientA和他希望访问的ServerB知晓，KDC会为这个SessionKey生成两个Copy，分别被ClientA(msg1)和ServerB(SessionTicket)使用。然后从Account database中提取ClientA的MasterKeyA和ServerB的MasterKeyB分别对这两个Copy进行对称加密。我们把通过Server的MasterKeyB加密过的数据包称为SessionTicket，和SessionKey一起被加密的还包含关于Client的一些信息。
2. ClientA通过自己的MasterKeyA对KDC加密的msg1进行解密从而获得SessionKey，随后创建Authenticator（Client Info + Timestamp）并用SessionKey对其加密(msg3)。最后连同从KDC获得的、被Server的MasterKeyB加密过的数据包（Client Info + Session Key）一并发送到Server端。
3. 当ServerB接收到这两组数据后，先使用他自己的MasterKeyB对SessionTicket进行解密，从而获得SessionKey。随后使用该SessionKey解密Authenticator(msg3)，通过比较Authenticator中的Client Info和Session Ticket中的Client Info从而实现对Client的认证。

#### 为何要用Timestamp

我们试想这样的现象：Client向Server发送的数据包被某个恶意网络监听者截获，该监听者随后将数据包座位自己的Credential冒充该Client对Server进行访问，在这种情况下，依然可以很顺利地获得Server的成功认证。为了解决这个问题，Client在Authenticator中会加入一个当前时间的Timestamp。

在Server对Authenticator中的Client Info和Session Ticket中的Client Info进行比较之前，会先提取Authenticator中的Timestamp，并同当前的时间进行比较，如果他们之间的偏差超出一个可以接受的时间范围（一般是5mins），Server会直接拒绝该Client的请求。在这里需要知道的是，Server维护着一个列表，这个列表记录着在这个可接受的时间范围内所有进行认证的Client和认证的时间。对于时间偏差在这个可接受的范围中的Client，Server会从这个这个列表中获得最近一个该Client的认证时间，只有当Authenticator中的Timestamp晚于通过一个Client的最近的认证时间的情况下，Server采用进行后续的认证流程。

#### 双向认证（Mutual Authentication）

Kerberos一个重要的优势在于它能够提供双向认证：不但Server可以对Client 进行认证，Client也能对Server进行认证。

具体过程是这样的，如果Client需要对他访问的Server进行认证，会在它向Server发送的Credential中设置一个是否需要认证的Flag。Server在对Client认证成功之后，会把Authenticator中的Timestamp提出出来，通过Session Key进行加密，当Client接收到并使用Session Key进行解密之后，如果确认Timestamp和原来的完全一致，那么他可以认定Server正式他试图访问的Server。

那么为什么Server不直接把通过Session Key进行加密的Authenticator原样发送给Client，而要把Timestamp提取出来加密发送给Client呢？原因在于防止恶意的监听者通过获取的Client发送的Authenticator冒充Server获得Client的认证。

### 二、引入Ticket-Granting Service (TGS)

Kerberos实际上一个基于Ticket的认证方式。Client想要获取Server端的资源，先得通过Server的认证；而认证的先决条件是Client向Server提供从KDC获得的一个有Server的Master Key进行加密的**Session Ticket（Session Key + Client Info）**。可以这么说，Session Ticket是Client进入Server领域的一张门票。而这张门票必须从一个合法的Ticket颁发机构获得，这个颁发机构就是**Client和Server双方信任的KDC**， 同时这张Ticket具有超强的防伪标识：它是被Server的Master Key加密的。对Client来说， 获得Session Ticket是整个认证过程中最为关键的部分。

如果我们把Client提供给Server进行认证的Ticket比作股票的话，那么Client在从KDC那边获得Ticket之前，需要先获得这个Ticket的认购权证（合法的股票账户证书），这个认购权证在Kerberos中被称为TGT：Ticket Granting Ticket，TGT的分发方仍然是KDC。

我们现在来看看Client是如何从KDC处获得TGT的：

{% mermaid %}
sequenceDiagram
participant KDC
participant ClientA
participant ServerB
ClientA ->> KDC: [1] I'm clientA, I want a TGT to get tickets
Note over KDC,ClientA: KDC create new sessionKey2
KDC -->> ClientA: [2] msg1: sessionKey2 [encrypt by masterKeyA]
KDC -->> ClientA: [2] TGT(sessionKey2+ClientInfo) [encrypt by KDC master key]
Note over KDC,ClientA: ClientA get sessionKey2 with decrypt by hash(pwdA)
ClientA ->> KDC: [3] msg2 (ClientInfo+ServerB) [encrypt by sessionKey2]
ClientA ->> KDC: [3] TGT(sessionKey2+ClientInfo) [encrypt by KDC master key]
Note over KDC,ClientA: KDC decrypt and compare two clientInfos
KDC -->> ClientA: [4] msg3: sessionKey1(encrypt by masterKeyA)
KDC -->> ClientA: [4] SessionTicket: sessionKey1+clientInfo(encrypt by masterKeyB)
{% endmermaid %}

具体流程如下：

1. 首先Client向KDC发起对TGT的申请，申请的内容大致可以这样表示：“我需要一张TGT用以申请获取用以访问所有Server的Ticket”。KDC在收到该申请请求后，生成一个用于该Client和KDC进行安全通信的SessionKey2（SKDC-Client）。为了保证该SessionKey2仅供该Client和自己使用，KDC使用Client的Master Key和自己的Master Key对生成的Session Key进行加密，从而获得两个加密的SessionKey2（SKDC-Client）的Copy。对于后者，随SKDC-Client一起被加密的还包含以后用于鉴定Client身份的关于Client的一些信息。最后KDC将这两份Copy一并发送给Client。这里有一点需要注意的是：为了免去KDC对于基于不同Client的Session Key进行维护的麻烦，就像Server不会保存Session Key（SServer-Client）一样，KDC也不会去保存这个Session Key（SKDC-Client），而选择完全靠Client自己提供的方式。
2. 当Client收到KDC的两个加密数据包之后，先使用自己的Master Key对第一个Copy进行解密，从而获得KDC和Client的SessionKey2（SKDC-Client），并把该SessionKey2和TGT进行缓存。此后Client可以使用SessionKey2向KDC申请用以访问每个Server的Ticket，相对于Client的Master Key这个Long-term Key，SessionKey2（SKDC-Client）是一个Short-term Key，安全保证得到更好的保障，这也是Kerberos多了这一步的关键所在。同时需要注意的是SessionKey2（SKDC-Client）是一个Session Key，他具有自己的生命周期，同时TGT和Session相互关联，当Session Key过期，TGT也就宣告失效，此后Client不得不重新向KDC申请新的TGT，KDC将会生成一个不同Session Key和与之关联的TGT。同时，由于Client Log off也导致SKDC-Client的失效，所以SKDC-Client又被称为Logon Session Key。
3. 接下来，我们看看Client如何使用TGT来从KDC获得基于某个Server的Ticket。在这里我要强调一下，Ticket是基于某个具体的Server的，而TGT则是和具体的Server无关的，Client可以使用一个TGT从KDC获得基于不同Server的Ticket。我们言归正传，Client在获得自己和KDC的SessionKey2（SKDC-Client）之后，生成自己的Authenticator（ClientInfo）以及所要访问的ServerB名称的并使用SessionKey2(SKDC-Client)进行加密。随后连同TGT一并发送给KDC。KDC使用自己的Master Key对TGT进行解密，提取Client Info和SessionKey2（SKDC-Client），然后使用这个SessionKey2（SKDC-Client）解密Authenticator获得Client Info，对两个Client Info进行比较进而验证对方的真实身份。验证成功，生成一份基于Client所要访问的Server的Ticket给Client，这个过程就是我们第一节中介绍的一样了。

### 三、Kerberos的3个Sub-protocol：整个Authentication

整个Kerberos authentication的流程大体上包含以下3个子过程：

1. Client向KDC申请TGT（Ticket Granting Ticket）
2. Client通过获得TGT向DKC申请用于访问Server的Ticket
3. Client最终向为了Server对自己的认证向其提交Ticket

不过上面的介绍离真正的Kerberos Authentication还是有一点出入。Kerberos整个认证过程通过3个sub-protocol来完成。这个3个Sub-Protocol分别完成上面列出的3个子过程。这3个sub-protocol分别为：

1. Authentication Service Exchange
2. Ticket Granting Service Exchange
3. Client/Server Exchange

#### Authentication Service Exchange

#### Ticket Granting Service Exchange

#### Client/Server Exchange
