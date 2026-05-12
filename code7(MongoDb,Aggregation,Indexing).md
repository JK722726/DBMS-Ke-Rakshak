5. Program / MongoDB Commands
(A) Create a Collection and Insert Data
use StudentDB;
db.users.insertMany([
{ name: "Alice", age: 21, sales: 5000, city: "Delhi" },
{ name: "Bob", age: 25, sales: 7000, city: "Mumbai" },
{ name: "Charlie", age: 23, sales: 6500, city: "Delhi" }
]);


(B) CRUD Operations
1. Read Data
db.users.find();

2. Filtered Read
db.users.find({ city: "Delhi" });

3. Update Data
db.users.updateOne(
{ name: "Alice" },
{ $set: { sales: 5500 } }
);

4. Delete Data
db.users.deleteOne({ name: "Charlie" });


(C) Aggregation Queries
1. Calculate Average Age
db.users.aggregate([
{ $group: { _id: null, average_age: { $avg: "$age" } } }
]);

2. Total Sales
db.users.aggregate([
{ $group: { _id: null, total_sales: { $sum: "$sales" } } }
]);

3. Sales by City
db.users.aggregate([
{ $group: { _id: "$city", total_city_sales: { $sum: "$sales" } } }
]);


(D) Index Creation
db.users.createIndex({ city: 1 });

Checking Indexes
db.users.getIndexes();
