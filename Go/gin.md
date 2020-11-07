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

## structをHTMLで表示

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
	"time"
)

// Todoはtodoリストを格納するための構造体
type Todo struct {
	CtreatedAt time.Time
	Human      string
	Content    string
	Status     int
}

func main() {

	todo := []Todo{
		Todo{
			CtreatedAt: time.Now(),
			Human:      "totsuka",
			Content:    "早起き",
			Status:     0,
		},
		Todo{
			CtreatedAt: time.Now(),
			Human:      "suzuki",
			Content:    "洗濯物",
			Status:     1,
		},
	}

	router := gin.Default()                       //define gin variable
	router.Static("styles", "./styles")           //difine static directory(ex css)
	router.LoadHTMLFiles("./template/index.html") //define HTML file

	router.GET("/todo", func(c *gin.Context) { //c = ctx = context : 文脈・状態 // *gin.Contextは型!!
		c.HTML(http.StatusOK, "index.html", map[string]interface{}{
			"todo": todo,
		})
	})

	router.Run() // listen and serve on 0.0.0.0:8080 (for windows "localhost:8080")
}

```

```HTML
<!-- HTML -->
    {{range .todo}}
      <li>{{.CtreatedAt}}{{.Human}}{{.Content}}{{.Status}}</li>
    {{end}}
```
