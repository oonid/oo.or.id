 

# oo.or.id

### Prerequisites

* hugo

### Installing

```bash
git clone git@github.com:oonid/oo.or.id.git
cd oo.or.id
git submodule update --init --recursive
```

### Deployment

```bash
HUGO_ENV=production nice hugo --cleanDestinationDir --minify --debug
firebase deploy
```

### Authors

**oon arfiandwi** [@oonid](https://github.com/oonid)

### License

CC-BY-SA
