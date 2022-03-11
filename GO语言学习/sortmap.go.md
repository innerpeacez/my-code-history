```go
package sortmap

type SortMap []*SortMapNode

type SortMapNode struct {
	Key string
	Val interface{}
}

func (c *SortMap) Put(key string, val interface{}) {
	index, _, ok := c.get(key)
	if ok {
		(*c)[index].Val = val
	} else {
		node := &SortMapNode{Key: key, Val: val}
		*c = append(*c, node)
	}
}

func (c *SortMap) Get(key string) (interface{}, bool) {
	_, val, ok := c.get(key)
	return val, ok
}

func (c *SortMap) get(key string) (int, interface{}, bool) {
	for index, node := range *c {
		if node.Key == key {
			return index, node.Val, true
		}
	}
	return -1, nil, false
}

```





