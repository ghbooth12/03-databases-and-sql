## Written Answers

1. What's a RubyGem and why would you use one?

  **What's a RubyGem**
    * RubyGems is a package manager for Ruby.
    * It provides a standard format for distributing Ruby programs and libraries in a "Gem"(a self-contained format).
    * The interface for RubyGems is a command-line tool called gem which can install libraries and manage RubyGems.
    * RubyGems integrates with Ruby run-time loader to help find and load installed gems from standardized library folders.

  **Why would you use RubyGem?**
    * It easily manages the installation of gems, and a server for distributing them.

2. What's the difference between lazy and eager loading?

  **Lazy loading**
    * related objects (child objects) are not loaded automatically with its parent object until they are requested.

    e.g. Load only the displayed images on page load and load the others if/when they are required.

  **Eager loading**
    * related objects (child objects) are loaded automatically with its parent object.

    e.g. Load every single image required before you render the page.

3. What's the difference between the CREATE TABLE and INSERT INTO SQL statements?

  **CREATE TABLE**
    * CREATE TABLE is a clause and tells SQL to create a new table.
    * This statement is followed by table name and a list of parameters defining each column in the table and its data type.

    ```bash
    CREATE TABLE user (
      id INTEGER PRIMARY KEY,
      name TEXT,
      age INTEGER
      );
    ```

  **INSERT INTO**
    * INSERT INTO is a clause and adds the specified row or rows to the table.
    * This statement is followed by table name and a parameter identifying the columns that data will be insert into. Then it is followed by VALUES clause and a parameter identifying the values being inserted.

    ```bash
    INSERT INTO user (name, age) VALUES (
      "John Doe",
      30
    );
    ```

4. What's the difference between extend and include? When would you use one or the other?

  **include**
    * It adds instance methods to a class.
    * It is used when an object needs to call a method such as "save". So many different objects can call "save" every time when needed.

  **extend**
    * It adds class methods to a class.
    * It is used when a class itself needs to call a method such as "create". An object couldn't call "create" because the object wouldn't exist yet.

  ```ruby
  module Say
    def greet
      puts "Hi"
    end
  end

  class Animal
    include Say  # instance can call greet.
  end

  class Plant
    extend Say  # Plant class can call greet.
  end

  # include: adds instance method
  dog = Animal.new
  dog.greet  # Hi
  Animal.greet # undefined method `greet' for Animal:Class

  # extend: adds class method
  kale = Plant.new
  kale.greet # undefined method `greet' for #<Plant:0x007fa30826eac8>
  Plant.greet # Hi
  ```

5. In persistence.rb, why do the save methods need to be instance (vs. class) methods?

  Because "save" method would be called on the object. So many different objects can call "save" and all those different objects which is data can be save individually.

  If "save" was a class method, only class could save a new data/change. It couldn't save many different data. It would only save 'a' new data.


6. Given the Jar-Jar Binks example earlier, what is the final SQL query in persistence.rb's save! method?

  class | character_name | star_rating
  ------|----------------|------------
  Character | "Jar-Jar Binks" | 1

  ```bash
  UPDATE character
  SET character_name='Jar-Jar Binks',star_rating=1
  WHERE id = 1;
  ```

7. AddressBook's entries instance variable no longer returns anything. We'll fix this in a later checkpoint. What changes will we need to make?

  `@entries` will be an instance(object) of the `Base` class. So this object can create table and data using SQL. Because the `Base` class is defined inside `BlocRecord` module, which is like the `ActiveRecord`. So the instances of the `Base` class have abilities that the `Base` class have. These instances can save data and then it adds to a table as a row.

---------------------------------------------------------

## Coding Answers

1) Write a Ruby method that converts snake_case to CamelCase using regular expressions.
```ruby
def to_camel_case(snake_case)
  string = snake_case.gsub(/\//, '::')
  string.gsub!(/[a-z\d]+_/) {|match| match.capitalize}
  string.gsub!(/_[a-z\d]+/) {|match| match[1..-1].capitalize}
  string.tr!('_', '-')
  string
end
```
2) Assume we add the ability to relate an AddressBook to an Entry. Once this happens, we'll need to find all Entry records given an AddressBook.
  * Assuming you have an AddressBook, it might get called like this:

  ```ruby
  entries = Entry.find_by("address_book_id", address_book.id)
  ```
  * Your code should use a SELECT…WHERE SQL query and return an array of Entry objects to the caller.

  ```ruby
  def find_by(attribute, value)
    row = connection.get_first_row(<<-SQL)
      SELECT #{columns.join(',')} FROM entry
      WHERE #{attribute} = #{value};
    SQL

    row
  end
  ```
