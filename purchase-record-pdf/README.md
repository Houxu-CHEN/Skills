# purchase-record-pdf

把本地订单截图、付款截图、发票 PDF 或发票截图整理成一份结构化购买记录 PDF。

这个 skill 适合下面这些场景：

- 按固定版式整理报销材料
- 需要把订单截图、付款截图、发票做成逐项单页
- 需要首页分类汇总、末页 TODO、以及金额一致性检查

## 目录结构

```text
purchase-record-pdf/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
└── assets/
    ├── purchase_record_template.tex
    └── examples/
        ├── example_order_redacted.png
        ├── example_payment_redacted.png
        └── example_invoice_redacted.png
```

## 内置内容

- `SKILL.md`
  - 中文工作流说明，定义触发条件、配对规则、版式和验证要求
- `assets/purchase_record_template.tex`
  - 通用 LaTeX 模版
  - 默认结构为：首页总表、逐项单页、末页 TODO
  - 单页构图固定为：左上购买记录，右上付款记录，下方发票
- `assets/examples/*.png`
  - 三张脱敏示例图
  - 来自真实样例，但已对姓名、手机号、付款方式、订单号、票面主体和开票人等信息做马赛克处理
- `examples/example_report.tex`
  - 可直接编译的示例文档

## 快速使用

1. 把模版复制到你的项目目录。

```bash
cp assets/purchase_record_template.tex /path/to/project/report.tex
```

2. 根据实际记录替换首页汇总表、每页 `\recordpage` 内容和 TODO。

3. 编译 PDF。

```bash
/Library/TeX/texbin/xelatex -interaction=nonstopmode -output-directory examples examples/example_report.tex
```

## 示例输出

- 示例源码：[`examples/example_report.tex`](./examples/example_report.tex)
- 示例 PDF：编译后生成在 `examples/example_report.pdf`

## 说明

- 模版默认按相对路径引用图片，便于直接复制到项目目录继续编辑
- 如果用户要求“三个金额严格相等”，就不能把金额不一致的发票强行并入正文
- 缺订单截图、缺付款截图、缺发票、已有但未匹配，应该在 TODO 中分开列出
