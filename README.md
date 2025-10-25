README
# AWS環境構築（CloudFormation初級編）

このプロジェクトは、**AWS CloudFormation**を使用して本番環境を想定したインフラ構築を自動化する学習課題です。  
RaiseTech AWSコースで学んだ内容をもとに、テンプレートからStackを作成し、アプリケーションが動作する環境を構築しました。

---

## 概要

CloudFormationテンプレートを用いて、以下のAWSリソースを自動構築しました。

- VPC（CIDR: 10.0.0.0/16）
- Public / Private Subnet（2つのAZに配置）
- Internet Gateway / Route Table
- EC2（Amazon Linux 2, t2.micro）
- RDS（MySQL 8.0.39）
- ALB（Application Load Balancer）
- Security Groups（SSH, HTTP, 8080, RDSアクセス）

構築後、EC2上でSpring Bootアプリを起動し、ALB経由でアクセスできることを確認しています。

---

## 使用技術

| カテゴリ | 使用技術 |
|-----------|-----------|
| インフラ構築 | AWS CloudFormation |
| サービス | VPC, EC2, RDS, ALB, IAM, Route53 |
| 開発環境 | VSCode / GitHub / PowerShell |
| OS・AMI | Amazon Linux 2 |
| DB | MySQL 8.0.39 |
| アプリ | Spring Boot（動作確認用） |

---

## 構成図（Architecture）

<img width="810" height="702" alt="スクリーンショット 2025-10-25 161034" src="https://github.com/user-attachments/assets/d32b09eb-2ebb-4d54-b5cd-3f685cdf6de1" />

> ※ 構成図例：  
> VPC内に2つのAZ、PublicサブネットにEC2とALB、PrivateサブネットにRDSを配置。  
> ALB → EC2 → RDSの通信をSecurityGroupで制御。

---

## デプロイ手順（Setup）

1. CloudFormationテンプレート（YAML）をAWSコンソールからアップロード  
2. 以下のパラメータを入力してスタックを作成  
   - `DBMasterUsername`  
   - `DBMasterUserPassword`  
   - `MyKeyName`（SSH接続用キーペア名）
3. スタックの作成完了後、出力されたリソース情報（VPC, EC2, ALBなど）を確認
4. EC2にSSH接続し、Spring Bootアプリを起動
5. ALBのDNS名経由でアプリにアクセス（ポート80）

---

## 動作確認

| 確認項目 | 内容 |
|-----------|------|
| EC2起動確認 | SSH接続が可能であることを確認 |
| アプリ動作確認 | EC2上でSpring Bootアプリ（8080ポート）が起動することを確認 |
| ALB経由アクセス | ALBのDNS名でアクセスし、アプリのレスポンスを確認 |
| RDS接続確認 | EC2からRDSに接続できることを確認（DB連携成功） |

---

## 学んだこと・工夫した点

- CloudFormationのテンプレート構文を正確に書く重要性を実感  
  → 1文字のミスでリソースが作成できなくなるため、YAML構文や依存関係を細かく確認
- リソース間の依存関係を整理して構築の流れを理解（例：RDSSubnetGroup → RDSInstance）
- セキュリティグループ設定の重要性を学習  
  → SSH（22番ポート）は特定IPのみ許可、RDSはEC2からのみアクセス可能に設定
- CloudFormationと実際のAWS管理コンソール操作の対応関係を把握  
  → 「Infrastructure as Code」の考え方を実践的に理解
- GitHubでのコード管理、ブランチ運用、VSCodeとPowerShellの連携操作に慣れた

---

##  今後の改善点

- テンプレート内の変数やパラメータをより汎用的に設定（例：VPC CIDRなど）  
- 出力情報（Outputs）を追加して、構築結果の確認を自動化  
- CloudFormationのRollbackやChangeSet機能の活用  
- ALBのHTTPS化対応（証明書の自動発行）

---

## リポジトリ構成

```bash
.
├── templates/
│   └── cloudformation-aws-study.yaml   # CloudFormationテンプレート
├── images/
│   └── cloudformation-architecture.png # 構成図
└── README.md
🗒️ 備考
本プロジェクトはRaiseTech AWSコース内「CloudFormation初級編」の課題です。
学習目的として構築していますが、本番構成を意識し、再現性とセキュリティを重視しました。
テストブランチ上でアプリの動作確認を完了しています。
