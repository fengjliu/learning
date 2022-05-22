# learning

to access github through token

1. generate token

Why: 为什么要把密码换成token

下面是 Github 官方的解释： 近年来，GitHub 客户受益于 http://GitHub.com 的许多安全增强功能，例如双因素身份验证、登录警报、经过验证的设备、防止使用泄露密码和 WebAuthn 支持。 这些功能使攻击者更难获取在多个网站上重复使用的密码并使用它来尝试访问您的 GitHub 帐户。 尽管有这些改进，但由于历史原因，未启用双因素身份验证的客户仍能够仅使用其 GitHub 用户名和密码继续对 Git 和 API 操作进行身份验证。

从 2021 年 8 月 13 日开始，我们将在对 Git 操作进行身份验证时不再接受帐户密码，并将要求使用基于令牌（token）的身份验证，例如个人访问令牌（针对开发人员）或 OAuth 或 GitHub 应用程序安装令牌（针对集成商） http://GitHub.com 上所有经过身份验证的 Git 操作。 您也可以继续在您喜欢的地方使用 SSH 密钥（如果你要使用 ssh 密钥可以参考）。

修改为 token 的好处：

令牌（token）与基于密码的身份验证相比，令牌提供了许多安全优势： - 唯一： 令牌特定于 GitHub，可以按使用或按设备生成 - 可撤销：可以随时单独撤销令牌，而无需更新未受影响的凭据 - 有限 ： 令牌可以缩小范围以仅允许用例所需的访问 - 随机：令牌不需要记住或定期输入的更简单密码可能会受到的字典类型或蛮力尝试的影响

How:
打开 Github，在个人设置页面，找到【Setting】，然后打开找到【Devloper Settting】，如下图
然后，选择个人访问令牌【Personal access tokens】，然后选中生成令牌【Generate new token】
在上个步骤中，选择要授予此令牌 token 的范围或权限。

要使用 token 从命令行访问仓库，请选择 repo
要使用 token 从命令行删除仓库，请选择 delete_repo
其他根据需要进行勾选

生成 token 后，记得把你的 token 保存下来，以便进行后面的操作。把 token 直接添加远程仓库链接中，这样就可以避免同一个仓库每次提交代码都要输入 token 了。

<your_token>：换成你自己得到的 token : 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
<USERNAME>：是你自己 github 的用户名  
<REPO>：是你的仓库名称
https://github.com/fengjliu/learning.git
git remote set-url origin https://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@github.com/fengjliu/learning.git

https://github.com/fengjliu/learning.git

2. execute below command

https://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@github.com/fengjliu/learning.git
