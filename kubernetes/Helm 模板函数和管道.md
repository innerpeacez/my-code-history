#### helm chart 常用函数

##### quote 函数

将对象转换成字符串

```yaml
{{ quote .Values.favorite.drink }}
```

使用管道

```yaml
{{ .Values.favorite.drink | quote }}
```

管道可以将几个功能链接在一起，如：

```yaml
{{ .Values.favorite.food | upper | quote }}
```

多个函数，将执行完上一个管道中的函数，再将值作为参数传递到下一个管道中的函数中执行

##### default 函数

允许在模板内部指定默认值，以防该值被省略

```yaml
{{ .Values.favorite.drink | default "tea" | quote }}
```

