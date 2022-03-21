# How to setup Heroku

## For Ruby on Rails

Last tested on 2022-03-21
- Ruby Version: 3.0.0
- Rails Version: 7.0.2.3
- Stack: heroku-20

Assumes existing git repo with project & existing heroku account
1. Create new heroku app: https://dashboard.heroku.com/apps
1. Log into heroku via your terminal
    ```
    $ heroku login
    ```
1. Set remote branch to point to heroku project
    ```
    $ heroku git:remote -a <name_of_project>
    ```
1. Install buildpacks (set of scripts that may contain dependencies required by the programming language). You can run this in the terminal OR in your app's settings accessible via the heroku web console. 
    ```
    $ heroku buildpacks:add --index 1 heroku/nodejs # required by webpacker
    $ heroku buildpacks:add --index 2 heroku/ruby
    ```
    [This step should preempt the `Command "webpack" not found` error](https://github.com/rails/webpacker/issues/512#issuecomment-309268946)
1. Push your repo's changes to the heroku project. When this finishes, the site is up but the database will not be set up.
    ```
    $ git push heroku master
    ```
1. Check database migration status:
    ```
    $ heroku run rake db:migrate:status
    Running rake db:migrate:status on ⬢ <name_of_project>... up, run.4801 (Free)
    Schema migrations table does not exist yet.
    ```
1. Run database migrations:
    ```
    $ heroku run rake db:migrate
    Running rake db:migrate on ⬢ <name_of_project>... up, run.4768 (Free)
    I, [2022-03-21T07:03:28.811892 #4]  INFO -- : Migrating to DeviseCreateUsers (20220115023216)
    == 20220115023216 DeviseCreateUsers: migrating ================================
    -- create_table(:users)
      -> 0.0563s
    -- add_index(:users, :email, {:unique=>true})
      -> 0.0245s
    -- add_index(:users, :reset_password_token, {:unique=>true})
      -> 0.0390s
    == 20220115023216 DeviseCreateUsers: migrated (0.1201s) =======================
    ```
1. You should be able to use your site now!
