# Flask API Project

## Setting Up the Project

To create the project directory structure and set up a virtual environment, run the following commands in your VScode command line git bash or powershell:

### To open up your terminal in VScode

- Use the shortcut "Ctrl + ` " (backtick key). - Windows users

## Using gitbash

```bash
mkdir -p flask_api_project/{templates,static} && touch flask_api_project/app.py flask_api_project/templates/index.html flask_api_project/static/style.css && python -m venv flask_api_project/venv
```

![1](img/Screenshot%20(201).png)


The command above will create the project enviroment

1. **In the `app.py`** file paste the code below
    ```python
    from flask import Flask, request, jsonify, render_template

    app = Flask(__name__)

    users = []

    @app.route('/')
    def home():
        return render_template('index.html')

    @app.route('/users', methods=['POST'])
    def create_user():
        user = request.get_json()
        users.append(user)
        return jsonify(user), 201

    @app.route('/users', methods=['GET'])
    def get_users():
        return jsonify(users), 200

            @app.route('/users/<int:user_id>', methods=['GET'])
    def get_user(user_id):
        user = next((u for u in users if u['id'] == user_id), None)
        return jsonify(user), 200 if user else 404

    @app.route('/users/<int:user_id>', methods=['PUT'])
    def update_user(user_id):
        user = request.get_json()
        index = next((i for i, u in enumerate(users) if u['id'] == user_id), None)
        if index is not None:
            users[index] = user
            return jsonify(user), 200
        return '', 404

    @app.route('/users/<int:user_id>', methods=['DELETE'])
    def delete_user(user_id):
        global users
        users = [u for u in users if u['id'] != user_id]
        return '', 204

    if __name__ == '__main__':
                app.run(debug=True)
    ```

2. **In the `index.html`** in the `templates` directory paste the code below:
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>API-Based Application</title>
        <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
    </head>
    <body>
        <h1>User Management</h1>
        <form id="userForm">
            <input type="text" id="name" placeholder="Name" required>
            <input type="email" id="email" placeholder="Email" required>
            <button type="submit">Add User</button>
        </form>
        <ul id="userList"></ul>

        <script>
            document.getElementById('userForm').addEventListener('submit', async function (event) {
                event.preventDefault();
                               const name = document.getElementById('name').value;
                const email = document.getElementById('email').value;
                
                const response = await fetch('/users', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({ name, email })
                });

                const user = await response.json();
                document.getElementById('userList').innerHTML += `<li>${user.name} (${user.email})</li>`;
            });
        </script>
    </body>
    </html>
    ```

    3. **In the `style.css`** in the `static` directory paste this code below there:
    ```css
    body {
        font-family: Arial, sans-serif;
        margin: 20px;
    }

    form {
        margin-bottom: 20px;
    }

    input {
        margin-right: 10px;
    }
    ```

 ## Running the Application

## Window Users
```bash
python -m venv venv
source venv/Scripts/activate
pip install Flask
```

Run your Flask application:
```bash
flask run
```

Open your browser and go to `http://127.0.0.1:5000` to see your application.

![2](img/Screenshot%20(207).png)

### Testing the API

**Using Postman**:
 ### What is Postman?

    Postman is a popular API development and testing tool that simplifies the process of creating, testing, documenting, and sharing APIs. It provides a user-friendly interface to make HTTP requests, set request headers, define request parameters, and handle responses. Postman can be very helpful for developers working with APIs, as it allows them to quickly test endpoints, debug issues, and automate testing.
    Watch this short tutorial on how to use postman [here](https://www.youtube.com/watch?v=CLG0ha_a0q8)

    Download Postman if you haven't intsalled it before

    Windows - [here](https://dl.pstmn.io/download/latest/win64)

 - Create a new request.

 ![3](img/Screenshot%20(203).png)

 - Set the request type to `POST` and enter `http://127.0.0.1:5000/users`.

- Go to the Body tab, select `raw`, and choose `JSON` from the dropdown.
- Enter the JSON data:

        ```json
        {
            "id": 1,
            "name": "your name",
            "email": "yourname@example.com"
        }
        ```
- Click `Send` and check the response.

It would like this:
  ![4](img/Screenshot%20(206).png)

# HTTP Status Codes and their meaning:

- 200 OK: The request was successful (common for GET, PUT).
- 201 Created: A new resource was created (common for POST).
- 204 No Content: The resource was successfully deleted (common for DELETE).
- 404 Not Found: The requested resource was not found (if you try to GET, PUT, or DELETE a non-existing user).

