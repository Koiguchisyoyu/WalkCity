# Walk City

歩いた量に応じて街が育つ、ウォーキング継続支援Webアプリです。

歩数をコインへ変換し、建物や道路を配置して自分の街を発展させます。運動の成果をゲーム内の変化として可視化することで、日々のウォーキングを楽しみながら続けられる体験を目指しました。

- 公開デモ: [https://walk-city.vercel.app](https://walk-city.vercel.app)
- 開発時期: 2026年8月
- 開発形式: Progateハッカソンでのチーム開発

> [!NOTE]
> 本リポジトリはチーム開発の成果物です。以下の技術スタックはプロジェクト全体で使用したものであり、個人の担当範囲は「個人の担当範囲」に分けて記載しています。

## 解決したい課題

ウォーキングは成果を実感しにくく、習慣化する前にやめてしまうことがあります。そこで、歩数を街の成長へ結び付け、運動の積み重ねを目に見える形で残せるようにしました。

主な対象は、運動不足を感じているものの、本格的な運動にはまだ踏み出せていない若年層です。

## 主な機能

- Google Healthと連携した歩数の取得
- 歩数に応じたコインの獲得
- 建物・道路・川・橋の配置と移動
- 建物の効果による人口計算
- 人口ランキング
- Googleアカウントによるログイン
- PC・スマートフォン対応

## 技術スタック

### フロントエンド

- TypeScript
- React
- Vite
- Tailwind CSS
- Vitest / Testing Library

### バックエンド・インフラ

- Supabase Database
- Supabase Auth
- Supabase Edge Functions
- PostgreSQL / Row Level Security
- Google Health API
- Vercel

Supabaseにはユーザー、街、建物などのデータを保存し、Row Level Securityでユーザーごとのアクセスを制御しています。Edge FunctionsはGoogle Health APIとの連携と歩数データの反映に利用しています。

## アーキテクチャ

![Walk Cityのアーキテクチャ図](https://ptera-publish.topaz.dev/project/01M187RHJFY562XDP86F4DHNPA.png)

フロントエンドではAPIの型とインターフェースを先に定義し、モック実装とSupabase実装を`ApiProvider`で切り替えられる構成にしました。これにより、バックエンドの完成を待たずに画面開発とテストを進められます。

## チーム開発で工夫したこと

開発途中で、フロントエンドとバックエンドの仕様書に食い違いがあり、接続に時間がかかる問題が発生しました。そこで、接続部分の担当を明確にし、フロントエンド側の型・インターフェースを共通の契約として仕様を整理しました。

また、機能ごとにブランチとPull Requestを作成し、変更内容、確認方法、テスト結果を記録してから統合しました。

## 個人の担当範囲

GitHubユーザー [`Koiguchisyoyu`](https://github.com/Koiguchisyoyu) は、主に次を担当しました。

- マップ機能の仕様整理
- マップ用の型、モックデータ、配置・移動判定の実装
- 建物の衝突判定と道路隣接判定
- タウンマップ中心のダッシュボード
- マーケット、建物配置・移動、川・橋などのゲーム機能
- PC・スマートフォン向け画面の調整
- 複数の設計書に存在したAPI仕様の矛盾整理
- GitHubのブランチ・Pull Requestを使った変更管理

TypeScriptの実装では生成AIによるコーディング支援を利用し、機能単位でPull Requestを作成しました。Reactの基盤構築とSupabaseバックエンドは、他のチームメンバーが主に担当しています。

### 代表的なPull Request

- [#3 Map基盤の型・モック・配置判定を追加](https://github.com/Megane14916/walk-city/pull/3)
- [#5 タウンマップ常時表示ダッシュボードを追加](https://github.com/Megane14916/walk-city/pull/5)
- [#7 マーケットの購入・配置機能を追加](https://github.com/Megane14916/walk-city/pull/7)
- [#13 川と橋のマップ機能を追加](https://github.com/Megane14916/walk-city/pull/13)
- [#17 API設計書間の矛盾14件を解消](https://github.com/Megane14916/walk-city/pull/17)
- [#23 スマートフォン向けヘッダーとマップ操作を改善](https://github.com/Megane14916/walk-city/pull/23)
- [#26 設定ダイアログにログアウト機能を追加](https://github.com/Megane14916/walk-city/pull/26)

## ローカルでの実行

Supabaseへ接続しなくても、モックデータを使ってフロントエンドを起動できます。

```sh
git clone https://github.com/Koiguchisyoyu/WalkCity.git
cd WalkCity/frontend
npm install
cp .env.example .env.local
npm run dev
```

Windows PowerShellでは、`cp`の代わりに次を実行してください。

```powershell
Copy-Item .env.example .env.local
```

`.env.local`の`VITE_API_MODE`を`mock`にすると、Supabaseの認証情報なしで動作を確認できます。

## テストと品質確認

```sh
cd frontend
npm test
npm run lint
npm run build
```

本番接続に関する変更では、テスト、Lint、本番ビルドがすべて成功することを確認してから統合します。

## ディレクトリ構成

```text
WalkCity/
├── docs/       # 設計書、API仕様、実装計画
├── frontend/   # React・TypeScriptフロントエンド
└── supabase/   # Edge Functions、migration、DBテスト
```

より詳しいフロントエンドの構成と環境変数については、[frontend/README.md](frontend/README.md)を参照してください。
