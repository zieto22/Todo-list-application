# Flask Todo List Application

This is a simple web application built with Flask that allows users to register, log in, and manage a personal todo list. The project demonstrates basic user authentication, session management, and CRUD operations for managing todo items. It is targeted at developers looking to learn how to create a web application with user authentication and simple todo list functionality.

This application provides an excellent starting point for those new to web development with Flask. It covers essential aspects such as user registration and login, secure password handling, and maintaining user-specific data. The todo list functionality allows users to add, complete, and delete tasks, showcasing the CRUD (Create, Read, Update, Delete) operations.
## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Features](#features)
- [License](#license)
- [Contact](#contact)
- [Acknowledgments](#acknowledgments)

## Installation

Follow these steps to install the project locally:

```bash
# Clone the repository
git clone https://github.com/zieto22/Todo-list-application
cd Todo-list-application

# Create a virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`

# Install necessary dependencies
pip install flask flask_wtf flask_sqlalchemy


# Run the application
python app.py
```

## Usage

Here's how you can get started with using the project and different functions.

Open a web browser and navigate to http://localhost:5000.
- Register a new account by clicking on "Create Account".
``` python

# Example of user registration code snippet
@app.route('/register', methods=['GET', 'POST'])
def register():
    form = RegistrationForm()
    if form.validate_on_submit():
        hashed_password = generate_password_hash(form.password.data, method='pbkdf2:sha256')
        new_user = User(username=form.username.data, password=hashed_password)
        db.session.add(new_user)
        db.session.commit()
        flash('Your account has been created!', 'success')
        return redirect(url_for('index'))
    return render_template('register.html', form=form)


```
- Log in with your new account.
```python

# Example of user login code snippet
@app.route('/login', methods=['POST'])
def login():
    username = request.form['username']
    password = request.form['password']
    user = User.query.filter_by(username=username).first()
    if user and check_password_hash(user.password, password):
        session['user_id'] = user.id
        return redirect(url_for('todo_list'))
    else:
        flash('Invalid username or password', 'danger')
        return redirect(url_for('index'))


```
- Manage your todo list by adding, completing, or deleting tasks

```python
# Example of adding a todo item code snippet
@app.route('/todos', methods=['GET', 'POST'])
def todo_list():
    if 'user_id' not in session:
        return redirect(url_for('index'))
    user_id = session['user_id']
    user = User.query.get(user_id)
    form = TodoForm()
    if form.validate_on_submit():
        new_todo = Todo(
            description=form.description.data,
            completed=False,
            user_id=user_id
        )
        db.session.add(new_todo)
        db.session.commit()
        return redirect(url_for('todo_list'))
    todos = Todo.query.filter_by(user_id=user_id).all()
    return render_template('todos.html', todos=todos, form=form)

```
- Completing a Todo Item
```python
# Example of Completing a Todo Item
@app.route('/complete_todo/<int:todo_id>', methods=['POST'])
def complete_todo(todo_id):
    todo = Todo.query.get(todo_id)
    if todo:
        todo.completed = True
        db.session.commit()
    return redirect(url_for('todo_list'))
```
- Deleting a todo item
```python
# Example of Deleting a todo item code snippet
@app.route('/delete_todo/<int:todo_id>', methods=['POST'])
def delete_todo(todo_id):
    todo = Todo.query.get(todo_id)
    if todo:
        db.session.delete(todo)
        db.session.commit()
    return redirect(url_for('todo_list'))
```
## Features

some Key features to this application are

- User Registration and Authentication: Secure registration and login functionality using hashed passwords.
- Todo List Management: Add, complete, and delete todo items.
- Session Management: User sessions to keep track of logged-in users.
- Responsive Design: Simple and responsive user interface using Bootstrap.

## License

This project is licensed under the MIT License. For more details, see the LICENSE file.

## Contact

Contact info - Zion Temo - Zion.temo@elev.ga.ntig.se

Project Link: https://github.com/zieto22/Todo-list-application

## Acknowledgments

Thanks to the Flask and SQLAlchemy communities for their excellent documentation and support.
Inspiration from various Flask tutorials and projects.
Bootstrap for providing a responsive design framework.
