# ASSIGNMENT – 2  
## MongoDB Compass (CLI – mongosh)

---

## Question 1  

In MongoDB Compass, use the CLI to perform the following operations:

1. Add **5 student records**
   - a) Insert **1 record** using `insertOne`
   - b) Insert **4 records** using `insertMany`
2. Remove the **1st record**
3. Query all students in the **"Computer"** department
4. Update the **"Computer"** department to **"BCA"**

---

## SOLUTION  

### Database Selection

```js
use SemesterVI

switched to db SemesterVI

db.students

SemesterVI.students

SOLUTION 1

db.students.insertOne({
  roll: 1,
  name: "Amit",
  department: "Computer",
  year: 3
})

OUTPUT : 

{
  acknowledged: true,
  insertedId: ObjectId('698038219a2abc176e55b5b5')
}

{
  acknowledged: true,
  insertedId: ObjectId('698038219a2abc176e55b5b5')
}



db.students.insertMany([
  { roll: 2, name: "Saikat", department: "Computer", year: 3 },
  { roll: 3, name: "Hiya", department: "Electrical", year: 2 },
  { roll: 4, name: "Riya", department: "Computer", year: 1 },
  { roll: 5, name: "Rahul", department: "Mechanical", year: 4 }
])


OUTPUT :

{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('698038529a2abc176e55b5b6'),
    '1': ObjectId('698038529a2abc176e55b5b7'),
    '2': ObjectId('698038529a2abc176e55b5b8'),
    '3': ObjectId('698038529a2abc176e55b5b9')
  }
}

SOLUTION 2

db.students.deleteOne({ roll: 1 })


OUTPUT :

{
  acknowledged: true,
  deletedCount: 1
}

SOLUTION 3

db.students.find({ department: "Computer" })


OUTPUT :

{
  _id: ObjectId('698038529a2abc176e55b5b6'),
  roll: 2,
  name: 'Saikat',
  department: 'Computer',
  year: 3
}



{
  _id: ObjectId('698038529a2abc176e55b5b8'),
  roll: 4,
  name: 'Riya',
  department: 'Computer',
  year: 1
}

SOLUTION 4 

db.students.updateMany(
  { department: "Computer" },
  { $set: { department: "BCA" } }
)

OUTPUT :
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 2,
  modifiedCount: 2,
  upsertedCount: 0
}

OPTIONAL

db.students.find()


{
  _id: ObjectId('698038529a2abc176e55b5b6'),
  roll: 2,
  name: 'Saikat',
  department: 'BCA',
  year: 3
}



{
  _id: ObjectId('698038529a2abc176e55b5b7'),
  roll: 3,
  name: 'Hiya',
  department: 'Electrical',
  year: 2
}
