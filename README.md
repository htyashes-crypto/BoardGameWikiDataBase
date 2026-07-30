# BoardGameWikiDataBase

**HtyWiki 桌面应用的云端知识库。** 本仓库存放知识内容本身,不含任何应用代码。

## 目录即数据库

没有中央注册表——目录结构就是数据结构,App 扫描目录即得知识树。

```
docs/                            文档块
  <知识类型>/                     文件夹 = 知识类型(kebab-case,App 顶部大标签)
    _meta.json                   可选:显示名 / 排序 / 成员顺序
    <知识项>.md                   md 文件 = 知识项
    fig-*.png                    图片与引用它的 md 同目录
    <分类子目录>/                 可选:多级分类,可任意嵌套
teaches/                         交互教学块
  <教学类型>/
    <教学 id>/                   含 teach.json 的目录 = 教学项
      teach.json                 教学 DSL(scenes / popups / hotspots)
      img/                       该教学的背景图与素材
    <分类子目录>/                 不含 teach.json 的目录 = 分类文件夹
```

判别规则:`teaches/` 下**含 `teach.json` 的目录是教学项,不含的是分类文件夹**。教学 id 全局唯一,且必须与其目录名一致。

### `_meta.json`(每层可选)

```jsonc
{
  "title": "桌游编辑器",     // 本层显示名;缺省用文件夹名
  "order": 1,               // 类型排序,小在前
  "items": [                // 本层子级顺序;未列出的自动追加
    { "file": "intro.md", "title": "入门介绍" },   // 对象 = md 文件(仅 docs)
    "advanced"                                     // 字符串 = 子目录名 / 教学 id
  ]
}
```

## 内容约定

- md 首行是唯一 `# H1`,**无 frontmatter**;支持 GFM 表格 / 任务列表 / 围栏代码块;不支持原始 HTML、数学公式、Mermaid、`:::note` directive。
- 跨条目跳转用 `app://` 协议:`app://doc/<相对 docs 的路径>`(`.md` 可省)、`app://teach/<id>[?step=<sceneId>]`。
- 图片用相对路径引用:`![说明](./fig-xxx.png)`。

## 怎么修改

推荐通过 **HtyWiki 应用**修改:联网模式下应用会把本地变更打包成一次提交推到这里(需要管理员发放的上传密钥)。应用内的「AI 导入知识」会生成提示词,交给 AI Agent 按上述规范批量整理文档。

直接在本仓库编辑也可以——应用下次同步时会拉取最新内容。

## 注意

`.gitattributes` 里的 `* -text` 不要删。应用用 git blob sha 判断文件是否变化,任何换行符改写都会让同一份内容在两端算出不同的 sha,导致同步与待上传差分永久误报。
