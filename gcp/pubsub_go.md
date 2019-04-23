### ライブラリの取得

```bash
$ go get -u cloud.google.com/go/pubsub
```

---
### topic, subscription の作成

```bash
$ gcloud pubsub topics create test-topic-sdk
Created topic [projects/test-gcp-app-1555841619674/topics/test-topic-sdk].

$ gcloud pubsub subscriptions create test-sub-sdk --topic test-topic-sdk
Created subscription [projects/test-gcp-app-1555841619674/subscriptions/test-sub-sdk].
```

---
### メッセージの送信

#### コード
[pubsub_publish_message.go](./pubsub_publish_message.go)
```golang
package main

import (
	"context"
	"fmt"
	"log"
	"os"

	"cloud.google.com/go/pubsub"
)

func main() {
	ctx := context.Background()
	proj := os.Getenv("GOOGLE_CLOUD_PROJECT")
	if proj == "" {
		fmt.Fprintf(os.Stderr, "GOOGLE_CLOUD_PROJECT environment variable must be set.\n")
		os.Exit(1)
	}
	client, err := pubsub.NewClient(ctx, proj)
	if err != nil {
		log.Fatalf("Could not create pubsub Client: %v", err)
	}

	const topic = "test-topic-sdk"

	if err := publish(client, topic, "Hello World!"); err != nil {
		log.Fatalf("Failed to publish: %v", err)
	}
}

func publish(client *pubsub.Client, topic, msg string) error {
	ctx := context.Background()
	t := client.Topic(topic)
	result := t.Publish(ctx, &pubsub.Message{
		Data: []byte(msg),
	})

	id, err := result.Get(ctx)
	if err != nil {
		return err
	}
	fmt.Printf("Published a message; msg ID: %v\n", id)

	return nil
}
```

#### 実行
```bash
$ export GOOGLE_CLOUD_PROJECT=test-gcp-app-1555841619674; go run pubsub_publish_message.go
Published a message; msg ID: 525459957986821
```

---
### メッセージの受信

#### コード
[pubsub_receive_message.go](./pubsub_receive_message.go)
```golang
package main

import (
	"context"
	"fmt"
	"log"
	"os"

	"cloud.google.com/go/pubsub"
)

func main() {
	ctx := context.Background()
	proj := os.Getenv("GOOGLE_CLOUD_PROJECT")
	if proj == "" {
		fmt.Fprintf(os.Stderr, "GOOGLE_CLOUD_PROJECT environment variable must be set.\n")
		os.Exit(1)
	}
	client, err := pubsub.NewClient(ctx, proj)
	if err != nil {
		log.Fatalf("Could not create pubsub Client: %v", err)
	}

	const sub = "test-sub-sdk"

	if err := pullMsg(client, sub); err != nil {
		log.Fatal(err)
	}
}

func pullMsg(client *pubsub.Client, subName string) error {
	ctx := context.Background()

	sub := client.Subscription(subName)
	cctx, cancel := context.WithCancel(ctx)
	err := sub.Receive(cctx, func(ctx context.Context, msg *pubsub.Message) {
		fmt.Printf("Got message: %q\n", string(msg.Data))
		msg.Ack()
		cancel()
	})
	if err != nil {
		return err
	}
	return nil
}
```

#### 実行
```bash
$ go run pubsub_receive_message.go
Got message: "Hello World!"
```
