# 术语篇

## `XSRF`

> XSRF（Cross-Site Request Forgery 跨站请求伪造）是一种网络攻击方式,它利用了网站对用户的信任关系来实现攻击,它会欺骗用户浏览器,让浏览器以用户的名义向网站发送非法请求,从而攻击依赖用户浏览器 Cookie 进行身份验证的网站。
>
> - `Cross-Site:`跨站点,表示攻击行为发生在不同的网站
> - `Request:`请求,表示攻击者是通过构造请求进行攻击。
> - `Forgery`伪造,表示这些请求是攻击者伪造的。
>
> XSRF 的攻击原理是:攻击者通过某种手段(例如通过社会工程学的方式)让受害者点击了一个含有攻击代码的链接,这个链接会向受信任的网站自动提交请求。由于浏览器会自动带上用户登录的 cookie,所以受信任网站会以为这个请求是用户自己发出的。于是,攻击者就可借助用户的身份,在受信任网站上完成非法操作.
>
> 具体来说,XSRF 攻击一般包括以下几个步骤:
>
> 1. 攻击者通过社工或其他手段,诱使受害者点击含有攻击代码的链接。
> 2. 受害者在不知情的情况下,访问了攻击者制作的带有恶意代码的网站。该恶意代码会向受信任网站自动提交请求。
> 3. 由于受害者已经登录了受信任网站,所以浏览器会自动在请求中带上对应的 cookie
> 4. 受信任网站在收到这个自动请求后,会误认为是用户发出的正常操作,于是执行了攻击者指定的操作。
> 5. 攻击者达到了利用受信任网站来进行未经授权操作的目的。
>
> 防范 XSRF 攻击的方法包括:在请求中添加 `token` 并校验、 `referer` 校验、添加验证码等。总之就是要求在用户敏感操作时,不能仅依据 cookie 进行认证,还需要附加参数进行多重校验。

token 校验

> token 校验是防范跨站请求伪造(XSRF)攻击的一个重要技术。
>
> 它的工作原理是:
>
> 1. 服务器向用户浏览器颁发一个随机 token,并设置到 cookie 中。
> 2. 当用户在网站上进行敏感操作时,需要将这个 token 值作为参数提交给服务器。 3.服务器收到请求后,会校验请求的参数中的 token 值和 cookie 中的 token 值是否一致。
> 3. 如果 token 值不一致,则判定为 XSRF 攻击。
>
> 通过这种手段,服务器能校验该请求是否来自于持有真实 token 的用户浏览器,从而防止请求被伪造。
>
> 与 Referer 校验相比,token 校验更可靠,因为 token 存储在服务器端,不能被恶意网站获取和伪造。这可以有效防止 XSRF 攻击。
>
> 但 token 也有可能被窃取,所以通常 token 校验会与其他技术如验证码结合使用,来实现多层防御,更充分防范 XSRF。

referer 校验

> referer 校验是一种常用的防范跨站请求伪造(XSRF)的技术。
>
> Referer 是 HTTP 头中的一个字段,当浏览器发送请求时,Referer 会包含用户来源页面的 URL。
>
> Referer 校验就是服务器端对这个 Referer 头进行校验,以判断请求是否来自网站本身。
>
> 其工作原理是:
>
> 1. 用户登录网站 A,获取 cookie。
> 2. 用户访问网站 B,网站 B 中的图片链接指向网站 A,并携带攻击指令。
> 3. 浏览器自动为网站 A 请求添加 cookie,以访问图片。
> 4. 网站 A 收到请求,检查 Referer 值为网站 B,而非自己,判定为 XSRF 攻击,拒绝请求。
>
> 这样攻击者由于无法伪造 Referer,就无法绕过 Referer 校验,实现 XSRF 攻击。
>
> 需要注意的是 Referer 也可以被伪造,所以不能完全依赖 Referer 校验来防御 XSRF,通常需要添加 token 或者验证码等多重防护措施。但 Referer 校验可以有效抵御大部分 XSRF 攻击。
