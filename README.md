# Blinkfox Java 编程风格指南

这是关于 Java 编程风格相关的仓库。核心来自于 Google 的 Java 编程风格指南（[Google Java Style Guide](https://checkstyle.sourceforge.io/styleguides/google-java-style-20180523/javaguide.html#s3.3.3-import-ordering-and-spacing)，并对该份英文指南做了翻译，并最终整理出了适合我和更多人的一份规范来。

- [查看文档 https://blinkfox.github.io/java-style](https://blinkfox.github.io/java-style)

## 主要内容

本仓库核心内容如下：

```bash
- docs
  - checks
    - blinkfox-checks.xml            # 经过部分新增、修改和详细注释的，符合 Blinkfox 编程风格的 checkstyle 文件.
    - blinkfox-idea-java-style.xml   # 符合 Blinkfox 编程风格的，可导入 Intellij IDEA 中的 Java code style 的格式化文件.
    - google-checks.xml              # 未经修改的符合 Google 编程风格的原生 checkstyle 文件.
  - guide
    - blinkfox-java-style-guide.md   # 基于 Google 原生的编程风格而新增、修改的，且符合 Blinkfox 编程风格的简要中文指南
    - google-java-style-guide-cn.md  # 翻译的 Google 原生的编程风格指南的中文文档
    - google-java-style-guide.md     # Google 原生的编程风格指南英文文档
- LICENSE                            # 仓库协议，Apache License2.0
```

本仓库的核心产出结果主要是《Google Java 编程风格指南.md》、《Blinkfox Java 编程风格指南》、《blinkfox-checks.xml》、《blinkfox-idea-java-style.xml》。

其中《Google Java 编程风格指南.md》和《Blinkfox Java 编程风格指南》是文档型，主要供大家阅读参考。而[《blinkfox-checks.xml》](https://github.com/blinkfox/java-style/blob/master/checks/blinkfox-checks.xml)和[《blinkfox-idea-java-style.xml》](https://github.com/blinkfox/java-style/blob/master/checks/blinkfox-idea-java-style.xml) 才是真正可实际用于工具和项目中的代码风格检查和格式化文件。

## License

Apache License 2.0。
