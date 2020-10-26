# The power of Strapi - Building Web Application has never been easier

For us, developers, starting a new web project can be a tedious job. It raises questions like:

1. What technology should I use for the front-end?
2. What technology should I use for the back-end?
3. What database is the best?

Because nowadays all Javascript technologies like **React**, **Angular** and **Vue** are very popular for building Web applications, so we can get an answer for the first question very fast. But what about the back-end? Should I use **NodeJS** or **.NET Core**? Is it better to use a **relational** or **non-relational** database? Well, **Strapi** has the answer to all these questions.

# What is Strapi ?

Strapi is an open-source Headless CMS that gives developers the freedom to choose their favorite tools and frameworks. With all the plugins and features Strapi gives the developers the ability for customization and extensibility. Strapi is also very Developer-Friendly by providing an API that can be easily accessed either via REST or GraphQL endpoint.

In this article, we are going to build a simple web application using Strapi and Angular.

First, we are going to set up and build our API.

*****

## Install Strapi

```bash
npx create-strapi-app blog_api --quickstart
```
Once the setup from the command above is finished Strapi will automatically run (NOTE: when manually start the project run the command strapi develop) and we can navigate to our admin panel on the following link: http://localhost:1337/admin. When you navigate you will able to see the registration form.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/qawzds0zvny9yhu8pa1u.png)

When we finish with registering our first user, we can start building our API.

First, what we need to do for our Blog Application is to define the models that we will have. We will define three models: Page, Post, and Content.

To create a new Model navigate to Content Type Builder.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/uwed2lha8rolrsggnqys.png)

Our model `Content` will have:

1. Title - type `Text`
2. Value - type `RichText`
3. IsPublished - type `Boolean`
4. CoverImage - type `Media`
5. Relation to `Post` (Content belong to many `Posts`)
6. Relation to `Page` (Content belong to many `Pages`)

`Page` model will have:

1. Name - type `Text`
2. Relation to `Content` (`Page` has many `Contents`)
3. Relation to `Post` (`Page` has many and belongs to many `Posts`)

and `Post` model will have:

1. IsDeleted - type `Boolean`
2. Relation to `Page` (`Post` has many and belongs to many `Pages`)
3. Relation to `Contents` (`Post` has many `Contents`)

As soon as we define our models we are ready to create some pages, contents, and posts. We can simply do that by navigating to each model and click `Add new [name-of-the-model]`

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/rd3qkvyio48invmjd2zp.png)

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/4oe31kesqgdln90mue63.png)

Now, when we have models and data into our database we need to give access to our visitors of blog application. To do that we need to navigate to `Roles and Permissions` tab in the menu. We can see there are by default two types of roles: `Public` and `Authorized`. We navigate to `Public` role and select:

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/eyfog290mygxjxq27s9s.png)

And that's it. Our API is ready. Now we only need to make our web application.

***

# Angular Application

Install the Angular CLI with the following command:
```bash
npm install -g @angular/cli
```

Run the following commands at the root of your strapi server to create and run a new angular app:
```bash
ng new blog-web 
cd blog-web 
ng serve
```

If you navigate to http://localhost:4200/ you will able to see the new app.

Now, we can start with styling our application and access data from our API. First, we will create services and API calls to get our data from Strapi. Navigate to `src` folder and run the following commands:
```bash
mkdir services
cd services
ng g s page
ng g s post
ng g s content
```
Angular CLI will create these services so we are ready for coding. In `environment.ts` we will put our API URL.
![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/i2ttbi1lgzr5qcdmpr6n.png)

Navigate to page service and insert the following code:

* page-service.ts
<script src="https://gist.github.com/mkubdev/a31ad653532ddf439204cc435591e724.js"></script>


We created two methods: one for getting all pages and one for getting page by id. We will make the same for `post` and `content` services.

NOTE: Before using `HttpClient` we need to register into `app-module.ts`

1. Go to app-module.ts
2. Import the `HttpClientModule` from `@angular/common/http`,
```ts
import { HttpClientModule } from '@angular/common/http';
```
3. Add it to the `@NgModule.imports` array :
```ts
imports:[HttpClientModule,  ...]
```

* post-service.ts
<script src="https://gist.github.com/mkubdev/2361d88c7aa0a086505836b413732c4d.js "></script>

* content-service.ts
<script src="https://gist.github.com/mkubdev/eb7fb6902f19b17501859b5a32b97e50.js "></script>


Next, we will create `post-component` that will contain style and functionality for posts and we will use `app-component` for displaying our landing page. Navigate to `app` folder and create a new folder called components. Here, we will store all components that we use in our blog application. With the following command we can generate a new component:
```bash
ng g c post
```

Insert the following code into the post component :
* post.component.html 
<script src="https://gist.github.com/mkubdev/8d894cdead7a25ae68d63a8392486015.js "></script>

* post.component.css
<script src="https://gist.github.com/mkubdev/ef3e8543f44c49f32c3e0ab3e8a15bb1.js "></script>

* post.component.ts
<script src="https://gist.github.com/mkubdev/94c46a46524f59a5e6af6352b91ac1cb.js "></script>

****

Because we are using bootstrap classes we need to include bootstrap into our project as well. We can do that by adding the following into `index.html` :
```html
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootswatch/4.3.1/cosmo/bootstrap.min.css">
```

And we are almost done. The only thing that left is to modify `app-component` and our blog is ready for use.

* app.component.html
<script src="https://gist.github.com/mkubdev/b25a3c707c671c93194e92af5bda7f03.js "></script>


* app.component.scss
<script src="https://gist.github.com/mkubdev/738d5daa05b9b91aed29e8dd8298bbf9.js "></script>

* app.component.ts
<script src="https://gist.github.com/mkubdev/d4867b24e1f147e3239e505e88ec9c28.js "></script>

Congratulations, we successfully built a Blog application.
![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/pegfy4g53ys9v9sa1suy.png)

****

# Conclusion

Feel free to continue working on your blog. You can try various scenarios navigation, styling e.t.c. Play with models into **Strapi** and API calls from your **Angular** application.

***

### Dev.to Link :

https://dev.to/mkubdev/the-power-of-strapi-building-web-application-has-never-been-easier-291i/
