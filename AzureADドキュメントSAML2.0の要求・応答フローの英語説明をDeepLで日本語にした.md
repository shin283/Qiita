# はじめに
AzureADを使用してシングルサインオン(SSO)をする要件があり、
その時に調べた、認証方式のSAML2.0の認証要求と応答についてのまとめ。

Azureドキュメントに、認証要求と応答のフローがあったのでそれで学習しました。
[シングル サインオンの SAML プロトコル](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/single-sign-on-saml-protocol)

フローの説明が英語でしたので、日本語にしたく、話題のDeepLを使用して日本語訳にしました。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/364aa432-e6c1-330a-0408-2b8d721a67bf.png)



# 英語・日本語対応表
| No | 英語 | 日本語 |
|:---|:---|:---|
| 1 | User tries to access the application | ユーザーがアプリケーションにアクセスしようとする |
| 2 | Application finds the identity provider to authenticate the user | アプリケーションは、ユーザーを認証するためのIDプロバイダを見つけます |
| 3 | Application generates a SAML2.0 AuthnRequest  and redirects the user’s browser to the Azure AD SAML single sign-on URL | アプリケーションは、SAML2.0のAuthnRequestを生成し、ユーザーのブラウザをAzure ADのSAMLシングルサインオンURLにリダイレクトする |
| 4 | If the user is not signed in, Azure AD authenticates the user and generates a SAML token | ユーザーがサインインしていない場合、Azure AD はユーザーを認証し、SAML トークンを生成します |
| 5 | Azure AD posts the SAML response to the application via the user’s browser | Azure AD は、ユーザーのブラウザを介してアプリケーションに SAML レスポンスを投稿します |
| 6 | Application verifies the SAML Response | アプリケーションがSAML レスポンスを検証する |
| 7 | Application completes user sign-in | アプリケーションはユーザーのサインインを完了します |

# 作成した図
![AzureAD.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/57805e53-452d-d30a-9966-7389a01d422e.png)


# 参考
* [シングル サインオンの SAML プロトコル](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/single-sign-on-saml-protocol)
* [DeepL翻訳](https://www.deepl.com/translator)
* 
