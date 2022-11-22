## CRUD

### CREATE - 생성하기

- db.콜렉션명.insert(객체) : 복수의 객체를 넣을 수 있음.
- 모델의 new 키워드 사용

```javascript
var Todo = mongoose.model("Todo", todoSchema);
var todo = new Todo({
  todoid: 1,
  content: "MongoDB",
  completed: false,
});

todo.save();
```

create 메소드 사용

```javascript
const newUser = await User.create({
  email,
  name,
});
```

### READ - 읽기

- db.콜렉션명.find()
- db.콜렉션명.findOne()
- db.콜렉션명.find().sort( { "\_id": 1 } ) : 오름차순은 1, 내림차순은 -1로 쓸 것.

```javascript
Users.find(조건, 프로젝션, 콜백); // 조건에 해당하는 모든 것을 쿼리
Users.findOne(조건, 프로젝션, 콜백); // 조건에 해당하는 첫 번째 것을 쿼리
Users.findById(아이디, 프로젝션, 콜백);
```

```javascript
User.find(
  {
    name: "zerocho",
    birth: { $gt: 20 },
    role: { $in: ["owner", "admin"] },
  },
  {
    name: 1,
    birth: 1,
    medals: 1,
  }
)
  .sort({ medals: -1 })
  .limit(5);
```

### UPDATE - 수정하기

- db 내 변수 수정
- db.콜렉션명.update(탐색기준, 수정할 변수명)
- findOneAndUpdate, findByIdAndUpdate 메소드를 쓴다.

```javascript
Users.findOneAndUpdate(조건, 변경, 옵션, 콜백);
Users.findByIdAndUpdate(아이디, 변경, 옵션, 콜백);
```

### DELETE - 삭제하기

- db.콜렉션명.remove(탐색기준)
- db.bios.remove( { } ) : 전부 삭제하기
- findOneAndRemove, findByIdAndRemove 메소드를 쓴다.

```javascript
Users.findOneAndRemove(조건, 콜백);
Users.findByIdAndRemove(아이디, 콜백);
```

## 그 외

### Populate : 채우다.

용도 : ref된 객체들의 데이터를 불러오기 위해 사용함.

**주의사항 : populate된 데이터로 값을 조회하기 위해선 ObjectID로 조회하여야 한다.**

```javascript
const userSchema = new mongoose.Schema({
  name: String,
  friends: [{ type: mongoose.Schema.Types.ObjectId, ref: 'User' }],
  bestFriend: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }],
});
mongoose.model('User', userSchema);
```

모델명.find({기준}).populate(필드명).

모델명.findById(id값).populate(필드명).

⇒ populate하고 싶은 값의 필드명을 적어주며, 해당 필드명은 populate하고 싶어하는 모델로 ref 설정이 되어 있다. 이 관계는 1:1일 필요가 없다!

```javascript
User.findOne({ name: "zero" })
  .populate("bestFriend")
  .exec((err, data) => {
    console.log(data);
  }); // 또는 populate({ path: 'bestFriend' })도 가능
```

## 제한자 사용하기 (with find method)

### 크기 비교

$gt, $gte : 특정 값보다 큰 값들을 검색할 경우, 해당 값보다 크거나 같은 값을 갖는 필드를 찾을 경우

$lt, $lte : 특정 값보다 작은 값들을 찾을 경우, 작거나 같은 값을 찾는 경우

```
/* 5 초과 10 미만 검색 */
db.posts.find( {"awesome": {"$gt": 5, "$lt": 10}} )
```

### 논리비교

$ne, $eq : Not Equal, Equal

```
/* Not Equal */
db.posts.find( {"title": {"$ne": "MongoDB"}} )
```

$or : 여러 조건 중 적어도 하나를 만족하는 다큐먼트를 찾을 경우

$and : 여러 개의 조건을 모두 만족하는 다큐먼트를 찾을 경우

$nor : 모두 다 만족하지 않는 경우

