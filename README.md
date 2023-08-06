<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://embed-ssl.wistia.com/deliveries/71b320f6f756ae4cc7b1450cf7bf8f7d.jpg" width="400" alt="Laravel Logo"></a></p>

## What are the RESTful APIs

REST is a set of architectural constraints, not a protocol or a standard. API developers can implement REST in a variety of ways.

When a client request is made via a RESTful API, it transfers a representation of the state of the resource to the requester or endpoint. This information, or representation, is delivered in one of several formats via HTTP: JSON (Javascript Object Notation), HTML, XLT, Python, PHP, or plain text. JSON is the most generally popular file format to use because, despite its name, it’s language-agnostic, as well as readable by both humans and machines.

Something else to keep in mind: Headers and parameters are also important in the HTTP methods of a RESTful API HTTP request, as they contain important identifier information as to the request's metadata, authorization, uniform resource identifier (URI), caching, cookies, and more. There are request headers and response headers, each with their own HTTP connection information and status codes.

In order for an API to be considered RESTful, it has to conform to these criteria:

-   A client-server architecture made up of clients, servers, and resources, with requests managed through HTTP.

-   Stateless client-server communication, meaning no client information is stored between get requests and each request is separate and unconnected.

-   Cacheable data that streamlines client-server interactions.

-   A uniform interface between components so that information is transferred in a standard form. This requires that:

    -   resources requested are identifiable and separate from the representations sent to the client.
    -   resources can be manipulated by the client via the representation they receive because the representation contains enough information to do so.
    -   self-descriptive messages returned to the client have enough information to describe how the client should process it.
    -   hypertext/hypermedia is available, meaning that after accessing a resource the client should be able to use hyperlinks to find all other currently available actions they can take.

-   A layered system that organizes each type of server (those responsible for security, load-balancing, etc.) involved the retrieval of requested information into hierarchies, invisible to the client.

-   Code-on-demand (optional): the ability to send executable code from the server to the client when requested, extending client functionality.

## Folders we are using here

-   .env file <= database connection
-   app > models directory <= all the model classes
-   database directory <= migrations, factories and seeders
-   api.php <= writing RESTful APIs

## What you need to generate

-   Models
-   Migrations
-   Factories
-   Seeders
-   Controllers
-   API Resources

## References

-   Modifying columns: https://laravel.com/docs/5.3/migrations#changing-columns
-   FakerPHP for dummy data: https://github.com/FakerPHP/Faker

## All of the commands and changes to the code

-   Creating a new laravel project: `laravel new project_name`
-   Creating models, migrations, factories and seeders: `php artisan make:model model_name`
-   Creating a controller for the API: `php artisan make:controller ControllerNameController --api --model=model_name`
-   Creating an API Resource(Resource Type): `php artisan make:resource ModelNameResource`
-   Creating an API Resource(Collection Type): `php artisan make:resource ModelNameCollection`
-   Update the up method of the migration file:
    -   2023_08_06_183340_create_petitions_table.php
    ```
    public function up(): void
    {
        Schema::create('petitions', function (Blueprint $table) {
            $table->id();
            $table->String("title");
            $table->Text("category");
            $table->Text("description");
            $table->String("author");
            $table->Integer("signers");
            $table->timestamps();
        });
    }
    ```
-   Running the database migration: `php artisan migrate`

    -   Eroor:

    ```
    Illuminate\Database\QueryException

    SQLSTATE[HY000] [2002] No connection could be made because the target machine actively refused it (Connection: mysql, SQL: select * from information_schema.tables where table_schema = first_php_rest_api and table_name = migrations and table_type = 'BASE TABLE')
    ```

    -   Solution: Run xampp and start Apache and MySQL

-   Modifications
    -   Install doctrine/dbal for data table modifications: `omposer require doctrine/dbal`
    -   Modifying the a table: `php artisan make:migration change_column_type_in_table`
    -   Update the up method of the migration file:
        2023_08_06_191640_change_category_type_in_petitions.php
    ```
    public function up(): void
    {
        Schema::table('petitions', function (Blueprint $table) {
            $table -> String("category")->change();
        });
    }
    ```
    -   Run the database migration again: `php artisan migrate`
-   Adding dummy data to the database

    -   Update the factory file
        PetitionFactory.php
        ```
        public function definition(): array
        {
          return [
              'title'=>$this->faker->word,
              'category'=>$this->faker->text(50),
              'description'=>$this->faker->text(200),
              'author'=>$this->faker->name,
              'signers'=>$this->faker->numberBetween(0, 1000000)
          ];
        }
        ```
    -   Adding factory to the seeder

        -   1st method (Using the DatabaseSeeder.php - seed for all the tables at once)
            DatabaseSeeder.php

            ```
            public function run(): void
            {
              Petition::factory(50)->create();
            }
            ```

            and put the import "use App\Models\Petition;"
            or you can simply use **PhpStorm IDE**. It highligts missing imports for you.
            and also, you can achieve the same thing in Vs Code using **"Intellicode, php intelephense and Php namespace resolver extentions"**

        -   2nd method (Using the PetitionSeeder.php - seed the tables, one by one individually)
            PetitionSeeder.php

        ```

        ```

    -   Run the seed command: `php artisan db:seed`
