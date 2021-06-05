# Cache
- CacheService
- MemoryCacheService
- RedisService of [core-go/redis](https://github.com/core-go/redis) is an implementation of CacheService

## Installation
Please make sure to initialize a Go module before installing core-go/cache:

```shell
go get -u github.com/core-go/cache
```

Import:
```go
import "github.com/core-go/cache"
```

## Example
```go
package main

import (
	"fmt"
	"github.com/core-go/cache"
	"github.com/core-go/redis"
	"time"
)

type User struct {
	FirstName string `json:"firstName,omitempty"`
	LastName  string `json:"lastName,omitempty"`
	Email     string `json:"email,omitempty"`
}

func main() {
	user := User{
		FirstName: "Peter",
		LastName:  "Parker",
		Email:     "peter.parker@gmail.com",
	}

	//Example for MemoryCacheService
	cacheConfig := cache.CacheConfig{
		Size: 10*1024*1024,
		CleaningEnable: true,
		CleaningInterval: 10 * time.Minute}
	cacheService, _ := cache.NewMemoryCacheService(cacheConfig)
	cacheService.Put("user", &user, 10 * time.Hour)

	data, _ := cacheService.Get("user")
	fmt.Println("user in cache: ", data)

	//Example for RedisService
	redisService, _ := redis.NewRedisService("redis://localhost:6379")
	redisService.Put("user", &user, 0)

	redisData, _ := redisService.Get("user")
	fmt.Println("user in redis: ", redisData)
}
```