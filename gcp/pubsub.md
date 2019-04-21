
topic の確認

```bash
$ gcloud pubsub topics list
Listed 0 items.
```

topic の作成・確認

```bash
$ gcloud pubsub topics create test-topic
Created topic [projects/test-gcp-app-1555841619674/topics/test-topic].

$ gcloud pubsub topics list
---
name: projects/test-gcp-app-1555841619674/topics/test-topic
```

subscription の確認

```bash
$ gcloud pubsub subscriptions list
Listed 0 items.
```

subscription の作成・確認

```bash
$ gcloud pubsub subscriptions create --topic test-topic test-sub
Created subscription [projects/test-gcp-app-1555841619674/subscriptions/test-sub].

$ gcloud pubsub subscriptions list
name: projects/test-gcp-app-1555841619674/subscriptions/test-sub
pushConfig: {}
topic: projects/test-gcp-app-1555841619674/topics/test-topic
```

メッセージの送信

```bash
$ gcloud pubsub topics publish test-topic --message "Hello Pub/Sub"
messageIds:
- '522946134410866'
```

メッセージの受信 (ack なし)

```bash
$ gcloud pubsub subscriptions pull test-sub
┌───────────────┬─────────────────┬────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│      DATA     │    MESSAGE_ID   │ ATTRIBUTES │                                                                               ACK_ID                                                                               │
├───────────────┼─────────────────┼────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ Hello Pub/Sub │ 522946134410866 │            │ XkASTDYORElTK0MLKlgRTgQhIT4wPkVTRFAGFixdRkhRNxkIaFEOT14jPzUgKEUUC1MTUVx1E0gQalszdQdRDRlzezB2PF1AVwZMB3RfURsfWVx-SgJTDxB2e2B2bl4SAQpDVVa-_v_56Z8aZhs9XBJLLD5-PTtFQQ │
└───────────────┴─────────────────┴────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

$ gcloud pubsub subscriptions pull test-sub
┌───────────────┬─────────────────┬────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│      DATA     │    MESSAGE_ID   │ ATTRIBUTES │                                                                               ACK_ID                                                                               │
├───────────────┼─────────────────┼────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ Hello Pub/Sub │ 522946134410866 │            │ XkASTDYORElTK0MLKlgRTgQhIT4wPkVTRFAGFixdRkhRNxkIaFEOT14jPzUgKEUUC1MTUVx1E0gQalszdQdRDRlzezB2PF1AVwZMB3RfURsfWVx-SgJTDxB2e2B2bl4SAQpDVVa-_v_56Z8aZhs9XBJLLD5-PTtFQQ │
└───────────────┴─────────────────┴────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

メッセージの受信 (ack あり)

```bash
$ gcloud pubsub subscriptions pull --auto-ack test-sub
┌───────────────┬─────────────────┬────────────┐
│      DATA     │    MESSAGE_ID   │ ATTRIBUTES │
├───────────────┼─────────────────┼────────────┤
│ Hello Pub/Sub │ 522946134410866 │            │
└───────────────┴─────────────────┴────────────┘

$ gcloud pubsub subscriptions pull test-sub
Listed 0 items.
```

subscriptions の作成・確認 (メッセージ保持期間 10 分)

```bash
$ gcloud pubsub subscriptions create --topic test-topic test-sub-10m --message-retention-duration=10m
Created subscription [projects/test-gcp-app-1555841619674/subscriptions/test-sub-10m].

$ gcloud pubsub subscriptions list
---
ackDeadlineSeconds: 10
expirationPolicy:
  ttl: 2678400s
messageRetentionDuration: 604800s
name: projects/test-gcp-app-1555841619674/subscriptions/test-sub
pushConfig: {}
topic: projects/test-gcp-app-1555841619674/topics/test-topic
---
ackDeadlineSeconds: 10
expirationPolicy:
  ttl: 2678400s
