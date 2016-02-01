========== TRANSLATE FROM SQL TO ACTIVE RECORD

```
SELECT *
FROM
  users;
```
```
User.all
```


```
SELECT *
FROM
  users
WHERE
  user.id = 1;
```
```
User.find(1)
```


```
SELECT *
FROM
  posts
ORDER BY
  created_at DESC
LIMIT 1;
```
```
User.all.order(created_at: :desc).limit(1)
```



```
SELECT *
FROM
  users
JOIN
  posts
ON
  posts.author_id = users.id
WHERE
  posts.created_at >= '2014-08-31 00:00:00';
```
```
User.all.joins("JOIN posts on posts.author_id = users.id").where(created_at: >= '2014-08-31 00:00:00')
```

```
SELECT
  count(*)
FROM
  users
GROUP BY
  favorite_color
HAVING
  count(*) > 1;
```
```
User.all.select("count(*) as user_count").group(:favorite_color).having("user_count > 1")
```


* The most recently updated user
```
User.order(updated_at: desc:).first
```


* The oldest user (by age)
```
User.order(age: desc:).first
```


* all the users
```
User.all
```

* all posts sorted in descending order by date created
```
Post.all.order(created_at: desc:)
```


============= FROM ACTIVE RECORD TO SQL

Post.all
```
SELECT * FROM posts
```

Post.first
``` 
SELECT * FROM posts ORDER BY id LIMIT 1;
```

Post.last
```
SELECT * FROM posts ORDER BY id DESC LIMIT 1;
```

Post.where(:id => 4)
```
SELECT * FROM posts WHERE id = 4;
```

Post.find(4)
```
SELECT * FROM posts WHERE id = 4;
```


User.count
```
SELECT COUNT(*) FROM users;
```


Post.select(:name).where(:created_at > 3.days.ago).order(:created_at)
```
SELECT name FROM posts WHERE created_at > (NOW() - '3 days')
  ORDER BY created_at;
```


Post.select("COUNT(*)").group(:category_id)
```
SELECT COUNT(*) as post_count FROM posts
  GROUP BY category_id; 
```


All posts created before 2014
```
SELECT * FROM posts WHERE created_at <= '2014-01-01';
```


A list of all (unique) first names for authors who have written at least 3 posts
```
SELECT DISTINCT(a.first_name) FROM authors a
  JOIN posts p ON a.id = p.author_id
  WHERE COUNT(p.id) >= 3
  GROUP BY a.id;
```


The posts with titles that start with the word "The"
```
SELECT * FROM posts WHERE title LIKE 'The%';
```


Posts with IDs of 3,5,7, and 9
```
SELECT * FROM posts WHERE id IN (3,5,7,9);
```



===============================
Custom Questions
===============================
Select top 5 users who have made the most posts:

SQL:
```
SELECT * FROM users JOIN posts ON users.id = posts.user_id
GROUP BY users.id
ORDER BY COUNT(posts.id) DESC
LIMIT 5
```
ActiveRecord:
```
User.all.select('count(post_id)' as post_count).joins('JOIN posts ON user.id = posts.user_id').group(:user_id).order(post_count: :desc).limit(5)
```

Find all users with 'James' as a last name:

SQL:
```
SELECT * FROM users WHERE last_name = 'James'
```

ActiveRecord:
```
User.all.where("last_name = 'James'")
```

Find all users who have not made a post:
SQL:
```
SELECT * FROM users LEFT JOIN posts ON users.id = posts.user_id
WHERE posts.user_id IS NULL
```

ActiveRecord:
```
User.all.joins('LEFT JOIN posts ON users.id = posts.user_id').where("posts.user_id IS NULL")
```