```javascript
db.users.find().all([{ name: "zerocho" }, { age: 24 }]); // 이름이 zerocho고 나이가 24면
db.users.find().or([{ name: "zerocho" }, { name: "babo" }]); // 이름이 zerocho거나 babo면
```

$in : 배열 안의 값 중 하나라도 일치하는지 여부 따짐
$nin : 하나도 일치하지 않는지 따짐
$all : 모두 다 일치하는 지를 따짐

### 합계

$sum : 각 문서에서 해당 필드의 값을 합함.

### 날짜

$year : 연 데이터를 반환해줌.
$month : 월 데이터를 반환해줌.
$day : 일 데이터를 반환해줌.

### 기타

$inc : 갱신 제한자를 통해, 카운터를 증가시킬 수 있다.
$set : 필드(콜렉션 내 key 값)의 값만을 변경한다.

> db.[콜렉션명].update({ name:"cake" }, { $set: {price: 10000} });

```javascript
db.zip.aggregate([
  { $group: { _id: "$state", total_pop: { $sum: "$pop" } } },
  { $limit: 5 },
]);
```

### aggregate

Model.aggregate does not cast it's arguments. You'll need to create instances of Date.

입력값 : $필드명을 통해 처리할 수 있음.

$match : 조건에 만족하는 문서만 filtering한다.

$group : 문서에 대한 gruping 연산을 한다. 또한 Group에 대한 id를 지정해야하고, 특정 필드에 대한 집계 연산이 가능하다.

```javascript
db.orders.aggregate([{
	$match : { status: "A"} },
// 해당 조건에 부합하는 document들을 필터링해줌.
	$group : { _id: "$cust_id", total: { $sum: "$amount" } } }
// 여러 개의 document를 하나로 합치는 역할을 함.
]);
```

```javascript
// DATA

{"_id": 1, "item": "abc", "price": 10, "quantity" : 2, "date": ISODate("2014-01-01T08:00:00Z")}
{"_id": 2, "item": "jkl", "price": 20, "quantity" : 1, "date": ISODate("2014-02-03T09:00:00Z")}
{"_id": 3, "item": "xyz", "price": 5, "quantity" : 5, "date": ISODate("2014-02-03T09:05:00Z")}
{"_id": 4, "item": "abc", "price": 10, "quantity" : 10, "date": ISODate("2014-02-15T08:00:00Z") }
{"_id": 5, "item": "xyz", "price": 5, "quantity" : 10, "date": ISODate("2014-02-15T09:05:00Z")}

// QUERY

db.sales.aggregate(
   [
     {
       $group:
         {
           _id: { day: { $dayOfYear: "$date"}, year: { $year: "$date" } },
           totalAmount: { $sum: { $multiply: [ "$price", "$quantity" ] } },
           count: { $sum: 1 }
         }
     }
   ]
)

// RESULT

{ "_id" : { "day" : 46, "year" : 2014 }, "totalAmount" : 150, "count" : 2 }
{ "_id" : { "day" : 34, "year" : 2014 }, "totalAmount" : 45, "count" : 2 }
{ "_id" : { "day" : 1, "year" : 2014 }, "totalAmount" : 20, "count" : 1 }
```

mongoose에서의 몽고db의 집계함수 실행은 mongodb에서의 쿼리와 형태가 거의 같다.

```javascript
모델명.aggregate(
  [
    {
      $group: {
        _id: {
          random_text1: "$random_text1", //매핑할 대상 및 결과 이름
          random_text2: "$random_text2",
          date: "$date",
        },
        count: { $sum: 1 },
      },
    },
  ],
  function (rr, ra) {
    if (ra) {
      console.log(ra);
    }
  }
);
```

### group

대부분의 RDBMS는 합계, 평균, 편차와같은 내장 집계함수를 많이 제공하지만, Mongodb에는 구현될 때까지 group,map-reduce를 사용해야한다.

group()은 최소한 3개의 파라미터를 받는다.

