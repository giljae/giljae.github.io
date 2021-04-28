---
layout: post
title:  "MongoDB Schema Design — Part #1"
tags: [mongodb]
comments: true
---

이제까지 MongoDB를 로그 분석용으로 주로 활용했었고 다른 용도로 사용 할 경우에 스키마를 어떻게 구성해야 하는지에 대해서 검색한 결과를 정리한다. RDBMS의 스키마 디자인과는 다른 전략으로 접근해야 하고 아래 사항을 고려해야 한다.

* User requirement 기반으로 스키마를 디자인한다.
* 데이터를 read할 때 join하는 것이 아니라 데이타를 write할때 join해야 한다.
* 객체간의 관계를 고려한다. (Multiple collection과 Embedded)

MongoDB는 아래의 방법으로 관계를 표현 할 수 있다.
<pre>
<code>
db.person.findOne() { 
  name: ‘Kate Monster’,
  ssn: ‘123–456–7890’, 
  addresses : [ 
    { 
      street: ‘123 Sesame St’, 
      city: ‘Anytown’, 
      cc: ‘USA’ 
    }, 
    { 
      street: ‘123 Avenue Q’, 
      city: ‘New York’, 
      cc: ‘USA’ 
    } 
  ] 
}
</code>
</pre>

위와 같이 embedded 방법을 쓸 경우에는 한 Query로 모든 정보를 가져 올 수 있다는 장점이 있지만, embedded details 정보만 독자적으로 가져 올 수 없다는 단점도 존재한다.
Embedded된 데이터의 크기가 증가하게 될 경우에 document의 사이즈 제한을 넘어서는 일이 생길 수 있다. 아래와 같이 part의 document가 있다고 하면,
<pre>
<code>
db.parts.findOne() { 
  _id : ObjectID(‘AAAA’), 
  partno : ‘123-aff-456’, 
  name : ‘#4 grommet’, 
  qty: 94, 
  cost: 0.94, 
  price: 3.99 
}
</code>
</pre>

각각의 Product는 하나의 document를 가지고 part document의 ObjectID를 array로 가지고 있는 구조로 구성한다.
<pre>
<code>
db.products.findOne() { 
  name : ‘left-handed smoke shifter’, 
  manufacturer : ‘Acme Corp’, 
  catalog_number: 1234, 
  parts : [ // array of references to Part documents 
    ObjectID(‘AAAA’), // reference to the #4 grommet above 
    ObjectID(‘F17C’), // reference to a different Part 
    ObjectID(‘D2AA’), // etc 
  ]
</code>
</pre>

위와 같은 경우에는 application-level join으로 두 document를 연결하여 사용해야 한다.
<pre>
<code>
// Fetch the Product document identified by this catalog number
product = db.products.findOne({catalog_number: 1234}); 

// Fetch all the Parts that are linked to this Product 
product_parts = db.parts.find({_id: { $in : product.parts } } ).toArray() ;
</code>
</pre>

각 document를 독자적으로 관리 할 수 있다는 장점이 있지만 각 document를 여러 번 호출해야 한다는 단점이 존재한다.
event logging system처럼 많은 양의 데이터를 처리해야 할 경우에는 16MB document size의 제한 때문에 위에서 언급한 방법을 사용하지 못한다. 이런 유형에서는 parent-referencing 방식을 사용해야 한다.
<pre>
<code>
db.hosts.findOne() { 
  _id : ObjectID(‘AAAB’), 
  name : ‘goofy.example.com’, 
  ipaddr : ‘127.66.66.66’ 
} 

db.logmsg.findOne() { 
  time : ISODate(“2014–03–28T09:42:41.382Z”), 
  message : ‘cpu is on fire!’, 
  host: ObjectID(‘AAAB’) // Reference to the Host document 
}
</code>
</pre>

application-level join을 사용하여 데이터를 가져온다.
<pre>
<code>
// find the parent ‘host’ document 
host = db.hosts.findOne({ipaddr : ‘127.66.66.66’}); // assumes unique index 

// find the most recent 5000 log message documents linked to that host 
last_5k_msg = db.logmsg.find({host: host._id}).sort({time : -1}).limit(5000).toArray()
</code>
</pre>

#### References:
* http://blog.mongodb.org/post/87200945828/6-rules-of-thumb-for-mongodb-schema-design-part-1
