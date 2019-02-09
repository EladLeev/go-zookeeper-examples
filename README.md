# Go-zookeeper examples

Just a few examples on how to use go-zookeeper - Native Go Zookeeper Client Library.

[https://github.com/samuel/go-zookeeper](https://github.com/samuel/go-zookeeper)


# Files

The full code is available as [examples.go](https://github.com/EladLeev/go-zookeeper-examples/blob/master/examples/examples.go "examples.go") in this repo, which hopefully will be merged into Samuel repo.

# How to
## Get
Use `Get` to simply get data from ZK.

`Get` gets a string path, and return the data as []byte.
```go
func getZnode(c *zk.Conn, path string) string {
	data, _, err := c.Get(path)
	if err != nil {
		panic(err)
	}
	s := string(data[:])
	return s
}
```

## Children
Use `Children` to get a slice of strings with all the children of provided path.
```go
func getChildren(c *zk.Conn, path string) []string {
	data, _, err := c.Children(zkPath)
	if err != nil {
		panic(err)
	}
	return data
}
```

## Exists
Use `Exists` to check if zNode exist.

Retrun value is bool
```go
func checkZnode(c *zk.Conn, path string) bool {
	exists, _, err := c.Exists(zkPath)
	if err != nil {
		panic(err)
	}
	return exists
}
```

## GetW
Set watch on znode, and wait for event.

Catch the event and use switch - case to do something, or just print the event name.
```go
	data, _, ch, err := c.GetW(zkPath)
	if err != nil {
		panic(err)
	}
	fmt.Printf("The current data is: %v.\nI'm watching.\n", string(data[:]))
	for event := range ch {
		switch event.Type {
		case 1:
			fmt.Printf("Node created.")
			// Do somthing
		case 2:
			fmt.Printf("Node deleted!")
			// Do somthing
		case 3:
			fmt.Printf("Node changed.")
			// Do somthing
		case 4:
			fmt.Printf("Node children changed.")
			// Do somthing
		default:
			fmt.Printf("Something happen.")
		}
		// Or just ...
		fmt.Println(event.Type)
	}
```
