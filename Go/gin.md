## about gin

gin installation:  
https://github.com/gin-gonic/gin#installation  

official:  
https://gin-gonic.com/docs/  

Qiita (gin template)  
https://qiita.com/lanevok/items/dbf591a3916070fcba0d

## to install gin
```
$ go get -u github.com/gin-gonic/gin
```

## to use gin
```go
import "github.com/gin-gonic/gin"
```
## bind html

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	r := gin.Default()
	r.LoadHTMLFiles("./template/index.html")

	r.GET("/todo", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", map[string]interface{}{})
	})

	r.Run() // listen and serve on 0.0.0.0:8080 (for windows "localhost:8080")
}
```
