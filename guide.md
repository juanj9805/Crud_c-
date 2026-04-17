# ASP.NET Core MVC CRUD — Reference Guide

A step-by-step reference for building a full CRUD application using ASP.NET Core MVC with Entity Framework Core, deployed on a Linux server with Nginx and HTTPS via Certbot.

---

## 1. Project Setup

### Setting a Default Index Page

To configure which view loads when the application starts, you need to set the default route in your routing configuration. The screenshots below show where and how to do this.

![alt text](<Captura desde 2026-04-16 19-45-04.png>)
![alt text](<Captura desde 2026-04-16 19-45-06.png>)

### `dotnet run` vs `dotnet watch`

Use `dotnet run` instead of `dotnet watch` when working on the backend. `dotnet watch` is intended for front-end hot-reload scenarios and can suppress error output in the terminal, making it harder to debug backend issues.

---

## 2. Database Context & Environment Variables

### Registering DbContext in `Program.cs`

The `DbContext` class is your application's gateway to the database. It must be registered as a service in `Program.cs` so ASP.NET Core's dependency injection system can provide it to your controllers. The screenshot below shows the registration pattern.

![alt text](./assets/image-41.png)

### Configuring Environment Variables

Sensitive values like the database connection string should not be hardcoded. Store them in environment variables or `appsettings.json`. The following screenshot shows where to add these values so the application can read them at startup.

![alt text](./assets/image-2.png)

---

## 3. Controller & Views — Read All (Index)

### Index Action

The `Index` action in the controller queries the database and passes the result to the view. Since it returns multiple records, the model type in the view must be `IEnumerable<T>`.

![alt text](./assets/image-8.png)

The `Ok()` helper returns an HTTP 200 response with the data serialized as JSON. If you need a different status code (e.g., 404 or 201), you must construct the response explicitly using an `if/else` or the appropriate result method.

### Index View

In the view, the `@` symbol marks server-side C# code embedded in the HTML (Razor syntax). Any variable prefixed with `@` is evaluated on the server before the page is sent to the browser.

![alt text](./assets/image-9.png)

The view model is declared as `IEnumerable<YourModel>` to support iterating over the list of records.

### Linking to Actions with Tag Helpers

ASP.NET Core provides tag helpers like `asp-action` and `asp-controller` to generate URLs for links and forms. When you use `asp-action`, ASP.NET resolves the correct URL automatically — you don't need to hardcode paths.

![alt text](./assets/image-16.png)

Each field in the row links to the appropriate action. Both the tag helper attribute and the controller action name must match exactly.

![alt text](./assets/image-17.png)

### User Controller — Supporting Actions

![alt text](./assets/image-15.png)

---

## 4. Create

### Create View

The Create view contains a form for submitting a new record. Each input must map to a property of the model.

![alt text](./assets/image-18.png)

### Form Buttons and Actions

Each button in the form must specify which controller action it targets using `asp-action`. Without this, the form submission won't route to the correct method.

![alt text](./assets/image-19.png)

---

## 5. Edit

### Edit View

The Edit view is similar to Create but pre-populates the fields with the existing record's data. Use `asp-for` to bind each input to the corresponding model property. This automatically sets both the `name` attribute (needed for model binding) and the `value` attribute (pre-filled data).

![alt text](./assets/image-21.png)

![alt text](./assets/image-22.png)

`asp-for="PropertyName"` must match the exact name of the property in your model class. When it works correctly, it sets the `name` attribute on the input automatically.

If `asp-for` is not populating the field name correctly, you can set the `name` attribute manually as a fallback:

![alt text](./assets/image-24.png)

### Edit Controller Actions

The Edit feature requires two controller actions:
1. A `GET` action to load the existing record and display the form.
2. A `POST` action to receive the updated data and save it.

Use `asp-action` in the form tag to point to the correct controller method.

![alt text](./assets/image-25.png)

![alt text](./assets/image-26.png)

---

## 6. HTTP POST & Model Binding

### Receiving Form Data

When a form is submitted, ASP.NET Core binds the posted form fields to the action's parameter object automatically — this is called **model binding**. Decorate the action with `[HttpPost]` to ensure it only handles POST requests.

![alt text](./assets/image-29.png)

![alt text](./assets/image-30.png)

The parameter passed to the action should represent the **incoming (new) data** from the form, not the existing record. ASP.NET will populate its properties from the form fields.

You can also validate the model inside the action using `ModelState.IsValid` before saving to the database.

### Redirecting After Save

After a successful create or edit, redirect the user to the index page (or any named action) instead of returning a view directly. This follows the **POST/Redirect/GET** pattern and prevents duplicate form submissions on page refresh.

![alt text](./assets/image-32.png)

---

## 7. Delete (Destroy)

### Delete Controller Action

The Delete action finds the record by ID, removes it, and redirects back to the index.

![alt text](./assets/image-35.png)

Important: HTML forms only support `GET` and `POST` methods — there is no native `DELETE` method in HTML. You must use `method="post"` on your delete form.

### Using a Form Instead of an Anchor Tag

A common mistake is using an `<a>` tag to trigger a delete. This does not work because clicking a link sends a `GET` request, not a `POST`. The correct approach is a `<form>` with `method="post"` and a submit button.

![alt text](./assets/image-37.png)

Replace any `<a>` tags for delete with a form like this. The submit button is what triggers the POST request to the Delete action.

---

## 8. Show (Read One)

### Show View

The Show view displays the details of a single record. The model type here is the entity directly (not `IEnumerable`), since you're showing one item.

![alt text](./assets/image-38.png)

![alt text](./assets/image-39.png)

### Show Controller Action

The controller action receives the record ID as a route parameter, fetches the matching record from the database, and passes it to the view.

![alt text](./assets/image-40.png)

---

## 9. Additional Notes

![alt text](./assets/image-43.png)

---

## 10. Deployment — Nginx + SSL

### Nginx Configuration

This guide assumes the application is deployed on a Linux server with Nginx as the reverse proxy. The Nginx configuration must proxy requests to the .NET application process (usually running on a local port like `5000`).

### Setting Up HTTPS with Certbot

Certbot is a free tool that automatically provisions and renews TLS certificates from Let's Encrypt. Since the server is using Nginx, run the following command:

```bash
sudo certbot --nginx
```

Certbot will detect your Nginx configuration, prompt you to select the domain to secure, and automatically update the Nginx config with the SSL settings.

![alt text](./assets/image-44.png)

![alt text](./assets/image-45.png)

Follow the prompts to select the certificate options:

![alt text](./assets/image-46.png)

![alt text](./assets/image-47.png)

After completion, Certbot generates and injects the necessary SSL directives into your Nginx configuration file automatically:

![alt text](./assets/image-48.png)