messageRetentionDuration: 600s
name: projects/test-gcp-app-1555841619674/subscriptions/test-sub-10m
pushConfig: {}
topic: projects/test-gcp-app-1555841619674/topics/test-topic
```

メッセージ送信

```bash
$ gcloud pubsub topics publish test-topic --message "Hello Pub/Sub 10m"
messageIds:
- '522979987118502'
```

メッセージ受信 (メッセージ送信から 10 分以内)

```bash
$ gcloud pubsub subscriptions pull test-sub
┌───────────────────┬─────────────────┬────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│        DATA       │    MESSAGE_ID   │ ATTRIBUTES │                                                                               ACK_ID                                                                               │
├───────────────────┼─────────────────┼────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ Hello Pub/Sub 10m │ 522979987118502 │            │ XkASTDYORElTK0MLKlgRTgQhIT4wPkVTRFAGFixdRkhRNxkIaFEOT14jPzUgKEUUC1MTUVx1E0gQalszdQdRDRlzezB2PAhCCAMWUnRfURsfWVx-SgJTDxB1dGh9bVsSCQdFUVa-_v_56Z8aZhs9XBJLLD5-PTtFQQ │
└───────────────────┴─────────────────┴────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

$ gcloud pubsub subscriptions pull test-sub-10m
┌───────────────────┬─────────────────┬────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│        DATA       │    MESSAGE_ID   │ ATTRIBUTES │                                                                               ACK_ID                                                                               │
├───────────────────┼─────────────────┼────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ Hello Pub/Sub 10m │ 522979987118502 │            │ XkASTDYORElTK0MLKlgRTgQhIT4wPkVTRFAGFixdRkhRNxkIaFEOT14jPzUgKEUUC1MTUVx1E0gQalszdQdRDRlzezB2PAhCCAMWUnRfURsfWVx-SgJTDxB1dGh9bVsSCQdFUVbFj9vsmJgaZhs9XBJLLD5-PTtFQQ │
└───────────────────┴─────────────────┴────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

メッセージ受信 (メッセージ送信から 10 分経過後)

```bash
$ gcloud pubsub subscriptions pull test-sub
┌───────────────────┬─────────────────┬────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│        DATA       │    MESSAGE_ID   │ ATTRIBUTES │                                                                               ACK_ID                                                                               │
├───────────────────┼─────────────────┼────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ Hello Pub/Sub 10m │ 522979987118502 │            │ XkASTDYORElTK0MLKlgRTgQhIT4wPkVTRFAGFixdRkhRNxkIaFEOT14jPzUgKEUUC1MTUVx1E0gQalszdQdRDRlzezB2PAhCCAMWUnRfURsfWVx-SgJTDxB1dGh9bVsSCQdFUVa-_v_56Z8aZhs9XBJLLD5-PTtFQQ │
└───────────────────┴─────────────────┴────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

$ gcloud pubsub subscriptions pull test-sub-10m
Listed 0 items.
```

topic の削除・確認

```bash
$ gcloud pubsub topics delete test-topic
Deleted topic [projects/test-gcp-app-1555841619674/topics/test-topic].

$ gcloud pubsub topics list
Listed 0 items.
```

subscription の確認 (topic が削除されている場合)

```bash
$ gcloud pubsub subscriptions list
---
ackDeadlineSeconds: 10
expirationPolicy:
  ttl: 2678400s
messageRetentionDuration: 604800s
name: projects/test-gcp-app-1555841619674/subscriptions/test-sub
pushConfig: {}
topic: _deleted-topic_
---
ackDeadlineSeconds: 10
expirationPolicy:
  ttl: 2678400s
messageRetentionDuration: 600s
name: projects/test-gcp-app-1555841619674/subscriptions/test-sub-10m
pushConfig: {}
topic: _deleted-topic_
```

subscription の削除・確認

 ```bash
$ gcloud pubsub subscriptions delete test-sub-10m
Deleted subscription [projects/test-gcp-app-1555841619674/subscriptions/test-sub-10m].

$ gcloud pubsub subscriptions list
---
ackDeadlineSeconds: 10
expirationPolicy:
  ttl: 2678400s
messageRetentionDuration: 604800s
name: projects/test-gcp-app-1555841619674/subscriptions/test-sub
pushConfig: {}
topic: _deleted-topic_

$ gcloud pubsub subscriptions delete test-sub
Deleted subscription [projects/test-gcp-app-1555841619674/subscriptions/test-sub].

$ gcloud pubsub subscriptions list
Listed 0 items.
```
