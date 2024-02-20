How to use and set up draft resource:

1. To generate a complete, database-backed web resource, the general format for draft:resource is:

- rails generate draft:resource <MODEL_NAME_SINGULAR> <COLUMN_1_NAME:>:<COLUMN_1_DATATYPE> <COLUMN_2_NAME>:<COLUMN_2_DATATYPE> etc.

-> use this command in the terminal. 

Example:

rails generate draft:resource post title:string body:text expires_on:date board_id:integer

General format of draft:resource:
  a. The model name must be singular.
  b. Separate COLUMN NAMES and datatypes with COLONS (NO SPACES).
  c. Separate NAME:DATATYPE pairs with SPACES (NO COMMAS). 

2. Use the command: rake db:migrate -> this will execute the migrations that were written for us by the generator = create the table in the back-end. Until we use this command, we won't have the actual tables we defined.

-> What happens when we use the command: rake db:migrate 

create  app/controllers/posts_controller.rb (b)
invoke  active_record
create    db/migrate/20200602191145_create_posts.rb (a)
create    app/models/post.rb (a) 
create  app/views/posts (c)
create  app/views/posts/index.html.erb (c)
create  app/views/posts/show.html.erb (c)
  route  RESTful routes
insert  config/routes.rb (d)

-> The migration creates: look above for examples via tags.
  a. A model and a migration for the table 
  b. A controller named after the table.
  c. Some view tempaltes related to the table. 
  d. Some routes related to the table. 

3. If you made a mistake with the generated resource:

  a. Using command: rake db:rollback 
  -> if there is a mistake when generating draft:resource AND we already ran the command: rake db:migrate use this command. This will get the database back into the state it was before = command looks at the most recent migration and REVERSES IT.

  b. Using command: rails destroy draft:resource post 
  -> anything that you can: rails generate = we can also use rails destroy draft:resource post. This will REMOVE all the files that the command: rails generate draft:resource post previously added. 

4. How to add or remove columns from your table: modifying existing tables after running command: rake db:migrations
  
  a. Using command: add_column

Example 1: generating a new migration file (NOT a whole model) to ADD a column.

Use command: rails g migration AddImageToPosts

-> afterwards, navigate to the migration file and add instructions to make the change we want within the Method "change" (or the Method "up" - depends)

Example 2: 

```ruby
  def change
    add_column :posts, :image, :string
  end 
```

  b. Using command: remove_column

Example 1: generating a new migration file to REMOVE a column.

Use command: rails g migration RemoveExpiresOnFromPosts

-> afterwards, navigate to the migration file and add instructions to make the change we want within the Method "change" (or the Method "up" - depends)

Example 2: 

```ruby
  def change
    remove_column :posts, :expires_on
  end 
```
  c. Finally, use the command: rake db:migrate to implement the change.

  d. LAST RESORT - use command: rake db:drop 

-> This command will destroy the ENTIRE database and all of the data within it. It is a reset button. The rest is fixing the migration files, running command: rake db:migrate then rake sample_data to recover the sample data. 
