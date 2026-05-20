# purchase-record-pdf

把本地订单截图、付款截图、发票 PDF 或发票截图整理成一份结构化购买记录 PDF。

这个 skill 适合下面这些场景：

- 按固定版式整理报销材料
- 需要把订单截图、付款截图、发票做成逐项单页
- 需要首页分类汇总、末页 TODO、以及金额一致性检查

## 工作目录约定

每次正式开始整理前，先新建一个独立工作目录，专门放：

- 本次要用的购买记录图片
- 本次要用的消费记录图片
- 发票图片或发票 PDF
- 购买记录 `.tex`
- 最终输出 `.pdf`

不要把这些文件继续散落在项目根目录或 `~/Downloads`。推荐目录名例如：

```text
购买记录材料/
四张机-购买记录/
2026-05-20-购买记录/
```

## 目录结构

```text
purchase-record-pdf/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
├── assets/
│   └── purchase_record_template.tex
└── examples/
    ├── 1-购买记录.png
    ├── 1-消费记录.png
    ├── 1-发票.png
    ├── example_report.tex
    └── example_report.pdf
```

## 内置内容

- `SKILL.md`
  - 中文工作流说明，定义触发条件、配对规则、版式和验证要求
- `assets/purchase_record_template.tex`
  - 通用 LaTeX 模版
  - 默认结构为：首页总表、逐项单页、末页 TODO
  - 单页构图固定为：左上购买记录，右上消费记录，下方发票
- `examples/1-购买记录.png`、`examples/1-消费记录.png`、`examples/1-发票.png`
  - 三张脱敏示例图
  - 与示例购买记录放在同一目录，并按 `序号-购买记录 / 序号-消费记录 / 序号-发票` 命名
  - 来自真实样例，但已对姓名、手机号、付款方式、订单号、票面主体和开票人等信息做马赛克处理
- `examples/example_report.tex`
  - 可直接编译的示例文档

## 快速使用

1. 把模版复制到你的项目目录。

```bash
cp assets/purchase_record_template.tex /path/to/project/report.tex
```

2. 先新建工作目录，再把模版、图片和输出文件都放进去。

3. 根据实际记录替换首页汇总表、每页 `\recordpage` 内容和 TODO。

建议先把要入册的素材复制到这个工作目录，并改成下面这种名字：

```text
1-购买记录.png
1-消费记录.png
1-发票.pdf
2-购买记录.jpg
2-消费记录.jpg
2-发票.png
```

4. 编译 PDF。

```bash
/Library/TeX/texbin/xelatex -interaction=nonstopmode -output-directory examples examples/example_report.tex
```

## 示例输出

- 示例源码：[`examples/example_report.tex`](./examples/example_report.tex)
- 示例 PDF：编译后生成在 `examples/example_report.pdf`

## 说明

- 模版默认按相对路径引用图片，便于直接复制到项目目录继续编辑
- 默认不要继续直接引用 `~/Downloads`；先建工作目录，再把已确认素材复制进去并统一改名
- 如果用户要求“三个金额严格相等”，就不能把金额不一致的发票强行并入正文
- 缺订单截图、缺付款截图、缺发票、已有但未匹配，应该在 TODO 中分开列出
