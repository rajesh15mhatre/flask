# Step 1: Install Flask and create a virtual environment (if not already done):


$ sudo apt-get update
$ sudo apt-get install python3-pip
$ sudo pip3 install virtualenv
$ mkdir todoapp
$ cd todoapp
$ virtualenv venv
$ source venv/bin/activate

# Install flask
$ pip install flask

create app.py

from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

todos = []

@app.route('/')
def index():
    return render_template('index.html', todos=todos)

@app.route('/add', methods=['POST'])
def add():
    todo = request.form.get('todo')
    todos.append(todo)
    return redirect(url_for('index'))

@app.route('/delete/<int:todo_id>')
def delete(todo_id):
    if todo_id < len(todos):
        todos.pop(todo_id)
    return redirect(url_for('index'))

if __name__ == '__main__':
    app.run(debug=True)
    
    
Create template\index.html

<!DOCTYPE html>
<html>
<head>
    <title>Todo App</title>
</head>
<body>
    <h1>Todo List</h1>
    <form action="{{ url_for('add') }}" method="POST">
        <input type="text" name="todo" placeholder="Enter a task">
        <input type="submit" value="Add">
    </form>
    <ul>
        {% for todo in todos %}
            <li>{{ todo }} <a href="{{ url_for('delete', todo_id=loop.index-1) }}">Delete</a></li>
        {% endfor %}
    </ul>
</body>
</html>

Run flask app

$ python app.py
