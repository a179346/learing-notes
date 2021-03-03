# 問題
在Lambda，之前都使用 "手動更改程式" 或 "手動上傳程式碼" 來更新程式。
<br/>
常導致不知道程式現在版本，或哪段程式現在是否有加到prod、dev環境。

# 解決方法
加入自動部屬，讓Lambda上的程式進度可以與git上的branch相對應。
<br/>
當有branch push到bitbucket上，自動執行部屬。達到Lambda與git同步。

# 步驟
## 1. 需準備好 ***BitBucket Repository*** 及 ***Jenkins server***
## 2. Jenkins安裝 Bitbucket Plugin
[Bitbucket Plugin](https://plugins.jenkins.io/bitbucket/)
## 3. 在Jenkins新增作業，選擇Free style
![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/jenkins-0.PNG)
## 4. 設定Jenkins組態
#### (1) 原始碼管理
![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/jenkins-1.PNG)
![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/jenkins-2.PNG)
#### (2) 建置觸發程序
![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/jenkins-3.PNG)
#### (3) 建置環境
![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/jenkins-4.PNG)
#### (4) 建置
![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/jenkins-5.PNG)

> 備註:
> 
> 此處用到的 "npm run deploy" ，會抓取現在的branch，利用 aws-sdk 將程式deploy到對應的Lambda上。

![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/aws-sdk-1.PNG)

[aws-sdk 參考文件](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/lambda.html#updatefunctioncode)

## 5. BitBucket設定
#### (1) 建立Jenkins API Token (用於BitBucket Webhook)
![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/jenkins-token.PNG)

#### (2) 至Bitbucket專案設定 "Add webhook"
![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/bitbucket-1.png)


![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/bitbucket-2.png)

> 備註:
> 
> webhook url: 
> 
> http://[jenkins 帳號]:[jenkins api token]@[jenkins url]/git/notifyCommit?url=[bitbucket ssh]
>
> * (1) 建立該 Jenkins api token 的 Jenkins User ID
> * (2) 剛剛在 jenkins 建立的 api token
> * (3) jenkins server url (後面可加port) EX. "1.2.3.4:8080"
> * (4) bitbucket ssh EX. "git@bitbucket.org:????.git"

## 6. 測試 CI/CD

#### (1) PUSH程式到 bitbucket repository 的該branch

#### (2) 檢查 Jenkins 是否自動觸發建置
![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/test-1.PNG)

#### (3) 檢查建置過程是否正常運作
![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/test-2.jpg)

![image](https://github.com/a179346/learing-notes/blob/main/files/ci_cd_with_jenkins_bitbucket/test-3.PNG)

若印出來的log一切正常，表示建置成功。