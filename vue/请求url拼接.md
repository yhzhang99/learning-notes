# 请求url拼接.md

``` javascript
fetchData() {
      request
        .get({
          url: `${this.topic}stories.json`,
        })
        .then((response) => {
          this.addId(response);
          this.getNews();
        });
    }
```