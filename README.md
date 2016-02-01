TRANSLATE FROM SQL TO ACTIVE RECORD


SELECT *
FROM
  users;

User.all




SELECT *
FROM
  users
WHERE
  user.id = 1;

User.find(1)




SELECT *
FROM
  posts
ORDER BY
  created_at DESC
LIMIT 1;

User.all.order(created_at: :desc).limit(1)





SELECT *
FROM
  users
JOIN
  posts
ON
  posts.author_id = users.id
WHERE
  posts.created_at >= '2014-08-31 00:00:00';


User.all.joins("JOIN posts on posts.author_id = users.id").where(created_at: >= '2014-08-31 00:00:00')



SELECT
  count(*)
FROM
  users
GROUP BY
  favorite_color
HAVING
  count(*) > 1;

User.all.select("count(*) as user_count").group(:favorite_color).having("user_count > 1")



* The most recently updated user

User.order(updated_at: desc:).first



* The oldest user (by age)

User.order(age: desc:).first



* all the users

User.all


* all posts sorted in descending order by date created

Post.all.order(created_at: desc:)