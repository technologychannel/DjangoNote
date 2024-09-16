### Django Templates
Template is a text file that define the structure of that file, such as html page. In Django Template are used to generate dynamic HTML by injecting data to placehoders.

### Why Use Templates?
- To Seprate Presntation Logic From business logic.
- Template allows us to dynamically generate content based on data pass from view.

### Template Variables
- Template Variable {{variable}}
- Template Tags {{% %}} [Loop or Condition]


### How to Display HTML In Django
- First create one folder named "templates" in your app directory
- Inside templates folder create another folder "your app name"
- Put your html file inside that "your app name" folder.
- For e.g index.html
- To show html page use following code:
```python
def index(request):
    return render(request, 'htmlapp/index.html')
``` 


