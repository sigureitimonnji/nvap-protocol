# NVAP: Use Case (As-Is vs. To-Be)
## NVAP: ユースケース（現状と理想の対比）

[EN] This document illustrates the operational and security transformation achieved by NVAP in a call center environment using an As-Is (Current State) vs. To-Be (Future State with NVAP) framework.
[JP] 本ドキュメントでは、コールセンター環境におけるNVAP導入前（As-Is）と導入後（To-Be）の業務フローおよびセキュリティの変化を対比形式で定義します。

---

## 1. Outbound Call Scenario (User-initiated)
### シナリオA：ユーザーからの発信（例：金融機関のサポート窓口）

#### 🔴 As-Is (Current State) / 現状の課題
* **[EN]**
  * **User Friction**: The user must manually navigate IVR menus, wait for an operator, and then verbally recite sensitive data (name, address, date of birth) in a public space (e.g., inside a train), risking social engineering.
  * **Security Flaw**: Fraudsters can easily bypass this check using leaked personal data purchased from the dark web or by using AI-generated deepfake voices.
  * **Operational Cost**: The average handle time (AHT) increases by 30 to 45 seconds per call just to complete the manual identity verification.
* **[JP]**
  * **ユーザーの手間**: 混雑した電車内などの公衆の場であっても、暗証番号や氏名・住所・生年月日を口頭で発話させられるため、周囲への個人情報漏洩（ソーシャルエンジニアリング）のリスクに常に晒される。
  * **セキュリティの脆弱性**: ダークウェブ等で流通している個人情報の漏洩リストや、AIによって偽造されたクローン音声（ディープフェイクボイス）を使えば、第三者によるなりすまし突破が極めて容易である。
  * **運用のコスト**: オペレーターが口頭確認とシステム照合を行うためだけに、1通話あたり「30秒〜45秒」の不毛な時間が消費され、莫大なコールセンター人件費のロスが発生している。

#### 🟢 To-Be (Future State with NVAP) / 理想の解決策
* **[EN]**
  * **Frictionless UX**: The user performs Face ID and taps "Call." While moving the phone to their ear, the device completes a silent acoustic scan. The moment the operator answers, the verification is already complete.
  * **Bulletproof Security**: Even if a hacker steals the user's phone number (SIM Swap) or replicates their voice, the protocol rejects the call at the packet layer unless the physical ear canal shape matches the user's biometric token encrypted inside the RTP header.
  * **Zero Verification Cost**: Handover time to the operator is instantaneous. Manual verification time is reduced to 0 seconds.
* **[JP]**
  * **摩擦ゼロのUX**: ユーザーはFace ID（1次認証）を行って発信ボタンを押すだけ。スマホを耳に持っていく数秒の間に自動で無音スキャン（2次認証）が走るため、電話がつながった瞬間には本人確認が完全に終わっている。
  * **強固なセキュリティ**: ハッカーが電話番号を盗み出す「SIMスワップ」を仕掛けたり、本人の声を完璧に真似たAIボイスを使ったりしても、パケット層（RTPヘッダー暗号化領域）に乗っている物理的な耳の反射データが一致しない限り、システム側で自動的にアクセスを拒否する。
  * **確認コストの消滅**: 電話が繋がった時にはオペレーターの画面に「本人確認済」と自動表示されるため、確認にかかる時間が「0秒」になり、業務効率が劇的に向上する。

---

## 2. Inbound Call Scenario (Recipient-initiated)
### シナリオB：企業からの重要着信（例：クレジットカード会社からの不正利用確認）

#### 🔴 As-Is (Current State) / 現状の課題
* **[EN]**
  * **Phishing Risk**: When a bank calls a user, the user has no way to verify if the caller is a genuine bank employee or a phishing scammer asking for their personal info.
  * **Process Friction**: The user must go through the same tedious verbal verification process even though the bank initiated the call.
* **[JP]**
  * **フィッシング詐欺のリスク**: 銀行やカード会社を名乗る不審な電話がかかってきた際、ユーザー側はそれが本物の社員なのか、個人情報を盗み出そうとする詐欺師（フィッシング）なのかを区別する術がない。
  * **プロセスの不条理**: 企業側からかけてきた電話であるにもかかわらず、ユーザー側が再び氏名や生年月日を口頭で答えさせられるという不条理な手間が発生する。

#### 🟢 To-Be (Future State with NVAP) / 理想の解決策
* **[EN]**
  * **Anti-Phishing Shield**: Answering the call verifies device possession. The ear scan generates a cryptographic token sent via RTP/RTCP. If the calling party cannot decode/verify this protocol header, the smartphone triggers a scam warning to the user.
  * **Seamless Connection**: The user answers the phone and brings it to their ear. The bank instantly knows it is the legitimate cardholder, and the conversation directly starts with the core topic (e.g., "We detected a suspicious transaction...").
* **[JP]**
  * **対フィッシング防御**: 着信に応答して耳に当てるだけで、RTCP/RTPのヘッダー拡張層で双方向のプロトコル検証が行われる。もし相手がデコーダーを持たない詐欺グループだった場合、スマホ側が自動で「詐欺電話の疑い」と画面に警告を出す防衛シグナリングとして機能する。
  * **シームレスな会話開始**: 応答ボタンを押して耳に当てた瞬間には、正真正銘の本人が出たことがカード会社側に100%伝わっているため、無駄な確認をすべてスキップして「先ほど不審な決済を検知したのですが…」といきなり本題から会話をスタートできる。
