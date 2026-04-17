### Set page as default

If we want to put a view as a index page here we can do it 
![alt text](<Captura desde 2026-04-16 19-45-04.png>)
![alt text](<Captura desde 2026-04-16 19-45-06.png>)

### Advice
Is better run dotnet run than wathc in front op but backen do not show erros


### Instance db context on program
![alt text](./assets/image-41.png)
This is when you do the .env

- Here on this file we must add the .env variables
![alt text](./assets/image-2.png)

### User controller
![alt text](./assets/image-8.png)
ok helps to return an http code on json format
If we need a differente status code we must create it (if else)

Watch it
![alt text](./assets/image-20.png)



### View
![alt text](./assets/image-9.png)
Here we need to create the model IEnumerable<data type>
- @ On c# do reference to data bring from the server

![alt text](./assets/image-16.png)
- each field 
- When we do a reference to a user if we just put the asp.action already take the action
- Both values must be the same

#### each one make reference to the parameter 
![alt text](./assets/image-17.png)

### User controller
![alt text](./assets/image-15.png)


### Create

![alt text](./assets/image-18.png)


![alt text](./assets/image-19.png)
each button must have a reference to its method(controller)

### Edit view

![alt text](./assets/image-21.png)

![alt text](./assets/image-22.png)
- same name as the model "asp-for"
- if this work asp-for put the names


![alt text](./assets/image-24.png)
In case that this not work we must do this

### User controller

1
![alt text](./assets/image-25.png)
asp-action<Same method>
![alt text](./assets/image-26.png)

### Http

![alt text](./assets/image-29.png)
![alt text](./assets/image-30.png)
- WIth this this gonna work with http post
- verify data
- user that we pass as parameter must be the new

### Return to somewhere with that name
![alt text](./assets/image-32.png)


### Destroy

![alt text](./assets/image-35.png)
- Even thought this is a delete web form just have post or get
- to have in account
- Destroy
- return index


### Possible error
![alt text](./assets/image-37.png)
- delete does not work with a a tag we must create a form
- button with it's submit is the one that work


### Show view
![alt text](./assets/image-38.png)
![alt text](./assets/image-39.png)

### User controller
![alt text](./assets/image-40.png)


### Advice ignore
![alt text](./assets/image-43.png)


### cerbot
![alt text](./assets/image-44.png)
- As on the server we are working with nginx here we gonna continue with it and our system(linux)

### Cerbot terminal
```
    sudo certbot --nginx

```
![alt text](./assets/image-45.png)
```
    Option certbot

```
![alt text](./assets/image-46.png)
![alt text](./assets/image-47.png)

cerbot create this for us neccesary congifs 
![alt text](./assets/image-48.png)
