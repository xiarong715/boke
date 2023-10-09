---
title: "Go 一种典型的编程方式"
date: 2023-06-09T11:59:12+08:00
draft: false
tags: ["go", "编程方式"]
categories: ["Golang"]
---

## Go 一种典型的编程方式

把各个要用到的参数都放到一个结构体中，初始化结构体，并为结构体添加方法。避免变量散落在各处，也符合面向对象编程思想。很多的语言都采用把变量集中放到一个结构体中，比如C语言。这是编程的基本思想，所以特别提取出来説一下。

```go
// 演示一下
// 非常值得借鉴
// handler.go
package footkeeper

// 操作的结构体
type FaasHandler struct {
    datapath string
    settingpath string
    ...
}

// 局部方法，只能在包内使用
func (faasHandler *FaasHandler) sendMulticast() {
    ...
}

// 全局方法，可在包外使用
// 初始化结构
// 在包外用于初始化结构体
func NewFaasHandler(dpath string) *FaasHandler {
    faas := &FaasHandler{datapath: dpath}
    faas.settingpath = filepath.Join(dpath, "setting.yml")
    ...
    return faas
}

// 结构体的方法
// 对结构体操作的方法
func (faasHandler *FaasHandler) SetFaasEnv(key, value string) {
    faasHandler.env[key] = value
}

func (faasHandler *FaasHandler) Init() {
    ...
}

func (faasHandler *FaasHandler) HandleRequest(gw string, w http.ResponseWriter, r *http.Request) {
    ...
}

// 实现http.Handler接口
// http请求的入口函数
// 最后转给处理请求的函数
func (faasHandler *FaasHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	...
    faasHandler.HandleRequest(gw, w, r)		// 调用真正处理请求的逻辑
}

...
```

```go
// main.go
package main

func main() {
    dataidr := "/data/faas"
	faas := footkeeper.NewFaasHandler(datadir)
	faas.Init()
	faas.SetFaasEnv("LOCALHOST_URL", "http://127.0.0.1:8000")
	http.Handle("/", faas)
	http.ListenAndServe(api_listen_addr, nil)
}
```



从http代码中也有类似的做法。

```go
// main.go
http.Handle("/", faas)

// server.go
// 需要用到的变量放到一个结构体中
type ServeMux struct {
    mu sync.RWMutex
    m map[string]muxEntry
    es []muxEntry
    hosts bool
}

type muxEntry struct {
    h Handler
    pattern string
}

// 定义全局变量
var defaultServeMux ServeMux
var DefaultServeMux = &defaultServeMux

// 包外直接使用的函数
// 函数中调用了方法。结构体实例是在包内定义的。
// 实现了从包外调用到包内逻辑的通路。
func Handle(pattern string, handler Handler) { 
	DefaultServeMux.Handle(pattern, handler) 
}

func (mux *ServeMux) Handle(pattern string, handler Handler) {
    mux.mu.Lock()
    defer mux.mu.Unlock()
    e := muxEntry{h: handler, pattern: pattern}
    mux.m[pattern] = e
    if pattern[len(pattern)-1] == '/' {
        mux.es = appendSorted(mux.es, e)
    }
    ...
}

// 追加并排序
func appendSorted(es []muxEntry, e muxEntry) []muxEntry {
    ...
}

type Server struct {
    Addr string
    Handler Handler
    ...
}

func ListenAndServe(addr string, handler Handler) error {
    server := &Server{Addr: addr, Handler: handler}
    return server.ListenAndServe()
}

func (srv *Server) ListenAndServe() error {
    ...
    ln, err := net.Listen("tcp", addr)
    return srv.Serve(ln)
}

func (srv *Server) Serve(l net.Listener) error {
    ...
    ctx := context.WithValue(context.Background(), ServerContextKey, srv)
    for {
        rw, err := l.Accept()
        connCtx := ctx
        c := srv.newConn(rw)
        go c.serve(connCtx)
    }
}

func (srv *Server) newConn(rwc net.Conn) *conn {
    c := &conn{
        server: srv,
        rwc: 	rwc,
    }
    ...
    return c
}

func (c *conn) serve(ctx context.Content) {
    ...
    serverHandler{c.server}.ServeHTTP(w, w.req)	// 调用到上面自定义的ServeHTTP方法
    ...
}

type serverHandler struct {
    srv *Server
}

func (sh serverHandler) ServeHTTP(rw ResponseWriter, req *Request) {
    handler := sh.srv.Handler		// 
    if handler == nil {
        handler = DefaultServeMux
    }
    ...
    handler.ServeHTTP(rw, req)
}

func (mux *ServeMux) ServeHTTP(w ResponseWriter, r *Request) {
    ...
    h, _ := mux.Handler(r)
    h.ServeHTTP(w, r)
}

func (mux *ServeMux) Handler(r *Request) (h Handler, pattern string) {
    return mux.handler(host, r.URL.Path)
}

func (mux *ServeMux) handler(host, path string) (h Handler, pattern string) {
    mux.mu.RLock()
    defer mux.mu.RUnlock()
    
    if mux.hosts {
        h, pattern = mux.match(host + path)
    }
    if h == nil {
        h, pattern = mux.match(path)
    }
    if h == nil {
        h, pattern = NotFoundHandler(), ""
    }
    
    return
}

func (mux *ServeMux) match(path string) (h Handler, pattern string) {
    v, ok := mux.m[path]
    if ok {
        return v.h, v.pattern
    }
    for _, e := range mux.es {
        if strings.HasPrefix(path, e.pattern) {
        	return e.h, e.pttern   
        }
    }
    
    return nil, ""
}
```



```go
// 下次分析 
http.HandleFunc("/", helloFunc)

func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
	DefaultServeMux.HandleFunc(pattern, handler)
}

// 待分析
```

