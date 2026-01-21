Task Management API (TaskMaster). 

This project follows a structured process from conceptualization to deployment.

1. Requirement Analysis
The goal is to build a RESTful API that allows users to create, read, update, and delete (CRUD) tasks.

Functional Requirements: Users can add tasks with a title and description, mark them as complete, and delete them.

Technical Requirements: Python 3.x, Flask framework, and an in-memory data store for simplicity.

2. Design
We define the nomenclatures that will be used consistently in both the design and the code:

Entity: Task

Attributes: id (int), title (string), description (string), is_completed (boolean).

Endpoints: POST /tasks, GET /tasks, PUT /tasks/<id>, DELETE /tasks/<id>.


3. Implementation (Coding)
I have implemented the TaskMaster API below. This code satisfies the design requirements and uses the exact nomenclatures defined.

Python

from flask import Flask, jsonify, request

app = Flask(__name__)

# Data Store (In-memory)
tasks = []
task_id_counter = 1

@app.route('/tasks', methods=['POST'])
def create_task():
    global task_id_counter
    data = request.get_json()
    new_task = {
        'id': task_id_counter,
        'title': data.get('title'),
        'description': data.get('description'),
        'is_completed': False
    }
    tasks.append(new_task)
    task_id_counter += 1
    return jsonify(new_task), 201

@app.route('/tasks', methods=['GET'])
def get_tasks():
    return jsonify(tasks), 200

@app.route('/tasks/<int:id>', methods=['PUT'])
def update_task(id):
    task = next((t for t in tasks if t['id'] == id), None)
    if not task:
        return jsonify({'error': 'Task not found'}), 404
    
    data = request.get_json()
    task['title'] = data.get('title', task['title'])
    task['description'] = data.get('description', task['description'])
    task['is_completed'] = data.get('is_completed', task['is_completed'])
    return jsonify(task), 200

@app.route('/tasks/<int:id>', methods=['DELETE'])
def delete_task(id):
    global tasks
    tasks = [t for t in tasks if t['id'] != id]
    return '', 204




    4. Testing (Pre-Deployment)
Before a project moves to the deployment stage, it must pass a Quality Assurance (QA) gate. This involves verifying that the Task entity behaves as expected.

Validation: Ensuring is_completed is always a boolean.

Environment Parity: Checking that the code runs the same on a "Staging" server as it does on a developer's laptop.

5. Deployment
Deployment is the process of moving the application to a production environment. For a Python-based Flask API like TaskMaster, this typically involves three main steps:

A. Environment Configuration
We move away from the built-in Flask development server (which is not secure for production) and use a production-grade WSGI Server like Gunicorn.

Dependencies: We create a requirements.txt file to ensure the server knows exactly which libraries to install.

Variables: Sensitive data (like API keys, though not used here) are moved to Environment Variables.

B. Containerization
To ensure the project runs the same way regardless of the underlying hardware, we use Docker. We create a Dockerfile that packages the code, the Python runtime, and the dependencies into a single "image."

C. Hosting Strategy
We choose a hosting provider (Cloud Platform) to run our container:

Platform as a Service (PaaS): Using a service like Render or Heroku. You simply connect your code, and they handle the server management.

Infrastructure as a Service (IaaS): Using AWS (EC2) or Google Cloud. This gives us full control over the virtual machine but requires manual setup of the OS and security firewalls.

6. Maintenance and Monitoring
Once the TaskMaster API is live, the SDLC enters the maintenance phase.

Log Management: Tracking errors if a user tries to DELETE a task that doesn't exist.

Scaling: If thousands of users start creating tasks, we "scale up" by adding more CPU power or "scale out" by running multiple instances of the API behind a Load Balancer.

if __name__ == '__main__':
    app.run(debug=True)
