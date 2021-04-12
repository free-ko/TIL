# Elastic Search

## 1. Query Sorted

- ES에서 이미 셋팅되어 있는 \_id가 아닌 그 안에 있는 필드의 속성에 접근해야 됨
- ex) `_id.dynamic.product_name : asc; // 이렇게 하면 적용 안됨`
- ex) `dynamic.product_name : asc; // 이렇게 하면 적용 됨`

```JSON
{
   "sort" : [
        { "age" : "desc" },
        { "balance" : "desc" },
        "_score"
    ],
   "query":{
      "match_all":{}
   }
}
```
