---
title: "Updating Wiki GitHub to Google Docs with the NYTimes Library"
date: 2023-03-30 18:19:20+07:00
publishDate: 2021-03-30 18:19:20+07:00
image: "/images/2023/nytimes_library.gif"
url: "/content/updating-wiki-github-to-google-docs-with-nytimes-library"
tags: ["Wiki", "NYTimes Library", "Google Docs"]
draft: false

---


{{< figure src="/images/2023/nytimes_library.gif" title="Wiki as a Collaborative Documentation Site Powered by Google Docs Using the NYTimes Library" >}}

After several months of using a wiki with GitHub as the back-end and editing with Gollum, as I mentioned in article below, I have become intrigued by the wiki platform that my office has been using.

{{< article link="/content/mengelola-wiki-github-gollum/" >}}

My office use Google Docs as one of its document collaboration suite tools. With the additional of open-source software from NYTimes, called Library [^1], we can generate a wiki from online documents. These documents are stored on Google Drive and are created using Google Docs.

I employed container-based app to install the wiki with Library.
First, I downloaded the `Dockerfile` from the latest release [^2], then I build my Docker image based on that `Dockerfile`.
Once the build process was complete, I used the container image to run my wiki instance.

```bash
$ cd wiki_oo/
$ ls
Dockerfile
$ docker build -t oonid/wiki-library:1.5.1 .

$ docker run --name wiki_oo --env-file .env \
    -p 127.0.0.1:3000:3000 -d oonid/wiki-library:1.5.1
```

Library built with JavaScript (Node.js).
What particularly interests me is the process of parsing the Google Docs file and then generating static HTML from it. :thinking:

Image courtesy of the NYTimes Open Team: https://open.nytimes.com/we-built-a-collaborative-documentation-site-deploy-your-own-with-the-push-of-a-button-134de99c42fc


[^1]: https://github.com/nytimes/library
[^2]: https://github.com/nytimes/library/releases/tag/v1.5.1
