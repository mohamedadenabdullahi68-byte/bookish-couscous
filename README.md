MOHAMED ADEN ABDULLAHI

Exercise A: User Manual Procedure
Setting Up a Python Virtual Environment and Installing a Package
Prerequisites
Before you begin, make sure you have:
A computer running Windows, macOS, or Linux
Python 3.8 or later installed (check by running python --version or python3 --version in a terminal)
A terminal or command-line application (Terminal on macOS/Linux, Command Prompt or PowerShell on Windows)
Basic familiarity with navigating folders using a terminal (e.g., cd command)
An internet connection (to download the package)
Procedure
Open your terminal application.
 Expected result: A terminal window appears with a blinking cursor, ready to accept commands.


Navigate to the folder where you want to create your project by typing cd path/to/your/folder and pressing Enter.
 Expected result: The terminal prompt updates to show your new current directory.


Create a virtual environment by typing python -m venv venv (use python3 instead of python on macOS/Linux if needed) and pressing Enter.
 Expected result: The command completes with no output, and a new folder named venv appears in your project folder.


Activate the virtual environment.


On Windows: type venv\Scripts\activate
On macOS/Linux: type source venv/bin/activate
 Expected result: Your terminal prompt changes to show (venv) at the beginning of the line, indicating the environment is active.
Confirm the virtual environment is active by typing python --version.
 Expected result: The terminal displays the Python version number, confirming Python is accessible from within the virtual environment.


Install a package by typing pip install requests and pressing Enter.
 Expected result: The terminal displays download and installation progress messages, ending with a line similar to Successfully installed requests-2.x.x.


Verify the installation by typing pip list.
 Expected result: A list of installed packages appears, including requests and its version number.


Screenshot Description
Screenshot to include: A terminal window immediately after Step 4 (activation). It should show the command that was typed (source venv/bin/activate or the Windows equivalent) and, critically, the resulting prompt line with (venv) visible at the start — this is the visual confirmation students look for and often miss.
Troubleshooting Note
Most common error: After activating the virtual environment, running pip install appears to succeed, but later, import statements in code still fail with ModuleNotFoundError. This almost always happens because the virtual environment was deactivated (e.g., the terminal was closed and reopened) without the student realizing it. Fix: Check that your prompt still shows (venv). If it doesn't, re-run the activation command from Step 4 before installing or running anything.

Exercise B: API Reference Entry
Create Task
POST /api/v1/projects/{projectId}/tasks
Description
Creates a new task within the specified project. The task is created in an open/incomplete state and, if an assignee is provided, that user is notified according to the project's notification settings.
Path Parameters
Name
Type
Required
Description
projectId
string (UUID)
Required
The unique identifier of the project in which to create the task.

Query Parameters
Name
Type
Required
Description
notify
boolean
Optional
Whether to send a notification to the assignee, if one is set. Defaults to true.

Request Body Parameters
Name
Type
Required
Description
title
string
Required
The task title. Maximum 200 characters.
description
string
Optional
A longer description of the task. Supports plain text or Markdown. Maximum 5,000 characters.
assigneeId
string (UUID)
Optional
The user ID of the person the task is assigned to. Must be a member of the project. Omit to leave the task unassigned.
dueDate
string (ISO 8601 date)
Optional
The date the task is due, in YYYY-MM-DD format. Omit if there is no due date.
priority
string (enum)
Optional
One of low, medium, or high. Defaults to medium if omitted.

Request Headers
Header
Required
Description
Authorization
Required
Bearer token for the authenticated user, in the form Bearer <token>.
Content-Type
Required
Must be application/json.

Response Codes
Status Code
Meaning
201 Created
The task was created successfully. The response body contains the new task.
400 Bad Request
The request body is malformed, missing a required field, or contains an invalid value (e.g., an invalid priority value or malformed dueDate).
401 Unauthorized
The request is missing a valid Authorization header or the token is invalid/expired.
403 Forbidden
The authenticated user does not have permission to create tasks in this project.
404 Not Found
The specified projectId does not exist, or assigneeId does not correspond to a member of the project.
422 Unprocessable Entity
The request is well-formed JSON but fails a business rule (e.g., dueDate is in the past).
500 Internal Server Error
An unexpected error occurred on the server.

Example Request
POST /api/v1/projects/8f14e45f-ceea-4e37-9c0a-1a1b2c3d4e5f/tasks
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

{
  "title": "Draft Q3 marketing report",
  "description": "Summarize campaign performance for Q3 and outline key recommendations for Q4.",
  "assigneeId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "dueDate": "2026-10-15",
  "priority": "high"
}

Example Response (201 Created)
{
  "id": "b2c3d4e5-f6a7-4b8c-9d0e-1f2a3b4c5d6e",
  "projectId": "8f14e45f-ceea-4e37-9c0a-1a1b2c3d4e5f",
  "title": "Draft Q3 marketing report",
  "description": "Summarize campaign performance for Q3 and outline key recommendations for Q4.",
  "assigneeId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "dueDate": "2026-10-15",
  "priority": "high",
  "status": "open",
  "createdAt": "2026-09-18T09:32:11Z",
  "updatedAt": "2026-09-18T09:32:11Z"
}


Ready for the peer critique step whenever you are — if you paste in a partner's draft (or want me to play that role and critique yours), let me know which exercise to start with.

