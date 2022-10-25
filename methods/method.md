Book Store Model and Repository Classes Design Recipe

1. Design and create the Table

2. Create Test SQL seeds

TRUNCATE TABLE books RESTART IDENTITY; -- replace with your own table name.

INSERT INTO books (title, author_name) VALUES ('Emma', 'Jane Austen');
INSERT INTO books (title, author_name) VALUES ('Dracula', 'Bram Stoker');

psql -h 127.0.0.1 your_database_name < seeds_{table_name}.sql

3. Define the class names

# EXAMPLE
# Table name: books

# Model class
# (in lib/book.rb)
class Book
end

# Repository class
# (in lib/book_repository.rb)
class BookRepository
end
4. Implement the Model class
Define the attributes of your Model class. You can usually map the table columns to the attributes of the class, including primary and foreign keys.

# EXAMPLE
# Table name: students

# Model class
# (in lib/student.rb)

class Book
  # Replace the attributes by your own columns.
  attr_accessor :id, :title, :author_name
end

5. Define the Repository Class interface

# EXAMPLE
# Table name: books

# Repository class
# (in lib/book_repository.rb)

class BookRepository

  # Selecting all records
  # No arguments
  def all
    # Executes the SQL query:
    # SELECT id, title, author_name FROM books;

    # Returns an array of Book objects.
  end
end

6. Write Test Examples


# EXAMPLES

# 1
# Get all books

repo = BookRepository.new

books = repo.all

books.length # =>  2
books.first.id #=> "1"
books.first.title #=> "Emma"

7. Reload the SQL seeds before each test run
Running the SQL code present in the seed file will empty the table and re-insert the seed data.

This is so you get a fresh table contents every time you run the test suite.

# EXAMPLE

# file: spec/student_repository_spec.rb

def reset_students_table
  seed_sql = File.read('spec/seeds_students.sql')
  connection = PG.connect({ host: '127.0.0.1', dbname: 'students' })
  connection.exec(seed_sql)
end

describe StudentRepository do
  before(:each) do 
    reset_students_table
  end

  # (your tests will go here).
end
8. Test-drive and implement the Repository class behaviour
After each test you write, follow the test-driving process of red, green, refactor to implement the behaviour.

