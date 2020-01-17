---
title: "Translate Sphinx Doc to Bahasa Indonesia"
date: 2019-10-22 23:37:15+08:00
image: "/images/2014/python-powered-h-140x182.png"
url: "/content/translate-sphinx-doc-to-bahasa-indonesia"
tags: ["Python", "Bahasa Indonesia", "Documentation", "Sphinx", "Open Source"]
draft: false
---

{{< figure src="/images/2019/docs_python_org_1.webp" title="Python Documentation in Bahasa Indonesia" >}}

The story starts from here..

My effort to translate Python Documentation to Bahasa Indonesia, finally live at [docs.python.org](https://docs.python.org/id/3.8/)

And you know what happened?
See the above picture. It starts with text "documentation" with lowercase "d".

After I search it on my project [`python-docs-id`](https://github.com/python/python-docs-id), I can't found that text. Googling it a while, then realized it comes from **Sphinx-doc** [^1], the app that generates that documentation from all those ".po" files.

K! it means, it will need another project to translate then, here we go!

goes to **Sphinx-doc** project in Transifex [^2].

and boom!

{{< figure src="/images/2019/sphinx_doc_transifex_1.webp" title="Sphinx Documentation in Bahasa Indonesia" >}}

I have translated (the rest, needed) them all. Now the project is 100% translated for Bahasa Indonesia.

and worth to mention, thank you (Sakti Dwi Cahyono)[https://www.transifex.com/user/profile/54krpl/], now I have been one of Coordinator of Sphinx Doc translation to Bahasa Indonesia in Transifex.

[^1]: http://www.sphinx-doc.org
[^2]: https://www.transifex.com/sphinx-doc/