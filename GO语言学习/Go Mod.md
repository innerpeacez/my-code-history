**go mod 初始化**

```go
go mod init
```

**查看所有依赖**

```go
go list -m all
```

**删除未使用的依赖**

```go
go mod tidy
```

**获取所有依赖**（注意：这里会获取所有依赖的 latest 版本）

```go
go get -u all
```

**列出指定依赖的所有版本**（以 rsc.io/sampler 为例）

```go
go list -m -versions rsc.io/sampler
```

