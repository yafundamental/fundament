Part of: [[Управление потоком исполнения]]

- ловит панику, но только если паника была вызвана в той же горутине, что и recover
- работает только так
```
func main() {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("got panic:", err)
		}
	}()

	panic("my panic")

	fmt.Println("after panic")
}

-> output:
# got panic: my panic
```
