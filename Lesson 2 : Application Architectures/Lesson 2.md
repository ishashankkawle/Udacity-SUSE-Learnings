# Lesson 2 - Architecture Consideration for Cloud Native Applications

## Prerequisites

Make sure you have [git](https://git-scm.com/) and [python](https://www.python.org/) installed before proceeding to this lesson. Having a good understanding of Python and
[Flask](https://flask.palletsprojects.com/en/2.0.x/) is necessary for completing the exercises given in the lesson. Also a little bit of familiarity with HTTP and JS0N would be
enough to meet all the requirements for this lesson.
Git and Python comes pre-installed with linux, but you need to install it if you work on windows 0S. Downlaod them from the link given above. We have also provided the flask's
official documentation for your reference.

## Installation

To check if Git is successfully installed on your system or not, open up your terminal (or command prompt/Powershell on windows) and type `git --version` It should return a
similar output if you have successfully installed it on your system 👇

![image](https://user-images.githubusercontent.com/79275152/122796322-e49b8400-d2db-11eb-86f8-c4bfa52b7a10.png)

*Do not forget to add python to **PATH** while installing it...*

![image](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse2.mm.bing.net%2Fth%3Fid%3DOIP.UNz9AXM-IS28DSgiAvEWLAHaEm%26pid%3DApi&f=1)

Now to check if Python is successfully installed on your system or not, open up your terminal (or command prompt/Powershell on windows) and type `python --version` It should return a similar output if you have successfully installed it on your system 👇

![image](https://user-images.githubusercontent.com/79275152/122796520-1f9db780-d2dc-11eb-8879-a0be58349aa8.png)

---

#### Okay, so now that we are done with all the prerequisites, we are ready to begin this lesson, and complete its exercises .

This lesson is basically about the architecture consideration one must have while developing cloud native Applications . We have 2 architectures
**Monoliths** and **Microservices**, each of them having its own advantages and disadvantages which probably depends upon one's requirement while developing an application.
This lesson also briefly explains about how each architecture has a set of *trade - offs*, that needs to be thoroughly examined before deciding on the final structure of the
application.

**Once you are done with the installations and are familiar with Python and Flask, We can now move forward to complete the Exercises given in this lesson**

### Exercise 2.7 Trade - offs for monoliths and microservices

This is a simple exercise and you only need to write your answer/opinion to the given statement.

### Exercise 2.11 Endpoints for Application Status

For setting up the environment, enough details are already given in this exercise, you first need to clone [course exercise repo](https://github.com/udacity/nd064_course_1)
using git clone command and then you have to navigate inside *python-helLoworld* folder using `cd exercises/python-helloworld` command. 
Stay inside *Python-helloworld* directory on your terminal and write this command `python app.py` to access the hello world application.

![image](https://user-images.githubusercontent.com/79275152/122799069-ee72b680-d2de-11eb-8b7e-3770caf35967.png)

ctrl+click' on http://192.168.43.242:5000/ to view the running application on your browser. 


>Don't worry if you recieve an erroy saying `"No module name flask"` then you need to install flask first.
If you look inside the *python-helloworld* directory then you are going to find 2 files there, one is **app.py** and the other one is **requirements.txt** which includes the list all of all the required dependencies to run or access this application. 

You can install flask by either running `pip install flask` or by `pip install -r requirements.txt` on your terminal.

So now that everything is perfectly setup, we are ready to proceed with the exercise. We have to extend our hello world flask application by adding 2 more routes or you can say endpoints, that are '/metrics' and '/status' , which are required to return a JSON response which can be hardcoded at this stage. Before, we had only main route in our **app.py** file but after adding both the endpoints, your **app-py** file should look like this 👇

```python
from flask import Flask
from flask import json

app = Flask(__name__)

@app.route('/status')
def status():
    response = app.response_class(
            response=json.dumps({"result":"OK - healthy"}),
            status=200,
            mimetype='application/json'
    )

    return response

@app.route('/metrics')
def metrics():
    response = app.response_class(
            response=json.dumps({"status":"success","code":0,"data":{"UserCount":140,"UserCountActive":23}}),
            status=200,
            mimetype='application/json'
    )

    return response

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run(host='0.0.0.0')
```

Now save this modified file and hit ctrl+c to quit your previous command in the terminal, run `python app.py` again to access your application the same way you did previously. This time go to the URL of yur browser and add `/metrics` & `/status` one by one at the end of the URL to access your newly created endpoints. Like this 👇

![image](https://user-images.githubusercontent.com/79275152/122800372-7a391280-d2e0-11eb-9d8a-73ecb3c9f510.png)


**Great! Congrats, You have just now completed your exercise 2.11**

### Exercise 2.13 - Application Logging

This exercise is pretty much similar to the previous one and has no extra installations/requirements. You only need to have an understand of logging to understand what's written inside your **app.py** file. You can refer to this [logging python module](https://docs.python.org/3/library/logging.html#logging.basicConfig) for your referance. 

---

## Contributors

1) [Snehel](https://github.com/snehel4510)

