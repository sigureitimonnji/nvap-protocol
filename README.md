# NVAP: No-Voice Authentication Protocol (White Paper)

## 1. Abstract (概要)
**[EN]** NVAP is a seamless multi-factor biometric authentication protocol designed to eliminate the need for vocalizing sensitive personal information (address, name, etc.) during identity verification. By leveraging existing smartphone sensors and VoLTE infrastructure, it replaces manual voice-based verification with a secure, automated acoustic tunnel.

**[JP]** NVAPは、本人確認の際に住所や氏名などの機微な情報を発声（露出）させることなく、スマートフォンの既存機能を活用して多要素生体認証をシームレスに実現するプロトコルです。既存のVoLTE網を「認証のバイパス」として利用し、アナログな名乗りによる確認を自動化します。

## 2. Problem Statement (現状の課題)
**[EN]** Despite the arrival of high-performance devices like **iPhone 17** and **Pixel 9a**, which feature advanced 120Hz displays and powerful AI processors, our identity verification process remains stuck in the analog era. In public spaces, users are still forced to speak their address or DOB to call centers. This is a severe privacy risk and prone to social engineering.

**[JP]** **iPhone 17** や **Pixel 9a** といった、120Hzの高リフレッシュレートディスプレイや強力なAIプロセッサを搭載した最新デバイスが普及した現代においても、本人確認プロセスだけがアナログな時代に留まっている不合理を解決します。公衆の場であっても個人情報を発声せざるを得ない現状は、プライバシー侵害であり、ソーシャルエンジニアリングのリスクを高めます。

## 3. Proposed Architecture (提案構成)
This protocol requires **NO NEW HARDWARE**. It is a software-defined solution for modern smartphones.

*   **Client-Side (iOS / Android):**
    1. **1st Factor**: Device login (Fingerprint/FaceID) ensuring physical possession.
    2. **2nd Factor**: **Ear Acoustic Scanning**. Using the ear speaker to emit high-frequency pulses (18k-22kHz) and capturing the ear canal's unique reflection via the microphone.
*   **In-band Signaling (VoLTE):**
    The authentication "OK" flag is digitally encoded and overlaid onto the 300Hz-7kHz voice band as **Acoustic Watermarking (Steganography)**.

**[JP]**
このプロトコルは「新しいハードウェアを一切必要としない」、現代のスマートフォン向けに最適化されたソフトウェア・ディファインド（ソフトウェア制御）なソリューションです。

*   **クライアント側（iOS / Android）:**
    1. **第1の要素（所有物認証）**：端末へのログイン（指紋認証やFaceID）。これにより、ユーザーが物理的にその端末を所有していることを保証します。
    2. **第2の要素（生体認証）**：耳音響スキャン（Ear Acoustic Scanning）。イヤースピーカーから高周波パルス（18kHz〜22kHz）を照射し、外耳道（耳の穴）の形状によって跳ね返ってきた固有の反射音をマイクで集音・解析します。

*    **帯域内シグナリング（VoLTE通話内でのデータ伝送）**
認証成功の「OKフラグ」をデジタルデータ化し、音響電子透かし（ステガノグラフィ）の技術を用いて、VoLTEの音声帯域（300Hz〜7kHz）に重ね合わせて送信します。

## 4. Security Analysis (セキュリティ解析)
*   **No Spoofing**: Combines "Something you have" (Smartphone) with "Something you are" (Ear canal shape). Replay attacks via voice recordings are physically impossible due to real-time acoustic reflection.
*   **Infrastructure Ready**: Works on existing VoLTE/HD-Voice networks without modifying carrier hardware.

**[JP]**
*   なりすまし（スプーフィング）の防止「所有しているもの（スマートフォン）」と「その人自身（外耳道の形状）」を組み合わせた多要素認証です。リアルタイムな音響反射を利用するため、録音された音声を使ったリプレイアタック（再送攻撃）は物理的に不可能です。
*   既存インフラへの即時適応通信キャリア側のハードウェアに一切手を加えることなく、既存のVoLTEやHD-Voice（高音質通話）ネットワーク上でそのまま動作します。

## 5. Implementation & Call for Action (実装案と協力要請)
**[EN]** This is a theoretical framework. I am the architect of the communication logic, not a DSP (Digital Signal Processing) or acoustic specialist. 
**We are seeking contributors to build a PoC, specifically for:**
*   Android/iOS real-time acoustic watermarking libraries.
*   Server-side/Receiver-side decoders for VoLTE streams.

**[JP]** 本文書は理論的枠組みの提示を目的としています。私はロジックの設計者であり、信号処理の専門家ではありません。**以下の専門を持つコントリビューターによるPoC実装を熱望します：**
*   Android/iOS上でのリアルタイム音響重畳ライブラリ。
*   受信側でのVoLTEストリーム・デコーダー。

---
## Disclaimer / 免責事項
This project describes a conceptual protocol. Implementers are responsible for obtaining necessary licenses for any third-party biometric technologies mentioned. Designed by sigureitimonnji.

## License
[MIT License](LICENSE)
