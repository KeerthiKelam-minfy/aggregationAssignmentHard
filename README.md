# aggregationAssignmentHard

<pre>
  Q1:

  Code:
db.products.aggregate([
    {
        $group: {
            _id: "$category",
            totalInventoryValue: {
                $sum: { $multiply: ["$price", "$quantity"] }
            }
        }
    },
    {
        $project: {
            _id: 0,
            categoryName: "$_id",
            totalInventoryValue: 1,
            valueClassification: {
                $switch: {
                    branches: [
                        {
                            case: {$gt: ["$totalInventoryValue", 10000]},
                            then: "High Value"
                        },
                        {
                            case: {
                                $and: [
                                    { $gt: ["$totalInventoryValue", 5000] },
                                    { $lte: ["$totalInventoryValue", 10000] }
                                ]
                            },
                            then: "Medium Value"
                        }
                    ],
                     default: "Standard Value"
                }
            }
        }
    }
])
</pre>

**Output:  **

![image](https://github.com/user-attachments/assets/9bed588a-52af-485c-96b6-eaa5c87aa26f)




