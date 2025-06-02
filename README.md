# aggregationAssignmentHard

<pre>
1. Category Value and Classification:

- **Task:** For each `category`, calculate its total inventory value (sum of `price * quantity` for all products in that category). Then, classify each category: if total value > 10000, it's "High Value"; if > 5000, it's "Medium Value"; otherwise, it's "Standard Value".
- **Output:** Display `category` (as `categoryName`), `totalInventoryValue`, and `valueClassification`.
- **Hint:** First `$group` by category to sum `price * quantity`. Then use `$project` with `$cond` (possibly nested or using `$switch`) for classification.
  
</pre>


<pre>


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




