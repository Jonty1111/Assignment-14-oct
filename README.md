# Assignment-14-oct



1.Write a query to find the person whose name is 'Steve'
Ans:
db.Employee.find({name:"Steve"})

2.Write a query to display the fields name, age and phone with only personal and work for all the documents in the collection employee.
Ans:
db.employee.find({},{name:1,age:1,phone:{personal:1,work:1}})

3.Write a query to find an employee with the privileges contain 'min' as three letters somewhere in his name using regex.
Ans:
db.Employee.find({privileges:{$regex:"min"}}).pretty()

4.Write a query to find the different kinds of points available in points collection under employee collection?
Ans:
db.Employee.distinct("points.points")

5.Write a query to find the count of total number of points of all the employees in the collection ? 
Ans:
db.Employee.aggregate([
  {
    "$addFields": {
      "total": {
        "$add": [
          {
            "$sum": "$points.points"
          },
          {
            "$sum": "$points.bonus"
          }
        ]
      }
    }
  }
])



6.Write a query to which employees got atleast 60 total points including bonus from the collection ?
Ans:
db.Employee.aggregate([
  {
    "$addFields": {
      "total": {
        "$add": [
          {
            "$sum": "$points.points"
          },
          {
            "$sum": "$points.bonus"
          }
        ]
      }
    }
  },
  {
      $match:{total:{$gte:60}}  }
])



7.Update all the employees who got more than 90 total points to a badge "vladimers"
Ans:db.Employee.aggregate([
  {
    "$addFields": {
      "total": {
        "$add": [
          {
            "$sum": "$points.points"
          },
          {
            "$sum": "$points.bonus"
          }
        ]
      }
    }
  },
  {
      $match:{total:{$gte:90}}
  },
  {
      $set:{ badges: ["vladimers"]}
  }
])



8.Update any one employee by changing his privileges from a string to an array ["user", "admin"]
Ans:
db.Employee.updateMany({
    _id: {
        $in: [
             ObjectId("6348f22b186077f15233f488")
            ]
    }
},
{
    $set:{
        privileges:["users","admin"]
        
    }
}
)


9.Update employee whose favorite food is pizza and points bonus as 8 to with badge "foodie" adding to the array 
Ans:
//In this we get our ObjectId 
db.Employee.find({'favorites.food':"pizza",'points.bonus':8
})
//we have extracted this objectid using above query and then do the updation according to requirement in below query
db.Employee.updateMany({    _id: {        $in: [                ObjectId("6348f22b186077f15233f48d")            ]    }
},
{    $addToSet:{badges:"foodie"}
}
)


10.Select employees who finished 20 in total from the array
Ans:db.Employee.aggregate([
    {
        $addFields:{
            Total:{
                $sum:"$finished"
                
            }
        }
    },
    {
        $match:{Total:20}
    }
   ])
