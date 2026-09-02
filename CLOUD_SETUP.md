# 端末間同期・Gmail返信連携の設定

この機能はSupabase無料プランとGoogle Apps Scriptを使います。秘密鍵や管理者メールは公開GitHubへ保存しません。

## 1. Supabaseを作成

1. Supabaseで無料プロジェクトを作成します。
2. SQL Editorを開き、`supabase/schema.sql`の内容を実行します。
3. Authentication → URL Configurationで、公開したMeeting CanvasのURLをSite URLとRedirect URLへ登録します。
4. Project URLとPublishable keyを`src/config.js`へ設定します。

Publishable keyはWebアプリで公開することを前提としたキーです。Secret keyは`src/config.js`へ入れないでください。データ保護はRow Level Securityが担当します。

## 2. Google Apps Scriptを作成

1. 新しいApps Scriptプロジェクトを作り、`apps-script/Code.gs`を貼り付けます。
2. プロジェクトの設定 → スクリプトプロパティへ次を追加します。
   - `SUPABASE_URL`: Supabase Project URL
   - `SUPABASE_SECRET_KEY`: Supabase Secret key
   - `ADMIN_EMAIL`: 問い合わせを受け取る自分のGmail
3. Apps Script上で`setup`関数を1回実行し、Gmail・外部接続の権限を許可します。
4. 5分ごとの自動同期トリガーが作成されます。

## 3. Gmailで返信

新規問い合わせは件名 `[Meeting Canvas #XXXXXXXX]` で届きます。そのメールへ通常どおり返信してください。最大約5分後に返信内容がユーザーのアプリへ表示されます。件名のチケット番号は変更しないでください。

## プライバシー

- 録画・録音ファイルはSupabaseへ保存しません。
- 同期するのは会議名、文字起こし、議事録、問い合わせです。
- 各ユーザーはログインした自分のデータだけを閲覧できます。
- Supabase Secret keyはGitHub、ブラウザコード、チャットへ貼らないでください。
