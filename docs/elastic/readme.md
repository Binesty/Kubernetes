## Restore 

```js
POST /_snapshot/snapshots-repository/workerservice-snapshot-vt0m-g3ft-wq9d57pzcplw/_restore
{
  "indices": "elastic-workerservice-*-production-*",
  "rename_pattern": "elastic-workerservice-(.*?)-production-(.*)",
  "rename_replacement": "elastic-workerservice-$1-restored-$2"
}
```