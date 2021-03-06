---

copyright:
 years: 2015, 2016

---

# 启用通知
{: #c_enable_push-notifications}
上次更新时间：2016 年 8 月 16 日
{: .last-updated}

您可以让应用程序具有接收推送通知以及向您的设备发送推送通知的能力。

本部分描述了如何使您的移动应用程序能够接收推送通知，如何创建基本通知、获取和初始化 SDK 或插件，以及如何注册您的设备来接收推送通知。您还可以通过 [REST API](t_restapi.html) 使移动应用程序能够接收推送通知。

注：为设备注册推送服务时，{{site.data.keyword.mobilepushshort}} 服务会保持对通知提供者颁发的令牌的唯一引用。对于 Apple，通知提供者为 APNs；而对于 Google，则为 GCM。{{site.data.keyword.mobilepushshort}} 服务通知提供者可能会基于一些原因让令牌失效。 

例如，在设备上卸载应用程序期间。在这种情况下，如果有传递通知的尝试，通知提供者就会给出有关设备已失效的响应，{{site.data.keyword.mobilepushshort}} 服务会根据该响应来除去设备注册。这样会抑制后续的尝试，阻止将通知发送给这些已失效的设备。
