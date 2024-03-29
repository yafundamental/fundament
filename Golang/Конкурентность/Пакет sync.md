>> является частью [[Конкурентность|конкурентности Golang'а]]

### Основные сведения
---
1. конкурентно исполняемые goroutine'ы время от времени необходимо синхронизировать
	- во-первых для согласованной работы
	- во-вторых во избежания возникновения Race Condition (гонка данных, состояние гонки) -- явление, когда одновременно к одним и тем же данным имеет доступ несколько горутин, из-за чего случается Undefined Behavior
2. несколько основных примитивов синхронизации, используемые в Golang:
	- `sync.Mutex`
	- [[#sync.RWMutex|sync.RWMutex]]
	- `sync.Map`
	- `sync.WaitGroup`
	- channel'ы
	- `atomic`

### sync.RWMutex
---
- несколько goroutine'н могут делать mtx.RLock, при этом первая попавшаяся goroutine'а, которая захочет сделать mtx.Lock будет ждать всех mtx.RUnlock
- mtx.RLock ожидает освобождения mtx.Unlock
##### Вопрос
Казалось бы, если mtx.RLock на самом деле ничего не блокирует и пропускает все горутины, то зачем он вообще нужен? На самом деле, если у нас есть активные, не анлокнутые mtx.RLock, то mtx.Lock будет ждать всех mtx.RUnlock, чтобы опеспечить корректный конкурентный доступ к данным
