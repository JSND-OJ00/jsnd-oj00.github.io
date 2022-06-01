# MySQL Practice

## schema

```sql
CREATE TABLE `user` (
  `id` int PRIMARY KEY AUTO_INCREMENT,
  `name` varchar(255) not NULL,
  `email` varchar(255) not NULL
);

CREATE TABLE `content` (
  `id` int PRIMARY KEY AUTO_INCREMENT,
  `title` varchar(255) not NULL,
  `body` varchar(255) not NULL, 
  `created_at` timestamp not NULL DEFAULT CURRENT_TIMESTAMP,
  `userId` int,
  FOREIGN KEY (`userId`) REFERENCES `user` (`id`)
);

CREATE TABLE `category` (
  `id` int PRIMARY KEY AUTO_INCREMENT,
  `name`varchar(255) not NULL
);

CREATE TABLE `content_category` (
  `id` int PRIMARY KEY AUTO_INCREMENT,
  `contentId` int not NULL,
  `categoryId` int not NULL,
  FOREIGN KEY (`contentId`) REFERENCES `content` (`id`),
  FOREIGN KEY (`categoryId`) REFERENCES `category` (`id`)
);

CREATE TABLE `role` (
  `id` int PRIMARY KEY AUTO_INCREMENT,
  `name` varchar(255) not NULL
);

ALTER TABLE `user` ADD roleId int;
ALTER TABLE `user` ADD FOREIGN KEY (`roleId`) REFERENCES `role` (`id`);
```

test5

```jsx
----------------------------------------------------------------------------------------------
    TODO: Q 5-1-1. category 테이블의 구조를 보기위한 SQL을 작성해주세요.
        - 요구사항에 맞는 category 테이블을 작성해야만, 테스트를 통과합니다.
*/
const PART5_1_1 = `DESCRIBE category`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-1-2. content_category 테이블의 구조를 보기위한 SQL을 작성해주세요.
        - 요구사항에 맞는 content_category 테이블을 작성해야만, 테스트를 통과합니다.
*/
const PART5_1_2 = `DESCRIBE content_category`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-1-3. role 테이블의 구조를 보기위한 SQL을 작성해주세요.
        - 요구사항에 맞는 role 테이블을 작성해야만, 테스트를 통과합니다.
*/
const PART5_1_3 = `DESCRIBE role`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-1-4. user 테이블의 구조를 보기위한 SQL을 작성해주세요.
        - 요구사항에 맞는 user 테이블을 작성해야만, 테스트를 통과합니다.
*/
const PART5_1_4 = `DESCRIBE user`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-2-1. category 테이블에 존재하는 데이터에서 id, name을 찾는 SQL을 작성해주세요.
*/

const PART5_2_1 = `SELECT id, name FROM category`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-2-2. user의 name과 email 그리고 그 user가 속한 role name(컬럼명: roleName)을 찾기 위한 SQL을 작성해주세요.
        - 속한 role이 없더라도, user의 name과 email,role name을 모두 찾아야합니다.
*/
const PART5_2_2 = `SELECT user.name, user.email, role.name AS roleName FROM user LEFT JOIN role ON user.roleId = role.id`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-2-3. 어느 role에도 속하지 않는 user의 모든 컬럼 데이터를 찾기위한 SQL을 작성해주세요.
*/
const PART5_2_3 = `SELECT * FROM user WHERE roleId is NULL`;

/*
----------------------------------------------------------------------------------------------
    TODO: content_category 테이블의 의미를 테이블 관계로서 이해하고 있는지 점검해주세요. 질문자체가 이해가 안된다면, Help-Desk에 질문해주세요.
----------------------------------------------------------------------------------------------    
*/

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-2-4. content_category 테이블에 존재하는 모든 칼럼의 데이터를 찾기위한 SQL을 작성해주세요.
*/
const PART5_2_4 = `SELECT * FROM content_category`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-2-5. jiSungPark이 작성한 content의 title을 찾기위한 SQL을 작성해주세요.
*/
const PART5_2_5 = `SELECT title FROM content WHERE content.userId = (SELECT id FROM user WHERE user.name = 'jiSungPark')`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-2-6. JiSungPark이 작성한 content의 category name을 찾기위한 SQL을 작성해주세요.
*/
const PART5_2_6 = `SELECT category.name FROM category
JOIN content_category ON category.id = content_category.categoryId
JOIN content ON content_category.contentId = content.id
JOIN user ON content.userId = user.id
WHERE user.name = 'jiSungPark'`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-2-7. category의 name이 soccer인 content의 title, body, created_at을 찾기위한 SQL을 작성해주세요.
*/
const PART5_2_7 = `SELECT content.title, content.body, content.created_at FROM content
JOIN content_category ON content.id=content_category.contentId
JOIN category ON content_category.categoryId=category.id WHERE category.name='soccer'`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-2-8. category의 name이 soccer인 content의 title, body, created_at, user의 name을 찾기위한 SQL을 작성해주세요.
*/
const PART5_2_8 = `SELECT user.name, content.title, content.body, content.created_at FROM user
JOIN content ON user.id = content.userId
JOIN content_category ON content.id=content_category.contentId
JOIN category ON content_category.categoryId=category.id WHERE category.name='soccer'`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-2-9. duRiCha가 작성한 글의 개수 (컬럼명: ContentCount)를 출력하기 위한 SQL을 작성해주세요.
*/
const PART5_2_9 = `SELECT count(content.userId) as ContentCount 
FROM content 
JOIN user 
ON user.id = content.userId 
WHERE user.name = 'duRiCha'`;

/*
----------------------------------------------------------------------------------------------
    TODO: Q 5-2-10. 각 user(컬럼명: name)가 작성한 글의 개수 (컬럼명: ContentCount)를 출력하기 위한 SQL을 작성해주세요.
*/
const PART5_2_10 = `SELECT user.name as name, count(content.userId) as ContentCount FROM content RIGHT JOIN user ON user.id = content.userId GROUP BY user.name`;
```
