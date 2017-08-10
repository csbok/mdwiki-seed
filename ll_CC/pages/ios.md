# Facebook 연동

https://developers.facebook.com/docs/ios/getting-started
https://developers.facebook.com/docs/facebook-login/ios
http://blog.naver.com/PostView.nhn?blogId=dla210&logNo=220718893889

위의 문서들을 보고 차근 차근 진행하면 된다.
주의해야할 점은 문서에선 
FBSDKCoreKit.framework
FBSDKLoginKit.framework
FBSDKShareKit.framework
위의 3개의 파일만 Framework에 포함하면 된다고 나오지만,
Apple Mach-O Linker (Id) Error 와 같은 링크 에러를 내게 된다.
문서에는 없지만, Bolts.framework도 포함시켜줘야 에러가 나지 않는다.
