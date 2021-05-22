Hey there! Today I'll show you how create your first SQLite database and manage some data into it without SQL at all. All need is package [SQLLEX](https://github.com/v1a0/sqllex), let's install it and begin.

&#x200B;

Type in terminal:

    pip install sqllex

Or if you have many python versions install it for which one you need (type version instead of x):

    python3.x -m pip install sqllex

&#x200B;

Well done! Now we need to decide what database structure do we need. Imagine you are admin some chat-bot and you need to collect data from your chats about users.

For the firs example let it be **id(s)** and **username(s)** of user(s). So inside our database (it's just a file) we will create a table called "users" with two fields (columns). Values in the first one column (id) have to INTEGER and UNIQUE (because users can't have same id), in the second column (username) values have contains TEXT-like data, I guess it's clear.

&#x200B;

So let's code it!

&#x200B;

    # import all from sqllex package
    from sqllex import *
    
    # Create database file 'database.db'
    db = SQLite3x(path='database.db')
    
    # Create table inside database
    db.create_table(
        'users',
        {
            'id': [INTEGER, UNIQUE],
            'username': TEXT
        },
        IF_NOT_EXIST=True
        # it'll avoid you an error if table already exist
    )

&#x200B;

Run this code and you'll see that in the same directory where locate your script were created database file:

&#x200B;

You can open it (by [sqlitebrowser](https://github.com/sqlitebrowser/sqlitebrowser) for example) and make sure it works right:

&#x200B;

Without this tool, you can check it like this:

    print(db.tables_names)
    
    # ['users']

Good, now it's time to add some data into this table.

&#x200B;

For the first let's get table 'users' as object by itself. Don't be scared! It's for your  convenience.

    users = db['users']

&#x200B;

And now insert new record (user) in this table:

    users.insert(1, "User_1")

Or try insert is like this:

    users.insert([2, "User_2"])

Or like this:

    users.insert(id=3, username="User_3")

&#x200B;

Awesome! We're in:

&#x200B;

So now we need to get this back somehow. Here we have 'select' or 'select\_all' method for this.

    print(
        users.select_all()
    )
    
    # [[1, 'User_1'], [2, 'User_2'], [3, 'User_3']]

&#x200B;

Good job! But what if we need select only one record where **id = 1**. Well, you have 2 different way for it:

&#x200B;

The first one is ***find*** method:

    print(
        users.find(id=1)
    )
    
    # [1, 'User_1']

&#x200B;

And the second one:

    print(
        users.select_all(id=1)
    )
    
    # [1, 'User_1']

# 

Perfect. But what if you don't need full record, but only specific column. For example you looking for name of user who have id=3. Ezy, just ***select*** one specific column by ***select*** method:

    print(
        users.select('username', id=3)
    )
    
    # ['User_3']

&#x200B;

&#x200B;

&#x200B;

Excellent! And now imagine one more case, some of your users changed nickname, how can you ***update*** nickname to have up-to-date data. You wouldn't believe me but it's have ***update*** method!

    users.update(
        SET={
            'username': 'NEW_User_3'
        },
        WHERE={
            'id': 3
        }
    )

&#x200B;

If you scared about dictionaries and {{{{}}}}, just use lists instead. Like this:

     users.update(
        SET=['username', 'NEW_User_2'],
        WHERE=['id', 2]
    )

&#x200B;

And also you don't have to use ***SET*** and ***WHERE***, but in my opinion it's harder to read without it.

    users.update(
        ['username', 'NEW_User_1'],
        ['id', 1]
    )

&#x200B;

No matter which way you prefer, result will be the same.

&#x200B;

&#x200B;

&#x200B;

So when all records updated, let's check results. You can also use ***select*** or ***select\_all*** methods as in examples before, but I just want to show you something calling *"syntax sugar".* Just get column 'username' as item of dict.

    print(
        users['username']
    )
    
    # ['NEW_User_1', 'NEW_User_2', 'NEW_User_3']

&#x200B;

&#x200B;

&#x200B;

And that's it. Almost done. Now let's ***delete*** records where **id < 4** (all of them) and finally ***drop*** (erase) the table.

    users.delete(id=['<', 4])
    
    print(
        users['username']
    )
    
    # []

&#x200B;

And ***drop*** the table

    db.drop('users')
    
    print(
        db.tables_names
    )
    
    # []

&#x200B;

# Congratulations! Now you know how to create and admin your own sqlite databases with SQLLEX!

# Explore more and learn how awesome SQL with SQLLEX project!

&#x200B;

If you love this tool and guides like this, you could [give a star for our project on github](https://github.com/v1a0/sqllex).

You can find more complex examples [here\_1](https://github.com/v1a0/sqllex/wiki/SQLite3x-%7C-AN-BIG-EXAMPLE),  [here\_2](https://github.com/v1a0/sqllex/wiki/SQLite3x-%7C-SIMPLEST-EXAMPLE), [here\_3](https://deepnote.com/@abid/SQLLEX-Simple-and-Faster-7WXrco0hRXaqvAiXo8QJBQ#).

What would you like we code next?

&#x200B;

Thanks for read and stay awesome.