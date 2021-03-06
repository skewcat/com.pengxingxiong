MongoDB支持客户端灵活配置写入策略（[writeConcern](https://docs.mongodb.com/manual/reference/write-concern/)），以满足不同场景的需求。

```javascript
db.collection.insert({x: 1}, {writeConcern: {w: 1}})
```

## writeConcern选项

MongoDB支持的WriteConncern选项如下

1. w: 数据写入到number个节点才向用客户端确认
   - {w: 0} 对客户端的写入不需要发送任何确认，适用于性能要求高，但不关注正确性的场景
   - {w: 1} 默认的writeConcern，数据写入到Primary就向客户端发送确认
   - {w: “majority”} 数据写入到副本集大多数成员后向客户端发送确认，适用于对数据安全性要求比较高的场景，该选项会降低写入性能
2. j: 写入操作的journal持久化后才向客户端确认
   - 默认为”{j: false}，如果要求Primary写入持久化了才向客户端确认，则指定该选项为true
3. wtimeout: 写入超时时间，仅w的值大于1时有效。
   - 当指定{w: }时，数据需要成功写入number个节点才算成功，如果写入过程中有节点故障，可能导致这个条件一直不能满足，从而一直不能向客户端发送确认结果，针对这种情况，客户端可设置wtimeout选项来指定超时时间，当写入过程持续超过该时间仍未结束，则认为写入失败。

## {w: “majority”}解析

{w: 1}、{j: true}等writeConcern选项很好理解，Primary等待条件满足发送确认；但{w: “majority”}则相对复杂些，需要确认数据成功写入到大多数节点才算成功，而MongoDB的复制是通过Secondary不断拉取oplog并重放来实现的，并不是Primary主动将写入同步给Secondary，那么Primary是如何确认数据已成功写入到大多数节点的？

1. Client向Primary发起请求，指定writeConcern为{w: “majority”}，Primary收到请求，本地写入并记录写请求到oplog，然后等待大多数节点都同步了这条/批oplog（Secondary应用完oplog会向主报告最新进度)。
2. Secondary拉取到Primary上新写入的oplog，本地重放并记录oplog。为了让Secondary能在第一时间内拉取到主上的oplog，find命令支持一个[awaitData的选项](https://docs.mongodb.com/manual/reference/command/find/#dbcmd.find)，当find没有任何符合条件的文档时，并不立即返回，而是等待最多maxTimeMS(默认为2s)时间看是否有新的符合条件的数据，如果有就返回；所以当新写入oplog时，备立马能获取到新的oplog。
3. Secondary上有单独的线程，当oplog的最新时间戳发生更新时，就会向Primary发送replSetUpdatePosition命令更新自己的oplog时间戳。
4. 当Primary发现有足够多的节点oplog时间戳已经满足条件了，向客户端发送确认。

## 参考资料

- [MongoDB云数据库](https://www.aliyun.com/product/mongodb)
- [find command](https://docs.mongodb.com/manual/reference/command/find/#dbcmd.find)
- [writeConcern](https://docs.mongodb.com/manual/reference/write-concern/)