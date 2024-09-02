
1) Install postgres in local

2) Steps to connnect postgres to redwood:
a) In .env.defaults file:
DATABASE_URL=postgresql://postgres:admin@localhost:5432/postgres?connection_limit=1

b) Change the name of the database in schema.prisma
provider = "postgresql"


Web:
----
To generate page:
yarn redwood generate page home /
yarn redwood generate page about
A note about <Metadata>:

At the top of your newly created page is a <Metadata> component,
which contains the title and description for your page, essential
to good SEO. Check out this page for best practices:

https://developers.google.com/search/docs/advanced/appearance/good-titles-snippets

yarn redwood g layout blog //g - generate
a) Once layout blog page is created, add links of different pages. For eg: <Link to={routes.home()}>Home</Link>
b) In Routes.jsx file wrap the pages with Set, which is an import from "@redwoodjs/router". For eg:
      <Set wrap={BlogLayout}>
        <Route path="/about" page={AboutPage} name="about" />
        <Route path="/" page={HomePage} name="home" />
      </Set>
	  
Getting data from database:
Api:
----
1) Create schema in schema.prisma file. 
2) yarn rw prisma migrate dev
3) yarn rw prisma studio - To open prisma studio - http://localhost:5555 

yarn rw g scaffold post - To generate everything to perform CRUD(create, retrieve, update and delete) operation
Then point it to http://localhost:8910/posts 
List of tasks that happens when we expecute this command
https://docs.redwoodjs.com/docs/tutorial/chapter2/getting-dynamic

Cells:
When do you need cells?
If your component needs some data from the database or other service that may be delayed in responding.

-A best practice for Redwood is to create a Page for each unique URL your app has.

yarn rw g cell Articles - To create a cell, which will display list of articles
In the generated cell for example ArticlesCell.jsx,
a) Check if query names are same, else use alias. For eg: articles: posts
b) Add necessary fields to import. For example id, title, body
c) Import this cell(ArticlesCell) to which ever page that is needed to display the results
d) Fine tune the display using the properties from the data in success(ArticlesCell.jsx)

Steps to build a detailed page from the list of pages:
Routing Params
yarn rw g page Article

yarn rw g cell Article  - To create this cell for using in individual ArticlePage

Note: In RedwoodJS, By default, any props you give to a cell will automatically be turned into variables and given to the query. 

yarn rw g component Article - To create a component

Building a Form:
1) yarn rw g page contact

Saving Data:
------------
In Schema.prisma:
Add Contact model
Tip: To mark a field as optional (that is, allowing NULL as a value) you can suffix the datatype with a question mark, e.g. name String?. This will allow name's value to be either a String or NULL.

yarn rw prisma migrate dev

Create an SDL & Service:
yarn rw g sdl Contact

Note:
Queries and mutations in an SDL file are automatically mapped to resolvers defined in a service, so when you generate an SDL file you'll get a service file as well, since one requires the other.

Tip:
GraphQL's SDL syntax requires an extra ! when a field is required. Remember: schema.prisma syntax requires an extra ? character when a field is not required.
Note that having at least one schema directive is required for each Query and Mutation or you'll get an error:


yarn rw g sdl Contact --no-crud //if you just need a simple read-only SDL, you can skip creating the create/update/delete mutations by passing a flag to the SDL generator like this

Note:
It's usually recommended to not rely solely on the database for input validation: what format your data should be in is a concern of your business logic, and in a Redwood app the business logic lives in the Services!

Authentication
==============
https://docs.redwoodjs.com/docs/tutorial/chapter4/authentication
yarn rw setup auth dbAuth //  This is a self-hosted authentication (named dbAuth in Redwood) since it's the simplest to get started with and doesn't involve any third party signups.
Create user model
yarn rw prisma migrate dev

yarn rw g dbAuth //Generator to create pages for login, signup and forgot password pages
Then add useAuth() as shown in example

In api\src\lib\auth.js, we need to modiy per the following:
The getCurrentUser() function is where the magic happens: whatever is returned by this function is the content of currentUser, in both the web and api sides! In the case of dbAuth, the single argument passed in, session, contains the id of the user that's logged in. It then looks up the user in the database with Prisma, selecting just the id. Let's add email to this list.

Read about session secret in Authentication tutorial when ever you want to logout all users or change/deploy to a new environment or host. 

Deployment
===========

