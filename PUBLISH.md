# 发布步骤

当前目录就是待发布内容（已在 README / llms.txt / prompt.md 里写好
`https://cdn.jsdelivr.net/gh/Cognifair/bio-exam-index@v1/…` 地址）。仓库名已确认可用：`Cognifair/bio-exam-index`（你 GitHub 账号存在，该仓库 404 = 未被占用）。

## 1. 本地建仓并推送

```powershell
cd C:\Users\Onoya\MinerU\exports\publish
git init -b main
git add -A
git commit -m "索引 v1：1097 题 / 218 考点"
git remote add origin https://github.com/Cognifair/bio-exam-index.git
git push -u origin main
git tag v1
git push origin v1
```

> 仓库必须是 **public**；私有仓库 jsDelivr 拉不到。

## 2. 验证 CDN 生效

推送后等 1–2 分钟（jsDelivr 首次拉取要同步），然后本机测：

```powershell
Invoke-WebRequest "https://cdn.jsdelivr.net/gh/Cognifair/bio-exam-index@v1/points.json" -UseBasicParsing | Select-Object StatusCode, @{n='KB';e={[int]($_.RawContentLength/1024)}}
```

- 200 且约 60 KB → 可以用；
- 404 → 检查 tag 是否推了（`git ls-remote --tags origin`）、仓库是否 public；
- 想强制刷新缓存：访问 `https://cdn.jsdelivr.net/gh/Cognifair/bio-exam-index@main/points.json` 再切回 `@v1`。

## 3. 国内备用线路（可选，推荐）

没代理的人 jsDelivr 偶尔也慢。备一份到阿里云 OSS 公共读：

```powershell
# 需先装 ossutil 并配置 AK
ossutil cp -r C:\Users\Onoya\MinerU\exports\publish\ oss://<你的bucket>/bio-exam-index/ --update
```

对外给：`https://<bucket>.oss-cn-hangzhou.aliyuncs.com/bio-exam-index/points.json`
（OSS 默认域名访问不需要备案；1.24 MB 存储，一年几毛钱。）

## 4. Gitee 镜像（可选）

```powershell
# Gitee 建同名仓库后
git remote add gitee https://gitee.com/Cognifair/bio-exam-index.git
git push gitee main --tags
```

给：`https://gitee.com/Cognifair/bio-exam-index/raw/v1/points.json`
注意 Gitee raw 偶尔会跳登录页，别当唯一入口。

## 5. 发出去之前最后三件事

1. 打开 `points.json` 抽 3 条 `root_id`，在客户端点开确认能跳转到正确卡片；
2. 确认牌组可见性（现在是公开）；
3. 把仓库链接和提示词一起贴到论坛/群；
   创作者只需要复制 `prompt.md` 里那一段。

## 6. 以后更新

```powershell
cd C:\Users\Onoya\MinerU
python export_exam_index.py && python split_chapters.py
# 新题补标考点后
python build_exam_index_pkg.py && python build_publish.py
cd exports\publish && git add -A && git commit -m "索引 v2" && git tag v2 && git push --tags
```

**别覆盖旧 tag**：`@v1` 保持不动，发 `@v2`，让用旧地址的人拿到不变的内容。
