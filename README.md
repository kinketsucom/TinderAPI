# TinderAPIの使い方
TinderのAPIについて日本語のドキュメントがなかったので、わかっているものをここに書いていきます。

作者にコンタクト:@kinketsucom(twitter)

__注意__
作成日時は2017/8/31でAPIに変更があるかもしれません。気が向いたら直しますが、このドキュメントを通して行う各自の行為には責任を持ちません。また、削除要請には即座に応じます。

## 使用した使用言語
swift3です。apiを叩いてiOSアプリケーションを作成することができます。
ここではAlamofireを用いて通信を行なっています。
通信に関しては起動を確認した最低限のものです。  
```swift
import Alamofire
import SwiftyJSON
import AlamofireSwiftyJSON
```

## Tinderのapiトークンの取得
access_tokenはfacebook apiから入手するものです。
```swift
let url = "https://api.gotinder.com/v2/auth/login/facebook";
let param = ["token":access_token]
Alamofire.request(url,method:.post,parameters:param).responseSwiftyJSON { response in
    switch response.result {
    case .success(let value):
        print(value)
    case .failure(let error):
      print(error)
    }
}
```
valueの中にapi_tokenが含まれます。

## 近くの人の情報を取得
上記api_tokenを用います
```swift
let url = "https://api.gotinder.com/recs/core?locale=ja-JP"
let head = ["X-Auth-Token":api_token]
Alamofire.request(url,method:.get, headers:head).responseSwiftyJSON { response in
    switch response.result {
      case .success(let value):
        print(value)
      case .failure(let error):
        print(error)
    }
}
```
valueの中に近くのユーザー情報が含まれます。

## Likeを送信
ユーザーにLikeを送信します。このとき,  
ライクするユーザーの
+ id
+ contenthash
+ s_number

というパラメータをurlにつけてgetします。
```swift
let url = "https://api.gotinder.com/like/ここにid?content_hash=ここにcontent_hash&s_number=ここにs_number"
let head = ["X-Auth-Token":api_token]
Alamofire.request(url,method:.get,headers:head).responseSwiftyJSON { response in
    switch response.result {
    case .success(let value):
      print(value)
    case .failure(let error):
      print(error)
    }

}
```
valueは
{match:true}か{match:false}として返ってきます。もちろん前者が嬉しい方です。
