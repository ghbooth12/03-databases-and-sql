1. remove duplicates in a query response

  * SQL statement
  ```bash
  SELECT DISTINCT title FROM post;

  # result set
  title              
  -------------------
  the Drinkin'' groud
  Deep in the Heart o
  Paleo poutine willi
  About Computer  
  ```

  * ORM
  ```bash
  Post.select(:title).distinct
  ```


2. filter records using inequalities, pattern matching, ranges, and boolean logic
  * inequalities
    * SQL statement
    ```bash
    SELECT * FROM user WHERE email != 'caryn.mclaughlin@email.com';

    # result set
    id          full_name   email             
    ----------  ----------  ------------------
    1           John Doe    john.doe@email.com
    2           Wayne Cox   wayne.cox@email.co
    ```
    * ORM
    ```bash
    User.where.not(email: 'caryn.mclaughlin@email.com')

    or

    User.find_each {|user| user.email != 'caryn.mclaughlin@email.com'}
    ```

  * patter matching
    * SQL statement
    ```bash
    SELECT * FROM user where full_name LIKE '%ox%';

    # result set
    id          full_name   email              
    ----------  ----------  -------------------
    2           Wayne Cox   wayne.cox@email.com
    ```
    * ORM
    ```bash
    User.find_each {|user| user.full_name.include?('ox')}
    ```

  * ranges
    * SQL statement
    ```bash
    SELECT * FROM user WHERE full_name BETWEEN 'A' AND 'D';

    # result set
    id          full_name         email                     
    ----------  ----------------  --------------------------
    3           Caryn McLaughlin  caryn.mclaughlin@email.com
    ```
    * ORM
    ```bash
    User.where(full_name: 'A'..'D')
    ```

  * boolean logic
    * SQL statement
    ```bash
    SELECT * FROM user WHERE id < 2 OR full_name = 'Caryn McLaughlin';

    # result set
    id          full_name   email             
    ----------  ----------  ------------------
    1           John Doe    john.doe@email.com
    3           Caryn McLa  caryn.mclaughlin@e
    ```
    * ORM
    ```bash
    User.find_each do |user|
      user.id < 2 || full_name = 'Caryn McLaughlin'
    end
    ```

3. sort records in a particular order

  * DESC
    * SQL statement
    ```bash
    SELECT * FROM user ORDER BY full_name DESC;

    # result set
    id          full_name   email              
    ----------  ----------  -------------------
    2           Wayne Cox   wayne.cox@email.com
    1           John Doe    john.doe@email.com
    3           Caryn McLa  caryn.mclaughlin@em
    ```

    * ORM
    ```bash
    User.order(full_name: :desc)
    ```

  * ASC
    * SQL statement
    ```bash
    SELECT * FROM user ORDER BY full_name ASC;

    # result set
    id          full_name         email                     
    ----------  ----------------  --------------------------
    3           Caryn McLaughlin  caryn.mclaughlin@email.com
    1           John Doe          john.doe@email.com        
    2           Wayne Cox         wayne.cox@email.com  
    ```

    * ORM
    ```bash
    User.order(full_name: :asc)
    ```

4. limit the number of records returned

  * SQL statement
  ```bash
  SELECT * FROM post WHERE user_id = 2 LIMIT 2;

  # result set
  id          title                body                                              user_id   
  ----------  -------------------  ------------------------------------------------  ----------
  1           the Drinkin'' groud  When the sun goes down Follow the drinkn'' groud  2         
  2           Deep in the Heart o  The stars at night are big and bright, Deep in t  2
  ```

  * ORM
  ```bash
  Post.find(user_id: 2).limit(2)
  ```

5. group records into sections

  * SQL statement
  ```bash
  SELECT user_id, COUNT(*) FROM post GROUP BY user_id;

  # result set
  user_id     COUNT(*)  
  ----------  ----------
  1           1         
  2           3         
  3           2        
  ```

  * ORM
  ```bash
  Post.select(user_id, count(\*)).group(user_id)
  ```

6. perform calculations using aggregate functions
  aggregate functions: COUNT(), SUM(), MAX(), MIN(), AVG(), ROUND()

  * SQL statement
  ```bash
  SELECT id, MAX(full_name), email FROM user;

  # result set
  id          MAX(full_name)  email              
  ----------  --------------  -------------------
  2           Wayne Cox       wayne.cox@email.com
  ```

  * ORM
  ```bash
  User.count(:full_name)
  Inventory.sum(:quantity)
  Inventory.maximum(:quantity)
  Inventory.minimum(:quantity)
  Inventory.average(:quantity)
  ```



7. join tables using cross join, inner join, and outer join

  * cross join: It is not very useful. It combines every row of the `post` table with every row of the `user` table.
    * SQL statement
    ```bash
    SELECT post.title, user.full_name FROM post, user;

    # result set
    title                full_name
    -------------------  ----------
    the Drinkin'' groud  John Doe  
    the Drinkin'' groud  Wayne Cox
    the Drinkin'' groud  Caryn McLa
    Deep in the Heart o  John Doe  
    Deep in the Heart o  Wayne Cox
    Deep in the Heart o  Caryn McLa
    Paleo poutine willi  John Doe  
    Paleo poutine willi  Wayne Cox
    Paleo poutine willi  Caryn McLa
    About Computer       John Doe  
    About Computer       Wayne Cox
    About Computer       Caryn McLa
    About Computer       John Doe  
    About Computer       Wayne Cox
    About Computer       Caryn McLa
    About Computer       John Doe  
    About Computer       Wayne Cox
    About Computer       Caryn McLa
    ```

    * ORM
    ```bash
    ?
    ```

  * Inner Join
    * SQL statement
    ```bash
    SELECT post.title, user.full_name FROM post JOIN user ON post.user_id = user.id;

    # result
    title                full_name
    -------------------  ----------
    the Drinkin'' groud  Wayne Cox
    Deep in the Heart o  Wayne Cox
    Paleo poutine willi  Caryn McLa
    About Computer       John Doe  
    About Computer       Wayne Cox
    About Computer       Caryn McLa
    ```

    * ORM
    ```bash
    User.joins(:posts)
    ```

  * Outer Join
    * SQL statement
    ```bash
    SELECT post.title, user.full_name FROM post LEFT JOIN user ON post.user_id = user.id;

    # result
    title                full_name
    -------------------  ----------
    the Drinkin'' groud  Wayne Cox
    Deep in the Heart o  Wayne Cox
    Paleo poutine willi  Caryn McLa
    About Computer       John Doe  
    About Computer       Wayne Cox
    About Computer       Caryn McLa
    Some Post Title
    ```

    * ORM
    ```bash
    User.left_outer_joins(:posts)
    ```
