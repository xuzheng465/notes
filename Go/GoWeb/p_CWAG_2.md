# Routing Requests



# Working With HTTP Requests



https://localhost:8000/search?q=products&page=1

```go
func (w http.ResponseWriter, r *http.Request) {
  url := r.URL	// net/url.URL
  query := url.Query()	// Value (map[string][]string) string: []string
  q := query["q"]				// []string{"products"}
  page := query.Get("page")	// "1" first instance
}
```



### Form Data

```go
func (w http.ResponseWriter, r *http.Request) {
  err := r.ParseForm()		// populate Form and PostForm
  f := r.Form							// net/url.Values
  username := f["username"]	// []string {"mike"}
  pass:=f.Get("password")
}
```



中间件的一个非常常见的用例是在应用程序内进行日志记录, What sites are being accessed within your application inorder to gather some analytic information. 