 

# oo.or.id

### Prerequisites

* hugo
* node v12 for search index

### Installing

```bash
git clone git@github.com:oonid/oo.or.id.git
cd oo.or.id
git submodule update --init --recursive
npm install parser-front-matter lunr
```

### Deployment

```bash
node ./build-lunarjs-index.js > static/search-index.json
hugo --cleanDestinationDir
hugo --minify && firebase deploy
```

### Authors

**oon arfiandwi** [@oonid](https://github.com/oonid)

### License

CC-BY-SA