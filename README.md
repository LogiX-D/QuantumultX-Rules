# QuantumultX-Rules

Quantumult X 原生分流规则。

| 规则 | 用途 | 默认策略 |
| --- | --- | --- |
| [Crypto.list](Crypto.list) | 加密货币交易平台 | `Crypto` |
| [Payment.list](Payment.list) | 国际金融支付、电子钱包和跨境汇款 | `Payment` |

## Payment 订阅

直链：<https://raw.githubusercontent.com/LogiX-D/QuantumultX-Rules/main/Payment.list>

覆盖 PayPal / PayPal.Me、Venmo、Braintree / Fastlane、Stripe、Link、Wise、Revolut、Payoneer、Skrill、Neteller、Airwallex、Adyen、Checkout.com、Klarna、Square、Cash App。这里的 Link 指 **Stripe Link（link.com）**。

将以下条目合并到现有配置的对应区段；已有同名 `Payment` 策略时无需重复创建：

```ini
[policy]
static = Payment, direct, server-tag-regex=.*

[filter_remote]
https://raw.githubusercontent.com/LogiX-D/QuantumultX-Rules/main/Payment.list, tag=Payment, force-policy=Payment, update-interval=86400, opt-parser=false, enabled=true
```

`Payment` 是手动选择策略组，候选包含直连及已有节点。导入后在 Quantumult X 中为它选择所需出口。若使用现有策略组，将订阅中的 `force-policy=Payment` 改成该组的准确名称；如需全部直连，改为 `force-policy=direct` 即可，不必创建 `Payment` 组。`force-policy` 会覆盖规则文件中的策略字段，订阅每 86400 秒更新一次。

## 匹配范围

- 使用原生 `host-suffix, 域名, Payment` 格式，匹配根域名和其子域名；`m.stripe.network` 使用 `host` 精确匹配。
- 收录已核对的服务专属域名，涵盖主要网站、支付入口、API 和静态资源。域名后缀规则也可能覆盖该服务的非支付页面。
- 不使用 `pay`、`link` 等关键词，不收录 `cloudfront.net`、`amazonaws.com` 等共享 CDN 后缀，也不将整个 Apple、Google、Shopify 或商家托管域名纳入支付规则。
- 这是核心域名规则集，不保证覆盖所有地区域名、商家自定义支付域名、第三方验证码或发卡行 3-D Secure 验证页面；这些请求仍按其他规则处理。
- 如果网络活动中显示命中了其他策略，请检查本地规则和其他订阅的重叠匹配。需要调整远程规则位置时，可在 Quantumult X 中使用“插入资源”选项，再检查实际命中结果。

规则只决定网络出口。格式和匹配检查不能替代设备上的实际登录与支付验证。

## 域名来源

核对日期：2026-10-27。依据官方文档中的服务入口、域名清单和资源地址整理；后缀匹配是本规则集的归并方式，并非服务商提供的 Quantumult X 规则。

| 服务 | 收录域名 | 官方依据 |
| --- | --- | --- |
| PayPal / PayPal.Me | `paypal.com`、`paypalobjects.com`、`paypal.me` | [JavaScript SDK CSP](https://developer.paypal.com/sdk/js/v5/best-practices/)、[PayPal.Me FAQ](https://www.paypal.com/us/cshelp/article/paypalme-frequently-asked-questions-help432) |
| Venmo | `venmo.com` | [PayPal SDK CSP](https://developer.paypal.com/sdk/js/v5/best-practices/) |
| Braintree / Fastlane | `braintreegateway.com`、`braintree-api.com` | [Fastlane CSP](https://developer.paypal.com/braintree/docs/guides/fastlane/advanced-option) |
| Stripe | `stripe.com`、`stripecdn.com`、`m.stripe.network`（精确） | [Domains and IP addresses](https://docs.stripe.com/ips) |
| Link | `link.com` | [Stripe 官方网页识别说明](https://support.stripe.com/questions/verify-you-are-on-an-official-stripe-webpage) |
| Wise | `wise.com`、`transferwise.com` | [Wise 更名公告](https://wise.com/us/blog/world-meet-wise) |
| Revolut | `revolut.com` | [官方 API 入门](https://developer.revolut.com/docs/guides/manage-accounts/get-started/make-your-first-api-request) |
| Payoneer | `payoneer.com` | [官网及账户入口](https://www.payoneer.com/) |
| Skrill | `skrill.com` | [Quick Checkout Guide](https://www.skrill.com/fileadmin/content/pdf/Skrill_Quick_Checkout_Guide_v10.7.pdf) |
| Neteller | `neteller.com` | [官网及账户入口](https://www.neteller.com/en/) |
| Airwallex | `airwallex.com` | [API 入口](https://www.airwallex.com/docs/partner-api/introduction)、[支付链接](https://www.airwallex.com/docs/payments/integration-options/invoices-and-payment-links/payment-links/set-up-your-custom-domain) |
| Adyen | `adyen.com`、`adyenpayments.com` | [Allowlisting](https://docs.adyen.com/development-resources/security/integration-security/allowlisting/) |
| Checkout.com | `checkout.com` | [官方 CSP 指引](https://support.checkout.com/hc/en-us/articles/28404565761938-What-causes-Content-Security-Policy-CSP-errors-during-the-payment-authentication-step) |
| Klarna | `klarna.com`、`klarnacdn.net`、`klarnaevt.com`、`klarnaservices.com` | [Checkout CSP](https://docs.klarna.com/acquirer/klarna/web-payments/integrate-with-klarna-payments/integrate-via-sdk/step-2-checkout/)、[Web SDK 迁移](https://docs.klarna.com/acquirer/klarna/on-site-messaging/additional-resources/migration-to-the-new-klarna-websdk/) |
| Square | `squareup.com`、`squareup1.com`、`squareup2.com`、`squarecdn.com`、`sqaddons.com`、`square.link` | [官方网络清单](https://squareup.com/help/us/en/article/8348-set-up-network-requirements-for-square-hardware) |
| Cash App | `cash.app`、`cash.me` | [Square 官方网络清单](https://squareup.com/help/us/en/article/8348-set-up-network-requirements-for-square-hardware) |

Quantumult X 配置语法参考：[官方 sample.conf](https://github.com/crossutility/Quantumult-X/blob/master/sample.conf)、[官方 filter.snippet](https://github.com/crossutility/Quantumult-X/blob/master/filter.snippet)。
