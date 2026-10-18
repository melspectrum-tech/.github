# 安全策略 · Security Policy

## 报告漏洞

如果你在我们的产品、固件、App、网站或网络（AS218883）中发现了安全问题，请**不要**开公开 issue，而是发邮件给 **contact@melspectrum.com**，标题以 `[SECURITY]` 开头。

邮件里请尽量写清楚：

- 受影响的产品、仓库、版本或固件版本
- 复现步骤或 PoC
- 你判断的影响范围

我们会在 3 个工作日内确认收到。修复之前，请不要公开细节。

If you find a security issue in our products, firmware, apps, websites or network (AS218883), please email **contact@melspectrum.com** with a subject starting with `[SECURITY]` instead of opening a public issue. We aim to acknowledge reports within 3 business days.

## 内部规则（成员必读）

- **密钥泄露**：不管泄露到了哪里（git 历史、聊天、截图、日志），都要先轮换，再通知负责人，最后再考虑清理痕迹。不要因为“仓库是私有的”就不处理。
- **设备安全**：BLE 配对、OTA 签名、设备鉴权相关的改动，必须有第二个人 review，并且在 PR 里说明威胁模型有什么变化。
- **网络基础设施**（`infra` team 的仓库）：改动路由、防火墙或 WireGuard 配置前，先确认有回滚路径和带外入口，之后再上线。
- **依赖**：引入新的第三方依赖时，在 PR 里说明来源和用途。不要从不可信的来源下载二进制文件。
