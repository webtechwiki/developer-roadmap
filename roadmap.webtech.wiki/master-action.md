# master 分支action的工作流

## aws-costs.yml

这个YAML文件定义了一个GitHub Actions工作流，主要功能是每天自动获取AWS云服务费用并发送到Slack频道。具体作用如下：

1. **工作流名称**：`Sends Daily AWS Costs to Slack`（每天发送AWS费用到Slack）

2. **触发条件**：
   - 可以手动触发（`workflow_dispatch`）
   - 每天UTC时间7:00自动运行（`cron: "0 7 * * *"`）

3. **执行任务**：
   - 使用`ubuntu-latest`运行环境
   - 安装`aws-cost-cli`工具
   - 调用该工具获取AWS费用数据并发送到Slack

4. **所需配置**：
   - AWS访问凭证（ACCESS_KEY/SECRET_KEY/REGION）
   - Slack频道和Token
   - 这些敏感信息都存储在GitHub Secrets中

5. **实际用途**：
   - 为团队提供每日AWS费用监控
   - 帮助控制云服务成本
   - 通过Slack通知方便团队随时查看

这是一个典型的云成本监控自动化方案，适合使用AWS云服务并需要定期关注费用的团队。

## close-feedback.yml

这个GitHub Action的作用是自动关闭带有"feedback left"标签且7天内没有更新的Pull Request。具体功能包括：

1. 触发方式：
   - 可以手动触发(workflow_dispatch)
   - 每天0点自动执行(cron定时任务)

2. 执行逻辑：
   - 扫描仓库中所有基于master分支的开放PR
   - 检查PR是否带有"feedback left"标签
   - 如果PR7天内没有更新(updated_at时间早于7天前)
     - 在PR中添加关闭评论
     - 将PR状态改为closed

3. 使用场景：
   - 用于清理需要反馈但长期无响应的PR
   - 保持PR列表整洁，避免积压
   - 自动通知贡献者PR被关闭的原因

4. 技术实现：
   - 使用GitHub官方actions/github-script@v3
   - 通过GitHub API操作PR和issue
   - 使用GITHUB_TOKEN进行认证

## cloudfront-api-cache.yml

这个 GitHub Action 的作用是清除 CloudFront API 缓存。具体功能包括：

1. 通过手动触发 (workflow_dispatch) 执行
2. 发送 HTTP 请求到 GitHub API 触发另一个仓库的 Ansible 工作流
3. 使用 curl 命令执行 POST 请求，需要 GH_PAT 令牌认证
4. 目标是执行 roadmap_web.yml playbook 中的 cloudfront-api 标签任务
5. 主要用途是在 API 内容更新后立即刷新 CloudFront 缓存

