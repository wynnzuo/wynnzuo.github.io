---
title: "项目"
description: "个人项目与技术作品集"
date: 2026-07-03
draft: false
disableComments: true
---

<style>
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
  gap: 24px;
  margin: 24px 0;
}
.project-card {
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 24px;
  background: var(--entry);
  transition: transform 0.2s, box-shadow 0.2s;
}
.project-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(0,0,0,0.1);
}
.project-card h3 {
  margin-top: 0;
  margin-bottom: 8px;
  font-size: 1.15em;
}
.project-card .desc {
  color: var(--secondary);
  font-size: 0.9em;
  line-height: 1.65;
  margin-bottom: 14px;
}
.project-card .tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-bottom: 16px;
}
.project-card .tags span {
  background: var(--tertiary);
  color: var(--primary);
  padding: 2px 10px;
  border-radius: 20px;
  font-size: 0.76em;
  font-weight: 500;
}
.project-card .links {
  display: flex;
  gap: 10px;
}
.project-card .links a {
  font-size: 0.82em;
  padding: 5px 14px;
  border-radius: 6px;
  text-decoration: none;
  font-weight: 500;
  transition: opacity 0.2s;
}
.project-card .links a:hover { opacity: 0.8; }
.project-card .links .gh { background: var(--primary); color: var(--theme); }
.project-card .links .demo { border: 1px solid var(--border); color: var(--primary); }

.skill-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin: 16px 0;
}
.skill-grid span {
  padding: 4px 14px;
  border-radius: 20px;
  font-size: 0.83em;
  font-weight: 500;
}
.skill-java    { background: #e76f0018; color: #e76f00; }
.skill-spring  { background: #6db33f18; color: #6db33f; }
.skill-react   { background: #61dafb18; color: #61dafb; }
.skill-vue     { background: #4fc08d18; color: #4fc08d; }
.skill-python  { background: #3776ab18; color: #3776ab; }
.skill-docker  { background: #2496ed18; color: #2496ed; }
.skill-k8s     { background: #326ce518; color: #326ce5; }
.skill-go      { background: #00add818; color: #00add8; }
.skill-mysql   { background: #4479a118; color: #4479a1; }
.skill-redis   { background: #dc382d18; color: #dc382d; }
.skill-aws     { background: #ff990018; color: #ff9900; }
.skill-other   { background: var(--tertiary); color: var(--primary); }
.project-card .links .blog { border: 1px solid var(--border); color: var(--primary); }
</style>

## 👨‍💻 关于我

后端开发，熟悉 Java / Spring Boot 技术栈，具备分布式系统设计与微服务架构经验，对系统性能优化与高可用架构有深入实践。

<div class="skill-grid">
  <span class="skill-java">Java</span>
  <span class="skill-spring">Spring Boot</span>
  <span class="skill-spring">Spring Cloud</span>
  <span class="skill-go">Go</span>
  <span class="skill-python">Python</span>
  <span class="skill-react">React</span>
  <span class="skill-vue">Vue</span>
  <span class="skill-mysql">MySQL</span>
  <span class="skill-redis">Redis</span>
  <span class="skill-docker">Docker</span>
  <span class="skill-k8s">Kubernetes</span>
  <span class="skill-aws">AWS</span>
  <span class="skill-other">Kafka</span>
  <span class="skill-other">Elasticsearch</span>
  <span class="skill-other">GitHub Actions</span>
</div>

---

## 📂 项目展示

<div class="projects-grid">

<div class="project-card">
<h3>分布式电商平台</h3>
<div class="desc">基于微服务架构的全链路电商平台，包含商品、订单、支付、库存、用户等核心服务。采用 Spring Cloud Alibaba 生态，集成 Sentinel 限流降级、Seata 分布式事务、Nacos 配置中心。</div>
<div class="tags"><span>Java</span><span>Spring Cloud</span><span>Redis</span><span>RocketMQ</span><span>MySQL</span><span>Docker</span></div>
<div class="links"><a class="gh" href="https://github.com/wynnzuo" target="_blank">GitHub</a><a class="demo" href="#" target="_blank">在线演示</a></div>
</div>

<div class="project-card">
<h3>博客系统 (Hugo + PaperMod)</h3>
<div class="desc">基于 Hugo 静态站点生成器与 PaperMod 主题构建，集成 Giscus 评论系统、全文搜索、ASCII 关系图。GitHub Actions 自动构建部署到 Pages。包含 23 篇设计模式系列文章。</div>
<div class="tags"><span>Hugo</span><span>PaperMod</span><span>Giscus</span><span>GitHub Actions</span><span>GitHub Pages</span></div>
<div class="links"><a class="gh" href="https://github.com/wynnzuo/wynnzuo.github.io" target="_blank">GitHub</a><a class="demo" href="https://wynnzuo.github.io" target="_blank">在线演示</a></div>
</div>

<div class="project-card">
<h3>API 网关 & 认证中心</h3>
<div class="desc">基于 Spring Cloud Gateway + OAuth2 的统一 API 网关与认证授权中心。支持 JWT 令牌、RBAC 权限模型、接口限流、请求日志审计、灰度发布等功能。</div>
<div class="tags"><span>Java</span><span>Spring Gateway</span><span>OAuth2</span><span>Redis</span><span>JWT</span><span>Docker</span></div>
<div class="links"><a class="gh" href="https://github.com/wynnzuo" target="_blank">GitHub</a></div>
</div>

<div class="project-card">
<h3>数据可视化看板</h3>
<div class="desc">通用数据可视化平台，支持多数据源接入（MySQL、Elasticsearch）与自定义图表配置。后端使用 Python FastAPI 提供数据接口，前端 React + ECharts 渲染交互式仪表盘。</div>
<div class="tags"><span>Python</span><span>FastAPI</span><span>React</span><span>ECharts</span><span>Elasticsearch</span></div>
<div class="links"><a class="gh" href="https://github.com/wynnzuo" target="_blank">GitHub</a></div>
</div>

<div class="project-card">
<h3>消息推送平台</h3>
<div class="desc">多渠道消息推送中台，统一管理短信、邮件、App Push、微信模板消息等渠道。支持模板管理、发送审核、失败重试、全链路追踪与渠道降级。</div>
<div class="tags"><span>Java</span><span>Spring Boot</span><span>Kafka</span><span>Redis</span><span>MySQL</span></div>
<div class="links"><a class="gh" href="https://github.com/wynnzuo" target="_blank">GitHub</a><a class="demo" href="#" target="_blank">在线演示</a></div>
</div>

<div class="project-card">
<h3>Kubernetes 运维工具</h3>
<div class="desc">K8s 集群日常运维命令行工具，封装常用 kubectl 操作，支持多集群切换、Pod 日志实时查看、资源监控、批量更新与回滚。Go 编写，单二进制分发。</div>
<div class="tags"><span>Go</span><span>Kubernetes</span><span>Docker</span><span>Prometheus</span></div>
<div class="links"><a class="gh" href="https://github.com/wynnzuo" target="_blank">GitHub</a></div>
</div>

</div>

---

## 📬 联系我

- **GitHub**: [github.com/wynnzuo](https://github.com/wynnzuo)
- **Email**: wynnzuo@163.com
- **博客**: [wynnzuo.github.io](https://wynnzuo.github.io)
