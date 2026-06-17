# NVAP: No-Voice Authentication Protocol (White Paper)

## 1. Abstract (概要)

**[EN]** NVAP is a seamless, multi-factor biometric authentication protocol designed to eliminate the need for vocalizing sensitive personal information (such as names and addresses) during identity verification. By leveraging existing smartphone sensors and VoLTE infrastructure, it replaces manual, voice-based verification with a secure, automated, and completely silent network-layer signaling flow.

**[JP]** NVAPは、本人確認の際に住所や氏名などの機微な情報を発声（露出）させることなく、スマートフォンの既存機能を活用して多要素生体認証をシームレスに実現するプロトコルです。既存のVoLTE網を「認証のバイパス」として利用し、アナログな名乗りによる確認を自動化します。

## 2. Problem Statement (現状の課題)

**[EN]** Despite the widespread adoption of high-performance devices like the iPhone 17 and Pixel 9a—which feature advanced 120Hz displays and powerful AI processors—identity verification processes remain stuck in the analog era. In public spaces, users are still forced to speak their address or date of birth to call centers. This presents a severe privacy risk and leaves users vulnerable to social engineering.

**[JP]** iPhone 17やPixel 9aといった、120Hzの高リフレッシュレートディスプレイや強力なAIプロセッサを搭載した最新デバイスが普及した現代においても、本人確認プロセスだけがアナログな時代に留まっている不合理を解決します。公衆の場であっても個人情報を発声せざるを得ない現状は、プライバシー侵害であり、ソーシャルエンジニアリングのリスクを高めます。

## 3. Proposed Architecture (提案構成)

This protocol requires **NO NEW HARDWARE**. It is a software-defined solution optimized for modern smartphones.

- **Client-Side (iOS / Android) / クライアント側:**
  1. **1st Factor (Possession) / 第1の要素（所有物認証）**: Device unlock (Fingerprint/Face ID or the act of answering an inbound call) to ensure physical possession of the verified hardware.
  2. **2nd Factor (Biometric) / 第2の要素（生体認証）**: **Ear Acoustic Scanning**. While the user is moving the phone toward their ear, the device's ear speaker emits high-frequency probing pulses (18 kHz–22 kHz), and the microphone captures the ear canal's unique acoustic reflection. Feature extraction and biometric matching are completed entirely locally on the device.

- **Network-Layer Signaling (VoLTE) / ネットワーク層シグナリング:**
  To guarantee deterministic transmission and bypass audio-level constraints (such as voice compression codecs or SRTP encryption), NVAP utilizes a hybrid control/data plane architecture via RTP/RTCP header extensions:
  1. **RTCP Pre-Notification (Control Plane)**: Sends an "NVAP-Flag" via RTCP packet extensions immediately upon call connection to alert the receiver's infrastructure.
  2. **RTP Header Extension (Data Plane)**: Embeds the encrypted "Auth-Token" within the RTP Header Extension (per RFC 8285) of the initial media streaming packets, enabling a truly silent, codec-independent verification.

**[JP]** このプロトコルは「新しいハードウェアを一切必要としない」、現代のスマートフォン向けに最適化されたソフトウェア・ディファインドなソリューションです。

- **デバイス内部処理:**
  1. **第1の要素（所有物認証）**：手動による端末ロック解除（指紋/Face ID）、または着信時に「応答ボタンを押す」という行為そのものにより、対象の通信端末を物理的に所有していることを保証します。
  2. **第2の要素（生体認証）**：耳音響スキャン（Ear Acoustic Scanning）。応答ボタンを押してからスマホを耳に持っていく間の「空中にあるわずかな数秒間」を利用し、イヤースピーカーから高周波パルス（18kHz〜22kHz）を照射。外耳道からの固有の反射音をマイクで集音・解析し、端末ローカル内で超高速に認証判定（Auth Result）を完了させます。
- **ネットワーク層でのデータ伝送（RTP/RTCPハイブリッド方式）:**
  音声圧縮コーデックによるデータの破損や、音声暗号化（SRTP）の干渉を完全に回避するため、音声データ本体（ペイロード）ではなく、ネットワークプロトコルのヘッダー領域を活用します。
  1. **RTCPによる事前通知（制御プレーン）**：回線が接続された瞬間、RTCPパケットの拡張領域を用いて「NVAP認証情報あり」のメタフラグを相手先に通知し、受け入れ準備をさせます。
  2. **RTPヘッダー拡張による結果伝送（データプレーン）**：実際の音声ストリームであるRTPパケットのヘッダー拡張領域（RFC 8285等）に暗号化した認証トークンを乗せてダイレクトに伝送し、完全な無音認証を完結させます。

## 4. Security Analysis (セキュリティ解析)

- **[EN]**
  - **Spoofing Prevention**: Combines "something you have" (smartphone hardware/SIM) with "something you are" (ear canal shape). Replay attacks using voice recordings are physically impossible due to the real-time nature of the acoustic reflection.
  - **Encryption & Codec Immune**: By operating entirely at the RTP/RTCP protocol header layer, the authentication data remains unaffected by cellular voice compression codecs or media-level encryption (SRTP).
- **[JP]**
  - **なりすまし（スプーフィング）の防止**：「所有しているもの（スマートフォン）」と「その人自身（外耳道の形状）」を組み合わせた多要素認証です。リアルタイムな音響反射を利用するため、録音された音声を使ったリプレイアタック（再送攻撃）は物理的に不可能です。
  - **暗号化・コーデックへの耐性**：RTP/RTCPのプロトコルヘッダー層で動作するため、通信キャリアの音声圧縮コーデックによる信号の減衰や消失、通話の暗号化（SRTP）による影響を一切受けることなく、100%確実に相手先システムへデータを届けられます。

## 5. Implementation & Call for Action (実装案と協力要請)

**[EN]** This document outlines a theoretical framework. As the architect of the communication logic, I am seeking contributors and research institutions with expertise in DSP (Digital Signal Processing), network protocol development, or ear acoustic biometrics to help build a Proof of Concept (PoC), specifically for:
- Integration of existing ear acoustic authentication engines into the NVAP pre-call timeline.
- iOS/Android client-side implementations for embedding custom data into RTP/RTCP header extensions.
- Receiver-side/Call center infrastructure decoders for parsing custom RTP extension fields.

**[JP]** 本文書は理論的枠組みの提示を目的としています。私はロジックの設計者であり、個別の生体認証アルゴリズムや信号処理の専門家ではありません。この画期的な通信シグナリングプロトコルの概念実証（PoC）に向けて、以下の専門性を持つコントリビューターや研究機関（耳音響認証の最先端チームなど）の協力を熱望します：
- 既存の耳音響認証エンジンを、NVAPの「耳に当てる前の空中タイムライン」へ統合するアプリ実装。
- iOS/Androidにおける、RTP/RTCPヘッダー拡張領域へのカスタムデータ埋め込み処理の実装。
- 受信側（コールセンター等）のインフラにおける、カスタムRTP拡張フィールドの解析（Parse）システム。

## Disclaimer / 免責事項

This project describes a conceptual protocol. Implementers are responsible for obtaining necessary licenses for any third-party biometric technologies mentioned. Designed by sigureitimonnji.

## License

MIT License
