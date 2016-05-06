# Active Record

## Learning Objectives
- Define ORM and why we use it over a database language.
- Explain what Active record is and what problems it solves.
- Explain convention over configuration and how it relates to Active Record
- Define a class that inherits from AR
- Utilize AR to perform the following CRUD actions on a database
  - create
  - new
  - save
  - all
  - find, find_by
  - where
  - update
  - destroy
- Differentiate between class versus instant methods in Active Record objects/classes
- Utilize `has_many`, `belongs_to` to establish associations/relationships with AR
- Seed a database using AR


## Framing (5 / 5)
Think about what we have learned so far in this unit. We now have a way to persist data in a database. We've also learned about how OOP allows us to programmatically represent real things as objects in ruby. Which is AWESOME! But really databases just seems like data in this kind of cryptic place on our local computer.  We have to make super long SQL statements to do CRUD. It'd be really nice if we had some kind of way to interface between the database and our servers/applications in order to streamline this process. Enter ORM's.

### Information Dive (5 / 10)
For the next 5 minutes, research what ORM's are.

[ORM - wikipedia](https://en.wikipedia.org/wiki/Object-relational_mapping)

[AR Read 1.1 - 1.3](http://guides.rubyonrails.org/active_record_basics.html)

### T&T (5 / 15)
Now, turn & talk to your neighbor and discuss:

1. At a high level, what are ORM's and how might they be useful?
2. What is the importance of interfacing the server with the database?

## ORM's & Active Record (10 / 25)
- *Official* wikipedia definition. A programming technique for converting data between incompatible type systems in object-oriented programming languages.

> Thats sounds like a lot of 5 dollar words, but what does it really mean?

We need a way to encapsulate our databases into objects that we can talk to on our server. ORM's serve that purpose. Remember those tables we created in SQL? Well, its an object represented on our server now. That's what ORM's do.

More concretely ORM's:
- 'Map' (translate) objects to rows in our DB (and vice versa)
- **Conventions**:
  - 1 table per Model/Class/Entity
  - table name is model name pluralized
  - each column is an attribute for that model
- Table associations are handled using foreign keys

It just so happens you will be learning one of the best ORM's on the market. It has some of the best documentation and best syntax(because ruby is awesome) the industry has to offer. This ORM is Active Record.

> Active Record is the M in MVC - the model - which is the layer of the system responsible for representing business data and logic. Active Record facilitates the creation and use of business objects whose data requires persistent storage to a database. It is an implementation of the Active Record pattern which itself is a description of an Object Relational Mapping system. - Taken from AR docs

## Active Record
### Convention Over configuration (ST-WG - 10 / 35)
Before we get started with code, I want to highlight a reoccurring theme with Active Record and Rails in general. You'll often here us say Convention over Configuration.

**Question:**  Without getting into the specifics of AR, what do you think we mean by convention over configuration?

Basically Active Record and Rails, and other frameworks have a whole bunch of conventions that they follow so that you do not have to mess with different configuration details later. These conventions exist because developers agree on best practices, and therefore allows us to spend less time trying to configure, when there all ready is an accepted way to do things. Some of the common ones we will encounter are naming conventions such as: plural vs single, capital or lower, camel or snake.

**BOARD:** Throughout this lesson, I will write on the board Active Record's conventions so we can list them as we go.

In a nutshell, if you don't follow the conventions, you're going to have a bad time.

Alright! Let's get started with some code!

### Setup SQL - WDI (I Do - 10 / 45)
> Throughout the day, I'll be doing some code simulating a "wdi application" then you will code along with our Tunr
applications. Please **do not** code along the wdi application.

Let's go over our domain model for both applications.

![ERDs](./active-record.png)


[Tunr Deployed link](https://wdi-dc6-tunr-demo.herokuapp.com/artists)

I want to be able to do CRUD for these models with Active Record. We'll be going into greater detail about how we are going to use Active Record as an interface between our server and our database, but to start, the first thing that I want to do is create/setup a database.

First let's create our database and create our schema file in the terminal:

![](./output-0.png)

> All we did here was create a database in PSQL. If you ever want to drop a database the command is `$ dropdb <database_name>` (VERY DANGEROUS)

Next, I want to update the schema file and then load tables for our models into the database:

in `db/wdi_schema.sql` file:

![](./output-1.png)

now lets run our `wdi_schema.sql` file in the terminal:

![](./output-2.png)

(ST-WG) Is this potentially really dangerous?

**Question (ST-WG):** Why did we do this? Why not just go into psql and create the tables in postgres?

It'll be nice going forward with your application that we package the schema up so that its modular. We can have others quickly pick up our code and have our exact database setup.

### Setup SQL - Tunr (You Do - 10 / 55)

Do everything that happened in the wdi app to set up your own app Tunr. An app of artists and songs.

You'll know you did this right if:

You enter psql and run these commands:

![](./output-3.png)

The output should be:
![](./output-4.png)

## Break (10/65)

[solution code](https://github.com/ga-wdi-exercises/tunr-active-record/archive/v1.0.zip)

### Setup Ruby & Add Functionality - WDI (I Do - 20 / 85)
Great, now we have a table loaded into our database we're now ready to get started on the ruby side.
Let's first create all the directories/files we're going to need in the terminal:

![](./output-5.png)

> The `app.rb` file is our main application file. This is where a lot of the main program logic will live.

> The `Gemfile` will contain all the dependencies for our program.

> The `models/student.rb` file will contain the class definition for the Student class that will represent the students table in SQL

> by convention we always name our model file names singular

### The `Gemfile` - an aside
A Gemfile is a file we create which is used for describing gem dependencies for Ruby programs. Allows for easier collaboration of apps. TLDR: Take someone's code, use it in your program.

We stand on the shoulders of giants. Oh there's code for that? yoink.

In the `Gemfile`:

![](./output-6.png)

> Without these gems, we would be unable to talk to our SQL database.

Then I'm going to run `$ bundle install` in the terminal.


### Functionality - WDI (I Do - 20 / 105)  

In the `models/student.rb` file, let's define our student model:

![](./output-7.png)

> In this ruby file, we create a class of Student that inherits from `ActiveRecord::Base`. Essentially, when we inherit from `ActiveRecord::Base`, it gives this class a whole bunch of functionality that maps the ruby `Student` class to the `students` table in postgres.

Now, lets create a file that handles the connection between our application and our database:

![](./output-8.png)

In `db/connection.rb`:
![](./output-9.png)

Finally, let's build out the functionality of the `app.rb` file(which in this case is just dropping us into pry):

![](./output-10.png)

> note the difference between `require` and `require_relative`. With `require` we are getting gems and `require_relative` we are getting files relative to the location of the file we wrote `require_relative` in

### Functionality - Tunr (You Do - 20 / 125) + Break(10/135)

[Part 1.1 & 1.2 - Define Artist & Setup Your `app.rb` to Connect The Database](https://github.com/ga-wdi-exercises/tunr-active-record#part-11---create-the-artist-model-using-active-record)

You'll know you did this right if:

Run your program(`$ ruby app.rb`) and then when you enter `pry` run this line of code:

you should get something like this(it won't be the same letters and numbers)
![](./output-11.png)

[solution code](https://github.com/ga-wdi-exercises/tunr-active-record/archive/v1.1.zip)

### Seed the database - Tunr (You Do - 10 / 145)

Create a new file in the `db` directory called `seed.sql`.

In the `db/seed.sql` file place the contents of this [website](https://raw.githubusercontent.com/ga-wdi-exercises/tunr-active-record/c8e99172a6c09930ed18392aae5a98e571cbc507/db/seeds.sql)

Then, just like the schema file, load the `db/seed.sql` into the tunr database you created.

You did this right if:

You run your program and enter this and get the same output:

![](./output-12.png)

### Methods - WDI (I Do -  30/175)

Great! We've got everything done that we need to get setup with single model CRUD in our application. Let's run it in the terminal:

![](./output-13.png)

When we run this app, we can see that it drops us into pry. Let's write some code in pry to update our database... **IN REALTIME!!!**

**Board:** I want to come up with an ongoing list of class methods vs instance methods that we can add to as we use them

Let's create an instance of the Student object on the ruby side:

> **Note** the syntax for creating a new instance.

![](./output-14.png)

To save our instance to the database we use `.save`:

![](./output-15.png)

If we want to initialize an instance of an object AND save it to the database we use `.create`:

![](./output-16.png)

(ST-WG) Why is there a distinction between when it's saved in one command versus two.

One really handy feature we get from an Active Record inherited class is that all of the attribute columns of our model are now `attr_accessor`'s as well. So we can do things like:

![](./output-17.png)

> **Note:** Should be noted that when we set the attribute using a setter, it doesn't automatically save to the database, its not until we call `.save` on the object that it saves to the database

To get all of the students we use `.all`:

![](./output-18.png)

We can also find a student by its ID using `.find`:

![](./output-19.png)

Additionally we can also find a student by an attribute using `.find_by`:

![](./output-20.png)

> Note that find_by only returns the first object that meets the requirements of the arguments

If you want all students that meet a certain query then we use `.where`:

![](./output-21.png)

We can also update attributes and save at the same time using the `.update` method:

![](./output-22.png)

> If we want to update the attribute and not save we would have to use something from this [post](http://www.davidverhasselt.com/set-attributes-in-activerecord/)

Finally we can also just destroy/delete rows in our database with the `.destroy` method:

![](./output-23.png)

> This is exciting stuff by the way, imagine, while we do these things, that our students model is instead a post on facebook, or a comment on facebook. So the next time you comment on someone's facebook page you have an idea now of whats happening on the database layer. Maybe not the whole picture, but you have an idea. We're going to build on that idea in the coming week and half, and thats really exciting.

### Methods - Tunr (You Do (In Pry!) - 15 / 190)

[Part 1.3 - Use Your Artist Model](https://github.com/ga-wdi-exercises/tunr-active-record#part-13---use-your-artist-model)

## Associations

### Reframing (10 / 200)
**Question:** We have a lot of choice when it comes to databases, why are we using SQL?

We use SQL because it is a relational database. But what does that really mean? Basically we want the ability to associate models in our domain. That can come in a variety of ways in a relational database, but at the heart of it is essentially this:

One model has many other instances of another model. And that other model belongs to the original. Not really clear, eh?

Let's take a look at an example in the wild.

With the application Facebook, there are many users. Each of these users have several models associated with them. Let's look at one more closely. We'll be looking at posts(status updates)

The first model we'll be looking at is Users. The second is Posts.

A User has many Posts
And every Post belongs to a certain user.

> Note the plurality of the nouns used in these two sentences

When we start organizing our objects in this manner and program these associations, it becomes much easier to query our database for what we need. IE. If we were on Adrian's facebook page, it wouldn't make sense for us to query EVERY post in facebook and then do `.where(facebook_user: "Adrian")` That would get really expensive. Instead we can do something like, `adrian.posts` and then BAM, we got all of adrian's posts.

> Note that this is just speaking at a high level and not modeling the exact syntax, however it's incredibly close to how the code actually would look if it were modeled in AR.

Let's see what some of this stuff looks like in code. We're going to be adding an instructor model to our program.

### Associations in Schema - WDI (I Do - 10 / 210)

**NOTE:** In this section, we are reviewing our schema and how it reflects associations for our domain. We are NOT updating the schema file.

In `wdi_schema.sql`:

![](./output-24.png)

Make note of the foreign key in `students`

## Break (10/220)
### Updating Class Definitions - WDI (I Do - 10 / 230)

Next I want to create a new file for my Instructor AR Class definition `$ touch models/instructor.rb`. In it I'll put:

![](./output-25.png)

Now In my `models/student.rb` we have to reflect the association:

![](./output-26.png)
> note the plurality of `students` and singularity of `instructor`.  

We also need to include the `models/instructor.rb` file into our `app.rb` so in `app.rb` we need to add

![](./output-27.png)

### Updating Class Definitions - Tunr (You Do - 10 / 240)

[Part 1.4 - Create Your Song Model / Setup Associations](https://github.com/ga-wdi-exercises/tunr-active-record#part-14---create-your-song-model--setup-associations)

[solution code](https://github.com/ga-wdi-exercises/tunr-active-record/archive/v1.2.zip)

### Association Helper Methods - WDI (I Do - 30 / 270)

So we added some code, but we can't yet see the functionality it gives us.

Basically when we added those two lines of code `has_many :students` `belongs_to :instructor` we created some helper methods that allow us to query the database more effectively.

Lets create some objects in our `app.rb` so we can see what were talking about:

![](./output-28.png)

> ST-WG Why do we need to use `.destroy_all`?

Now that we have this association, we can now easily query the database for the relevant records.

If I want to get all of Jesse's students or set Jesse's students I can now write this code:
![](./output-29.png)
> note that when students is being used as a setter method above, it actually changes the instructor_id column for those students to match Jesse's primary ID. Any student that previously was assigned to Jesse and not reassigned in the setter will now have an instructor_id of nil

Alternatively if I wanted to get a student's instructor I could write this code:

![](./output-30.png)

We can also create new students under a certain instructor by doing the following:

![](./output-31.png)
> **Note** that we did not pass in an instructor id above. Active Record is smart and does that for us.

### Association Helper Methods - Tunr (You Do - 15 / 285)

[Part 1.5 - Use Your Model Assocations](https://github.com/ga-wdi-exercises/tunr-active-record#part-15---use-your-model-associations)

### Seeding a Database - WDI (15 / 300)
Seeding a database is not all that different from the things we've been doing today. What's the purpose of seed data? **(ST-WG)**

We want some sort of data in our database so that we can test our applications. Let's create a seed file in the terminal: `$ touch db/seeds.rb`

In our `db/seeds.rb` file let's put the following:

![](./output-32.png)

Once we get rid of this duplicate CRUD code in `app.rb` we can just run this seed file once and know our data is good.

Now when we run our application with `ruby app.rb`, we enter into Pry with all our data loaded.

## Closing
Who misses writing SQL queries by hand? Exactly. Active Record is extremely powerful and helpful, and allows us to easily interface with the business models for our applications.

Review Learning Objectives


### Resources
- [Active Record Basics](http://guides.rubyonrails.org/active_record_basics.html)
- [Active Record Query Interface](http://guides.rubyonrails.org/active_record_querying.html)

### Appendix

#### Instance vs Class Methods
| method              | instance | class    |
|---------------------|:--------:|:--------:|
| .new                |          |     x    |
| .save               |     x    |          |
| .create             |          |     x    |
| attribute accessors |     x    |          |
| .all                |          |     x    |
| .find               |          |     x    |
| .find_by            |          |     x    |
| .where              |          |     x    |
| .update             |     x    |          |
| .destroy            |     x    |          |
| .destroy_all        |          |     x    |

#### Conventions in AR
|description      | Rule    |
|-----------------|---------|
|table names      |snake case and plural|
|model file names |snake case and singular|
|model definition |Camel case and singular|
|argument for has_many| snake case and plural|
|argument for belongs_to| snake case and singular|
|more to follow....|||


