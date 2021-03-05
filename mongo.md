# Remove duplicate documents from collection

`db = db.getSiblingDB**(**"walmart_dev"**);**

db.getCollection**(**"ReportOverview"**)**.aggregate**(**

  **[**

​    **{** 

​      "$group" **:** **{** 

​        "_id" **:** "$reportPermissionKey"**,** 

​        "dups" **:** **{** 

​          "$addToSet" **:** "$_id"

​        **},** 

​        "count" **:** **{** 

​          "$sum" **:** 1.0

​        **}**

​      **}**

​    **},** 

​    **{** 

​      "$match" **:** **{** 

​        "count" **:** **{** 

​          "$gt" **:** 1.0

​        **}**

​      **}**

​    **}**

  **],** 

  **{** 

​    "allowDiskUse" **:** false

  **}**

**)**.forEach**(**function**(**doc**)** **{**

  doc.dups.shift**();**

  db.ReportOverview.remove**({**

​    _id**:** **{**$in**:** doc.dups**}**

  **});**

**})**`

