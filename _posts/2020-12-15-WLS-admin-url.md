---
title: "[WebLogic] 웹로직 콘솔 특정 ip 접속 제한 방법"
author: Ingyung Park
date: 2020-12-17 23:40:00 +0900
categories: [WAS,WebLogic]
tags: [WebLogic,WAS]
---

---
**Contents**

{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}
---

<br/>

# **WebLogic Admin Console 특정 ip만 허용/차단 방법**

---

웹로직 콘솔 접속을 하신 뒤, **[도메인명] > [보안(Security)] > [필터(Filter)]** 로 들어가주세요.

<br />

변경하실 내용은 다음과 같습니다.

<br />

### **1. 접속 필터 입력**

접속 필터 란에 아래 내용을 작성해주세요.

**weblogic.security.net.ConnectionFilterImpl**

 <br />

### **2. 접속 필터 규칙 입력**

필터 규칙은 보통 아래와 같은 포맷으로 작성합니다.



```bash
['TargetAddress'] ['LocalAddress'] ['LocalPort'] ['Action'] ['Protocols']
```





 <br />



#### **[규칙 설명]**

---



**`TargetAddress`** : **한 개 이상의 대상 시스템 (ip 또는 호스트명)**

**`LocalAddress`** : **WebLogic 인스턴스가 기동중인 호스트 정보**

> \* 표시는 모든 로컬 주소를 의미함

**`LocalPort`** : **WebLogic 인스턴스가 Listening 하고 있는 로컬 포트**

> (WebLogic Admin Console에 대한 설정이면 AdminServer 포트 입력)

**`Action`** : **allow 또는 deny**

**`Protocols`** : **http, https, t3, t3s 등 프로토콜**

> 설정하지 않으면 모든 프로토콜에 해당됨

  <br />



#### **[예시]**

---



ex 1) 192.168.1.0 ~ 192.168.1.255 범위의 ip에서만 7001 포트 접근을 허용하는 설정

```bash
192.168.1.0/255.255.255.0 * 7001 allow
```

> **/255.255.255.0** 와 **/24**는 동일한 의미

  <br />

ex 2) 192.168.x.x 대역을 가진 ip에서만 8002 포트 접근을 차단하는 설정

```bash
192.168.1.0/16 * 8002 deny
```

> 여기서 **/16**은 **/255.255.0.0** 과 동일한 의미

  <br />

ex 3) 모든 ip에 대해 7001 포트로 Listen 하는 http, https 프로토콜에 대해 접근을 차단하는 설정 (`t3`, `t3s` 가능)

```bash
* * 7001 deny http https
```



  <br />

 참고 문서를 보시려면 [링크](https://docs.oracle.com/middleware/12212/wls/SCPRG/con_filtr.htm#SCPRG388)를 클릭해주세요.

 <br />

위의 설정이 모두 완료되셨으면, 저장을 누르시고 변경 내용 및 활성화 버튼을 눌러 적용해주세요.

AdminServer를 재기동하고, 순차적으로 Managed Server도 재기동하여 서비스 이상없이 잘 반영이 되었는지 확인해주시면 됩니다.



<br/>

