
# はじめに
オンプレでActive Directory（以下AD）、クラウドでAzure Active Directory（以下Azure AD）を使っている。
そこの課題として、

* ADとAzure ADでUPNが異なっている
* Azure ADが使い慣れていないUPN（メールアドレスではない）

そのため、ユーザもオンプレでログインする時と、SaaS利用でログインする時で別々のアカウントを使っていて使い勝手が悪い。

本来、どのようにした方が良いのか、ベストプラクティスとしてはどのようにした方が良いのか？


# Azureドキュメントを見ると
> アプリケーションで現在 UPN を使用している場合は、エクスペリエンスを向上させるため、ユーザーのプライマリ電子メール アドレスと一致するように UPN を設定することをお勧めします。
ハイブリッド環境では、ユーザーの UPN がオンプレミスのディレクトリと Azure Active Directory で同一であることが重要です。

[Azure Active Directory でのユーザー プリンシパル名の変更の計画とトラブルシューティング](https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/howto-troubleshoot-upn-changes)



基本的には、

* UPNは同一
* UPNはメールアドレスと一致

が基本戦略のよう。
一括変更したい時はどうするか？


# 一括変更したいときは

> UPN の一括変更をロールアウトする
> UPN の一括変更については、パイロットのベスト プラクティスに従ってください。 また、すぐに解決できない問題が見つかった場合に UPN を元に戻すため、テスト済みのロールバック計画も用意します。 パイロットを実行したら、さまざまな組織の役割とアプリやデバイスの特定のセットが含まれる少数のユーザーのセットを対象にすることができます。

[UPN の一括変更をロールアウトする](https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/howto-troubleshoot-upn-changes#roll-out-bulk-upn-changes)

一括で変更できるよう。
ただし、いきなりじゃなくて徐々に。

ということで、
方法は、

* [パイロットのベスト プラクティス](https://docs.microsoft.com/ja-jp/azure/active-directory/fundamentals/active-directory-deployment-plans#best-practices-for-a-pilot)
* 動的グループ メンバーシップ ルールで簡単にパイロットグループ作成
 * [Azure Active Directory の動的グループ メンバーシップ ルール](https://docs.microsoft.com/ja-jp/azure/active-directory/users-groups-roles/groups-dynamic-membership)

で徐々に移行すればOKということがわかった。