첫번째 파라미터 key는 **데이터를 어떻게 그룹으로 묶을것인지를 지정**한다.

두번째 파라미터는 **리듀스(reduce funciton) 함수**로, **결과 값에 대해 그룹으로 묶는 자바스크립트 함수**다.

세번짜 파라미터는 **각 키의 값에 대해 처음 반복할 때 리듀스 함수에게 제공하는 초기 도큐먼트**다.

key : 그룹으로 나누는 기준이 되는 키를 표현한 도큐먼트. 복합키도 가능.

keyf : 그룹을 위한 키를 계산을 통해 만들어야 하는경우 필요한 자바스크립트함수. key를 지정하지 않으면 반드시 필요.

initial : 집계 결과의 초기값으로 사용되는 도큐먼트.(필수)

reduce : 집계 기능을 수행하는 함수.현재 도큐먼트와, 집계결과를 저장하는 도큐먼트 2개의 파라미터를 받는다.리턴할 필요가 없다.(필수)

cond : 집계를 수행하기 위한 도큐먼트를 필터링 하는 쿼리 셀렉터.

finalize : 결과값을 리턴하기전 각 도큐먼트에 적용되는 함수.

```javascript
db.reviews.group({
  key: { user_id: true },
  initial: { reviews: 0, votes: 0.0 },
  reduce: function (doc, aggregator) {
    aggregator.reviews += 1;
    aggregator.votes += doc.votes;
  },
  finalize: function (doc) {
    doc.average_votes = doc.votes / doc.reviews;
  },
});
```

### 맵-리듀스

- 맵-리듀스는 group의 좀더 유연한 버전으로 생각할 수 있다.
- 그룹 키에 대한 세밀한 제어를 할수 있다.
- 새 컬렉션에 결과를 저장하는 것 같은 다양한 출력 옵션을 가질 수 있다.
- 맵-리듀스는 샤드 구성에서 대량의 데이터를 반복 처리하기 위해 필요한 분산된 어그리 게이터를 지원한다.
- map : 각 도큐먼트에 적용하는 자바스크립트 함수. 집계에 사용 할 키와 값을 선택하기 위해 emit()을 의무적으로 호출 해야함.
- reduce : 키와 값의 리스트를 받는 자바스크립트 함수. 반드시 받은 값과 같은 구조의 값을 리턴 해야함.
- query : 매핑될 컬렉션을 필터하는 퀄리 셀렉터, group의 cond와 같은 기능.
- sort : 쿼리에 적용할 정렬 조건. limit옵션과 사용할때 유용
- limit : 쿼리와 정렬에 적용되어 결과 값의 개수를 제한하는 정수.
- out : 결과가 어떻게 리턴되는지를 결정.(자세한 설명은 134p참조)
- finalize : 리듀스가 완료된 후 각 결과 도큐먼트에 적용될 자바스크립트 함수.
- scope : map, reduce, finalize 함수에 의해 액세스 되는 전역 변서의 값을 지정하는 도큐먼트.
- verbose : true일 경우 맵-리듀스 실행 시간에 대한 통계 데이터를 리턴.

```javascript
//1. 2010년 이후 월 판매액을 구하여 total컬렉션에 저장하기
map = function () {
  var shipping_month =
    this.purchase_date.getMonth() + "-" + this.purchase_date.getFullYear();
  var tmpItems = 0;
  this.line_items.forEach(function (item) {
    tmpItems += item.quantity;
  });

  emit(shipping_month, { order_total: this.sub_total, items_total: tmpItems });
};

reduce = function (key, values) {
  var tmpTotal = 0;
  var tmpItems = 0;

  tmpTotal += doc.order_total;
  tmpItems += doc.items_total;

  return { total: tmpTotal, items: tmpItems };
};

filter = { purchase_date: { $gte: new Date(2010, 0, 1) } };

db.orders.mapReduce(map, reduce, { query: filter, out: "totals" });
```
