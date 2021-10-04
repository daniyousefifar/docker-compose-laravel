# Docker Compose Laravel
Set Up Laravel with Docker Compose

## Step 1 - Downloading Laravel and Installing Dependencies
First, check that you in your desired path and clone the latest Laravel release to a diecotry called **`laravel-app`**

`$ cd /your/path/to/install/`

`$ git clone https://github.com/laravel/laravel.git laravel-app`

Move into the `laravel-app` directory:

`$ cd /you/path/to/install/laravel-app`

Next, use Docker's composer image to mount the directories that you will need for your Laravel project and avoid the overhead of installing Composer globally:

`$ docker run --rm -v $(pwd):/app composer install`

As a final step, set permissions on the project directory so that it is owned by your **non-root** user:

`$ sudo chown -R $USER:$USER /your/path/to/install/laravel-app`

## Step 2 - Clone the repository into your Laravel project
First, clone the **docker-compose-laravel** repository from Github:

`$ git clone https://github.com/daniyousefifar/docker-compose-laravel.git`

Next, move **docker-compose-laravel** repository files to your laravel project:

`$ mv docker-compose-laravel/.docker /your/path/to/install/laravel-project/`

`$ mv docker-compose-laravel/docker-compose.yml /your/path/to/install/laravel-project/`

Next, remove **docker-compose-laravel** folder from your project

`$ rm -rf docker-compose-laravel/`

## Step 3 - Modifying Environment Settings and Running the Containers
Now that you have defined all of your services in your `docker-compose` file and created the configuration files for these services, you can start the containers. As a final step, though, we will make a copy of the `.env.exmaple` file that Laravel includes by default and name the copy `.env`, which is the file Laravel expects to define its environment:

`$ cp .env.example .env`

You can now modify the `.env` file on the app container to include details about your setup.

Open the file using `vim` or your text editor of choice:

`$ vim .env`

Find the block that specifies `DB_CONNECTION` and update it to reflect the specifics of your setup. You will modify the following fields:
- `DB_HOST` will be your db database container.
- `DB_DATABASE` will be the **laravel** database.
- `DB_USERNAME` will be the username you will use for your database. In this case, we will use **root**.
- `DB_PASSWORD` will be the secure password you would like to use for this user account.

Save you changes and exit your editor.

With all of your services defined in your `docker-compose` file, you just need to issue a single command to start all of the containers, create the volumes, and set up and connect the networks:

`$ docker-compose up -d`

When you run `docker-compose` up for the first time, it will download all of the necessary Docker images, which might take a while. Once the images are downloaded and stored in your local machine, Compose will create your containers. The `-d` flag daemonizes the process, running your containers in the background.

We’ll now use `docker-compose exec` to set the application key for the Laravel application. The `docker-compose exec` command allows you to run specific commands in containers.

The following command will generate a key and copy it to your `.env` file, ensuring that your user sessions and encrypted data remain secure:

`$ docker-compose exec app php artisan key:generate`

You now have the environment settings required to run your application. To cache these settings into a file, which will boost your application’s load speed, run:

`$ docker-compose exec app php artisan config:cache`

As a final step, visit `http://your_server_ip` in the browser. You will see the home page for your Laravel application.

With your containers running and your configuration information in place, you can move on to configuring your user information for the **laravel** database on the `db` container.

### Thanks
[DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-laravel-nginx-and-mysql-with-docker-compose)



