---
title: "What I have learned from Building Static Site with 100K+ Pages"
date: 2020-09-12 14:15:00+08:00
publishDate: 2020-09-12 14:15:00+08:00
image: "/images/2020/3000000_views_photos_google_maps.png"
url: "/content/what-i-have-learned-building-static-site-100k-pages"
tags: ["Static Site Generator", "Hugo", "Firebase", "Docker", "Markdown"]
draft: false

---

{{< figure src="/images/2020/static-site-100k-pages.webp" title="What I have learned from Building Static Site with 100K+ Pages" >}}

On daily basis, I only work with a static site which has 1K pages. But then I need to export more than 100.000 markdown files to a static website.

As before, I choose Hugo to build the static site, which I think it should be fast and light enough to process that. But.. hm, not exactly as I thought.. :clown_face:

So, here are some taken points.

## hugo serve

The first problem that I got was no response when I run `hugo serve`, it was because building the 100K+ markdown files was not easy and light process.

So, forget about preview 100K pages, get some samples of few files or a few hundred files, only to test and preview the site.

## hugo

My first response of not responded `hugo serve` command actually stop the process (`ctrl c`) then run `hugo` command to build the markdown files. Of course, this command has produced the same result, not responded.

To debug, try to add debug parameter `hugo --debug` and then it started to show the building process one by one for thousands of template rendering.

After waiting for about **3 hours** with ~~an almost~~ overheat notebook, finally, the build process was done successfully :grinning:

Upload to Firebase hosting, and it takes almost 2GB storage, including the static images and documents. First deployment success :ballot_box_with_check:.

## sitemap

I start to evaluate on the Google Webmaster Tools and I saw that the sitemap got rejected because it was more than 50K or URLs. Here's [^1] the reference for maximum 50MB and 50.000 URLs.

Try to find the solution in hugo discussion board [^2][^3], but currently no easy solution to split it. May be because not so many sites will produce more than 50K URLs.

I did not want to spend to much time with this problem, so I just use this [^4] python script to split the sitemap xml file.

## delete 100K+ files in single directory

Sometimes I need to clean up the markdown files, but the linux `rm` command unable to delete a list of more than 100.000 file names, because the argument was too long.

So I need to use a pipe to `xargs rm` command:

```bash
find contents/ -name "*.md" | xargs rm
```

## set lower resource usages to rebuild the static site

As I mentioned before, the build process took about 3 hours with 8 CPUs usages, which triggered a high temperature in my notebook. Next, I am planning to build on small VPS instances, or even Github actions. So, I need to simulate the process to set lower resource usages.

My first try was to set a lower priority of the `hugo` process by running command `nice hugo`, but I saw that it's not effective, even though the resource usages getting lower.

2nd try was using docker to limit the CPU usage, and it worked very well. Here's the command that I used:

```bash
HUGO_ENV=production docker run --rm -it --cpus="1.5" -v $(pwd):/src klakegg/hugo:0.74.3 --cleanDestinationDir --minify --debug

```

I used minimal docker images from [^5] and docker runtime options from [^6].

I took about 7 hours to complete the `hugo` build process with 1.5 CPUs. Sorry, can not share the metrics.

## Linux ext2 file system limitation

So, I have been thinking to store the files on portable storage like USB flash drive, which I use Linux ext2 (extended 2) file system because I don't think I need journal for portable storage, but then I got another error related to maximum directory.

Linux ext2 file system limits sublevel-directory to 31998 due to the link-count limit [^7].

Re-format to ext4 file system, you said? Yes, change it to Linux ext4 file system was not that easy to configure (`dir_index`), even my computer SSD which I used to build the static site using ext4, but at some point, on flash drive I still got stuck with mysterious no space left on device [^8] over hundred of thousands files and folders on ext4.


That's all folks! but at the end, I just really enjoy the experienced. :raised_hands:



Photo by [Clark Gu](https://unsplash.com/@atluminon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on Unsplash.

[^1]: https://support.google.com/webmasters/answer/183668?hl=en&ref_topic=4581190
[^2]: https://discourse.gohugo.io/t/please-add-limit-the-sitemap-size/20421
[^3]: https://discourse.gohugo.io/t/limitation-for-sitemap-xml-size/2726
[^4]: https://gist.github.com/kravietz/f2eec690ecb4dde84206
[^5]: https://github.com/klakegg/docker-hugo
[^6]: https://docs.docker.com/config/containers/resource_constraints/
[^7]: https://en.wikipedia.org/wiki/Ext2
[^8]: https://blog.merovius.de/2013/10/20/ext4-mysterious-no-space-left-on.html
