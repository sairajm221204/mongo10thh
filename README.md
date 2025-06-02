# mongo10thh
Develop an aggregation pipeline to illustrate Text search on Catalog data collection.

Sample catalog Data Collection

// catalog collection
{
  "_id": 1,
  "title": "Introduction to Python",
  "description": "This book covers the basics of Python programming including syntax, data types, and functions."
}
{
  "_id": 2,
  "title": "Advanced Python Techniques",
  "description": "Covers advanced topics in Python such as decorators, generators, and metaclasses."
}
{
  "_id": 3,
  "title": "Learning JavaScript",
  "description": "Start your web development journey with JavaScript and explore its features."
}



Create a Text Index
db.catalog.createIndex({
  title: "text",
  description: "text"
});

Aggregation Pipeline with $match + $text

db.catalog.aggregate([
  {
    $match: {
      $text: { $search: "Python" }
    }
  },
  {
    $project: {
      title: 1,
      description: 1,
      score: { $meta: "textScore" }
    }
  },
  {
    $sort: {
      score: -1
    }
  }
]);

Output
[
  {
    "_id": 1,
    "title": "Introduction to Python",
    "description": "...",
    "score": 1.5
  },
  {
    "_id": 2,
    "title": "Advanced Python Techniques",
    "description": "...",
    "score": 1.2
  }
]
